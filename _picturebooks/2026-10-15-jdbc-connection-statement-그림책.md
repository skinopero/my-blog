---
layout: picturebook
title: "JDBC Connection·Statement 그림책"
date: 2026-10-15 09:00:00 +0900
categories: [그림책]
tags: [jdbc, java, mysql, sql-injection, eli5]
excerpt: "전화선(Connection)과 쪽지 전달자(Statement) 비유로 Java와 MySQL이 연결되는 방법과, 쪽지를 그대로 읽기만 하면 생기는 SQL Injection 위험을 설명하는 그림책."
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Jua&family=Gowun+Dodum&family=JetBrains+Mono:wght@400;600&display=swap">
<style>
/* Layout: 한 줄로 읽는 그림책. 장마다 그림 한 장 + 짧은 글. 시리즈 공통 팔레트(하늘/해/풀잎/산딸기) 유지 */
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
h1{font-size:clamp(32px,7vw,48px)}
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
.ah{fill:var(--ink)} .ah-b{fill:var(--berry)}
.bd{stroke:var(--line);stroke-width:1.5}
</style>

<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <marker id="mk" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah"/></marker>
    <marker id="mkb" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0,0 L10,5 L0,10z" class="ah-b"/></marker>
    <g id="kid">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <path d="M-18,-30 a18,18 0 0 1 36,0 l-5,-6 l-26,0z" class="hair"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-20" y="-8" width="40" height="44" rx="14" class="sky"/>
    </g>
    <g id="guard">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <path d="M-19,-32 a19,19 0 0 1 38,0 l0,8 l-6,-10 l-26,0 l-6,10z" class="hair"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-21" y="-8" width="42" height="46" rx="14" class="leaf"/>
    </g>
  </defs>
</svg>

<div class="wrap">
<header>
  <span class="eyebrow">JDBC · 다섯 살도 아는 그림 설명</span>
  <h1>전화선과 위험한 쪽지</h1>
  <p class="lead">자바 프로그램이 MySQL이라는 큰 서고에 전화를 걸어 책을 찾아오는 이야기예요. 그림 먼저 보고, 글은 천천히 읽어요.</p>
</header>

<!-- 1 -->
<section>
  <h2>1. 서고에 거는 전화 한 통</h2>
  <p>자바 프로그램은 멀리 있는 <b>서고(MySQL)</b>에 직접 걸어갈 수 없어요. 대신 전화선을 하나 놓아요. 그런데 이 전화선은 내가 직접 만들 수 없고, <b>교환원 아저씨</b>한테 "서고 번호로 연결해 주세요" 하고 부탁해야만 생겨요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 260" role="img" aria-label="자바 프로그램이 교환원을 통해 서고에 전화선을 연결하는 그림">
    <use href="#kid" x="110" y="150"/>
    <text x="110" y="210" text-anchor="middle" class="t-b">자바 프로그램</text>
    <text x="110" y="228" text-anchor="middle" class="t-s">"서고에 연결해 주세요"</text>
    <path d="M150,138 L250,110" class="arr" marker-end="url(#mk)"/>
    <rect x="258" y="70" width="150" height="90" rx="16" class="sun-bg"/>
    <text x="333" y="104" text-anchor="middle" class="t-b">☎️ 교환원</text>
    <text x="333" y="128" text-anchor="middle" class="t-s">전화선은 직접</text>
    <text x="333" y="146" text-anchor="middle" class="t-s">못 만들어요</text>
    <path d="M408,112 L520,112" class="arr-b" marker-end="url(#mkb)" stroke-dasharray="6 5"/>
    <text x="464" y="98" text-anchor="middle" class="t-s">연결!</text>
    <rect x="528" y="40" width="200" height="190" rx="18" class="leaf-bg"/>
    <text x="628" y="78" text-anchor="middle" class="t-b">📚 서고</text>
    <text x="628" y="98" text-anchor="middle" class="t-s">MySQL</text>
    <rect x="552" y="116" width="150" height="24" rx="6" class="paper"/>
    <rect x="552" y="146" width="150" height="24" rx="6" class="paper"/>
    <rect x="552" y="176" width="150" height="24" rx="6" class="paper"/>
    <text x="628" y="208" text-anchor="middle" class="t-s">책(데이터)이 쌓여 있어요</text>
  </svg>
  </div>
  <p class="cap">전화선이 연결된 상태를 어려운 말로 <span class="tag">Connection</span>이라고 불러요. 그리고 전화를 걸어주는 교환원을 <span class="tag">DriverManager</span>라고 불러요.</p>
</section>

