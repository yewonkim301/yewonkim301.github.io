---
title: "[Spring] @ResponseBody와 @ResponseEntity 란?"
date: 2025-09-03
categories: [Spring]
tags: [TIL, Spring]
---


# # @ResponseBody와 @ResponseEntity<T>

## 1. @ResponseBody

- `@ResponseBody` 어노테이션은 Spring MVC에서 컨트롤러 메서드의 반환 값을 HTTP 응답 본문에 직접 매핑하는 데 사용된다.

- 주로 RESTful 웹 서비스에서 JSON 또는 XML 형식의 데이터를 반환할 때 사용된다.

- `@ResponseBody` 어노테이션이 적용된 메서드는 반환 값을 뷰(View)로 해석하지 않고, HTTP 응답 본문에 직접 작성한다.

- 예시:
```java
@RestController
public class MyController {
    
    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello, World!";
    }
}
```
- 위의 예시에서 `/hello` 엔드포인트에 GET 요청을 보내면 "Hello, World!"라는 문자열이 HTTP 응답 본문에 직접 포함되어 반환된다.

<br />

### # 참고 : @RestController 와 @Controller 차이

- `@RestController`는 `@Controller`와 `@ResponseBody`를 결합한 어노테이션으로, 클래스 레벨에 적용되어 모든 메서드에 `@ResponseBody`가 자동으로 적용된다.

- **따라서**, `@RestController`를 사용하면 각 메서드에 `@ResponseBody`를 명시적으로 추가할 필요가 없다.

- 예시:
```java
@RestController
public class MyController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

<br /><br />

## 2. ResponseEntity<T>

- `ResponseEntity<T>`는 Spring MVC에서 HTTP 응답 전체를 나타내는 클래스이다.

- `ResponseEntity<T>`는 `HttpEntity<T>`를 상속하기 때문에 HTTP 상태 코드, 헤더, 본문을 모두 포함할 수 있으며, 보다 세밀한 제어가 가능하다.

- 예시:
```java
@RestController
public class MyController {
    
    @GetMapping("/hello")
    public ResponseEntity<String> hello() {
        return ResponseEntity
                .status(HttpStatus.OK) // 상태 코드 설정
                .header("Custom-Header", "value") // 헤더 설정
                .body("Hello, World!"); // 본문 설정
    }
}
```
- 위의 예시에서 `/hello` 엔드포인트에 GET 요청을 보내면 HTTP 상태 코드 200(OK), 커스텀 헤더, 그리고 "Hello, World!"라는 본문이 포함된 응답이 반환된다.

<br />

### # @ResponseBody vs ResponseEntity<T>

- **@ResponseBody** : 메서드의 반환 값을 HTTP 응답 본문에 직접 매핑하는 데 사용되며, 주로 단순한 데이터 반환에 적합
    - 장점 : 어노테이션 추가만으로 간단하게 HTTP 응답을 만들 수 있음
    - 단점 : 상태 코드와 헤더를 제어하기 어려움

- **ResponseEntity<T>** : HTTP 응답 전체를 제어할 수 있어, 상태 코드와 헤더 설정이 필요한 경우에 더 유용
    - 장점 : 상태 코드, 헤더, 본문을 모두 제어할 수 있어 유연함
    - 단점 : 코드가 다소 복잡해질 수 있음

- **따라서**, 단순한 데이터 반환에는 @ResponseBody를, 상태 코드와 헤더를 포함한 복잡한 응답이 필요한 경우에는 ResponseEntity<T>를 사용하는 것이 좋다.

<br /><br />

참고 : 
- [@ResponseBody(or ResponseEntity<T>)가 있을 때와 없을 때의 동작 방식의 차이점을 말해주세요.](https://www.maeil-mail.kr/question/9)
- [@ResponseBody, @ResponseEntity and customized response object in Spring](https://medium.com/@wellsbabo/responsebody-responseentity-and-customized-response-object-in-spring-641f00b0e401)
- [[Spring] @ResponseBody VS ResponseEntity<T> : 무엇을 사용할까?](https://ksh-coding.tistory.com/89)