---
layout: picturebook
title: "static·싱글톤·final 그림책"
date: 2026-10-05 09:00:00 +0900
categories: [그림책]
tags: [java, static, singleton, final, eli5]
excerpt: "모두가 함께 보는 교실 칠판(static), 어느 반에서 불러도 같은 한 분인 교장 선생님(싱글톤), 돌에 새긴 이름(final)으로 값을 통제하는 세 가지 방법을 설명하는 그림책."
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Jua&family=Gowun+Dodum&family=JetBrains+Mono:wght@400;600&display=swap">
<style>
:root{
  --bg:#F4F7FB; --paper:#FFFFFF; --ink:#1E2940; --soft:#5B6884; --line:#D5DDEA;
  --sky:#3577BF; --sky-bg:#E3EEFA; --sun:#E9A21F; --sun-bg:#FDF1D8;
  --leaf:#2F9C6E; --leaf-bg:#DDF3E9; --berry:#C9486C; --berry-bg:#FBE3EA;
  --skin:#F6D2B4; --hair:#4A3426; --stone:#8B93A3; --stone-bg:#EAEDF3;
  --f-display:"Jua","Gowun Dodum",system-ui,sans-serif;
  --f-body:"Gowun Dodum","Apple SD Gothic Neo","Malgun Gothic",system-ui,sans-serif;
  --f-code:"JetBrains Mono",ui-monospace,Menlo,monospace;
}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]){
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; --stone:#8893A8; --stone-bg:#232C3F; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; --stone:#8893A8; --stone-bg:#232C3F; color-scheme:dark}
body{background:var(--bg);color:var(--ink);font-family:var(--f-body);font-size:17px;line-height:1.7}
.wrap{max-width:820px;margin:0 auto;padding-block:40px 64px;padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));display:grid;gap:44px}
header{display:grid;gap:8px}
.eyebrow{font-size:13px;letter-spacing:.08em;color:var(--sky);font-weight:700}
h1,h2{font-family:var(--f-display);font-weight:400;text-wrap:balance;margin:0;line-height:1.25}
h1{font-size:clamp(32px,6.5vw,48px)}
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
.term small{font-family:var(--f-code);font-size:12px;color:var(--soft)}
.sum{background:var(--sun-bg);border-radius:18px;padding:20px 22px;font-size:18px}
.t{fill:var(--ink);font-family:var(--f-body);font-size:15px}
.t-b{fill:var(--ink);font-family:var(--f-display);font-size:18px}
.t-s{fill:var(--soft);font-family:var(--f-body);font-size:13px}
.t-w{fill:var(--paper);font-family:var(--f-display);font-size:16px}
.skin{fill:var(--skin)} .hair{fill:var(--hair)} .eye{fill:var(--ink)}
.sky{fill:var(--sky)} .sky-bg{fill:var(--sky-bg)} .sun{fill:var(--sun)} .sun-bg{fill:var(--sun-bg)}
.leaf{fill:var(--leaf)} .leaf-bg{fill:var(--leaf-bg)} .berry{fill:var(--berry)} .berry-bg{fill:var(--berry-bg)}
.stone{fill:var(--stone)} .stone-bg{fill:var(--stone-bg)}
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
      <circle cx="0" cy="-26" r="16" class="skin"/>
      <path d="M-16,-28 a16,16 0 0 1 32,0 l-4,-6 l-24,0z" class="hair"/>
      <circle cx="-5" cy="-26" r="1.8" class="eye"/><circle cx="5" cy="-26" r="1.8" class="eye"/>
      <rect x="-18" y="-8" width="36" height="40" rx="12" class="sky"/>
    </g>
    <g id="kid2">
      <circle cx="0" cy="-26" r="16" class="skin"/>
      <path d="M-16,-28 a16,16 0 0 1 32,0 l-4,-6 l-24,0z" class="hair"/>
      <circle cx="-5" cy="-26" r="1.8" class="eye"/><circle cx="5" cy="-26" r="1.8" class="eye"/>
      <rect x="-18" y="-8" width="36" height="40" rx="12" class="leaf"/>
    </g>
    <g id="principal">
      <circle cx="0" cy="-28" r="17" class="skin"/>
      <path d="M-17,-30 a17,17 0 0 1 34,0 l-4,-7 l-26,0z" class="hair"/>
      <circle cx="-5.5" cy="-28" r="2" class="eye"/><circle cx="5.5" cy="-28" r="2" class="eye"/>
      <rect x="-20" y="-8" width="40" height="44" rx="13" class="berry"/>
      <rect x="-6" y="-6" width="12" height="20" class="paper"/>
    </g>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">JAVA · 다섯 살도 아는 그림 설명</span>
  <h1>모두의 칠판, 교장 선생님, 돌에 새긴 이름</h1>
  <p class="lead">static·싱글톤·final, 이 세 가지는 전부 "값을 어떻게 통제할까"라는 같은 질문에서 나왔어요. 교실 그림으로 하나씩 풀어볼게요.</p>