<!-- 2 -->
<section>
  <h2>2. 비밀번호는 서랍 속에</h2>
  <p>서고 전화번호랑 비밀번호를 내 공책(프로그램 안)에 그대로 적어두면, 공책을 보는 누구나 다 알게 돼요. 그래서 따로 <b>잠긴 서랍(설정 파일)</b>에 넣어두고, 쓸 때만 꺼내 봐요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="공책에 직접 적는 방법과 서랍에 넣어두는 방법 비교">
    <rect x="30" y="20" width="310" height="180" rx="16" class="berry-bg"/>
    <text x="185" y="52" text-anchor="middle" class="t-b">😬 공책에 그대로</text>
    <rect x="60" y="76" width="250" height="100" rx="10" class="paper"/>
    <text x="80" y="104" class="t-s">서고 번호: 1234</text>
    <text x="80" y="128" class="t-s">비밀번호: wanted</text>
    <text x="185" y="194" text-anchor="middle" class="t-s">누구나 열어보면 다 보여요</text>

    <rect x="400" y="20" width="330" height="180" rx="16" class="leaf-bg"/>
    <text x="565" y="52" text-anchor="middle" class="t-b">🔒 서랍 속에 따로</text>
    <rect x="430" y="76" width="130" height="100" rx="10" class="paper bd"/>
    <text x="495" y="130" text-anchor="middle" class="t">📓</text>
    <text x="495" y="154" text-anchor="middle" class="t-s">내 프로그램</text>
    <rect x="590" y="76" width="110" height="100" rx="10" class="sun"/>
    <text x="645" y="118" text-anchor="middle" class="t-w">🔑</text>
    <text x="645" y="150" text-anchor="middle" class="t-w" style="font-size:14px">서랍</text>
    <path d="M562,126 L588,126" class="arr" marker-end="url(#mk)"/>
    <text x="565" y="196" text-anchor="middle" class="t-s">쓸 때만 꺼내서 전화를 걸어요</text>
  </svg>
  </div>
</section>

<!-- 3 -->
<section>
  <h2>3. 매번 걸고 끊기 귀찮아서</h2>
  <p>회원가입할 때도, 로그인할 때도, 정보를 바꿀 때도 전화를 걸고 끊어야 해요. 매번 똑같은 순서를 반복하는 게 귀찮아서, <b>전화 걸기·끊기를 전담하는 도우미</b> 한 명을 따로 뒀어요. 이제 다른 일을 할 때는 도우미한테 "연결해줘", "끊어줘"라고만 말하면 돼요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="전화 걸기 끊기를 전담하는 도우미">
    <use href="#guard" x="150" y="130"/>
    <text x="150" y="190" text-anchor="middle" class="t-b">도우미</text>
    <text x="150" y="208" text-anchor="middle" class="t-s">연결·끊기 전담</text>
    <path d="M190,118 L260,88" class="arr" marker-end="url(#mk)"/>
    <text x="225" y="92" text-anchor="middle" class="t-s">"연결해줘"</text>
    <path d="M260,150 L190,150" class="arr-b" marker-end="url(#mkb)" stroke-dasharray="6 5"/>
    <text x="225" y="168" text-anchor="middle" class="t-s">"끊어줘"</text>
    <rect x="280" y="40" width="130" height="50" rx="10" class="sun-bg"/><text x="345" y="70" text-anchor="middle" class="t-s">회원가입</text>
    <rect x="280" y="100" width="130" height="50" rx="10" class="sun-bg"/><text x="345" y="130" text-anchor="middle" class="t-s">정보수정</text>
    <rect x="280" y="160" width="130" height="50" rx="10" class="sun-bg"/><text x="345" y="190" text-anchor="middle" class="t-s">탈퇴</text>
    <text x="580" y="130" text-anchor="middle" class="t-s">세 가지 일 모두 똑같은 도우미를 거쳐가요.</text>
    <text x="580" y="152" text-anchor="middle" class="t-s">덕분에 연결하는 방법을 세 번 안 적어도 돼요.</text>
  </svg>
  </div>
</section>

