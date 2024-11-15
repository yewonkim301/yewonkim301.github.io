---
title: "[JPA] @Transient 어노테이션이란?"
date: 2024-11-15
categories: [JPA]
tags: [TIL, Querydsl, JPA, Spring]
---

## 📍 @Transient 이란?

- **@Transient**는 JPA의 표준이라 할 수 있는 **javax.persistence** 패키지에 포함되어 있는 컬럼 매핑 레퍼런스 어노테이션이다.
  
- **@Transient** 컬럼을 제외하기 위해 사용된다. 다시 말해서, 영속 대상에서 제외시키기 위해서 사용하는 것이다.

<br />

**그렇다면 영속 대상에서 제외한다는 것은 무슨 의미일까?**

### # 영속 대상에서 제외
- JPA에선 Entity Manager가 존재하고, 이 엔티티 매니저에 의해 @Entity가 관리된다.
  
![img](/assets/img/til/entityManager.png)
  
- 그림과 같이 엔티티 객체의 상태가 영속 상태(managed, persistent state)일 때, 비로소 엔티티 매니저에 의해 관리된다. 그리고 영속 상태의 엔티티 객체는 엔티티 매니저의 의해 변화에 대한 **자동 감지(Dirty Checking)**, **CRUD SQL 자동 생성 작업** 및 그 외 일련의 모든 JPA의 내부적인 동작 프로세스에서 활용된다.

하지만 영속 대상에서 제외된다면, 엔티티 매니저의 관리 대상에서 제외되므로 앞서 말한 작업들을 수행하지 않게 되는 것이다.
  
<br /><br />


## 사용 예시

```
package com.delivery_project.entity;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import java.util.UUID;

@Entity
@Table(name = "p_restaurants")
@NoArgsConstructor
@AllArgsConstructor
@Getter
@Builder
public class Restaurant extends Timestamped {

    @Id
    private UUID id;

    @Column(nullable = false, length = 255)
    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "category_id", nullable = false)
    private Category category;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "owner_id", nullable = false)
    private User owner;

    @Column(nullable = false, length = 255)
    private String address;

    @Column(nullable = false)
    private Boolean isHidden = false;

    @Transient // DB에 저장하지는 않고 평점 계산용으로 사용
    private Double averageRating;

    public void setAverageRating(Double averageRating) {
        this.averageRating = averageRating;
    }
}
```

<br /><br />

참고 : 
- [[JPA] @Transient와 @Transient? (Spring Data와 JPA의 관계에 대해서)](https://dogfood.tistory.com/entry/JPA-Transient%EC%99%80-Transient-Spring-Data%EC%99%80-JPA%EC%9D%98-%EA%B4%80%EA%B3%84%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C?category=951035)
- [[Spring JPA] Column으로 쓰지않는 변수에 대한 선언. @Transient
출처: https://juntcom.tistory.com/90 [쏘니의 개발블로그:티스토리]](https://juntcom.tistory.com/90)
- [JPA에서 @Transient 애노테이션이 존재하는 이유](https://gmoon92.github.io/jpa/2019/09/29/what-is-the-transient-annotation-used-for-in-jpa.html)