</header>

<section>
  <h2>1. 각자 책상 vs 모두의 칠판</h2>
  <p>교실에 있는 친구들은 저마다 <b>자기 책상 위에 작은 공책</b>을 하나씩 가지고 있어요. 내가 내 공책에 숫자를 하나 적어도, 옆 친구 공책에는 아무 영향이 없어요. 이게 보통 값이에요.</p>
  <p>그런데 교실 앞에는 <b>모두가 함께 보는 커다란 칠판</b>이 딱 하나 걸려 있어요. 아무나 그 칠판에 숫자를 하나 늘리면, 교실에 있는 <b>모든 친구</b>가 늘어난 숫자를 똑같이 보게 돼요. 이게 바로 공유되는 값이에요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 300" role="img" aria-label="각자의 공책과 모두가 보는 칠판 비교">
    <rect x="20" y="20" width="330" height="260" rx="16" class="sky-bg"/>
    <text x="185" y="52" text-anchor="middle" class="t-b">각자 책상 위 공책</text>
    <use href="#kid" x="100" y="150"/>
    <rect x="70" y="160" width="60" height="40" rx="6" class="paper bd"/>
    <text x="100" y="186" text-anchor="middle" class="t">1</text>
    <use href="#kid2" x="270" y="150"/>
    <rect x="240" y="160" width="60" height="40" rx="6" class="paper bd"/>
    <text x="270" y="186" text-anchor="middle" class="t">0</text>
    <text x="185" y="240" text-anchor="middle" class="t-s">내 공책에 적어도</text>
    <text x="185" y="260" text-anchor="middle" class="t-s">옆 친구 공책은 그대로예요</text>

    <rect x="390" y="20" width="350" height="260" rx="16" class="sun-bg"/>
    <text x="565" y="52" text-anchor="middle" class="t-b">모두의 칠판</text>
    <rect x="470" y="70" width="190" height="70" rx="10" class="paper" stroke="var(--sun)" stroke-width="3"/>
    <text x="565" y="112" text-anchor="middle" class="t-b" style="font-size:26px">3</text>
    <use href="#kid" x="440" y="230"/>
    <use href="#kid2" x="565" y="240"/>
    <use href="#principal" x="690" y="220" transform="translate(690,220) scale(.8) translate(-690,-220)"/>
    <path d="M460,214 L500,150" class="arr" marker-end="url(#mk)"/>
    <path d="M565,220 L565,150" class="arr" marker-end="url(#mk)"/>
    <path d="M670,200 L630,150" class="arr" marker-end="url(#mk)"/>
    <text x="565" y="260" text-anchor="middle" class="t-s">누가 써도 다 같이 보는 숫자예요</text>
  </svg>
  </div>
  <p class="cap">보통 값은 "내 책상 위 공책"처럼 각자 따로 가지고 있어서 서로 안 보여요. 모두가 함께 쓰는 값은 "교실 전체가 보는 칠판" 하나뿐이라, 누가 적어도 전부에게 보여요.</p>
</section>

