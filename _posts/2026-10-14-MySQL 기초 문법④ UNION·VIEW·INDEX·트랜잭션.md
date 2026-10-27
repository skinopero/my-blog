---
layout: post
title: "MySQL 기초 문법④ — UNION·VIEW·INDEX·트랜잭션"
date: 2026-10-14 15:00:00 +0900
categories: [개념]
tags: [mysql, sql, transaction, index, view]
mermaid: true
---

DDL·DML까지 배우고 나니, 오늘은 조회 결과를 합치고(UNION), 가상 테이블로 감추고(VIEW), 검색을 빠르게 하고(INDEX), 여러 작업을 하나로 묶는(트랜잭션) 네 가지 실무형 문법을 배웠다.

## 1. UNION — 여러 쿼리 결과를 하나로 합치기

```sql
select `email`, `status` from users where status = 'active'
union
select `name`, `email`, `status` from users where status = 'suspended';
```

`UNION`은 수학의 합집합처럼, 각 쿼리문의 결과를 합치고 **중복된 행은 제거**한다. 단, 합치려는 쿼리들의 **컬럼 개수가 같아야 하고, 컬럼 순서·데이터 타입도 호환**되어야 한다는 제약이 있다 — 위 예제는 실제로는 컬럼 개수가 달라서 그대로 실행하면 에러가 나는 흔한 실수 케이스였다.

## 2. VIEW — 복잡한 쿼리를 가상 테이블로

```sql
create view v_coureses_status as
select c.course_id, c.title, count(e.enrollment_id) as '등록 학생 수'
from courses as c
left join enrollments as e on c.course_id = e.course_id
group by c.course_id, c.title;

select * from v_coureses_status where `등록 학생 수` >= 3;
```

View를 쓰는 이유 세 가지를 배웠다.

| 이유 | 설명 |
|---|---|
| 편의성 | 복잡한 JOIN·집계 쿼리를 한 번 만들어두면, 이후엔 단순한 테이블처럼 조회만 하면 됨 |
| 보안성 | 원본 테이블의 민감한 컬럼을 숨기고, 허용된 컬럼으로만 구성된 가상 테이블을 제공 |
| 가독성·속도 | 자주 쓰는 컬럼만 모아둬서 가독성이 높고, 불필요한 컬럼 조회가 없어 속도도 좋아짐 |

```sql
create or replace view v_coureses_status as ...  -- view 재정의
drop view v_coureses_status;                      -- view 삭제
```

View는 실제 데이터를 담고 있는 게 아니라, **쿼리 자체를 저장해뒀다가 조회할 때마다 다시 실행**한다는 점에서 "가상" 테이블이라고 불린다.

## 3. INDEX — 검색 속도를 좌우하는 양날의 검

```sql
explain select * from users where name = '홍길동'; -- 인덱스 적용 전: 120 row full scan

create index idx_users_name on users(name);

explain select * from users where name = '홍길동'; -- 인덱스 적용 후: 1 row만 scan
```

인덱스는 "목차"라는 뜻 그대로, 특정 컬럼의 값과 데이터의 물리적 위치를 미리 정리해두는 구조다. 인덱스가 없으면 데이터베이스는 첫 행부터 끝까지 하나씩 비교하는 **풀 테이블 스캔**을 하지만, 인덱스가 있으면 목차를 보고 원하는 위치로 바로 이동할 수 있다.

다만 인덱스는 **양날의 검(trade-off)**이라고 배웠다.

| 장점 | 단점 |
|---|---|
| SELECT 성능 대폭 향상 | 인덱스 자체가 추가 저장 공간을 차지 |
| | INSERT/UPDATE/DELETE 성능 저하 — 데이터가 바뀔 때마다 인덱스도 다시 정렬해야 함 |

그래서 인덱스는 **조회가 압도적으로 많고, 값이 자주 바뀌지 않는 컬럼**에 거는 게 좋다고 배웠다. PK·UNIQUE 제약이 걸린 컬럼은 유일성이 보장되기 때문에 테이블 생성 시 자동으로 인덱스가 적용된다는 것도 확인했다.

