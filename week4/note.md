> ## 목표
>
> 자바가 제공하는 제어문 학습하기
>
> ## 학습할 것
>
> - 선택문
> - 반복문
>
> ## 과제
>
> 0. [JUnit5 학습]()
> 1. [live-study 대시 보드를 만드는 코드 구현]()
> 2. [LinkedList를 구현]()
> 3. [Stack 구현]()
> 4. [앞서 만든 ListNode를 사용해서 Stack 구현]()
> 5. [Queue 구현]()

## 조건문 (if, else if, else)

표현식이 참인지 판별하여 조건에 맞는 code-block을 선택한다.

```java
char alpa = 'a';
if (alpa >= 'a' && alpa <= 'z') 
{
  System.out.println("소문자");
} else if (alpa >= 'A' && alpa <= 'Z') {
  System.out.println("대문자");
} else {
  System.out.println("알파벳이 아닙니다.");
}
```



## 선택문 (switch)

여러 분기를 만들 수 있다는 점에서 `if-else`와 유사하지만, `보다 간단하고 가독성이 높은 형태로 구현할 수 있다.`

```java
public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int month = sc.nextInt();
        System.out.println(getWhichSeason(month));
    }

    private static String getWhichSeason(int month) {
        String season = "";
        switch (month) {
            case 12, 1, 2:
                season = "겨울";
                break;
            case 9, 10, 11:
                season = "가을";
                break;
            case 6, 7, 8:
                season = "여름";
                break;
            case 3, 4, 5:
                season = "봄";
                break;
            default:
                throw new IllegalStateException("올바른 값을 입력하세요");
        }
        return season;
    }
}
```



- 자바 12에 break를 대신할 `arrow, yield` 가 소개되었고, 이를 통하여 결과값을 더욱 편리하게 반환할 수 있다.

```java
    private static String getWhichSeason(int month) {
        String season = switch (month) {
            case 12, 1, 2 -> "겨울";
            case 9, 10, 11 -> "가을";
            case 6, 7, 8 -> "여름";
            case 3, 4, 5 -> "봄";
            default -> throw new IllegalStateException("올바른 값을 입력하세요");
        };
        return season;
    }
```



```java
    private static String getWhichSeason(int month) {
        String season = switch (month) {
            case 12, 1, 2:
                yield "겨울";
            case 9, 10, 11:
                yield "가을";
            case 6, 7, 8:
                yield "여름";
            case 3, 4, 5:
                yield "봄";
            default:
                throw new IllegalStateException("올바른 값을 입력하세요");
        };
        return season;
    }
```





## 반복문 (while, do-while, for, foreach)

- while(조건), do-while(조건)
  - 조건이 참이라면 block을 반복 실행한다.

```java
// 1 ~ 9, 10 ~ 19까지 출력하기
	public static void main(String[] args) {
        int num = 1;
        while (num < 10) {
            System.out.print(num + " ");
            num++;
        }
        System.out.println();
        do {
            System.out.print(num + " ");
            num++;
        } while (num < 20);
    }
```

```
1 2 3 4 5 6 7 8 9 
10 11 12 13 14 15 16 17 18 19
```

- for, foreach
  - for (카운팅 변수 초기화; 표현식; 명령문) { block }
  - 표현식이 만족하는 동안 block을 반복 실행한다.

```java
// for 0 ~ 9 출력
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
System.out.println();
// foreach
var list = new ArrayList<>();
for (int i = 1; i < 10; i++) {
  list.add(i);
}
list.forEach(num -> {
  System.out.println(num);
});
```

```
0 1 2 3 4 5 6 7 8 9 
1 2 3 4 5 6 7 8 9
```