<section>
  <h2>2. 교장 선생님은 한 명뿐</h2>
  <p>학교 여기저기 교실 문을 두드려서 "교장 선생님 불러주세요!" 하면, 어느 교실에서 불러도 결국 오시는 분은 <b>늘 같은 한 분</b>이에요. 교장실 문도 잠겨 있어서, 아무나 마음대로 "새 교장 선생님"을 또 만들 수는 없어요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 290" role="img" aria-label="어느 교실에서 불러도 같은 교장 선생님이 온다">
    <rect x="30" y="30" width="140" height="90" rx="12" class="sky-bg"/><text x="100" y="80" text-anchor="middle" class="t">1반</text>
    <rect x="30" y="150" width="140" height="90" rx="12" class="leaf-bg"/><text x="100" y="200" text-anchor="middle" class="t">2반</text>
    <rect x="200" y="90" width="140" height="90" rx="12" class="berry-bg"/><text x="270" y="140" text-anchor="middle" class="t">3반</text>
    <path d="M170,75 C320,75 320,220 420,220" class="arr" marker-end="url(#mk)"/>
    <path d="M170,195 C320,195 320,220 420,220" class="arr" marker-end="url(#mk)"/>
    <path d="M340,135 C380,160 380,190 420,220" class="arr" marker-end="url(#mk)"/>
    <use href="#principal" x="460" y="190"/>
    <rect x="420" y="40" width="200" height="60" rx="10" class="stone-bg"/>
    <text x="520" y="76" text-anchor="middle" class="t-b">교장실 문은 잠김</text>
    <text x="460" y="260" text-anchor="middle" class="t-s">어느 반에서 불러도</text>
    <text x="460" y="278" text-anchor="middle" class="t-s">오시는 분은 항상 한 분</text>
  </svg>
  </div>
  <p class="cap">"교장실 문이 잠겨 있다"는 건, 외부에서 마음대로 새 교장 선생님을 만들 수 없게 막아둔 거예요. 교장 선생님을 모시는 유일한 방법은 정해진 창구(안내 데스크)에 요청하는 것뿐이고, 그 창구는 항상 똑같은 한 분을 모셔다 줘요.</p>
  <p>어떤 학교는 <b>아침부터 교장실에 미리 앉아 계시고</b>(이른 준비), 어떤 학교는 <b>처음 누가 부를 때 그제서야 교장실 문을 열고 들어가 앉으세요</b>(천천히 준비) — 준비하는 시점만 다를 뿐, 한 번 자리를 잡으시면 그 뒤로는 똑같이 한 분이에요.</p>
</section>

<section>
  <h2>3. 돌에 새긴 이름</h2>
  <p>칠판에 분필로 쓴 숫자는 지우고 다시 쓸 수 있어요. 그런데 <b>돌에 새긴 이름</b>은 한 번 새기면 다시는 못 바꿔요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 260" role="img" aria-label="분필로 쓴 숫자와 돌에 새긴 이름 비교">
    <rect x="30" y="30" width="320" height="190" rx="16" class="sky-bg"/>
    <text x="190" y="62" text-anchor="middle" class="t-b">분필 칠판</text>
    <rect x="80" y="86" width="220" height="90" rx="8" class="ink" fill="#2B3A52"/>
    <text x="190" y="138" text-anchor="middle" class="t-w" style="font-size:30px">7</text>
    <text x="190" y="196" text-anchor="middle" class="t-s">지우고 다시 쓸 수 있어요</text>

    <rect x="400" y="30" width="330" height="190" rx="16" class="stone-bg"/>
    <text x="565" y="62" text-anchor="middle" class="t-b">돌판</text>
    <rect x="460" y="86" width="210" height="90" rx="8" class="stone"/>
    <text x="565" y="138" text-anchor="middle" class="t-w" style="font-size:30px">서연</text>
    <text x="565" y="196" text-anchor="middle" class="t-s">한 번 새기면 영원히 그대로예요</text>
  </svg>
  </div>
  <p class="cap">돌에 새길 때는 <b>반드시 처음에 한 번은 새겨야</b> 해요 — 아무것도 안 새긴 빈 돌은 허락되지 않아요. 그리고 한 번 새긴 뒤에는, 누구도 지우거나 고칠 수 없어요.</p>
</section>

<div class="dict">
  <div class="term"><b>static · 모두의 칠판</b><span>각자가 아니라 교실 전체가 함께 보는 값</span></div>
  <div class="term"><b>싱글톤 · 교장 선생님</b><span>어디서 불러도 항상 같은 한 분</span></div>
  <div class="term"><b>final · 돌에 새긴 이름</b><span>한 번 새기면 다시는 못 바꾸는 값</span></div>
  <div class="term"><b>잠긴 교장실 문</b><span>외부에서 멋대로 새 교장 선생님을 만들지 못하게 막아두는 것</span></div>
</div>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  static은 "다 같이 보게", 싱글톤은 "딱 한 분만 계시게", final은 "한 번 새기면 못 바꾸게" — 셋 다 값을 각자에게 맡기지 않고 예측 가능하게 통제하는 방법이에요.
</div>
</div>
