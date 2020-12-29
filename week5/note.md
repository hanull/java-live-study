### 5주차 과제 : Class

> 클래스 정의하는 방법
>
> 객체 만드는 방법 (new 키워드 이해하기)
>
> 메소드 정의하는 방법
>
> 생성자 정의하는 방법
>
> this 키워드 이해하기



## 객체 지향 프로그래밍 (OOP, Object-Oriented Programming)

객체 지향 프로그래밍은 시스템을 여러 개의 독립된 단위, 즉 상태와 행위를 가진 `객체`들의 모임으로 분할하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다. (협력)

객체 지향 프로그래밍은 프로그램을 `유연`하고 `변경`이 용이하게 만들기 때문에 대규모 소프 트웨어 개발에 많이 사용된다.



## 클래스 (Class)

객체들의 협력 관계를 코드로 옮기는 도구이다.

협력에 참여하여 메시지를 주고 받는 객체를 만드는데 필요한 구현 메커니즘이다.

클래스는 객체의 상태를 표현하는 `field`와 행위를 표현하는 `method`로 구성된다. 또한 생성된 인스턴스의 필드를 초기화 하는 특별한 메서드인 `constructor`를 가진다.



## 객체 만드는 방법 (new 키워드 이해하기)

```java
class Animal {
  int age;
  int size;
}
```

```java
Animal dog = new Animal();
Animal cat = new Animal();
```

new는 인스턴스를 생성해주는 역할을 담당한다. new 연산자를 통해 메모리(Heap 영역)에 데이터를 저장할 공간을 할당받고 생성자를 호출한다.



### 필드(field)

```java
class student {
  static String school;	// 1. 클래스 변수
  String name;					// 2. 인스턴스 변수
  
  void eat (String menu) {	// 3. 지역 변수
    String desert = "coke";
    System.out.println("급식으로" + menu + "를 먹는다.");
    System.out.println("디저트로" + desert + "를 마신다");
  }
    
}
```

1. 클래스 변수
   - 클래스 영역에서 static 키워드를 가진 변수
   - 모든 인스턴스가 공유하는 변수
   - 인스턴스 생성 없이 사용 가능
   - this 사용 불가
   - 클래스가 메모리에 올라갈 때 method 영역에 생성되고, 프로그램이 종료될 때 제거된다.
2. 인스턴스 변수
   - 클래스 영역에서 static 키워드를 가지지 않은 변수
   - 각 인스턴스 마다 생성되는 변수
   - new 키워드로 새로운 객체가 생성될 때마다 생성되어 heap영역에 메모리를 할당받고, 해당 객체가 더 이상 사용되지 않으면 Garbage Collector에 의해 메모리에서 제거된다.
3. 지역 변수
   - 특정 메소드 안에서만 사용하는 변수
   - 변수가 선언되는 시점부터 블럭이 종료되는 시점까지만 stack 영역에 존재한다.



### 메소드(method)

- 메소드는 선언부(리턴타입, 메소드이름, 매개변수선언)와 실행 블록으로 구성된다.

- 메소드 이름은 자바 식별자 규칙에 맞게 작성한다.
  - 숫자로 시작하면 안 되고, '$'와 '_'를 제외한 특수 문자를 사용하지 말아야 한다.
  - 관례적으로 메소드 명은 소문자로 작성한다.
  - 서로 다른 단어가 혼합된 이름이라면 뒤이어 오는 단어의 첫머리 글자는 대문자로 작성한다.



### 생성자(constructor)

- 생성자는 인스턴스 변수를 초기화하는 특수한 메서드이다.
- 생성자는 클래스 이름과 같아야 한다.
- 반환값이 없고, 반환타입을 선언하지 않는다.
- 여러 개의 생성자를 가질 수 있다.
- 생성자가 정의되지 않은 경우, 자바 컴파일러가 클래스에 default 생성자를 자동으로 추가한다.
- 하지만 생성자가 하나라도 정의된 경우, default 생성자는 자동추가가 되지 않는다.

```java
class student {
  static String school;
  String name;
  int age;
  
  student(){}
  
  student (String name) {
    this.name = name;
  }
  
  student (String name, int age) {
    this.name = name;
    this.age = age;
  }
  
}
```



## this

- 객체 자신을 의미한다. 인스턴스의 주소가 저장되어 있다.
- 객체 내부에서 필드에 접근하기 위해 this를 사용할 수 있다.
- 생성자 내에서 다른 생성자를 호출할 때 클래스 이름 대신 this를 사용해야한다. 단, 반드시 첫 줄에서만 호출이 가능하다.

```java
public class Student {

    String name;
    int age = 0;

    public Student() {}

    public Student(String name) {
        this(name, 10);
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public static void main(String[] args) {
        Student test = new Student("홍길동");
        System.out.println(test.age);
    }

}
```

```
10
```

