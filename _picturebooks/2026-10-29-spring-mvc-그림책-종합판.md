---
layout: picturebook
title: "Spring MVC 그림책 (종합판)"
date: 2026-10-29 09:10:00 +0900
categories: [그림책]
tags: [spring, spring-mvc, eli5]
excerpt: "위 두 동화책의 내용을 한 장짜리 큰 그림(안내판 → 웨이터 → 그릇 찾기 → 보건실·스톱워치)으로 압축한 종합판 그림책."
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
.t-w{fill:var(--paper);font-family:var(--f-display);font-size:17px}
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
    <g id="chef">
      <circle cx="0" cy="-28" r="18" class="skin"/>
      <rect x="-16" y="-58" width="32" height="14" rx="3" class="paper bd"/>
      <ellipse cx="0" cy="-62" rx="20" ry="10" class="paper bd"/>
      <circle cx="-6" cy="-28" r="2" class="eye"/><circle cx="6" cy="-28" r="2" class="eye"/>
      <rect x="-22" y="-8" width="44" height="46" rx="14" class="leaf"/>
    </g>
  </defs>
</svg>
<div class="wrap">
<header>
  <span class="eyebrow">SPRING WEB MVC · 다섯 살도 아는 그림 설명</span>
  <h1>식당에 손님이 왔어요</h1>
  <p class="lead">chap01~05 수업 코드를 식당 그림으로 풀었어요. 손님이 주문하고, 웨이터가 받고, 그릇에 담아 내주는 이야기예요.</p>
</header>

<!-- 0 큰 그림 -->
<section>
  <h2>먼저 큰 그림</h2>
  <p>손님(브라우저)이 주소를 들고 와요. <b>안내판</b>이 어느 웨이터한테 갈지 알려주고, <b>웨이터(Controller)</b>가 주문을 받아서 <b>쟁반(Model)</b>에 음식을 올리고, 알맞은 <b>그릇(View)</b>에 담아서 내줘요. 문 앞에는 <b>스톱워치 아저씨(Interceptor)</b>가 있고, 누가 넘어지면 <b>보건 선생님(ExceptionHandler)</b>이 와요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 290" role="img" aria-label="Spring MVC 전체 흐름">
    <use href="#kid" x="60" y="140"/>
    <text x="60" y="200" text-anchor="middle" class="t-b">손님</text>
    <text x="60" y="218" text-anchor="middle" class="t-cs">/menu/regist</text>
    <path d="M90,128 L130,128" class="arr" marker-end="url(#mk)"/>
    <rect x="135" y="80" width="90" height="96" rx="14" class="berry-bg"/>
    <text x="180" y="112" text-anchor="middle" class="t-b">⏱ 문지기</text>
    <text x="180" y="132" text-anchor="middle" class="t-cs">Interceptor</text>
    <text x="180" y="152" text-anchor="middle" class="t-s">chap05</text>
    <path d="M228,128 L262,128" class="arr" marker-end="url(#mk)"/>
    <rect x="266" y="80" width="100" height="96" rx="14" class="sun-bg"/>
    <text x="316" y="112" text-anchor="middle" class="t-b">🪧 안내판</text>
    <text x="316" y="132" text-anchor="middle" class="t-cs">@GetMapping</text>
    <text x="316" y="152" text-anchor="middle" class="t-s">chap01</text>
    <path d="M369,128 L403,128" class="arr" marker-end="url(#mk)"/>
    <rect x="407" y="70" width="140" height="116" rx="14" class="leaf-bg"/>
    <text x="477" y="102" text-anchor="middle" class="t-b">🤵 웨이터</text>
    <text x="477" y="122" text-anchor="middle" class="t-cs">@Controller</text>
    <text x="477" y="142" text-anchor="middle" class="t-s">주문서 받기 · chap02</text>
    <text x="477" y="166" text-anchor="middle" class="t-s">🍽 쟁반 = Model</text>
    <path d="M550,128 L584,128" class="arr" marker-end="url(#mk)"/>
    <rect x="588" y="80" width="150" height="96" rx="14" class="sky-bg"/>
    <text x="663" y="112" text-anchor="middle" class="t-b">🥣 그릇 찾기</text>
    <text x="663" y="132" text-anchor="middle" class="t-cs">ViewResolver</text>
    <text x="663" y="152" text-anchor="middle" class="t-s">chap03</text>
    <path d="M663,180 C663,250 100,260 64,186" class="arr" stroke-dasharray="6 5" marker-end="url(#mk)"/>
    <text x="380" y="262" text-anchor="middle" class="t-s">완성된 화면(HTML)이 손님에게 돌아가요</text>
    <rect x="420" y="10" width="200" height="34" rx="10" class="berry"/>
    <text x="520" y="32" text-anchor="middle" class="t-w" style="font-size:15px">🩹 넘어지면 보건실 · chap04</text>
    <path d="M477,46 L477,66" class="arr-b" marker-end="url(#mkb)"/>
  </svg>
  </div>
  <p class="cap">예전에 배운 서블릿이 하던 일을 Spring이 나눠서 맡아요. 요청이 오면 서블릿이 바로 <code>@Controller</code>에게 넘겨줘요.</p>
