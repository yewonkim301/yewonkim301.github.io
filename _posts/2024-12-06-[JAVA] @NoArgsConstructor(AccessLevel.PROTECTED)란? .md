---
title: "[JAVA] @NoArgsConstructor(AccessLevel.PROTECTED)란?"
date: 2024-12-06
categories: [JAVA]
tags: [TIL, JAVA]
---

## 📍 @NoargsConstructor(AccessLevel.PROTECTED)

**NoArgsContstruct(AcessLevel.PROTECTED)** 는 Entity나 DTO에서 많이 사용한다.

기본 생성자의 접근 제어를 **PROTECTED**로 설정해놓게 되면 무분별한 객체 생성에 대해 한번 더 체크할 수 있는 수단이 되기 때문이다.

<br />

예를 들어서 기본 생성자의 권한이 public 인 경우, 아래와 같은 상황이 발생할 수 있다.

```
/// User.java
@Getter
@Setter
@NoArgsConstructor
public class User {
    private String name;
    private Long age;
    private String email;
}


/// Main.java
public static void main(String[] args) {
    User user = new User();
    user.setName("testname");
    user.setEmail("test@test.com");
    
    /// age가 설정되지 않았으므로 user는 완전하지 않은 객체
}
```

<br />

`@NoArgsConstructor(access = AccessLevel.PROTECTED)` 와 `@Builder`를 같이 사용해서 아래와 같이 생성자별로 설정되는 멤버변수 내용을 정의하고 생성자에 @Builder를 설정하게되면, 해당 생성자를 사용하는 Builder가 생성되어 의미있는 객체만 생성할 수 있게 된다.

```
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
    private String name;
    private Long age;
    private String email;

    @Builder
    public User(Long age, String email) {
        this.name = "test_name";
        this.age = age;
        this.email = email;
    }

    @Builder
    public User(String name, Long age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
}
```

<br />

또한 `@NoArgsConstructor(access = AccessLevel.PROTECTED)` 사용은 **프록시 때문이**기도 하다.

### 그렇다면 프록시란 무엇일까?

#### 1. JPA의 객체 생성 방식
JPA는 데이터베이스와의 상호작용을 위해 엔티티 객체를 생성할 때 리플렉션(reflection)을 사용하며, 리플렉션을 통해 클래스의 기본 생성자를 호출하여 객체를 생성한다.
따라서 기본 생성자가 없으면 JPA는 해당 엔티티를 인스턴스화할 수 없다.

<br />

#### 2. 프록시 객체 생성

![img](/assets/img/til/NoArgs_protected1.png)

**JPA**는 **Lazy Loading(지연 로딩)**과 같은 기능을 지원하기 위해 엔티티의 프록시 객체를 생성할 수 있다. 이 **프록시 객체**는 **껍데기**이며, target이라는 변수에 호출할 객체의 참조값을 가지고 있다.
그리고 실제 객체가 필요할 때까지 로딩을 지연시키는 역할을 한다.
프록시 객체는 위에서 언급했듯이 껍데기이기 때문에 프록시 객체를 생성하려면 생성하기 위해서도 기본 생성자가 필요하다.

<br />

#### 3. 직렬화 및 역직렬화

![img](/assets/img/til/NoArgs_protected2.png)

직렬화된 데이터(예: JSON, XML 등)를 객체로 변환할 때, 역직렬화 라이브러리(예: Jackson, Gson 등)는 해당 클래스의 인스턴스를 생성해야 한다. 이때 기본 생성자를 사용하여 객체를 생성한다. 만약 기본 생성자가 없다면, 라이브러리는 객체를 생성할 수 없고, 결과적으로 역직렬화가 실패하게 된다.

<br />

### 따라서,
프록시 객체를 사용하기 위해서 JPA 구현체는 실제 엔티티의 기본 생성자를 통해 프록시 객체를 생성하는데 **AccessLevel.PRIVATE** 를 사용하게 되면 프록시 객체를 사용할 수 없기 때문에 프록시 객체를 사용하기 위해서는 **AccessLevel.PROTECTED** 혹은 **AccessLevel.PUBLIC** 을 사용해야 한다.

<br /><br />

#### # 참고

- `@NoArgsConstructor(access = AccessLevel.PUBLIC)` : 기본 생성자를 이용하여, 값을 주입하는 방식을 최대한 방지하기 위해서 사용을 권장하지 않는다.

- `@NoArgsConstructor(access = AccessLevel.PRIVATE)` : 프록시 객체 생성시 문제가 생기기 때문에 사용을 권장하지 않는다.

<br /><br />

참고 : 
- [@NoargsConstructor(AccessLevel.PROTECTED) 와 @Builder](https://cobbybb.tistory.com/14)
- [@NoArgsConstructor(access=accessLevel.PROTECTED)에 대하여](https://velog.io/@alsgudtkwjs/NoArgsConstructoraccessaccessLevel.PROTECTED%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)