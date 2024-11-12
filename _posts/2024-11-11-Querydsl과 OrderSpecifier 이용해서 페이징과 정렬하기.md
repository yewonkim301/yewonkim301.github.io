---
title: "Querydsl과 OrderSpecifier 이용해서 페이징과 정렬하기"
date: 2024-11-12
categories: [Querydsl]
tags: [TIL, Spring, Java, Querydsl, OrderSpecifier]
---

### 도입 계기
프로젝트의 요구 사항에 따라 페이징과 정렬 기능을 구현해야 했다. 팀원들과 **Querydsl**을 이용해서 페이징을 구현해보기로 하여, **Querydsl**을 이용하여 페이징을 처리하는 방법에 대해 알아보게 되었다.
<br /><br />

## 📍Querydsl란?
- **Querydsl**에 대해서 간단하게 알아보자면, **Querydsl**은 동적 쿼리 생성과 안전한 타입을 지원하여 JPA로 구현하기 어려운 복잡한 쿼리를 작성할 때 유용하게 사용할 수 있다.
- `booleanExpression` : 쿼리를 동적으로 생성하기 위해 사용하는 기능이다. `booleanExpression`에서 `null`을 반환하면 해당 `where`절이 무시된다.
<br /><br />

## 📍OrderSpecifier란?
- `Querydsl`에서 정렬 기능을 위해 사용되는 클래스로, 여러 정렬 조건을 유연하게 추가할 수 있다. 예를 들어서 생성일 순 과 이름 순 등 다중 정렬 기준이 필요한 경우, 조건에 따라 동적으로 정렬 순서를 변경할 수 있다.
<br /><br />

### # Querydsl 과 OrderSpecifier 사용하여 페이징과 정렬하기
1. `pageRequestUtils`
  - 여러 도메인에서 정렬 기능을 활용하기 위해 pageRequestUtils를 따로 분리하였다.
  - `PageRequest`를 이용해서 한페이지에 존재시킬 데이터 개수 size, 가져오길 원하는 페이지 번호, 정렬시킬 기준들을 전달한다.

```
  public class PageRequestUtils {

    public static PageRequest getPageRequest(int page, int size, String sortProperty, boolean ascending) {
        size = adjustPageSize(size);

        Sort.Direction direction = ascending ? Sort.Direction.ASC : Sort.Direction.DESC;
        return PageRequest.of(page, size, Sort.by(direction, sortProperty));
    }

    private static int adjustPageSize(int size) {
        if (size == 10 || size == 30 || size == 50) {
            return size;
        }
        return 10;
    }
}
```
<br />

2. `controller`
```
@GetMapping("/category/{categoryId}")
    public Page<RestaurantResponseDto> getRestaurantsByCategory(
        @PathVariable UUID categoryId,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(defaultValue = "createdAt") String sortProperty,
        @RequestParam(defaultValue = "true") boolean ascending
    ) {
        PageRequest pageRequest = PageRequestUtils.getPageRequest(page, size, sortProperty, ascending);
        return restaurantService.getRestaurantsByCategory(pageRequest, categoryId);
    }
```
<br />

3. `service`
```
public Page<RestaurantResponseDto> getRestaurantsByCategory(PageRequest pageRequest, UUID categoryId) {
        // Querydsl로 조건을 생성 (카테고리에 해당하는 가게 조회)
        BooleanExpression predicate = qRestaurant.category.id.eq(categoryId);

        // Querydsl 페이징 적용
        List<Restaurant> restaurants = restaurantRepository.findRestaurants(predicate, pageRequest);

        // DTO로 변환
        List<RestaurantResponseDto> restaurantResponseDtos = restaurants.stream()
            .map(restaurant -> RestaurantResponseDto.builder()
                .id(restaurant.getId())
                .name(restaurant.getName())
                .categoryId(restaurant.getCategory().getId())
                .ownerId(restaurant.getOwner().getId())
                .address(restaurant.getAddress())
                .isHidden(restaurant.getIsHidden())
                .build())
            .toList();

        return new PageImpl<>(restaurantResponseDtos, pageRequest, restaurants.size());
    }
```
<br />

4. `RestarantRepositoryCustomImpl`

```
@Repository
public class RestaurantRepositoryCustomImpl implements RestaurantRepositoryCustom {

    private final JPAQueryFactory queryFactory;

    @PersistenceContext
    private EntityManager entityManager;
    public RestaurantRepositoryCustomImpl(EntityManager entityManager) {
        this.entityManager = entityManager;
        this.queryFactory = new JPAQueryFactory(entityManager);
    }

    @Override
    public List<Restaurant> findRestaurants(BooleanExpression predicate, PageRequest pageRequest) {
        QRestaurant qRestaurant = QRestaurant.restaurant;

        // 정렬 기준 설정
        OrderSpecifier<?> orderSpecifier = getOrderSpecifier(qRestaurant, pageRequest);

        // Querydsl 쿼리 생성
        return queryFactory
            .selectFrom(qRestaurant)
            .where(predicate)
            .offset(pageRequest.getOffset())
            .limit(pageRequest.getPageSize())
            .orderBy(orderSpecifier)
            .fetch();
    }

    private OrderSpecifier<?> getOrderSpecifier(QRestaurant qRestaurant, PageRequest pageRequest) {

        String sortProperty = pageRequest.getSort().getOrderFor("createdAt") != null ? "createdAt" : "updatedAt";
        boolean ascending = pageRequest.getSort().getOrderFor(sortProperty).getDirection().isAscending();

        // 정렬 기준 설정
        if ("createdAt".equals(sortProperty)) {
            return ascending ? qRestaurant.createdAt.asc() : qRestaurant.createdAt.desc();
        } else if ("updatedAt".equals(sortProperty)) {
            return ascending ? qRestaurant.updatedAt.asc() : qRestaurant.updatedAt.desc();
        } else {
            return qRestaurant.createdAt.desc();
        }
    }

}
```


<br /><br />
참고 : 
- [QueryDSL - Pageable, PageRequest, Slice 에 대해서](https://velog.io/@rnqhstlr2297/QueryDSL-Pageable-PageRequest-Slice-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)
- [[Querydsl]동적 sorting을 위한 OrderSpecifier 클래스 구현](https://velog.io/@seungho1216/Querydsl%EB%8F%99%EC%A0%81-sorting%EC%9D%84-%EC%9C%84%ED%95%9C-OrderSpecifier-%ED%81%B4%EB%9E%98%EC%8A%A4-%EA%B5%AC%ED%98%84)
- [QueryDSL을 이용해서 Pageable로 Sorting하는 방법](https://medium.com/@hgbaek1128/querydsl%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4%EC%84%9C-pageable%EB%A1%9C-sorting%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-65c63ebaebd8)
- [[JPA] QueryDSL에서 Sorting(정렬)하는 Util 메서드 만들기](https://hellou8363.tistory.com/81)
- [[ Spring-Boot ] JPA QueryDsl 정렬 사용하기](https://itmoon.tistory.com/73)