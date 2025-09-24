---
title: "[JAVA] Checked Exception 과 Unchecked Exception"
date: 2025-09-24
categories: [JAVA]
tags: [TIL, JAVA]
---

# # Checked Exception 과 Unchecked Exception

## 1. Checked Exception

- 컴파일 시점에 확인되며, 반드시 처리해야 하는 예외

- 예외 처리를 하지 않으면 컴파일 오류가 발생

- 주로 외부 자원(파일, 네트워크 등)과 관련된 예외

- 예시: `IOException`, `SQLException`

```java
import java.io.FileReader;
import java.io.IOException;
public class CheckedExceptionExample {
    public static void main(String[] args) {
        try {
            FileReader file = new FileReader("nonexistentfile.txt");
        } catch (IOException e) {
            e.printStackTrace();
            }
     }
}
```

<br /><br />

## 2. Unchecked Exception

- 런타임 시점에 발생하며, 반드시 처리하지 않아도 되는 예외

- 예외 처리를 하지 않아도 컴파일 오류가 발생하지 않음

- 주로 프로그래밍 오류(논리적 오류, 잘못된 API 사용 등)와 관련된 예외

- 예시: `NullPointerException`, `ArrayIndexOutOfBoundsException`

```java
public class UncheckedExceptionExample {
    public static void main(String[] args) {
        String str = null;
        System.out.println(str.length()); // NullPointerException 발생
    }
}
```

<br /><br />

### # Error와 Exception 의 차이

- Error: 시스템 수준의 심각한 문제로, 애플리케이션이 복구할 수 없는 경우 발생 (예: OutOfMemoryError)

- Exception: 애플리케이션에서 처리할 수 있는 문제로, 프로그램이 계속 실행될 수 있는 경우 발생 (예: IOException, NullPointerException)

<br /><br />

### # 정리

| 구분   | Checked Exception | Unchecked Exception   |
|---------|------------|----------------|
| 발생 시점 | 컴파일 시점 | 런타임 시점 |
| 처리 의무 | 반드시 처리해야 함 | 반드시 처리하지 않아도 됨 |
| 주로 발생하는 상황 | 외부 자원과 관련된 예외 | 프로그래밍 오류와 관련된 예외 |
| 예시 | IOException, SQLException | NullPointerException ArrayIndexOutOfBoundsException |


<br /><br />

참고 : 
- [자바에서 Checked Exception과 Unchecked Exception에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/50)