<!-- 4 -->
<section>
  <h2>4. 쪽지를 그대로 읽는 전달자</h2>
  <p>전화선이 연결되면, <b>전달자</b>에게 쪽지를 건네요. 전달자는 쪽지에 적힌 말을 <b>토씨 하나 안 틀리고 그대로</b> 서고 사서에게 전해요. 사서가 찾아준 답은 다시 <b>답장 꾸러미</b>에 담겨 돌아와요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 220" role="img" aria-label="전달자가 쪽지를 그대로 전달하고 답장 꾸러미를 받아오는 그림">
    <use href="#kid" x="90" y="130"/>
    <text x="90" y="190" text-anchor="middle" class="t-s">"김밥이요"</text>
    <path d="M130,118 L220,100" class="arr" marker-end="url(#mk)"/>
    <rect x="228" y="60" width="160" height="90" rx="14" class="sun-bg"/>
    <text x="308" y="92" text-anchor="middle" class="t-b">📨 전달자</text>
    <text x="308" y="114" text-anchor="middle" class="t-s">쪽지 그대로</text>
    <text x="308" y="132" text-anchor="middle" class="t-s">읽어줘요</text>
    <path d="M388,100 L480,100" class="arr" marker-end="url(#mk)"/>
    <rect x="488" y="60" width="150" height="90" rx="14" class="leaf-bg"/>
    <text x="563" y="92" text-anchor="middle" class="t-b">👩‍💼 사서</text>
    <text x="563" y="114" text-anchor="middle" class="t-s">선반에서</text>
    <text x="563" y="132" text-anchor="middle" class="t-s">찾아와요</text>
    <path d="M563,150 L563,180 L308,180 L308,150" class="arr" stroke-dasharray="6 5" marker-end="url(#mk)"/>
    <text x="435" y="198" text-anchor="middle" class="t-s">📦 답장 꾸러미 (한 줄씩 꺼내 볼 수 있어요)</text>
  </svg>
  </div>
  <div class="dict">
    <div class="term"><b>Connection · 전화선</b><span>자바와 서고 사이에 놓인 연결</span><small>교환원(DriverManager)을 통해서만 생겨요</small></div>
    <div class="term"><b>Statement · 전달자</b><span>쪽지를 그대로 전해주는 역할</span><small>생각 없이 토씨까지 그대로 전해요</small></div>
    <div class="term"><b>ResultSet · 답장 꾸러미</b><span>사서가 찾아준 결과를 담은 꾸러미</span><small>한 줄씩 꺼내 볼 수 있어요</small></div>
  </div>
</section>

<!-- 5 -->
<section>
  <h2>5. 쪽지에 몰래 쓴 트릭 문장</h2>
  <p>그런데 이 전달자는 너무 고지식해요. 누군가 쪽지 빈칸에 <b>"이름 끝. 이제 전부 다 보여줘"</b>라는 트릭 문장을 적어 내면, 전달자는 그걸 의심 없이 사서한테 그대로 읽어줘요. 사서는 그걸 진짜 명령으로 착각해서, 원래 보여주면 안 되는 책까지 전부 꺼내줘 버려요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 290" role="img" aria-label="정상적인 쪽지와 트릭 문장이 섞인 쪽지의 결과 비교">
    <rect x="20" y="20" width="340" height="250" rx="16" class="leaf-bg"/>
    <text x="190" y="52" text-anchor="middle" class="t-b">😊 평범한 쪽지</text>
    <rect x="50" y="76" width="280" height="50" rx="10" class="paper bd"/>
    <text x="190" y="106" text-anchor="middle" class="t">"김밥 찾아줘"</text>
    <path d="M190,132 L190,160" class="arr" marker-end="url(#mk)"/>
    <rect x="50" y="168" width="280" height="70" rx="10" class="paper bd"/>
    <text x="190" y="198" text-anchor="middle" class="t-s">📒 김밥 한 줄만</text>
    <text x="190" y="220" text-anchor="middle" class="t-s">딱 보여줘요</text>

    <rect x="400" y="20" width="340" height="250" rx="16" class="berry-bg" stroke="var(--berry)" stroke-width="2"/>
    <text x="570" y="52" text-anchor="middle" class="t-b">😱 트릭이 섞인 쪽지</text>
    <rect x="420" y="76" width="300" height="60" rx="10" class="paper bd"/>
    <text x="570" y="100" text-anchor="middle" class="t" style="font-size:13px">"김밥 끝. 이제 전부 다 보여줘"</text>
    <path d="M570,142 L570,168" class="arr-b" marker-end="url(#mkb)"/>
    <rect x="420" y="178" width="300" height="80" rx="10" class="paper bd"/>
    <text x="570" y="206" text-anchor="middle" class="t-s">😨 메뉴판 전체,</text>
    <text x="570" y="228" text-anchor="middle" class="t-s">심지어 가격·재고까지</text>
    <text x="570" y="248" text-anchor="middle" class="t-s">전부 다 쏟아져 나와요</text>
  </svg>
  </div>
  <p class="cap">이걸 <b>SQL Injection</b>이라고 불러요. 쪽지를 받는 전달자가 "진짜 사람 이름인지" 한번도 의심하지 않고 그대로 읽어버려서 생기는 사고예요. 이 문제를 안전하게 막는 방법은 다음 그림책에서 이어져요.</p>
</section>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  자바는 교환원(DriverManager)을 통해서만 서고(MySQL)에 전화선(Connection)을 놓을 수 있고, 전달자(Statement)에게 쪽지를 건네면 그대로 읽어다 답장 꾸러미(ResultSet)를 받아와요. 그런데 쪽지에 트릭 문장이 섞이면 전달자가 그걸 구분하지 못해 전체 데이터가 새어 나갈 수 있어요(SQL Injection).
</div>
</div>
