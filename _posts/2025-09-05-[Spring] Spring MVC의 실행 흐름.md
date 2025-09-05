---
title: "[Spring] Spring MVC의 실행 흐름"
date: 2025-09-05
categories: [Spring]
tags: [TIL, Spring]
---


# # Spring MVC의 실행 흐름

Spring MVC는 **Model-View-Controller** 아키텍처 패턴을 따르는 웹 프레임워크로, 클라이언트의 요청을 처리하고 응답을 생성하는 과정을 여러 단계로 나누어 관리한다. Spring MVC의 실행 흐름은 다음과 같은 주요 단계로 구성된다.

![img](/assets/img/til/cs/mvc.png)
<br />

**1. 클라이언트 요청**
클라이언트(웹 브라우저 등)가 특정 URL로 HTTP 요청을 보낸다. 이 요청은 웹 서버(예: Tomcat)로 전달된다.
<br />

**2. DispatcherServlet**
웹 서버는 모든 요청을 `DispatcherServlet`으로 전달한다. 이때 `DispatcherServlet`이 **프론트 컨트롤러**의 역할을 수행한다.
<br />

**3. HandlerMapping**
`DispatcherServlet`은 `HandlerMapping`을 사용하여 요청 URL에 매핑된 컨트롤러(Handler)를 찾는다. `HandlerMapping`은 URL 패턴과 컨트롤러 메서드 간의 매핑 정보를 관리한다.
<br />

**4. Interceptor (선택 사항)**
요청이 컨트롤러에 도달하기 전에 `HandlerInterceptor`가 등록되어 있다면, 인터셉터가 실행된다. 인터셉터는 요청 전처리, 후처리, 완료 후 작업 등을 수행할 수 있다.
<br />

**5. Controller**
`DispatcherServlet`은 매핑된 컨트롤러의 메서드를 호출하여 요청을 처리한다. 컨트롤러는 비즈니스 로직을 수행하고, 모델 데이터를 생성하거나 수정한다.
<br />

**6. Service & Repository**
컨트롤러는 필요에 따라 서비스 계층(Service Layer)과 데이터 접근 계층(Repository Layer)를 호출하여 비즈니스 로직을 처리하고, 데이터베이스와 상호작용한다.
<br />

**. Model**
컨트롤러는 처리 결과를 모델(Model) 객체에 담아 `DispatcherServlet`에게 반환한다. 모델은 뷰(View)에서 사용할 데이터를 포함한다.
<br />

**8. ViewResolver**
`DispatcherServlet`은 `ViewResolver`를 사용하여 모델 데이터를 표시할 뷰(View)를 결정한다. 뷰는 JSP, Thymeleaf, FreeMarker 등 다양한 템플릿 엔진을 사용할 수 있다.
<br />

**9. View**
`ViewResolver`가 결정한 뷰가 렌더링되어 모델 데이터를 포함한 HTML 응답이 생성된다.
<br />

**10.  클라이언트 응답**
생성된 HTML 응답이 `DispatcherServlet`을 통해 클라이언트로 전달된다. 클라이언트는 이 응답을 받아 화면에 표시한다.
<br />

![img](/assets/img/til/cs/mvc2.png)

<br /><br />

참고 : 
- [Spring MVC의 실행 흐름에 대해 설명해주세요.](https://www.maeil-mail.kr/question/11)
- [[Spring] 스프링 MVC 프로젝트의 기본 구조와 동작 순서](https://hpark3.tistory.com/28)
