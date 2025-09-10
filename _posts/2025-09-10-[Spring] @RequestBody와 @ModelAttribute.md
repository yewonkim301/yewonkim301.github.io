---
title: "[Spring] @RequestBody와 @ModelAttribute"
date: 2025-09-10
categories: [Spring]
tags: [TIL, Spring]
---

`@RequestBody`와 `@ModelAttribute`는 클라이언트 측에서 보낸 데이터를 Java 객체로 만들어준다.

<br /><br />

# # @RequestBody

`@RequestBody`는 HTTP 요청의 본문(body)에 담긴 JSON, XML 등의 데이터를 Java 객체로 변환하는 데 사용된다. 주로 RESTful API에서 클라이언트가 서버로 데이터를 보낼 때 사용된다. `@RequestBody`는 주로 POST, PUT, PATCH 요청과 함께 사용된다.

내부적으로 HttpMessageConverter를 거치는데, 이때 ObjectMapper를 통해 JSON 값을 java 객체로 역직렬화한다. 따라서 변환될 java 객체에 기본 생성자를 정의해야 하고, getter나 setter를 선언해야 한다.

<br />

**예시**

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    // user 객체를 사용하여 사용자 생성 로직 수행
    return ResponseEntity.ok(user);
}
```

<br /><br />

# # @ModelAttribute

`@ModelAttribute`는 주로 폼 데이터를 처리하는 데 사용된다. 클라이언트가 폼을 통해 데이터를 제출할 때, `@ModelAttribute`는 요청 파라미터를 Java 객체의 필드에 바인딩한다. 주로 GET, POST 요청과 함께 사용된다.

내부적으로는 스프링의 데이터 바인딩 기능을 사용하여 요청 파라미터를 자바 객체에 바인딩한다. 따라서 자바 객체에 기본 생성자와 getter/setter가 필요하다.

<br />

### # 두 가지 사용법

1. 메서드 단에서의 사용법은 jsp의 Model에 하나 이상의 속성을 추가하고 싶을 때 사용

```java
@ModelAttribute
public void addAttributes(Model model) {
    model.addAttribute("attributeName", "attributeValue"); // model.addAttribute(“속성 이름”, “속성 값”)
}
```

2. 메서드 매개변수 단에서의 사용법은 폼 데이터를 자바 객체에 바인딩할 때 사용

```java
@PostMapping("/submit")
public String submitForm(@ModelAttribute FormData formData) {
    // formData 객체를 사용하여 폼 데이터 처리 로직 수행
    return "result";
}

<br /><br />

### # 주요 차이점

| 특성  | @RequestBody    | @ModelAttribute   |
|-------|-----------------|-------------------|
| 데이터 위치 | HTTP 요청 본문 (body) | 요청 파라미터 (query
| 데이터 형식 | JSON, XML 등 | 폼 데이터 (application/x-www-form-urlencoded) |
| HTTP 메서드 | 주로 POST, PUT, PATCH | 주로 GET, POST |
| 데이터 바인딩 방식 | HttpMessageConverter 사용 | 스프링의 데이터 바인딩 사용 |
| 기본 생성자 필요 | O | O |
| Getter/Setter 필요 | O | O |
| 다중 값 처리 | JSON 배열 등 | 리스트 형태로 바인딩 가능 |
| 폼 데이터 처리 | X | O |
| 파일 업로드 처리 | X | O (MultipartFile 사용) |
| 예외 처리 | HttpMessageNotReadableException | BindException |
| 유효성 검사 | @Valid, @Validated 사용 가능 | @Valid, @Validated 사용 가능 |
| 스프링 MVC 통합 | RESTful 웹 서비스에 적합 | 전통적인 웹 애플리케이션에 적합 |
| 바인딩 대상 | 단일 객체 또는 컬렉션 | 단일 객체 또는 컬렉션 |
| 기본 값 설정 | X | O (defaultValue 속성 사용) |
| 커스텀 바인딩 | X | O (WebDataBinder 사용) |
| 세션 속성 | X | O (SessionAttributes 사용) |
| 폼 검증 | X | O (BindingResult 사용) |

<br /><br />

참고 : 
- [RequestBody VS ModelAttribute의 차이점을 말해주세요.](https://www.maeil-mail.kr/question/14)