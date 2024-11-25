---
title: "[MSA] MSA를 위한 Spring cloud 환경"
date: 2024-11-25
categories: [MSA]
tags: [TIL, MSA, Spring]
image:
  path: /assets/img/til/msa.png
---

## 📍 Spring Cloud 란?

Spring Cloud는 분산 시스템과 MSA를 쉽게 개발하고 구축하기 위한 Spring 기반의 프레임워크 모음이다.

분산 환경에서 적용될 수 있는 다양한 아키텍처 패턴들을 구현하고 있으며, 의존성 추가 및 어노테이션 추가 만으로 구성 가능하고 기존 Spring Application과 쉽게 연동할 수 있다.

<br /><br />

## # Spring Cloud 아키텍처

![img](/assets/img/til/spring_cloud.jpeg)

<br />

### # Spring Cloud API Gateway

- 마이크로서비스 아키텍처에서 Gateway로서 **클라이언트의 단일 진입점 역할을 하는 서버 역할**을 한다.
- 인증, 권한 부여, 로드 밸런싱, 라우팅, 트래픽 제어 등의 기능을 제공한다.
- 클라이언트는 API Gateway를 통해 여러 서비스에 대한 요청을 보내고, API Gateway는 이를 해당 서비스로 라우팅해준다.

<br />

### # Spring Cloud Config Server

- **설정 관리를 중앙에서 효과적으로 관리할 수 있게 해주는 서버 역할**을 한다.
- 각 마이크로서비스는 Config Server를 통해 필요한 설정 정보를 동적으로 가져올 수 있으며, 설정의 중앙 집중 관리로 유지보수 및 변경 관리를 용이하게 한다.

<br />

### # Spring Service Registry

- MSA에서 **서비스 인스턴스의 등록과 검색을 담당하는 서버 역할**을 한다.
- 서비스 인스턴스의 등록과 해제를 관리하며 서비스 디스커버리를 통해 클라이언트는 특정 서비스 인스턴스를 동적으로 찾을 수 있다.
- 예 : Netflix Eureka

<br />

### # Spring Cloud Netflix Hystrix
- 장애가 일어났을 때, **한 마이크로서비스의 장애가 다른 서비스로 전파되지 않도록 사전에 차단해주는 역할**을 한다.

<br />

### # Distributed Tracing
- 마이크로서비스 아키텍처에서 여러 서비스 간의 트랜잭션을 추적하고 모니터링하는 기술이다.
- 예 : Zipkin, Jaeger

<br />


참고 : 
- [[Spring Cloud] Microservice(MSA) 구축 : (1) MSA의 이해와 Spring Cloud의 구조](https://sjh9708.tistory.com/119)
- [[MSA] MSA를 위한 기술(1)-Spring Boot & Spring Cloud & Docker](https://velog.io/@sorzzzzy/MSA-MSA%EB%A5%BC-%EC%9C%84%ED%95%9C-%EA%B8%B0%EC%88%A01-Spring-Boot-Spring-Cloud-Docker)
- [[MSA] Spring Boot 프로젝트에서 MSA 실습하기(Spring Cloud) - 1](https://memodayoungee.tistory.com/157)