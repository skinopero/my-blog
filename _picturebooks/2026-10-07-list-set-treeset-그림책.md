---
layout: picturebook
title: "List·Set·TreeSet 그림책"
date: 2026-10-07 09:00:00 +0900
categories: [그림책]
tags: [java, collection, list, set, eli5]
excerpt: "번호표 들고 서는 줄(List), 중복 없는 바구니(Set), 저절로 정렬되는 마법 책장(TreeSet)으로 세 자료구조의 차이를 설명하는 그림책."
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
h1{font-size:clamp(30px,6vw,46px)}
h2{font-size:clamp(24px,4.5vw,30px)}
p{margin:0;max-width:62ch}
.lead{font-size:19px;color:var(--soft)}
section{display:grid;gap:14px}
.pic{background:var(--paper);border:1px solid var(--line);border-radius:18px;padding:18px;overflow-x:auto}
.pic svg{display:block;width:100%;height:auto;min-width:560px}
.cap{font-size:14px;color:var(--soft)}
.dict{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.term{background:var(--paper);border:1px solid var(--line);border-radius:14px;padding:14px 16px;display:grid;gap:4px;min-width:0}
.term b{font-family:var(--f-display);font-weight:400;font-size:20px}
.sum{background:var(--sun-bg);border-radius:18px;padding:20px 22px;font-size:18px}
.t{fill:var(--ink);font-family:var(--f-body);font-size:15px}
.t-b{fill:var(--ink);font-family:var(--f-display);font-size:18px}
.t-s{fill:var(--soft);font-family:var(--f-body);font-size:13px}
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
      <circle cx="0" cy="-22" r="13" class="skin"/>
      <path d="M-13,-24 a13,13 0 0 1 26,0 l-3,-5 l-20,0z" class="hair"/>
      <circle cx="-4" cy="-22" r="1.6" class="eye"/><circle cx="4" cy="-22" r="1.6" class="eye"/>
      <rect x="-15" y="-7" width="30" height="34" rx="10" class="sky"/>
    </g>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">JAVA · 다섯 살도 아는 그림 설명</span>
  <h1>번호표 줄, 중복 없는 바구니, 저절로 정리되는 책장</h1>
  <p class="lead">List, Set, TreeSet — 여러 개를 한 번에 담아두는 세 가지 방법을, 줄 서기와 바구니, 책장 비유로 풀어볼게요.</p>
</header>

<section>
  <h2>1. 번호표를 들고 줄 서기 — List</h2>
  <p>놀이공원 줄처럼, <b>번호표를 들고 순서대로</b> 줄을 서요. 같은 이름을 가진 친구 두 명이 와도 둘 다 줄을 설 수 있고(중복 허용), 중간에 새치기해서 끼어 넣거나 중간 사람을 빼낼 수도 있어요. 번호표가 있으니 "3번이 누구야?" 하고 바로 찾을 수도 있어요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 240" role="img" aria-label="번호표를 들고 순서대로 서 있는 줄, 같은 이름도 중복 가능">
    <text x="50" y="40" class="t-b">List</text>
    <g>
      <use href="#kid" x="90" y="150"/><circle cx="90" cy="178" r="12" class="paper bd"/><text x="90" y="182" text-anchor="middle" class="t" style="font-size:12px">1</text><text x="90" y="210" text-anchor="middle" class="t-s">민준</text>
      <use href="#kid" x="200" y="150"/><circle cx="200" cy="178" r="12" class="paper bd"/><text x="200" y="182" text-anchor="middle" class="t" style="font-size:12px">2</text><text x="200" y="210" text-anchor="middle" class="t-s">서연</text>
      <use href="#kid" x="310" y="150"/><circle cx="310" cy="178" r="12" class="paper bd"/><text x="310" y="182" text-anchor="middle" class="t" style="font-size:12px">3</text><text x="310" y="210" text-anchor="middle" class="t-s">민준</text>
      <use href="#kid" x="420" y="150"/><circle cx="420" cy="178" r="12" class="paper bd"/><text x="420" y="182" text-anchor="middle" class="t" style="font-size:12px">4</text><text x="420" y="210" text-anchor="middle" class="t-s">하은</text>
    </g>
    <rect x="460" y="90" width="260" height="120" rx="14" class="sky-bg"/>
    <text x="590" y="120" text-anchor="middle" class="t">✅ 같은 이름(민준)도</text>
    <text x="590" y="142" text-anchor="middle" class="t">　두 번 줄 설 수 있어요</text>
    <text x="590" y="172" text-anchor="middle" class="t">✅ 번호표가 있어서</text>
    <text x="590" y="194" text-anchor="middle" class="t">　순서가 그대로 유지돼요</text>
  </svg>
  </div>
  <p class="cap">List는 "순서가 있고(번호표), 중복도 괜찮은" 줄이에요. 중간에 새로 끼워 넣거나, 특정 번호의 사람만 빼낼 수도 있어요.</p>
</section>

<section>
  <h2>2. 중복 없는 바구니 — Set</h2>
  <p>이번엔 번호표 없이, 이름이 적힌 <b>공을 바구니에 던져 넣는다</b>고 생각해 보세요. 순서는 상관없어요. 그런데 이미 바구니 안에 있는 이름과 <b>똑같은 이름의 공을 또 던지면, 그냥 하나로 합쳐져 버려요</b> — 바구니 안에는 같은 이름이 두 개 있을 수 없어요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 260" role="img" aria-label="같은 이름의 공을 또 던지면 하나로 합쳐지는 바구니">
    <circle cx="150" cy="80" r="26" class="sky"/><text x="150" y="86" text-anchor="middle" class="t-w">java</text>
    <circle cx="260" cy="60" r="26" class="leaf"/><text x="260" y="66" text-anchor="middle" class="t-w">db</text>
    <circle cx="370" cy="90" r="26" class="berry"/><text x="370" y="96" text-anchor="middle" class="t-w">jpa</text>
    <circle cx="420" cy="40" r="26" class="berry" opacity=".45" stroke="var(--berry)" stroke-width="3" stroke-dasharray="4 4"/><text x="420" y="46" text-anchor="middle" class="t-w">jpa</text>
    <path d="M420,66 C420,110 400,130 380,145" class="arr" marker-end="url(#mk)"/>
    <text x="470" y="40" class="t-s">또 던지면…</text>

    <path d="M250,130 C250,150 250,160 250,170" class="ln"/>
    <ellipse cx="250" cy="230" rx="220" ry="40" class="sun-bg"/>
    <text x="150" y="222" text-anchor="middle" class="t" style="font-size:13px">java</text>
    <text x="250" y="238" text-anchor="middle" class="t" style="font-size:13px">db</text>
    <text x="340" y="218" text-anchor="middle" class="t" style="font-size:13px">jpa (하나로 합쳐짐)</text>

    <rect x="520" y="60" width="200" height="140" rx="14" class="bg" stroke="var(--line)" stroke-dasharray="6 5"/>
    <text x="620" y="94" text-anchor="middle" class="t-b">바구니 (Set)</text>
    <text x="545" y="126" class="t">· 순서 없음</text>
    <text x="545" y="152" class="t">· 중복 안 됨</text>
    <text x="545" y="178" class="t">· 이름표만 있음</text>
  </svg>
  </div>
  <p class="cap">List는 번호표를 들고 줄 서는 거라 같은 이름이 두 번 줄 설 수 있지만, Set은 이름표만 보고 바구니에 던지는 거라 이미 있는 이름을 또 던지면 그냥 하나로 합쳐져 버려요.</p>
</section>

<section>
  <h2>3. 저절로 정리되는 책장 — TreeSet</h2>
  <p>바구니처럼 중복은 안 되지만, 책을 꽂을 때마다 <b>저절로 순서에 맞게 정리되는 마법 책장</b>이 있다고 생각해 보세요. 아무 번호나 꽂아도, 책장이 알아서 작은 번호부터 큰 번호 순서로 다시 배열해줘요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 240" role="img" aria-label="숫자를 아무렇게나 꽂아도 저절로 오름차순으로 정렬되는 책장">
    <text x="150" y="40" text-anchor="middle" class="t-b">아무렇게나 꽂아도</text>
    <rect x="60" y="60" width="50" height="60" rx="6" class="sun"/><text x="85" y="96" text-anchor="middle" class="t-w" style="font-size:20px">7</text>
    <rect x="120" y="60" width="50" height="60" rx="6" class="sun"/><text x="145" y="96" text-anchor="middle" class="t-w" style="font-size:20px">2</text>
    <rect x="180" y="60" width="50" height="60" rx="6" class="sun"/><text x="205" y="96" text-anchor="middle" class="t-w" style="font-size:20px">9</text>

    <path d="M260,90 C320,90 320,90 380,90" class="arr" marker-end="url(#mk)"/>
    <text x="320" y="70" text-anchor="middle" class="t-s">마법 책장</text>

    <text x="590" y="40" text-anchor="middle" class="t-b">저절로 이렇게 정리돼요</text>
    <rect x="450" y="60" width="50" height="60" rx="6" class="leaf"/><text x="475" y="96" text-anchor="middle" class="t-w" style="font-size:20px">2</text>
    <rect x="510" y="60" width="50" height="60" rx="6" class="leaf"/><text x="535" y="96" text-anchor="middle" class="t-w" style="font-size:20px">7</text>
    <rect x="570" y="60" width="50" height="60" rx="6" class="leaf"/><text x="595" y="96" text-anchor="middle" class="t-w" style="font-size:20px">9</text>

    <rect x="120" y="150" width="520" height="70" rx="14" class="sky-bg"/>
    <text x="380" y="180" text-anchor="middle" class="t">바구니(Set)와 똑같이 중복은 안 되지만,</text>
    <text x="380" y="204" text-anchor="middle" class="t">여기에 더해 항상 작은 수부터 큰 수 순서로 정리돼요</text>
  </svg>
  </div>
  <p class="cap">로또 번호를 뽑을 때처럼 "중복 없이, 보기 좋게 순서대로" 모아야 할 때 이 마법 책장이 딱 맞아요.</p>
</section>

<div class="dict">
  <div class="term"><b>List · 번호표 줄</b><span>순서가 있고, 같은 이름도 중복으로 설 수 있어요</span></div>
  <div class="term"><b>Set · 중복 없는 바구니</b><span>순서는 없고, 같은 이름을 또 넣으면 하나로 합쳐져요</span></div>
  <div class="term"><b>TreeSet · 저절로 정리되는 책장</b><span>중복도 안 되고, 항상 순서대로 저절로 정리돼요</span></div>
</div>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  List는 순서와 중복을 둘 다 허락하는 줄, Set은 둘 다 허락하지 않는 바구니, TreeSet은 거기에 "저절로 정리"까지 더해진 마법 책장이에요.
</div>
</div>
