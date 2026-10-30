---
layout: picturebook
title: "Flexbox·Grid 그림책"
date: 2026-09-03 09:10:00 +0900
categories: [그림책]
tags: [css, flexbox, grid, tailwind, eli5]
excerpt: "책상 위 장난감을 한 줄로 세우는 Flexbox와 바둑판 칸에 넣는 Grid를 비교하고, Tailwind는 미리 만든 스티커를 바로 붙이는 방식이라는 비유로 설명하는 그림책."
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
.sky{fill:var(--sky)} .sky-bg{fill:var(--sky-bg)} .sun{fill:var(--sun)} .sun-bg{fill:var(--sun-bg)}
.leaf{fill:var(--leaf)} .leaf-bg{fill:var(--leaf-bg)} .berry{fill:var(--berry)} .berry-bg{fill:var(--berry-bg)}
.paper{fill:var(--paper)} .bg{fill:var(--bg)}
.ln{stroke:var(--line);fill:none;stroke-width:2}
.arr{stroke:var(--ink);fill:none;stroke-width:2.2;stroke-linecap:round}
.arr-b{stroke:var(--berry);fill:none;stroke-width:2.2;stroke-linecap:round}
.ah{fill:var(--ink)} .ah-b{fill:var(--berry)}
.bd{stroke:var(--line);stroke-width:1.5}
</style>

<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <marker id="mk" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah"/></marker>
    <marker id="mkb" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah-b"/></marker>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">CSS 배치 · 다섯 살도 아는 그림 설명</span>
  <h1>장난감 정리하는 두 가지 방법</h1>
  <p class="lead">카드 세 장을 나란히 두고 싶은 이야기예요. 책상 위에 장난감을 줄 세우는 것과, 바둑판처럼 칸을 그려두고 넣는 것 중 뭐가 다른지 봐요.</p>
</header>

<!-- 1 -->
<section>
  <h2>1. 그냥 두면 위아래로 쌓여요</h2>
  <p>카드를 아무것도 안 하고 그냥 두면, 한 장씩 <b>위에서 아래로 차곡차곡</b> 쌓여요. 가로로 나란히 두고 싶어도 저절로 그렇게 되지는 않아요.</p>
  <div class="pic">
  <svg viewBox="0 0 400 260" role="img" aria-label="카드 세 장이 세로로 쌓여 있다">
    <rect x="130" y="20" width="140" height="60" rx="10" class="sun-bg"/><text x="200" y="55" text-anchor="middle" class="t-b">카드 1</text>
    <rect x="130" y="100" width="140" height="60" rx="10" class="sun-bg"/><text x="200" y="135" text-anchor="middle" class="t-b">카드 2</text>
    <rect x="130" y="180" width="140" height="60" rx="10" class="sun-bg"/><text x="200" y="215" text-anchor="middle" class="t-b">카드 3</text>
    <text x="340" y="130" class="t-s" transform="rotate(90 340 130)">위→아래로 쌓임</text>
  </svg>
  </div>
</section>

<!-- 2 -->
<section>
  <h2>2. Flexbox — 한 줄로 나란히 세우기</h2>
  <p>카드를 담은 <b>상자 전체</b>에게 "얘들아, 한 줄로 서!"라고 말해주는 게 Flexbox예요. 그러면 카드들이 가로로 나란히 서요. 그다음엔 두 가지를 정할 수 있어요 — <b>가로로 어디에 모일지</b>(왼쪽/가운데/오른쪽/양끝), <b>세로로 어디에 맞출지</b>예요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 240" role="img" aria-label="카드 세 장이 가로로 나란히 서 있고 양끝 정렬된 모습">
    <rect x="20" y="30" width="720" height="180" rx="16" class="bg" stroke="var(--line)" stroke-dasharray="6 5"/>
    <text x="40" y="54" class="t-s">카드를 담은 상자</text>
    <rect x="50" y="80" width="140" height="100" rx="10" class="sun-bg"/><text x="120" y="135" text-anchor="middle" class="t-b">카드 1</text>
    <rect x="310" y="90" width="140" height="80" rx="10" class="leaf-bg"/><text x="380" y="135" text-anchor="middle" class="t-b">카드 2</text>
    <rect x="570" y="80" width="140" height="100" rx="10" class="sky-bg"/><text x="640" y="135" text-anchor="middle" class="t-b">카드 3</text>
    <text x="380" y="30" text-anchor="middle" class="t-cs">양 끝에 붙이고 사이는 고르게</text>
    <text x="120" y="200" text-anchor="middle" class="t-s">세로는 위쪽 맞춤</text>
    <text x="640" y="200" text-anchor="middle" class="t-s">세로는 위쪽 맞춤</text>
  </svg>
  </div>
  <p class="cap">설명이 길어서 카드 2가 키가 작아도, 세로 맞춤을 "위쪽"으로 해두면 다른 카드를 억지로 늘리지 않고 자기 크기만큼만 유지해요.</p>
