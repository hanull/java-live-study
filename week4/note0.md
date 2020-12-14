> ### 목표
>
> JUnit 5가 무엇인지, 어떻게 사용하는지를 알고 테스트 코드 작성에 익숙해지기

## JUnit 5란?

java의 대표적인 Testing Framework이다. 그리고 이전 버전의 JUnit과 다르게 3개의 서브 모듈로 구성되어 있다.

```
JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage
```

- JUnit Platform
  - JVM에서 돌아가는 테스트 프레임워크이다.
  - 테스트를 하기 위한 TestEngine API를 정의한다.
  - 커맨드 라인에서 돌아가는 콘솔 런쳐를 제공한다.
  - JUnit 4 기반 테스트를 제공한다.
- JUnit Jupiter
  - JUnit 5에서 테스트 및 확장하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합이다. 
  - new asserts, new annotations, Java 8 Lambda Expressions 과 같은 JUnit 5에 테스트를 작성하기 위해 추가된 확장 기능들이 포함되어 있다.
- JUnit Vintage
  - JUnit 3, 4(하위 버전) 의 테스트를 실행할 수 있는 TestEngine 을 제공한다.



## 어노테이션

| Annotation           | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| `@Test`              | 테스트 메소드임을 나타낸다.                                  |
| `@ParameterizedTest` | 매개변수가 있는 테스트임을 나타낸다. 여러 인자를 한 번에 테스트 할 수 있다. @ValueSource(strings = { "racecar", "radar"}) |
| `@RepeatedTest`      | 반복 테스트임을 나타낸다. @RepeatedTest(10) : 10번 반복      |
| `@DisplayName`       | IntelliJ의 왼쪽 하단에 테스트들이 실행될 때 각 테스트들이 어떤 이름으로 보여질지를 설정할 수 있다. |
| `@BeforeEach`        | 각 테스트 전에 실행 된다. 즉 테스트 하나 당 한 번씩 실행되는 것이다. JUnit4의 @Before와 같다. |
| `@AfterEach`         | 각 테스트 이후에 실행 된다. JUnit4의 After와 같다.           |
| `@BeforeAll`         | 모든 테스트 전에 딱 한 번 실행 된다. JUnit4의 @BeforeClass와 같다. |
| `@AfterAll`          | 모든 테스트 이후에 딱 한 번 실행 된다. JUnit4의 @AfterClass와 같다. |
| `@Tag`               | 테스트 필터링을 위한 테그를 선언하는데 사용                  |
| `@Disabled`          | 테스트 클래스 혹은 메소드를 비활성하는데 사용(JUnit 4의 @Ignore와 유사) |
| `@Timeout`           | 주어진 시간을 초과할 경우, 테스트 실패를 나타내기 위해 사용  |



## Assertions

Jupiter assertions는 정적 메소드이며,  `org.junit.jupiter.api.Assertions` 에 있다. 람다 표현식을 사용할 수 있다.



## Assumptions

JUnit5에서 개선한 파트이고 특정 환경에 있을 때만 테스트를 실행할 수 있다. 예를들어, 테스트를 특정 운영체제에서만 실행하거나 특정 개발환경 (테스트, 개발환경 등)에서만 실행하고 싶을 때 유용하게 쓸 수 있다. Assertion과 똑같이 정적 메소드로 정의되어 있다.

```java
class AssumptionsDemo {

    private final Calculator calculator = new Calculator();

    @Test
    void testOnlyOnCiServer() {
        assumeTrue("CI".equals(System.getenv("ENV")));
        // remainder of test
    }

    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, calculator.divide(4, 2));
            });

        // perform these assertions in all environments
        assertEquals(42, calculator.multiply(6, 7));
    }

}
```





## 참고

- [junit5 가이드 문서](https://junit.org/junit5/docs/current/user-guide/)