## 4. 트랜잭션 — 여러 작업을 하나의 단위로

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 3000 WHERE id = 1; -- 홍길동 -3000
UPDATE accounts SET balance = balance + 3000 WHERE id = 2; -- 김짱구 +3000

SELECT * FROM accounts; -- 아직 영구 반영은 아니다
COMMIT; -- 모든 작업 성공 반영!
```

계좌이체는 논리적으로 하나의 작업이지만, 실제로는 **UPDATE가 2번** 일어난다. 그 사이에 문제가 생기면 "돈이 한쪽에서만 빠져나가고 다른 쪽엔 안 들어오는" 상태가 될 수 있다.

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 3000 WHERE id = 1; -- 성공
UPDATE accounts SET balance = balance + 3000 WHERE id = 99; -- 없는 계좌! 실패
ROLLBACK; -- 작업 중 일부가 실패했으니 전부 되돌리기
```

다섯 살 아이에게 설명한다면: "친구한테 사탕을 주려고 내 사탕을 빼긴 했는데, 친구가 없어서 못 줬어. 그럼 어떻게 해야 할까? 빼간 내 사탕도 다시 돌려받아야지, 사탕이 그냥 사라지면 안 되잖아. `ROLLBACK`이 바로 '아까 뺀 거 다시 돌려놔'야."

![계좌이체처럼 두 번의 UPDATE로 이루어진 작업에서, 하나라도 실패하면 ROLLBACK으로 전부 되돌리고, 모두 성공해야만 COMMIT으로 영구 반영한다는 그림]({{ site.baseurl }}/assets/images/2026-10-14-transaction-atomicity.svg)

```
START TRANSACTION -> 여러 작업 실행 -> 모두 성공하면 COMMIT, 하나라도 실패하면 ROLLBACK
```

## 오늘의 정리

UNION은 여러 쿼리 결과를 하나로 합치고, VIEW는 복잡한 쿼리를 가상 테이블로 감싸 재사용성·보안성을 높이고, INDEX는 조회 속도를 올리는 대신 데이터 변경 비용을 치러야 하는 트레이드오프였다. 트랜잭션은 여러 DML 작업을 하나의 논리적 단위로 묶어서, 중간에 실패하면 전부 되돌리는(ROLLBACK) 안전장치였다 — 10/12일 배운 "DB가 해결해주는 것들"에서 이름만 들었던 트랜잭션을, 오늘 실제 SQL로 직접 다뤄본 셈이다.

## 더 학습하면 좋은 개념

- **UNION vs UNION ALL** — 오늘 배운 UNION은 중복을 제거하는데, 중복 제거 과정 자체가 비용이 크다. 중복 제거가 필요 없다면 더 빠른 `UNION ALL`을 쓸 수 있다.
- **트랜잭션 격리 수준(Isolation Level)** — 오늘은 "성공 아니면 전부 취소"만 봤는데, 여러 트랜잭션이 동시에 실행될 때 서로 얼마나 영향을 주고받는지는 격리 수준이 정한다.
- **복합 인덱스(Composite Index)** — 오늘은 컬럼 하나짜리 인덱스만 만들었는데, 여러 컬럼을 묶은 인덱스는 컬럼 순서에 따라 효과가 달라진다.

## 참고 자료

- [MySQL 8.4 Reference Manual - UNION Clause](https://dev.mysql.com/doc/refman/8.4/en/union.html)
- [MySQL 8.4 Reference Manual - CREATE VIEW Statement](https://dev.mysql.com/doc/refman/8.4/en/create-view.html)
- [MySQL 8.4 Reference Manual - Optimization and Indexes](https://dev.mysql.com/doc/refman/8.4/en/mysql-indexes.html)
- [MySQL 8.4 Reference Manual - START TRANSACTION Statement](https://dev.mysql.com/doc/refman/8.4/en/commit.html)
