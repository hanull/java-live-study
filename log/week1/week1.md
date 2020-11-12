> ## 과제
>
> JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가
>
> ## 목표
>
> 자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기
>
> ## 학습할 내용
>
> - [JVM이란 무엇인가](#1.-jvm이란-무엇인가)
>
> - [컴파일 하는 방법](#2.-컴파일-하는-방법)
>
> - [실행하는 방법](#3.-실행하는 방법)
>
> - [바이트코드란 무엇인가](#4.-바이트코드란-무엇인가)
>
> - [JIT 컴파일러란 무엇이며 어떻게 동작하는지](#5.-jit-컴파일러란-무엇이며,-어떻게-동작하는가)
>
> - [JVM 구성요소](#6.-jvm-구성요소)
>
> - [JDK와 JRE의 차이](#7.-jdk와-jre의-차이)



## 1. JVM이란 무엇인가

- 자바 가상 머신(JVM: Java Virtual Machine)의 약자이다.
- 자바 바이트 코드를 OS에 맞게 `해석`하고 `실행` 한다.
- 자바 프로그램과 OS사이에서 `중개자` 역할을 수행하며, 자바 프로그램이 OS에 `독립적` 이도록 해준다.  즉, 자바 프로그램이 어느 운영체제 위에서도 동일하게 작동할 수 있게 해준다. (단, JVM은 운영체제에 종속적으로 각 운영체제에 맞는 JVM이 설치되어야 한다.)
  - OS 의 메모리 영역에 직접적으로 접근하지 않고, JVM을 통해 간접적으로 접근한다.
- 메모리 관리, 가비지 컬렉션을 수행 한다.

### 가비지 컬렉션

자바 프로그램에서 사용되지 않는 메모리를 지속적으로 찾아내서 제거하도록 하는 것을 말한다.  가비지 컬렉션은 실행 중인 JVM 내부에서 일어난다.



## 2. 컴파일 하는 방법

### 컴파일이란?

일반적으로 소스 코드를 기계가 인식할 수 있는 네이티브 코드로 변환하는 과정을 컴파일이라 한다. 하지만, 자바에서의 컴파일은 자바 언어를 JVM이 인식할 수 있는 `바이트코드(.class)` 로 변환하는 것을 의미한다. 즉, 자바 코드를 자바 언어 스펙에 따라 `분석/검증하고`, JVM 스펙의 class 파일 구조에 맞는 `바이트코드를 만들어내는` 과정이다.

그런데, 자바에서도 기계가 인식할 수 있는 네이티브 코드로 변환하는 과정을 의미할 때도 있다. JIT 컴파일러가 하는 컴파일은 바이트코드로 변환하는 것이 아니라 바이트코드를 네이티브 코드로 변환하는 것을 의미한다.



### 컴파일 방법

- `javac` 명령으로 바이트코드 생성

![바이트코드 생성](https://github.com/hanull/java-study/tree/master/log/week1/img/javac.png)

- 생성된 바이트코드는 `javap` 명령으로 확인 가능

![바이트코드 확인](https://github.com/hanull/java-study/tree/master/log/week1/img/javac2.png)



### 컴파일 과정

1. 어휘 분석
   - 소스 코드에서 문자 단위로 읽어서 어휘소를 식별하고, `토큰 스트림` 을 생성한다.
   - 식별자 토큰(변수 이름, 상수 이름, 함수 이름 등)이 심볼 테이블에 저장된다.

2. 구문 분석

   - 토큰 스트림의 언어의 스펙으로 정해진 문법 형식에 맞는지 검사한다. 맞지 않으면 `컴파일 에러`, 맞다면 `파스 트리` 를 생성한다.

   ![출처 : https://en.wikipedia.org/wiki/Compiler](https://github.com/hanull/java-study/tree/master/log/week1/img/parsertree.gif)

3. 의미 분석

   - 타입 검사, 자동 타입 변환 등을 수행한다.

     ```java
     int number = "String";
     ```

   - 의미 분석 단계 후, 파스 트리에 타입 관련 정보 등이 추가된다.

4. 중간 코드 생성 및 최적화
   - 의미 분석 단계를 통과한 파스 트리를 바탕으로 `중간 코드를 생성`한다.



## 3. 실행하는 방법

### 실행 방법

`java` 명령으로 자바 애플리케이션을 실행할 수 있다. 

![](https://github.com/hanull/java-study/tree/master/log/week1/img/java.png)



### 실행 과정

java 명령으로 애플리케이션을 실행하면, `JVM 실행` 되면서 `시작 클래스를 생성`, `링크`, `초기화`하고 `main 메서드를 호출`한다.

1. JVM 실행

   - JVM 단위로 `힙`과 `메서드 영역`이 생성된다.

2. 시작 클래스 생성

   - 바이트코드를 `클래스 로더`가 읽어들여 `메서드 영역` 에 저장한다.
   - 상수 풀의 내용을 바탕으로, `런타임 상수 풀`이 `클래스 단위`로 만들어져서 `메서드 영역`에 함께 저장된다.

3. 링크

   - `확인(verify), 준비(prepare), 해석(resolve)` 과정을 진행한다.
   - 확인(verify) : 클래스, 인터페이스의 `바이너리 표현이 구조적으로 올바른지` 확인한다.
   - 준비(prepare) : 클래스, 인터페이스의 `정적(static) 필드`를 생성하고, `기본값으로 초기화`하는 과정이다. (특정값으로 초기화하는 과정은 `초기화 과정`에서 수행된다.)
   - 해석(resolve) : 심볼리 참조가 구체적인 값을 가리키도록 동적으로 결정하는 과정이다. (해석 시점은 JVM 구현체에 따라 다를 수 있다.)

4. 초기화

   - 정적(static) 블럭을 하나로 합친 것을 실행한다. 즉, static 블럭에 있는 특정값으로 정적 초기화를 한다. 

5. main 메서드 호출

   - `main 메서드 프레임`이 생성되고, 로직에 따라 동적 수행을 한다.

   - `PC Register, JVM Stack, Native Method Stack`이 한 개씩  `생성`된다.



## 4. 바이트코드란 무엇인가

- JVM이 이해할 수 있는 언어로 변환된 코드를 말한다.

- 바이트코드는 JVM만 설치되어 있다면 어느 OS에서도 실행 가능하다.

  

## 5. JIT 컴파일러란 무엇이며 어떻게 동작하는가




## 6. JVM 구성요소





## 7. JDK와 JRE의 차이

 ### JRE (Java Runtime Environment)

```
JRE = JVM + 라이브러리
```

- 자바 애플리케이션을 `실행`할 수 있도록 구성된 배포판이다. 즉, JVM의 `실행 환경`을 구현했다고 할 수 있다.
- 하지만, 개발 도구는 포함하지 않는다. (JDK에서 제공)

![출처 : https://wikidocs.net/257](https://github.com/hanull/java-study/tree/master/log/week1/img/jre.jpg)



### JDK (Java Development Kit)

```
JDK = JRE + 개발 도구
```

- 자바 기반 애플리케이션 `개발`을 위한 소프트웨어 패키지다.
- 오라클은 자바 11부터는 JDK만 제공하며 JRE를 따로 제공하지 않는다.

![출처 : https://wikidocs.net/257](https://github.com/hanull/java-study/tree/master/log/week1/img/jdk.jpg)



## 8. 참조

- https://asfirstalways.tistory.com/158#recentEntries

- https://www.inflearn.com/course/the-java-code-manipulation#description
- https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-1