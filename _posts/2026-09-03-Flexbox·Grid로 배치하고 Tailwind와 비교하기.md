---
layout: post
title: "Flexbox·Grid로 배치하고 Tailwind와 비교하기"
date: 2026-09-03 16:00:00 +0900
categories: [개념]
tags: [css, flexbox, grid, tailwind]
mermaid: true
---

어제 선택자를 배웠다면, 오늘은 선택한 요소들을 실제로 "어떻게 배치할지"를 배웠다. 상품 카드 3장을 나란히 놓는 걸 목표로 Flexbox와 Grid를 써봤고, 마지막엔 같은 결과를 Tailwind CSS로도 만들어 비교했다.

## 1. div만으로 배치하면 생기는 문제

```html
<div class="card">카드1</div>
<div class="card">카드2</div>
<div class="card">카드3</div>
```

`div`는 기본적으로 블록 요소라서, 그냥 두면 화면 가로축을 전부 차지하며 **위에서 아래로 쌓인다.** 화면을 늘리거나 줄였을 때 유연하게 배치되지도 않는다. 카드 3장을 가로로 나란히 두려면 별도의 배치 방식이 필요했다.

## 2. Flexbox — 한 방향으로 줄 세우기

```css
.card-list {
    display: flex;              /* 카드를 감싸는 상자에 적용 */
    flex-direction: row;        /* 기본값: 가로로 */
    justify-content: space-between; /* 주축 방향 정렬 */
    align-items: flex-start;    /* 교차축 방향 정렬 */
    gap: 20px;                  /* 항목 사이 간격 */
    flex-wrap: wrap;            /* 공간 부족하면 다음 줄로 */
}
```

`display: flex`는 카드 하나하나가 아니라 **카드를 감싸는 상자(컨테이너)**에 줘야 한다는 게 핵심이었다. Flex는 두 개의 축을 가진다.

- **주축(main-axis)** — `flex-direction`이 정한 방향(기본은 가로)
- **교차축(cross-axis)** — 주축과 직각인 방향

다섯 살 아이에게 설명한다면: "`justify-content`는 카드들을 가로로 어디에 모을지(왼쪽/가운데/오른쪽/양끝) 정하는 거고, `align-items`는 세로로 어디에 맞출지 정하는 거야. 설명이 긴 카드만 키가 커도, `align-items: flex-start`를 쓰면 다른 카드들을 억지로 늘리지 않고 자기 내용만큼만 높이를 갖게 할 수 있어."

![justify-content는 가로(주축) 정렬을, align-items는 세로(교차축) 정렬을 담당하며, flex-start를 쓰면 카드 높이가 늘어나지 않고 내용만큼만 유지된다는 그림]({{ site.baseurl }}/assets/images/2026-09-03-flex-axis.svg)

| 값(`justify-content`) | 의미 |
|---|---|
| `flex-start`/`center`/`flex-end` | 왼쪽/가운데/오른쪽에 모음 |
| `space-between` | 양 끝에 붙이고 사이를 고르게 |
| `space-around`/`space-evenly` | 바깥쪽 여백까지 나눠 갖는 방식이 다름 |

## 3. Grid — 칸을 그려두고 채우기

```css
.grid-list {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* 열 3개, 남은 공간을 1:1:1로 */
    gap: 20px;
}
```

Flex가 "한 줄로 세우고 정렬"한다면, Grid는 **"칸을 먼저 그려두고 그 칸에 넣으며 정렬"**한다는 차이를 배웠다. 열의 개수(`grid-template-columns`)만 지정하면 행은 자동으로 결정된다 — 아이템 6개를 3열로 지정하면 자동으로 2행이 되는 식이다.

## 4. 블록과 인라인을 CSS로 바꾸기

```css
.box-block-span { display: block; } /* span인데 자기 줄을 차지하게 */
.box-inline-div { display: inline; } /* div인데 옆으로 붙게 */
```

어제 HTML 시간에 배운 "div는 블록, span은 인라인"이라는 기본값을, CSS의 `display` 속성으로 직접 뒤집을 수 있다는 것도 확인했다.

## 5. 호버 애니메이션과 Tailwind 비교

```css
.card:hover {
    transform: translate(0, -8px) scale(1.05); /* 위로 뜨면서 커짐 */
    box-shadow: 0 8px 20px rgba(0,0,0,0.12);
}
.badge {
    animation-name: blink;
    animation-duration: 1.2s;
    animation-iteration-count: infinite;
}
@keyframes blink {
    0%   { background-color: #DC2626; }
    50%  { background-color: #F97316; }
    100% { background-color: #DC2626; }
}
```

마우스를 올리면 카드가 떠오르는 효과(`transform` + `transition`)와, NEW 배지가 깜빡이는 애니메이션(`@keyframes`)을 직접 CSS로 만들어봤다. 그 다음 같은 결과를 **Tailwind CSS**의 유틸리티 클래스로 바꿔봤다.

```html
<!-- 순수 CSS: 클래스 이름 + 별도 .css 파일 -->
<div class="card">...</div>

<!-- Tailwind: 클래스 이름 자체가 스타일 -->
<div class="bg-white border border-gray-200 rounded-lg p-4
            transition hover:bg-gray-50 hover:-translate-y-2 hover:scale-105 hover:shadow-lg">
  ...
</div>
```

| 구분 | 순수 CSS | Tailwind |
|---|---|---|
| 스타일 작성 위치 | 별도 `.css` 파일에 클래스 정의 | HTML 클래스 속성에 바로 작성 |
| 새 스타일이 필요할 때 | CSS 파일에 규칙 추가 | 이미 있는 유틸리티 클래스 조합 |
| 가독성 | CSS와 HTML이 분리되어 있음 | 클래스 목록만 봐도 스타일이 보임 |

## 오늘의 정리

여러 요소를 한 줄로 세우고 정렬할 땐 Flexbox(주축/교차축), 칸을 그려두고 채울 땐 Grid를 쓴다는 기본기를 잡았다. `display` 속성 하나로 블록/인라인 기본값을 뒤집을 수 있다는 것도 확인했고, 같은 카드 배치·호버 효과를 순수 CSS와 Tailwind로 각각 만들어보며 두 방식의 차이를 직접 비교했다.

## 더 학습하면 좋은 개념

- **Flexbox의 `flex-grow`/`flex-shrink`/`flex-basis`** — 오늘은 정렬(`justify-content`, `align-items`)만 다뤘는데, 아이템 각각이 "얼마나 늘어나고 줄어들지"를 정하는 세 속성도 Flexbox의 핵심이다.
- **CSS Grid의 `grid-template-areas`** — 오늘은 단순히 열 개수만 지정했는데, 이름을 붙인 영역으로 레이아웃 전체를 설계하는 더 강력한 방법이 있다.
- **반응형 디자인과 미디어 쿼리(`@media`)** — 화면 크기에 따라 `flex-direction`이나 `grid-template-columns`를 다르게 주는 방법. 오늘 배운 `flex-wrap`과 함께 모바일 대응의 기본이 된다.
- **Tailwind의 설정 파일(`tailwind.config.js`)** — 오늘은 기본 제공되는 클래스만 썼는데, 실무에서는 색상·간격 등을 프로젝트에 맞게 커스터마이징한다.

## 참고 자료

- [MDN - Basic concepts of flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [MDN - CSS grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)
- [Tailwind CSS - Core Concepts](https://tailwindcss.com/docs/styling-with-utility-classes)
