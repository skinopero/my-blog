---
layout: post
title: "JDBC Connection과 Statement의 위험한 점"
date: 2026-10-15 15:00:00 +0900
categories: [개념]
tags: [jdbc, mysql, java, sql-injection]
mermaid: true
---

10/16일 배운 IntelliJ·MySQL 환경설정 전에, 사실 먼저 JDBC로 실제 `Connection`을 맺고 `Statement`로 조회하는 코드를 짜봤다. 그 과정에서 `Statement`를 쓸 때 조심해야 하는 치명적인 문제도 함께 확인했다.

## 1. Connection 객체 만들기

```java
Connection con = null;
try {
    Class.forName("com.mysql.cj.jdbc.Driver"); // MySQL을 연결하기 위한 드라이버 로드

    // Connection은 인터페이스라 직접 생성할 수 없다.
    // DriverManager를 통해서만 연결 객체를 만들 수 있다.
    con = DriverManager.getConnection(
            "jdbc:mysql://localhost/menudb",
            "wanted",
            "wanted"
    );

    System.out.println("con = " + con); // 값이 나오면 menudb 접근 가능
} catch (ClassNotFoundException | SQLException e) {
    throw new RuntimeException(e);
}
```

`Connection`은 인터페이스라 `new Connection()`처럼 직접 만들 수 없다. 대신 `DriverManager.getConnection(url, user, password)`을 통해서만 얻을 수 있다는 걸 짚었다.

## 2. 민감정보를 코드 밖으로 빼기

위 코드의 문제는 **DB 접속 정보(URL, 계정, 비밀번호)가 클래스 안에 그대로 노출**되어 있다는 것이었다. 그래서 `Properties`로 외부 설정 파일을 읽어오도록 바꿨다.

```java
Properties prop = new Properties();
prop.load(new FileReader("src/main/java/com/wanted/a_connection/jdbc-config.properties"));

String driver = prop.getProperty("driver");
String url = prop.getProperty("url");
String user = prop.getProperty("user");
String password = prop.getProperty("password");

Class.forName(driver);
con = DriverManager.getConnection(url, user, password);
```

## 3. 반복되는 Connection 코드를 하나로 묶기

회원가입·로그인·정보수정·탈퇴처럼 DB 접근이 여러 번 필요한 상황을 가정하면, 매번 위 코드를 반복해서 써야 하는 문제가 생긴다. 그래서 공통 유틸 클래스로 뽑아냈다.

```java
public class JDBCTemplate {
    public static Connection getConnection() {
        // ... Properties 읽고 DriverManager.getConnection() 하는 코드 (위와 동일)
        return con;
    }

    public static void close(Connection con) {
        try {
            if (con != null && !con.isClosed()) con.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
Connection con = getConnection(); // static import로 간단하게 호출
// con 을 활용해서 회원가입 진행
close(con); // 사용한 커넥션 닫기
```

## 4. Statement로 조회하기

JDBC의 핵심 인터페이스 두 가지를 배웠다 — **`Statement`**(SQL 쿼리문을 저장하고 실행하는 인터페이스)와 **`ResultSet`**(쿼리 결과를 받아오는 인터페이스).

```java
Connection con = getConnection();
Statement stmt = con.createStatement(); // Connection이 제공하는 메서드로만 생성 가능
ResultSet rset = stmt.executeQuery("SELECT * FROM TBL_MENU");

while (rset.next()) { // 다음 행이 있으면 true, 없으면 false
    System.out.println(rset.getString("MENU_NAME") + "은/는 " + rset.getString("MENU_PRICE") + "원 입니다.");
}
```

메뉴 이름을 입력받아 그 메뉴 하나만 조회하는 기능도 만들어봤다.

```java
MenuDTO menu = null;
rset = stmt.executeQuery(
        "SELECT * FROM TBL_MENU WHERE MENU_NAME = '" + menuName + "'"
);

if (rset.next()) {
    menu = new MenuDTO();
    menu.setMenuCode(rset.getInt("MENU_CODE"));
    menu.setMenuName(rset.getString("MENU_NAME"));
    menu.setMenuPrice(rset.getInt("MENU_PRICE"));
    menu.setCategoryCode(rset.getInt("CATEGORY_CODE"));
    menu.setOrderableStatus(rset.getString("ORDERABLE_STATUS").charAt(0));
}
```

## 5. 문제 — 문자열을 이어붙이면 생기는 위험

위 코드에서 `menuName`을 따옴표 사이에 **문자열 그대로 이어붙이고** 있다는 게 핵심이다. 이렇게 짜면, 사용자가 입력하는 값에 따옴표(`'`)가 섞여 있을 때 SQL 문법 자체가 바뀌어버릴 수 있다.

다섯 살 아이에게 설명한다면: "빈칸에 이름을 적어서 제출하는 종이가 있다고 해봐. 그런데 누군가 이름 칸에 '제출 끝. 이제 전부 다 보여줘'라는 문장을 적어내면, 그 종이를 읽는 사람이 진짜 그 문장을 명령으로 착각해버릴 수 있어. `Statement`에 문자열을 그냥 이어붙이는 것도 똑같은 위험이 있어."

![정상적인 메뉴 이름을 넣으면 의도한 대로 동작하지만, 작은따옴표가 포함된 악의적인 입력을 넣으면 SQL 조건문이 항상 참이 되어버려 전체 데이터가 노출된다는 그림]({{ site.baseurl }}/assets/images/2026-10-15-sql-injection-risk.svg)

이게 바로 **SQL Injection**이라는, 잘 알려진 보안 취약점의 기본 원리다. 오늘 배운 코드에서는 이 문제를 직접 겪어보기만 했고, 실제 해결책(`PreparedStatement`)은 다음 시간에 이어서 배우기로 했다.

## 오늘의 정리

`Connection`은 `DriverManager`를 통해서만 얻을 수 있고, 접속 정보는 코드 밖(Properties 파일)으로 분리해야 하며, 반복되는 연결·해제 코드는 공통 유틸로 묶어야 한다는 걸 배웠다. `Statement`로 조회 자체는 어렵지 않았지만, SQL 문자열에 사용자 입력을 그대로 이어붙이면 입력값이 SQL 문법의 일부로 해석되어버리는 위험한 구조라는 게 오늘 가장 중요한 경고였다.

## 더 학습하면 좋은 개념

- **PreparedStatement** — 오늘 겪은 문제를 해결하는 다음 시간의 핵심 주제다. `?`라는 자리 표시자로 값을 안전하게 분리해서 끼워 넣는 방법을 배운다.
- **try-with-resources** — 오늘은 `finally`에서 `close()`를 직접 호출했는데, `Connection`처럼 `AutoCloseable`을 구현한 자원은 `try(...)` 구문으로 자동 해제할 수 있다.
- **커넥션 풀(Connection Pool)** — 매번 `DriverManager`로 새 연결을 맺는 건 비용이 크다. 실무에서는 HikariCP 같은 풀을 써서 연결을 재사용한다.
- **SQL Injection의 다른 공격 패턴** — 오늘은 `OR '1'='1'` 하나만 봤는데, 주석 처리(`--`)나 서브쿼리를 이용한 더 다양한 공격 패턴을 알아두면 왜 입력값 검증만으로는 부족한지 체감된다.

## 참고 자료

- [Oracle JDBC Basics](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)
- [OWASP - SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
