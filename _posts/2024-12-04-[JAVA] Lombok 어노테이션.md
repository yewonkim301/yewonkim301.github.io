---
title: "[JAVA] Lombok 어노테이션"
date: 2024-12-04
categories: [JAVA]
tags: [TIL, JAVA]
image:
  path: /assets/img/til/docker.png
---

## 📍 Lombok 이란?

자바 라이브러리로 코드 에디터나 빌드 툴(IntelliJ, Eclipse, XCode 등)에 추가하여 코드를 효율적으로 작성할 수 있도록 도와준다. class명 위에 어노테이션을 명시해줌으로써 getter나 setter, equals와 같은 method를 따로 작성해야 하는 번거로움을 덜어주는 역할을 한다.

<br /><br />

### # @NoArgsConstructor

파라미터가 없는 기본 생성자를 생성자를 만들어준다.

주의해야 할 점은 만약, 항상 초기화가 필요한 **final**이 붙은 field가 있는데 **@NoArgsConstructor**을 사용한다면 **compile error**가 발생할 것이다.

<br /><br />

### # @AllArgsConstructor

모든 필드 값을 파라미터로 받는 생성자를 만들어준다.

**@NonNull**이 붙어있는 field의 경우, 생성되는 생성자 내부에 해당 parameter에 **null check 로직이 생성**된다.

<br /><br />

### # @RequiredArgsConstructor

`final`이나 `@NonNull`인 필드 값만 파라미터로 받는 생성자를 만들어준다.

초기화되지 않은 모든 **final fields**와, 선언될 때 초기화되지 않은 **@NonNull**로 표시된 field까지 parameter를 가진다.
특히 **@NonNull**이 달려있는 field의 경우, 생성되는 생성자 내부에 명시적인 **null 체크 로직** 또한 생성된다. 따라서 **@NonNull**이 붙어있는 field 중 하나라도 null 값을 포함한다면 **NullPointerException**이 발생하게 된다.

그래서 만약 @NonNull이 붙어있는 field 중 어떠한 것이라도 null 값을 포함한다면 NullPointerException이 발생하게 된다.

<br /><br />

### # @Data

`@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@ToString`, `@EqualsAndHashCode` 를 모두 합쳐놓은 어노테이션이다.

#### # 참고

**1.@ToString**

- `toString()` 메서드의 기능을 한다.
- `exclude` 속성을 사용하면, 특정 필드를 toString() 결과에서 제외시킬 수 있다.
  ```
  @ToString(exclude = "password")
  ```


**2.@EqualsAndHashCode**

- `equals`와 `hashCode` 메서드를 생성할 수 있다.
- `callSuper` 속성을 통해 `equals`와 `hashCode` 메서드 자동 생성 시 부모 클래스의 필드까지 감안할지 안 할지에 대해서 설정할 수 있다. `@EqualsAndHashCode(callSuper = true)` 로 설정할 시, 부모 클래스 필드 값들도 동일한지 확인하게 된다.
<br /><br />

### # @Builder

**빌더 패턴(Builder Pattern)** 은 복잡한 객체의 생성 과정과 표현 방법을 분리하여 다양한 구성의 인스턴스를 만드는 생성 패턴이다. 생성자에 들어갈 매개 변수를 메서드로 하나하나 받아들이고 마지막에 통합 빌드해서 객체를 생성하는 방식이다.

빌더는 **생성자의 유무**에 따라 아래와 같이 동작한다.
- 생성자가 **없는** 경우 : 모든 멤버 변수를 파라미터로 받는 기본 생성자 생성
- 생성자가 **있는** 경우 : 따로 생성자를 생성하지 않음

<br />

#### # 빌더 패턴을 사용하는 이유

1. **필요한 데이터만 설정할 수 있음**

2. **유연성을 확보할 수 있음**
 : 빌더 패턴을 이용하면 새로운 변수가 추가되는 등의 상황이 생겨도 기존의 코드에 영향을 주지 않을 수 있다

3. **가독성을 높일 수 있음**
 :  빌더 패턴을 적용하면 직관적으로 어떤 데이터에 어떤 값이 설정되는지 쉽게 파악하여 가독성을 높일 수 있다.

4. **변경 가능성을 최소화할 수 있음**
 : **Setter**를 구현한다는 것은 불필요하게 변경 가능성을 열어두는 것이다. 이는 유지보수 시에 값이 할당된 지점을 찾기 힘들게 만들며 불필요한 코드 리딩 등을 유발한다. 
 만약 값을 할당하는 시점이 객체의 생성뿐이라면 객체에 잘못된 값이 들어왔을 때 그 지점을 찾기 쉬우므로 유지보수성이 훨씬 높아질 것이다. 


<br /><br />


참고 : 
- [[자바] 자주 사용되는 Lombok 어노테이션](https://www.daleseo.com/lombok-popular-annotations/)
- [[JAVA] Lombok 어노테이션 @Data](https://zi-c.tistory.com/19)
- [@Builder, @All/NoArgsConstructor 제대로 알고 사용하자!](https://velog.io/@maketheworldwise/Builder-AllNoArgsConstructor-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)
- [[Lombok] 공식 문서를 통해 알아보는 @NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor + 사용시 주의사항(단점)](https://siahn95.tistory.com/170)
- [💠 빌더(Builder) 패턴 - 완벽 마스터하기](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B9%8C%EB%8D%94Builder-%ED%8C%A8%ED%84%B4-%EB%81%9D%ED%8C%90%EC%99%95-%EC%A0%95%EB%A6%AC)
- [[Java] 빌더 패턴(Builder Pattern)을 사용해야 하는 이유](https://mangkyu.tistory.com/163)