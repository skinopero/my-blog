---
layout: picturebook
title: "JDBC로 Java-MySQL 연결하기 그림책"
date: 2026-10-16 09:00:00 +0900
categories: [그림책]
tags: [jdbc, java, mysql, eli5]
excerpt: "두 마을 사이에 다리를 놓는 비유로 드라이버 추가, 접속정보 등록, Test Connection까지 실제 연결 과정을 설명하는 그림책."
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Jua&family=Gowun+Dodum&family=JetBrains+Mono:wght@400;600&display=swap">
<style>
/* Layout: 한 줄로 읽는 그림책. 두 마을 사이에 다리를 놓는 비유로 JDBC 연결 과정을 설명. 시리즈 공통 팔레트 유지 */
:root{
  --bg:#F4F7FB; --paper:#FFFFFF; --ink:#1E2940; --soft:#5B6884; --line:#D5DDEA;
  --sky:#3577BF; --sky-bg:#E3EEFA; --sun:#E9A21F; --sun-bg:#FDF1D8;
  --leaf:#2F9C6E; --leaf-bg:#DDF3E9; --berry:#C9486C; --berry-bg:#FBE3EA;
  --skin:#F6D2B4; --hair:#4A3426;
  --f-display:"Jua","Gowun Dodum",system-ui,sans-serif;
  --f-body:"Gowun Dodum","Apple SD Gothic Neo","Malgun Gothic",system-ui,sans-serif;
  --f-code:"JetBrains Mono",ui-monospace,Menlo,monospace;
  color-scheme:light;
}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]){
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#141B29; --paper:#1D2638; --ink:#E9EEF7; --soft:#A3AEC4; --line:#33405A;
  --sky:#7DB2EC; --sky-bg:#1F3352; --sun:#F3C15A; --sun-bg:#3A2F17; --leaf:#5FCB9A; --leaf-bg:#183A2D;
  --berry:#EE84A2; --berry-bg:#3D1F2A; --skin:#E8BE9C; --hair:#2A1C13; color-scheme:dark}
body{margin:0;background:var(--bg);color:var(--ink);font-family:var(--f-body);font-size:17px;line-height:1.7}
.wrap{max-width:820px;margin:0 auto;padding-block:40px 64px;padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));display:grid;gap:44px}
header{display:grid;gap:8px}
.eyebrow{font-size:13px;letter-spacing:.08em;color:var(--sky);font-weight:700}
h1,h2{font-family:var(--f-display);font-weight:400;text-wrap:balance;margin:0;line-height:1.25}
h1{font-size:clamp(30px,6.6vw,46px)}
h2{font-size:clamp(22px,4.2vw,28px)}
p{margin:0;max-width:62ch}
.lead{font-size:19px;color:var(--soft)}
section{display:grid;gap:14px}
.pic{background:var(--paper);border:1px solid var(--line);border-radius:18px;padding:18px;overflow-x:auto}
.pic svg{display:block;width:100%;height:auto;min-width:560px}
.cap{font-size:14px;color:var(--soft)}
.tag{font-family:var(--f-code);font-size:.82em;background:var(--sky-bg);color:var(--sky);padding:1px 7px;border-radius:6px}
.dict{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.term{background:var(--paper);border:1px solid var(--line);border-radius:14px;padding:14px 16px;display:grid;gap:4px;min-width:0}
.term b{font-family:var(--f-display);font-weight:400;font-size:20px}
.term small{color:var(--soft);font-size:13px}
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
    <g id="kid">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <path d="M-18,-30 a18,18 0 0 1 36,0 l-5,-6 l-26,0z" class="hair"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-20" y="-8" width="40" height="44" rx="14" class="sky"/>
    </g>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">JDBC · 다섯 살도 아는 그림 설명</span>
  <h1>두 마을 사이에 다리 놓기</h1>
  <p class="lead">자바 마을과 MySQL 마을은 서로 다른 말을 써요. 둘을 잇는 다리를 놓는 과정을 그림으로 따라가요.</p>
</header>

<!-- 1 -->
<section>
  <h2>1. 서로 다른 말을 쓰는 두 마을</h2>
  <p>자바 마을과 MySQL 마을은 강을 사이에 두고 떨어져 있어요. 서로 하는 말이 달라서, 그냥은 이야기를 주고받을 수 없어요. 그래서 둘 사이에 <b>통역사가 서 있는 다리(JDBC)</b>를 놓아요. 자바가 "회원 이름으로 찾아줘"라고 말하면, 통역사가 MySQL이 알아듣는 말로 바꿔서 전해줘요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 240" role="img" aria-label="자바 마을과 MySQL 마을 사이에 통역사가 서 있는 다리">
    <rect x="20" y="60" width="200" height="140" rx="16" class="sky-bg"/>
    <text x="120" y="92" text-anchor="middle" class="t-b">자바 마을</text>
    <use href="#kid" x="120" y="160"/>
    <text x="120" y="212" text-anchor="middle" class="t-s">"회원 이름으로 찾아줘"</text>
    <rect x="280" y="100" width="200" height="60" rx="30" class="sun-bg"/>
    <text x="380" y="126" text-anchor="middle" class="t-b">🌉 다리</text>
    <text x="380" y="148" text-anchor="middle" class="t-s">통역사가 서 있어요</text>
    <path d="M220,130 L278,130" class="arr" marker-end="url(#mk)"/>
    <path d="M482,130 L538,130" class="arr" marker-end="url(#mk)"/>
    <rect x="540" y="60" width="200" height="140" rx="16" class="leaf-bg"/>
    <text x="640" y="92" text-anchor="middle" class="t-b">MySQL 마을</text>
    <text x="640" y="150" text-anchor="middle" class="t" style="font-size:28px">📚</text>
    <text x="640" y="180" text-anchor="middle" class="t-s">자기들 말로 책을 찾아줘요</text>
  </svg>
  </div>
  <p class="cap">통역사가 할 수 있는 일의 목록(약속)은 정해져 있지만, 실제로 통역하는 일은 <b>드라이버</b>라는 전문 통역사가 맡아요. MySQL용 통역사를 Oracle용으로 바꿔도 자바 마을 사람들의 말투는 그대로예요.</p>
</section>

<!-- 2 -->
<section>
  <h2>2. 다리를 지을 재료 가져오기</h2>
  <p>다리를 놓으려면 먼저 재료가 있어야 해요. 재료 창고(인터넷 저장소)에서 "MySQL용 통역사 재료"를 찾아서, 내 공사 목록에 추가해요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 200" role="img" aria-label="재료 창고에서 통역사 재료를 가져와 공사 목록에 추가하는 그림">
    <rect x="30" y="30" width="220" height="140" rx="16" class="sun-bg"/>
    <text x="140" y="62" text-anchor="middle" class="t-b">🏬 재료 창고</text>
    <rect x="60" y="86" width="160" height="50" rx="10" class="paper bd"/>
    <text x="140" y="116" text-anchor="middle" class="t-s">MySQL 통역사 재료</text>
    <path d="M250,110 L340,110" class="arr" marker-end="url(#mk)"/>
    <text x="295" y="98" text-anchor="middle" class="t-s">가져오기</text>
    <rect x="350" y="30" width="380" height="140" rx="16" class="leaf-bg"/>
    <text x="540" y="62" text-anchor="middle" class="t-b">📋 내 공사 목록</text>
    <rect x="380" y="86" width="320" height="30" rx="7" class="paper bd"/><text x="400" y="106" class="t-s">· 땅 고르기</text>
    <rect x="380" y="122" width="320" height="30" rx="7" class="paper bd" stroke="var(--leaf)" stroke-width="2"/><text x="400" y="142" class="t-s">· MySQL 통역사 재료 ✓ 추가됨</text>
  </svg>
  </div>
</section>

<!-- 3 -->
<section>
  <h2>3. 두 마을이 같은 글자를 쓰게 맞추기</h2>
  <p>다리를 놓기 전에, 양쪽 마을이 <b>같은 글자(한글 포함)</b>를 주고받을 수 있게 미리 맞춰둬요. 이 약속을 안 맞추면 다리를 건너는 동안 글자가 깨져서 이상한 모양으로 도착해요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 180" role="img" aria-label="글자 약속을 맞추지 않았을 때와 맞췄을 때 비교">
    <rect x="30" y="20" width="330" height="140" rx="16" class="berry-bg"/>
    <text x="195" y="52" text-anchor="middle" class="t-b">😖 약속 안 맞추면</text>
    <rect x="60" y="76" width="270" height="50" rx="10" class="paper bd"/>
    <text x="195" y="106" text-anchor="middle" class="t">"김밥" → "??밥"</text>
    <rect x="400" y="20" width="330" height="140" rx="16" class="leaf-bg"/>
    <text x="565" y="52" text-anchor="middle" class="t-b">🙂 약속(UTF-8) 맞추면</text>
    <rect x="430" y="76" width="270" height="50" rx="10" class="paper bd"/>
    <text x="565" y="106" text-anchor="middle" class="t">"김밥" → "김밥"</text>
  </svg>
  </div>
</section>

<!-- 4 -->
<section>
  <h2>4. 다리 접수처에 등록하기</h2>
  <p>다리를 실제로 쓰려면 접수처에 <b>어느 마을의 어느 창고로 갈지(주소), 몇 번 문으로 들어갈지(포트), 누구 이름으로(계정), 비밀 암호는 무엇인지(비밀번호)</b>를 적어서 등록해요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 200" role="img" aria-label="다리 접수처에 주소, 문 번호, 이름, 비밀번호를 적어 등록하는 그림">
    <rect x="40" y="20" width="680" height="160" rx="16" class="paper" stroke="var(--line)" stroke-width="2"/>
    <text x="70" y="52" class="t-b">🖊️ 접수처 등록 카드</text>
    <rect x="70" y="68" width="300" height="34" rx="8" class="sky-bg"/><text x="90" y="90" class="t-s">주소: menudb 마을</text>
    <rect x="70" y="108" width="300" height="34" rx="8" class="sky-bg"/><text x="90" y="130" class="t-s">문 번호: 정해진 포트</text>
    <rect x="390" y="68" width="320" height="34" rx="8" class="sun-bg"/><text x="410" y="90" class="t-s">이름: wanted</text>
    <rect x="390" y="108" width="320" height="34" rx="8" class="sun-bg"/><text x="410" y="130" class="t-s">비밀 암호: ********</text>
    <text x="70" y="164" class="t-s">이 카드가 등록돼야 다리 문지기가 통과시켜 줘요.</text>
  </svg>
  </div>
</section>

<!-- 5 -->
<section>
  <h2>5. 시험 삼아 먼저 건너보기</h2>
  <p>등록만 하고 끝내지 않고, <b>진짜 건널 수 있는지 미리 시험</b>해봐요. 실패하면 "마을이 문을 열어뒀는지", "이름·암호가 맞는지", "문 번호가 맞는지"부터 하나씩 확인하고 다시 시도해요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="시험 삼아 건너보고 실패하면 확인 후 다시 시도하는 순환 그림">
    <use href="#kid" x="110" y="120"/>
    <text x="110" y="180" text-anchor="middle" class="t-s">시험 삼아 건너기</text>
    <path d="M150,110 Q260,60 370,110" class="arr" marker-end="url(#mk)"/>
    <rect x="300" y="100" width="160" height="60" rx="14" class="sun-bg"/>
    <text x="380" y="134" text-anchor="middle" class="t-b" style="font-size:15px">🚧 다리 입구</text>
    <path d="M460,140 Q520,180 420,190" class="arr-b" marker-end="url(#mkb)" stroke-dasharray="6 5"/>
    <text x="470" y="200" text-anchor="middle" class="t-s">실패! 다시 확인</text>
    <rect x="280" y="40" width="240" height="40" rx="10" class="berry-bg"/>
    <text x="400" y="66" text-anchor="middle" class="t-s">문 열림? 이름·암호 맞음? 포트 맞음?</text>
    <path d="M460,120 L560,120" class="arr-l" marker-end="url(#mkl)" stroke-dasharray="4 4"/>
    <rect x="570" y="90" width="160" height="60" rx="14" class="leaf-bg"/>
    <text x="650" y="124" text-anchor="middle" class="t-b" style="font-size:15px">✅ 성공!</text>
  </svg>
  </div>
</section>

<!-- 6 -->
<section>
  <h2>6. 다리를 건너 짐 날라보기</h2>
  <p>시험 통과 후, 진짜로 짐(데이터) 하나를 다리 건너로 옮겨봐요. 짐이 무사히 도착해서 확인되면, 자바 마을 쪽 공사는 아직 시작하지 않았지만 다리 자체는 튼튼하다는 게 증명된 거예요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 170" role="img" aria-label="시험 삼아 짐을 다리 건너로 옮겨 확인하는 그림">
    <rect x="30" y="50" width="160" height="70" rx="14" class="sky-bg"/><text x="110" y="90" text-anchor="middle" class="t-s">📦 짐 하나</text>
    <path d="M190,85 L560,85" class="arr" marker-end="url(#mk)" stroke-dasharray="8 6"/>
    <text x="375" y="72" text-anchor="middle" class="t-s">다리를 건너요</text>
    <rect x="570" y="50" width="160" height="70" rx="14" class="leaf-bg"/><text x="650" y="90" text-anchor="middle" class="t-s">✅ 무사 도착</text>
  </svg>
  </div>
</section>

<div class="dict">
  <div class="term"><b>JDBC · 다리</b><span>자바 마을과 MySQL 마을을 잇는 통역 약속</span><small>java.sql 패키지</small></div>
  <div class="term"><b>드라이버 · 전문 통역사</b><span>실제로 번역을 담당하는 일꾼</span><small>DBMS가 바뀌면 통역사만 바뀌어요</small></div>
  <div class="term"><b>Test Connection · 시험 삼아 건너기</b><span>등록만 하고 끝내지 않고 실제로 건널 수 있는지 확인</span><small>실패하면 등록 카드부터 재확인</small></div>
</div>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  서로 다른 말을 쓰는 자바 마을과 MySQL 마을 사이에 통역사(JDBC)가 선 다리를 놓고, 재료(드라이버)를 가져와 접수처에 주소·이름·암호를 등록한 다음, 시험 삼아 건너보고(Test Connection) 짐을 옮겨 확인하는 것까지가 오늘 한 일이에요.
</div>
</div>
