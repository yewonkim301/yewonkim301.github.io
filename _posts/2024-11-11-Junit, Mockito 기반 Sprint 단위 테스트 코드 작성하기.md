---
title: "Junit, Mockito 기반 Sprint 단위 테스트 코드 작성하기"
date: 2024-11-11
categories: [Spring], [TIL]
tags: [TIL, Spring, Java, Junit, Mockito]
image:
  path: 
---

### 도입 계기
팀원들과 도메인을 나눠서 프로젝트를 진행하고 있다. 내가 구현하고 있는 도메인의 엔티티가 다른 팀원의 엔티티를 참조하고 있는데, 아직 서로 개발 중에 있어 내가 구현하고 있는 엔티티가 참조할만한 임시 객체가 필요했다. 이 문제를 해결하기 위해 찾아보다가 **Mock**을 통해 임시 객체를 생성할 수 있다는 것을 알게 되었다.
<br /><br />

## 📍Mockito(모키토)란?
- 모키토는 개발자가 동작을 직접 제어할 수 있는 가짜 객체를 지원하는 테스트 프레임워크이다.
- Spring으로 웹 애플리케이션을 개발하면 여러 객체들 간의 의존성이 생기는데, 이러한 의존성을 해결하기 위해 Mockito 라이브러리를 활용하여 가짜 객체를 주입시킬 수 있다. 
<br /><br />

### # Mockito를 활용하여 테스트 코드 작성하기
1. Mockito로 임시 객체(Mock Object) 생성하기
  : `@Mock` 혹은 `Mock()` 메서드 사용하여 임시 객체를 생성한다.


2. `when-thenReturn` 구문을 사용하여 특정 메서드 호출 시 반환될 값을 정의하기
  ```
  when(restaurantService.createRestaurant(requestDto, mockUser))
      .thenReturn(new Restaurant(...));
  ```


3. `verify()` 메서드를 사용해 특정 메서드가 호출되었는지 검증하기
  ```
  verify(restaurantService, times(1)).createRestaurant(requestDto, mockUser);
  ```


4. Mock 객체를 활용하여 의존성 주입하기
  : `@MockBean`을 사용하여 의존성 주입이 필요한 곳에 직접 주입하여 사용할 수 있다.


5. 예외 상황 테스트하기
  ```
   doThrow(new RuntimeException("예외 메시지"))
    .when(restaurantService).createRestaurant(any(RestaurantRequestDto.class), any(User.class));
  ```
<br />

#### # 테스트 코드 작성 예시
```
@MockBean
private RestaurantService restaurantService;

@Test
@DisplayName("가게 생성 테스트")
public void createRestaurantTest() throws Exception {
    // Mock 객체 생성
    User mockUser = mock(User.class);
    Category mockCategory = mock(Category.class);

    RestaurantRequestDto requestDto = RestaurantRequestDto.builder()
        .name("가게명")
        .address("주소")
        .categoryId(UUID.randomUUID())
        .ownerId(UUID.randomUUID())
        .isHidden(false)
        .build();

    // mock 객체로 서비스의 동작을 정의
    doNothing().when(restaurantService).createRestaurant(requestDto, mockUser);

    // 요청을 JSON으로 변환하여 MockMvc로 전송
    mockMvc.perform(post("/api/restaurants")
            .contentType("application/json")
            .content(objectMapper.writeValueAsString(requestDto)))
        .andExpect(status().isCreated())
        .andExpect(jsonPath("$.message").value("성공 메시지"));
}
```

<br /><br />

참고 :
- [[Spring] JUnit & Mockito 기반 Spring 단위 테스트 코드 작성](https://velog.io/@sussa3007/Spring-JUnit-Mockito-%EA%B8%B0%EB%B0%98-Spring-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1)
- [[Spring] JUnit과 Mockito 기반의 Spring 단위 테스트 코드 작성법 (3/3)
출처: https://mangkyu.tistory.com/145 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/145)
