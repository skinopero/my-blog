---
layout: post
title: "배열·클래스·생성자로 묶어보기"
date: 2026-10-01
categories: [개념]
tags: [java, string, array, class, constructor]
mermaid: true
---

오늘 배운 걸 한 줄로 요약하면 "변수 하나로는 부족할 때 어떻게 묶어서 관리하는가"였다. 같은 타입을 여러 개 묶으면 배열, 다른 타입을 하나로 묶으면 클래스(사용자 정의 자료형), 그리고 그 클래스가 태어나는 순간을 책임지는 게 생성자였다.

## 1. String은 그 자체로 참 신기한 녀석

```java
String str1 = "applemango";
String str2 = "kiwi";
System.out.println("str1 의 길이 : " + str1.concat(str2));
```

여기서 한 번 헷갈렸다. 출력 라벨은 "길이"라고 적혀 있지만 실제로 호출한 건 `length()`가 아니라 `concat()`이다. 그래서 숫자가 아니라 `"applemangokiwi"`라는 합쳐진 문자열이 출력된다. **주석이나 변수명이 아니라 실제로 호출한 메소드가 뭔지**를 봐야 한다는 걸 직접 겪으면서 배웠다.

| 메소드 | 하는 일 |
|---|---|
| `length()` | 문자열의 길이를 `int`로 반환 |
| `charAt(index)` | 해당 인덱스에 있는 문자 한 글자를 반환 |
| `concat(str)` | 문자열 뒤에 다른 문자열을 이어 붙임 |
| `trim()` | 문자열 앞뒤의 공백을 제거 (가운데 공백은 그대로) |

인덱스는 0부터 시작한다. `"abc"`면 `a`가 0번, `b`가 1번, `c`가 2번이다. 그래서 문자열을 한 글자씩 출력하고 싶으면 이렇게 쓴다.

```java
for (int i = 0; i < str1.length(); i++) {
    System.out.println(str1.charAt(i));
}
```

## 2. 문자열 비교: `==`는 왜 자꾸 틀릴까

```java
String str1 = "java";              // 리터럴 방식
String str2 = new String("java");  // 객체 생성 방식
String str3 = "java";
String str4 = new String("java");

System.out.println(str1 == str2); // false
System.out.println(str1 == str3); // true
System.out.println(str2 == str4); // false
System.out.println(str1.equals(str2)); // true
```

```mermaid
flowchart LR
    subgraph Pool["문자열 리터럴 풀"]
        J["\"java\""]
    end
    subgraph Heap["heap (new로 생성)"]
        H1["new String(\"java\") #1"]
        H2["new String(\"java\") #2"]
    end
    str1 --> J
    str3 --> J
    str2 --> H1
    str4 --> H2
```

`==`는 "같은 공간을 가리키는가"를 비교한다. 리터럴(`"java"`)로 만든 `str1`과 `str3`는 같은 문자열 풀 공간을 가리켜서 `true`가 나오지만, `new String(...)`은 `new`를 만날 때마다 **항상 새로운 공간**을 만들기 때문에 값이 같아도 `str2 == str4`는 `false`다. 값만 비교하고 싶을 때는 생성 방식과 상관없이 `equals()`를 써야 한다.

## 3. 변수 하나로는 부족해서 — 배열

```java
int[] iarr = new int[5];
System.out.println(iarr.length); // 5
System.out.println(iarr[1]);     // 0 (아직 아무 값도 안 넣었는데!)
```

아무것도 넣지 않은 배열 공간을 출력했는데 `0`이 나와서 신기했다. heap 메모리는 빈 값으로 둘 수 없어서, 값을 넣지 않아도 JVM이 자료형별 기본값으로 미리 채워둔다고 한다.

| 자료형 | 기본값 |
|---|---|
| 정수(`int` 등) | `0` |
| 실수(`double` 등) | `0.0` |
| 논리(`boolean`) | `false` |
| 참조형(객체, 배열 등) | `null` |

다섯 살 아이에게 설명한다면: "서랍장을 새로 사면, 빈 서랍이 아니라 이미 '0'이라는 쪽지가 하나씩 들어있는 거야. 네가 뭘 넣기 전까지는 그 쪽지가 그 자리를 지키고 있는 거지."

![5칸짜리 배열의 각 칸에 이미 0이 들어있는 모습]({{ site.baseurl }}/assets/images/2026-10-01-array-default-drawer.svg)

배열은 `자료형[] 변수명 = new 자료형[크기];`로 선언·할당하고, `변수명[index]`로 각 칸에 접근한다. 반복문과 함께 쓰면 칸 하나하나를 순서대로 다룰 수 있어서 특히 유용했다.

## 4. 배열 + Scanner — 성적 평균 계산기

```java
Scanner sc = new Scanner(System.in);
int[] scores = new int[5];

for (int i = 0; i < scores.length; i++) {
    System.out.println((i + 1) + "번째 학생의 점수를 입력해주세요 : ");
    scores[i] = sc.nextInt();
}

double sum = 0;
for (int i = 0; i < scores.length; i++) {
    sum += scores[i];
}
double avg = sum / scores.length;
```

