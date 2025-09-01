---
title: "[Spring] @Value 어노테이션의 작동 방식과 사용 시 주의할 점"
date: 2025-09-01
categories: [Spring]
tags: [TIL, Spring]
---


# # @Value 어노테이션

`@Value` 어노테이션은 Spring 프레임워크에서 제공하는 기능으로, 외부 설정 파일(예: application.properties, application.yml)이나 시스템 환경 변수 등에서 값을 주입받기 위해 사용된다. 이를 통해 애플리케이션의 설정 값을 코드에 직접 하드코딩하지 않고도 유연하게 관리할 수 있다.


```java
import org.springframework.beans.factory.annotation.Value;

@Component
public class MyComponent {
    
    @Value("${my.property}")
    private String myProperty;

    // ...
}
```

위 예제에서 `@Value("${my.property}")`는 `application.properties` 파일에 정의된 `my.property` 값을 `myProperty` 필드에 주입한다.

<br /><br />

## # @Value 어노테이션의 작동 방식

`@Value`는 스프링이 구동될 때 Component Scan을 통해 Bean으로 등록된 객체와 의존 관계를 주입할 때 등록된다.

<br />

### # 작동 순서
1. **스프링 컨테이너 초기화** : 스프링 애플리케이션이 시작되면, 스프링 컨테이너가 초기화되고, 설정 파일(application.properties 또는 application.yml)을 로드한다.

2. **Bean 생성** : 스프링 컨테이너는 `@Component`, `@Service`, `@Repository`, `@Controller` 등으로 어노테이션된 클래스를 스캔하여 Bean으로 등록한다.

3. **의존성 주입** : Bean이 생성될 때, `@Value` 어노테이션이 붙은 필드나 메서드에 대해 설정 파일에서 지정한 값을 찾아 주입한다. 이때 `${}` 구문을 사용하여 프로퍼티 키를 참조한다.

<br /><br />

## # @Value 어노테이션 사용 시 주의할 점

1. **프로퍼티 키 존재 여부** : `@Value` 어노테이션에 지정된 프로퍼티 키가 설정 파일에 존재하지 않으면 애플리케이션이 시작될 때 예외가 발생한다. 따라서, 반드시 해당 키가 설정 파일에 정의되어 있는지 확인해야 한다.

2. **기본값 설정** : 프로퍼티 키가 존재하지 않을 경우를 대비해 기본값을 설정할 수 있다. 기본값은 `:` 뒤에 지정한다.

```java
@Value("${my.property:defaultValue}")
private String myProperty;
```

3. **타입 변환** : `@Value` 어노테이션은 문자열 값을 주입하므로, 주입되는 필드의 타입이 문자열이 아닌 경우 스프링이 자동으로 타입 변환을 시도한다. 이 과정에서 변환이 불가능한 경우 예외가 발생할 수 있으므로 주의해야 한다.

4. **스프링 컨텍스트 내에서만 사용 가능** : `@Value` 어노테이션은 스프링 컨텍스트 내에서 관리되는 Bean에만 적용할 수 있다. 일반적인 POJO 클래스에서는 사용할 수 없다.

```java
public class MyComponent {
    
    @Value("${my.property}")
    private String myProperty; // ❌ 잘못된 사용

    // ...
}
```

5. **static으로 선언된 변수나 메서드에서 값을 가져오지 못함** : static 변수나 메서드는 객체가 생성이 되기전에 메모리 영역에 올라가면서 문제가 발생을 한다. 의존성 주입을 받기도 전에 메모리 영역에 올라가 해당 값을 가져오지 못한다.

```java
@Component

public class MyComponent {
    
    @Value("${my.property}")
    private static String myProperty; // ❌ 잘못된 사용, myProperty는 null이 됨

    // ...
}
```

<br /><br />

## # 참고 : @ConfigurationProperties

`@ConfigurationProperties` 어노테이션은 Spring Boot에서 제공하는 기능으로, 외부 설정 파일(application.properties 또는 application.yml)에서 여러 개의 관련된 설정 값을 하나의 POJO 클래스에 바인딩할 때 사용된다. 이를 통해 설정 값을 구조화하고, 타입 안전성을 제공하며, 코드의 가독성을 높일 수 있다.

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import java.util.List;

@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    
    private String name;
    private String version;
    private List<String> servers;

    // getters and setters
}
```

```application.properties
app.name=MyApplication
app.version=1.0.0
app.servers=server1,server2,server3
```

위 예제에서 `@ConfigurationProperties(prefix = "app")`는 `application.properties` 파일에 정의된 `app.name`, `app.version`, `app.servers` 값을 `AppProperties` 클래스의 필드에 주입한다.

<br />

### # @Value vs @ConfigurationProperties

| 특징 | @Value | @ConfigurationProperties |
|------|--------|-------------------------|
| 사용 용도 | 단일 프로퍼티 주입 | 관련된 여러 프로퍼티|
| 타입 변환 | 단일 값에 대해 자동 변환 | 복잡한 구조와 컬렉션 지원 |
| 기본값 설정 | 가능 (예: `${my.property:defaultValue}`) | 불가능 |
| 가독성 | 낮음 (여러 개의 @Value 사용 시) | 높음 (관련 프로퍼티를 하나의 클래스에 묶음) |
| 유지보수 | 어려움 (프로퍼티가 많아질수록) | 용이 (설정 변경 시 클래스만 수정) |


<br /><br />

참고 : 
- [@Value 어노테이션 사용 시 주의할 점을 설명해주세요.](https://www.maeil-mail.kr/question/7)
- [[Spring] Spring에서 @Value 어노테이션 사용 시 주의할 점 정리](https://devhooney.tistory.com/496)
- [[Spring] @Value 사용 방법과 주의 사항과 동작 방식](https://dreamsea77.tistory.com/372)
