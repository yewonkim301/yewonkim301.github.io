---
title: "[Spring] Spring AOP를 이용해 권한 체크하기"
date: 2024-11-22
categories: [Spring]
tags: [TIL, Spring]
image:
  path: /assets/img/til/spring.svg
---

주문 서비스를 개발하는 프로젝트에서 권한 체크가 필요할 때마다 도메인별로 서비스에서 권한 체크를 하는 메서드를 작성하여 권한 체크를 하도록 작성하였다.

```
private void validateUserAccess(User user) {
        if (!(user.getRole().equals(UserRoleEnum.MANAGER) || user.getRole()
            .equals(UserRoleEnum.MASTER))) {
            throw new BadRequestException("접근권한이 없습니다.");
        }
    }
```

이렇게 도메인별로 권한 체크를 하는 메서드를 작성하는 경우, 코드가 중복되어 작성되게 된다.


<br />

## 📍 AOP 란?

- **A**spect **O**riented **P**rogramming 의 약자로 관점 지향 프로그래밍이다.
- **AOP**는 애플리케이션 전체에 걸쳐 사용되는 기능을 재사용하도록 지원한다. 쉽게 말해서 공통적으로 처리하는 로직을 따로 모듈화해서 원하는 시점, 원하는 메서드에 적용할 수 있게 만들어 주는 것이다.

<br /><br />

## # AOP를 이용해서 권한 체크하기

<br />

### # 어노테이션 생성

권한 관리를 하기 위한 AuthAnnotation 생성

```
@Target(ElementType.METHOD) // 이 어노테이션이 부착될 수 있는 타입을 의미
@Retention(RetentionPolicy.RUNTIME) // 어느 시점까지 메모리를 가져 갈것인지 설정
public @interface AuthCheck {
    Role role() default Role.ROLE_MEMBER;
}
```

<br /><br />

### # 인터셉터 생성

AuthCheck가 걸린 메서드를 실행할 때 권한을 체크하는 인터셉터

```
@Slf4j
@Aspect
@RequiredArgsConstructor
@Component
public class AuthCheckAspect {

    private static final String AUTHORIZATION = "Authorization";

    private final JwtService jwtService;
    private final MemberRepository memberRepository;

    private final HttpServletRequest httpServletRequest;

    @Around("@annotation(wave.myarh.global.auth.AuthCheck)") // 해당 어노테이션이 붙은 메서드가, value에 넣은 어노테이션이 붙은 메서드가 실행 되기 전, 후 동작
    public Object authCheck(ProceedingJoinPoint pjp) throws Throwable {
        String token = httpServletRequest.getHeader(AUTHORIZATION);

        JwtPayload payload = jwtService.getPayload(token);
        log.info("AuthCheck(email) : " + payload.getEmail());

        Optional<Member> member = memberRepository.findByEmail(payload.getEmail());
        if (member.isEmpty()) {
            throw new UserAuthenticationException();
        }

        MemberContext.currentMember.set(member.get());
        return pjp.proceed();

    }
}
```

<br /><br />

### # 컨트롤러에서 권한 체크하기

```
@Slf4j
@RequiredArgsConstructor
@RestController
@RequestMapping("/api/members")
public class MemberApiController {

    @AuthCheck
    @GetMapping("/check")
    public ResponseEntity<?> checkAuth() {
        return ResponseEntity.ok().body(ResponseApiDto.of(HttpStatus.OK,"권한_확인"));
    }
}
```


<br /><br />


참고 : 
- [[Spring] Spring AOP를 이용한 권한 체크](https://waveofymymind.tistory.com/83)
- [AOP를 이용한 권한관리](https://velog.io/@alstn_dev/AOP%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EA%B6%8C%ED%95%9C%EA%B4%80%EB%A6%AC)
- [[Spring] AOP(Aspect-Oriented Programming)](https://junghyungil.tistory.com/111)