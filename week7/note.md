### 7주차 과제 : 패키지

> package 키워드
>
> import 키워드
>
> CLASSPATH
>
> 접근지시자



## 패키지(package)

- 클래스나 인터페이스의 집합을 의미한다.
- 파일을 계층적 구조로 관리하기 위해서 폴더를 사용하는 것 처럼, Java 의 클래스도 package 라는 구조를 통해서 계층적으로 관리한다.
- 보통 www를 제외한 도메인의 역순 구조를 많이 사용한다. www.hanul.com 이 도메인이라면, com.hanul 이 기본 package가 된다. 하위 package는 업무 구분 등으로 구성한다.
- 모든 클래스는 클래스명과 패키지명이 있는데 이 둘을 합쳐서 완전하게 한 클래스로 표현할 수 있다. 이를 `FQCN(Fully Qualified Class Name) ` 이라고 한다.
  - `String` 클래스의 패키지는 `java.lang`이고, FQCN은 `java.lang.String`이 된다.

### 패키지는 어떤 문제를 해결하기 위해 사용할까??

- 유지보수
  - 패키지를 이용하면 특정 클래스나 다른 관련된 클래스를 찾기 쉽다. 수백개의 클래스가 하나의 패키지에 있다면, 특정 클래스를 찾는데  오랜 시간 걸릴 것이다. 하지만 클래스를 비슷한 특징을 가진 클래스끼리 한 패키지로 묶어 관리한다면, 있을법한 패키지만을 찾으면서 더 빠르게 클래스를 찾을 수 있다. 이러한 점으로 패키지를 이용하면 유지보수를 더 쉽게 할 수 있다. 



## import

- 다른 package 에 정의된 모듈(클래스, 인터페이스 등)을 사용하고자 할 때는 `import` 키워드를 사용한다. 
-  `java.lang` 패키지의 클래스는 기본적으로 제공하는 패키지이기 때문에 import 구문 없이 참조 가능하다. 그리고 동일 패키지의 클래스도 import 구문 없이 참조 가능하다.

### static import

임의의 패키지 클래스를 import static 으로 import 한다면, 클래스명 없이 사용할 수 있다.

```java
import static java.lang.Math.abs;

public class Main {

    public static void main(String[] args) {
        System.out.println(abs(-1));
    }
}
```



## CLASSPATH

클래스 패스는 JVM이 프로그램을 실행할 때 클래스를 찾기 위한 기준이 되는 경로를 말한다.

IDE로 주로 intellij를 사용하는데 클래스 패스를 설정하지 않으면 기본적으로 현재 디렉토리를 잡아주고 그 위치에서 파일을 찾는다.

### CLASSPATH 환경변수

```java
export CLASSPATH = 경로
```

### -classpath 옵션

`java` 명령 실행 시 옵션으로 클래스패스를 지정할 수 있다.

```java
java -classpath 경로 TestClassPath
```

 `CLASSPATH` 환경 변수 설정 후 해당 옵션으로 실행해 보면,  `-classpath` 옵션으로 지정한 경로의 결과가 실행 된다. 따라서 `-classpath` 의  우선 순위가 높은 것을 알 수 있다.



## 접근지시자 (Access Modifiers)

접근 지시자는 멤버 변수나 메소드들의 접근 범위를 정의하기 위해 사용한다.

- public : 접근을 제한하지 않아 어디서든 접근 가능
- private : 클래스 내부에서만 접근 가능
- protected : 클래스 내부, 동일 패키지, 상속받은 클래스에서만 접근을 가능
- default(명시하지 않음) : 클래스 내부와 동일 패키지에서만 접근이 가능

| 지시자      | 클래스 내부 | 동일 패키지 | 상속받은 클래스 | 이 외 |
| :---------- | :---------- | :---------- | :-------------- | :---- |
| private   | O           | X           | X               | X     |
| default     | O           | O           | X               | X     |
| protected | O           | O           | O               | X     |
| public    | O           | O           | O               | O     |



### 참고
- [삐멜 소프트웨어 엔지니어](https://imasoftwareengineer.tistory.com/71)