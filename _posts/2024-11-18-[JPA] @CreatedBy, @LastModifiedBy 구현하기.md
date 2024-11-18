---
title: "[JPA] @CreatedBy, @LastModifiedBy 구현하기"
date: 2024-11-18
categories: [JPA]
tags: [TIL, Querydsl, JPA, Spring]
---

## 📍 @JPA Auditing 이란?

- **Audit**은 사전적으로 '감사하다', '심사하다'의 의미를 가지고 있으며, **Auditing**은 Spring boot Jpa 에서 지원하는 기능 중 하나이다.
- **Auditing**은 해당 데이터를 보고 있다가 생성 또는 수정이 발생하면 자동으로 값을 넣어주는 편리한 기능이다.

<br /><br />

### # AuditorAwareImpl 작성하기
: 현재 로그인한 사용자의 정보를 등록자와 수정자로 등록하기 위해 AuditAware이란 인터페이스를 구현한 클래스를 생성한다.

```
package com.delivery_project.config;

import java.util.Optional;
import org.springframework.data.domain.AuditorAware;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;

public class AuditorAwareImpl implements AuditorAware<String> {

    @Override
    public Optional<String> getCurrentAuditor() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if (authentication == null || !authentication.isAuthenticated()) {
            return Optional.empty();
        }

        return Optional.of(authentication.getName()); // 현재 사용자 이름을 반환
    }
}

```

<br /><br />

### # JpaAuditingConfig 작성하기
:EnableJpaAuditing을 통해 Audit 기능을 활성화한다.

```
package com.delivery_project.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.domain.AuditorAware;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@EnableJpaAuditing(auditorAwareRef = "auditorProvider")
@Configuration
public class JpaAuditingConfig {

    @Bean
    public AuditorAware<String> auditorProvider() {
        return new AuditorAwareImpl();
    }
}

```

<br /><br />

### # Timestamped Entity 작성하기

: **Auditing**이 필요한 **Entity**에서 상속받을 **BaseEntity(Timestamped Entity)**를 생성한다.

1. `@MappedSuperclass` (javax.persistence)
Entity에서 Table에 대한 공통 매핑 정보가 필요할 때 부모 클래스에 정의하고 상속받아 해당 필드를 사용하여 중복을 제거한다.

2. `@EntityListeners`  (javax.persistence)
Entity를 DB에 적용하기 이전, 이후에 커스텀 콜백을 요청할 수 있는 어노테이션

1. `Class AuditingEntityListener` (org.springframework.data.jpa)
Entity 영속성 및 업데이트에 대한 Auditing 정보를 캡처하는 JPA Entity Listener

1. `@CreatedDate` (org.springframework.data)
데이터 생성 날짜 자동 저장 어노테이션

1. `@LastModifiedDate` (org.springframework.data)
데이터 수정 날짜 자동 저장 어노테이션

1. `@CreatedBy` (org.springframework.data)
데이터 생성자 자동 저장 어노테이션

1. `@LastModifiedBy` (org.springframework.data)
데이터 수정자 자동 저장 어노테이션

```
package com.delivery_project.entity;

import jakarta.persistence.Column;
import jakarta.persistence.EntityListeners;
import jakarta.persistence.MappedSuperclass;
import jakarta.persistence.PrePersist;
import java.time.LocalDateTime;
import lombok.Getter;
import org.springframework.data.annotation.CreatedBy;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedBy;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class Timestamped {

    @CreatedDate
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @CreatedBy
    @Column(name = "created_by", length = 255, updatable = false)
    private String createdBy;

    @LastModifiedDate
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @LastModifiedBy
    @Column(name = "updated_by", length = 255)
    private String updatedBy;

    @Column(name = "deleted_at")
    private LocalDateTime deletedAt;

    @Column(name = "deleted_by", length = 255)
    private String deletedBy;

    @PrePersist
    protected void onCreate() {
        this.createdAt = LocalDateTime.now(); // 최초 생성 시점
    }

    // 소프트 삭제 메서드
    public void delete(String deletedBy) {
        this.deletedAt = LocalDateTime.now();
        this.deletedBy = deletedBy;
    }

    // 삭제 여부 확인 메서드
    public boolean isDeleted() {
        return this.deletedAt != null;
    }
}
```

<br /><br />



참고 : 
- [[Spring]Data JPA, Auditing적용 및 Auditing 직접 구현하기](https://velog.io/@wonizizi99/SpringData-JPA-Auditing)
- [[JPA ] Auditing 기능 살펴보기](https://web-km.tistory.com/42)
- [JPA) JPA Auditing으로 자동화](https://dwc04112.tistory.com/271)
- [JPA Auditing을 이용한 Entity 공통 속성 설정](https://jizero-study.tistory.com/43)