</section>

<!-- 3 -->
<section>
  <h2>3. 공간이 모자라면 다음 줄로</h2>
  <p>카드가 많아서 한 줄에 다 안 들어가면 어떻게 할까요? "모자라면 다음 줄로 넘어가도 돼"라고 미리 말해두면, 넘치는 카드가 알아서 아랫줄로 내려가요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 260" role="img" aria-label="카드가 많아져서 둘째 줄로 넘어가는 모습">
    <rect x="20" y="20" width="720" height="220" rx="16" class="bg" stroke="var(--line)" stroke-dasharray="6 5"/>
    <rect x="40" y="50" width="130" height="80" rx="10" class="sun-bg"/><text x="105" y="95" text-anchor="middle" class="t-b">1</text>
    <rect x="190" y="50" width="130" height="80" rx="10" class="sun-bg"/><text x="255" y="95" text-anchor="middle" class="t-b">2</text>
    <rect x="340" y="50" width="130" height="80" rx="10" class="sun-bg"/><text x="405" y="95" text-anchor="middle" class="t-b">3</text>
    <rect x="490" y="50" width="130" height="80" rx="10" class="sun-bg"/><text x="555" y="95" text-anchor="middle" class="t-b">4</text>
    <path d="M640,90 L700,90" class="arr-b" marker-end="url(#mkb)"/>
    <text x="640" y="70" class="t-s" fill="var(--berry)">더 안 들어가요!</text>
    <rect x="40" y="150" width="130" height="80" rx="10" class="leaf-bg"/><text x="105" y="195" text-anchor="middle" class="t-b">5</text>
    <text x="300" y="195" class="t-s">↙ 둘째 줄로 톡 떨어져요</text>
  </svg>
  </div>
</section>

<!-- 4 -->
<section>
  <h2>4. Grid — 칸을 먼저 그려두기</h2>
  <p>Flexbox가 "줄을 세우고 정렬"하는 거라면, Grid는 <b>바둑판처럼 칸을 먼저 그려두고</b> 그 칸 안에 하나씩 넣는 방식이에요. "세로줄 3개로 나눠줘"라고만 하면, 몇 번째 가로줄에 놓을지는 자동으로 정해져요.</p>
  <div class="pic">
  <svg viewBox="0 0 600 280" role="img" aria-label="바둑판처럼 그려둔 칸에 카드 6장이 채워진 모습">
    <text x="300" y="26" text-anchor="middle" class="t-s">세로줄 3개로 미리 그려둔 칸</text>
    <g>
      <rect x="30" y="50" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
      <rect x="215" y="50" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
      <rect x="400" y="50" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
      <rect x="30" y="155" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
      <rect x="215" y="155" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
      <rect x="400" y="155" width="170" height="90" rx="10" class="sky-bg" stroke="var(--line)" stroke-dasharray="4 4"/>
    </g>
    <text x="115" y="100" text-anchor="middle" class="t-b">1</text>
    <text x="300" y="100" text-anchor="middle" class="t-b">2</text>
    <text x="485" y="100" text-anchor="middle" class="t-b">3</text>
    <text x="115" y="205" text-anchor="middle" class="t-b">4</text>
    <text x="300" y="205" text-anchor="middle" class="t-b">5</text>
    <text x="485" y="205" text-anchor="middle" class="t-b">6</text>
    <text x="300" y="270" text-anchor="middle" class="t-s">6개를 넣으면 자동으로 2줄이 돼요</text>
  </svg>
  </div>
  <div class="dict">
    <div class="term"><b>Flexbox · 줄 세우기</b><span>한 방향으로 줄을 세우고 간격을 정렬해요</span></div>
    <div class="term"><b>Grid · 바둑판 칸</b><span>칸을 먼저 그려두고 그 안에 채워 넣어요</span></div>
  </div>
