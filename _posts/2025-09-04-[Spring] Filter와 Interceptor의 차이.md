---
title: "[Spring] Filter와 Interceptor의 차이"
date: 2025-09-04
categories: [Spring]
tags: [TIL, Spring]
---


# # Filter(필터)

필터는 **서블릿 컨테이너에서** 제공하는 기능으로, **클라이언트의 요청이 서블릿에 도달하기 전에 요청을 가로채서 처리할 수 있는 컴포넌트**이다. 필터는 주로 인증, 로깅, 데이터 압축, 인코딩 설정 등과 같은 공통적인 작업을 수행하는 데 사용된다.

![img](/assets/img/til/cs/filter.png)

<br />

### # 특징

- **서블릿 컨테이너 레벨**: 필터는 서블릿 컨테이너(예: Tomacat)에서 동작하며, 모든 서블릿 요청에 대해 적용될 수 있다.

- **요청 및 응답 처리**: 필터는 요청이 서블릿에 도달하기 전과 응답이 클라이언트로 돌아가기 전에 모두 처리할 수 있다.

- **체인 구조**: 여러 개의 필터를 체인 형태로 연결하여 순차적으로 처리할 수 있다.

- **구현 인터페이스**: `javax.servlet.Filter` 인터페이스를 구현하여 필터를 정의한다.

- **순서**: `web.xml`이나 `@WebFilter` 애노테이션을 통해 설정할 수 있으며, 필터의 순서는 설정 파일에서 정의한다.

```java
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // 요청 처리 전 작업
        chain.doFilter(request, response); // 다음 필터 또는 서블릿 호출
        // 응답 처리 후 작업
    }
}
```

<br /><br />

# # Interceptor(인터셉터)

인터셉터는 **스프링 프레임워크에서** 제공하는 기능으로, **컨트롤러에 요청이 도달하기 전에 가로채서 처리할 수 있는 컴포넌트**이다. 인터셉터는 주로 인증, 권한 부여, 로깅, 성능 모니터링 등과 같은 작업을 수행하는 데 사용된다.

![img](/assets/img/til/cs/interceptor.png)

<br />

### # 특징

- **스프링 프레임워크 레벨**: 인터셉터는 Spring MVC의 핸들러 수준에서 동작하며, 특정 컨트롤러 또는 URL 패턴에 대해 적용될 수 있다.

- **요청 처리 전후 작업**: 인터셉터는 요청이 컨트롤러에 도달하기 전과 컨트롤러가 응답을 반환한 후 모두 처리할 수 있다.

- **체인 구조**: 여러 개의 인터셉터를 체인 형태로 연결하여 순차적으로 처리할 수 있다.

- **구현 인터페이스**: `HandlerInterceptor` 인터페이스를 구현하여 인터셉터를 정의한다.

- **순서**: `WebMvcConfigurer`를 통해 설정할 수 있으며, 인터셉터의 순서는 설정 파일에서 정의한다.

```java
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        // 컨트롤러의 메서드가 호출되기 전에 실행
        return true; // true를 반환하면 다음 인터셉터 또는 컨트롤러 호출
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           ModelAndView modelAndView) throws Exception {
        // 컨트롤러의 메서드가 실행된 후, 뷰가 렌더링되기 전에 실행
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                                Exception ex) throws Exception {
        // 뷰가 렌더링된 후 실행
    }
}
```

<br /><br />

# # Filter VS Interceptor

| **특징**   | **Filter(필터)**    | **Interceptor(인터셉터)**  |
|--------|-----------------|-------------------------|
| **동작 레벨** | 서블릿 컨테이너 레벨 | 스프링 |
| **적용 대상** | 모든 서블릿 요청 | 특정 컨트롤러 또는 URL 패턴 |
| **구현 인터페이스** | `javax.servlet.Filter` | `HandlerInterceptor` |
| **실행 순서** | 서블릿 이전/이후, 서블릿 필터 | 컨트롤러 이전이나 이후에는 스프링 인터셉터가 필터 이후까지 작동하지 않는다. |
| **설정 방법** | `web.xml` 또는 `@WebFilter` | `WebMvcConfigurer` |
| **Request/Reponse 객체 조작 가능 여부** | O | X |
| **주요 용도** | 인증, 로깅, 인코딩 설정 등 | 인증, 권한 부여, 로깅 등 |


<br /><br />

참고 : 
- [Filter와 Interceptor의 차이점을 말해주세요.](https://www.maeil-mail.kr/question/10)
- [[Spring] 필터(Filter)와 인터셉터(Interceptor)의 개념 및 차이](https://dev-coco.tistory.com/173)