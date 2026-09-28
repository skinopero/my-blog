---
layout: post
title: "내가 짠 코드로 만든 자바 기초 퀴즈"
date: 2026-09-28
categories: [퀴즈]
tags: [java, operator, variable, quiz]
---

오늘 실습에서 짠 코드 네 덩어리 — 실행 구조, 연산자, 변수, `Scanner` — 를 그대로 다시 읽어보고, 그 안에서 헷갈렸던 부분만 뽑아서 눌러보는 퀴즈로 만들어봤어.

## 1. 실행 구조 — main()이 시작점

```java
package com.wanted.a_basic;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World!!!~");
    }
}
```

자바 프로그램은 전부 클래스 안에서 동작하고, 우리가 만든 `main()` 메서드가 프로그램의 시작점이야. 실행 과정은 다섯 단계.

| 단계 | 하는 일 |
|---|---|
| 1. `.java` 작성 | 사람이 읽을 수 있는 자바 문법으로 코드 작성 |
| 2. `javac` 컴파일 | JDK의 컴파일러가 `.java`를 `.class`(바이트코드)로 번역 |
| 3. `.class` 로드 | JVM이 바이트코드 파일을 읽어들임 |
| 4. 바이트코드 해석 | JVM이 지금 이 OS가 알아듣는 기계어로 변환 |
| 5. 실행 | 변환된 기계어를 CPU가 실행 |

## 2. 연산자 — 문자열과 숫자가 만나면

```java
int a = 10;
int b = 3;

System.out.println("덧셈 : " + (a + b));
System.out.println("나눗셈 : " + (a / b));
System.out.println("나머지 : " + (a % b));

boolean isGreater = a > b;
System.out.println("a != b : " + (a != b));

boolean isTrue = true;
boolean isFalse = false;
System.out.println("둘 다 참이니? : " + (isFalse && isTrue));
System.out.println("둘 중 하나는 참이니? : " + (isFalse || isTrue));

int age = 20;
System.out.println("++age: " + (++age));
System.out.println("age++: " + (age++));
System.out.println("age: " + (age));
```

가장 헷갈렸던 두 가지.

- **문자열 + 연산은 유의해야 한다** — `"안녕" + 3`처럼 문자열이 `+` 연산을 만나면, 피연산자를 문자열로 바꿔서 이어 붙인다. 계산이 아니라 이어붙이기.
- **전위(`++age`) vs 후위(`age++`)** — 전위는 "먼저 더하고 나서 보여준다"이고, 후위는 "먼저 보여주고 나서 더한다"이다. 그래서 `age`가 20일 때 `++age`는 21을 보여주지만, 그다음 `age++`는 (이미 21인) 21을 그대로 보여주고 나서야 22가 된다.

## 3. 변수 — 자료형마다 크기가 다르다

```java
byte b = 10;   // 1 byte
short s = 10;  // 2 byte
int i = 10;    // 4 byte
long l = 10;   // 8 byte

float f = 3.14f;  // 4 byte
double d = 3.14;  // 8 byte

char ch = 'ㅎ';
String str = "안녕하세요!";
String str2 = "안녕하세요!";  // 같은 영역에서 str은 이미 썼으니 이름을 다르게

boolean bl = true;
```

| 자료형 | 크기 | 담는 것 |
|---|---|---|
| `byte` | 1 byte | 정수 |
| `short` | 2 byte | 정수 |
| `int` | 4 byte | 정수 |
| `long` | 8 byte | 정수 |
| `float` | 4 byte | 실수 |
| `double` | 8 byte | 실수 |

리터럴은 "값" 그 자체 — 정수, 실수, 문자, 문자열, 논리로 구성돼. 그리고 같은 중괄호(`{}`) 영역 안에서는 같은 변수 이름을 두 번 쓸 수 없어서, 두 번째 문자열 변수는 `str2`라고 이름을 다르게 지었어.

## 4. 사용자 입력 — Scanner

```java
Scanner sc = new Scanner(System.in);
System.out.print("당신의 이름을 입력하세요 :");
String name = sc.nextLine();
System.out.println("이름 :" + name + "입니다!");
```

`Scanner`는 키보드로 들어오는 입력을 읽는 도구야. `System.in`을 넣어서 만들고, `nextLine()`으로 한 줄 전체를 문자열로 받아.

## 5. 확인 퀴즈

버튼을 눌러보면 바로 정답과 설명이 나와.

{% include quiz-java-operators.html %}

## 더 학습하면 좋은 개념

- **오버플로우(overflow)** — `byte`·`short`·`int`처럼 크기가 정해진 자료형은 담을 수 있는 값의 범위를 넘으면 값이 이상하게 순환해. `int` 최대값에 1을 더하면 왜 음수가 되는지 알아두면 자료형을 왜 상황에 맞게 골라야 하는지 이해된다.
- **형변환(casting)** — 오늘은 문자열과 숫자가 만났을 때 자동으로 문자열로 바뀌는 경우만 봤는데, `int`와 `double`처럼 숫자끼리도 서로 바꿔야 할 때가 있어. 자동 형변환과 명시적 형변환의 차이를 다음에 볼 필요가 있다.
- **단축 평가(short-circuit evaluation)** — `&&`, `||`는 왼쪽 값만으로 결과가 정해지면 오른쪽은 아예 실행하지 않아. `A && B`에서 A가 앞에 오는 게 왜 면접 질문이 되는지는 이 개념을 알아야 완전히 이해된다.
- **`BufferedReader` vs `Scanner`** — 오늘은 `Scanner`로 입력을 받았는데, 실무에서는 더 빠른 `BufferedReader`를 쓰기도 해. 언제 무엇을 골라야 하는지 비교해두면 좋다.