</section>

<!-- 5 -->
<section>
  <h2>5. 마우스를 올리면 움직이는 카드</h2>
  <p>카드 위에 마우스를 올리면 살짝 떠오르면서 커지게 만들 수 있어요. "NEW" 표시가 빨강→주황→빨강으로 계속 깜빡이게 만드는 것도 가능해요. 둘 다 "시간에 따라 모양이 천천히 바뀌는 효과"예요.</p>
  <div class="pic">
  <svg viewBox="0 0 600 220" role="img" aria-label="마우스를 올리면 카드가 떠오르고, NEW 배지가 깜빡이는 모습">
    <rect x="60" y="100" width="160" height="90" rx="12" class="paper bd"/>
    <text x="140" y="150" text-anchor="middle" class="t-b">가만히</text>
    <rect x="340" y="70" width="160" height="90" rx="12" class="sun-bg"/>
    <text x="420" y="120" text-anchor="middle" class="t-b">↑ 떠오르고 커짐</text>
    <circle cx="500" cy="60" r="22" class="berry"/>
    <text x="500" y="65" text-anchor="middle" class="t-w" style="font-size:13px">NEW</text>
    <text x="500" y="100" text-anchor="middle" class="t-s">깜빡깜빡</text>
    <path d="M250,145 L320,120" class="arr" marker-end="url(#mk)"/>
    <text x="280" y="105" text-anchor="middle" class="t-s">마우스 올리면</text>
  </svg>
  </div>
</section>

<!-- 6 -->
<section>
  <h2>6. Tailwind — 미리 만들어진 스티커 상자</h2>
  <p>원래는 "이 카드는 흰 배경, 둥근 모서리, 마우스 올리면 위로 떠오르게" 같은 규칙을 <b>다른 종이(CSS 파일)</b>에 하나씩 적어둬야 해요. Tailwind는 이런 규칙들을 이미 <b>스티커</b>로 만들어둔 상자예요. 카드에 바로 스티커를 붙이기만 하면 끝이에요.</p>
  <div class="pic">
  <svg viewBox="0 0 700 240" role="img" aria-label="별도 종이에 규칙을 적는 방식과 스티커를 바로 붙이는 방식 비교">
    <rect x="20" y="20" width="310" height="200" rx="16" class="bg" stroke="var(--line)"/>
    <text x="175" y="50" text-anchor="middle" class="t-b">순수 CSS</text>
    <rect x="50" y="70" width="120" height="70" rx="10" class="sun-bg"/><text x="110" y="110" text-anchor="middle" class="t-s">카드</text>
    <rect x="200" y="70" width="100" height="100" rx="8" class="paper bd"/>
    <text x="250" y="100" text-anchor="middle" class="t-s">📋 규칙 적힌</text>
    <text x="250" y="118" text-anchor="middle" class="t-s">다른 종이</text>
    <path d="M172,105 L198,105" class="arr" marker-end="url(#mk)"/>
    <text x="175" y="200" text-anchor="middle" class="t-s">카드와 규칙이 따로 있어요</text>

    <rect x="370" y="20" width="310" height="200" rx="16" class="leaf-bg"/>
    <text x="525" y="50" text-anchor="middle" class="t-b">Tailwind</text>
    <rect x="420" y="80" width="200" height="80" rx="10" class="paper bd"/>
    <text x="520" y="115" text-anchor="middle" class="t-s">카드</text>
    <circle cx="450" cy="70" r="14" class="sky"/>
    <circle cx="480" cy="70" r="14" class="sun"/>
    <circle cx="510" cy="70" r="14" class="berry"/>
    <text x="525" y="190" text-anchor="middle" class="t-s">스티커(클래스)를 바로 붙여요</text>
  </svg>
  </div>
</section>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  여러 개를 한 줄로 세우고 정렬할 땐 Flexbox, 칸을 미리 그려두고 채울 땐 Grid를 써요. Tailwind는 규칙을 따로 적는 대신, 미리 만들어진 스티커를 카드에 바로 붙이는 방식이에요.
</div>
</div>
