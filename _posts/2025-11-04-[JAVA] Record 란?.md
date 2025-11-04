---
title: "[JAVA] Record 란?"
date: 2025-11-04
categories: [JAVA]
tags: [TIL, JAVA]
---

# # Record 란?

Java 16에서 정식으로 도입된 Record는 불변(immutable) 데이터 객체를 간편하게 정의할 수 있는 특별한 클래스이다. Record를 사용하면 데이터 객체를 정의할 때 반복적인 코드를 줄이고, 코드의 가독성을 높일 수 있다.

<br />

## # Record의 특징

1. **불변성(Immutable)**: Record의 필드는 기본적으로 final로 선언되어, 객체가 생성된 후에는 값을 변경할 수 없다.

2. **자동 생성 메서드**: Record를 정의하면 컴파일러가 자동으로 생성자, 접근자(getter), equals(), hashCode(), toString() 메서드를 생성해준다.

3. **간결한 문법**: Record는 클래스를 정의할 때 필요한 코드를 최소화하여, 데이터 객체를 더 간결하게 표현할 수 있다.

<br />

## # Record 사용법

```java
public record Person(String name, int age) {}
```

위 예제에서 `Person`이라는 Record는 `name`과 `age`라는 두 개의 필드를 가진다. 

- 컴파일러에서 자동으로 생성되는 메서드
    - 생성자: `public Person(String name, int age)`

    - 접근자: `public String name()`, `public int age()`

    - equals(), hashCode(), toString() 메서드

- Record를 사용 예시:

```java
public class Main {
    public static void main(String[] args) {
        Person person = new Person("Alice", 30);
        
        // 접근자 메서드 사용
        System.out.println("Name: " + person.name());
        System.out.println("Age: " + person.age());
        
        // toString() 메서드 사용
        System.out.println(person);
    }
}
```

- 출력 결과:
```
Name: Alice
Age: 30
Person[name=Alice, age=30]
```

<br />

## # Record의 활용

Record는 주로 데이터 전송 객체(DTO), 값 객체(Value Object), 설정 객체(Configuration Object) 등 불변 데이터를 표현하는 데 사용된다. 예를 들어, API 응답 데이터를 표현할 때 Record를 사용하면 코드가 더 간결해지고 유지보수가 쉬워진다.

```java
public record ApiResponse(int statusCode, String message, Object data) {}
```

Record는 Java에서 불변 데이터를 다루는 데 매우 유용한 도구로, 코드의 간결성과 가독성을 크게 향상시킨다. 따라서 데이터 객체를 정의할 때 Record를 적극적으로 활용하는 것이 좋다.

<br /><br />

참고 : 
- [Record를 DTO로 사용하는 이유가 뭔가요?](https://www.maeil-mail.kr/question/107)
