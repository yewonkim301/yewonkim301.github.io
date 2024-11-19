---
title: "[Spring] @PreAuthorize, @Secured 란?"
date: 2024-11-19
categories: [Spring]
tags: [TIL, Spring, SpringSecurity]
---

프로젝트를 진행하면서 다른 팀원분의 코드를 통해 `@PreAuthorize` 어노테이션을 알게 되었다.
권한 검증이 필요한 부분에서 나는 아래와 같이 별도의 메서드를 만들어서 처리해주고 있었는데, 어노테이션만으로 깔끔하게 권한 검증을 할 수 있는 `@PreAuthorize`에 대해 더 자세히 알아보고자 한다.

```
private void validateUserAccess(User user, UUID ownerId) {
        if (!(user.getId().equals(ownerId) || user.getRole().equals(UserRoleEnum.MANAGER) || user.getRole()
            .equals(UserRoleEnum.MASTER))) {
            throw new BadRequestException("해당 가게에 대한 접근권한이 없습니다.");
        }
    }
```

### # 활성화하기

: SecurityConfig 파일 설정하기

- `@EnableMethodSecurity(securedEnabled = true, prePostEnabled = true)`
  - **securedEnabled**: @Secure 애노테이션 사용가능 여부에 대한 속성
  - **prePostEnabled**: @PreAuthorize, @PostAuthorize 애노테이션 사용가능 여부에 대한 속성

```
@Configuration
@EnableWebSecurity // 스프링 시큐리티 필터가 스프링 필터체인에 등록됨.
@EnableMethodSecurity(securedEnabled = true, prePostEnabled = true) 
public class SecurityConfig {
```

<br /><br />

## 📍 @PreAuthorize

- 특정 권한을 가진 사용자만 메서드를 호출할 수 있도록 제한할 수 있다.
- String 형태로 넣어줄 수 있고, and나 or 등 논리 연산자도 넣어줄 수 있다.

<br />

### # 사용 예시

```
@PreAuthorize("hasRole('ROLE_ADMIN')")
@GetMapping("/info")
public @ResponseBody String info() {
    return "개인정보";
}
```

### # 사용가능한 표현식
- `hasRole([role])` : 현재 사용자의 권한이 파라미터의 권한과 동일한 경우 true
- `hasAnyRole([role1,role2])` : 현재 사용자의 권한디 파라미터의 권한 중 일치하는 것이 있는 경우 true
- `principal` : 사용자를 증명하는 주요객체(User)를 직접 접근할 수 있다.
- `authentication` : SecurityContext에 있는 authentication 객체에 접근 할 수 있다.
- `permitAll` : 모든 접근 허용
- `denyAll` : 모든 접근 비허용
- `isAnonymous()` : 현재 사용자가 익명(비로그인)인 상태인 경우 true
- `isRememberMe()` : 현재 사용자가 RememberMe 사용자라면 true
- `isAuthenticated()` : 현재 사용자가 익명이 아니라면 (로그인 상태라면) true
- `isFullyAuthenticated()` : 현재 사용자가 익명이거나 RememberMe 사용자가 아니라면 true

<br />

#### +) @PostAuthorize

- `@PostAuthorize` : 메서드가 실행되고 나서 응답을 보내기 전에 인증을 거친다.
  - 사용 예시: 사용자가 자신의 프로필 정보를 요청하고 해당 메서드가 사용자 정보를 반환시, `@PostAuthorize` 를 사용하여 반환된 사용자 정보가 요청한 사용자의 정보와 일치하는지를 확인이 가능하다. **-> 인증 객체와 리턴 객체의 정보를 비교할 수 있다.**

- `@PreAuthorize` : 메서드가 실행되기 전에 인증을 거친다.

<br /><br />


## 📍 @Secured

- 권한검증이 필요한 곳에 **@Secured** 어노테이션을 붙여주면, 어노테이션에 인자로 받은 권한이 유저에게 있을 때만 실행하도록 할 수 있다.
<br />

### # 사용 예시

```
@Secured("ROLE_ADMIN")
@GetMapping("/info")
public @ResponseBody String info() {
    return "개인정보";
}
```

<br />

### 차이점
- `@Secured`는 표현식 사용할 수 없다.
- `@PreAuthroize`, `@PostAuthorize`는 표현식 사용을 사용하여 디테일한 설정이 가능하다.

<br /><br />


참고 : 
- [[프로젝트] 시큐리티 권한 체크 `@PreAuthorize 또는 @Secured`](https://oth3410.tistory.com/356)
- [[공부정리] @Secured(), @PreAuthorize, @PostAuthorize](https://velog.io/@joon6093/SecuredPreAuthorize-PostAuthorize)
- [[Spring Security] 4. 권한 처리](https://velog.io/@shon5544/Spring-Security-4.-%EA%B6%8C%ED%95%9C-%EC%B2%98%EB%A6%AC)