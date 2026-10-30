---
layout: picturebook
title: "Hello.java가 CPU에 도착하기까지 그림책"
date: 2026-09-28 09:00:00 +0900
categories: [그림책]
tags: [java, jvm, eli5]
excerpt: "내가 쓴 주문서(소스코드)를 통역사(컴파일러)가 공용 주문표(바이트코드)로 바꾸고, 주방(JVM)이 다시 통역해서 요리사(CPU)가 실행하는 과정을 러시아 인형(JDK/JRE/JVM) 비유로 설명하는 그림책."
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Jua&family=Gowun+Dodum&family=JetBrains+Mono:wght@400;600&display=swap">
<style>
:root{
  --bg:#F4F7FB; --paper:#FFFFFF; --ink:#1E2940; --soft:#5B6884; --line:#D5DDEA;
  --sky:#3577BF; --sky-bg:#E3EEFA; --sun:#E9A21F; --sun-bg:#FDF1D8;
  --leaf:#2F9C6E; --leaf-bg:#DDF3E9; --berry:#C9486C; --berry-bg:#FBE3EA;
  --skin:#F6D2B4; --hair:#4A3426;
  --f-display:"Jua","Gowun Dodum",system-ui,sans-serif;
  --f-body:"Gowun Dodum","Apple SD Gothic Neo","Malgun Gothic",system-ui,sans-serif;
  --f-code:"JetBrains Mono",ui-monospace,Menlo,monospace;
}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]){
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; color-scheme:dark}
body{background:var(--bg);color:var(--ink);font-family:var(--f-body);font-size:17px;line-height:1.7}
.wrap{max-width:820px;margin:0 auto;padding-block:40px 64px;padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));display:grid;gap:44px}
header{display:grid;gap:8px}
.eyebrow{font-size:13px;letter-spacing:.08em;color:var(--sky);font-weight:700}
h1,h2{font-family:var(--f-display);font-weight:400;text-wrap:balance;margin:0;line-height:1.25}
h1{font-size:clamp(34px,7vw,52px)}
h2{font-size:clamp(24px,4.5vw,30px)}
p{margin:0;max-width:62ch}
.lead{font-size:19px;color:var(--soft)}
section{display:grid;gap:14px}
.pic{background:var(--paper);border:1px solid var(--line);border-radius:18px;padding:18px;overflow-x:auto}
.pic svg{display:block;width:100%;height:auto;min-width:560px}
.cap{font-size:14px;color:var(--soft)}
code{font-family:var(--f-code);font-size:.86em;background:var(--sky-bg);color:var(--sky);padding:1px 6px;border-radius:6px}
.dict{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.term{background:var(--paper);border:1px solid var(--line);border-radius:14px;padding:14px 16px;display:grid;gap:4px;min-width:0}
.term b{font-family:var(--f-display);font-weight:400;font-size:20px}
.term small{font-family:var(--f-code);font-size:12px;color:var(--soft)}
.sum{background:var(--sun-bg);border-radius:18px;padding:20px 22px;font-size:18px}
.t{fill:var(--ink);font-family:var(--f-body);font-size:15px}
.t-b{fill:var(--ink);font-family:var(--f-display);font-size:18px}
.t-s{fill:var(--soft);font-family:var(--f-body);font-size:13px}
.t-c{fill:var(--ink);font-family:var(--f-code);font-size:13px}
.t-cs{fill:var(--soft);font-family:var(--f-code);font-size:12px}
.t-w{fill:var(--paper);font-family:var(--f-display);font-size:16px}
.skin{fill:var(--skin)} .hair{fill:var(--hair)} .eye{fill:var(--ink)}
.sky{fill:var(--sky)} .sky-bg{fill:var(--sky-bg)} .sun{fill:var(--sun)} .sun-bg{fill:var(--sun-bg)}
.leaf{fill:var(--leaf)} .leaf-bg{fill:var(--leaf-bg)} .berry{fill:var(--berry)} .berry-bg{fill:var(--berry-bg)}
.paper{fill:var(--paper)} .bg{fill:var(--bg)}
.ln{stroke:var(--line);fill:none;stroke-width:2}
.arr{stroke:var(--ink);fill:none;stroke-width:2.2;stroke-linecap:round}
.ah{fill:var(--ink)}
.bd{stroke:var(--line);stroke-width:1.5}
</style>

<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <marker id="mk" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah"/></marker>
    <g id="kid">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <path d="M-18,-30 a18,18 0 0 1 36,0 l-5,-6 l-26,0z" class="hair"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-20" y="-8" width="40" height="44" rx="14" class="sky"/>
    </g>
    <g id="helper">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <path d="M-18,-32 a18,18 0 0 1 36,0 l0,10 l-6,-12 l-24,0 l-6,12z" class="hair"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-20" y="-8" width="40" height="46" rx="14" class="leaf"/>
    </g>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">자바가 처음 실행되는 과정 · 다섯 살도 아는 그림 설명</span>
  <h1>내 주문이 요리로 나오기까지</h1>
  <p class="lead">화면에 딱 한 줄, "Hello, Java!"가 나타나기까지 몇 사람을 거치는지 따라가 봐요.</p>
</header>

<!-- 1 -->
<section>
  <h2>1. 내가 쓴 주문서</h2>
  <p>나는 내가 아는 말로 주문서를 써요. "안녕 자바라고 화면에 써줘"라고요. 그런데 이 주문서는 <b>내 말</b>로 쓴 거라서, 주방(컴퓨터)은 아직 이걸 못 알아들어요.</p>
  <div class="pic">
  <svg viewBox="0 0 500 220" role="img" aria-label="아이가 자기 말로 쓴 주문서를 들고 있는 모습">
    <use href="#kid" x="130" y="140"/>
    <text x="130" y="200" text-anchor="middle" class="t-b">나</text>
    <rect x="260" y="60" width="200" height="100" rx="12" class="sun-bg"/>
    <text x="360" y="100" text-anchor="middle" class="t-b">📝 주문서</text>
    <text x="360" y="124" text-anchor="middle" class="t-s">"안녕 자바라고</text>
    <text x="360" y="144" text-anchor="middle" class="t-s">화면에 써줘"</text>
    <text x="360" y="40" text-anchor="middle" class="t-cs">Hello.java</text>
  </svg>
  </div>
</section>

<!-- 2 -->
<section>
  <h2>2. 통역사 아저씨가 공용 주문표로 바꿔요</h2>
  <p>주방은 내 말을 못 알아듣지만, <b>통역사(컴파일러)</b>는 알아들어요. 통역사는 내 주문서를 읽고, 어느 주방에서나 통하는 <b>공용 주문표</b>로 다시 써줘요. 이 공용 주문표가 바로 "바이트코드"예요.</p>
  <div class="pic">
  <svg viewBox="0 0 700 220" role="img" aria-label="통역사가 주문서를 공용 주문표로 바꾸는 모습">
    <rect x="20" y="60" width="150" height="100" rx="12" class="sun-bg"/>
    <text x="95" y="100" text-anchor="middle" class="t-b">📝 주문서</text>
    <text x="95" y="122" text-anchor="middle" class="t-s">내 말</text>
    <text x="95" y="40" text-anchor="middle" class="t-cs">Hello.java</text>
    <use href="#helper" x="330" y="150"/>
    <text x="330" y="40" text-anchor="middle" class="t-b">🗣️ 통역사</text>
    <text x="330" y="62" text-anchor="middle" class="t-cs">컴파일러(javac)</text>
    <path d="M175,110 L290,110" class="arr" marker-end="url(#mk)"/>
    <path d="M375,110 L540,110" class="arr" marker-end="url(#mk)"/>
    <rect x="550" y="60" width="150" height="100" rx="12" class="leaf-bg"/>
    <text x="625" y="100" text-anchor="middle" class="t-b">📋 공용 주문표</text>
    <text x="625" y="122" text-anchor="middle" class="t-s">어디서나 통함</text>
    <text x="625" y="40" text-anchor="middle" class="t-cs">Hello.class · 바이트코드</text>
  </svg>
  </div>
  <p class="cap">문법이 이상하면 통역사가 이 단계에서 "이 말은 못 알아듣겠어요!"라고 바로 알려줘요.</p>
</section>

<!-- 3 -->
<section>
  <h2>3. 상자 안에 상자, 또 상자 — 러시아 인형</h2>
  <p>이 통역사와 주방 도구를 전부 담은 큰 상자를 하나만 사면 돼요. 그 상자를 열어보면 안에 중간 상자가, 그 안에 또 작은 상자가 들어있어요. 큰 상자 하나만 있으면 안의 작은 상자들도 전부 같이 따라와요.</p>
  <div class="pic">
  <svg viewBox="0 0 400 260" role="img" aria-label="큰 인형 안에 중간 인형, 그 안에 작은 인형이 들어있는 모습">
    <ellipse cx="200" cy="160" rx="160" ry="90" class="sun-bg"/>
    <text x="200" y="50" text-anchor="middle" class="t-b">가장 큰 상자</text>
    <text x="200" y="72" text-anchor="middle" class="t-cs">JDK · 만들기+실행 도구 전부</text>
    <ellipse cx="200" cy="175" rx="110" ry="62" class="leaf-bg"/>
    <text x="200" y="128" text-anchor="middle" class="t-s">중간 상자</text>
    <text x="200" y="146" text-anchor="middle" class="t-cs">JRE · 실행 환경</text>
    <ellipse cx="200" cy="195" rx="60" ry="34" class="sky"/>
    <text x="200" y="200" text-anchor="middle" class="t-w">제일 작은 상자</text>
    <text x="200" y="240" text-anchor="middle" class="t-cs">JVM · 통역하는 기계</text>
  </svg>
  </div>
  <p class="cap">큰 상자(JDK) 하나만 설치하면, 통역사도 주방 기계도 전부 한 번에 생겨요.</p>
</section>

<!-- 4 -->
<section>
  <h2>4. 주방이 공용 주문표를 다시 내 주방 말로</h2>
  <p>공용 주문표는 <b>이 컴퓨터만 알아듣는 말</b>이 아니에요. 그래서 주방(JVM)이 한 번 더 통역해서, <b>진짜 요리사(CPU)</b>가 바로 알아듣는 말로 바꿔줘요. 그제서야 CPU가 실제로 요리를 해요.</p>
  <div class="pic">
  <svg viewBox="0 0 700 220" role="img" aria-label="주방이 공용 주문표를 CPU가 아는 말로 바꿔서 전달하는 모습">
    <rect x="20" y="60" width="150" height="100" rx="12" class="leaf-bg"/>
    <text x="95" y="100" text-anchor="middle" class="t-b">📋 공용 주문표</text>
    <text x="95" y="40" text-anchor="middle" class="t-cs">Hello.class</text>
    <rect x="280" y="40" width="160" height="140" rx="14" class="sky-bg"/>
    <text x="360" y="75" text-anchor="middle" class="t-b">🏠 주방</text>
    <text x="360" y="95" text-anchor="middle" class="t-cs">JVM</text>
    <text x="360" y="125" text-anchor="middle" class="t-s">"이 컴퓨터 말로</text>
    <text x="360" y="143" text-anchor="middle" class="t-s">다시 바꿔줄게"</text>
    <path d="M175,110 L270,110" class="arr" marker-end="url(#mk)"/>
    <path d="M445,110 L540,110" class="arr" marker-end="url(#mk)"/>
    <rect x="550" y="60" width="140" height="100" rx="12" class="sun-bg"/>
    <text x="620" y="100" text-anchor="middle" class="t-b">👨‍🍳 요리사</text>
    <text x="620" y="122" text-anchor="middle" class="t-s">CPU가 진짜로 요리</text>
    <text x="620" y="40" text-anchor="middle" class="t-cs">"Hello, Java!" 출력</text>
  </svg>
  </div>
</section>

<!-- 5 -->
<section>
  <h2>5. 같은 주문표로 어느 주방에서나</h2>
  <p>이 공용 주문표 하나만 있으면 Windows 주방이든, Mac 주방이든, 그 주방에 있는 통역 기계(JVM)가 알아서 자기 주방 말로 바꿔줘요. 그래서 주문서를 한 번만 써두면, 어디서나 똑같이 요리돼요.</p>
  <div class="pic">
  <svg viewBox="0 0 700 240" role="img" aria-label="같은 공용 주문표가 여러 다른 주방에서 똑같이 요리되는 모습">
    <rect x="280" y="20" width="140" height="70" rx="10" class="leaf-bg"/>
    <text x="350" y="60" text-anchor="middle" class="t-b">📋 같은 주문표</text>
    <path d="M300,95 C220,130 160,150 120,170" class="arr" marker-end="url(#mk)"/>
    <path d="M350,95 L350,160" class="arr" marker-end="url(#mk)"/>
    <path d="M400,95 C480,130 540,150 580,170" class="arr" marker-end="url(#mk)"/>
    <rect x="40" y="170" width="160" height="60" rx="10" class="sky-bg"/><text x="120" y="205" text-anchor="middle" class="t-s">Windows 주방</text>
    <rect x="270" y="170" width="160" height="60" rx="10" class="sun-bg"/><text x="350" y="205" text-anchor="middle" class="t-s">Mac 주방</text>
    <rect x="500" y="170" width="160" height="60" rx="10" class="berry-bg"/><text x="580" y="205" text-anchor="middle" class="t-s">Linux 주방</text>
  </svg>
  </div>
  <div class="dict">
    <div class="term"><b>통역사 · 컴파일러</b><span>내 말을 공용 주문표로 바꿔줘요</span><small>javac</small></div>
    <div class="term"><b>공용 주문표 · 바이트코드</b><span>어느 주방에서나 통하는 중간 형태</span><small>.class 파일</small></div>
    <div class="term"><b>주방 · JVM</b><span>공용 주문표를 이 컴퓨터 말로 다시 바꿔줘요</span><small>Java Virtual Machine</small></div>
    <div class="term"><b>진짜 요리사 · CPU</b><span>최종적으로 실제 일을 해요</span><small>기계어 실행</small></div>
  </div>
</section>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  내가 쓴 주문서를 통역사(컴파일러)가 공용 주문표(바이트코드)로 바꾸고, 그 주문표를 주방(JVM)이 다시 이 컴퓨터 말로 통역해서 진짜 요리사(CPU)가 실행해요. 주문표 하나만 있으면 어느 주방에서나 똑같이 요리돼요.
</div>
</div>
