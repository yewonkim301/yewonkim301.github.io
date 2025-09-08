---
title: "[Spring] @Controller와 @RestController 차이"
date: 2025-09-08
categories: [Spring]
tags: [TIL, Spring]
---

Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 `@Controller`와 `@RestController`가 있다. 이 두 가지 어노테이션의 주요 차이점은 **HTTP ResponseBody가 생성되는 방식**이다.

<br /><br />

# # @Controller

`@Controller`는 **Spring MVC에서 사용**되는 어노테이션으로, 주로 웹 애플리케이션에서 요청을 처리하고 **뷰(View)를 반환하는 역할**을 한다.

### 1. View 반환

`@Controller`는 주로 HTML, JSP, Thymeleaf 등의 뷰를 반환하는 데 사용된다. 메서드가 문자열을 반환하면, 이는 뷰 이름으로 해석되어 해당 뷰가 렌더링된다.

![img](/assets/img/til/cs/@controller_view.png)

**작동 순서**
1. 클라이언트가 HTTP 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter를 통해 요청을 Controller로 위임한다.
4. Controller에서 요청을 처리한 후 ViewName을 반환한다.
5. DispatcherServlet은 ViewResolver를 통해 ViewName에 해당하는 View를 찾아 사용자에게 반환한다.

<br />

**예제 코드**
```java
@Controller
public class MyController {
    
    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "hello"; // hello.html 또는 hello.jsp 뷰를 반환
    }
}
```

### 2. Data 반환

`@Controller`에서 JSON, XML 등의 데이터를 반환하려면, 메서드에 `@ResponseBody` 어노테이션을 추가해야 한다. 이 경우, 반환된 데이터는 HTTP Response Body에 직접 작성된다.

![img](/assets/img/til/cs/@controller_data.png)

**작동 순서**
1. 클라이언트가 HTTP 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter를 통해 요청을 Controller로 위임한다.
4. Controller에서 요청을 처리한 후 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

<br />

**예제 코드**
```java
@Controller
@RequiredArgsConstructor
public class MyController {

    private final DataService dataService;
    
    @GetMapping("/data")
    @ResponseBody
    public ResponseEntity<MyData> getData(@RequestParam String dataName) {
        return new ResponseEntity.ok(dataService.findData(dataName)); // JSON 형태로 반환
    }
}
```
→ 컨트롤러를 통해 객체를 반환할 때에는 일반적으로 `ResponseEntity`로 감싸서 반환한다.


<br /><br />

# # @RestController

`@RestController`는 `@Controller`와 `@ResponseBody`를 결합한 어노테이션으로, **RESTful 웹 서비스**를 개발할 때 주로 사용된다. 이 어노테이션이 붙은 클래스의 모든 메서드는 기본적으로 HTTP Response Body에 데이터를 직접 작성한다.

![img](/assets/img/til/cs/@restController.png)

**작동 순서**
1. 클라이언트가 HTTP 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter를 통해 요청을 Controller로 위임한다.
4. Controller에서 요청을 처리한 후 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

<br />

**예제 코드**
```java
@RestController
@RequiredArgsConstructor
public class MyRestController {

    private final DataService dataService;
    
    @GetMapping("/data")
    public ResponseEntity<MyData> getData(@RequestParam String dataName) {
        return new ResponseEntity.ok(dataService.findData(dataName)); // JSON 형태로 반환
    }
}
```
→ `@RestController`에서는 `@ResponseBody`를 생략할 수 있다.

→ HTTP 상태 코드, 헤더 등을 포함하여 응답을 세밀하게 제어하기 위해 `ResponseEntity`로 감싸서 반환한다.

<br /><br />

참고 : 
- [@Controller 와 @RestController 의 차이점을 설명해주세요.](https://www.maeil-mail.kr/question/12)
- [[Spring] @Controller와 @RestController 차이](https://mangkyu.tistory.com/49)
