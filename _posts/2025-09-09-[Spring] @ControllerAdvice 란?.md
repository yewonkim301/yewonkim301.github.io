---
title: "[Spring] @ControllerAdvice 란?"
date: 2025-09-09
categories: [Spring]
tags: [TIL, Spring]
---

# # @ControllerAdvice 란?

`@ControllerAdvice`는 **모든 컨트롤러에 대해 전역 기능을 제공하는 어노테이션**이며, Spring 3.2에서 도입되었다. `@ControllerAdvice`가 선언된 클래스에 `@ExceptionHandler`, `@InitBinder`, `@ModelAttribute`를 등록하면 예외 처리, 바인딩 등을 한 곳에서 처리할 수 있어, 코드의 중복을 줄이고 유지보수성을 높일 수 있다.

<br />

### # 주요 기능

1. **예외 처리**: `@ExceptionHandler` 메서드를 사용하여 특정 예외가 발생했을 때 공통적으로 처리할 수 있다.

2. **바인딩 설정**: `@InitBinder` 메서드를 사용하여 모든 컨트롤러에서 공통적으로 사용할 바인딩 설정을 정의할 수 있다.

3. **모델 속성 추가**: `@ModelAttribute` 메서드를 사용하여 모든 컨트롤러에 공통적으로 추가할 모델 속성을 정의할 수 있다.

<br />

## 예시 코드

```java
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.ui.Model;
import org.springframework.http.ResponseEntity;

@ControllerAdvice
public class GlobalControllerAdvice {
    
    // 예외 처리
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAllExceptions(Exception ex) {
        return ResponseEntity.status(500).body("An error occurred: " + ex.getMessage());
    }

    // 바인딩 설정
    @InitBinder
    public void initBinder(WebDataBinder binder) {
        // 공통 바인딩 설정
    }

    // 모델 속성 추가
    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("globalAttribute", "This is a global attribute");
    }
}
```
<br /><br />

### # @RestControllerAdvice

`@RestControllerAdvice`는 `@ControllerAdvice`와 동일한 기능을 제공하지만, `@ResponseBody`가 기본적으로 적용되어 있어 JSON 형식의 응답을 반환하는 RESTful 웹 서비스에 적합하다.

```java
import org.springframework.web.bind.annotation.RestControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.RestController;

@RestControllerAdvice
public class GlobalRestControllerAdvice {
    
    @ExceptionHandler(Exception.class)
    @ResponseBody
    public ResponseEntity<String> handleAllExceptions(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body("An error occurred: " + ex.getMessage());
    }
}
```

<br /><br />

## 요약

- `@ControllerAdvice`는 모든 컨트롤러에 대해 전역 기능을 제공하는 어노테이션이다.
- 예외 처리, 바인딩 설정, 모델 속성 추가 등 다양한 기능을 한 곳에서 관리할 수 있다.
- 코드의 중복을 줄이고 유지보수성을 높이는 데 유용하다.

<br /><br />

참고 : 
- [ControllerAdvice에 대해 설명해주세요.](https://www.maeil-mail.kr/question/13)
- [[Java, Spring] @ControllerAdvice, @RestControllerAdvice란 ?](https://chanhan.tistory.com/entry/Java-Spring-ControllerAdvice-RestControllerAdvice%EB%9E%80)