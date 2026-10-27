---
layout: post
title: "MySQL 기초 문법① — SELECT·WHERE·ORDER BY·GROUP BY"
date: 2026-10-12 09:00:00 +0900
categories: [개념]
tags: [mysql, sql, select]
mermaid: true
---

오늘은 "왜 DB가 필요한가·SQL 실행 순서"라는 개념 수업과는 별개로, 실제 MySQL 문법 수업에서 SELECT의 기본기를 처음부터 다졌다. SELECT는 SQL의 90% 이상을 차지한다고 할 만큼 핵심이라고 배웠다.

## 1. SELECT — 무엇을, 어디서

```sql
select menu_name from tbl_menu;                 -- 한 컬럼만
select menu_code, category_code, menu_price      -- 여러 컬럼
from tbl_menu;
select * from tbl_menu;                          -- * : 모든 컬럼

select menu_name as '메뉴 이름', menu_price as '가격' -- as : 별칭(Alias)
from tbl_menu;
```

SELECT의 핵심은 두 가지 질문으로 요약됐다 — **"어떤 열을 가져올 것인가"**(컬럼)와 **"어떤 행을 가져올 것인가"**(조건, 바로 다음 챕터인 WHERE).

## 2. WHERE — 조건에 맞는 행만 고르기

```sql
select menu_name, menu_price, orderable_status
from tbl_menu
where orderable_status = 'Y';
```

| 연산자 | 의미 |
|---|---|
| `=`, `!=`/`<>`, `>`, `<`, `>=`, `<=` | 비교 |
| `LIKE` | 패턴 포함 (`%단어` 끝남, `단어%` 시작함, `%단어%` 포함) |
| `IN (...)` | 목록 중 하나와 일치 |
| `BETWEEN a AND b` | 두 값 사이 범위 |
| `IS NULL` / `IS NOT NULL` | NULL 비교는 `=`가 아니라 이걸로 해야 함 |

```sql
-- 틀리기 쉬운 AND/OR 우선순위
select emp_name, salary, job_code
from tbl_employee
where job_code = 'J2' and salary >= 2000000
    or job_code = 'J7';
```

이 조건절을 보고 "J2이면서 급여 200만원 이상, **또는** J7"이라고 읽었는데, 실제로는 AND가 OR보다 먼저 묶인다. 다섯 살 아이에게 설명한다면: "AND는 OR보다 힘이 세서 먼저 짝꿍을 맺어. `A and B or C`는 `(A and B) or C`로 묶이는 거지, `A and (B or C)`가 아니야."

![AND가 OR보다 먼저 묶여서, 의도와 다르게 (job_code='J2' AND salary>=200) OR (job_code='J7')으로 해석된다는 그림]({{ site.baseurl }}/assets/images/2026-10-12-and-or-precedence.svg)

의도한 대로 묶으려면 **괄호로 직접 순서를 지정**해야 한다 — `where job_code = 'J2' and (salary >= 2000000 or job_code = 'J7')`처럼.

## 3. ORDER BY — 정렬하기

```sql
select menu_code, menu_name, menu_price
from tbl_menu
order by menu_price desc; -- asc(오름차순, 기본값) / desc(내림차순)
```

```sql
-- 연산 결과로도 정렬 가능, 백틱(`)으로 별칭을 감싸면 그 별칭으로 정렬
select menu_code, menu_price, menu_code * menu_price as `연산 결과`
from tbl_menu
order by `연산 결과` desc;
```

NULL이 섞인 컬럼을 정렬할 때는 `order by ref_category_code is null asc`처럼 쓰면, NULL 값들을 맨 뒤로 보내면서 나머지를 오름차순 정렬할 수 있었다.

## 4. DISTINCT와 LIMIT

```sql
select distinct category_code from tbl_menu order by category_code; -- 중복 제거

select * from tbl_menu order by menu_price desc limit 1, 5; -- limit [offset, ] row_count
```

`LIMIT`는 수백만 건 데이터를 한 번에 조회하는 부하를 막아주고, 웹 페이징 기능(1페이지에 10개씩 등)을 구현하는 데 필수적인 기능이라고 배웠다.

## 5. GROUP BY와 집계 함수

```sql
select count(*) as '총 사용자수' from users;
select sum(score) as '총점', avg(score) as '평균' from grades;
```

| 집계 함수 | 역할 |
|---|---|
| `count(컬럼)` | 데이터 개수 (NULL 제외) |
| `sum(컬럼)` | 합계 |
| `avg(컬럼)` | 평균 |
| `max`/`min(컬럼)` | 최댓값/최솟값 |

```sql
-- 에러! 비집계 컬럼(name)과 집계함수(count)를 같이 쓰면 안 된다
select name, count(*) as '총 사용자 수' from users;

-- group by로 해결: status 별로 묶어서 집계
select status, count(*) as '사용자 수'
from users
group by status;
```

`name`처럼 집계 대상이 아닌 컬럼을 집계 함수와 그냥 같이 쓰면 에러가 난다 — "어떤 name 값을 count(*)와 묶어야 할지" DB가 알 수 없기 때문이다. `GROUP BY`로 특정 컬럼 기준 그룹을 만들어야 비로소 그 그룹 단위로 집계할 수 있다.

```sql
-- having: 그룹화 이후에 필터링
select author_id as '교육자 번호', count(*) as '개설 강좌 수'
from courses
group by author_id
having count(*) >= 3;
```

```mermaid
flowchart LR
    A[FROM: 원본 테이블] --> B[WHERE: 행 필터링]
    B --> C[GROUP BY: 그룹 묶기]
    C --> D[HAVING: 그룹 필터링]
    D --> E[집계 함수 계산]
```

`WHERE`는 그룹화 **전** 행을 거르고, `HAVING`은 그룹화 **후** 그룹을 거른다는 차이가 핵심이었다 — 10/12일 배운 "SQL 실행 순서"(FROM→WHERE→GROUP BY→HAVING→SELECT)가 여기서 그대로 쓰이고 있었다.

## 오늘의 정리

SELECT는 "어떤 열, 어떤 행"이라는 두 질문으로 요약되고, WHERE의 AND/OR 우선순위처럼 눈으로 읽은 것과 실제 동작이 다를 수 있다는 걸 직접 겪었다. GROUP BY/HAVING은 집계 함수와 짝을 이뤄 "그룹별 통계"를 뽑아내는 도구였고, WHERE와 HAVING의 시점 차이(그룹화 전/후)를 분명히 구분해야 했다.

## 더 학습하면 좋은 개념

- **WITH ROLLUP** — GROUP BY 결과에 소계·총계를 함께 보여주는 옵션. 오늘 배운 집계의 다음 단계다.
- **CASE WHEN** — SELECT 절 안에서 조건별로 다른 값을 보여주는 문법. WHERE의 조건문과는 다른 용도로 자주 쓰인다.
- **인덱스와 ORDER BY/WHERE 성능** — 오늘은 문법만 익혔는데, 같은 조건이라도 인덱스가 있는지에 따라 속도가 크게 달라진다. 뒤에 배울 INDEX 수업과 이어진다.

## 참고 자료

- [MySQL 8.4 Reference Manual - SELECT Statement](https://dev.mysql.com/doc/refman/8.4/en/select.html)
- [MySQL 8.4 Reference Manual - Operator Precedence](https://dev.mysql.com/doc/refman/8.4/en/operator-precedence.html)