변수 5개(`num1`~`num5`)를 따로 두고 일일이 더하던 방식과 비교하면, 배열 하나에 반복문만 돌리면 되니 사람 수가 늘어나도 코드를 더 안 늘려도 된다는 게 체감됐다. `sum`을 `double`로 선언해둔 덕분에 `avg = sum / scores.length` 계산도 자동으로 실수 나눗셈이 됐다.

## 5. 서로 다른 타입을 묶고 싶을 때 — 사용자 정의 자료형

회원 한 명의 정보(아이디, 비밀번호, 이름, 나이, 성별, 취미)는 타입이 전부 달라서 배열 하나로 묶을 수 없었다. 그래서 클래스를 만들었다.

```java
public class Member {
    String id;
    String pwd;
    String name;
    int age;
    char gender;
    String[] hobby;
}
```

이렇게 메소드 없이 클래스 안에 바로 선언한 변수를 **필드(== 인스턴스 변수 == 속성)**라고 부른다. `Member member = new Member();`로 인스턴스를 만들고 나서 `member.name`을 바로 출력해보니 역시 `null`, `member.age`는 `0`이 나왔다 — 배열과 똑같이 필드도 기본값으로 먼저 채워진다는 걸 다시 확인한 셈이다.

이렇게 묶어두면 좋은 점은, 변수명을 하나하나 관리할 필요가 없고, 메소드에 회원 정보를 넘길 때도 `member` 하나만 전달인자로 넘기면 된다는 것이었다.

## 6. 생성자 — 객체가 태어나는 순간

```java
public Member(String id, String pwd, String name, int age, char gender, String[] hobby) {
    System.out.println("매개변수 있는 생성자 동작함...");
    this.id = id;
    this.pwd = pwd;
    this.name = name;
    this.age = age;
    this.gender = gender;
    this.hobby = hobby;
}
```

`Member member = new Member();`를 쓸 때 사실 `Member()`라는 생성자가 호출되고 있었다는 걸 오늘 처음 알았다. 지금까지는 JDK 컴파일러가 자동으로 만들어준 "매개변수 없는 기본 생성자"가 조용히 동작하고 있었던 것.

| 생성자 종류 | 필드 초기화 방식 |
|---|---|
| 매개변수 없는 생성자 | 필드가 기본값(`0`, `null`, `false` 등)으로 남음 |
| 매개변수 있는 생성자 | 전달인자로 넘어온 값으로 `this.필드 = 매개변수` 초기화 |

여기서 반드시 기억해야 할 함정 하나: **매개변수가 있는 생성자를 하나라도 직접 작성하면, 컴파일러는 더 이상 기본 생성자를 자동으로 만들어주지 않는다.** 그래서 `new Member()`처럼 인자 없이 호출하던 코드는 생성자를 추가하는 순간 컴파일 에러가 날 수 있다.

마지막으로 `toString()`을 오버라이드해서 `member`를 출력했을 때 `Member@1a2b3c` 같은 주소 대신 필드 값이 읽기 좋게 나오도록 만들었다.

```java
@Override
public String toString() {
    return "Member{id='" + id + "', name='" + name + "', age=" + age + "}";
}
```

## 오늘의 정리

같은 타입이 여러 개면 배열로, 다른 타입을 하나로 묶고 싶으면 클래스(사용자 정의 자료형)로 해결한다는 걸 배웠다. 그리고 배열 칸과 클래스 필드 모두 "값을 안 넣어도 기본값으로 채워진다"는 공통점이 있었고, 생성자는 그 객체가 태어나는 순간 필드를 어떻게 채울지 정하는 역할이었다.

## 더 학습하면 좋은 개념

- **`this` 키워드** — 생성자 안에서 `this.id = id`처럼 쓴 `this`가 정확히 무엇을 가리키는지, 왜 매개변수 이름과 필드 이름이 같아도 구분되는지 짚어볼 필요가 있다.
- **생성자 오버로딩** — 매개변수 없는 생성자와 있는 생성자를 동시에 두고 싶다면 어떻게 해야 하는지(둘 다 직접 작성해야 한다는 것과 연결된다).
- **`String`과 `StringBuilder`** — 오늘 `concat()`으로 문자열을 이어 붙였는데, 문자열을 반복해서 수정할 일이 많다면 `String`이 불변(immutable)이라는 점 때문에 `StringBuilder`를 쓰는 게 더 나을 수 있다.
- **배열의 배열(2차원 배열)** — 오늘은 1차원 배열만 다뤘는데, 표 형태의 데이터를 다루려면 배열 안에 배열을 넣는 2차원 배열이 필요해진다.

## 참고 자료

- [Oracle Java Tutorials - Strings](https://docs.oracle.com/javase/tutorial/java/data/strings.html)
- [Oracle - String (Java SE 21 API)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html)
- [Oracle Java Tutorials - Arrays](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)
- [Oracle Java Tutorials - Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html)
- [Oracle Java Tutorials - Creating Objects (생성자)](https://docs.oracle.com/javase/tutorial/java/javaOO/objectcreation.html)