</section>

<!-- 1 -->
<section>
  <h2>1. 안내판 보고 찾아가기 (요청 매핑)</h2>
  <p>손님이 들고 온 <b>주소</b>를 보고 어느 웨이터의 어떤 일로 보낼지 정하는 게 <b>요청 매핑</b>이에요. 같은 주소라도 손님이 <b>구경하러 왔는지(GET)</b>, <b>주문서를 내러 왔는지(POST)</b>에 따라 다른 곳으로 보내요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 300" role="img" aria-label="요청 매핑 안내판">
    <rect x="30" y="30" width="330" height="240" rx="16" class="sun-bg"/>
    <text x="195" y="62" text-anchor="middle" class="t-b">같은 문, 다른 손님</text>
    <text x="195" y="84" text-anchor="middle" class="t-cs">/menu/regist</text>
    <rect x="50" y="104" width="140" height="70" rx="12" class="paper bd"/>
    <text x="120" y="132" text-anchor="middle" class="t">👀 구경할래요</text>
    <text x="120" y="156" text-anchor="middle" class="t-c">GET</text>
    <rect x="200" y="104" width="140" height="70" rx="12" class="paper bd"/>
    <text x="270" y="132" text-anchor="middle" class="t">📝 주문할래요</text>
    <text x="270" y="156" text-anchor="middle" class="t-c">POST</text>
    <text x="120" y="204" text-anchor="middle" class="t-cs">@GetMapping</text>
    <text x="270" y="204" text-anchor="middle" class="t-cs">@PostMapping</text>
    <text x="195" y="244" text-anchor="middle" class="t-s">@RequestMapping(method=…)로 써도 같아요</text>
    <rect x="390" y="30" width="340" height="240" rx="16" class="leaf-bg"/>
    <text x="560" y="62" text-anchor="middle" class="t-b">층 안내 + 방 번호</text>
    <rect x="410" y="80" width="300" height="40" rx="10" class="leaf"/>
    <text x="560" y="105" text-anchor="middle" class="t-w" style="font-size:15px">🏢 2층 주문 코너 · /order/*</text>
    <text x="560" y="138" text-anchor="middle" class="t-s">클래스 위에 붙이면 모든 방의 앞주소가 돼요</text>
    <rect x="410" y="152" width="140" height="40" rx="10" class="paper bd"/>
    <text x="480" y="177" text-anchor="middle" class="t-c">/regist</text>
    <rect x="570" y="152" width="140" height="40" rx="10" class="paper bd"/>
    <text x="640" y="177" text-anchor="middle" class="t-c">/detail/{번호}</text>
    <text x="560" y="222" text-anchor="middle" class="t-c">/order/detail/<tspan style="fill:var(--berry);font-weight:600">3</tspan></text>
    <text x="560" y="246" text-anchor="middle" class="t-s">주소 안의 3을 꺼내 쓰기 = @PathVariable</text>
  </svg>
  </div>
  <p class="cap"><code>@RequestMapping(value = {"/", "/main"})</code>처럼 문을 여러 개 달아도 같은 방으로 와요.</p>
</section>

<!-- 2 -->
<section>
  <h2>2. 주문서 받는 세 가지 방법 (핸들러 메서드)</h2>
  <p>웨이터가 손님 주문서에서 내용을 꺼내는 방법이 세 가지 있어요. 손님이 쓴 칸 이름(<code>name="price"</code>)과 웨이터가 찾는 이름이 같아야 해요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 330" role="img" aria-label="WebRequest, RequestParam, ModelAttribute 비교">
    <rect x="16" y="20" width="236" height="290" rx="16" class="sun-bg"/>
    <text x="134" y="52" text-anchor="middle" class="t-b">WebRequest</text>
    <text x="134" y="72" text-anchor="middle" class="t-s">주문서를 통째로 받아서</text>
    <text x="134" y="90" text-anchor="middle" class="t-s">하나씩 직접 꺼내기</text>
    <rect x="64" y="110" width="140" height="100" rx="8" class="paper bd"/>
    <text x="80" y="136" class="t-cs">name : 김밥</text>
    <text x="80" y="160" class="t-cs">price : 3000</text>
    <text x="80" y="184" class="t-cs">category : 1</text>
    <text x="134" y="240" text-anchor="middle" class="t-s">😮‍💨 글자라서 숫자로</text>
    <text x="134" y="258" text-anchor="middle" class="t-s">직접 바꿔야 해요</text>
    <text x="134" y="292" text-anchor="middle" class="t-cs">getParameter("name")</text>
    <rect x="262" y="20" width="236" height="290" rx="16" class="sky-bg"/>
    <text x="380" y="52" text-anchor="middle" class="t-b">@RequestParam</text>
    <text x="380" y="72" text-anchor="middle" class="t-s">이름 불러서</text>
    <text x="380" y="90" text-anchor="middle" class="t-s">하나씩 쏙 받기</text>
    <rect x="300" y="116" width="160" height="34" rx="17" class="sky"/>
    <text x="380" y="138" text-anchor="middle" class="t-w" style="font-size:15px">"modifyPrice 나와!"</text>
    <rect x="300" y="164" width="160" height="34" rx="17" class="paper bd"/>
    <text x="380" y="186" text-anchor="middle" class="t-c">int modifyPrice</text>
    <text x="380" y="240" text-anchor="middle" class="t-s">⚠️ 이름이 틀리면</text>
    <text x="380" y="258" text-anchor="middle" class="t-s">400 Bad Request</text>
    <text x="380" y="292" text-anchor="middle" class="t-cs">@RequestParam int price</text>
    <rect x="508" y="20" width="236" height="290" rx="16" class="leaf-bg" stroke="var(--leaf)" stroke-width="2.5"/>
    <text x="626" y="52" text-anchor="middle" class="t-b">@ModelAttribute</text>
    <text x="626" y="72" text-anchor="middle" class="t-s">한 상자에 다 담아서</text>
    <text x="626" y="90" text-anchor="middle" class="t-s">한 번에 받기</text>
    <rect x="556" y="112" width="140" height="104" rx="10" class="leaf"/>
    <text x="626" y="136" text-anchor="middle" class="t-w" style="font-size:15px">📦 MenuDTO</text>
    <text x="626" y="160" text-anchor="middle" class="t-cs" style="fill:var(--paper)">name · price</text>
    <text x="626" y="180" text-anchor="middle" class="t-cs" style="fill:var(--paper)">categoryCode</text>
    <text x="626" y="200" text-anchor="middle" class="t-cs" style="fill:var(--paper)">orderableStatus</text>
    <text x="626" y="240" text-anchor="middle" class="t-s">😄 상자가 그대로</text>
    <text x="626" y="258" text-anchor="middle" class="t-s">쟁반(Model)에도 올라가요</text>
    <text x="626" y="292" text-anchor="middle" class="t-cs">@ModelAttribute MenuDTO</text>
  </svg>
  </div>
  <div class="dict">
    <div class="term"><b>Model · 쟁반</b><span>화면에 보여줄 값을 이름표 붙여 올려두는 곳</span><small>model.addAttribute("message", …)</small></div>
    <div class="term"><b>DTO · 상자</b><span>여러 값을 한 번에 담아 옮기는 상자</span><small>MenuDTO</small></div>
    <div class="term"><b>void 반환 · 주소 = 그릇</b><span>아무것도 안 돌려주면, 들어온 주소가 곧 화면 이름이 돼요</span><small>/request/regist → request/regist.html</small></div>
  </div>
</section>

<!-- 3 -->
<section>
  <h2>3. 알맞은 그릇 찾기 (뷰 리졸버)</h2>
  <p>웨이터가 <code>"result"</code>라고만 말하면, <b>그릇 찾는 아저씨(ViewResolver)</b>가 앞에 <code>templates/</code>를, 뒤에 <code>.html</code>을 붙여서 진짜 그릇을 찾아와요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 180" role="img" aria-label="접두사와 접미사를 붙여 뷰 경로 만들기">
    <rect x="30" y="56" width="200" height="56" rx="12" class="sun-bg"/>
    <text x="130" y="90" text-anchor="middle" class="t-c">resources/templates/</text>
    <text x="130" y="40" text-anchor="middle" class="t-s">앞에 붙이기 (접두사)</text>
    <text x="250" y="92" text-anchor="middle" class="t-b">+</text>
    <rect x="270" y="56" width="140" height="56" rx="12" class="leaf"/>
    <text x="340" y="91" text-anchor="middle" class="t-w">"result"</text>
    <text x="340" y="40" text-anchor="middle" class="t-s">웨이터가 한 말</text>
    <text x="430" y="92" text-anchor="middle" class="t-b">+</text>
    <rect x="450" y="56" width="90" height="56" rx="12" class="sun-bg"/>
    <text x="495" y="90" text-anchor="middle" class="t-c">.html</text>
    <text x="495" y="40" text-anchor="middle" class="t-s">뒤에 붙이기</text>
    <path d="M550,84 L590,84" class="arr" marker-end="url(#mk)"/>
    <rect x="596" y="46" width="140" height="76" rx="14" class="sky-bg"/>
    <text x="666" y="80" text-anchor="middle" class="t-b">🥣 result.html</text>
    <text x="666" y="102" text-anchor="middle" class="t-s">진짜 그릇</text>
    <text x="380" y="160" text-anchor="middle" class="t-s">그릇 이름 말하는 법: String 돌려주기 · void(주소가 곧 이름) · ModelAndView(쟁반+그릇 한 번에)</text>
  </svg>
  </div>
  <p>음식을 내주는 방법도 두 가지예요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 300" role="img" aria-label="forward와 redirect 비교">
    <rect x="16" y="20" width="350" height="260" rx="16" class="leaf-bg"/>
    <text x="191" y="52" text-anchor="middle" class="t-b">forward (기본)</text>
    <text x="191" y="72" text-anchor="middle" class="t-s">웨이터가 직접 들고 와요</text>
    <use href="#kid" x="80" y="170"/>
    <rect x="240" y="120" width="100" height="70" rx="12" class="leaf"/>
    <text x="290" y="160" text-anchor="middle" class="t-w">🤵</text>
    <path d="M110,140 L232,140" class="arr" marker-end="url(#mk)"/>
    <path d="M232,176 L110,176" class="arr-l" marker-end="url(#mkl)"/>
    <text x="171" y="130" text-anchor="middle" class="t-s">주문 1번</text>
    <text x="171" y="198" text-anchor="middle" class="t-s">🍽 쟁반째</text>
    <text x="191" y="250" text-anchor="middle" class="t-s">쟁반(Model)의 값이 그대로 보여요</text>
    <rect x="394" y="20" width="350" height="260" rx="16" class="berry-bg"/>
    <text x="569" y="52" text-anchor="middle" class="t-b">redirect</text>
    <text x="569" y="72" text-anchor="middle" class="t-s">"저쪽 창구로 다시 가세요"</text>
    <use href="#kid" x="450" y="170"/>
    <rect x="590" y="96" width="120" height="44" rx="10" class="paper bd"/>
    <text x="650" y="123" text-anchor="middle" class="t-c">/string-redirect</text>
    <rect x="590" y="170" width="120" height="44" rx="10" class="paper bd"/>
    <text x="650" y="197" text-anchor="middle" class="t-c">/ (메인)</text>
    <path d="M480,128 L582,118" class="arr" marker-end="url(#mk)"/>
    <path d="M582,128 C540,140 520,150 486,150" class="arr-b" stroke-dasharray="5 4" marker-end="url(#mkb)"/>
    <path d="M480,186 L582,192" class="arr" marker-end="url(#mk)"/>
    <text x="530" y="108" text-anchor="middle" class="t-s">① 주문</text>
    <text x="530" y="212" text-anchor="middle" class="t-s">② 다시 감</text>
    <text x="569" y="246" text-anchor="middle" class="t-s">두 번 가니까 쟁반이 비어요 😢</text>
    <text x="569" y="266" text-anchor="middle" class="t-s">→ 한 번만 보이는 쪽지: addFlashAttribute</text>
  </svg>
  </div>
  <p class="cap">redirect는 <code>return "redirect:/";</code>처럼 앞에 <code>redirect:</code>를 붙여요. 회원가입처럼 "다 했으니 메인으로 가" 할 때 써요. 쪽지(<code>RedirectAttributes</code>)는 한 번 보고 나면 사라져요.</p>
</section>

<!-- 4 -->
<section>
  <h2>4. 넘어지면 누가 도와줄까? (예외 처리)</h2>
  <p>웨이터가 일하다 넘어지면(예외 발생) 흐름을 멈추고 도와주는 사람이 와요. <b>우리 반 선생님</b>이 먼저 오고, 우리 반 선생님이 없으면 <b>보건실 선생님</b>이 와요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 320" role="img" aria-label="컨트롤러 안 예외 핸들러와 전역 예외 핸들러 우선순위">
    <rect x="20" y="20" width="330" height="200" rx="16" class="sun-bg"/>
    <text x="40" y="50" class="t-b">🏫 ExceptionTestController 반</text>
    <rect x="40" y="70" width="140" height="40" rx="10" class="paper bd"/>
    <text x="110" y="95" text-anchor="middle" class="t-c">nullpointer</text>
    <text x="110" y="130" text-anchor="middle" class="t-s">💥 넘어짐!</text>
    <rect x="200" y="70" width="130" height="80" rx="12" class="sun"/>
    <text x="265" y="100" text-anchor="middle" class="t-w" style="font-size:15px">우리 반 선생님</text>
    <text x="265" y="122" text-anchor="middle" class="t-cs" style="fill:var(--paper)">@ExceptionHandler</text>
    <path d="M182,90 L194,90" class="arr" marker-end="url(#mk)"/>
    <text x="185" y="196" text-anchor="middle" class="t-s">같은 반 선생님이 1등으로 와요</text>
    <rect x="20" y="236" width="330" height="70" rx="16" class="bg" stroke="var(--line)"/>
    <text x="40" y="264" class="t-b">🏫 GlobalTestController 반</text>
    <text x="40" y="288" class="t-s">💥 넘어졌는데 우리 반 선생님이 없어요…</text>
    <path d="M352,270 C420,270 420,200 452,200" class="arr-b" marker-end="url(#mkb)"/>
    <rect x="460" y="40" width="280" height="260" rx="18" class="berry-bg" stroke="var(--berry)" stroke-width="2"/>
    <text x="600" y="76" text-anchor="middle" class="t-b">🩹 보건실 (전체 담당)</text>
    <text x="600" y="96" text-anchor="middle" class="t-cs">@ControllerAdvice</text>
    <rect x="480" y="116" width="240" height="60" rx="12" class="paper bd"/>
    <text x="600" y="142" text-anchor="middle" class="t-c">NullPointerException</text>
    <text x="600" y="162" text-anchor="middle" class="t-s">이 아픔 전문 선생님</text>
    <rect x="480" y="188" width="240" height="60" rx="12" class="paper bd"/>
    <text x="600" y="214" text-anchor="middle" class="t-c">Exception</text>
    <text x="600" y="234" text-anchor="middle" class="t-s">그 밖에 뭐든 봐주는 선생님</text>
    <text x="600" y="276" text-anchor="middle" class="t-s">→ error/default.html 보여주기</text>
  </svg>
  </div>
  <p>내가 직접 만든 아픔도 있어요. <code>MemberNotFoundException</code>은 "회원이 없어요!"라는 우리만의 예외예요. <code>throw</code>로 던지면 그 아픔 담당 선생님이 받아줘요.</p>
  <p class="cap">AOP에서 Advice는 "여러 곳에 반복되는 일 묶음"이었죠. <code>@ControllerAdvice</code>도 같은 뜻이에요. 모든 컨트롤러에 공통으로 필요한 예외 처리를 한곳에 묶어둔 거예요.</p>
</section>

<!-- 5 -->
<section>
  <h2>5. 문 앞의 스톱워치 아저씨 (인터셉터)</h2>
  <p>웨이터한테 가기 전에, 문 앞에서 <b>스톱워치 아저씨(Interceptor)</b>를 꼭 지나가요. 아저씨는 들어갈 때 시계를 켜고, 웨이터 일이 끝나면 시계를 봐서 몇 초 걸렸는지 쟁반에 적어줘요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 290" role="img" aria-label="preHandle과 postHandle">
    <use href="#kid" x="60" y="150"/>
    <text x="60" y="212" text-anchor="middle" class="t-s">/stopwatch</text>
    <rect x="140" y="50" width="200" height="200" rx="18" class="berry-bg"/>
    <text x="240" y="82" text-anchor="middle" class="t-b">⏱ 스톱워치 아저씨</text>
    <text x="240" y="102" text-anchor="middle" class="t-cs">TimerInterceptor</text>
    <rect x="160" y="118" width="160" height="50" rx="10" class="paper bd"/>
    <text x="240" y="140" text-anchor="middle" class="t-c">preHandle</text>
    <text x="240" y="158" text-anchor="middle" class="t-s">들어갈 때 · 시계 켜기</text>
    <rect x="160" y="182" width="160" height="50" rx="10" class="paper bd"/>
    <text x="240" y="204" text-anchor="middle" class="t-c">postHandle</text>
    <text x="240" y="222" text-anchor="middle" class="t-s">나올 때 · 걸린 시간 적기</text>
    <path d="M90,140 L152,140" class="arr" marker-end="url(#mk)"/>
    <path d="M324,140 L450,140" class="arr" marker-end="url(#mk)"/>
    <text x="387" y="128" text-anchor="middle" class="t-s">true면 통과 ✅</text>
    <text x="387" y="164" text-anchor="middle" class="t-s">false면 못 들어가요 ⛔</text>
    <rect x="456" y="96" width="150" height="110" rx="14" class="leaf-bg"/>
    <text x="531" y="130" text-anchor="middle" class="t-b">🤵 웨이터</text>
    <text x="531" y="152" text-anchor="middle" class="t-cs">InterceptorController</text>
    <text x="531" y="176" text-anchor="middle" class="t-s">2초 동안 일하기 😴</text>
    <path d="M456,206 C420,230 360,220 326,210" class="arr-l" marker-end="url(#mkl)"/>
    <rect x="630" y="96" width="114" height="110" rx="14" class="sky-bg"/>
    <text x="687" y="130" text-anchor="middle" class="t-b">🥣 화면</text>
    <text x="687" y="156" text-anchor="middle" class="t-s">수행 시간:</text>
    <text x="687" y="178" text-anchor="middle" class="t-c">약 2000ms</text>
    <path d="M610,150 L624,150" class="arr" marker-end="url(#mk)"/>
  </svg>
  </div>
  <p>그런데 아저씨는 <b>출근 명단에 이름을 올려야</b> 일을 해요. 그 명단이 <code>WebConfig</code>(<code>WebMvcConfigurer</code>)예요. 여기서 "이 주소는 아저씨가 지켜요"라고 등록해 줘요.</p>
  <div class="pic">
  <svg viewBox="0 0 760 250" role="img" aria-label="addPathPatterns와 excludePathPatterns">
    <rect x="20" y="20" width="300" height="210" rx="16" class="sun-bg"/>
    <text x="170" y="52" text-anchor="middle" class="t-b">📋 출근 명단 (WebConfig)</text>
    <text x="40" y="86" class="t">1. 아저씨를 데려와요</text>
    <text x="56" y="106" class="t-cs">생성자 주입으로 받기</text>
    <text x="40" y="140" class="t">2. 지킬 문을 정해요</text>
    <text x="56" y="160" class="t-cs">addPathPatterns("/*")</text>
    <text x="40" y="194" class="t">3. 안 지켜도 되는 문</text>
    <text x="56" y="214" class="t-cs">excludePathPatterns(…)</text>
    <path d="M326,125 L370,125" class="arr" marker-end="url(#mk)"/>
    <rect x="380" y="20" width="170" height="210" rx="16" class="berry-bg"/>
    <text x="465" y="52" text-anchor="middle" class="t-b">⏱ 지켜요</text>
    <rect x="400" y="72" width="130" height="34" rx="10" class="paper bd"/>
    <text x="465" y="94" text-anchor="middle" class="t-c">/stopwatch</text>
    <text x="465" y="136" text-anchor="middle" class="t-s">/* = 한 칸짜리 주소</text>
    <text x="465" y="156" text-anchor="middle" class="t-s">(웨이터한테 가는 손님)</text>
    <rect x="570" y="20" width="170" height="210" rx="16" class="bg" stroke="var(--line)" stroke-dasharray="6 5"/>
    <text x="655" y="52" text-anchor="middle" class="t-b">🚪 그냥 통과</text>
    <text x="655" y="86" text-anchor="middle" class="t-c">/css/**</text>
    <text x="655" y="110" text-anchor="middle" class="t-c">/images/**</text>
    <text x="655" y="134" text-anchor="middle" class="t-c">/js/**</text>
    <text x="655" y="158" text-anchor="middle" class="t-c">/video/**</text>
    <text x="655" y="192" text-anchor="middle" class="t-s">그림·꾸미기 파일은</text>
    <text x="655" y="210" text-anchor="middle" class="t-s">시간 잴 필요 없어요</text>
  </svg>
  </div>
  <p class="cap"><code>/*</code>는 <code>/stopwatch</code>처럼 <b>한 칸</b>짜리 주소만, <code>/**</code>는 <code>/images/a/b.png</code>처럼 <b>안쪽 깊은 곳까지 전부</b>예요.</p>
  <div class="dict">
    <div class="term"><b>Interceptor · 문지기</b><span>컨트롤러 앞뒤에서 공통으로 할 일을 하는 사람</span><small>HandlerInterceptor</small></div>
    <div class="term"><b>preHandle · 들어갈 때</b><span>true면 들여보내고 false면 막아요</span><small>boolean preHandle(…)</small></div>
    <div class="term"><b>postHandle · 나올 때</b><span>웨이터 일이 끝난 뒤, 화면 그리기 전에 해요</span><small>void postHandle(…)</small></div>
    <div class="term"><b>WebConfig · 출근 명단</b><span>Spring MVC 기본 설정에 내 설정을 더하는 곳</span><small>addInterceptors(…)</small></div>
  </div>
  <p class="cap">AOP의 <code>@Around</code> 시계 비서랑 비슷하죠? 인터셉터는 <b>웹 요청이 들어오는 문 앞</b>에서만 일한다는 점이 달라요.</p>
</section>

<div class="sum">
  <b style="font-family:var(--f-display);font-weight:400;font-size:22px">한 줄 정리</b><br>
  손님 주소를 안내판(매핑)이 웨이터에게 보내면, 웨이터(Controller)가 주문서를 받아(파라미터) 쟁반(Model)에 담고, 그릇 아저씨(ViewResolver)가 그릇을 찾아 내줘요. 문 앞엔 스톱워치 아저씨(Interceptor), 넘어지면 보건실(ExceptionHandler)이 있어요.
</div>
</div>
