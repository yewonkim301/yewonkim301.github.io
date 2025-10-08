---
title: "[Spring] @Component, @Controller, @Service, @Repository의 차이점"
date: 2025-10-08
categories: [Spring]
tags: [TIL, Spring]
---

# # `@Component`, `@Controller`, `@Service`, `@Repository`의 차이점

Spring 프레임워크에서 `@Component`, `@Controller`, `@Service`, `@Repository`는 모두 스프링의 컴포넌트 스캔 기능을 통해 빈(Bean)으로 등록되는 클래스에 사용되는 어노테이션이다. 이들 어노테이션은 각각의 역할과 목적에 따라 구분된다.

<br />

## # `@Component`

`@Component`는 가장 일반적인 스테레오타입 어노테이션으로, 스프링이 관리하는 빈으로 등록할 클래스를 나타낸다. 특정한 역할이 없는 일반적인 컴포넌트에 사용된다.

```java
@Component
public class MyComponent {
    // ...
}
```

<br />

## # `@Controller`

`@Controller`는 주로 웹 애플리케이션에서 사용되며, HTTP 요청을 처리하는 컨트롤러 클래스를 나타낸다. 이 어노테이션이 붙은 클래스는 요청을 받아서 적절한 뷰(View)를 반환하거나 데이터를 응답한다.

```java
@Controller
public class MyController {
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

<br />

## # `@Service`

`@Service`는 비즈니스 로직을 처리하는 서비스 클래스를 나타낸다. 이 어노테이션은 서비스 계층에서 사용되며, 주로 트랜잭션 관리와 같은 비즈니스 관련 작업을 수행한다.

```java
@Service
public class MyService {
    public void performBusinessLogic() {
        // 비즈니스 로직 구현
    }
}
```

<br />

## # `@Repository`

`@Repository`는 데이터 액세스 계층에서 사용되며, 데이터베이스와의 상호작용을 담당하는 클래스에 붙인다. 이 어노테이션은 데이터베이스 예외를 스프링의 데이터 접근 예외로 변환하는 기능을 제공한다.

```java
@Repository
public class MyRepository {
    public void saveData() {
        // 데이터 저장 로직 구현
    }
}
```

<br />

## # 정리

| 어노테이션       | 역할       | 사용 위치   | 특징           |
|------------------|------------|-------------|----------------|
| `@Component`     | 컴포넌트   | 일반 클래스  | 일반적인 빈 등록 |
| `@Controller`    | 컨트롤러   | 프레젠테이션 계층 | HTTP 요청 처리  |
| `@Service`       | 서비스     | 비즈니스 계층 | 비즈니스 로직 처리 |
| `@Repository`    | 리포지토리 | 데이터 액세스 계층 | 데이터베이스 예외 변환 |


<br /><br />

## # `@Controller`, `@Repository` 대신 `@Component` 사용하면 안되는 이유

**Spring 6(Spring Boot 3) 이전 버전**에서는 **@Component + @RequestMapping**으로도 Bean 및 핸들러로 등록되었다. 하지만 **Spring 6 이후** 부터 **@Controller** 외에는 핸들러로 등록하지 않아 웹 요청을 정상적으로 수행할 수 없다.

또한, **@Repository**는 데이터베이스 예외를 스프링의 데이터 접근 예외로 변환하는 기능을 제공하기 때문에, 이 어노테이션을 사용하지 않고 **@Component**로 대체할 경우 예외 처리가 제대로 이루어지지 않을 수 있다.

<br /><br />

참고 : 
- [@Component, @Controller, @Service, @Repository의 차이점에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/72)