---
title: "[CS] PreparedStatement 란?"
date: 2025-08-14
categories: [CS]
tags: [TIL, CS, SQL]
---

# # PreparedStatement 란?

**PreparedStatement**는 Java에서 데이터베이스와 상호작용할 때 사용하는 인터페이스로, SQL 쿼리를 미리 컴파일하고 실행할 수 있게 해준다.

```java
String userId = "user123";
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);  // 구문 분석 & 컴파일
pstmt.setString(1, userId);  // 바인딩
ResultSet rs = pstmt.executeQuery();  // 쿼리 실행
```

➔ 쿼리 문장 구조는 미리 확정해두고, **?(placeholder) 파라미터만 특정 값으로 치환** 하는 방식

<br />

#### 참고 : Statement
Statement는 SQL 쿼리를 실행하기 위한 기본 인터페이스로, 매번 쿼리를 컴파일하고 실행합니다. 이는 성능 저하와 SQL Injection 공격에 취약할 수 있습니다.

```java
String userId = "user123";
String sql = "SELECT * FROM users WHERE id = '" + userId + "'";
Statement stmt = connection.createStatement();  // 구문 분석 & 컴파일
ResultSet rs = stmt.executeQuery(sql);          // 쿼리 실행
```

➔ `userId` string을 쿼리 중간에 `+` 로 연결하는 방식

<br /><br />

### # PreparedStatement의 특징

1. **SQL Injection 방지**

    PreparedStatement는 파라미터 바인딩을 통해 SQL Injection 공격을 방지합니다. 사용자가 입력한 값이 SQL 쿼리의 일부로 해석되지 않도록 합니다.

2. **성능 향상**

    DB는 쿼리를 실행하기 전에 먼저 SQL 문장을 파싱하여 문법을 검사하고, 실행 계획(쿼리 플랜)을 수립한다.<br />
    일반 Statement는 매번 이 과정을 반복해야 하지만, **PreparedStatement**는 동일한 SQL 문장을 한 번만 파싱한 뒤 DB 내부의 **Statement Cache에 저장**한다. <br />
    이후 동일한 SQL을 실행할 때는 캐시된 실행 계획을 재사용하므로 파싱 과정을 건너뛰어 불필요한 자원 소모를 줄이고 성능을 향상시킨다. 

- **Statment**

    ```java
    select name from tbl where id = 1 -- miss, parse 후 캐싱
    select name from tbl where id = 1 -- hit
    select name from tbl where id = 2 -- miss, parse 후 캐싱
    ```

    ➔ 파라미터가 달라지면 sql_text도 달라지기 때문에, 다시 parse 후 캐싱해야 한다.

- **PreparedStatement**

    ```java
    select name from tbl where id = ? -- miss, parse 후 캐싱 (id = 1)
    select name from tbl where id = ? -- hit, (id = 2)
    ```

    ➔ param 부분이 ? 로 처리 된 상태로 parse 후 caching 하기 때문에, 파라미터가 변경되어도 cache를 재활용 할 수 있게 된다.


3. **유지보수 용이**
   
   쿼리와 파라미터를 분리하여 작성할 수 있어 코드의 가독성이 높아지고 유지보수가 용이합니다.

4. **타입 안전성**
   
   PreparedStatement는 각 파라미터의 타입을 명시적으로 지정할 수 있어, 데이터 타입에 대한 오류를 줄일 수 있습니다.

<br /><br />

참고 : 
- [Statement와 PreparedStatement의 차이점은 무엇인가요?](https://www.maeil-mail.kr/question/291)
- [PreparedStatement란](https://umbum.dev/580/)