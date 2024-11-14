---
title: "[JPA] N+1 문제와 leftJoin, fetchJoin"
date: 2024-11-14
categories: [JPA]
tags: [TIL, Querydsl, JPA]
---

## 📍 N+1 문제란?

- `N+1` 문제는 데이터베이스에서 여러 개의 데이터를 조회할 때 발생하는 성능 문제이다. 주로 연관된 엔티티들을 조회할 때 발생하며, 기본적으로 여러 개의 쿼리가 불필요하게 실행되면서 발생한다.
  
- 예를 들어서 `N`개의 `Restaurant`를 조회하는 경우, 각 `Restaurant`마다 연관된 `Category`나 `Owner`, `Review`를 조회한다고 하면
  - 1개의 쿼리로 Restaurant `N`개를 조회하고
  - 각 Restaurant에 대해 Category, Owner, Review를 조회하는 `N`개의 추가 쿼리가 실행된다.

<br />

**이러한 문제를 해결하기 위해 `fetch join`과 `left join`을 사용할 수 있다.**

<br /><br />

## 📍 `fetch join` 이란?

- `fetch join`은 연관된 엔티티들을 한 번의 쿼리로 가져오는 방식이다.
  
- `fetch join`을 사용하면, 연관된 엔티티들을 **즉시 로딩(Eager Loading)**하여 추가적인 쿼리를 실행하지 않도록 할 수 있다.
  
- **단점** : 모든 연관된 엔티티들을 `select`문에 명시적으로 포함시켜야 한다. 다시 말해서 `select` 구문에서 모든 필드를 지정해야 하기 때문에 쿼리가 복잡해질 수 있다.

<br />

### 사용 예시
```
queryFactory
    .selectFrom(qRestaurant)
    .leftJoin(qRestaurant.category).fetchJoin()  // fetch join을 사용하여 연관된 category를 한 번에 가져옴
    .leftJoin(qRestaurant.owner).fetchJoin()     // fetch join을 사용하여 연관된 owner를 한 번에 가져옴
    .fetch();
```

<br /><br />

## 📍 `left join` 이란?
- `left join`은 SQL에서 두 테이블을 조인할 때 사용되는 연산자이다.
  - 왼쪽 테이블의 모든 행을 반환하고, 오른쪽 테이블과 일치하는 행이 있으면 그 데이터를 포함한다.
  - 오른쪽 테이블에 일치하는 행이 없으면 `NULL`을 반환한다.
  
- **주의할 점** : `left join`은 `fetch join`과 달리 연관된 엔티티를 반드시 `select`하지 않으면 여러 개의 쿼리가 실행될 수 있다. 이 경우 추가적인 쿼리 실행이 발생할 수 있어 `N+1` 문제가 발생할 수 있다.

![img](/assets/img/til/left_join.png)

<br />

### 사용 예시
```
queryFactory
    .selectFrom(qRestaurant)
    .leftJoin(qRestaurant.category)  // left join을 사용하여 연관된 category를 가져옴
    .leftJoin(qRestaurant.owner)     // left join을 사용하여 연관된 owner를 가져옴
    .fetch();
```

<br /><br />

## 📍 정리
- `fetch join`을 사용하면 연관된 엔티티들을 한 번의 쿼리로 가져와 `N+1` 문제를 해결할 수 있다.

- `left join`은 기본적으로 연관된 엔티티들을 지연 로딩 방식으로 가져오지만, 필요한 필드를 `select`하면 `N+1` 문제를 방지할 수 있다.

**-> 따라서 `fetch join`이나 `left join`을 적절하게 사용하여 연관된 데이터를 한 번의 쿼리로 효율적으로 가져올 수 있도록 해야 한다.** 

<br /><br />

참고 : 
- [[JPA] QueryDSL Fetch & FetchJoin](https://velog.io/@moonyoung/JPA-QueryDSL-Fetch-FetchJoin)
- [[Springboot] Querydsl로 JPA N+1 문제 해결하기](https://velog.io/@joonghyun/Springboot-Querydsl%EB%A1%9C-JPA-N1-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)
- [[JPA] QueryDSL에서 JPA N+1 문제 해결하기](https://97class.tistory.com/11)