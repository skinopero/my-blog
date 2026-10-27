---
layout: post
title: "MySQL 기초 문법③ — DDL·DML로 테이블 정의하고 데이터 조작하기"
date: 2026-10-13 15:00:00 +0900
categories: [개념]
tags: [mysql, sql, ddl, dml]
mermaid: true
---

JOIN·서브쿼리로 조회(SELECT)를 깊이 파봤다면, 오늘은 테이블 자체를 정의하는 DDL과, 데이터를 직접 넣고 바꾸고 지우는 DML을 배웠다.

## 1. DDL — 테이블을 만들 때 "규칙"을 함께 정의한다

> 테이블을 만드는 것은 단순히 칸을 나누는 작업이 아니다. 데이터가 지켜야 할 규칙을 정의하고, 데이터베이스가 스스로 데이터의 품질을 유지하도록 만드는 과정이다.

```sql
CREATE TABLE tb1 (
    user_id INT PRIMARY KEY,
    user_pwd VARCHAR(12) NOT NULL COMMENT '사용자 비밀번호',
    email VARCHAR(30) NOT NULL UNIQUE COMMENT '이메일',
    user_status VARCHAR(10) DEFAULT 'active' COMMENT '계정 활성 상태',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '가입 일시'
) COMMENT = '서비스 유저 정보 테이블';
```

이 규칙은 **데이터 타입**(어떤 종류의 값만 들어올 수 있는지)과 **제약 조건**(값들 사이의 논리적 규칙) 두 가지로 이뤄진다.

| 제약 조건 | 역할 |
|---|---|
| `NOT NULL` | 이 칸은 비워둘 수 없음 (필수 입력) |
| `UNIQUE` | 이 칸의 값은 테이블 내에서 중복 불가 |
| `PRIMARY KEY` | NOT NULL + UNIQUE를 모두 가지는 대표 식별자 |
| `FOREIGN KEY` | 다른 테이블의 PK를 참조해 관계를 맺고 일관성을 보장 |
| `DEFAULT` | 값을 안 넣었을 때 쓸 기본값 |

```sql
CREATE TABLE tb2 (
    user_id INT PRIMARY KEY,
    author_id INT NOT NULL,
    ...
    FOREIGN KEY (author_id) REFERENCES tb1(user_id)
);
```

`tb2.author_id`가 `tb1.user_id`를 참조하도록 외래키를 걸면, **부모 테이블(tb1)에 존재하지 않는 `user_id`를 자식 테이블(tb2)에 넣으려고 하면 데이터베이스가 자동으로 차단**한다 — 이게 참조 무결성을 지키는 메커니즘이다.

```sql
-- 이미 만든 테이블 구조 바꾸기
alter table courses_temp add column price int null default 0;      -- 추가
alter table courses_temp modify column price varchar(200) null;     -- 수정
alter table courses_temp drop column price;                          -- 삭제

-- 데이터+구조 삭제 vs 데이터만 삭제
drop table courses_temp;      -- 구조까지 영구 삭제, 빠르지만 롤백 불가
truncate table courses_temp;  -- 구조는 남기고 데이터만 삭제
```

## 2. DML — INSERT, UPDATE, DELETE

```sql
insert into users (`email`, `name`) values ('email@gmail.com', '이공두');

-- UNIQUE 위반: 같은 이메일 중복 불가
insert into users (`email`, `name`) values ('email@gmail.com', '이겅두'); -- 에러!

-- NOT NULL 위반: name은 반드시 있어야 함
insert into users (`email`) values ('email2@gmail.com'); -- 에러!

-- 외래키 참조 무결성 위반: 존재하지 않는 author_id
insert into courses (`author_id`, `title`, `description`)
values (999, 'SQL 제약조건!', '제약 조건을 확인해보아요'); -- 에러!
```

데이터를 삽입할 때 에러가 나면, **테이블의 제약조건과 데이터 타입부터 확인**해야 한다는 걸 직접 에러를 겪으며 배웠다. `author_id = 999`처럼 `users` 테이블에 없는 회원을 참조하려 하면, 외래키 제약조건이 "고아 데이터"가 생기는 걸 막아준다.

```sql
-- 위험한 UPDATE: WHERE가 없으면 테이블 전체가 바뀐다!
update users set `name` = '이공두';

-- 안전한 UPDATE: 행 하나를 정확히 식별하는 조건(PK)
update users set `name` = '조평순' where `user_id` = 1;
```

다섯 살 아이에게 설명한다면: "반 전체 학생 이름표를 '철수'로 몽땅 바꿔버리면 안 되잖아. '1번 학생 이름표만 철수로 바꿔줘'처럼, 누구 이름표인지 콕 집어서 말해야 해. 그 '콕 집어 말하기'가 바로 `WHERE`야."

![UPDATE문에서 WHERE를 빠뜨리면 테이블의 모든 행이 한꺼번에 바뀌어버리지만, PK 같은 유일한 값으로 WHERE 조건을 걸면 의도한 행 하나만 안전하게 수정된다는 그림]({{ site.baseurl }}/assets/images/2026-10-13-update-without-where.svg)

```sql
-- DELETE도 같은 함정이 있고, 외래키가 보호해주기도 한다
delete from users where email = 'email@gmail.com'; -- 정상

delete from users where user_id = 1; -- 에러! courses가 이 user_id를 참조 중이면 거부된다
```

수정·삭제 작업에서 가장 중요한 건 **WHERE 조건절**이고, 그 조건은 PK나 UNIQUE처럼 "행 하나를 정확히 식별할 수 있는 값"으로 거는 게 권장된다고 배웠다.

## 오늘의 정리

DDL은 테이블의 생김새(데이터 타입)뿐 아니라 "지켜야 할 규칙"(제약조건)까지 함께 정의하는 작업이었다. DML에서는 INSERT 시 제약조건 위반 에러들을 직접 겪어보며 각 제약조건이 실제로 무엇을 막아주는지 확인했고, UPDATE·DELETE에서는 WHERE 조건절 하나를 빠뜨리는 게 얼마나 위험한지 체감했다.

## 더 학습하면 좋은 개념

- **CHECK 제약조건** — 오늘 다룬 다섯 가지 제약조건 외에, 값의 범위 자체를 제한하는 CHECK도 있다(10/14일 데이터베이스 설계 수업에서 이미 살짝 다뤘다).
- **CASCADE 옵션** — 오늘은 부모 데이터 삭제가 "거부"되는 경우만 봤는데, 자식 데이터를 함께 삭제/NULL 처리하는 옵션도 있다. 10/15일 물리적 모델링 글에서 이어진다.
- **트랜잭션으로 DML 묶기** — 오늘 배운 INSERT/UPDATE/DELETE 여러 개를 하나의 작업 단위로 묶어 안전하게 처리하는 법은, 이어지는 트랜잭션 수업에서 배운다.

## 참고 자료

- [MySQL 8.4 Reference Manual - Data Definition Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-data-definition-statements.html)
- [MySQL 8.4 Reference Manual - Data Manipulation Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-data-manipulation-statements.html)
