---
layout: picturebook
title: "PreparedStatement·DAO 그림책"
date: 2026-10-19 09:10:00 +0900
categories: [그림책]
tags: [jdbc, java, dao, sql-injection, eli5]
excerpt: "빈 종이(위험)와 양식지(안전) 비유로 SQL Injection 방지 원리, 조회/변경 양식지 구분, 한 가지 일만 전담하는 DAO를 설명하는 그림책."
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
.dict{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.term{background:var(--paper);border:1px solid var(--line);border-radius:14px;padding:14px 16px;display:grid;gap:4px;min-width:0}
.term b{font-family:var(--f-display);font-weight:400;font-size:20px}
.term small{font-family:var(--f-body);font-size:13px;color:var(--soft)}
.sum{background:var(--sun-bg);border-radius:18px;padding:20px 22px;font-size:18px}
.t{fill:var(--ink);font-family:var(--f-body);font-size:15px}
.t-b{fill:var(--ink);font-family:var(--f-display);font-size:18px}
.t-s{fill:var(--soft);font-family:var(--f-body);font-size:13px}
.t-w{fill:var(--paper);font-family:var(--f-display);font-size:16px}
.skin{fill:var(--skin)} .hair{fill:var(--hair)} .eye{fill:var(--ink)}
.sky{fill:var(--sky)} .sky-bg{fill:var(--sky-bg)} .sun{fill:var(--sun)} .sun-bg{fill:var(--sun-bg)}
.leaf{fill:var(--leaf)} .leaf-bg{fill:var(--leaf-bg)} .berry{fill:var(--berry)} .berry-bg{fill:var(--berry-bg)}
.paper{fill:var(--paper)} .bg{fill:var(--bg)}
.arr{stroke:var(--ink);fill:none;stroke-width:2.2;stroke-linecap:round}
.arr-b{stroke:var(--berry);fill:none;stroke-width:2.2;stroke-linecap:round}
.arr-l{stroke:var(--leaf);fill:none;stroke-width:2.2;stroke-linecap:round}
.ah{fill:var(--ink)} .ah-b{fill:var(--berry)} .ah-l{fill:var(--leaf)}
.bd{stroke:var(--line);stroke-width:1.5}
</style>

<svg width="0" height="0" style="position:absolute" aria-hidden="true">
<defs>
  <marker id="mk" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah"/></marker>
  <marker id="mkb" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah-b"/></marker>
  <marker id="mkl" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah-l"/></marker>
  <g id="guest"><circle cx="0" cy="-26" r="16" class="skin"/><path d="M-16,-28 a16,16 0 0 1 32,0 l-4,-6 l-24,0z" class="hair"/><circle cx="-5" cy="-26" r="1.8" class="eye"/><circle cx="5" cy="-26" r="1.8" class="eye"/><rect x="-18" y="-8" width="36" height="40" rx="12" class="sun"/></g>
  <g id="keeper"><circle cx="0" cy="-26" r="16" class="skin"/><path d="M-16,-28 a16,16 0 0 1 32,0 l-4,-6 l-24,0z" class="hair"/><circle cx="-5" cy="-26" r="1.8" class="eye"/><circle cx="5" cy="-26" r="1.8" class="eye"/><rect x="-18" y="-8" width="36" height="40" rx="12" class="sky-bg" stroke="var(--sky)" stroke-width="2.5"/></g>
</defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">PreparedStatement · DAO · 다섯 살도 아는 그림 설명</span>
  <h1>빈 종이 대신 양식지</h1>
  <p class="lead">어제는 손님이 빈 종이에 직접 글씨를 썼다면, 오늘은 미리 찍어둔 양식지를 써요. 코드 없이 그림으로만 따라가요.</p>
</header>

<section>
  <h2>1. 어제 문제 — 빈 종이는 위험해요</h2>
  <p>냉장고 담당이 재료를 찾을 때, 손님이 부른 이름을 그대로 빈 종이에 받아 적었어요. 그런데 손님이 이름 대신 <b>이상한 명령어</b>를 적으면, 냉장고 담당이 그걸 진짜 명령으로 착각할 수 있었어요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="빈 종이에 이상한 명령어를 적어 위험해지는 상황">
    <use href="#guest" x="90" y="140"/><text x="90" y="195" text-anchor="middle" class="t-s">손님</text>
    <rect x="170" y="90" width="220" height="90" rx="12" class="berry-bg" stroke="var(--berry)" stroke-width="2"/>
    <text x="280" y="118" text-anchor="middle" class="t-b">📄 빈 종이</text>
    <text x="280" y="146" text-anchor="middle" class="t-s">"어떤 글자든 다 받아 적을게요"</text>
    <text x="280" y="168" text-anchor="middle" class="t-s" style="fill:var(--berry)">😨 이름인 척 명령어가 섞여도 몰라요</text>
    <path d="M120,128 L160,128" class="arr" marker-end="url(#mk)"/>
    <path d="M390,128 L440,128" class="arr-b" marker-end="url(#mkb)"/>
    <use href="#keeper" x="480" y="140"/><text x="480" y="195" text-anchor="middle" class="t-s">냉장고 담당</text>
    <text x="620" y="128" class="t-s" style="fill:var(--berry)">"어? 이것도 명령인가?"</text>
  </svg>
  </div>
  <p class="cap">이걸 'SQL Injection'이라고 불러요. 빈 종이에 뭐든 적을 수 있다는 게 문제의 시작이었어요.</p>
</section>

<section>
  <h2>2. 오늘 해결 — 양식지를 먼저 찍어둬요</h2>
  <p>오늘은 반대로 해요. <b>양식지(틀)</b>를 먼저 다 찍어내고, 그 다음에 정해진 빈칸에만 이름을 끼워 넣어요. 양식지 자체는 이미 완성돼 있어서, 빈칸에 뭘 적어도 양식지의 다른 부분은 절대 안 바뀌어요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 240" role="img" aria-label="양식지를 먼저 찍고 빈칸만 채우는 안전한 방식">
    <rect x="40" y="30" width="300" height="180" rx="14" class="leaf-bg" stroke="var(--leaf)" stroke-width="2.5"/>
    <text x="190" y="62" text-anchor="middle" class="t-b">📋 양식지 (이미 완성)</text>
    <text x="70" y="96" class="t">메뉴 이름:</text>
    <rect x="170" y="80" width="130" height="26" rx="6" class="paper bd" stroke-dasharray="4 3"/>
    <text x="235" y="98" text-anchor="middle" class="t-s">빈칸</text>
    <text x="70" y="140" class="t">가져올 항목:</text>
    <text x="70" y="166" class="t-s">가격 · 번호 · 품절여부</text>
    <text x="190" y="196" text-anchor="middle" class="t-s">→ 이 틀 자체는 절대 안 바뀜</text>
    <path d="M360,120 L440,120" class="arr-l" marker-end="url(#mkl)"/>
    <use href="#guest" x="490" y="140"/>
    <text x="490" y="195" text-anchor="middle" class="t-s">이름만 알려줘요</text>
    <path d="M480,128 L420,128" class="arr" marker-end="url(#mk)" stroke-dasharray="5 4"/>
    <text x="600" y="60" text-anchor="middle" class="t-b" style="fill:var(--leaf)">😊 빈칸에 뭘 적어도</text>
    <text x="600" y="82" text-anchor="middle" class="t-b" style="fill:var(--leaf)">양식지는 안 바뀌어요</text>
  </svg>
  </div>
  <p class="cap">이게 <b>PreparedStatement</b>예요. 손님이 아무리 이상한 글자를 넣어도, 그건 그냥 "빈칸에 적힌 글자"일 뿐이라 양식지의 규칙을 바꿀 수 없어요.</p>
</section>

<section>
  <h2>3. 양식지는 한 번만 찍고 계속 재사용해요</h2>
  <p>양식지를 매번 새로 인쇄하면 시간이 걸려요. 그래서 <b>딱 한 번만 인쇄</b>해두고, 다음 손님부터는 같은 틀에 빈칸만 새로 채워 넣어요. 그만큼 더 빨라요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 200" role="img" aria-label="양식지를 한 번만 인쇄하고 재사용하는 그림">
    <rect x="40" y="40" width="140" height="120" rx="12" class="sun-bg"/>
    <text x="110" y="80" text-anchor="middle" class="t-b">🖨️</text>
    <text x="110" y="110" text-anchor="middle" class="t-s">첫 손님 때</text>
    <text x="110" y="132" text-anchor="middle" class="t-s">딱 1번 인쇄</text>
    <path d="M190,100 L260,100" class="arr" marker-end="url(#mk)"/>
    <rect x="270" y="50" width="140" height="100" rx="10" class="paper bd"/>
    <text x="340" y="80" text-anchor="middle" class="t-s">빈칸만 교체</text>
    <text x="340" y="104" text-anchor="middle" class="t-s">2번째 손님</text>
    <path d="M410,100 L480,100" class="arr" marker-end="url(#mk)"/>
    <rect x="490" y="50" width="140" height="100" rx="10" class="paper bd"/>
    <text x="560" y="80" text-anchor="middle" class="t-s">빈칸만 교체</text>
    <text x="560" y="104" text-anchor="middle" class="t-s">3번째 손님</text>
    <path d="M630,100 L700,100" class="arr" marker-end="url(#mk)"/>
    <text x="700" y="100" class="t-s">…</text>
  </svg>
  </div>
</section>

<section>
  <h2>4. 조회하는 양식지 vs 바꾸는 양식지</h2>
  <p>양식지도 두 종류가 있어요. <b>보여만 달라는 양식지</b>는 결과를 받아오고, <b>추가·수정·삭제를 시키는 양식지</b>는 "몇 개가 바뀌었는지" 개수만 알려줘요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 200" role="img" aria-label="조회용 양식지와 변경용 양식지 비교">
    <rect x="30" y="20" width="340" height="160" rx="14" class="sky-bg"/>
    <text x="200" y="54" text-anchor="middle" class="t-b">🔍 조회용 양식지</text>
    <text x="200" y="86" text-anchor="middle" class="t-s">"메뉴 목록 보여줘"</text>
    <text x="200" y="120" text-anchor="middle" class="t-s">→ 결과 목록을 받아와요</text>
    <rect x="390" y="20" width="340" height="160" rx="14" class="berry-bg"/>
    <text x="560" y="54" text-anchor="middle" class="t-b">✏️ 변경용 양식지</text>
    <text x="560" y="86" text-anchor="middle" class="t-s">"가격 바꿔줘 / 삭제해줘"</text>
    <text x="560" y="120" text-anchor="middle" class="t-s">→ 몇 개가 바뀌었는지 숫자만 알려줘요</text>
  </svg>
  </div>
</section>

<section>
  <h2>5. 냉장고 담당은 한 가지 일만 전담해요</h2>
  <p>냉장고 담당 중에서도 <b>메뉴 담당</b>은 메뉴에 관한 일만, 다른 담당은 다른 일만 맡아요. 이렇게 한 가지 일만 전담하는 담당자를 <b>DAO</b>라고 불러요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="메뉴 담당 DAO가 양식지를 써서 냉장고에서 재료를 꺼내는 그림">
    <use href="#keeper" x="100" y="140"/>
    <text x="100" y="195" text-anchor="middle" class="t-b">메뉴 담당</text>
    <text x="100" y="213" text-anchor="middle" class="t-s">(MenuDAO)</text>
    <rect x="190" y="110" width="110" height="60" rx="10" class="leaf-bg" stroke="var(--leaf)" stroke-width="2"/>
    <text x="245" y="146" text-anchor="middle" class="t-s">📋 메뉴용</text>
    <text x="245" y="164" text-anchor="middle" class="t-s">양식지</text>
    <path d="M130,128 L186,128" class="arr" marker-end="url(#mk)"/>
    <path d="M304,128 L360,128" class="arr" marker-end="url(#mk)"/>
    <rect x="370" y="90" width="120" height="100" rx="14" class="sky-bg" stroke="var(--sky)" stroke-width="2.5"/>
    <text x="430" y="140" text-anchor="middle" class="t-b" style="font-size:24px">🧊</text>
    <text x="430" y="168" text-anchor="middle" class="t-s">냉장고(DB)</text>
    <text x="620" y="110" class="t-s">다른 담당(예: 회원 담당)은</text>
    <text x="620" y="130" class="t-s">메뉴에는 손 안 대요</text>
  </svg>
  </div>
  <p class="cap">메뉴 담당은 가격·재고 판단은 하지 않아요. 그건 주방장(Service) 몫이고, 메뉴 담당은 그저 "시킨 대로 양식지를 들고 냉장고를 여닫는" 일만 해요.</p>
</section>

<div class="dict">
  <div class="term"><b>양식지</b><span>이미 완성된, 안전하게 재사용되는 틀</span><small>PreparedStatement</small></div>
  <div class="term"><b>빈칸</b><span>양식지 안에서 값만 바꿔 끼우는 자리</span><small>Placeholder(?)</small></div>
  <div class="term"><b>조회용 양식지</b><span>결과 목록을 받아오는 용도</span><small>executeQuery()</small></div>
  <div class="term"><b>변경용 양식지</b><span>추가·수정·삭제하고 변경 개수만 받는 용도</span><small>executeUpdate()</small></div>
  <div class="term"><b>메뉴 담당</b><span>한 가지 주제만 전담하는 냉장고 담당</span><small>DAO</small></div>
</div>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  빈 종이에 손님 말을 그대로 받아 적으면 위험하지만, 양식지를 먼저 찍어두고 빈칸만 채우면 안전해요. 그리고 냉장고 담당은 한 가지 일만 전담해서, 메뉴 담당은 메뉴만, 다른 담당은 다른 것만 맡아요.
</div>
</div>
