---
title: "[Spring] Spring에서 객체를 Bean으로 관리하는 이유"
date: 2025-08-19
categories: [Spring]
tags: [TIL, Spring]
---

# # Spring에서 객체를 Bean으로 관리하는 이유

```
Bean은 Spring Container에 의해 관리되는 객체를 말한다.

→ Spring이 생성하고, 초기화하고, 주입하고, 소멸까지 책임지는 객체
```

<br /><br />

1. **의존성 관리 자동화** <br />

    - Spring은 의존성 주입(Dependency Injection)을 통해 개발자가 직접 의존 객체를 생성하지 않아도 자동으로 주입해준다.
    - 또한 Spring Boot 2.6 이후부터는 순환 참조를 기본적으로 금지하기 때문에, 애플리케이션 실행 시점에 순환 의존성 오류를 사전에 감지할 수 있다.

    ```java
    @Component
    class OrderService {
        private final PaymentService paymentService;

        // 생성자 주입
        public OrderService(PaymentService paymentService) {
            this.paymentService = paymentService;
        }
        
        public void processOrder() {
            paymentService.pay();
        }
    }
    ```
    → `OrderService`가 `PaymentService`를 직접 `new`로 만들 필요 없이, Spring이 알아서 주입해준다.

<br /><br />

1. **생명주기 관리** <br />
   
   - Spring은 Bean의 생명주기인 `객체의 생성 → 초기화 → 소멸`을 자동으로 관리한다. 이를 통해 리소스 할당 및 해제를 체계적으로 처리할 수 있다.
   - 또한 `@PostConstruct`, `@PreDestroy` 등을 통해 개발자가 직접 초기화/정리 로직을 삽입할 수 있다.

    ```java
    @Component
    class ResourceHandler {
        
        @PostConstruct
        public void init() {
            System.out.println("리소스 초기화");
        }

        @PreDestroy
        public void cleanup() {
            System.out.println("리소스 정리");
        }
    }
    ```
    → Spring이 Bean 생성 시 `@PostConstruct` 호출, 종료 시 `@PreDestroy` 호출

<br /><br />

3. **AOP(관점 지향 프로그래밍) 지원** <br />
   
   - Spring은 AOP를 지원하여, 공통 관심사를 분리하고 재사용성을 높인다. Bean으로 관리되는 객체는 AOP 기능을 쉽게 적용할 수 있다.

<br /><br />

4. **설정의 중앙화** <br />

    - Spring에서는 `@Configuration` 클래스나 `@Bean` 등록을 통해 어플리케이션 설정을 한 곳에서 관리할 수 있다. 이를 통해 코드의 가독성과 유지보수성을 향상시킬 수 있다.

    ```java
    @Configuration
    class AppConfig {
        @Bean
        fun dataSource(): DataSource {
            return HikariDataSource().apply {
                jdbcUrl = "jdbc:postgresql://localhost:5432/mydb"
                username = "user"
                password = "password"
                maximumPoolSize = 10
            }
        }
    }
    ```

<br /><br />

5. **싱글톤 패턴 구현** <br />

    - 기본적으로 Spring은 빈을 싱글톤으로 관리한다.
    - 따라서 하나의 객체가 애플리케이션 전체에서 공유되며 메모리 사용을 최적화하고, 불필요한 객체 생성을 방지한다.

    ```java
    // 아래 두 userRepository는 동일한 인스턴스
    @Service
    class UserService(private val userRepository: UserRepository)

    @Service
    class AuthService(private val userRepository: UserRepository)
    ```

<br /><br />

6. **테스트 용이성** <br />
   
    - Spring의 DI(Dependency Injection) 기능을 통해 테스트가 용이해진다. 빈으로 관리되는 컴포넌트는 모킹(mocking)이나 테스트용 구현체로 쉽게 대체할 수 있어 단위 테스트와 통합 테스트가 용이하다.

    ```java
    @SpringBootTest
    `class UserServiceTest {
        @MockBean
        lateinit var userRepository: UserRepository
        
        @Autowired
        lateinit var userService: UserService
        
        @Test
        fun testGetUser() {
            // given
            val userId = 1L
            whenever(userRepository.findById(userId)).thenReturn(User(userId, "Test User"))
            
            // when
            val result = userService.getUser(userId)
            
            // then
            assertEquals("Test User", result.name)
        }
    }
    ```

<br /><br />

참고 : 
- [Spring에서 객체를 Bean으로 관리하는 이유를 설명해주세요.](https://www.maeil-mail.kr/question/294)
- [Spring에서 Bean으로 객체를 관리하는 이유](https://velog.io/@msw0909/Spring%EC%97%90%EC%84%9C-Bean%EC%9C%BC%EB%A1%9C-%EA%B0%9D%EC%B2%B4%EB%A5%BC-%EA%B4%80%EB%A6%AC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
- 
