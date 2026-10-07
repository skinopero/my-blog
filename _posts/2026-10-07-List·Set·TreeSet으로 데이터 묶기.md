---
layout: post
title: "List·Set·TreeSet으로 데이터 묶기"
date: 2026-10-07 09:00:00 +0900
categories: [개념]
tags: [java, collection, list, set]
mermaid: true
---

오늘 제네릭 다음으로 배운 건 컬렉션(Collection)이었다. 배열은 크기가 고정되고 한 번 만들면 늘릴 수 없는데, 컬렉션은 그 한계를 풀어주는 자료구조였다.

## 1. List, Set, Map — 성격이 다른 세 가지 묶음

| 컬렉션 | 순서 | 중복 |
|---|---|---|
| `List` | 있음 (index로 접근) | 허용 |
| `Set` | 없음 | 허용 안 함 |
| `Map` | 키-값 쌍 (key는 `Set`처럼 동작) | key는 허용 안 함 |

인터페이스(`List`, `Set`)는 그 자체로 객체를 만들 수 없어서, 그걸 구현한 클래스(`ArrayList`, `HashSet` 등)를 통해 인스턴스를 만든다.

## 2. List — 타입을 안 정하면 생기는 일

```java
List list = new ArrayList();  // 타입을 지정하지 않음
list.add("apple");
list.add(2);
list.add(2.234);
list.add(true);
list.add(new Date());
```

타입을 안 정해두니 문자열이든 숫자든 날짜든 전부 한 바구니에 들어갔다. 배열과 달리 `list.add(1, "banana")`처럼 중간에 끼워 넣거나 `list.remove(2)`로 특정 위치를 지울 수도 있었다 — 배열의 "크기 고정, 수정 불가"라는 단점을 해결한 부분이었다.

```java
List<String> strings = new ArrayList<>();
strings.add("a"); strings.add("c"); strings.add("b"); strings.add("d");
Collections.sort(strings);  // [a, b, c, d]
```

타입을 `<String>`으로 못 박아두면 `Collections.sort()`처럼 정렬 같은 기능도 안전하게 쓸 수 있었다.

## 3. DTO + ArrayList — 책 목록 관리하기

**DTO(Data Transfer Object)**는 메소드(행위)가 아니라 **데이터 운반**에 집중하는 클래스다. 관례상 이 여섯 가지를 갖춘다.

| 구성 요소 | 역할 |
|---|---|
| 필드 | 운반할 데이터 (책번호, 제목, 저자, 가격) |
| 기본 생성자 | 빈 객체를 만들 때 |
| 모든 필드를 초기화하는 생성자 | 한 번에 값을 채워 만들 때 |
| getter | 값을 꺼낼 때 |
| setter | 값을 바꿀 때 |
| `toString()` | 출력할 때 주소 대신 실제 값이 보이게 |

```java
List<bookDTO> bookList = new ArrayList<>();
bookList.add(new bookDTO(1, "홍길동전", "허균", 50000));
bookList.add(new bookDTO(2, "목민심서", "정약용", 45000));
// ...

for (bookDTO book : bookList) {              // 향상된 for문
    System.out.println(book.getNo() + "번째 책 : " + book);
}
```

`for (타입 변수 : 컬렉션)` 형태의 **향상된 for문**을 쓰면 인덱스를 직접 세지 않아도 컬렉션 안의 값을 하나씩 꺼내 쓸 수 있었다.

## 4. Set — 순서도 없고, 중복도 없다

```java
Set<String> hset = new HashSet<>();
hset.add("java"); hset.add("db"); hset.add("spring"); hset.add("jpa");
hset.add("jpa");  // 중복! 하나만 남는다
System.out.println(hset);  // 순서는 보장되지 않음
```

다섯 살 아이에게 설명한다면: "List는 번호표를 들고 줄 서는 거라서 같은 사람이 두 번 줄 설 수도 있어. Set은 이름표 없이 바구니에 던져 넣는 거라서, 이미 있는 이름을 또 던지면 그냥 하나로 합쳐져 버려."

![List는 번호가 매겨진 줄, Set은 중복 없는 바구니로 비교한 그림]({{ site.baseurl }}/assets/images/2026-10-07-list-vs-set.svg)

## 5. TreeSet — 중복도 없고, 정렬까지 보장

```java
Set<Integer> lotto = new TreeSet<>();
while (lotto.size() < 7) {
    lotto.add((int)(Math.random() * 45) + 1);  // 1~45 사이 난수
}
System.out.println("lotto = " + lotto);  // 항상 오름차순으로 출력됨
```

`Math.random()`은 0 이상 1 미만의 난수를 주기 때문에, `* 45`로 범위를 늘리고 `+ 1`로 최솟값을 1로 맞췄다. `HashSet`과 똑같이 중복은 허용하지 않지만, `TreeSet`은 **이진 검색 트리** 구조라서 데이터가 항상 정렬된 상태로 유지된다는 차이가 있었다. 그래서 로또 번호를 뽑는 것처럼 "중복 없이, 정렬된 상태로" 모아야 할 때 잘 맞았다.

## 오늘의 정리

List는 "순서와 중복을 둘 다 허용"하고, Set은 "둘 다 허용하지 않고", TreeSet은 거기에 "정렬까지 보장"한다. 배열의 고정 크기 문제를 List가 풀어주고, 중복 제거가 필요하면 Set으로, 거기에 정렬까지 필요하면 TreeSet으로 올라가는 흐름이 자연스러웠다.

## 더 학습하면 좋은 개념

- **`LinkedList`** — 오늘은 `ArrayList`만 썼는데, 같은 `List` 인터페이스를 구현하면서도 내부 구조가 달라 삽입·삭제가 잦을 때 더 유리한 구현체가 있다.
- **`Map`과 `HashMap`** — 오늘 표에서 잠깐 언급만 했는데, 키-값 쌍으로 데이터를 다루는 `Map`은 다음 단계에서 비중 있게 다룰 필요가 있다.
- **`Comparable`과 `Comparator`** — `Collections.sort()`가 문자열은 기본으로 정렬했는데, `bookDTO`처럼 직접 만든 타입을 가격순으로 정렬하려면 이 둘 중 하나를 알아야 한다.
- **Stream API** — 컬렉션을 반복문 대신 더 선언적으로 다루는 방법. 향상된 for문의 다음 단계로 자연스럽게 이어진다.

## 참고 자료

- [Oracle Java Tutorials - The List Interface](https://docs.oracle.com/javase/tutorial/collections/interfaces/list.html)
- [Oracle Java Tutorials - The Set Interface](https://docs.oracle.com/javase/tutorial/collections/interfaces/set.html)
- [Oracle - TreeSet (Java SE 21 API)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeSet.html)
