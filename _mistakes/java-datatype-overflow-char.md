---
layout: post
title: "자료형 크기, byte 오버플로우, char 형변환에서 틀린 것들"
date: 2026-09-28
categories: [오답노트]
tags: [java, datatype, overflow, char]
---

자바 기초 퀴즈를 풀다가 틀린 네 문제를 정리했어. 다 "설명 들으면 아는데 문제로 나오면 헷갈리는" 유형이라, 각각 왜 헷갈렸는지까지 같이 적어둔다.

## 1. 자료형 크기 — long을 4바이트로 착각

**문제.** 다음 중 자료형과 크기(byte)의 연결이 잘못된 것은?

| 보기 | 내용 |
|---|---|
| A | `byte` — 1 byte |
| B | `int` — 4 byte |
| C | `long` — 4 byte |
| D | `double` — 8 byte |

**정답: C.** `long`은 4바이트가 아니라 8바이트야.

| 자료형 | 크기 |
|---|---|
| `byte` | 1 byte |
| `short` | 2 byte |
| `int` | 4 byte |
| `long` | 8 byte |

정수형은 `byte → short → int → long` 순서로 **두 배씩** 커져. `int`가 4바이트라는 것만 강하게 외워둬서, `long`도 "그냥 좀 더 큰 정수"라고 뭉뚱그려 생각하다가 4바이트로 착각했다. `long`은 `int`의 두 배(8바이트)라는 걸 순서와 함께 기억해야 한다.

## 2. byte 오버플로우 — 127에 1을 더하면?

**문제.**

```java
byte bnum = 127;
bnum++;
System.out.println(bnum);
```

| 보기 | 내용 |
|---|---|
| A | 128 |
| B | -128 |
| C | 0 |
| D | -1 |

**정답: B.** `byte`는 **2의 보수법**으로 -128 ~ 127을 표현해.

`127`을 이진수로 쓰면 `01111111`이야. 여기에 1을 더하면 `10000000`이 되는데, `byte`에서 이 비트 패턴은 128이 아니라 **-128**을 의미해. 최상위 비트가 부호 비트라서, 128이라는 값 자체를 `byte`가 표현할 수 없어(`byte`의 최댓값은 127) 오버플로우가 나면서 값이 반대편 끝(-128)으로 넘어가 버려.

"1을 더했으니 128이 되겠지"라고 산술적으로만 생각하면 틀리기 쉬운 문제였다. `byte`·`short`·`int`처럼 크기가 정해진 자료형은 **표현 범위가 있고, 그 범위를 넘으면 순환한다**는 걸 숫자가 아니라 비트로 생각해야 한다.

## 3. char와 int, 그리고 형변환

**문제.**

```java
char a = 'a';
int num3 = a;
System.out.println(num3); // ①

char c = (char) ('a' + 1);
System.out.println(c);    // ②
```

| 보기 | ① | ② |
|---|---|---|
| A | 97 | b |
| B | a1 | b |
| C | 97 | a1 |
| D | a | 98 |

**정답: A.**

`char`는 사실 내부적으로 문자가 아니라 **정수(유니코드 코드포인트)**를 저장하는 자료형이야. `'a'`의 코드값은 97이라서, `int num3 = a;`처럼 `int`에 대입하는 순간 문자가 아니라 **숫자 97**로 취급돼서 출력된다.

②는 반대 방향. `'a' + 1`을 계산할 때 `char`인 `'a'`는 자동으로 `int`(97)로 승격되고, 거기에 1을 더해 `98`이 나온다. 이 `98`을 다시 `(char)`로 캐스팅하면 코드값 98에 해당하는 문자, 즉 `'b'`로 바뀐다.

`char`를 "그냥 글자 한 칸"으로만 생각하면 이 문제가 헷갈린다. `char`는 **글자처럼 보이는 정수**라서, 정수 자리에 들어가면 숫자로, 문자 자리에 들어가면 글자로 나온다는 것이 핵심이다.

## 4. Scanner로 한 줄 입력받기

**문제.** 키보드로 입력받은 한 줄을 문자열로 읽는 `Scanner`의 메서드는?

| 보기 | 내용 |
|---|---|
| A | `nextInt()` |
| B | `nextLine()` |
| C | `readLine()` |
| D | `input()` |

**정답: B.** `nextLine()`은 입력 한 줄 전체를 문자열로 읽어.

헷갈리기 쉬운 이유는 `readLine()`이 이름만 보면 정말 그럴듯해서다. 하지만 `readLine()`은 `Scanner`가 아니라 `BufferedReader`의 메서드야. `Scanner`에는 `nextInt()`, `nextDouble()`, `nextLine()`처럼 `next` 계열 이름만 있고, `readLine()`은 아예 존재하지 않는다. "한 줄 읽기"라는 동작만 보고 이름을 유추하지 말고, **어느 클래스의 메서드인지**부터 확인해야 한다.

## 오늘의 패턴

네 문제 중 세 개가 "값이 어떻게 저장되고 표현되는지"를 몰라서 틀렸다 — 자료형 크기, 비트로 본 오버플로우, `char`의 정수 표현. 눈에 보이는 코드 뒤에 있는 실제 비트·범위를 그려보는 연습이 더 필요하다.

## 참고 자료

- [Oracle Java Tutorials - Primitive Data Types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
- [Oracle - Scanner (Java SE 21 API)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Scanner.html)
