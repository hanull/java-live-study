### 6주차 과제 : 상속

> 자바 상속의 특성
>
> super 키워드
>
> 메소드 오버라이딩
>
> 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
>
> 추상 클래스
>
> final 키워드
>
> Object 클래스



## 상속의 특성

- 자식 클래스는 부모 클래스의 정보를 상속 받을 수 있다.

- static 메소드, 변수도 상속 된다.

- 부모 클래스에서 멤버 변수 또는 메소드가 private 접근 제한자를 사용할 시, 

  멤버 변수는 상속 되나 바로 접근 불가능하다.

  메소드는 상속되지 않는다.

- 자식 클래스는 별도의 내용을 가질 수 있다.

- 코드를 공통으로 관리할 수 있기 때문에 코드의 추가와 변경이 용이하다.

- 다중 상속을 지원하지 않는다. 즉, 자식 클래스는 하나의 부모 클래스만 상속 받을 수 있다.

- 자식 클래스는 부모 클래스를 알지만, 부모 클래스는 자식 클래스를 알 수 없다.


### 자식 클래스 객체를 생성할 때(부모 참조) 메모리 할당은 어떻게 될까??

```java
Parent p2 = new Child();
p2.hello();
```

p2 객체가 생성될 때는 Object, Parent, Chile 클래스가 메모리 할당되고, p2는 `Parent` 의 주소를 참조한다. 만약 hello() 메소드가 Parent 의 공간에 없다면  `Object` 공간으로 올라가서 찾는다. Object 공간에도 없다면 에러를 발생한다.



자식 클래스를  참조하고 싶다면, 업캐스팅 된 p2를 다운 캐스팅하면 된다.

```java
Child child = (Child) p2;
child.hello();
```


## super 키워드



### 자식 클래스 객체를 생성할 때(부모 참조) 메모리 할당은 어떻게 되는가??

```java
Parent p2 = new Child();
p2.hello();
```

p2 객체가 생성될 때는 Object, Parent, Chile 클래스가 메모리 할당되고, p2는 `Parent` 의 주소를 참조한다. 만약 hello() 메소드가 Parent 의 공간에 없다면  `Object` 공간으로 올라가서 찾는다. Object 공간에도 없다면 에러를 발생한다.



자식 클래스를  참조하고 싶다면, 업캐스팅 된 p2를 다운 캐스팅하면 된다.

```java
Child child = (Child) p2;
child.hello();
```





### super

부모 클래스를 상속받은 자식 클래스의 생성자에는 super() 키워드가 생략되어 있다. 따라서 자식 객체를 생성 시 부모 클래스의 생성자를 호출한다.

```java
public class Parent {

	Parent() {
		System.out.println("부모 생성자");
	}
	
	void hello() {
		System.out.println("hello");
	}
}

public class Child extends Parent{

	Child() {
    // super();  
		System.out.println("자식 생성자");
	}
}
```

```java
public static void main(String[] args) {
		Parent parent = new Parent();
		Child child = new Child();
		
		child.hello();
	}
```

```
부모 생성자
부모 생성자
자식 생성자
hello
```


### 만약, 동일한 이름의 멤버 변수가 부모, 자식 클래스에 둘다 존재할 경우 어떤 멤버 변수를 사용될까?? 

```java
public class People {

    public String name = "JAVA";

}
```

```java
public class Student extends People {

    public String name = "live-study";

    public String getSuperClassName() {
        return super.name;
    }

    public static void main(String[] args) {
        Student student = new Student();
        System.out.println(student.name);
        System.out.println(student.getSuperClassName());
    }
}

```

```
live-study
JAVA
```

위와 같이 부모 클래스의 멤버 변수가 자식 멤버 변수에 의해 가려지고, 부모 클래스의 멤버 변수에 접근하려면은 super 키워드를 통해서 접근할 수 있다. 





## 메소드 오버라이딩 (Method Overriding)

자식 클래스가 부모 클래스의 메소드와 동일한 이름, 파라미터, 리턴 타입를 가질 경우 부모 클래스의 메소드를 덮어쓰는 것을 말한다. 즉, 부모 클래스의 메소드를 재정의하는 것이다.

