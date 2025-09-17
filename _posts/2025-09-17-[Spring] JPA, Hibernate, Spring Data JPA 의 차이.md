---
title: "[Spring] JPA, Hibernate, Spring Data JPA 의 차이"
date: 2025-09-17
categories: [Spring]
tags: [TIL, Spring]
---

# # JPA, Hibernate, Spring Data JPA 의 차이

JPA(Java Persistence API), Hibernate, Spring Data JPA는 모두 자바 애플리케이션에서 데이터베이스와 상호작용하기 위한 기술이지만, 각각의 역할과 기능에는 차이가 있다.

<br /><br />

## # JPA (Java Persistence API)

JPA는 **자바에서 객체와 관계형 데이터베이스 간의 매핑을 정의하는 표준 인터페이스**이다. JPA는 ORM(Object-Relational Mapping) 기술의 일종으로, 자바 객체를 데이터베이스 테이블에 매핑하고, 데이터베이스 작업을 추상화하여 개발자가 SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있도록 한다.

JPA는 인터페이스로서, 실제 구현체가 필요하다. 대표적인 JPA 구현체로는 Hibernate, EclipseLink, OpenJPA 등이 있다.

<br /><br />

## # Hibernate

Hibernate는 **JPA의 대표적인 구현체 중 하나**로, JPA 인터페이스를 구현하여 ORM 기능을 제공한다. Hibernate는 JPA의 표준 기능 외에도 다양한 추가 기능과 최적화 옵션을 제공하여 더 강력한 ORM 솔루션을 제공한다.

Hibernate는 JPA를 사용하여 데이터베이스와 상호작용하는 방법을 제공하며, JPA의 모든 기능을 지원한다. 또한, Hibernate는 자체적인 쿼리 언어인 HQL(Hibernate Query Language)을 제공하여 복잡한 쿼리를 작성할 수 있도록 한다.

<br /><br />

## # Spring Data JPA

Spring Data JPA는 **Spring Framework에서 제공하는 데이터 접근 계층을 간소화하기 위한 라이브러리**이다. Spring Data JPA는 JPA를 기반으로 하며, JPA와 Hibernate를 쉽게 사용할 수 있도록 다양한 기능을 제공한다.

Spring Data JPA는 리포지토리 패턴을 구현하여, 데이터베이스 작업을 위한 인터페이스를 자동으로 생성하고, CRUD(Create, Read, Update, Delete) 작업을 간편하게 수행할 수 있도록 한다. 또한, 쿼리 메서드(Query Method)를 통해 메서드 이름만으로도 복잡한 쿼리를 작성할 수 있는 기능을 제공한다.

<br /><br />

### # 정리

- **JPA**: 자바에서 객체와 관계형 데이터베이스 간의 매핑을 정의하는 표준 인터페이스.

- **Hibernate**: JPA의 대표적인 구현체로, ORM 기능을 제공하며 JPA의 모든 기능을 지원한다.

- **Spring Data JPA**: Spring Framework에서 제공하는 라이브러리로, JPA와 Hibernate를 쉽게 사용할 수 있도록 다양한 기능을 제공하며, 데이터 접근 계층을 간소화한다.

<br /><br />

참고 : 
- [JPA, Hibernate, Spring Data JPA 의 차이가 무엇인가요?](https://www.maeil-mail.kr/question/26)