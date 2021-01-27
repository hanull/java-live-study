### 8주차 과제 : 인터페이스

> 인터페이스 정의하는 방법
>
> 인터페이스 구현하는 방법
>
> 인터페이스를 활용한 다형성
>
> 인터페이스의 기본 메소드 (Default Method), 자바 8
>
> 인터페이스의 static 메소드, 자바 8
>
> 인터페이스의 private 메소드, 자바 9



## 인터페이스 정의

- 인터페이스는 추상 메서드의 집합이다. 
- 구현된 것이 없는 설계도(껍데기) 이다.
- 인터페이스의 모든 메소드는 추상 메소드이고, 모든 변수는 상수이다.
- public, abstract, static, final 등의 키워드를 암시적으로 사용하고 생략 가능.
- 인터페이스는 인스턴스화 할 수 없으므로 생성자가 필요 없다.
- 인터페이스는 다중 중첩 가능.
- `Java 8`부터  `static` 메소드를 정의 가능.
- `Java 8`부터  `default` 메소드를 정의 가능.
  `Java 9`부터  `private` 메소드를 정의 가능.

```java
interface 인터페이스이름 {
	(public) (static) (final) 타입 상수이름 = 값;	// 상수
	(public) (abstract) 메서드이름(매개변수목록);	// 추상메서드
}
```



## 인터페이스 구현하는 방법

인터페이스는 구현하는 의미이기 때문에 `implements` 키워드를 사용한다. 그리고 인터페이스에 정의되어 있는 메소드를 반드시 구현해야 한다.

```java
public interface Product {
	String getName();
	int price();
}
```

```java
public class TV implements Product{

	@Override
	public String getName() {
		return "TV";
	}

	@Override
	public int price() {
		return 2000000;
	}	
}
```



## 인터페이스를 활용한 다형성

```java
public interface Device {
    void print(String content) ;
}

```

```java
public class DeviceConsole implements Device{

	@Override
	public void print(String content) {
		System.out.println(content);		
	}
}
```



```java
public class DeviceFile implements Device{

	@Override
	public void print(String content) {
		try {
			FileWriter out = new FileWriter("eleven.txt");
			out.write(content);
			out.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}    
}
```

Device Interface 를 이용해서 DeviceConsole, DeviceFile 구현체를 만들었고 이 두개의 구현 클래스를 내 입맛에 맞게 사용할 수 있다. 다시말해 아래의 코드 처럼 매개변수에 인터페이스를 넘겨주고 입맛에 맞는 구현체를 끼워넣어서 사용할 수 있다.

```java
public class MyDevice {

	public void print(Device device, String msg) {	// 인터페이스를 매개변수로 사용할 수 있다.
		device.print(msg);
	}
}

```

```java
public class Test {//My프로젝트 사용
    public static void main(String[] args) {
	    MyDevice myDevice = new MyDevice();
	    
	    //레고블럭: DeviceConsole, DeviceFile 
	    DeviceConsole dc = new DeviceConsole();
	    DeviceFile df = new DeviceFile();
	    
	    Device test = new DeviceConsole();		// 인터페이스 타입의 참조변수로 이를 구현한 클래스의 인스턴스를 가리킬 수 있다.
	    
	    my.print(test, "test");
	    my.print(dc, "화이팅~!!");
	}
}

```



## 인터페이스 기본 메소드(Default Method), 자바 8

자바8 이전에 인터페이스가 가질 수 있는 메서드는 추상메서드뿐이었기 때문에 인터페이스의 구현 클래스들이 구현 코드를 작성할 때, 그 내용이 일치하더라도 클래스마다 새롭게 코드를 적어줘야 한다는 문제점이 있었다.

이 문제점을 해결하기 위해서 `Default Method` 가 생기게 되었다. default 메서드는 구현체가 있는 메소드이고, implements 하게되면 해당 메소드가 자동으로 상속되기 때문에 구현없이 바로 사용가능하다. 또한, 오버라이딩하여 재사용도 가능하다.

```java
public interface Folder {
	public fold();
  
  default public void power() {
    System.out.println("power On");
  }
}
```

```java
public static void main(String[] args) {
  Folder folder = new FolderPhone();
  
  folder.power();
}

```

```
power On
```



## 인터페이스 static 메소드, 자바 8

- static 메서드는 이미 구현된 메서드이다(default 메서드와 동일). 그리고 추상 메소드일 수 없다.
- `static` 메소드이므로 상속이 불가능하다.  
- 클래스에서와 동일하게 사용 가능하며 기본적으로 `public`으로 간주한다. `private` 으로도 사용 가능하다.



## 인터페이스 private 메소드, 자바 9

- `private 메서드`는 오직 해당 인터페이스 내에서만 접근 가능하다.
- 구현체 클래스에서 구현할 수 없고,  자식 인터페이스에서도 상속이 불가능하다. 따라서 인터페이스에서 구현이 되어 있어야하고 추상(`abstract`) 메소드일 수 없다.
- `static` 메소드도 `private`이 가능하다.



## 인터페이스와 추상 클래스의 차이점

상속은 본질적으로 `재사용` 인데 반해, 인터페이스는 `규약, 약속`이라고 생각하면 된다. 

추상클래스와 인터페이스는 인스턴스화 하는 것은 불가능하고, 구현부 메소드, 구현부가 없는 메소드 모두 가질 수 있다는 공통점이 있다. (자바8부터 인터페이스도 default 키워드를 통해서 메소드 구현부를 가질 수 있게 됨)

### 

| 추상 클래스(abstract class)                                  | 인터페이스(Interface)                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 추상 메소드를 가지고 있는 일반 클래스이다. 따라서 생성자 변수 등을 가질 수 있다. | 추상 메소드들의 집합이다.                                    |
| 관련도가 높은 클래스간에 코드를 공유하고 싶은 경우, 공통적으로 가지는 메소드나 변수가 많은 경우 사용한다. | 구현 클래스들간 관련성이 없는 경우가 대부분이다. 예를 들어, Comparable, Cloneable 인터페이스는 여러 클래스들에서 구현되는데, 구현클래스들 간에 관련성이 없는 경우가 대부분이다. |
| 다양한 접근 제어자(public, protected, private)를 사용하여 변수와 메소드를 정의할 수 있다. | 모든 변수는 `public static final`, 모든 메소드는 `public abstract` |
| 다중 상속 불가능                                             | 다른 여러개의 인터페이스들을 함께 구현 가능                  |
| 많은 추상 메소드를 가진 인터페이스를 사용할 경우, 사용하지 않아도 되는 메소드까지 모두 구현해야하는 불편함이 있는데, 이를 해소하기 위해서  추상 클래스를 사용한다. 자바 API 중 많은 추상 클래스가 `~Adapter` 라는 이름을 가지고 있는데, 위의 용도로 사용되는 것들이다. |                                                              |