```java
public class A {

    public int cal1(int x, int y) {
        return x + y;
    }

    public int cal2(int x, int y) {
        return x * y;
    }
}

public class B extends A {

    // A 클래스의 cal1 메소드를 오버라이딩
    @Override
    public int cal1(int x, int y) {
        return x - y;
    }
}
```

```java
A a = new A();
B b = new B();

a.cal1(3, 2); // 5
b.cal1(3, 2); // 1

a.cal2(3, 2); // 6
b.cal2(3, 2); // 6
```



## 메소드 디스패치 (Method Dispatch)

자바는 객체지향 프로그래밍언어로서 객체들간의 메세지 전송을 기반으로 문제를 해결하게된다. 메세지 전송이라는 표현은 결국 메서드를 호출하는것인데 이를 `dispatch`라고 부른다.

### 1. 스태틱 메소드 디스패치 (Static Method Dispatch)

구현 클래스를 이용해 컴파일 시점에 어떤 메소드를 호출할 것인지 명확하게 알고 있는 경우이다.  

```java
class Dispatch{
    public String method(){
        return "hello";
    }
}

public class Test {
    public static void main(String[] arg) {
        Dispatch dispatch = new Dispatch();
        System.out.println(dispatch.method());
    }
}

```



### 2. 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

인터페이스를 이용해 참조함으로서 호출되는 메서드가 동적으로 정해지는 것을 말한다.  컴파일러는 타입에 대한 정보를 알고있으므로 런타임시에 호출 객체를 확인해 해당 객체의 메서드를 호출한다. 런타임시에 호출 객체를 알 수 있으므로 바이트코드에도 어떤 객체의 메서드를 호출해야하는지 드러나지 않는다. 

예제코드에서 method() 메서드는 인자가 없는 메서드이지만, 자바는 묵시적으로 항상 호출 객체를 인자로 보내게된다. 호출 객체를 인자로 보내기때문에 this를 이용해 메서드 내부에서 호출객체를 참조할 수 있는 것이다. 또한 이것이 dynamic dispatch의 근거가 되게된다.

```java
interface Dispatchable{
    String method();
}

class Dispatch implements Dispatchable {
    public String method(){
        return "hello";
    }
}

public class Test {
    public static void main(String[] arg) {
        Dispatchable dispatch = new Dispatch();
        System.out.println(dispatch.method());
    }
}

```



## 추상 클래스

추상 클래스는 정의만 있고 구현은 되어있지 않은 메소드가 하나 이상 존재하는 클래스를 말한다. 

- 클래스, 메소드 앞에 `abstract` 키워드를 붙임으로써 추상 클래스를 생성할 수 있다.

- 추상 클래스를 상속한 클래스는 모든 추상 메소드를 재정의(오버라이딩) 해야한다. 만약 추상 메소드를 재정의하지 않고 추상 클래스를 상속한다면 그 자체도 추상 클래스가 되어야 한다.
- `static`, `private`, `final` 메소드는 하위 클래스에 의해 오버라이딩 될 수 없기 때문에 추상 메소드가 될 수 없다.





## final 키워드

`final` 키워드가 클래스, 변수, 메소드 앞에 붙으면 더 이상 변할 수 없는 상태가 된다.

### 특징

- final 클래스는 상속할 수 없다.
- final 메소드는 오버라이딩할 수 없다.
- final 변수의 값을 재할당할 수 없다.
- final 변수는 초기화가 필요하다.

### 왜 사용하는가??

의도하지 않았던 변경을 막아줌으로써 원하는 기능을 정상적으로 수행할 수 있도록 한다. 즉, 실수와 버그를 줄일 수 있다.



## Object 클래스

java.lang.Object 클래스는 모든 클래스의 `최상위 클래스`이다. 즉, 모든 클래스들은 Object 클래스로부터 상속받는다.

사용자가 클래스를 정의할 때 선언부에 명시적으로 extends Object 하지 않아도 자동으로 상속받게 된다.

### 주요 메소드

- [공식 문서 참고](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Object.html)



## 참고

- https://multifrontgraden.tistory.com/133



