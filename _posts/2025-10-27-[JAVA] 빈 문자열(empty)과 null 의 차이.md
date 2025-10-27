---
title: "[JAVA] 빈 문자열(empty)과 null 의 차이"
date: 2025-10-27
categories: [JAVA]
tags: [TIL, JAVA]
---

# # 빈 문자열(empty)과 null의 차이

Java에서 빈 문자열(empty)과 null은 서로 다른 개념이다. 이 둘의 차이점에 대해 알아보도록 하자.

## 1. 빈 문자열(empty)

빈 문자열은 **길이가 0인 문자열**을 의미한다. 즉, **아무런 문자도 포함하지 않는 문자열**이다. Java에서는 빈 문자열을 다음과 같이 표현할 수 있다.

```java
String emptyString = "";
```

빈 문자열은 메모리 상에 존재하며, 문자열 객체로서의 특성을 가진다. 

<br />

### # 정리 : 빈 문자열의 특징

- 길이(length)가 0이다. (`emptyString.length() == 0`)
- 문자열 연산이 가능하다. 예를 들어, 다른 문자열과 연결(concatenation)할 수 있다. (`emptyString + "Hello"` 결과는 `"Hello"`)

<br /><br />

## 2. null

null은 **객체가 존재하지 않음**을 나타내는 값이다. 즉, null은 **어떤 객체도 참조하지 않는 상태**를 의미한다. Java에서 null은 다음과 같이 표현할 수 있다.

```java
String nullString = null;
```

null은 메모리 상에 객체가 존재하지 않기 때문에, null 상태의 변수에 대해 문자열 연산을 시도하면 `NullPointerException`이 발생한다. 예를 들어:

```java
String nullString = null;
System.out.println(nullString.length()); // NullPointerException 발생
```

<br />

### # 정리 : null의 특징

- 객체가 존재하지 않음을 나타낸다.

- 문자열 연산을 시도하면 `NullPointerException`이 발생한다.

<br /><br />

참고 : 
- [Java 문자열 빈 값 vs 공백 vs null 비교](https://co1nam.tistory.com/17#google_vignette)
- [java null, 공백 차이](https://itrh.tistory.com/entry/java-null-%EA%B3%B5%EB%B0%B1-%EC%B0%A8%EC%9D%B4#google_vignette)