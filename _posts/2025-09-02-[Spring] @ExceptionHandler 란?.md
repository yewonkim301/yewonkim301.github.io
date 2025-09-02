---
title: "[Spring] @ExceptionHandler 란?"
date: 2025-09-02
categories: [Spring]
tags: [TIL, Spring]
---


# # @ExceptionHandler 란?

`@ExceptionHandler`는 스프링 프레임워크에서 제공하는 어노테이션으로, 특정 예외가 발생했을 때 해당 예외를 처리하는 메서드를 지정하는 데 사용된다. 이를 통해 컨트롤러 내에서 발생하는 예외를 중앙 집중식으로 관리하고, 사용자에게 적절한 응답을 반환할 수 있다.

```java
@RestController
@RequestMapping("/api")
public class MyController {
    
    @GetMapping("/example")
    public String example(@RequestParam("value") int value) {
        if (value < 0) {
            throw new IllegalArgumentException("Value must be non-negative");
        }
        return "Value is: " + value;
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
``` 

위 예제에서 `example` 메서드는 음수 값이 전달되면 `IllegalArgumentException`을 발생시킨다. 이 예외가 발생하면 `handleIllegalArgumentException` 메서드가 호출되어 적절한 HTTP 응답을 반환한다.

<br /><br />

## # @ExceptionHandler 동작 방식

`@ExceptionHandler`는 컨트롤러 클래스 내에서 특정 예외가 발생했을 때 해당 예외를 처리하는 메서드를 지정하는 데 사용된다. 이 어노테이션이 붙은 메서드는 해당 예외가 발생하면 자동으로 호출된다.

### # 동작 순서
1. **예외 발생** : 컨트롤러 메서드 내에서 예외

2. **예외 처리** : DispatcherServlet이 적절한 HandlerExceptionResolver를 찾아 예외를 처리

3. **@ExceptionHandler 메서드 호출** : 컨트롤러 클래스 내에서 `@ExceptionHandler` 어노테이션이 붙은 메서드가 있으면 해당 메서드를 호출하여 예외를 처리한다.

<br /><br />

## # @ExceptionHandler 사용 시 주의할 점

1. **컨트롤러 내에서만 사용 가능** : `@ExceptionHandler`는 컨트롤러 클래스 내에서만 사용할 수 있다. 일반적인 서비스 클래스나 다른 컴포넌트에서는 사용할 수 없다.

2. **예외 클래스 지정** : `@ExceptionHandler` 어노테이션에는 처리할 예외 클래스를 지정해야 한다. 여러 예외를 처리하려면 배열 형태로 지정할 수 있다.

```java
@ExceptionHandler({IllegalArgumentException.class, NullPointerException.class})

public ResponseEntity<String> handleMultipleExceptions(Exception ex) {
    return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
}
```

3. **글로벌 예외 처리** : 모든 컨트롤러에서 발생하는 예외를 중앙에서 처리하고 싶다면 `@ControllerAdvice` 어노테이션을 사용하여 글로벌 예외 처리 클래스를 만들 수 있다.

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAllExceptions(Exception ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```


<br /><br />

참고 : 
- [@ExceptionHandler 어노테이션은 무엇인가요?](https://www.maeil-mail.kr/question/8)
- [예외 처리(Exception Handling)](https://ss-hoon.github.io/spring/exception_handling/#:~:text=ExceptionHandler%EB%8A%94%20%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC%20%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EC%9D%B4%20%EB%B6%99%EC%96%B4%EC%9E%88%EB%8A%94%20Bean%EC%97%90%EC%84%9C%20%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94)
- [Exception 처리 방법(Exception Handler 사용법)](https://velog.io/@midas/Exception-%EC%B2%98%EB%A6%AC-%EB%B0%A9%EB%B2%95#:~:text=@ExceptionHandler.%20@Controller%20%2C%20@RestController%20%EA%B0%80%20%EC%A0%81%EC%9A%A9%EB%90%9C%20Bean,%EB%B0%9C%EC%83%9D%ED%95%9C%20%EC%98%88%EC%99%B8%EB%A5%BC%20%EB%B0%9B%EC%95%84%EC%84%9C%20%EC%B2%98%EB%A6%AC%ED%95%A0%EC%88%98%20%EC%9E%88%EB%8A%94%20%EA%B8%B0%EB%8A%A5%EC%9D%84%20%ED%95%9C%EB%8B%A4.)