---
title: "[Spring] Spring Actuator 란?"
date: 2024-12-19
categories: [Spring]
tags: [TIL, Spring]
---

## Spring Boot Actuator 란?

애플리케이션의 모니터링과 관리를 위한 다양한 엔드포인트를 제공하는 도구이다. 이를 통해 애플리케이션의 상태를 확인하고, 성능을 모니터링하며, 설정 정보를 조회하는 등의 작업을 할 수 있다.

<br /><br />

### 주요 엔드포인트

- `/actuator/health` : 애플리케이션의 상태를 확인한다. 기본적으로 활성화되어 있으며, 로드 밸런서 등의 헬스 체크에 사용된다.

- `/actuator/info` : 애플리케이션의 일반적인 정보를 제공한다.

- `/actuator/metrics` : 애플리케이션의 각종 메트릭 정보를 확인할 수 있다.

- `/actuator/env` : 애플리케이션의 환경 변수와 설정 정보를 조회한다. 민감한 정보가 포함될 수 있으므로 주의가 필요하다.

<br /><br />

### 보안 고려사항

Actuator 엔드포인트는 민감한 정보를 포함할 수 있으므로, 보안 설정을 해주어야 한다.

- **엔드포인트 노출 제한** : 필요한 엔드포인트만 노출하도록 설정한다. 예를 들어, `application.properties` 파일에 다음과 같이 설정할 수 있다:

    ```
    management.endpoints.web.exposure.include=health,info
    ```

    이를 통해 `/actuator/health`와 `/actuator/info` 엔드포인트만 외부에 노출된다.

- **민감한 엔드포인트 비활성화** : `/actuator/shutdown`과 같이 애플리케이션을 종료시킬 수 있는 엔드포인트는 기본적으로 비활성화되어 있으며, 활성화하지 않는 것이 좋다.

- **접근 제어** : Actuator 엔드포인트에 대한 접근 권한을 설정하여 인증된 사용자만 접근할 수 있도록 구성한다.

<br /><br />

### 헬스 체크 구성

`/actuator/health` 엔드포인트는 애플리케이션의 상태를 확인하는 데 사용된다. 기본적으로 애플리케이션이 정상적으로 동작하는지 확인하지만, 데이터베이스 연결 상태 등 추가적인 헬스 체크를 구성할 수 있다. 이를 위해 커스텀 `HealthIndicator`를 구현하거나, 기본 제공되는 `HealthIndicator`를 활용할 수 있다.

<br /><br />

### 마무리

Spring Boot Actuator는 애플리케이션의 상태를 모니터링하고 관리하는 데 유용한 도구이지만, 보안 설정을 신중하게 구성하여 민감한 정보의 노출을 방지하는 것이 중요하다.

<br /><br />

참고 : 
- [Spring Actuator 란 무엇일까](https://tweety1121.tistory.com/entry/Spring-Actuator-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C#google_vignette)
- [Spring Boot Actuator의 헬스체크 살펴보기](https://toss.tech/article/how-to-work-health-check-in-spring-boot-actuator)
- [Actuator 안전하게 사용하기](https://techblog.woowahan.com/9232/)

