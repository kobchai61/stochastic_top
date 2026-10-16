<!doctype html>
<html lang="th">
<head>a
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>คู่มือสอบ Stochastic</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@500;600;700&family=IBM+Plex+Mono:wght@400;500&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&display=swap">
<style>
[hidden]{display:none!important}
img{max-width:100%}
body{margin:0}

/* Layout: engineering lab notebook. Sticky lecture index (L14–L27) on the left, one reading column on the right.
   Each chapter = "เข้าใจก่อน" intuition → formula plates (numbered as in the slides) → worked examples → live calculator → pitfalls. */
:root{
  --bg:#f2f4f7; --surface:#ffffff; --plate:#f7f9fb; --ink:#14202f; --muted:#596579; --line:#d9dfe7; --grid:#e3e8ee;
  --accent:#0a6a88; --accent-ink:#ffffff; --accent-wash:rgba(10,106,136,.10);
  --tail:#bf6410; --tail-wash:rgba(191,100,16,.12);
  --good:#1d7548; --good-wash:rgba(29,117,72,.10); --bad:#b0352d; --bad-wash:rgba(176,53,45,.10);
  --f-display:"Chakra Petch","IBM Plex Sans Thai","Noto Sans Thai",system-ui,sans-serif;
  --f-body:"IBM Plex Sans Thai","Noto Sans Thai","Leelawadee UI","Thonburi",system-ui,sans-serif;
  --f-mono:"IBM Plex Mono",ui-monospace,"SFMono-Regular",Menlo,Consolas,monospace;
  --r:8px;
}
@media (prefers-color-scheme: dark){ :root:not([data-theme="light"]){
  --bg:#0e131a; --surface:#151c26; --plate:#18212d; --ink:#e4e9f0; --muted:#9aa6b8; --line:#2a3544; --grid:#1b2430;
  --accent:#4cb3d3; --accent-ink:#06222c; --accent-wash:rgba(76,179,211,.14);
  --tail:#eaa259; --tail-wash:rgba(234,162,89,.16);
  --good:#5cc690; --good-wash:rgba(92,198,144,.13); --bad:#ee7d73; --bad-wash:rgba(238,125,115,.14);
  color-scheme:dark;
}}
:root[data-theme="dark"]{
  --bg:#0e131a; --surface:#151c26; --plate:#18212d; --ink:#e4e9f0; --muted:#9aa6b8; --line:#2a3544; --grid:#1b2430;
  --accent:#4cb3d3; --accent-ink:#06222c; --accent-wash:rgba(76,179,211,.14);
  --tail:#eaa259; --tail-wash:rgba(234,162,89,.16);
  --good:#5cc690; --good-wash:rgba(92,198,144,.13); --bad:#ee7d73; --bad-wash:rgba(238,125,115,.14);
  color-scheme:dark;
}
*{box-sizing:border-box}
html{scroll-behavior:smooth}
@media (prefers-reduced-motion: reduce){html{scroll-behavior:auto}}
body{background:var(--bg);color:var(--ink);font:400 16px/1.8 var(--f-body);
  padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));}
a{color:var(--accent)}
:focus-visible{outline:2px solid var(--accent);outline-offset:2px;border-radius:4px}
h1,h2,h3,h4{font-family:var(--f-display);line-height:1.3;text-wrap:balance;margin:0}
h2{font-size:1.75rem;font-weight:700}
h3{font-size:1.25rem;font-weight:600;margin-top:2.2rem}
h4{font-size:1.05rem;font-weight:600}
p{margin:0}
code,.mono{font-family:var(--f-mono);font-size:.88em}
.wrap{max-width:1180px;margin:0 auto;display:grid;grid-template-columns:240px minmax(0,1fr);gap:48px;padding-block:28px 80px}
@media (max-width:900px){.wrap{grid-template-columns:minmax(0,1fr);gap:20px}}

/* ---------- sidebar ---------- */
.side{position:sticky;top:calc(env(safe-area-inset-top,0px) + 20px);align-self:start;max-height:calc(100vh - 40px);overflow:auto;padding-right:4px}
.side .brand{font:700 1.05rem/1.3 var(--f-display);margin-bottom:4px}
.side .sub{font:500 .78rem/1.4 var(--f-mono);color:var(--muted);letter-spacing:.04em;margin-bottom:18px}
.toc{list-style:none;margin:0;padding:0;display:flex;flex-direction:column;gap:1px}
.toc a{display:grid;grid-template-columns:44px 1fr;gap:8px;align-items:baseline;padding:5px 8px;border-radius:6px;color:var(--ink);text-decoration:none;font-size:.9rem;line-height:1.45}
.toc a .l{font:500 .72rem var(--f-mono);color:var(--muted);letter-spacing:.03em}
.toc a:hover{background:var(--accent-wash)}
.toc a.on{background:var(--accent-wash);color:var(--accent);font-weight:600}
.toc a.on .l{color:var(--accent)}
.toc .grp{font:600 .7rem var(--f-mono);letter-spacing:.1em;text-transform:uppercase;color:var(--muted);padding:14px 8px 4px}
.mtoc{display:none}
@media (max-width:900px){
  .side{display:none}
  .mtoc{display:block;background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:4px 14px}
  .mtoc summary{cursor:pointer;font:600 .95rem var(--f-display);padding:8px 0}
  .mtoc .toc{padding-bottom:10px}
}

/* ---------- hero ---------- */
.hero{position:relative;padding:40px 0 34px;border-bottom:1px solid var(--line);margin-bottom:8px}
.hero::before{content:"";position:absolute;inset:0;z-index:-1;
  background-image:linear-gradient(var(--grid) 1px,transparent 1px),linear-gradient(90deg,var(--grid) 1px,transparent 1px);
  background-size:24px 24px;mask-image:linear-gradient(180deg,#000 30%,transparent 100%);-webkit-mask-image:linear-gradient(180deg,#000 30%,transparent 100%)}
.eyebrow{font:600 .75rem var(--f-mono);letter-spacing:.12em;text-transform:uppercase;color:var(--accent)}
.hero h1{font-size:clamp(2rem,5vw,3.1rem);font-weight:700;margin:10px 0 14px;letter-spacing:-.01em}
.hero h1 span{color:var(--accent)}
.hero p.lead{max-width:62ch;color:var(--muted);font-size:1.05rem}
.route{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin-top:22px}
.route a{display:inline-flex;gap:8px;align-items:baseline;background:var(--surface);border:1px solid var(--line);border-radius:999px;padding:5px 13px;text-decoration:none;color:var(--ink);font-size:.88rem}
.route a b{font:600 .72rem var(--f-mono);color:var(--accent)}
.route .arr{color:var(--muted);font-family:var(--f-mono)}

/* ---------- chapters ---------- */
main{min-width:0}
.ch{padding-top:56px;scroll-margin-top:12px}
.ch-head{display:flex;flex-wrap:wrap;align-items:baseline;gap:6px 14px;margin-bottom:18px;padding-bottom:12px;border-bottom:2px solid var(--ink)}
.ch-head .lec{font:600 .78rem var(--f-mono);letter-spacing:.06em;color:var(--accent);border:1px solid var(--accent);border-radius:4px;padding:1px 7px}
.ch-head .sec{font:500 .8rem var(--f-mono);color:var(--muted)}
.ch-head h2{flex-basis:100%}
.prose{display:flex;flex-direction:column;gap:14px;max-width:72ch}
.prose ul,.prose ol{margin:0;padding-left:1.3em;display:flex;flex-direction:column;gap:6px}
.idea{background:var(--accent-wash);border-radius:var(--r);padding:16px 20px;display:flex;flex-direction:column;gap:10px;max-width:72ch}
.idea .tag,.plate .tag,.warn .tag,.tool .tag{font:600 .72rem var(--f-mono);letter-spacing:.1em;text-transform:uppercase}
.idea .tag{color:var(--accent)}
.plate{background:var(--plate);border:1px solid var(--line);border-radius:var(--r);padding:14px 18px;margin:14px 0;min-width:0}
.plate .tag{color:var(--muted);display:block;margin-bottom:2px}
.plate p{color:var(--muted);font-size:.93rem}
.warn{border:1px dashed var(--tail);border-radius:var(--r);padding:14px 18px;margin:16px 0;max-width:72ch;display:flex;flex-direction:column;gap:6px}
.warn .tag{color:var(--tail)}
.two{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px;margin:14px 0}
@media (max-width:640px){.two{grid-template-columns:minmax(0,1fr)}}
.two > div{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:14px 16px;min-width:0}
.two h4{margin-bottom:6px}
mjx-container[display="true"]{overflow-x:auto;overflow-y:hidden;max-width:100%;padding:4px 0}
mjx-container{color:var(--ink)}
main p,main li,.plate,.q,.step > div,.two > div,table.t td{overflow-wrap:anywhere}
.plate{overflow-x:auto}
mjx-container:not([display]){display:inline-block;max-width:100%;overflow-x:auto;overflow-y:hidden;vertical-align:middle}

/* ---------- examples ---------- */
.ex{margin:22px 0;border-top:1px solid var(--line);padding-top:16px;display:flex;flex-direction:column;gap:10px}
.ex h4 .no{font:600 .8rem var(--f-mono);color:var(--accent);margin-right:8px}
.q{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:12px 16px;max-width:72ch}
.q b:first-child{font-family:var(--f-display)}
details.sol{border-left:2px solid var(--line);padding-left:16px}
details.sol > summary{cursor:pointer;font:600 .85rem var(--f-mono);color:var(--accent);padding:4px 0;list-style:none}
details.sol > summary::-webkit-details-marker{display:none}
details.sol > summary::before{content:"▾ ";}
details.sol:not([open]) > summary::before{content:"▸ ";}
details.sol .steps{display:flex;flex-direction:column;gap:10px;padding:6px 0 4px;max-width:74ch}
.ans{display:inline-block;background:var(--good-wash);color:var(--good);font-weight:600;border-radius:4px;padding:1px 8px}
.step{display:grid;grid-template-columns:28px minmax(0,1fr);gap:8px}
.step > .n{font:600 .8rem/1.9 var(--f-mono);color:var(--muted)}
.step > div{min-width:0}

/* ---------- tables ---------- */
.tw{overflow-x:auto;margin:14px 0;border:1px solid var(--line);border-radius:var(--r);background:var(--surface)}
table.t{border-collapse:collapse;width:100%;font-size:.92rem}
table.t th,table.t td{padding:8px 12px;border-bottom:1px solid var(--line);text-align:left;vertical-align:top}
table.t th{font:600 .78rem var(--f-mono);letter-spacing:.04em;color:var(--muted);background:var(--plate);white-space:nowrap}
table.t tr:last-child td{border-bottom:0}
table.t td.num,table.t th.num{text-align:right;font-family:var(--f-mono);font-variant-numeric:tabular-nums}
table.t tr.hl td{background:var(--accent-wash)}

/* ---------- figures ---------- */
figure{margin:16px 0;min-width:0}
figure .fbox{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:10px;overflow-x:auto}
figure svg{display:block;width:100%;height:auto;min-width:320px}
figcaption{font-size:.85rem;color:var(--muted);margin-top:6px;max-width:72ch}
svg .curve{fill:none;stroke:var(--ink);stroke-width:2}
svg .curve2{fill:none;stroke:var(--accent);stroke-width:2}
svg .curve3{fill:none;stroke:var(--tail);stroke-width:2}
svg .axis{stroke:var(--muted);stroke-width:1}
svg .gridl{stroke:var(--grid);stroke-width:1}
svg .ax-t{fill:var(--muted);font:12px var(--f-mono)}
svg .lbl{fill:var(--ink);font:500 13px var(--f-body)}
svg .lbl-a{fill:var(--accent);font:600 13px var(--f-mono)}
svg .lbl-t{fill:var(--tail);font:600 13px var(--f-mono)}
svg .area-a{fill:var(--accent);fill-opacity:.2}
svg .area-t{fill:var(--tail);fill-opacity:.4}
svg .mark{stroke:var(--tail);stroke-width:1.5;stroke-dasharray:4 3}
svg .mark-a{stroke:var(--accent);stroke-width:1.5;stroke-dasharray:4 3}
svg .bar{fill:var(--accent);fill-opacity:.55}
svg .dot{fill:var(--tail)}
svg .node{fill:var(--surface);stroke:var(--ink);stroke-width:1.6}
svg .node-t{fill:var(--ink);font:600 15px var(--f-mono)}
svg .edge{fill:none;stroke:var(--accent);stroke-width:1.6}
svg .edge-b{fill:none;stroke:var(--tail);stroke-width:1.6}
svg .arrowhead{fill:var(--accent)}
svg .arrowhead-b{fill:var(--tail)}
svg .box{fill:var(--plate);stroke:var(--line);stroke-width:1.5}
svg .rect-a{fill:var(--accent);fill-opacity:.18;stroke:var(--accent);stroke-width:1.5}

/* ---------- tools ---------- */
.tool{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:18px 20px;margin:22px 0;box-shadow:0 1px 0 var(--line);min-width:0}
.tool .tag{color:var(--accent)}
.tool h4{margin:2px 0 4px}
.tool .hint{color:var(--muted);font-size:.88rem}
.fields{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:10px 14px;margin:14px 0}
.fields label{display:flex;flex-direction:column;gap:4px;font-size:.82rem;color:var(--muted);min-width:0}
.fields input,.fields select,.tool textarea{font:500 .95rem var(--f-mono);color:var(--ink);background:var(--plate);border:1px solid var(--line);border-radius:6px;padding:7px 9px;width:100%;min-width:0}
.tool textarea{min-height:64px;resize:vertical}
.fields .wide{grid-column:1/-1}
.seg{display:inline-flex;border:1px solid var(--line);border-radius:8px;overflow:hidden;flex-wrap:wrap}
.seg button{font:500 .85rem var(--f-body);background:var(--plate);color:var(--ink);border:0;padding:6px 12px;cursor:pointer}
.seg button[aria-pressed="true"]{background:var(--accent);color:var(--accent-ink)}
.out{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:10px;margin-top:6px}
.out .k{background:var(--plate);border:1px solid var(--line);border-radius:8px;padding:8px 12px;min-width:0}
.out .k span{display:block;font:500 .72rem var(--f-mono);color:var(--muted);letter-spacing:.04em}
.out .k b{font:600 1.08rem var(--f-mono);font-variant-numeric:tabular-nums;word-break:break-all}
.verdict{margin-top:12px;padding:10px 14px;border-radius:8px;font-weight:600}
.verdict.ok{background:var(--good-wash);color:var(--good)}
.verdict.no{background:var(--bad-wash);color:var(--bad)}
.verdict.info{background:var(--accent-wash);color:var(--accent)}
.matrix{display:inline-grid;gap:6px;margin:10px 0}
.matrix input{width:74px;font:500 .95rem var(--f-mono);text-align:center;color:var(--ink);background:var(--plate);border:1px solid var(--line);border-radius:6px;padding:6px}
.mout{font-family:var(--f-mono);font-size:.9rem;white-space:pre;overflow-x:auto;background:var(--plate);border:1px solid var(--line);border-radius:6px;padding:10px 12px;margin-top:8px}

/* ---------- checklist ---------- */
.check{list-style:none;padding:0;margin:14px 0;display:flex;flex-direction:column;gap:6px;max-width:74ch}
.check label{display:grid;grid-template-columns:22px 1fr;gap:10px;align-items:start;background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:9px 12px;cursor:pointer}
.check input{margin-top:6px;accent-color:var(--accent)}
.check input:checked + span{color:var(--muted);text-decoration:line-through}
.progress{font:500 .85rem var(--f-mono);color:var(--muted)}
.xref{display:flex;flex-wrap:wrap;gap:6px;align-items:center;font-size:.85rem;color:var(--muted);margin:-6px 0 14px}
.xref a{font:600 .8rem var(--f-mono);text-decoration:none;border:1px solid var(--line);background:var(--surface);border-radius:999px;padding:2px 10px}
footer{margin-top:60px;padding-top:18px;border-top:1px solid var(--line);color:var(--muted);font-size:.85rem}
</style>

</head>
<body>
<div class="wrap">
<aside class="side" aria-label="สารบัญ">
  <div class="brand">คู่มือสอบ Stochastic</div>
  <div class="sub">EN813001 · L14–L27</div>
  <nav><ul class="toc" id="toc">
    <li><a href="#overview"><span class="l">MAP</span><span>แผนที่ + เลือกสูตร</span></a></li>
    <li class="grp">Ch5–6 สถิติ</li>
    <li><a href="#clt"><span class="l">L14</span><span>CLT &amp; Stirling</span></a></li>
    <li><a href="#est"><span class="l">L14–15</span><span>Estimators</span></a></li>
    <li><a href="#hist"><span class="l">L15</span><span>Histogram</span></a></li>
    <li><a href="#chi"><span class="l">L16</span><span>Chi-squared test</span></a></li>
    <li><a href="#cimean"><span class="l">L17</span><span>CI ของ mean</span></a></li>
    <li><a href="#civar"><span class="l">L18–19</span><span>CI ของ variance</span></a></li>
    <li><a href="#test"><span class="l">L19–20</span><span>Hypothesis test</span></a></li>
    <li><a href="#reg"><span class="l">L20–21</span><span>Regression</span></a></li>
    <li class="grp">Ch7–8 หลายตัวแปร</li>
    <li><a href="#biv"><span class="l">L21–22</span><span>Bivariate</span></a></li>
    <li><a href="#cond"><span class="l">L22</span><span>Conditional</span></a></li>
    <li><a href="#vec"><span class="l">L22–23</span><span>Random vectors</span></a></li>
    <li class="grp">Ch10–12 Processes</li>
    <li><a href="#proc"><span class="l">L23–24</span><span>Random process</span></a></li>
    <li><a href="#poisson"><span class="l">L24–25</span><span>Poisson process</span></a></li>
    <li><a href="#markov"><span class="l">L25</span><span>Markov chain</span></a></li>
    <li><a href="#queue"><span class="l">L26–27</span><span>Queueing M/M/1</span></a></li>
    <li class="grp">ก่อนสอบ</li>
    <li><a href="#exam"><span class="l">EXAM</span><span>ข้อสอบเก่า Final 2564</span></a></li>
    <li><a href="#crit"><span class="l">CALC</span><span>หาค่าวิกฤต</span></a></li>
    <li><a href="#check"><span class="l">LIST</span><span>จุดพลาด + checklist</span></a></li>
  </ul></nav>
</aside>

<main>
<details class="mtoc"><summary>สารบัญ (L14–L27)</summary><div id="mtoc-slot"></div></details>

<header class="hero">
  <div class="eyebrow">EN813001 Stochastic Processes and Modeling · Lecture 14–27</div>
  <h1>คู่มือสอบ <span>Stochastic</span></h1>
  <p class="lead">อธิบายทุกหัวข้อตั้งแต่ CLT ไปจนถึงคิว M/M/1 แบบเข้าใจก่อนจำสูตร แต่ละบทมีแนวคิดภาษาง่าย สูตรเลขเดียวกับสไลด์ ตัวอย่างพร้อมวิธีทำทีละขั้น และเครื่องคิดเลขให้ลองเปลี่ยนตัวเลขเอง</p>
  <div class="route" aria-label="เส้นทางเนื้อหา">
    <a href="#clt"><b>Ch5</b>CLT</a><span class="arr">→</span>
    <a href="#est"><b>Ch6</b>ประมาณค่า · ทดสอบ</a><span class="arr">→</span>
    <a href="#biv"><b>Ch7–8</b>หลายตัวแปร</a><span class="arr">→</span>
    <a href="#proc"><b>Ch10–11</b>Process · Poisson</a><span class="arr">→</span>
    <a href="#markov"><b>Ch12</b>Markov</a><span class="arr">→</span>
    <a href="#queue"><b>Q</b>คิว M/M/1</a><span class="arr">→</span>
    <a href="#exam"><b>EXAM</b>ข้อสอบเก่า</a>
  </div>
</header>

<section class="ch" id="overview">
  <div class="ch-head"><span class="lec">MAP</span><h2>ภาพรวม: เนื้อหาทั้งหมดเชื่อมกันยังไง</h2></div>
  <div class="prose">
    <p>ครึ่งแรก (Ch5–6) คือ <b>สถิติ</b>: เรามี data X₁, …, Xₙ แล้วอยากรู้ว่ามันมาจาก model อะไร มีพารามิเตอร์เท่าไร และมั่นใจได้แค่ไหน ครึ่งหลัง (Ch7–12) คือ <b>probability ของหลายตัวแปรและของสิ่งที่เปลี่ยนตามเวลา</b> ซึ่งไปจบที่การวิเคราะห์คิวในระบบเครือข่าย</p>
  </div>
  <div class="two">
    <div><h4>Probability (ทฤษฎี)</h4><p>รู้ pmf/pdf แล้ว → ทำนายว่าเหตุการณ์จะเกิดด้วยโอกาสเท่าไร</p></div>
    <div><h4>Statistics (ปฏิบัติ)</h4><p>มี data แล้ว → ย้อนกลับไปเดา pmf/pdf หรือพารามิเตอร์ และบอกความมั่นใจ</p></div>
  </div>
  <h3>เลือกสูตรให้ถูก: ตารางตัดสินใจ</h3>
  <div class="prose"><p>ข้อสอบ Ch6 ส่วนใหญ่วัดว่าเลือก statistic และตารางถูกหรือไม่ ใช้ตารางนี้ถามตัวเองก่อนลงมือคำนวณ</p></div>
  <div class="tw"><table class="t">
    <thead><tr><th>อยากรู้</th><th>เงื่อนไข</th><th>ตัวที่ใช้ / การแจกแจง</th><th>หาค่าวิกฤตจาก</th><th>คำตอบ</th></tr></thead>
    <tbody>
      <tr><td>mean m</td><td>รู้ σ, n ใหญ่ (หรือ data เป็น Gaussian)</td><td>\(Y_n=\frac{M_n-m}{\sigma/\sqrt n}\) ≈ N(0,1)</td><td>\(\Phi(y_{\alpha/2})=1-\alpha/2\)</td><td>\(M_n\pm\frac{\sigma y_{\alpha/2}}{\sqrt n}\)</td></tr>
      <tr><td>mean m</td><td>ไม่รู้ σ, n ใหญ่</td><td>แทน σ ด้วย Sₙ, ≈ N(0,1)</td><td>\(\Phi(y_{\alpha/2})=1-\alpha/2\)</td><td>\(M_n\pm\frac{S_n y_{\alpha/2}}{\sqrt n}\)</td></tr>
      <tr><td>mean m</td><td>ไม่รู้ σ, data เป็น Gaussian, n เล็ก</td><td>\(T=\frac{M_n-m}{S_n/\sqrt n}\) ~ Student t (n−1)</td><td>\(F_T(y)=1-\alpha/2\)</td><td>\(M_n\pm\frac{S_n y_{\alpha/2}}{\sqrt n}\)</td></tr>
      <tr><td>variance σ²</td><td>Gaussian, รู้ m</td><td>\(\frac{n}{\sigma^2}V_n^2\) ~ χ²(n)</td><td>\(F(\ell)=\alpha/2,\ F(u)=1-\alpha/2\)</td><td>\(\left[\frac{nV_n^2}{u},\frac{nV_n^2}{\ell}\right]\)</td></tr>
      <tr><td>variance σ²</td><td>Gaussian, ไม่รู้ m</td><td>\(\frac{n-1}{\sigma^2}S_n^2\) ~ χ²(n−1)</td><td>\(F(\ell)=\alpha/2,\ F(u)=1-\alpha/2\)</td><td>\(\left[\frac{(n-1)S_n^2}{u},\frac{(n-1)S_n^2}{\ell}\right]\)</td></tr>
      <tr><td>pmf/pdf fit ไหม</td><td>n ใหญ่, ทุก bin มี npⱼ ≥ 5</td><td>\(Z=\sum\frac{(H_j-np_j)^2}{np_j}\) ~ χ²(m−1−r)</td><td>\(F_Z(z_\alpha)=1-\alpha\)</td><td>Z ≤ z_α ยอมรับ</td></tr>
      <tr><td>ทดสอบ claim เรื่อง mean</td><td>H₀ vs H₁ (3 แบบ)</td><td>\(Z_n=\frac{M_n-m_0}{S_n/\sqrt n}\)</td><td>one-tailed \(\Phi(y_\alpha)=1-\alpha\) · two-tailed \(1-\alpha/2\)</td><td>ดูว่า Zₙ ตกใน acceptance region ไหม</td></tr>
    </tbody>
  </table></div>
</section>

<!-- ===================== L14 CLT ===================== -->
<section class="ch" id="clt">
  <div class="ch-head"><span class="lec">L14</span><span class="sec">Ch5 · Central Limit Theorem</span><h2>CLT approximation และ Stirling's formula</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>เอา random variable ที่ i.i.d. มาบวกกันเยอะๆ \(T_n=X_1+\cdots+X_n\) รูปร่างการแจกแจงของผลรวมจะค่อยๆ กลายเป็นระฆังคว่ำ (normal) เสมอ ไม่ว่า Xᵢ ตั้งต้นจะหน้าตาแบบไหน โดยมี mean \(nm\) และ variance \(n\sigma^2\)</p>
    <p>ปัญหาคือ ถ้า Xᵢ เป็นจำนวนเต็ม (เช่น Bernoulli) Tₙ ก็เป็นจำนวนเต็ม แต่ normal เป็น continuous ความน่าจะเป็นที่จุดเดียวเป็น 0 ทางแก้คือมองค่า k เป็น "แท่งกว้าง 1 หน่วย" ตั้งแต่ k − ½ ถึง k + ½ แล้วเอาพื้นที่ใต้ normal ตรงแถบนั้น ซึ่งประมาณได้เป็น <b>ความสูงของ density × ความกว้าง</b></p>
  </div>
  <div class="plate"><span class="tag">ขั้นที่ 1 · มองจำนวนเต็มเป็นแถบ</span>
  \[P(T_n=k)=P\!\left(k-\tfrac12&lt;T_n\le k+\tfrac12\right)=G_n\!\left(k+\tfrac12\right)-G_n\!\left(k-\tfrac12\right),\qquad G_n(x)=F_{Y_n}\!\left(\frac{x-nm}{\sigma\sqrt n}\right)\]</div>
  <div class="plate"><span class="tag">ขั้นที่ 2 · พื้นที่แถบแคบ ≈ ความสูง × ความกว้าง</span>
  \[F_{Y_n}(y+\delta/2)-F_{Y_n}(y-\delta/2)\approx\int_{y-\delta/2}^{y+\delta/2}\frac{e^{-t^2/2}}{\sqrt{2\pi}}\,dt\approx\frac{e^{-y^2/2}}{\sqrt{2\pi}}\,\delta,\qquad y=\frac{k-nm}{\sigma\sqrt n},\ \delta=\frac{1}{\sigma\sqrt n}\]</div>
  <div class="plate"><span class="tag">สูตรหลัก (5.14)</span>
  \[P(T_n=k)\approx\frac{1}{\sqrt{2\pi}}\exp\!\left[-\frac12\left(\frac{k-nm}{\sigma\sqrt n}\right)^{2}\right]\frac{1}{\sigma\sqrt n}\]
  <p>แม่นที่สุดเมื่อ k อยู่ใกล้ค่าเฉลี่ย nm และแย่ลงเมื่อ k อยู่ไกล (ที่หาง)</p></div>

  <div class="ex">
    <h4><span class="no">Ex 5.18</span>Stirling's formula</h4>
    <div class="q"><b>โจทย์:</b> ให้ X₁, …, Xₙ เป็น i.i.d. exp(1) ดังนั้น Tₙ = X₁ + ⋯ + Xₙ มี density gₙ เป็น Erlang(n, 1) เนื่องจาก m = σ² = 1 ใช้ CLT approximation หาสูตรประมาณ n!</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Erlang(n,1) ต้องประมาณได้ด้วย normal ที่ mean n, variance n \[g_n(x)=\frac{x^{n-1}e^{-x}}{(n-1)!}\approx\frac{1}{\sqrt{2\pi n}}\exp\!\left[-\frac12\left(\frac{x-n}{\sqrt n}\right)^2\right]\quad(5.13)\]</div></div>
      <div class="step"><span class="n">2</span><div>ประมาณแม่นสุดตรงกลาง จึงแทน x = n (เทอม exp กลายเป็น e⁰ = 1) \[\frac{n^{n-1}e^{-n}}{(n-1)!}\approx\frac{1}{\sqrt{2\pi n}}\ \Rightarrow\ (n-1)!\approx\sqrt{2\pi}\,n^{n-1/2}e^{-n}\]</div></div>
      <div class="step"><span class="n">3</span><div>คูณ n ทั้งสองข้าง ได้ <span class="ans">\(n!\approx\sqrt{2\pi}\,n^{n+1/2}e^{-n}\)</span></div></div>
    </div></details>
  </div>

  <div class="ex">
    <h4><span class="no">Ex 5.19</span>Normal approximation ของ binomial</h4>
    <div class="q"><b>โจทย์:</b> ให้ Xᵢ เป็น i.i.d. Bernoulli(p) ใช้ (5.14) ประมาณ P(Tₙ = k)</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Bernoulli(p) มี m = p และ σ² = p(1 − p) แทนใน (5.14): nm = np และ σ√n = √(np(1−p))</div></div>
      <div class="step"><span class="n">2</span><div>ผลรวมของ Bernoulli n ตัวคือ binomial(n, p) \[\binom{n}{k}p^k(1-p)^{n-k}\approx\frac{1}{\sqrt{2\pi}}\exp\!\left[-\frac12\left(\frac{k-np}{\sqrt{np(1-p)}}\right)^2\right]\frac{1}{\sqrt{np(1-p)}}\]</div></div>
      <div class="step"><span class="n">3</span><div>ข้อสังเกต: ถ้า k &gt; n ค่าจริงเป็น 0 (เพราะ Tₙ ≤ n) แต่สูตรขวามือยังให้ค่าบวก แสดงว่าสูตรนี้ใช้ได้ดีเฉพาะใกล้ np</div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-clt">
    <span class="tag">ลองเอง</span><h4>เทียบค่าจริงกับ CLT approximation</h4>
    <p class="hint">ลองขยับ k ให้ห่างจาก np แล้วดูว่า error โตขึ้นอย่างไร</p>
    <div class="fields">
      <label for="cl-n">n<input id="cl-n" type="number" value="100" min="1" max="2000"></label>
      <label for="cl-p">p<input id="cl-p" type="number" value="0.3" step="0.01" min="0.01" max="0.99"></label>
      <label for="cl-k">k<input id="cl-k" type="number" value="30" min="0"></label>
      <label for="cl-s">n สำหรับ Stirling<input id="cl-s" type="number" value="10" min="1" max="170"></label>
    </div>
    <div class="out" id="cl-out"></div>
  </div>
</section>

<!-- ===================== 6.1 Estimators ===================== -->
<section class="ch" id="est">
  <div class="ch-head"><span class="lec">L14–15</span><span class="sec">Ch6.1 · Parameter estimators</span><h2>การประมาณค่าพารามิเตอร์</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p><b>Parameter</b> (m, σ², p, λ) คือค่าคงที่ของ population ที่เราไม่รู้ ส่วน <b>estimator</b> คือสูตรที่เอา data X₁, …, Xₙ มาคำนวณเพื่อเดาค่านั้น เพราะ data สุ่ม estimator จึงเป็น random variable ด้วย ทุกครั้งที่เก็บ data ใหม่จะได้คำตอบไม่เท่าเดิม</p>
    <p>เราตัดสินว่า estimator ดีไหมจาก 2 คุณสมบัติ</p>
    <ul>
      <li><b>Unbiased</b> = E[estimator] = parameter: ถ้าทำการทดลองซ้ำหลายรอบ ค่าเฉลี่ยของคำตอบตรงเป้า ไม่เอียงไปทางใดทางหนึ่ง</li>
      <li><b>Strongly consistent</b> = estimator → parameter เมื่อ n → ∞: ยิ่งเก็บ data มาก ยิ่งเข้าใกล้ค่าจริง (มาจาก strong law of large numbers)</li>
      <li><b>Asymptotically unbiased</b> = E[estimator] → parameter เมื่อ n → ∞: ตอน n น้อยอาจเอียง แต่ความเอียงหายไปเมื่อ n ใหญ่</li>
    </ul>
  </div>
  <div class="plate"><span class="tag">Sample mean (6.1) และ sample variance (6.2)–(6.3)</span>
  \[M_n:=\frac1n\sum_{i=1}^n X_i,\qquad S_n^2:=\frac{1}{n-1}\sum_{i=1}^n(X_i-M_n)^2=\frac{1}{n-1}\left[\left(\sum_{i=1}^n X_i^2\right)-nM_n^2\right]\]
  <p>Sample standard deviation คือ \(S_n=\sqrt{S_n^2}\) · สูตร (6.3) สะดวกตอนกดเครื่องคิดเลข เพราะไม่ต้องลบ Mₙ ทีละตัว</p></div>
  <div class="plate"><span class="tag">ทั้งคู่ unbiased และ strongly consistent (6.4)</span>
  \[E[M_n]=\frac1n\sum_{i=1}^n E[X_i]=\frac1n\cdot nm=m,\qquad \lim_{n\to\infty}M_n=m\quad\text{and}\quad\lim_{n\to\infty}S_n^2=\sigma^2\]</div>

  <div class="warn"><span class="tag">ทำไม Sₙ² หารด้วย n − 1</span>
    <p>Mₙ คำนวณจาก data ชุดเดียวกัน จึงถูกดึงเข้าหา data เสมอ ทำให้ Σ(Xᵢ − Mₙ)² เล็กกว่า Σ(Xᵢ − m)² โดยเฉลี่ย จัดรูปได้ว่า</p>
    \[\sum_{i=1}^n(X_i-M_n)^2=\sum_{i=1}^n(X_i-m)^2-n(M_n-m)^2\ \Rightarrow\ E\Big[\sum(X_i-M_n)^2\Big]=n\sigma^2-n\cdot\frac{\sigma^2}{n}=(n-1)\sigma^2\]
    <p>จึงต้องหารด้วย n − 1 เพื่อให้ได้ σ² พอดี (เสีย 1 degree of freedom ไปกับการประมาณ m)</p>
  </div>

  <div class="tw"><table class="t">
    <thead><tr><th>Example</th><th>Estimator</th><th>Unbiased?</th><th>Strongly consistent?</th></tr></thead>
    <tbody>
      <tr><td>6.1 รู้ m</td><td>\(\frac1n\sum(X_i-m)^2\)</td><td>ใช่</td><td>ใช่</td></tr>
      <tr><td>6.2 ไม่รู้ m</td><td>\(\widetilde S_n^2=\frac1n\sum(X_i-M_n)^2\)</td><td>ไม่ (asymptotically unbiased)</td><td>ใช่</td></tr>
      <tr><td>6.3 binomial(N,p)</td><td>\(p_n=M_n/N\)</td><td>ใช่</td><td>ใช่</td></tr>
      <tr><td>6.4 exp(λ)</td><td>\(\lambda_n=1/M_n\)</td><td>โจทย์ไม่ถาม (จริงๆ biased)</td><td>ใช่</td></tr>
    </tbody></table></div>

  <div class="ex">
    <h4><span class="no">Ex 6.1</span>รู้ mean แล้วประมาณ variance</h4>
    <div class="q"><b>โจทย์:</b> X₁, …, Xₙ i.i.d. รู้ mean m แต่ไม่รู้ variance σ² ตรวจว่า \(\frac1n\sum_{i=1}^n(X_i-m)^2\) เป็น unbiased estimator ของ σ² หรือไม่ และ strongly consistent หรือไม่</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>Unbiased:</b> ใช้นิยาม variance E[(Xᵢ − m)²] = σ² \[E\left[\frac1n\sum_{i=1}^n(X_i-m)^2\right]=\frac1n\sum_{i=1}^nE[(X_i-m)^2]=\frac1n\sum_{i=1}^n\sigma^2=\sigma^2\]</div></div>
      <div class="step"><span class="n">2</span><div><b>Consistent:</b> กระจายกำลังสอง \[\frac1n\sum(X_i-m)^2=\left(\frac1n\sum X_i^2\right)-2mM_n+m^2\]</div></div>
      <div class="step"><span class="n">3</span><div>Strong LLN: Mₙ → m และ Xᵢ² ก็ i.i.d. จึง \(\frac1n\sum X_i^2\to E[X_i^2]=\sigma^2+m^2\)</div></div>
      <div class="step"><span class="n">4</span><div>รวมกัน: \((\sigma^2+m^2)-2m^2+m^2=\sigma^2\) <span class="ans">unbiased และ strongly consistent</span></div></div>
    </div></details>
  </div>

  <div class="ex">
    <h4><span class="no">Ex 6.2</span>ไม่รู้ mean แต่หารด้วย n</h4>
    <div class="q"><b>โจทย์:</b> ไม่รู้ทั้ง m และ σ² จึงลองแทน m ด้วย Mₙ ได้ \(\widetilde S_n^2:=\frac1n\sum_{i=1}^n(X_i-M_n)^2\) ถามว่า unbiased และ strongly consistent หรือไม่</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>เทียบกับ Sₙ²: \(\widetilde S_n^2=\frac{n-1}{n}S_n^2\ \Rightarrow\ E[\widetilde S_n^2]=\frac{n-1}{n}\sigma^2\ne\sigma^2\) ⇒ <b>biased</b></div></div>
      <div class="step"><span class="n">2</span><div>แต่ \(\frac{n-1}{n}\to1\) ⇒ E → σ² ⇒ <b>asymptotically unbiased</b></div></div>
      <div class="step"><span class="n">3</span><div>\(\lim\widetilde S_n^2=\lim\frac{n-1}{n}\cdot\lim S_n^2=1\cdot\sigma^2\) <span class="ans">biased แต่ strongly consistent</span></div></div>
    </div></details>
  </div>

  <div class="ex">
    <h4><span class="no">Ex 6.3</span>ประมาณ p ของ binomial</h4>
    <div class="q"><b>โจทย์:</b> X₁, …, Xₙ i.i.d. binomial(N, p) รู้ N ไม่รู้ p จงหา estimator ของ p ที่ unbiased และ strongly consistent</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>เทคนิคหลัก:</b> เขียน parameter ในรูปของ E[Xᵢ] แล้วแทน E[Xᵢ] ด้วย Mₙ · binomial มี E[Xᵢ] = Np ⇒ p = E[Xᵢ]/N ⇒ เสนอ \(p_n=M_n/N\)</div></div>
      <div class="step"><span class="n">2</span><div>\(E[p_n]=E[M_n]/N=Np/N=p\) ⇒ unbiased · \(M_n\to Np\Rightarrow p_n\to p\) ⇒ consistent <span class="ans">\(p_n=M_n/N\)</span></div></div>
    </div></details>
  </div>

  <div class="ex">
    <h4><span class="no">Ex 6.4</span>ประมาณ λ ของ exponential</h4>
    <div class="q"><b>โจทย์:</b> X₁, …, Xₙ i.i.d. exp(λ) ไม่รู้ λ จงหา strongly consistent estimator ของ λ</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>E[Xᵢ] = 1/λ ⇒ λ = 1/E[Xᵢ] ⇒ เสนอ \(\lambda_n=1/M_n\)</div></div>
      <div class="step"><span class="n">2</span><div>\(\lim\lambda_n=\frac{1}{\lim M_n}=\frac{1}{E[X_i]}=\lambda\) <span class="ans">\(\lambda_n=1/M_n\) strongly consistent</span></div></div>
      <div class="step"><span class="n">3</span><div>โจทย์ไม่ถาม unbiased และจริงๆ ไม่ unbiased เพราะ E[1/Mₙ] ≠ 1/E[Mₙ] (ค่าคาดหวังไม่ผ่านฟังก์ชันไม่เชิงเส้น)</div></div>
    </div></details>
  </div>
</section>

<!-- ===================== 6.2 Histogram ===================== -->
<section class="ch" id="hist">
  <div class="ch-head"><span class="lec">L15</span><span class="sec">Ch6.2 · Histograms</span><h2>Histogram: ประมาณ pmf/pdf ทั้งตัว</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>Estimator ในหัวข้อก่อนเดาตัวเลขตัวเดียว ส่วน histogram เดา <b>รูปร่างการแจกแจงทั้งหมด</b> แบ่งแกนเป็นช่อง (bin) แล้วนับว่า data ตกช่องละกี่ตัว สัดส่วนที่ตกในช่อง ≈ ความน่าจะเป็นของช่องนั้น</p>
    <p>ทำไมใช้ได้: การนับคือการบวก indicator (1 ถ้าตกช่อง, 0 ถ้าไม่ตก) ซึ่งเป็น Bernoulli ดังนั้น Hⱼ/n คือ sample mean ของ Bernoulli จึง → ความน่าจะเป็นของช่องตาม law of large numbers</p>
  </div>
  <div class="plate"><span class="tag">นิยาม bin และ count</span>
  \[e_1&lt;e_2&lt;\cdots&lt;e_{m+1},\quad e_1\le\min_i X_i,\ \max_i X_i\le e_{m+1},\qquad H_j:=\sum_{i=1}^n I_{[e_j,\,e_{j+1})}(X_i)\]
  <p>m bins ต้องใช้ m + 1 edges (edge sequence) · bin สุดท้ายใช้ช่วงปิด [eₘ, eₘ₊₁] เพื่อไม่ให้ค่าสูงสุดหลุด</p></div>
  <div class="plate"><span class="tag">(6.5)–(6.6)</span>
  \[E\big[I_{[e_j,e_{j+1})}(X_i)\big]=P(e_j\le X_i&lt;e_{j+1}),\qquad \frac{H_j}{n}\approx P(e_j\le X_i&lt;e_{j+1})\]</div>
  <div class="two">
    <div><h4>Data เป็นจำนวนเต็ม</h4><p>ใช้ bin กลางที่จำนวนเต็ม eⱼ = j − ½, eⱼ₊₁ = j + ½ ความสูงแท่ง Hⱼ/n ≈ P(Xᵢ = j) เทียบกับ pmf ได้ตรงๆ</p></div>
    <div><h4>Data ต่อเนื่อง</h4><p>หารด้วยความกว้าง bin เพิ่ม (normalized histogram) เพื่อให้ความสูงเป็น density \(\frac{H_j}{n(e_{j+1}-e_j)}\approx f_X(x)\)</p></div>
  </div>
  <figure><div class="fbox"><svg id="fig-hist" role="img" aria-label="Histogram ของ data 1000 ค่าจาก binomial เทียบกับ pmf"></svg></div>
    <figcaption>Ex 6.5 จาก SageMath ในคาบ: แท่ง = Hⱼ/1000 จาก data จริง, จุด = pmf ของ binomial(10, 0.2989) ที่ใช้ p ประมาณจาก data แท่งกับจุดใกล้กันทุก bin จึงดูด้วยตาว่า fit ดี</figcaption></figure>

  <div class="ex">
    <h4><span class="no">Ex 6.5</span>Histogram ของ binomial (Figure 6.1)</h4>
    <div class="q"><b>โจทย์:</b> สุ่ม 1000 ค่าจาก binomial(10, 0.3) วาด normalized histogram แล้วซ้อน pmf binomial ที่ใช้ p ที่ประมาณจาก data</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ใช้ Ex 6.3: \(p_n=\frac{M_n}{N}=0.2989\) (N = 10, n = 1000)</div></div>
      <div class="step"><span class="n">2</span><div>Data เป็นจำนวนเต็ม 0–10 ⇒ bin กลางที่จำนวนเต็ม ⇒ ความสูง Hⱼ/n</div></div>
      <div class="step"><span class="n">3</span><div>ซ้อนจุด \(p_Y(k)=\binom{10}{k}(0.2989)^k(0.7011)^{10-k}\) การดูด้วยตายังเป็นความรู้สึก จึงต้องวัดด้วย chi-squared test ในหัวข้อถัดไป (Ex 6.7)</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.6</span>Histogram ของ exponential (Figure 6.2)</h4>
    <div class="q"><b>โจทย์:</b> มี measurement 1000 ค่า วาด normalized histogram แล้วเชื่อว่าเป็น exponential ที่ไม่รู้ λ ได้ Mₙ = 0.206</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Ex 6.4: \(\lambda_n=1/M_n=1/0.206\approx4.86\)</div></div>
      <div class="step"><span class="n">2</span><div>ซ้อนเส้น \(f_X(x)=4.86e^{-4.86x}\) บน normalized histogram (ความสูงแท่ง = Hⱼ / [n × ความกว้าง bin])</div></div>
      <div class="step"><span class="n">3</span><div>ตรวจหน่วยจาก Sage: bin แรกกว้าง ≈ 0.1503 มี 481 ค่า ⇒ ความสูง = 481/(1000 × 0.1503) ≈ 3.199 ใกล้กับค่าเฉลี่ยของ f ในช่องนั้น (1 − e^(−4.86×0.15))/0.15 ≈ 3.4 ไม่ใช่ f(0) = 4.86 เพราะแท่งแทนค่าเฉลี่ยทั้งช่อง</div></div>
    </div></details>
  </div>
</section>

<!-- ===================== L16 Chi-squared ===================== -->
<section class="ch" id="chi">
  <div class="ch-head"><span class="lec">L16</span><span class="sec">Ch6.2 · Goodness of fit</span><h2>Chi-squared test: model ที่เดา fit กับ data ไหม</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>เทียบ "นับได้จริง" Hⱼ กับ "ควรได้ถ้าสมมติฐานถูก" npⱼ ทีละ bin แล้วรวมความต่างกำลังสอง หารด้วย npⱼ เพื่อปรับสเกล (bin ที่คาดว่ามีน้อย ความต่างนิดเดียวก็มีความหมาย) ได้ค่า Z ตัวเดียว</p>
    <p><b>Z เล็ก = fit ดี, Z ใหญ่ = fit ไม่ดี</b> เส้นแบ่งคือ z_α ที่เลือกให้ ถ้า model ถูกจริง โอกาสที่ Z จะเกิน z_α มีแค่ α (เช่น 5%)</p>
  </div>
  <div class="plate"><span class="tag">กฎตัดสิน</span>
  \[\begin{cases}\text{Accepting hypothesis}, & Z\le z_\alpha\\ \text{Rejecting hypothesis}, & Z&gt;z_\alpha\end{cases}\qquad P(Z&gt;z_\alpha)=\alpha\]<p>α = significance level มักใช้ 1% หรือ 5%</p></div>

  <h3>ขั้นตอน 4 ขั้น</h3>
  <div class="steps" style="display:flex;flex-direction:column;gap:12px;max-width:74ch;margin-top:12px">
    <div class="step"><span class="n">1</span><div><b>หา pⱼ จาก model ที่สมมติ</b> · discrete: pⱼ = P(X = j) จาก pmf · continuous: ใช้ cdf \[p_j=P(e_j\le X_i&lt;e_{j+1})=\int_{e_j}^{e_{j+1}}f_X(x)\,dx=F_X(e_{j+1})-F_X(e_j)\] เช่น exp(λ): F_X(x) = 1 − e^(−λx)</div></div>
    <div class="step"><span class="n">2</span><div><b>คำนวณ Z</b> \[Z:=\sum_{j=1}^m\frac{|H_j-np_j|^2}{np_j}\]</div></div>
    <div class="step"><span class="n">3</span><div><b>หา degrees of freedom</b> \(k=m-1-r\) โดย m = จำนวน bins, r = จำนวน parameter ที่ประมาณจาก data (ลบ 1 เพราะ ΣHⱼ = n บังคับไว้แล้ว)</div></div>
    <div class="step"><span class="n">4</span><div><b>หา z_α แล้วเทียบ</b> \[P(Z&gt;z_\alpha)=\alpha=\int_{z_\alpha}^\infty f_Z(z)\,dz=1-F_Z(z_\alpha)\iff F_Z(z_\alpha)=1-\alpha\]</div></div>
  </div>
  <div class="plate"><span class="tag">Chi-squared density ที่ k degrees of freedom</span>
  \[f_Z(z)=\frac{1}{2^{k/2}\,\Gamma(k/2)}\,z^{k/2-1}e^{-z/2},\quad z&gt;0,\qquad \Gamma(1/2)=\sqrt\pi\]
  <p>(Hⱼ − npⱼ)/√(npⱼ) มี mean 0 และ variance 1 − pⱼ ⇒ E[Z] = m − 1 · ทุก bin ควรมี npⱼ ≥ 5</p></div>
  <figure><div class="fbox"><svg id="fig-chi" role="img" aria-label="density ของ chi-squared 7 degrees of freedom และ rejection region"></svg></div>
    <figcaption>χ² ที่ k = 7 (Ex 6.9 กรณี binomial): พื้นที่ส้มทางขวาของ z_α = 14.067 มีค่า 0.05 คือ rejection region · Z = 7.01 ที่คำนวณได้ตกในฝั่งยอมรับ</figcaption></figure>

  <div class="ex">
    <h4><span class="no">Ex 6.7</span>Z ของ binomial fit</h4>
    <div class="q"><b>โจทย์:</b> ใน Ex 6.5 ซ้อน pmf binomial(10, 0.2989) บน histogram จงคำนวณ statistic Z</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>pⱼ = \(p_Y(j)=\binom{10}{j}(0.2989)^j(0.7011)^{10-j}\) แล้วคำนวณ (Hⱼ − npⱼ)²/(npⱼ) ทีละ bin แบบตาราง Excel ด้านล่าง</div></div>
      <div class="step"><span class="n">2</span><div>รวมทุก bin <span class="ans">Z = 7.01</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.8</span>Z ของ exponential fit</h4>
    <div class="q"><b>โจทย์:</b> ใน Ex 6.6 fit exp(4.86) จงคำนวณ Z ภายใต้ hypothesis นี้</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Continuous ⇒ \[p_j=F_X(e_{j+1})-F_X(e_j)=e^{-4.86e_j}-e^{-4.86e_{j+1}}\]</div></div>
      <div class="step"><span class="n">2</span><div>แทนใน Z <span class="ans">Z = 8.33</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.9</span>หา z_α ที่ α = 0.05 แล้วสรุป</h4>
    <div class="q"><b>โจทย์:</b> หา z_α สำหรับ α = 0.05 ใน Ex 6.5 และ 6.6 แล้วสรุปว่า fit หรือไม่</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>Binomial:</b> Figure 6.1 มี m = 9 bins, ประมาณ p 1 ตัว (r = 1) ⇒ k = 9 − 1 − 1 = 7 ⇒ z_α = 14.067 · Z = 7.01 ≤ 14.067 <span class="ans">ยอมรับ: fit ดี</span></div></div>
      <div class="step"><span class="n">2</span><div><b>Exponential:</b> Figure 6.2 มี m = 15 bins, ประมาณ λ (r = 1) ⇒ k = 13 ⇒ z_α = 22.362 · Z = 8.33 <span class="ans">ยอมรับ: fit ดี</span></div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-chi">
    <span class="tag">ลองเอง</span><h4>คำนวณ Z ทีละ bin</h4>
    <p class="hint">ใส่ค่าจากตาราง Excel ในสไลด์ไว้ให้แล้ว (Mₙ = 3.037, pₙ ≈ 0.304, 11 bins) ใส่ตัวเลขคั่นด้วย comma</p>
    <div class="fields">
      <label class="wide" for="chi-h">Hⱼ (นับได้จริง)<textarea id="chi-h">26, 111, 222, 284, 210, 97, 46, 4, 0, 0, 0</textarea></label>
      <label class="wide" for="chi-np">npⱼ (ค่าคาดหวัง) หรือ pⱼ ก็ได้ ระบบจะคูณ n ให้ถ้ารวมกันได้ 1<textarea id="chi-np">26.828, 116.958, 229.449, 266.746, 203.507, 106.464, 38.678, 9.635, 1.575, 0.153, 0.007</textarea></label>
      <label for="chi-r">r (parameter ที่ประมาณ)<input id="chi-r" type="number" value="1" min="0"></label>
      <label for="chi-a">α<input id="chi-a" type="number" value="0.05" step="0.01" min="0.001" max="0.5"></label>
    </div>
    <div class="tw"><table class="t" id="chi-tab"></table></div>
    <div class="out" id="chi-out"></div>
    <div id="chi-v"></div>
  </div>

  <h3>Table 6.1 · z_α ของ χ²</h3>
  <div class="tw"><table class="t" id="tab61"></table></div>
</section>

<!-- ===================== L17 CI mean ===================== -->
<section class="ch" id="cimean">
  <div class="ch-head"><span class="lec">L17</span><span class="sec">Ch6.3–6.5 · Confidence intervals</span><h2>Confidence interval ของ mean</h2></div>
  <p class="xref">ออกสอบจริง: <a href="#exam-q1">ข้อสอบ Final 2564 ข้อ 1</a></p>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>Mₙ ไม่เท่ากับ m พอดี แต่แกว่งอยู่รอบ m ด้วยความกว้าง (standard deviation) \(\sigma/\sqrt n\) เราจึงตอบเป็นช่วง \(M_n\pm\delta\) แล้วเลือก δ ให้ช่วงนี้ "ครอบ" m ได้ 100(1−α)% ของครั้งที่ทำการทดลองซ้ำ</p>
    <p>ระวังการตีความ: <b>m เป็นค่าคงที่ ช่วงต่างหากที่สุ่ม</b> ทุกครั้งที่เก็บ data ใหม่ ได้ Mₙ ใหม่ ช่วงก็ขยับ 95% ของช่วงเหล่านั้นจะครอบ m</p>
  </div>
  <div class="plate"><span class="tag">(6.7)–(6.8)</span>
  \[P\big(m\in[M_n-\delta,\,M_n+\delta]\big)=1-\alpha\iff P(|M_n-m|\le\delta)=1-\alpha\]
  <p>[Mₙ − δ, Mₙ + δ] = confidence interval · 1 − α = confidence level</p></div>

  <h3>6.3 รู้ σ: ใช้ CLT</h3>
  <div class="prose"><p>ตั้ง δ = σy/√n แล้วหารทั้งสองข้างใน |Mₙ − m| ≤ δ ด้วย σ/√n ได้ตัวแปรมาตรฐาน Yₙ ซึ่ง CLT บอกว่า ≈ N(0,1)</p></div>
  <div class="plate"><span class="tag">(6.10)</span>
  \[Y_n:=\frac{M_n-m}{\sigma/\sqrt n},\qquad P\left(\left|\frac{M_n-m}{\sigma/\sqrt n}\right|\le y\right)\approx\Phi(y)-\Phi(-y)=2\Phi(y)-1\]</div>
  <div class="plate"><span class="tag">ตั้งให้เท่ากับ 1 − α แล้วแก้หา y</span>
  \[1-\alpha=2\Phi(y)-1\iff\Phi(y_{\alpha/2})=1-\alpha/2\]</div>
  <figure><div class="fbox"><svg id="fig-ci" role="img" aria-label="normal density แสดงพื้นที่ตรงกลาง 1-alpha และหางสองข้าง"></svg></div>
    <figcaption>ที่ 1 − α = 95%: พื้นที่ตรงกลางระหว่าง −1.96 ถึง 1.96 = 0.95 เหลือหางละ α/2 = 0.025 จึงหา y จาก Φ(y) = 0.975 ไม่ใช่ 0.95</figcaption></figure>
  <div class="plate"><span class="tag">คำตอบ (6.11)–(6.12)</span>
  \[m=M_n\pm\frac{\sigma y_{\alpha/2}}{\sqrt n}\ \text{with }100(1-\alpha)\%\text{ probability},\qquad\left[M_n-\frac{\sigma y_{\alpha/2}}{\sqrt n},\ M_n+\frac{\sigma y_{\alpha/2}}{\sqrt n}\right]\]
  <p>ความกว้างของช่วง = 2σy/√n ⇒ อยากได้ช่วงแคบลงครึ่งหนึ่ง ต้องเพิ่ม n เป็น 4 เท่า</p></div>

  <h3>6.4 ไม่รู้ σ, n ใหญ่: แทนด้วย Sₙ</h3>
  <div class="plate"><span class="tag">(6.16)–(6.17)</span>
  \[m=M_n\pm\frac{S_n y_{\alpha/2}}{\sqrt n},\qquad\left[M_n-\frac{S_n y_{\alpha/2}}{\sqrt n},\ M_n+\frac{S_n y_{\alpha/2}}{\sqrt n}\right]\]
  <p>ใช้ได้เพราะ Sₙ → σ เมื่อ n ใหญ่ (Sₙ/σ → 1) ยังเปิดตาราง normal เหมือนเดิม</p></div>

  <h3>6.5 Data เป็น Gaussian</h3>
  <div class="two">
    <div><h4>รู้ σ</h4><p>Yₙ ~ N(0,1) <b>พอดี</b> ไม่ใช่แค่ประมาณ ใช้สูตร (6.11) ได้แม้ n เล็ก</p></div>
    <div><h4>ไม่รู้ σ</h4><p>T ไม่ใช่ normal แต่เป็น <b>Student t</b> ที่ ν = n − 1 ซึ่งหางหนากว่า เพื่อชดเชยความไม่แน่นอนจากการใช้ Sₙ</p></div>
  </div>
  <div class="plate"><span class="tag">Student t</span>
  \[T:=\frac{M_n-m}{S_n/\sqrt n},\qquad f(t)=\frac{\Gamma\!\left(\frac{\nu+1}{2}\right)}{\sqrt{\nu\pi}\,\Gamma\!\left(\frac{\nu}{2}\right)}\left(1+\frac{t^2}{\nu}\right)^{-\frac{\nu+1}{2}}\]
  \[P(|T|\le y)=F_T(y)-F_T(-y)=2F_T(y)-1=1-\alpha\iff F_T(y)=1-\alpha/2\]</div>

  <div class="tw"><table class="t">
    <thead><tr><th>1 − α</th><th class="num">y_α/2 normal (Table 6.2)</th><th class="num">t, n = 10 (Table 6.3)</th><th class="num">t, n = 100</th></tr></thead>
    <tbody>
      <tr><td>0.90</td><td class="num">1.645</td><td class="num">1.833</td><td class="num">1.660</td></tr>
      <tr><td>0.91</td><td class="num">1.695</td><td class="num">1.899</td><td class="num">1.712</td></tr>
      <tr><td>0.92</td><td class="num">1.751</td><td class="num">1.973</td><td class="num">1.769</td></tr>
      <tr><td>0.93</td><td class="num">1.812</td><td class="num">2.055</td><td class="num">1.832</td></tr>
      <tr><td>0.94</td><td class="num">1.881</td><td class="num">2.150</td><td class="num">1.903</td></tr>
      <tr class="hl"><td>0.95</td><td class="num">1.960</td><td class="num">2.262</td><td class="num">1.984</td></tr>
      <tr><td>0.96</td><td class="num">2.054</td><td class="num">2.398</td><td class="num">2.081</td></tr>
      <tr><td>0.97</td><td class="num">2.170</td><td class="num">2.574</td><td class="num">2.202</td></tr>
      <tr><td>0.98</td><td class="num">2.326</td><td class="num">2.821</td><td class="num">2.365</td></tr>
      <tr><td>0.99</td><td class="num">2.576</td><td class="num">3.250</td><td class="num">2.626</td></tr>
    </tbody></table></div>

  <div class="ex">
    <h4><span class="no">Ex 6.10</span>รู้ variance: CI 93% และ 97%</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. มี variance σ² = 2 ถ้า M₁₀₀ = 7.129 หา confidence interval 93% และ 97% ของ population mean</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>93%: Table 6.2 ได้ y = 1.812 \[\delta=\frac{1.812\sqrt2}{\sqrt{100}}=0.256\ \Rightarrow\ 7.129\pm0.256\] <span class="ans">[6.873, 7.385]</span></div></div>
      <div class="step"><span class="n">2</span><div>97%: y = 2.170 ⇒ δ = 0.307 <span class="ans">[6.822, 7.436]</span></div></div>
      <div class="step"><span class="n">3</span><div>สังเกต: มั่นใจมากขึ้น ⇒ ช่วงกว้างขึ้น</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.11</span>ไม่รู้ variance: Bernoulli</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. Bernoulli(p) หา CI 95% ของ p ถ้า M₁₀₀ = 0.28 และ S₁₀₀ = 0.451</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>m = E[Xᵢ] = p จึงหา CI ของ m ก็คือ CI ของ p · ไม่รู้ σ และ n = 100 ใหญ่ ⇒ สูตร (6.16)</div></div>
      <div class="step"><span class="n">2</span><div>\(\delta=\frac{0.451\times1.960}{\sqrt{100}}=0.088\) <span class="ans">p = 0.28 ± 0.088 → [0.192, 0.368]</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Election</span>ทำนายผลเลือกตั้ง</h4>
    <div class="q"><b>โจทย์:</b> สุ่มสำรวจผู้มีสิทธิ์ 4000 คน มี 2104 คนบอกว่าจะเลือก A ที่เหลือเลือก B ให้ p = สัดส่วนคะแนนจริงของ A จากผู้มีสิทธิ์หลายล้านคน สมมติ S₄₀₀₀ = 0.499 หา CI 95% ของ p</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Xᵢ = 1 ถ้าคนที่ i เลือก A ⇒ \(M_{4000}=\frac{2104}{4000}=0.526\)</div></div>
      <div class="step"><span class="n">2</span><div>\(\delta=\frac{0.499\times1.960}{\sqrt{4000}}=0.015\) <span class="ans">p = 0.526 ± 0.015</span></div></div>
      <div class="step"><span class="n">3</span><div>ขอบล่าง 0.511 &gt; 0.5 ⇒ มั่นใจ 95% ว่า A ชนะ (ข่าวจะรายงานว่า A ได้ 53% margin of error 2%)</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.12</span>Gaussian, รู้ variance (n เล็กก็ได้)</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. N(m, 2) ถ้า M₁₀ = 5.287 หา CI 90% ของ m</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>Gaussian ⇒ Yₙ ~ N(0,1) พอดี ไม่ต้องรอ n ใหญ่ · 90% ⇒ y = 1.645</div></div>
      <div class="step"><span class="n">2</span><div>\(\delta=\frac{1.645\sqrt2}{\sqrt{10}}=0.736\) <span class="ans">[4.551, 6.023]</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.13</span>Gaussian, ไม่รู้ variance (Student t)</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. N(m, σ²), M₁₀ = 5.287 และ S₁₀ = 1.564 หา CI 90% ของ m</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ไม่รู้ σ และ n = 10 เล็ก ⇒ T เป็น Student t ที่ ν = 9 · t เป็นฟังก์ชันคู่ (even) จึงได้ F_T(y) = 1 − α/2 = 0.95</div></div>
      <div class="step"><span class="n">2</span><div>Table 6.3 (n = 10) ⇒ y = 1.833 ⇒ \(\delta=\frac{1.564\times1.833}{\sqrt{10}}=0.907\) <span class="ans">[4.380, 6.194]</span></div></div>
      <div class="step"><span class="n">3</span><div>เทียบ Ex 6.12: Mₙ เท่ากันแต่ช่วงกว้างกว่า เพราะ 1.833 &gt; 1.645 (t หางหนากว่า normal)</div></div>
    </div></details>
  </div>
</section>

<!-- ===================== L18–19 CI variance ===================== -->
<section class="ch" id="civar">
  <div class="ch-head"><span class="lec">L18–19</span><span class="sec">Ch6.5 · Gaussian data</span><h2>Confidence interval ของ variance</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>ทำเหมือน CI ของ mean คือแปลง estimator ให้เป็นตัวแปรที่รู้การแจกแจง แต่คราวนี้เป็น <b>χ²</b> แทน normal เพราะผลรวมของ N(0,1) ยกกำลังสอง n ตัวคือ χ² ที่ n degrees of freedom</p>
    <p>χ² ไม่สมมาตร จึงต้องหาขอบสองตัว ℓ (หางซ้ายมีพื้นที่ α/2) และ u (หางขวามีพื้นที่ α/2) แยกกัน ช่วงที่ได้จึงไม่สมมาตรรอบค่าประมาณ</p>
  </div>
  <div class="plate"><span class="tag">ขอบทั้งสอง (F = χ² cdf)</span>
  \[F(\ell)=\alpha/2\quad\text{and}\quad F(u)=1-\alpha/2\]</div>

  <h3>รู้ mean m</h3>
  <div class="plate"><span class="tag">(6.18)</span>
  \[V_n^2:=\frac1n\sum_{i=1}^n(X_i-m)^2,\qquad\frac{n}{\sigma^2}V_n^2=\sum_{i=1}^n\left(\frac{X_i-m}{\sigma}\right)^2\sim\chi^2(n)\]</div>
  <div class="plate"><span class="tag">กลับอสมการ → (6.19)</span>
  \[P\left(\ell\le\frac{n}{\sigma^2}V_n^2\le u\right)=1-\alpha\iff P\left(\frac{nV_n^2}{\ell}\ge\sigma^2\ge\frac{nV_n^2}{u}\right)=1-\alpha\]
  \[\sigma^2\in\left[\frac{nV_n^2}{u},\ \frac{nV_n^2}{\ell}\right]\]
  <p>สังเกต u (ค่าใหญ่) ไปอยู่ใต้ <b>ขอบล่าง</b> เพราะกลับเศษเป็นส่วน</p></div>

  <h3>ไม่รู้ mean</h3>
  <div class="plate"><span class="tag">(6.20)</span>
  \[\frac{n-1}{\sigma^2}S_n^2\sim\chi^2(n-1),\qquad\sigma^2\in\left[\frac{(n-1)S_n^2}{u},\ \frac{(n-1)S_n^2}{\ell}\right]\]
  <p>เสีย 1 degree of freedom เพราะใช้ Mₙ แทน m (เหตุผลเดียวกับที่ Sₙ² หาร n − 1)</p></div>
  <figure><div class="fbox"><svg id="fig-civar" role="img" aria-label="chi-squared 99 degrees of freedom กับขอบ l และ u"></svg></div>
    <figcaption>Ex 6.15: χ² ที่ 99 degrees of freedom, 90% ⇒ ℓ = 77.046 และ u = 123.225 หางซ้ายและขวามีพื้นที่ 0.05 เท่ากัน แต่ระยะจากกึ่งกลางไม่เท่ากัน</figcaption></figure>

  <div class="plate"><span class="tag">Student T R.V. มาจากไหน</span>
  \[T=\frac{(M_n-m)/(\sigma/\sqrt n)}{\sqrt{\dfrac{n-1}{\sigma^2}S_n^2\Big/(n-1)}}=\frac{M_n-m}{S_n/\sqrt n}\]
  <p>ตัวเศษเป็น N(0,1) ตัวส่วนมี χ²(n − 1) และสองส่วนเป็น independent เมื่อ Xᵢ i.i.d. N(m, σ²) ⇒ T เป็น Student t ที่ ν = n − 1 และ σ ตัดกันหายไป</p></div>

  <div class="tw"><table class="t">
    <thead><tr><th>1 − α</th><th class="num">ℓ (Table 6.4, 100 dof)</th><th class="num">u</th><th class="num">ℓ (Table 6.5, 99 dof)</th><th class="num">u</th></tr></thead>
    <tbody>
      <tr class="hl"><td>0.90</td><td class="num">77.929</td><td class="num">124.342</td><td class="num">77.046</td><td class="num">123.225</td></tr>
      <tr><td>0.91</td><td class="num">77.326</td><td class="num">125.170</td><td class="num">76.447</td><td class="num">124.049</td></tr>
      <tr><td>0.92</td><td class="num">76.671</td><td class="num">126.079</td><td class="num">75.795</td><td class="num">124.955</td></tr>
      <tr><td>0.93</td><td class="num">75.949</td><td class="num">127.092</td><td class="num">75.077</td><td class="num">125.963</td></tr>
      <tr><td>0.94</td><td class="num">75.142</td><td class="num">128.237</td><td class="num">74.275</td><td class="num">127.103</td></tr>
      <tr><td>0.95</td><td class="num">74.222</td><td class="num">129.561</td><td class="num">73.361</td><td class="num">128.422</td></tr>
      <tr><td>0.96</td><td class="num">73.142</td><td class="num">131.142</td><td class="num">72.288</td><td class="num">129.996</td></tr>
      <tr><td>0.97</td><td class="num">71.818</td><td class="num">133.120</td><td class="num">70.972</td><td class="num">131.966</td></tr>
      <tr><td>0.98</td><td class="num">70.065</td><td class="num">135.807</td><td class="num">69.230</td><td class="num">134.642</td></tr>
      <tr><td>0.99</td><td class="num">67.328</td><td class="num">140.169</td><td class="num">66.510</td><td class="num">138.987</td></tr>
    </tbody></table></div>

  <div class="ex">
    <h4><span class="no">Ex 6.14</span>รู้ mean</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. N(5, σ²) สมมติ V²₁₀₀ = 1.645 หา CI 90% ของ σ²</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>รู้ m = 5 ⇒ χ² ที่ n = 100 dof · 90% ⇒ Table 6.4: ℓ = 77.929, u = 124.342</div></div>
      <div class="step"><span class="n">2</span><div>\[\left[\frac{100(1.645)}{124.342},\ \frac{100(1.645)}{77.929}\right]\] <span class="ans">[1.323, 2.111]</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.15</span>ไม่รู้ mean</h4>
    <div class="q"><b>โจทย์:</b> X₁, X₂, … i.i.d. N(m, σ²) ไม่รู้ทั้ง m และ σ² ถ้า S²₁₀₀ = 1.608 หา CI 90% ของ σ²</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ไม่รู้ m ⇒ χ² ที่ n − 1 = 99 dof · Table 6.5: ℓ = 77.046, u = 123.225</div></div>
      <div class="step"><span class="n">2</span><div>\[\left[\frac{99(1.608)}{123.225},\ \frac{99(1.608)}{77.046}\right]\] <span class="ans">[1.292, 2.067]</span></div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-ci">
    <span class="tag">ลองเอง</span><h4>เครื่องคิด confidence interval</h4>
    <p class="hint">ค่าเริ่มต้นคือ Ex 6.13 · ค่า y, ℓ, u คำนวณจาก inverse cdf จริง ไม่ต้องเปิดตาราง</p>
    <div class="seg" role="group" aria-label="ชนิดของ CI">
      <button type="button" id="ci-m-btn" aria-pressed="true">CI ของ mean</button>
      <button type="button" id="ci-v-btn" aria-pressed="false">CI ของ variance</button>
    </div>
    <div id="ci-m">
      <div class="fields">
        <label class="wide" for="ci-case">กรณี<select id="ci-case">
          <option value="z">รู้ σ (normal)</option>
          <option value="zs">ไม่รู้ σ, n ใหญ่ (ใช้ Sₙ, normal)</option>
          <option value="t" selected>Gaussian, ไม่รู้ σ (Student t, ν = n − 1)</option>
        </select></label>
        <label for="ci-mn">Mₙ<input id="ci-mn" type="number" value="5.287" step="any"></label>
        <label for="ci-s">σ หรือ Sₙ<input id="ci-s" type="number" value="1.564" step="any" min="0"></label>
        <label for="ci-n">n<input id="ci-n" type="number" value="10" min="2"></label>
        <label for="ci-c">1 − α (%)<input id="ci-c" type="number" value="90" min="50" max="99.9" step="0.1"></label>
      </div>
    </div>
    <div id="ci-v" hidden>
      <div class="fields">
        <label class="wide" for="cv-case">กรณี<select id="cv-case">
          <option value="known">รู้ mean (ใส่ Vₙ², χ² ที่ n dof)</option>
          <option value="unknown" selected>ไม่รู้ mean (ใส่ Sₙ², χ² ที่ n − 1 dof)</option>
        </select></label>
        <label for="cv-s2">Vₙ² หรือ Sₙ²<input id="cv-s2" type="number" value="1.608" step="any" min="0"></label>
        <label for="cv-n">n<input id="cv-n" type="number" value="100" min="2"></label>
        <label for="cv-c">1 − α (%)<input id="cv-c" type="number" value="90" min="50" max="99.9" step="0.1"></label>
      </div>
    </div>
    <div class="out" id="ci-out"></div>
  </div>
</section>

<!-- ===================== L19–20 Hypothesis test ===================== -->
<section class="ch" id="test">
  <div class="ch-head"><span class="lec">L19–20</span><span class="sec">Ch6.6 · Hypothesis tests for the mean</span><h2>Hypothesis test ของ mean</h2></div>
  <p class="xref">ออกสอบจริง: <a href="#exam-q2">ข้อสอบ Final 2564 ข้อ 2</a></p>
  <div class="idea">
    <span class="tag">เข้าใจก่อน: เหมือนศาล</span>
    <p><b>H₀ (null hypothesis)</b> = ฝ่ายที่ได้ benefit of the doubt เหมือนจำเลยที่ถือว่าบริสุทธิ์ไว้ก่อน · <b>H₁ (alternative hypothesis)</b> = ฝ่ายที่มีภาระพิสูจน์ (burden of proof) ต้องมีหลักฐานหนักแน่นจึงจะเชื่อ</p>
    <p>จำง่าย: <b>claim ของใคร คนนั้นต้องพิสูจน์ ⇒ claim อยู่ใน H₁</b> เราปฏิเสธ H₀ ก็ต่อเมื่อ data ค้านมันแรงมาก (Zₙ ตกใน rejection region)</p>
  </div>
  <div class="plate"><span class="tag">Decision rules (6.21)–(6.23), δ &gt; 0</span>
  \[\begin{aligned}&(1)\ H_0: m\le m_0\ \text{vs}\ H_1: m&gt;m_0, &&\text{accept }H_0\text{ if }M_n\le m_0+\delta\\ &(2)\ H_0: m&gt;m_0\ \text{vs}\ H_1: m\le m_0, &&\text{accept }H_0\text{ if }M_n&gt;m_0-\delta\\ &(3)\ H_0: m=m_0\ \text{vs}\ H_1: m\ne m_0, &&\text{accept }H_0\text{ if }|M_n-m_0|\le\delta\end{aligned}\]</div>
  <div class="plate"><span class="tag">Statistic (6.24) รู้ σ · (6.25) ไม่รู้ σ</span>
  \[Z_n=\frac{M_n-m_0}{\sigma/\sqrt n}\qquad\qquad Z_n=\frac{M_n-m_0}{S_n/\sqrt n}\]
  <p>หาร δ ด้วย σ/√n (หรือ Sₙ/√n) ทำให้กฎทั้ง 3 กลายเป็นการดูว่า Zₙ อยู่ในช่วงไหน</p></div>
  <div class="plate"><span class="tag">Acceptance regions</span>
  \[(1)\ Z_n\le y\iff Z_n\in(-\infty,y],\qquad(2)\ Z_n&gt;-y\iff Z_n\in(-y,\infty),\qquad(3)\ |Z_n|\le y\iff Z_n\in[-y,y]\]</div>
  <figure><div class="fbox"><svg id="fig-test" role="img" aria-label="rejection region ของ hypothesis test ทั้งสามแบบ"></svg></div>
    <figcaption>พื้นที่ส้ม = rejection (critical) region ที่ α = 0.05 · แบบ 1 หางขวา (y_α = 1.645) · แบบ 2 หางซ้าย (−y_α) · แบบ 3 สองหาง หางละ α/2 (y_α/2 = 1.960)</figcaption></figure>

  <h3>หา critical value</h3>
  <div class="plate"><span class="tag">แบบ 3 · two-tailed (6.26)</span>
  \[P(|Z_n|&gt;y)\approx1-[\Phi(y)-\Phi(-y)]=2[1-\Phi(y)]=\alpha\iff\Phi(y_{\alpha/2})=1-\alpha/2\]</div>
  <div class="plate"><span class="tag">แบบ 1 · one-tailed (6.27)</span>
  \[Z_n=\frac{M_n-m}{S_n/\sqrt n}+\frac{m-m_0}{S_n/\sqrt n},\qquad P(Z_n&gt;y)\le P\!\left(\frac{M_n-m}{S_n/\sqrt n}&gt;y\right)\approx1-\Phi(y)\]
  \[1-\Phi(y_\alpha)=\alpha\iff\Phi(y_\alpha)=1-\alpha\]
  <p>เมื่อ H₀ จริง (m ≤ m₀) เทอมหลังติดลบหรือศูนย์ จึงได้อสมการ ≤ · แบบ 2 ใช้ −y_α แบบสะท้อนกลับ</p></div>
  <div class="tw"><table class="t">
    <thead><tr><th class="num">α</th><th class="num">y_α/2 (two-tailed)</th><th class="num">y_α (one-tailed)</th></tr></thead>
    <tbody>
      <tr><td class="num">0.01</td><td class="num">2.576</td><td class="num">2.326</td></tr>
      <tr><td class="num">0.02</td><td class="num">2.326</td><td class="num">2.054</td></tr>
      <tr><td class="num">0.03</td><td class="num">2.170</td><td class="num">1.881</td></tr>
      <tr><td class="num">0.04</td><td class="num">2.054</td><td class="num">1.751</td></tr>
      <tr class="hl"><td class="num">0.05</td><td class="num">1.960</td><td class="num">1.645</td></tr>
      <tr><td class="num">0.06</td><td class="num">1.881</td><td class="num">1.555</td></tr>
      <tr><td class="num">0.07</td><td class="num">1.812</td><td class="num">1.476</td></tr>
      <tr><td class="num">0.08</td><td class="num">1.751</td><td class="num">1.405</td></tr>
      <tr><td class="num">0.09</td><td class="num">1.695</td><td class="num">1.341</td></tr>
      <tr><td class="num">0.10</td><td class="num">1.645</td><td class="num">1.282</td></tr>
    </tbody></table></div>

  <h3>Types of errors</h3>
  <div class="tw"><table class="t">
    <thead><tr><th></th><th>ความจริง: H₀ ถูก</th><th>ความจริง: H₁ ถูก</th></tr></thead>
    <tbody>
      <tr><td><b>ตัดสิน: ยอมรับ H₀</b></td><td>ถูกต้อง</td><td><b>Type II error</b> (มักเรียกความน่าจะเป็นว่า β)</td></tr>
      <tr><td><b>ตัดสิน: ปฏิเสธ H₀ (เชื่อ H₁)</b></td><td><b>Type I error</b> ความน่าจะเป็น ≤ α</td><td>ถูกต้อง</td></tr>
    </tbody></table></div>
  <div class="plate">\[P(\text{Type I Error})=P(H_1\text{ is declared}\mid H_0\text{ is True})=\alpha\]
  <p>สไลด์ L19 เขียน P(Type II) = α ด้วย ซึ่งน่าจะพิมพ์ผิด α ควบคุมแค่ Type I</p></div>

  <div class="ex">
    <h4><span class="no">Ex 6.16</span>แปลง claim เป็นสมมติฐาน</h4>
    <div class="q"><b>โจทย์:</b> ผู้ผลิตดาวเทียมสื่อสารอ้างว่าดาวเทียมใหม่มี bit-error probability ไม่เกิน p₀ จงเขียนเป็น hypothesis test</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ให้ Xᵢ = 1 ถ้า bit ที่ i รับผิด, 0 ถ้าถูก ⇒ Bernoulli ⇒ \(p:=P(X_i=1)=E[X_i]\)</div></div>
      <div class="step"><span class="n">2</span><div>Claim คือ p ≤ p₀ อีกทางคือ p &gt; p₀ ⇒ อยู่ในรูป m ≤ m₀ vs m &gt; m₀</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.17</span>รัฐบาลจะซื้อดาวเทียม</h4>
    <div class="q"><b>โจทย์:</b> ต่อจาก Ex 6.16 ถ้ารัฐบาลกำลังพิจารณาจะซื้อ ควรตั้ง H₀ และ H₁ อย่างไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>รัฐบาลให้ภาระพิสูจน์อยู่ที่ผู้ผลิต (เชื่อไว้ก่อนว่าดาวเทียมแย่)</div></div>
      <div class="step"><span class="n">2</span><div><span class="ans">H₀: p &gt; p₀ · H₁: p ≤ p₀</span> data ต้องค้าน H₀ แรงพอจึงจะเชื่อ claim</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 6.18</span>Zener diodes</h4>
    <div class="q"><b>โจทย์:</b> ผู้ผลิตอ้างว่าแรงดัน reverse-biased ของ Zener diode ไม่เกิน m₀ คุณได้ diode n ตัว แรงดัน X₁, …, Xₙ i.i.d. mean m (a) ใช้ Zₙ แบบ (6.24) หรือ (6.25) (b) ถ้าภาระพิสูจน์อยู่ที่ผู้ผลิต H₀, H₁ คืออะไร (c) ที่ α = 0.05 critical value และ critical region คืออะไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">a</span><div>โจทย์ไม่ให้ variance ⇒ (6.25) \(Z_n=\frac{M_n-m_0}{S_n/\sqrt n}\)</div></div>
      <div class="step"><span class="n">b</span><div>ผู้ผลิต claim m ≤ m₀ และต้องพิสูจน์ ⇒ <span class="ans">H₁: m ≤ m₀, H₀: m &gt; m₀</span> (แบบ 2)</div></div>
      <div class="step"><span class="n">c</span><div>แบบ 2 ยอมรับ H₀ เมื่อ Zₙ &gt; −y_α · y_α = 1.645 ⇒ acceptance (−1.645, ∞) <span class="ans">critical region (−∞, −1.645]</span></div></div>
      <div class="step"><span class="n">*</span><div>Simulation ในคาบ: m จริง 1.5, σ = 0.01, n = 200, m₀ = 1.6 ได้ Zₙ = −140.3 ตกใน critical region ⇒ ปฏิเสธ H₀ ⇒ เชื่อ claim ของผู้ผลิต</div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-test">
    <span class="tag">ลองเอง</span><h4>ทดสอบ mean</h4>
    <p class="hint">ค่าเริ่มต้นคือ Ex 6.18 แบบสมมติตัวเลข (Mₙ = 1.5, m₀ = 1.6, Sₙ = 0.01, n = 200)</p>
    <div class="fields">
      <label class="wide" for="ht-type">รูปแบบ<select id="ht-type">
        <option value="1">(1) H₀: m ≤ m₀ vs H₁: m &gt; m₀</option>
        <option value="2" selected>(2) H₀: m &gt; m₀ vs H₁: m ≤ m₀</option>
        <option value="3">(3) H₀: m = m₀ vs H₁: m ≠ m₀</option>
      </select></label>
      <label for="ht-mn">Mₙ<input id="ht-mn" type="number" value="1.5" step="any"></label>
      <label for="ht-m0">m₀<input id="ht-m0" type="number" value="1.6" step="any"></label>
      <label for="ht-s">σ หรือ Sₙ<input id="ht-s" type="number" value="0.01" step="any" min="0"></label>
      <label for="ht-n">n<input id="ht-n" type="number" value="200" min="2"></label>
      <label for="ht-a">α<input id="ht-a" type="number" value="0.05" step="0.01" min="0.001" max="0.5"></label>
    </div>
    <div class="out" id="ht-out"></div>
    <div id="ht-v"></div>
  </div>
</section>

<!-- ===================== L20–21 Regression ===================== -->
<section class="ch" id="reg">
  <div class="ch-head"><span class="lec">L20–21</span><span class="sec">Ch6.7 · Regression and curve fitting</span><h2>Regression: ลากเส้นที่ดีที่สุดผ่าน data</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>วัด output Yₖ ที่ input xₖ หลายจุด จุดไม่อยู่บนเส้นพอดีเพราะ noise (model เป็นแค่การประมาณ + measurement error) เราเลือกเส้น g ที่ทำให้ <b>ผลรวมระยะห่างแนวตั้งยกกำลังสอง</b> น้อยที่สุด เรียกว่า least squares</p>
    <p>Ex 6.19: ตัวต้านทาน v = iR ป้อนกระแสหลายค่า วัดแรงดัน แล้ว fit เส้นตรง ความชันที่ได้คือค่าประมาณของ R</p>
  </div>
  <div class="plate"><span class="tag">Model (6.29) และ error (6.30)</span>
  \[Y_k=g(x_k)+W_k,\quad k=1,\dots,n,\qquad e(g):=\sum_{k=1}^n|Y_k-g(x_k)|^2\]
  <p>Wₖ = noise ที่มี zero mean · g ที่ได้เรียกว่า regression curve ของ Y บน x</p></div>

  <h3>Ex 6.21 · Linear regression g(x) = ax + b</h3>
  <div class="ex" style="border-top:0;padding-top:0">
    <div class="q"><b>โจทย์:</b> หา a, b ที่ทำให้ \(e(a,b)=\sum_{k=1}^n|Y_k-[ax_k+b]|^2\) น้อยที่สุด</div>
    <details class="sol" open><summary>วิธีทำ (partial derivative)</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>\[\frac{\partial e}{\partial b}=-2\sum_{k=1}^n(Y_k-ax_k-b)=0\ \Rightarrow\ b=\overline Y-a\overline x\]</div></div>
      <div class="step"><span class="n">2</span><div>ดิฟเทียบ a แล้วแทน b จากข้อ 1 \[\frac{\partial e}{\partial a}=-2\sum_{k=1}^n x_k(Y_k-ax_k-b)=0\ \Rightarrow\ \sum x_k(Y_k-\overline Y)=a\sum x_k(x_k-\overline x)\]</div></div>
      <div class="step"><span class="n">3</span><div>เพราะ \(\sum\overline x(Y_k-\overline Y)=0\) และ \(\sum\overline x(x_k-\overline x)=0\) จึงลบเข้าไปได้ \[a=\frac{S_{xY}}{S_{xx}},\quad b=\overline Y-a\overline x,\quad S_{xx}:=\sum_{k=1}^n(x_k-\overline x)^2,\quad S_{xY}:=\sum_{k=1}^n(x_k-\overline x)(Y_k-\overline Y)\]</div></div>
    </div></details>
  </div>

  <h3>Orthogonality principle (วิธีที่ 2)</h3>
  <div class="prose"><p>ถ้าเซตของ g ที่ยอมให้เลือก (𝒢) ปิดภายใต้ linear combination (เป็น subspace) ตัวที่ดีที่สุด ĝ คือตัวที่ทำให้ <b>error vector ตั้งฉากกับทุก vector ใน 𝒢</b> เหมือนการฉายเวกเตอร์ลงระนาบ จุดที่ใกล้ที่สุดคือจุดที่เส้นเชื่อมตั้งฉาก</p></div>
  <div class="plate"><span class="tag">(6.31)</span>
  \[\sum_{k=1}^n[Y_k-\hat g(x_k)]\,g(x_k)=0\ \text{ for all }g\in\mathcal G\ \Rightarrow\ e(\hat g)\le e(g)\ \text{ for all }g\in\mathcal G\]
  <p>ใส่ g(x) = 1 และ g(x) = x ได้สองสมการเดียวกับ Ex 6.21 พอดี</p></div>
  <div class="ex">
    <h4><span class="no">Ex 6.22</span>Polynomial เป็น subspace</h4>
    <div class="q"><b>โจทย์:</b> ให้ \(\mathcal G_p=\{g(x):g(x)=a_0+a_1x+\cdots+a_px^p\}\) แสดงว่า 𝒢ₚ เป็น subspace</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>\[\lambda g_1+\mu g_2=(\lambda a_0+\mu b_0)+(\lambda a_1+\mu b_1)x+\cdots+(\lambda a_p+\mu b_p)x^p\]</div></div>
      <div class="step"><span class="n">2</span><div>ยังเป็น polynomial degree ≤ p <span class="ans">ปิดภายใต้ linear combination ⇒ subspace</span> · degree p มี p + 1 สัมประสิทธิ์ ⇒ ได้ p + 1 สมการ</div></div>
    </div></details>
  </div>
  <div class="plate"><span class="tag">ในโปรแกรม</span>
  \[\hat g(x)=a_1x^p+a_2x^{p-1}+\cdots+a_px+a_{p+1},\qquad a=\text{polyfit}(x,Y,p),\qquad\hat g(t)=\text{polyval}(a,t)\]</div>
  <div class="warn"><span class="tag">Overfitting (SageMath ในคาบ)</span>
    <p>x = [0,1,2,3,4], Y = x + noise แล้ว fit degree 4 ได้ ĝ(x) = −0.0989x⁴ + 0.8369x³ − 2.0491x² + 2.0756x + 0.5442 ผ่านทุกจุดพอดี (error = 0) แต่ ĝ(5) = 2.498 ห่างจาก 5 มาก เพราะ fit noise เข้าไปด้วย model จริงเป็นเส้นตรง ควรใช้ p = 1</p>
  </div>

  <div class="tool" id="tool-reg">
    <span class="tag">ลองเอง</span><h4>Linear regression</h4>
    <p class="hint">ข้อมูลตัวอย่าง (สมมติ): กระแส i (mA) กับแรงดัน V (V) ของตัวต้านทาน ความชัน a คือค่าประมาณ R (kΩ)</p>
    <div class="fields">
      <label class="wide" for="rg-x">xₖ<textarea id="rg-x">1, 2, 3, 4, 5, 6, 7, 8</textarea></label>
      <label class="wide" for="rg-y">Yₖ<textarea id="rg-y">2.3, 4.1, 6.4, 7.8, 10.3, 11.7, 14.2, 15.9</textarea></label>
    </div>
    <div class="out" id="rg-out"></div>
    <figure><div class="fbox"><svg id="fig-reg" role="img" aria-label="scatter plot กับเส้น regression"></svg></div></figure>
  </div>
</section>

<!-- ===================== Ch7 Bivariate ===================== -->
<section class="ch" id="biv">
  <div class="ch-head"><span class="lec">L21–22</span><span class="sec">Ch7.1–7.2 · Bivariate random variables</span><h2>สองตัวแปรพร้อมกัน: joint cdf และ joint density</h2></div>
  <p class="xref">ออกสอบจริง: <a href="#exam-q3">ข้อสอบ Final 2564 ข้อ 3</a></p>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>เมื่อมีสองตัวแปร X, Y ที่เกิดจากการทดลองเดียวกัน เราต้องบอกความน่าจะเป็นของ <b>คู่</b> (X, Y) ว่าตกในพื้นที่ใดบนระนาบ ข้อมูลทั้งหมดอยู่ใน joint cdf หรือ joint density ส่วน marginal คือการ "ลืม" ตัวแปรหนึ่งไป</p>
    <ul>
      <li><b>จาก cdf:</b> ลืม Y = ปล่อย y → ∞ (Y ≤ ∞ เป็นจริงเสมอ)</li>
      <li><b>จาก density:</b> ลืม Y = integrate ทิ้งตลอดแกน y</li>
      <li><b>Independent:</b> joint แยกเป็นผลคูณของ marginal ได้ (เช็กแบบไหนก็ได้ cdf, density หรือ characteristic function)</li>
    </ul>
  </div>
  <div class="plate"><span class="tag">Product set และ marginal probability (7.1)</span>
  \[\{X\in B,Y\in C\}:=\{\omega\in\Omega:X(\omega)\in B\ \text{and}\ Y(\omega)\in C\}=\{\omega\in\Omega:(X(\omega),Y(\omega))\in B\times C\}\]
  \[P(X\in B)=P((X,Y)\in B\times\mathbb R),\qquad P(Y\in C)=P((X,Y)\in\mathbb R\times C)\]</div>
  <div class="plate"><span class="tag">Joint cdf</span>
  \[F_{XY}(x,y):=P(X\le x,Y\le y)=P\big((X,Y)\in(-\infty,x]\times(-\infty,y]\big)\]</div>
  <div class="two">
    <div style="display:flex;flex-direction:column;justify-content:center">
      <div class="plate" style="margin:0"><span class="tag">Rectangle formula (7.2)</span>
      \[P(a&lt;X\le b,\,c&lt;Y\le d)=F_{XY}(b,d)-F_{XY}(a,d)-F_{XY}(b,c)+F_{XY}(a,c)\]</div>
      <p style="color:var(--muted);font-size:.9rem;margin-top:10px">F(b,d) คือพื้นที่มุมล่างซ้ายทั้งหมดของจุด (b,d) ลบแถบซ้ายและแถบล่างออก แต่มุม (a,c) ถูกลบสองครั้ง จึงต้องบวกคืนหนึ่งครั้ง</p>
    </div>
    <div><svg viewBox="0 0 360 250" role="img" aria-label="ภาพ rectangle formula">
      <line class="axis" x1="30" y1="225" x2="345" y2="225"/><line class="axis" x1="30" y1="225" x2="30" y2="10"/>
      <rect class="rect-a" x="130" y="60" width="140" height="95"/>
      <line class="mark" x1="130" y1="60" x2="30" y2="60"/><line class="mark" x1="130" y1="155" x2="130" y2="225"/>
      <line class="mark" x1="270" y1="155" x2="270" y2="225"/><line class="mark" x1="130" y1="155" x2="30" y2="155"/>
      <circle class="dot" cx="270" cy="60" r="4"/><circle class="dot" cx="130" cy="60" r="4"/><circle class="dot" cx="270" cy="155" r="4"/><circle class="dot" cx="130" cy="155" r="4"/>
      <text class="lbl-a" x="278" y="52">+F(b,d)</text><text class="lbl-t" x="122" y="52" text-anchor="end">−F(a,d)</text>
      <text class="lbl-t" x="278" y="172">−F(b,c)</text><text class="lbl-a" x="122" y="172" text-anchor="end">+F(a,c)</text>
      <text class="ax-t" x="130" y="242" text-anchor="middle">a</text><text class="ax-t" x="270" y="242" text-anchor="middle">b</text>
      <text class="ax-t" x="20" y="159" text-anchor="middle">c</text><text class="ax-t" x="20" y="64" text-anchor="middle">d</text>
      <text class="ax-t" x="340" y="242" text-anchor="end">x</text><text class="ax-t" x="20" y="18" text-anchor="middle">y</text>
      <text class="lbl" x="200" y="112" text-anchor="middle">P(a&lt;X≤b, c&lt;Y≤d)</text>
    </svg></div>
  </div>
  <div class="plate"><span class="tag">Marginal cdf (7.3)–(7.4)</span>
  \[F_X(x)=\lim_{y\to\infty}F_{XY}(x,y)=:F_{XY}(x,\infty),\qquad F_Y(y)=\lim_{x\to\infty}F_{XY}(x,y)=:F_{XY}(\infty,y)\]</div>
  <div class="plate"><span class="tag">Joint density และ marginal density (7.9)–(7.10)</span>
  \[P((X,Y)\in A)=\iint_A f_{XY}(x,y)\,dx\,dy,\qquad\int_{-\infty}^\infty\!\!\int_{-\infty}^\infty f_{XY}\,dx\,dy=1,\qquad f_{XY}(x,y)=\frac{\partial^2}{\partial y\,\partial x}F_{XY}(x,y)\]
  \[f_X(x)=\int_{-\infty}^\infty f_{XY}(x,y)\,dy,\qquad f_Y(y)=\int_{-\infty}^\infty f_{XY}(x,y)\,dx\]</div>
  <div class="plate"><span class="tag">Independence · expectation · characteristic function</span>
  \[F_{XY}=F_XF_Y\iff f_{XY}=f_Xf_Y\iff\varphi_{XY}(\nu_1,\nu_2)=\varphi_X(\nu_1)\varphi_Y(\nu_2)\]
  \[E[g(X,Y)]=\int_{-\infty}^\infty\!\!\int_{-\infty}^\infty g(x,y)f_{XY}(x,y)\,dx\,dy,\qquad\varphi_{XY}(\nu_1,\nu_2):=E\big[e^{j(\nu_1X+\nu_2Y)}\big]\]
  <p>φ_XY คือ 2D Fourier transform ของ f_XY</p></div>

  <div class="prose"><p>Ex 7.7, 7.8, 7.12, 7.13 ใช้ joint cdf ชุดเดียวกัน ทำต่อเนื่องเป็นเรื่องเดียว:</p></div>
  <div class="plate">\[F_{XY}(x,y)=\begin{cases}\dfrac{y+e^{-x(y+1)}}{y+1}-e^{-x}, & x,y&gt;0\\[4pt] 0, & \text{otherwise}\end{cases}\]</div>
  <div class="ex">
    <h4><span class="no">Ex 7.7</span>หา marginal cdf</h4>
    <div class="q"><b>โจทย์:</b> จาก F_XY ข้างบน หา F_X(x) และ F_Y(y)</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>แยกเทอมเพื่อดูลิมิตง่ายๆ (x, y &gt; 0) \[F_{XY}(x,y)=\frac{y}{y+1}+\frac{1}{y+1}\cdot e^{-x(y+1)}-e^{-x}\]</div></div>
      <div class="step"><span class="n">2</span><div>y → ∞: y/(y+1) → 1 และ (1/(y+1))·e^(−x(y+1)) → 0 \[F_X(x)=\begin{cases}1-e^{-x},&x&gt;0\\0,&x\le0\end{cases}\quad(7.5)\] ⇒ X ~ exp(1)</div></div>
      <div class="step"><span class="n">3</span><div>x → ∞: สองเทอมหลัง → 0 \[F_Y(y)=\begin{cases}y/(y+1),&y&gt;0\\0,&y\le0\end{cases}\quad(7.6)\]</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 7.8</span>X, Y ไม่ independent</h4>
    <div class="q"><b>โจทย์:</b> แสดงว่า X และ Y ใน Ex 7.7 ไม่ independent</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>\[F_X(x)F_Y(y)=(1-e^{-x})\frac{y}{y+1}\ne\frac{y+e^{-x(y+1)}}{y+1}-e^{-x}=F_{XY}(x,y)\] <span class="ans">ไม่ independent</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 7.12</span>Joint density จาก joint cdf</h4>
    <div class="q"><b>โจทย์:</b> หา f_XY จาก F_XY ชุดเดิม</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>\[\frac{\partial}{\partial x}F_{XY}=-\frac{(y+1)e^{-x(y+1)}}{y+1}+e^{-x}=e^{-x}-e^{-x(y+1)}\]</div></div>
      <div class="step"><span class="n">2</span><div>ดิฟต่อเทียบ y: e⁻ˣ ไม่มี y จึงหายไป \[\frac{\partial^2}{\partial y\,\partial x}F_{XY}=xe^{-x(y+1)}\] <span class="ans">f_XY(x,y) = x e^(−x(y+1)), x, y &gt; 0</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 7.13</span>Marginal density แล้วตรวจกับ Ex 7.7</h4>
    <div class="q"><b>โจทย์:</b> ใช้ f_XY จาก Ex 7.12 หา f_X และ f_Y แล้วตรวจคำตอบโดยดิฟ marginal cdf จาก Ex 7.7</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>f_XY = 0 เมื่อ y ≤ 0 ขอบล่างของ integral จึงเป็น 0 \[f_X(x)=\int_0^\infty xe^{-x}e^{-xy}\,dy=xe^{-x}\cdot\frac1x=e^{-x}\]</div></div>
      <div class="step"><span class="n">2</span><div>จัดให้เป็น mean ของ exp(λ) โดย λ = y + 1 ซึ่งเท่ากับ 1/λ \[f_Y(y)=\frac{1}{y+1}\int_0^\infty x\cdot(y+1)e^{-(y+1)x}\,dx=\frac{1}{y+1}\cdot\frac{1}{y+1}=\frac{1}{(y+1)^2}\]</div></div>
      <div class="step"><span class="n">3</span><div>ตรวจ: d/dx(1 − e⁻ˣ) = e⁻ˣ ✓ · d/dy(y/(y+1)) = 1/(y+1)² ✓ <span class="ans">f_X = e⁻ˣ, f_Y = 1/(y+1)²</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 7.9</span>ตรวจว่าเป็น joint density จริง</h4>
    <div class="q"><b>โจทย์:</b> แสดงว่า \(f_{XY}(x,y)=\frac{1}{2\pi}e^{-(2x^2-2xy+y^2)/2}\) เป็น valid joint probability density</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>f ≥ 0 ชัดเจน เหลือแสดงว่า integrate ได้ 1 · complete the square: 2x² − 2xy + y² = (y − x)² + x²</div></div>
      <div class="step"><span class="n">2</span><div>\[f_{XY}(x,y)=\underbrace{\frac{e^{-(y-x)^2/2}}{\sqrt{2\pi}}}_{N(x,1)\text{ in }y}\cdot\underbrace{\frac{e^{-x^2/2}}{\sqrt{2\pi}}}_{N(0,1)\text{ in }x}\]</div></div>
      <div class="step"><span class="n">3</span><div>Integral ในเทียบ y เป็น density ⇒ 1 · ที่เหลือเป็น density N(0,1) ⇒ 1 <span class="ans">valid</span></div></div>
    </div></details>
  </div>
</section>

<!-- ===================== 7.3 Conditional ===================== -->
<section class="ch" id="cond">
  <div class="ch-head"><span class="lec">L22</span><span class="sec">Ch7.3 · Conditional probability and expectation</span><h2>Conditional density และ law of total probability</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>ถ้า X continuous แล้ว P(X = x) = 0 เราจึงใช้สูตร P(A|B) = P(A∩B)/P(B) ตรงๆ ไม่ได้ (หารด้วยศูนย์) แทนที่จะทำแบบนั้น ให้นึกภาพ <b>ตัดแผ่น f_XY ตามเส้น X = x</b> แล้วหารด้วยพื้นที่ของชิ้นนั้น (ซึ่งก็คือ f_X(x)) เพื่อให้ชิ้นนั้นกลายเป็น density ที่ integrate ได้ 1</p>
    <p>Law of total probability คือการ <b>เฉลี่ยคำตอบแบบมีเงื่อนไขทุกค่า x ด้วยน้ำหนัก f_X(x)</b> · Substitution law: พอรู้ว่า X = x แล้ว X ในสูตรก็เป็นแค่ค่าคงที่ x</p>
  </div>
  <div class="plate"><span class="tag">Conditional density (7.12)</span>
  \[f_{Y|X}(y|x):=\frac{f_{XY}(x,y)}{f_X(x)},\ \ f_X(x)&gt;0,\qquad P(Y\in C|X=x):=\int_Cf_{Y|X}(y|x)\,dy,\qquad F_{Y|X}(y|x)=\int_{-\infty}^yf_{Y|X}(t|x)\,dt\]</div>
  <div class="plate"><span class="tag">Law of total probability (7.13)–(7.14) · Substitution law (7.15)</span>
  \[P(Y\in C)=\int_{-\infty}^\infty P(Y\in C|X=x)f_X(x)\,dx,\qquad P((X,Y)\in A)=\int_{-\infty}^\infty P((X,Y)\in A|X=x)f_X(x)\,dx\]
  \[P((X,Y)\in A|X=x)=P((x,Y)\in A|X=x)\]</div>
  <div class="plate"><span class="tag">Conditional expectation (7.18)–(7.20)</span>
  \[E[g(Y)|X=x]:=\int g(y)f_{Y|X}(y|x)\,dy,\qquad E[g(X,Y)|X=x]:=\int g(x,y)f_{Y|X}(y|x)\,dy\]
  \[E[g(X,Y)]=\int_{-\infty}^\infty E[g(X,Y)|X=x]\,f_X(x)\,dx\]</div>
  <div class="plate"><span class="tag">ถ้า X, Y independent: "drop the conditioning"</span>
  \[f_{Y|X}(y|x)=\frac{f_X(x)f_Y(y)}{f_X(x)}=f_Y(y)\ \Rightarrow\ P(Y\in C|X=x)=P(Y\in C),\quad E[g(Y)|X=x]=E[g(Y)]\]</div>
  <div class="ex">
    <h4><span class="no">Ex 7.18</span>E[Y²] และ E[Y²X³]</h4>
    <div class="q"><b>โจทย์:</b> X ~ exp(1) และเมื่อกำหนด X = x แล้ว Y มีการแจกแจงแบบมีเงื่อนไข N(0, x²) จงหา E[Y²] และ E[Y²X³]</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>เมื่อ X = x, Y ~ N(0, x²) ⇒ E[Y²|X=x] = variance + mean² = x² + 0 = x² \[f_{Y|X}(y|x)=\frac{e^{-(y/x)^2/2}}{\sqrt{2\pi}\,x}\]</div></div>
      <div class="step"><span class="n">2</span><div>\[E[Y^2]=\int E[Y^2|X=x]f_X(x)\,dx=\int x^2f_X(x)\,dx=E[X^2]=\frac{2!}{1^2}=2\] <span class="ans">E[Y²] = 2</span></div></div>
      <div class="step"><span class="n">3</span><div>Substitution law: เมื่อ X = x แล้ว X³ = x³ เป็นค่าคงที่ ดึงออกจาก E ได้ \[E[Y^2X^3]=\int x^3E[Y^2|X=x]f_X(x)\,dx=\int x^5f_X(x)\,dx=E[X^5]=5!\] <span class="ans">E[Y²X³] = 120</span></div></div>
      <div class="step"><span class="n">*</span><div>ใช้ moment ของ exp(λ): \(E[X^n]=n!/\lambda^n\)</div></div>
    </div></details>
  </div>
</section>

<!-- ===================== Ch8 Random vectors ===================== -->
<section class="ch" id="vec">
  <div class="ch-head"><span class="lec">L22–23</span><span class="sec">Ch8 · Introduction to random vectors</span><h2>Random vectors และ Jacobian transformation</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>รวมหลายตัวแปรเป็น column vector X = [X₁, …, Xₙ]′ แล้วทำทุกอย่าง <b>ทีละ entry</b> ค่าเฉลี่ยเป็น vector, ความสัมพันธ์ระหว่างทุกคู่เป็น matrix n×n</p>
    <p>Jacobian formula ใช้เมื่อแปลงตัวแปร Y = G(X): density ใหม่ = density เดิมที่จุดที่ย้อนกลับไป × <b>|det dH|</b> ซึ่งเป็นตัวปรับว่าการแปลงยืดหรือหดพื้นที่ไปกี่เท่า (ความน่าจะเป็นต้องรวมได้ 1 เท่าเดิม)</p>
  </div>
  <div class="plate"><span class="tag">Mean vector · correlation matrix · covariance matrix</span>
  \[E[X]:=\begin{bmatrix}E[X_1]\\\vdots\\E[X_n]\end{bmatrix},\qquad R:=E[XX'],\ R_{ij}=E[X_iX_j],\qquad C=\text{cov}(X):=E[(X-m)(X-m)'],\ C_{ij}=\text{cov}(X_i,X_j)\]
  <p>R′ = R และ C′ = C (symmetric) · Cᵢᵢ = var(Xᵢ) · C เป็น diagonal ⇔ ทุกคู่ uncorrelated</p></div>
  <div class="ex">
    <h4><span class="no">Ex 8.1</span>Correlation matrix 3 มิติ</h4>
    <div class="q"><b>โจทย์:</b> เขียน correlation matrix ของ W := [X, Y, Z]′</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>WW′ = (3×1)(1×3) = เมทริกซ์ 3×3 แล้ว take E ทีละ entry \[E[WW']=\begin{bmatrix}E[X^2]&E[XY]&E[XZ]\\E[YX]&E[Y^2]&E[YZ]\\E[ZX]&E[ZY]&E[Z^2]\end{bmatrix}\]</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 8.3</span>cov(AX) = ACA′</h4>
    <div class="q"><b>โจทย์:</b> ถ้า X มี covariance matrix C แสดงว่า Y := AX มี covariance matrix ACA′</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>A เป็นค่าคงที่ ดึงออกจาก E ได้: E[Y] = Am ⇒ Y − E[Y] = A(X − m)</div></div>
      <div class="step"><span class="n">2</span><div>ใช้ (AB)′ = B′A′ \[\text{cov}(Y)=E[A(X-m)(X-m)'A']=A\,E[(X-m)(X-m)']\,A'=ACA'\]</div></div>
    </div></details>
  </div>
  <div class="plate"><span class="tag">Cross-covariance (n × p)</span>\[\text{cov}(X,Y):=E[(X-m_X)(Y-m_Y)']\]</div>
  <div class="plate"><span class="tag">Jacobian formula (8.7) · Y = G(X), H := G⁻¹</span>
  \[f_Y(y)=f_X(H(y))\,|\det dH(y)|,\qquad dH(y):=\begin{bmatrix}\frac{\partial h_1}{\partial y_1}&\cdots&\frac{\partial h_1}{\partial y_n}\\\vdots&&\vdots\\\frac{\partial h_n}{\partial y_1}&\cdots&\frac{\partial h_n}{\partial y_n}\end{bmatrix}\]</div>
  <div class="ex">
    <h4><span class="no">Ex 8.10</span>U = 2X − 5Y, V = X − 4Y</h4>
    <div class="q"><b>โจทย์:</b> X, Y เป็น N(0,1) ที่ independent ให้ U := 2X − 5Y และ V := X − 4Y จงหา joint density ของ U, V และบอกว่า independent หรือไม่</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>แก้หา inverse H:</b> จาก v ได้ x = v + 4y แทนใน u ได้ u = 2v + 3y \[y=\tfrac13u-\tfrac23v,\qquad x=\tfrac43u-\tfrac53v\]</div></div>
      <div class="step"><span class="n">2</span><div><b>Jacobian:</b> \[dH=\begin{bmatrix}4/3&-5/3\\1/3&-2/3\end{bmatrix},\qquad\det dH=-\tfrac89+\tfrac59=-\tfrac13\ \Rightarrow\ |\det dH|=\tfrac13\]</div></div>
      <div class="step"><span class="n">3</span><div><b>แทนใน f_XY:</b> independent ⇒ \(f_{XY}=\frac{1}{2\pi}e^{-(x^2+y^2)/2}\) และ x² + y² = (17u² − 44uv + 29v²)/9 \[f_{UV}(u,v)=\frac{1}{2\pi}\exp\!\left[-\frac12\left(\tfrac{17}{9}u^2-\tfrac{44}{9}uv+\tfrac{29}{9}v^2\right)\right]\cdot\frac13\]</div></div>
      <div class="step"><span class="n">4</span><div>มีเทอม uv ⇒ correlation coefficient ≠ 0 <span class="ans">U, V ไม่ independent</span> เพราะทั้งคู่สร้างจาก X, Y ตัวเดียวกัน</div></div>
    </div></details>
  </div>
</section>

<!-- ===================== Ch10 Random processes ===================== -->
<section class="ch" id="proc">
  <div class="ch-head"><span class="lec">L23–24</span><span class="sec">Ch10 · Introduction to random processes</span><h2>Random process และ stationarity</h2></div>
  <p class="xref">ออกสอบจริง: <a href="#exam-q4">ข้อสอบ Final 2564 ข้อ 4</a></p>
  <div class="idea">
    <span class="tag">เข้าใจก่อน: มองได้สองแบบ</span>
    <p>Random process = ชุดของ random variable ที่เรียงตามเวลา X_t(ω) มีสองตัวแปร:</p>
    <ul>
      <li><b>Fix เวลา t</b> ⇒ ได้ random variable หนึ่งตัว (ดูแนวตั้ง ข้ามหลายๆ การทดลอง)</li>
      <li><b>Fix ω</b> (ทำการทดลองหนึ่งครั้ง) ⇒ ได้ฟังก์ชันของเวลาหนึ่งเส้น เรียกว่า realization, sample path หรือ sample function</li>
    </ul>
  </div>
  <div class="tw"><table class="t">
    <thead><tr><th>ชนิด</th><th>ตัวอย่างในคาบ</th></tr></thead>
    <tbody>
      <tr><td>Discrete-time {Xₙ, n = 1, 2, …}</td><td>Ex 10.1 bit ถูกพลิกใน noisy channel: i.i.d. Bernoulli(p) · Ex 10.2 thermal noise ของ amplifier วัดวินาทีละครั้ง: i.i.d. N(0,1)</td></tr>
      <tr><td>Continuous-time {X_t, t ≥ 0}</td><td>Ex 10.5 carrier ที่มี random phase · Ex 10.7 Brownian motion (Wiener process) ใช้ model integrated white noise, traffic ใน router, ตลาดหุ้น</td></tr>
    </tbody></table></div>
  <figure><div class="fbox"><svg id="fig-carrier" role="img" aria-label="3 realizations ของ carrier with random phase"></svg></div>
    <figcaption>Ex 10.5 (Figure 10.5): X_t = cos(2πft + Θ) ที่ Θ = 1.5, −0.67, −1.51 สุ่ม Θ ครั้งเดียวต่อการทดลอง แล้วทั้งเส้นเป็น cosine ปกติที่เลื่อน phase ความสุ่มอยู่ที่ phase อย่างเดียว</figcaption></figure>

  <div class="plate"><span class="tag">Mean function (10.1) · correlation function (10.2)</span>
  \[m_X(t):=E[X_t],\qquad R_X(t_1,t_2):=E[X_{t_1}X_{t_2}]\]
  <p>Mean function บอกพฤติกรรมเฉลี่ยตามเวลา · correlation function บอกว่าค่าที่สองเวลาเกี่ยวข้องกันแค่ไหน (process เรียบหรือแกว่งเร็ว)</p></div>
  <div class="plate"><span class="tag">Cross-correlation (10.8) · cross-covariance (10.9)</span>
  \[R_{XY}(t_1,t_2):=E[X_{t_1}Y_{t_2}],\qquad C_{XY}(t_1,t_2)=E\big[\{X_{t_1}-m_X(t_1)\}\{Y_{t_2}-m_Y(t_2)\}\big]=R_{XY}(t_1,t_2)-m_X(t_1)m_Y(t_2)\]</div>

  <div class="ex">
    <h4><span class="no">Ex 10.8</span>Mean และ correlation ของ carrier</h4>
    <div class="q"><b>โจทย์:</b> X_t = cos(2πft + Θ), Θ ~ uniform[−π, π] จงหา mean function และ correlation function</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>Mean:</b> fix t แล้ว X_t เป็นฟังก์ชันของ Θ ⇒ integrate เทียบ <b>θ ไม่ใช่ t</b> และ cosine ครบหนึ่งคาบ (ช่วงยาว 2π) \[E[X_t]=\int_{-\pi}^{\pi}\cos(2\pi ft+\theta)\frac{d\theta}{2\pi}=0\]</div></div>
      <div class="step"><span class="n">2</span><div><b>Correlation:</b> ใช้ \(\cos A\cos B=\tfrac12[\cos(A+B)+\cos(A-B)]\) (10.3) โดย Θ หักล้างกันในเทอม A − B \[R_X(t_1,t_2)=\tfrac12E\big[\cos(2\pi f[t_1+t_2]+2\Theta)\big]+\tfrac12\cos(2\pi f[t_1-t_2])\]</div></div>
      <div class="step"><span class="n">3</span><div>เทอมแรกมี 2θ ครบ 2 คาบ ⇒ 0 · เทอมสองไม่สุ่ม <span class="ans">R_X(t₁, t₂) = cos(2πf[t₁ − t₂])/2</span></div></div>
    </div></details>
  </div>

  <h3>Stationarity</h3>
  <div class="two">
    <div><h4>Strict-sense (SSS)</h4><p>การแจกแจง joint ของจุดเวลาใดๆ <b>ไม่เปลี่ยนเมื่อเลื่อนเวลา</b> ทั้งหมด ถ้าเป็นจริงทุก order n เรียกว่า strictly stationary (เงื่อนไขแรงมาก)</p></div>
    <div><h4>Wide-sense (WSS)</h4><p>ขอแค่สองอย่าง: (i) mean คงที่ไม่ขึ้นกับ t (ii) correlation ขึ้นกับ <b>ระยะห่าง</b> τ = t₁ − t₂ อย่างเดียว ไม่สนว่าอยู่เวลาไหน</p></div>
  </div>
  <div class="plate"><span class="tag">nth order strictly stationary</span>
  \[P\big((X_{t_1},\dots,X_{t_n})\in B\big)=P\big((X_{t_1+\Delta t},\dots,X_{t_n+\Delta t})\in B\big)\quad\forall\,B,\ \forall\,\Delta t\]</div>
  <div class="plate"><span class="tag">WSS</span>
  \[E[X_t]\ \text{does not depend on } t,\qquad E[X_{t_1}X_{t_2}]=R_X(t_1-t_2)=R_X(\tau)\]</div>
  <div class="ex">
    <h4><span class="no">Ex 10.13</span>Process ค่าคงที่เป็น strictly stationary</h4>
    <div class="q"><b>โจทย์:</b> ให้ Z เป็น random variable และ X_t := Z ทุก t แสดงว่า X_t strictly stationary</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ทั้งสองข้างเท่ากับ P((Z, …, Z) ∈ B) ซึ่งไม่มี Δt อยู่เลย ทุก n <span class="ans">strictly stationary</span></div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 10.16</span>Correlation function เป็นฟังก์ชันคู่เสมอ</h4>
    <div class="q"><b>โจทย์:</b> แสดงว่า univariate correlation function ของ WSS process เป็น even เสมอ</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ให้ t₁ = t − τ, t₂ = t \[\begin{aligned}R_X(-\tau)&=E[X_{t-\tau}X_t] &&\text{by (10.13)}\\&=E[X_tX_{t-\tau}]&&\text{multiplication commutes}\\&=R_X(t-[t-\tau])=R_X(\tau)&&\text{by (10.14)}\end{aligned}\]</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 10.17</span>Carrier เป็น WSS</h4>
    <div class="q"><b>โจทย์:</b> แสดงว่า carrier with random phase ใน Ex 10.8 เป็น WSS</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>(i) E[X_t] = 0 คงที่ ✓ (ii) R_X ขึ้นกับ t₁ − t₂ เท่านั้น ✓ <span class="ans">WSS, R_X(τ) = cos(2πfτ)/2</span></div></div>
    </div></details>
  </div>
  <div class="plate"><span class="tag">Power spectral density (คู่ Fourier ของ R_X)</span>
  \[S_X(f):=\int_{-\infty}^\infty R_X(\tau)e^{-j2\pi f\tau}\,d\tau,\qquad R_X(\tau)=\int_{-\infty}^\infty S_X(f)e^{j2\pi f\tau}\,df\]</div>
</section>

<!-- ===================== Ch11 Poisson ===================== -->
<section class="ch" id="poisson">
  <div class="ch-head"><span class="lec">L24–25</span><span class="sec">Ch11 · Advanced concepts in random processes</span><h2>Poisson process: นับเหตุการณ์ที่เกิดแบบสุ่มตามเวลา</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>N_t = จำนวนครั้งที่เหตุการณ์เกิดในช่วง [0, t] เช่น packet มาถึง router, photoelectron หลุดจาก detector, ลูกค้าเข้าร้าน กราฟของ N_t เป็น <b>ขั้นบันไดกระโดดทีละ 1</b> (counting process)</p>
    <p>Poisson process คือ counting process ที่ (1) เริ่มที่ 0 (2) จำนวนในช่วงยาว t − s เป็น Poisson ที่ mean λ(t − s) และ (3) ช่วงเวลาที่ไม่ซ้อนกันเป็นอิสระกัน ผลที่ตามมาคือ <b>เวลารอระหว่างเหตุการณ์เป็น exponential(λ)</b> ซึ่งไม่มีความจำ (memoryless)</p>
  </div>
  <figure><div class="fbox"><svg id="fig-pois" role="img" aria-label="sample path ของ Poisson process แบบขั้นบันได"></svg></div>
    <figcaption>Sample path หนึ่งเส้น: กระโดดขึ้น 1 ที่ jump times T₁, T₂, … ระยะห่างระหว่างจุดกระโดด Xₙ = Tₙ − Tₙ₋₁ คือ interarrival times ซึ่งเป็น i.i.d. exp(λ)</figcaption></figure>
  <div class="plate"><span class="tag">นิยาม 3 ข้อ</span>
  <ol style="margin:4px 0 8px;padding-left:1.3em;color:var(--ink)">
    <li>N₀ ≡ 0</li>
    <li>สำหรับ 0 ≤ s &lt; t &lt; ∞ increment N_t − N_s เป็น Poisson R.V. parameter λ(t − s) · λ = rate หรือ intensity (จำนวนต่อหน่วยเวลา)</li>
    <li>Independent increments: ช่วง (t₁,t₂], (t₂,t₃], …, (tₙ,tₙ₊₁] ไม่ซ้อนกัน ⇒ increments ในแต่ละช่วงเป็น independent</li>
  </ol>
  \[P(N_t-N_s=k)=\frac{[\lambda(t-s)]^ke^{-\lambda(t-s)}}{k!},\quad k=0,1,2,\dots\qquad E[N_t-N_s]=\text{var}(N_t-N_s)=\lambda(t-s)\]</div>

  <div class="ex">
    <h4><span class="no">Ex 11.1</span>Photoelectrons สองนาทีติดกัน</h4>
    <div class="q"><b>โจทย์:</b> Photoelectron ถูกปล่อยจาก photodetector ด้วย rate λ ต่อนาที จงหาความน่าจะเป็นที่ในแต่ละนาทีของสองนาทีติดกัน มี photoelectron มากกว่า 5 ตัว</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>"มากกว่า 5" = "≥ 6" ⇒ ต้องการ \(P(\{N_1-N_0\ge6\}\cap\{N_2-N_1\ge6\})\)</div></div>
      <div class="step"><span class="n">2</span><div>ช่วง (0,1] กับ (1,2] ไม่ซ้อนกัน ⇒ independent increments ⇒ แยกเป็นผลคูณได้ \[=P(N_1-N_0\ge6)\,P(N_2-N_1\ge6)\]</div></div>
      <div class="step"><span class="n">3</span><div>แต่ละช่วงยาว 1 นาที ⇒ Poisson(λ) \[\left(1-\sum_{k=0}^{5}\frac{\lambda^ke^{-\lambda}}{k!}\right)^2\]</div></div>
      <div class="step"><span class="n">*</span><div>ลองแทนตัวเลข (ไม่อยู่ในสไลด์): λ = 5 ⇒ P(N ≤ 5) ≈ 0.616 ⇒ คำตอบ ≈ 0.384² ≈ 0.147</div></div>
    </div></details>
  </div>

  <h3>Mean, correlation, covariance</h3>
  <div class="prose"><p>เทคนิคสำคัญ: เมื่อเจอ N_t กับ N_s (s &lt; t) ที่ช่วงซ้อนกัน ให้ <b>แยก N_t = (N_t − N_s) + N_s</b> เพื่อให้ได้ช่วงที่ไม่ซ้อนกัน แล้วใช้ independent increments</p></div>
  <div class="plate">
  \[E[N_t]=\lambda t,\quad\text{var}(N_t)=\lambda t,\quad E[N_t^2]=\text{var}+E^2=\lambda t+(\lambda t)^2\]
  \[E[N_tN_s]=E[(N_t-N_s)(N_s-N_0)]+E[N_s^2]=\lambda(t-s)\cdot\lambda s+\lambda s+(\lambda s)^2=(\lambda t)(\lambda s)+\lambda s\]
  \[\text{cov}(N_t,N_s)=E[N_tN_s]-(\lambda t)(\lambda s)=\lambda s\ \Rightarrow\ \text{cov}(N_{t_1},N_{t_2})=\lambda\min(t_1,t_2)\]</div>

  <h3>Jump times เป็น Erlang · interarrival เป็น exponential</h3>
  <div class="prose"><p>Tₙ = เวลาที่กระโดดครั้งที่ n · เหตุการณ์ "Tₙ &gt; t" (ครั้งที่ n ยังไม่เกิดจนถึงเวลา t) เหมือนกับ "N_t &lt; n" (ถึงเวลา t ยังนับได้ไม่ถึง n)</p></div>
  <div class="plate">
  \[T_n:=\min\{t&gt;0:N_t\ge n\},\qquad P(T_n&gt;t)=P(N_t&lt;n)=\sum_{k=0}^{n-1}\frac{(\lambda t)^k}{k!}e^{-\lambda t}\]
  \[f_{T_n}(t)=\lambda\frac{(\lambda t)^{n-1}e^{-\lambda t}}{(n-1)!},\ t\ge0\ \ (\text{Erlang}),\qquad f_{T_1}(t)=\lambda e^{-\lambda t}\]
  \[X_1=T_1,\quad X_n=T_n-T_{n-1},\quad T_n=X_1+\cdots+X_n,\qquad f_{X_i}(x)=\lambda e^{-\lambda x},\ x\ge0\]
  <p>ผลรวม exp(λ) i.i.d. n ตัว = Erlang(n, λ) ตรงกับที่ใช้ใน Ex 5.18</p></div>
  <div class="plate"><span class="tag">11.2 Renewal process</span>
  \[N_t:=\sum_{k=1}^\infty I_{[0,t]}(T_k),\qquad T_k:=X_1+\cdots+X_k\]
  <p>โครงสร้างเดียวกับ Poisson แต่ interarrival Xᵢ เป็น i.i.d. density อะไรก็ได้ ไม่จำเป็นต้องเป็น exponential</p></div>

  <div class="tool" id="tool-pois">
    <span class="tag">ลองเอง</span><h4>ความน่าจะเป็นของ Poisson process</h4>
    <p class="hint">ค่าเริ่มต้น: λ = 5 ต่อนาที, ช่วงยาว 1 นาที, k = 6 (Ex 11.1 แบบแทนตัวเลข)</p>
    <div class="fields">
      <label for="po-l">λ (ต่อหน่วยเวลา)<input id="po-l" type="number" value="5" step="any" min="0.001"></label>
      <label for="po-t">ความยาวช่วง t − s<input id="po-t" type="number" value="1" step="any" min="0.001"></label>
      <label for="po-k">k<input id="po-k" type="number" value="6" min="0"></label>
    </div>
    <div class="out" id="po-out"></div>
    <figure><div class="fbox"><svg id="fig-popmf" role="img" aria-label="pmf ของ Poisson"></svg></div></figure>
  </div>
</section>

<!-- ===================== Ch12 Markov ===================== -->
<section class="ch" id="markov">
  <div class="ch-head"><span class="lec">L25</span><span class="sec">Ch12 · Introduction to Markov chains</span><h2>Discrete-time Markov chain</h2></div>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p><b>Markov property:</b> รู้สถานะปัจจุบันแล้ว อดีตไม่ช่วยทำนายอนาคตเพิ่ม เหมือนเกมกระดานที่ตาต่อไปขึ้นกับช่องที่ยืนอยู่ตอนนี้เท่านั้น ไม่สนว่าเดินมาทางไหน</p>
    <p>ทั้ง chain จึงสรุปได้ด้วย <b>transition matrix P</b> (แถว = ต้นทาง, คอลัมน์ = ปลายทาง) และในระยะยาว chain มักเข้าสู่ <b>stationary distribution π</b> ที่ไม่เปลี่ยนอีกเมื่อเดินต่อ (π = πP)</p>
  </div>
  <div class="plate"><span class="tag">Markov property</span>
  \[P(X_{n+1}=i_{n+1}\mid X_n=i_n,\dots,X_0=i_0)=P(X_{n+1}=i_{n+1}\mid X_n=i_n)\]</div>
  <div class="ex">
    <h4><span class="no">Ex 12.3</span>Random walk เป็น Markov chain</h4>
    <div class="q"><b>โจทย์:</b> X₀ เป็น integer-valued R.V. ที่ independent กับ i.i.d. Z₁, Z₂, … โดย P(Zₙ = 1) = a, P(Zₙ = −1) = b, P(Zₙ = 0) = 1 − (a + b) แสดงว่า Xₙ := Xₙ₋₁ + Zₙ เป็น Markov chain</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>เขียนต่อกันได้ Xₙ = X₀ + Z₁ + ⋯ + Zₙ ⇒ X₀, …, Xₙ เป็นฟังก์ชันของ X₀, Z₁, …, Zₙ เท่านั้น ⇒ independent ของ Zₙ₊₁</div></div>
      <div class="step"><span class="n">2</span><div>\[\begin{aligned}P(X_{n+1}=j\mid X_n=i,\dots,X_0=x_0)&=P(i+Z_{n+1}=j\mid X_n=i,\dots,X_0=x_0)\\&=P(Z_{n+1}=j-i)=P(X_{n+1}=j\mid X_n=i)\end{aligned}\]</div></div>
      <div class="step"><span class="n">3</span><div>บรรทัดแรกใช้ substitution (แทน Xₙ = i) บรรทัดสองตัดเงื่อนไขทิ้งเพราะ Zₙ₊₁ independent ของอดีตทั้งหมด <span class="ans">Markov ∎</span></div></div>
    </div></details>
  </div>

  <div class="plate"><span class="tag">Transition probabilities (time homogeneous)</span>
  \[p_{ij}:=P(X_{n+1}=j\mid X_n=i),\qquad\sum_jp_{ij}=1,\qquad P=\begin{bmatrix}1-a&a\\b&1-b\end{bmatrix}\ \ (\text{Figure 12.2})\]<p>แต่ละแถวของ P รวมได้ 1 · Figure 12.2 คือ chain สอง state ที่ p₀₁ = a, p₁₀ = b</p></div>
  <div class="plate"><span class="tag">Random walk with barrier (12.9)–(12.10)</span>
  \[p_{ij}=\begin{cases}b_i,&j=i-1\\1-(a_i+b_i),&j=i\\a_i,&j=i+1\\0,&\text{otherwise}\end{cases}\qquad p_{0j}=\begin{cases}1-a_0,&j=0\\a_0,&j=1\\0,&\text{otherwise}\end{cases}\]
  <p>P เป็น tridiagonal · a₀ = 1 ⇒ reflecting barrier, a₀ = 0 ⇒ absorbing barrier · random walk with barrier = birth–death process · มี barrier สองด้าน (0 ถึง N) = queue with finite buffer</p></div>
  <div class="plate"><span class="tag">m-step (12.16)–(12.17) · Chapman–Kolmogorov (12.18)</span>
  \[p_{ij}^{(m)}:=P(X_m=j\mid X_0=i)=P(X_{n+m}=j\mid X_n=i),\quad p_{ij}^{(0)}=\delta_{ij},\qquad p_{ij}^{(n+m)}=\sum_kp_{ik}^{(n)}p_{kj}^{(m)}\]
  <p>ความหมาย: จะไป i → j ใน n + m ก้าว ต้องผ่านสถานะกลาง k สักตัวที่ก้าว n · ในรูปเมทริกซ์คือ P⁽ᵐ⁾ = Pᵐ</p></div>
  <div class="plate"><span class="tag">Stationary distribution (12.19)</span>
  \[\pi_j=\sum_k\pi_kp_{kj},\quad\pi_j\ge0,\quad\sum_j\pi_j=1\qquad\Longleftrightarrow\qquad\pi=\pi P\]
  <p>πⱼ คือ inner product ของแถว π กับ <b>คอลัมน์ที่ j</b> ของ P · สมการ πⱼ ชุดนี้มีหนึ่งสมการที่ซ้ำซ้อนเสมอ จึงต้องใช้ Σπⱼ = 1 แทนหนึ่งสมการ</p></div>

  <div class="two">
    <div><svg viewBox="0 0 420 290" role="img" aria-label="state transition diagram ของ Ex 12.4">
      <defs>
        <marker id="ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path class="arrowhead" d="M0,0 L10,5 L0,10 z"/></marker>
      </defs>
      <path class="edge" d="M114,70 L304,70" marker-end="url(#ah)"/><text class="lbl-a" x="210" y="60" text-anchor="middle">1/4</text>
      <path class="edge" d="M98,93 Q118,175 186,207" marker-end="url(#ah)"/><text class="lbl-a" x="108" y="168" text-anchor="middle">3/4</text>
      <path class="edge" d="M194,192 Q176,122 112,86" marker-end="url(#ah)"/><text class="lbl-a" x="168" y="124" text-anchor="middle">2/5</text>
      <path class="edge" d="M322,93 Q302,175 234,207" marker-end="url(#ah)"/><text class="lbl-a" x="312" y="168" text-anchor="middle">1/2</text>
      <path class="edge" d="M226,192 Q244,122 308,86" marker-end="url(#ah)"/><text class="lbl-a" x="252" y="124" text-anchor="middle">2/5</text>
      <path class="edge" d="M320,49 C300,6 362,6 342,49" marker-end="url(#ah)"/><text class="lbl-a" x="372" y="26">1/2</text>
      <path class="edge" d="M199,236 C178,282 242,282 221,236" marker-end="url(#ah)"/><text class="lbl-a" x="250" y="272">1/5</text>
      <circle class="node" cx="90" cy="70" r="24"/><text class="node-t" x="90" y="75" text-anchor="middle">0</text>
      <circle class="node" cx="330" cy="70" r="24"/><text class="node-t" x="330" y="75" text-anchor="middle">1</text>
      <circle class="node" cx="210" cy="215" r="24"/><text class="node-t" x="210" y="220" text-anchor="middle">2</text>
    </svg></div>
    <div style="display:flex;flex-direction:column;justify-content:center;gap:8px">
      <div class="plate" style="margin:0">\[P=\begin{bmatrix}0&1/4&3/4\\0&1/2&1/2\\2/5&2/5&1/5\end{bmatrix}\]</div>
      <p style="color:var(--muted);font-size:.9rem">ลูกศรออกจากแต่ละ state รวมกันได้ 1 · state 0 ไม่มีลูกศรวนกลับตัวเองเพราะ p₀₀ = 0</p>
    </div>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 12.4</span>หา stationary distribution</h4>
    <div class="q"><b>โจทย์:</b> หา stationary distribution π = [π₀ π₁ π₂] ของ chain ที่มี transition matrix P ข้างบน</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>j = 0 (คอลัมน์แรก [0, 0, 2/5]): \(\pi_0=0\pi_0+0\pi_1+\tfrac25\pi_2=\tfrac25\pi_2\)</div></div>
      <div class="step"><span class="n">2</span><div>j = 1 (คอลัมน์ [1/4, 1/2, 2/5]) แทน π₀: \[\pi_1=\tfrac14\left(\tfrac25\pi_2\right)+\tfrac12\pi_1+\tfrac25\pi_2\ \Rightarrow\ \tfrac12\pi_1=\tfrac12\pi_2\ \Rightarrow\ \pi_1=\pi_2\]</div></div>
      <div class="step"><span class="n">3</span><div>ใช้ Σπ = 1 แทนสมการ j = 2: \(\tfrac25\pi_2+\pi_2+\pi_2=1\Rightarrow\pi_2=\tfrac{5}{12}\) <span class="ans">π = [1/6, 5/12, 5/12]</span></div></div>
      <div class="step"><span class="n">✓</span><div>ตรวจคอลัมน์สุดท้าย: (3/4)(1/6) + (1/2)(5/12) + (1/5)(5/12) = 1/8 + 5/24 + 1/12 = 10/24 = 5/12 ✓</div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-mc">
    <span class="tag">ลองเอง</span><h4>แก้ π = πP และคำนวณ Pᵐ</h4>
    <p class="hint">พิมพ์เป็นเศษส่วนได้ เช่น 1/4 · ค่าเริ่มต้นคือ Ex 12.4</p>
    <div class="fields">
      <label for="mc-size">จำนวน state<select id="mc-size"><option>2</option><option selected>3</option><option>4</option></select></label>
      <label for="mc-m">m สำหรับ Pᵐ<input id="mc-m" type="number" value="20" min="0" max="500"></label>
    </div>
    <div id="mc-grid" class="matrix"></div>
    <div id="mc-v"></div>
    <div class="out" id="mc-out"></div>
    <div class="mout" id="mc-pm"></div>
  </div>
</section>

<!-- ===================== Queueing ===================== -->
<section class="ch" id="queue">
  <div class="ch-head"><span class="lec">L26–27</span><span class="sec">Queueing · Little's theorem · M/M/1</span><h2>ทฤษฎีคิว: Little's theorem และ M/M/1</h2></div>
  <p class="xref">ออกสอบจริง: <a href="#exam-q5">ข้อสอบ Final 2564 ข้อ 5</a></p>
  <div class="idea">
    <span class="tag">เข้าใจก่อน</span>
    <p>ใน data network ลูกค้า = packet และ service time = เวลาส่ง packet ออกสาย = L/C (ความยาว bits / ความเร็วสาย bits/s) คำถามหลักคือ เฉลี่ยแล้วมีของค้างในระบบกี่ตัว และแต่ละตัวต้องรอนานเท่าไร</p>
    <p><b>Little's theorem</b> ตอบเชื่อมสองคำถามนี้ด้วยสูตรเดียว N = λT ใช้ได้กับแทบทุกระบบที่เข้าสู่ steady state · ส่วน <b>M/M/1</b> คือคิวที่ลูกค้ามาแบบ Poisson บริการนานแบบ exponential และมี server ตัวเดียว ซึ่งวิเคราะห์ได้เป็น Markov chain</p>
  </div>
  <div class="plate"><span class="tag">ตัวอย่าง service time (สไลด์แรก L26)</span>
  \[L=2304\times8\text{ bits},\ C=300\times10^6\text{ bits/s}\ \Rightarrow\ \text{Service time}=\frac{L}{C}=\frac{2304\times8}{300\times10^6}=61.44\ \mu\text{s}\]</div>
  <div class="tw"><table class="t">
    <thead><tr><th>สัญลักษณ์</th><th>ความหมาย</th></tr></thead>
    <tbody>
      <tr><td>λ</td><td>arrival rate (ลูกค้าเข้าเฉลี่ยต่อหน่วยเวลา)</td></tr>
      <tr><td>μ = 1/X̄</td><td>service rate · X̄ = average service time</td></tr>
      <tr><td>N</td><td>จำนวนลูกค้าเฉลี่ยในระบบ (ในคิว + กำลังรับบริการ)</td></tr>
      <tr><td>T</td><td>เวลาเฉลี่ยที่ลูกค้าอยู่ในระบบ (รอ + บริการ)</td></tr>
      <tr><td>N_Q</td><td>จำนวนเฉลี่ยที่รอในคิว</td></tr>
      <tr><td>W</td><td>เวลารอเฉลี่ยในคิว (ไม่รวมบริการ)</td></tr>
      <tr><td>ρ = λ/μ</td><td>utilization factor = สัดส่วนเวลาที่ server ไม่ว่าง (สูงสุด 1)</td></tr>
    </tbody></table></div>

  <h3>3.2.1 Little's theorem</h3>
  <div class="two">
    <div><svg viewBox="0 0 380 150" role="img" aria-label="ภาพ Little's theorem">
      <defs><marker id="ah2" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path class="arrowhead" d="M0,0 L10,5 L0,10 z"/></marker></defs>
      <path class="edge" d="M10,75 L105,75" marker-end="url(#ah2)"/><text class="lbl-a" x="55" y="64" text-anchor="middle">λ เข้า</text>
      <rect class="box" x="110" y="25" width="160" height="100" rx="10"/>
      <text class="lbl" x="190" y="64" text-anchor="middle">ในระบบเฉลี่ย N ตัว</text>
      <text class="lbl" x="190" y="92" text-anchor="middle">อยู่ตัวละ T</text>
      <path class="edge" d="M275,75 L370,75" marker-end="url(#ah2)"/><text class="lbl-a" x="322" y="64" text-anchor="middle">ออก</text>
    </svg></div>
    <div style="display:flex;flex-direction:column;justify-content:center;gap:8px">
      <p>คิดง่ายๆ: ถ้ามีคนเข้า 2 คนต่อนาที และแต่ละคนอยู่ 5 นาที ในร้านจะมีคนเฉลี่ย 2 × 5 = 10 คน</p>
      <p style="color:var(--muted);font-size:.9rem">ใช้กับ "ระบบ" ส่วนไหนก็ได้ ทั้งระบบ เฉพาะคิว หรือเฉพาะ server</p>
    </div>
  </div>
  <div class="plate"><span class="tag">Time averages (3.1) → ลิมิต</span>
  \[N_t=\frac1t\int_0^tN(\tau)\,d\tau,\qquad\lambda_t=\frac{\alpha(t)}{t},\qquad T_t=\frac{\sum_{i=0}^{\alpha(t)}T_i}{\alpha(t)},\qquad N=\lim_{t\to\infty}N_t,\ \lambda=\lim_{t\to\infty}\lambda_t,\ T=\lim_{t\to\infty}T_t\]
  \[N=\lambda T,\qquad N_Q=\lambda W,\qquad\rho=\lambda\overline X\]</div>
  <div class="ex">
    <h4><span class="no">Ex 3.1</span>Little's theorem กับสายส่ง</h4>
    <div class="q"><b>โจทย์:</b> λ = arrival rate ของสายส่ง, N_Q = จำนวน packet เฉลี่ยที่รอในคิว (ไม่รวมตัวที่กำลังส่ง), W = เวลารอเฉลี่ยในคิว และ X̄ = เวลาส่งเฉลี่ย Little's theorem ให้อะไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>มองเฉพาะคิวเป็นระบบ ⇒ \(N_Q=\lambda W\)</div></div>
      <div class="step"><span class="n">2</span><div>มองเฉพาะสายส่งเป็นระบบ: เวลาอยู่ = X̄, จำนวนในระบบ = จำนวนที่กำลังส่ง ∈ {0, 1} ⇒ \(\rho=\lambda\overline X\) = สัดส่วนเวลาที่สายไม่ว่าง <span class="ans">utilization factor ≤ 1</span></div></div>
      <div class="step"><span class="n">*</span><div>สไลด์มีโน้ต "X̄ = μ" แต่ที่ถูกคือ X̄ = 1/μ จึงได้ ρ = λ/μ</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">Ex 3.2</span>Network หลาย node</h4>
    <div class="q"><b>โจทย์:</b> packet เข้า network ที่ n node ด้วย rate λ₁, …, λₙ ถ้า N = จำนวน packet เฉลี่ยทั้ง network delay เฉลี่ยต่อ packet คือเท่าไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>มองทั้ง network เป็นระบบเดียว arrival rate รวม = Σλᵢ <span class="ans">\(T=N/\sum_{i=1}^n\lambda_i\)</span> ไม่ขึ้นกับ packet length distribution หรือ routing · ใช้รายโหนดได้ Nᵢ = λᵢTᵢ</div></div>
    </div></details>
  </div>

  <h3>M/M/1 เป็น Markov chain</h3>
  <div class="prose"><p>ดูระบบที่เวลา 0, δ, 2δ, … (δ เล็กมาก) ให้ Nₖ = จำนวนลูกค้าที่เวลา kδ ในช่วงสั้นๆ แทบจะเกิดได้ทีละเหตุการณ์เดียว: มาใหม่ 1 คน (โอกาส λδ) หรือบริการเสร็จ 1 คน (โอกาส μδ) จึงได้ chain แบบ birth–death</p></div>
  <figure><div class="fbox"><svg viewBox="0 0 600 150" role="img" aria-label="birth-death chain ของ M/M/1">
    <defs>
      <marker id="ah3" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path class="arrowhead" d="M0,0 L10,5 L0,10 z"/></marker>
      <marker id="ah4" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path class="arrowhead-b" d="M0,0 L10,5 L0,10 z"/></marker>
    </defs>
    <path class="edge" d="M82,62 Q130,22 178,62" marker-end="url(#ah3)"/><text class="lbl-a" x="130" y="30" text-anchor="middle">λδ</text>
    <path class="edge" d="M222,62 Q270,22 318,62" marker-end="url(#ah3)"/><text class="lbl-a" x="270" y="30" text-anchor="middle">λδ</text>
    <path class="edge" d="M362,62 Q410,22 458,62" marker-end="url(#ah3)"/><text class="lbl-a" x="410" y="30" text-anchor="middle">λδ</text>
    <path class="edge-b" d="M178,98 Q130,138 82,98" marker-end="url(#ah4)"/><text class="lbl-t" x="130" y="140" text-anchor="middle">μδ</text>
    <path class="edge-b" d="M318,98 Q270,138 222,98" marker-end="url(#ah4)"/><text class="lbl-t" x="270" y="140" text-anchor="middle">μδ</text>
    <path class="edge-b" d="M458,98 Q410,138 362,98" marker-end="url(#ah4)"/><text class="lbl-t" x="410" y="140" text-anchor="middle">μδ</text>
    <circle class="node" cx="60" cy="80" r="24"/><text class="node-t" x="60" y="85" text-anchor="middle">0</text>
    <circle class="node" cx="200" cy="80" r="24"/><text class="node-t" x="200" y="85" text-anchor="middle">1</text>
    <circle class="node" cx="340" cy="80" r="24"/><text class="node-t" x="340" y="85" text-anchor="middle">2</text>
    <circle class="node" cx="480" cy="80" r="24"/><text class="node-t" x="480" y="85" text-anchor="middle">3</text>
    <text class="node-t" x="550" y="85" text-anchor="middle">…</text>
  </svg></div><figcaption>ลูกศรขึ้น (ฟ้า) = ลูกค้ามาใหม่, ลูกศรลง (ส้ม) = บริการเสร็จ · ที่เหลือคืออยู่ที่เดิม (P₀₀ = 1 − λδ, Pᵢᵢ = 1 − λδ − μδ)</figcaption></figure>
  <div class="plate"><span class="tag">(3.15)–(3.19)</span>
  \[\begin{aligned}P_{00}&=1-\lambda\delta+o(\delta)\\P_{ii}&=1-\lambda\delta-\mu\delta+o(\delta),&&i\ge1\\P_{i,i+1}&=\lambda\delta+o(\delta),&&i\ge0\\P_{i,i-1}&=\mu\delta+o(\delta),&&i\ge1\\P_{ij}&=o(\delta),&&j\ne i,i+1,i-1\end{aligned}\qquad\lim_{\delta\to0}\frac{o(\delta)}{\delta}=0\]
  <p>เช่น P{ไม่มีใครมาและไม่มีใครออกใน δ} = e^(−λδ)e^(−μδ) = 1 − λδ − μδ + o(δ) โดยใช้ eˣ = 1 + x + x²/2! + ⋯</p></div>

  <h3>Derivation: stationary distribution</h3>
  <div class="steps" style="display:flex;flex-direction:column;gap:12px;max-width:74ch;margin-top:12px">
    <div class="step"><span class="n">1</span><div><b>Balance:</b> ใน steady state ความถี่ที่ข้ามจาก n → n + 1 ต้องเท่ากับ n + 1 → n (ไม่งั้นจำนวนจะไหลสะสมด้านใดด้านหนึ่ง) \[p_n\lambda\delta+o(\delta)=p_{n+1}\mu\delta+o(\delta)\ \Rightarrow\ p_n\lambda=p_{n+1}\mu\quad(3.20)\]</div></div>
    <div class="step"><span class="n">2</span><div>\(p_{n+1}=\rho p_n\) ⇒ \(p_n=\rho^np_0\) (3.21)</div></div>
    <div class="step"><span class="n">3</span><div><b>Normalize</b> (ต้องการ ρ &lt; 1 ให้ geometric series ลู่เข้า) \[1=\sum_{n=0}^\infty\rho^np_0=\frac{p_0}{1-\rho}\ (3.22)\ \Rightarrow\ p_n=\rho^n(1-\rho)\ \ (3.23)\]</div></div>
    <div class="step"><span class="n">4</span><div><b>N</b> \[N=\sum_{n=0}^\infty n\rho^n(1-\rho)=\rho(1-\rho)\frac{\partial}{\partial\rho}\left(\frac{1}{1-\rho}\right)=\frac{\rho}{1-\rho}=\frac{\lambda}{\mu-\lambda}\quad(3.24)\]</div></div>
    <div class="step"><span class="n">5</span><div><b>T, W, N_Q</b> จาก Little \[T=\frac N\lambda=\frac{1}{\mu-\lambda}\ (3.26),\qquad W=T-\frac1\mu=\frac{\rho}{\mu-\lambda},\qquad N_Q=\lambda W=\frac{\rho^2}{1-\rho}\]</div></div>
  </div>
  <figure><div class="fbox"><svg id="fig-nrho" role="img" aria-label="กราฟ N เทียบ rho"></svg></div>
    <figcaption>Figure 3.7: N = ρ/(1 − ρ) โตช้าตอน ρ เล็ก แต่พุ่งขึ้นเร็วมากเมื่อ ρ → 1 ระบบที่ใช้งานเกือบเต็มจึงมีคิวยาวมาก</figcaption></figure>

  <div class="ex">
    <h4><span class="no">Ex 3.8</span>เพิ่ม arrival rate และ transmission rate ด้วย factor เดียวกัน</h4>
    <div class="q"><b>โจทย์:</b> arrival rate เพิ่มจาก λ เป็น Kλ (K &gt; 1) packet length distribution เท่าเดิม แต่ความจุสายเพิ่ม K เท่า เวลาส่งเฉลี่ยจึงเป็น 1/(Kμ) จะเกิดอะไรขึ้น</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>\[\frac LC=\frac1\mu\to\frac{L}{KC}=\frac{1}{K\mu},\qquad\rho=\frac{K\lambda}{K\mu}=\frac\lambda\mu\ \Rightarrow\ N=\frac{\rho}{1-\rho}\] ρ ไม่เปลี่ยน ⇒ N เท่าเดิม</div></div>
      <div class="step"><span class="n">2</span><div>\(T=N/(K\lambda)\) <span class="ans">delay ลดลง K เท่า</span> สายเร็วขึ้น K เท่ารับ packet ได้มากขึ้น K เท่า โดยแต่ละ packet รอสั้นลง K เท่า</div></div>
    </div></details>
  </div>
  <div class="ex">
    <h4><span class="no">ร้านซ่อมนาฬิกา</span>M/M/1 เต็มรูปแบบ</h4>
    <div class="q"><b>โจทย์:</b> ลูกค้ามาตาม Poisson process อัตรา 1 คนต่อ 10 นาที service time เป็น exponential mean 8 นาที (a) หา N, T, W (b) ถ้า arrival rate เพิ่มเป็น 10λ/9 จะเป็นเท่าไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">0</span><div>λ = 1/10, 1/μ = 8 ⇒ μ = 1/8 ⇒ ρ = 0.8 &lt; 1 ✓</div></div>
      <div class="step"><span class="n">a</span><div>\[N=\frac{1/10}{1/8-1/10}=4,\qquad T=\frac{1}{1/8-1/10}=40\text{ min},\qquad W=40-8=32\text{ min}\] <span class="ans">N = 4, T = 40 นาที, W = 32 นาที</span></div></div>
      <div class="step"><span class="n">b</span><div>λ′ = (10/9)(1/10) = 1/9 ⇒ μ − λ′ = 1/72, ρ = 8/9 \[N=\frac{1/9}{1/72}=8,\qquad T=72\text{ min},\qquad W=64\text{ min}\] <span class="ans">N = 8, T = 72 นาที, W = 64 นาที</span></div></div>
      <div class="step"><span class="n">*</span><div>Arrival เพิ่มแค่ราว 11% แต่ N และ W เพิ่มเป็น 2 เท่า เพราะ ρ ขยับจาก 0.8 เป็น 0.89 ใกล้ 1</div></div>
    </div></details>
  </div>

  <div class="tool" id="tool-mm1">
    <span class="tag">ลองเอง</span><h4>เครื่องคิด M/M/1</h4>
    <p class="hint">ใส่เป็นเวลาเฉลี่ย หน่วยเดียวกันทั้งสองช่อง · ค่าเริ่มต้นคือร้านซ่อมนาฬิกาข้อ (a) ลองเปลี่ยน 10 เป็น 9 เพื่อดูข้อ (b)</p>
    <div class="fields">
      <label for="mm-a">เวลาเฉลี่ยระหว่างลูกค้า 1/λ<input id="mm-a" type="number" value="10" step="any" min="0.0001"></label>
      <label for="mm-s">เวลาบริการเฉลี่ย 1/μ<input id="mm-s" type="number" value="8" step="any" min="0.0001"></label>
    </div>
    <div class="out" id="mm-out"></div>
    <div id="mm-v"></div>
    <figure><div class="fbox"><svg id="fig-pn" role="img" aria-label="กราฟ p_n ของ M/M/1"></svg></div><figcaption>pₙ = ρⁿ(1 − ρ) = ความน่าจะเป็นที่ในระบบมี n ตัว</figcaption></figure>
  </div>
</section>


<!-- ===================== Past exam ===================== -->
<section class="ch" id="exam">
  <div class="ch-head"><span class="lec">EXAM</span><span class="sec">Final · ภาคต้น ปีการศึกษา 2564 (10 พ.ย. 2021) · Open book · 3 ชั่วโมง</span><h2>ข้อสอบเก่า Final 2564 พร้อมเฉลยละเอียด</h2></div>
  <div class="idea">
    <span class="tag">อ่านก่อน: ข้อสอบชุดนี้ตัวเลขไม่เหมือนกันทุกคน</span>
    <p>ข้อสอบมี 5 ข้อ ข้อละ 6 คะแนน (ข้อที่มี n ข้อย่อย ข้อย่อยละ 6/n คะแนน) ตัวเลขในแต่ละข้อขึ้นกับค่า <b>j ∈ {1, 2, 3, 4}</b> ที่คำนวณจากรหัสนักศึกษา จึงต้องหา j ให้ถูกก่อน ผิดตรงนี้คือผิดทั้งฉบับ</p>
    <ol>
      <li>x₀ = เลขสองหลักสุดท้ายของรหัส โดยไม่นับขีด เช่น รหัส 613040102-6 ได้ x₀ = 26</li>
      <li>\(\mathring{\pi}\)(i) = เลขโดดตำแหน่งทศนิยมที่ i ของ \(\hat\pi\) (\(\hat\pi\) ในข้อสอบ <b>ไม่ใช่ π จริง</b> ตัวเลขบางหลักถูกเปลี่ยน ต้องนับจากที่ให้มา) ถ้า i ไม่อยู่ใน 1–105 ให้ \(\mathring{\pi}\)(i) = 5</li>
      <li>j = 2^\(\mathring{\pi}\)(x₀) mod 5 · ตัวอย่างในข้อสอบ: \(\mathring{\pi}\)(26) = 8 ⇒ j = 2⁸ mod 5 = 256 mod 5 = 1</li>
    </ol>
  </div>
  <div class="plate"><span class="tag">π̂ (pi hat) ที่ให้ในข้อสอบ (105 ตำแหน่ง)</span>
    <p class="mono" style="color:var(--ink);font-size:.9rem;line-height:1.9;word-spacing:.35em">3.14159 26535 89793 23846 26433 83279 51288 41971 69399 37512<br>58239 74944 59234 78164 56286 26899 86287 34825 34211 78679 82148</p>
    <p>ทางลัด: 2^d mod 5 วนทุก 4 ค่า (2⁰ → 1, 2¹ → 2, 2² → 4, 2³ → 3, 2⁴ → 1, …) จึงดูแค่ d mod 4 ก็พอ</p>
  </div>
  <div class="tool" id="tool-j">
    <span class="tag">ลองเอง</span><h4>หา j จากรหัส แล้วดูเฉลยตาม j ของคุณ</h4>
    <p class="hint">พิมพ์รหัสนักศึกษา หรือเลือก j ตรงๆ ก็ได้ · ค่าเริ่มต้น j = 2 ตรงกับชีทเฉลยลายมือที่แนบมา · ตัวเลขเฉลยทุกข้อด้านล่างจะเปลี่ยนตาม</p>
    <div class="fields">
      <label for="ej-id">รหัสนักศึกษา<input id="ej-id" type="text" placeholder="613040102-6" autocomplete="off"></label>
      <label for="ej-j">j<select id="ej-j"><option>1</option><option selected>2</option><option>3</option><option>4</option></select></label>
    </div>
    <div id="ej-v"></div>
  </div>
  <div class="plate"><span class="tag">สูตรที่แนบมากับข้อสอบ</span>
    <p>Poisson, Geometry₁, exp(λ) พร้อม \(E[X^n]=n!/\lambda^n\), Laplace \(f(x)=\frac\lambda2e^{-\lambda|x|}\) · \(\frac{1}{1-z}=\sum z^n\), \(e^z=\sum\frac{z^n}{n!}\), อนุพันธ์พื้นฐาน, product rule, \(\int_0^\infty x^ne^{-ax}dx=\frac{n!}{a^{n+1}}\), \(\int e^{ax}dx=\frac1ae^{ax}\) · ตาราง Φ(x) และ Q(x) = 1 − Φ(x) ที่ x = 0.0–3.9 · ตาราง y_α/2 ต้องเตรียมไปเอง (open book)</p>
  </div>

  <!-- Q1 -->
  <div class="ex" id="exam-q1">
    <h4><span class="no">ข้อ 1</span>Confidence interval ของ mean เมื่อรู้ variance</h4>
    <div class="q"><b>โจทย์:</b> กำหนดลำดับ (αⱼ) = (0.02, 0.03, 0.04, 0.05) และ α = αⱼ ให้ X₁, X₂, …, X₁₀₁ เป็น i.i.d. random variables ที่มี variance เท่ากับ 2.1 ถ้า sample mean เท่ากับ \(8.32+\frac{j}{1000}\) จงหา (1 − α) confidence interval สำหรับ E(X₃) (ใช้ทศนิยม 5 ตำแหน่ง)</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div><b>อ่านโจทย์ให้ออก:</b> Xᵢ เป็น i.i.d. ดังนั้น E(X₃) = E(Xᵢ) = m ตัวเลข 3 ไม่ได้มีความหมายพิเศษ · รู้ σ² = 2.1 และ n = 101 ใหญ่ ⇒ ใช้กรณี 6.3 (รู้ σ, CLT)</div></div>
      <div class="step"><span class="n">2</span><div>เลือก α = αⱼ แล้วหา y จาก \(\Phi(y_{\alpha/2})=1-\alpha/2\) (Table 6.2)</div></div>
      <div class="step"><span class="n">3</span><div>\[m=M_n\pm\frac{\sigma\,y_{\alpha/2}}{\sqrt n}=\left(8.32+\frac{j}{1000}\right)\pm\frac{\sqrt{2.1}\;y_{\alpha/2}}{\sqrt{101}}\]</div></div>
      <div class="step"><span class="n">=</span><div><div class="out" id="ex1-out"></div></div></div>
    </div></details>
    <div class="warn"><span class="tag">จุดที่ชีทเฉลยลายมือพลาด</span><p>ชีทใช้ \(\sqrt3\) เป็นตัวหาร (น่าจะสับสนกับเลข 3 ใน E(X₃)) ได้ δ = 1.81555 และช่วง [6.50645, 10.13755] ที่ถูกต้องคือหารด้วย \(\sqrt{n}=\sqrt{101}\) ช่วงจึงแคบกว่ามาก</p></div>
  </div>

  <!-- Q2 -->
  <div class="ex" id="exam-q2">
    <h4><span class="no">ข้อ 2</span>Hypothesis test: ใครได้ benefit of the doubt</h4>
    <div class="q"><b>โจทย์:</b> ผู้ให้บริการอินเทอร์เน็ตอ้างว่าการเชื่อมโยงมีความน่าจะเป็นของการสูญหายหนึ่งแพ็กเกตไม่เกิน p₀ นักศึกษาจึงทดสอบข้ออ้างนี้ด้วยการทดลองส่ง n แพ็กเกต ให้ Xᵢ = 1 ถ้าแพ็กเกตลำดับที่ i สูญหาย และ Xᵢ = 0 ถ้าไม่สูญหาย สมมตินักศึกษาคำนวณได้ \(Z_n=\frac{M_n-p_0}{S_n/\sqrt n}=1.4+\frac{j}{100}\)<br>(a) ถ้าผู้ให้บริการเอา E[Xᵢ] ≤ p₀ เป็น null hypothesis เขาควรยอมรับข้ออ้างนี้ ณ ระดับนัยสำคัญ 0.07 หรือไม่ เพราะอะไร<br>(b) ถ้านักศึกษาต้องการจับผิดด้วยการเอา E[Xᵢ] &gt; p₀ เป็น null hypothesis นักศึกษาควรยอมรับข้ออ้างของผู้ให้บริการ ณ ระดับนัยสำคัญ 0.07 หรือไม่ เพราะอะไร</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">0</span><div>ทั้งสองข้อเป็น <b>one-tailed</b> ⇒ ใช้ y_α ไม่ใช่ y_α/2 · α = 0.07 ⇒ Table 6.6: \(y_\alpha=1.476\) (ชีทหาค่า y_α/2 = 1.812 มาด้วยแต่ไม่ได้ใช้)</div></div>
      <div class="step"><span class="n">a</span><div>H₀: E[Xᵢ] ≤ p₀ vs H₁: E[Xᵢ] &gt; p₀ ⇒ <b>แบบ 1</b> ⇒ ยอมรับ H₀ เมื่อ \(Z_n\le y_\alpha\) (rejection region คือ \(Z_n&gt;1.476\))</div></div>
      <div class="step"><span class="n">b</span><div>H₀: E[Xᵢ] &gt; p₀ vs H₁: E[Xᵢ] ≤ p₀ ⇒ <b>แบบ 2</b> ⇒ ยอมรับ H₀ เมื่อ \(Z_n&gt;-y_\alpha\) (rejection region คือ \(Z_n\le-1.476\))</div></div>
      <div class="step"><span class="n">=</span><div><div class="out" id="ex2-out"></div><div id="ex2-v"></div></div></div>
      <div class="step"><span class="n">*</span><div><b>บทเรียนของข้อนี้:</b> data ชุดเดียวกันให้ข้อสรุปตรงข้ามกันได้ ขึ้นกับว่าใครได้ benefit of the doubt · (a) ข้ออ้างอยู่ใน H₀ จึงรอดเพียงเพราะ data ค้านไม่แรงพอ ซึ่งเป็นหลักฐานที่อ่อน · (b) ข้ออ้างอยู่ใน H₁ ต้องมีหลักฐานหนักแน่น แต่ Zₙ เป็นบวก (Mₙ &gt; p₀) จึงไม่มีหลักฐานสนับสนุนเลย ฝั่งผู้ซื้อบริการจึงควรตั้งแบบ (b)</div></div>
    </div></details>
    <div class="warn"><span class="tag">จุดที่ชีทเฉลยลายมือพลาด</span><p>ชีทหาช่วง reject ถูกทั้งสองข้อ แต่สรุปกลับด้าน (a) เขียนว่า "ไม่ควรยอมรับ" และ (b) เขียนว่า "ควรยอมรับ" ทั้งที่ Zₙ ไม่ตกในช่วง reject ทั้งสองกรณี ซึ่งแปลว่ายอมรับ H₀ ทั้งคู่ คำตอบที่ตรงกับเกณฑ์คือ (a) ยอมรับข้ออ้าง (b) ไม่ยอมรับข้ออ้าง</p></div>
  </div>

  <!-- Q3 -->
  <div class="ex" id="exam-q3">
    <h4><span class="no">ข้อ 3</span>Joint density จาก joint cdf</h4>
    <div class="q"><b>โจทย์:</b> ให้ \(F_{XY}(x,y)=\begin{cases}\dfrac{x+e^{-y(x+j)}}{x+j}-e^{-y},&x,y&gt;0\\0,&\text{otherwise}\end{cases}\) จงหา joint density \(f_{XY}(x,y)\)</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>ข้อนี้คือ Ex 7.12 ที่สลับบทบาท x กับ y และเปลี่ยน 1 เป็น j · ใช้ \(f_{XY}=\frac{\partial^2}{\partial x\,\partial y}F_{XY}\) และดิฟเทียบ y ก่อนจะง่ายกว่า · แยกเทอม \[F_{XY}=\frac{x}{x+j}+\frac{e^{-y(x+j)}}{x+j}-e^{-y}\]</div></div>
      <div class="step"><span class="n">2</span><div>ดิฟเทียบ y: เทอมแรกไม่มี y จึงเป็น 0 · เทอมสองดึง −(x + j) ออกมาตัดกับตัวส่วน \[\frac{\partial F_{XY}}{\partial y}=0+\frac{-(x+j)\,e^{-y(x+j)}}{x+j}+e^{-y}=e^{-y}-e^{-y(x+j)}\]</div></div>
      <div class="step"><span class="n">3</span><div>ดิฟต่อเทียบ x: e⁻ʸ ไม่มี x จึงหายไป \[\frac{\partial^2F_{XY}}{\partial x\,\partial y}=-(-y)\,e^{-y(x+j)}=y\,e^{-y(x+j)}\]</div></div>
      <div class="step"><span class="n">=</span><div>\[f_{XY}(x,y)=\begin{cases}y\,e^{-y(x+j)},&x,y&gt;0\\0,&\text{otherwise}\end{cases}\] <span class="ans" id="ex3-ans"></span></div></div>
      <div class="step"><span class="n">*</span><div>ข้อสังเกตเสริม (ไม่ต้องตอบในข้อสอบ): ถ้า integrate ตรวจจะได้ \(\iint f_{XY}=\int_0^\infty e^{-jy}dy=1/j\) ฟังก์ชันนี้จึงเป็น density ที่ถูกต้องเฉพาะ j = 1 ข้อสอบต้องการวัดแค่การดิฟสองครั้ง</div></div>
    </div></details>
  </div>

  <!-- Q4 -->
  <div class="ex" id="exam-q4">
    <h4><span class="no">ข้อ 4</span>Carrier with random phase</h4>
    <div class="q"><b>โจทย์:</b> ระบบสื่อสารมีสัญญาณพาหะที่เครื่องรับเป็น \(X_t=\cos\!\left(\frac{\sqrt5}{j}\pi t+\Theta\right)\) โดย Θ ~ uniform[−π, π] (a) จงหาฟังก์ชันค่าเฉลี่ยของ X_t (b) จงหาฟังก์ชันสหสัมพันธ์ของ X_t</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">0</span><div>ข้อนี้คือ Ex 10.8 ที่ \(2\pi f\) ถูกแทนด้วย \(\omega_0=\frac{\sqrt5}{j}\pi\) วิธีทำเหมือนเดิมทุกขั้น</div></div>
      <div class="step"><span class="n">a</span><div>Fix t แล้ว integrate เทียบ θ ครบหนึ่งคาบ \[m_X(t)=E[X_t]=\int_{-\pi}^{\pi}\cos(\omega_0t+\theta)\frac{d\theta}{2\pi}=\frac{\sin(\omega_0t+\theta)}{2\pi}\Bigg|_{\theta=-\pi}^{\theta=\pi}=0\]</div></div>
      <div class="step"><span class="n">b</span><div>ใช้ \(\cos A\cos B=\tfrac12[\cos(A+B)+\cos(A-B)]\) \[R_X(t_1,t_2)=\tfrac12E\big[\cos(\omega_0(t_1+t_2)+2\Theta)\big]+\tfrac12\cos(\omega_0(t_1-t_2))=\frac12\cos\!\left(\frac{\sqrt5}{j}\pi(t_1-t_2)\right)\] เทอมแรกเป็น 0 เพราะ integrate 2θ ครบสองคาบ</div></div>
      <div class="step"><span class="n">=</span><div><div class="out" id="ex4-out"></div></div></div>
      <div class="step"><span class="n">+</span><div>ถ้ามีข้อ (c) ถามต่อ: mean คงที่และ R_X ขึ้นกับ t₁ − t₂ เท่านั้น ⇒ X_t เป็น WSS (Ex 10.17)</div></div>
    </div></details>
    <div class="warn"><span class="tag">จุดที่ชีทเฉลยลายมือพลาด</span><p>ชีทตอบ \(R_X=\cos\!\left(\frac{\sqrt5}{2}\pi(t_1-t_2)\right)\) ขาดตัวคูณ ½ ที่มาจาก identity ของ cos A cos B</p></div>
  </div>

  <!-- Q5 -->
  <div class="ex" id="exam-q5">
    <h4><span class="no">ข้อ 5</span>Little's theorem กับร้านอาหาร</h4>
    <div class="q"><b>โจทย์:</b> ลูกค้ามาที่ร้านอาหารญี่ปุ่นเพื่อซื้ออาหารด้วยอัตรา 5 คนต่อนาที แต่ละคนรออาหารเฉลี่ย 5 นาที ความน่าจะเป็นที่ลูกค้ารับประทานที่ร้านคือ 0.5 และห่อกลับบ้านคือ 0.5 ลูกค้าที่รับประทานที่ร้านใช้เวลาโดยเฉลี่ย 20 + j นาที จงหาจำนวนลูกค้าเฉลี่ยในร้าน (Hint: ประยุกต์ Little's theorem)</div>
    <details class="sol" open><summary>วิธีทำ</summary><div class="steps">
      <div class="step"><span class="n">1</span><div>มองทั้งร้านเป็นระบบเดียว ⇒ ใช้ \(N=\lambda T\) โดย λ = 5 คน/นาที ไม่ต้องสนว่าข้างในเป็นคิวแบบไหน (ไม่ใช่ M/M/1)</div></div>
      <div class="step"><span class="n">2</span><div>T = เวลาเฉลี่ยที่ลูกค้าหนึ่งคนอยู่ในร้าน ใช้ค่าคาดหวังแบบมีเงื่อนไข: ทุกคนรอ 5 นาที แล้วครึ่งหนึ่งนั่งกินต่ออีก 20 + j นาที \[T=0.5\,(5+20+j)+0.5\,(5)=5+\frac{20+j}{2}\]</div></div>
      <div class="step"><span class="n">3</span><div>\[N=\lambda T=5\left(5+\frac{20+j}{2}\right)=75+2.5j\]</div></div>
      <div class="step"><span class="n">=</span><div><div class="out" id="ex5-out"></div></div></div>
    </div></details>
    <div class="warn"><span class="tag">ระวังการตีความ</span><p>เฉลยนี้ (และชีทลายมือ) ตีความว่า 20 + j นาทีคือเวลานั่งกิน <b>เพิ่มจาก</b> การรอ 5 นาที ถ้าตีความว่า 20 + j รวมเวลารอแล้ว จะได้ T = 0.5(20 + j) + 0.5(5) และ N = 62.5 + 2.5j ในห้องสอบควรเขียนสมมติฐานที่ใช้กำกับไว้</p></div>
  </div>
</section>

<!-- ===================== Critical values ===================== -->
<section class="ch" id="crit">
  <div class="ch-head"><span class="lec">CALC</span><h2>หาค่าวิกฤตแทนการเปิดตาราง</h2></div>
  <div class="prose"><p>ทุกตารางในสไลด์ (6.1–6.6) คือการแก้ F(x) = p สำหรับ normal, Student t หรือ χ² เครื่องนี้แก้ให้ตรงๆ เหมือนคำสั่ง <code>cum_distribution_function_inv</code> ใน SageMath</p></div>
  <div class="tool" id="tool-crit">
    <span class="tag">ลองเอง</span><h4>Inverse cdf</h4>
    <div class="fields">
      <label for="cr-d">การแจกแจง<select id="cr-d"><option value="n">Normal Φ</option><option value="t">Student t</option><option value="c" selected>Chi-squared χ²</option></select></label>
      <label for="cr-k">degrees of freedom<input id="cr-k" type="number" value="7" min="1"></label>
      <label for="cr-p">p = F(x)<input id="cr-p" type="number" value="0.95" step="0.001" min="0.0001" max="0.9999"></label>
      <label for="cr-x">หรือใส่ x เพื่อหา F(x)<input id="cr-x" type="number" value="7.01" step="any"></label>
    </div>
    <div class="out" id="cr-out"></div>
    <div class="tw"><table class="t">
      <thead><tr><th>ต้องการ</th><th>ใส่ p เท่าไร</th></tr></thead>
      <tbody>
        <tr><td>y_α/2 ของ CI mean / two-tailed test</td><td>Normal หรือ t, p = 1 − α/2</td></tr>
        <tr><td>y_α ของ one-tailed test</td><td>Normal, p = 1 − α</td></tr>
        <tr><td>z_α ของ chi-squared test</td><td>χ² ที่ k = m − 1 − r, p = 1 − α</td></tr>
        <tr><td>ℓ และ u ของ CI variance</td><td>χ² ที่ n (รู้ m) หรือ n − 1, p = α/2 และ 1 − α/2</td></tr>
      </tbody></table></div>
  </div>
</section>

<!-- ===================== Checklist ===================== -->
<section class="ch" id="check">
  <div class="ch-head"><span class="lec">LIST</span><h2>จุดที่มักพลาด และ checklist ก่อนสอบ</h2></div>
  <div class="warn"><span class="tag">จุดพลาดบ่อย</span>
    <ul style="margin:0;padding-left:1.2em;display:flex;flex-direction:column;gap:6px">
      <li>dof ของ chi-squared test คือ m − 1 − r อย่าลืมลบ r ตามจำนวน parameter ที่ประมาณจาก data</li>
      <li>CI ใช้ α/2 เสมอ (Φ(y) = 0.975 ที่ 95% ไม่ใช่ 0.95) · test แบบ 1, 2 ใช้ α · แบบ 3 ใช้ α/2</li>
      <li>CI ของ σ²: u อยู่ใต้ขอบล่าง ℓ อยู่ใต้ขอบบน</li>
      <li>รู้ m ใช้ χ² n dof · ไม่รู้ m ใช้ n − 1 dof (Table 6.4 vs 6.5)</li>
      <li>Gaussian + ไม่รู้ σ + n เล็ก ต้องใช้ Student t ไม่ใช่ normal</li>
      <li>"มากกว่า 5" คือ ≥ 6 ⇒ 1 − Σ ตั้งแต่ k = 0 ถึง 5</li>
      <li>Ex 10.8 integrate เทียบ θ ไม่ใช่ t</li>
      <li>Claim ของใคร (burden of proof) อยู่ใน H₁</li>
      <li>Transition matrix: แต่ละ <b>แถว</b> รวมได้ 1 · π = πP ใช้ <b>คอลัมน์</b> ของ P</li>
      <li>M/M/1 ต้องเช็ก ρ = λ/μ &lt; 1 ก่อนใช้สูตร · X̄ = 1/μ ไม่ใช่ μ</li>
      <li>สไลด์ L19 เขียน P(Type II) = α ซึ่งไม่ถูก · โน้ต L15 เรียก 1/Mₙ ว่า unbiased แต่ textbook พิสูจน์แค่ strongly consistent</li>
    </ul>
  </div>
  <p class="progress" id="ck-prog"></p>
  <ul class="check" id="ck">
    <li><label><input type="checkbox" id="ck1"><span>พิสูจน์ unbiased + strongly consistent ของ (1/n)Σ(Xᵢ − m)² (Ex 6.1)</span></label></li>
    <li><label><input type="checkbox" id="ck2"><span>อธิบายได้ว่าทำไม Sₙ² หาร n − 1 และ S̃ₙ² biased (Ex 6.2)</span></label></li>
    <li><label><input type="checkbox" id="ck3"><span>หา estimator ของ p และ λ ด้วยการแทน E[Xᵢ] ด้วย Mₙ (Ex 6.3–6.4)</span></label></li>
    <li><label><input type="checkbox" id="ck4"><span>ทำ chi-squared test ครบ: pⱼ → Z → k → z_α → สรุป (Ex 6.7–6.9)</span></label></li>
    <li><label><input type="checkbox" id="ck5"><span>CI ของ mean ทั้ง 4 กรณี (Ex 6.10–6.13 + เลือกตั้ง)</span></label></li>
    <li><label><input type="checkbox" id="ck6"><span>CI ของ variance ทั้ง 2 กรณี (Ex 6.14–6.15)</span></label></li>
    <li><label><input type="checkbox" id="ck7"><span>ตั้ง H₀/H₁ จาก claim และหา critical region (Ex 6.16–6.18)</span></label></li>
    <li><label><input type="checkbox" id="ck8"><span>หา a, b ของ linear regression และอธิบาย orthogonality principle (Ex 6.21–6.22)</span></label></li>
    <li><label><input type="checkbox" id="ck9"><span>หา marginal cdf/density และเช็ก independence (Ex 7.7–7.13)</span></label></li>
    <li><label><input type="checkbox" id="ck10"><span>ใช้ law of total probability หา E[Y²X³] (Ex 7.18)</span></label></li>
    <li><label><input type="checkbox" id="ck11"><span>พิสูจน์ cov(AX) = ACA′ และหา joint density ด้วย Jacobian (Ex 8.3, 8.10)</span></label></li>
    <li><label><input type="checkbox" id="ck12"><span>หา mean/correlation function และพิสูจน์ WSS (Ex 10.8, 10.16, 10.17)</span></label></li>
    <li><label><input type="checkbox" id="ck13"><span>Poisson: Ex 11.1, cov = λ min(t₁, t₂), Tₙ เป็น Erlang</span></label></li>
    <li><label><input type="checkbox" id="ck14"><span>พิสูจน์ random walk เป็น Markov และหา stationary distribution (Ex 12.3–12.4)</span></label></li>
    <li><label><input type="checkbox" id="ck15"><span>M/M/1: derive pₙ = ρⁿ(1 − ρ) แล้วหา N, T, W, N_Q (ร้านซ่อมนาฬิกา, Ex 3.8)</span></label></li>
  </ul>
  <footer>เนื้อหาเรียบเรียงจากสไลด์ Lecture 14–27 ของรายวิชา EN813001 · ตัวเลขในเครื่องคิดเลขคำนวณสดในเบราว์เซอร์</footer>
</section>
</main>
</div>

<script>
window.MathJax={tex:{inlineMath:[['\\(','\\)']],displayMath:[['\\[','\\]']]},svg:{fontCache:'global'},options:{skipHtmlTags:['script','noscript','style','textarea','pre','code','select','option']}};
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3.2.2/es5/tex-svg.js" async></script>
<script>
/* ---------- numerics: gamma, beta, normal / t / chi-square cdf + inverse ---------- */
function lgamma(z){
  const c=[0.99999999999980993,676.5203681218851,-1259.1392167224028,771.32342877765313,-176.61502916214059,12.507343278686905,-0.13857109526572012,9.9843695780195716e-6,1.5056327351493116e-7];
  if(z<0.5) return Math.log(Math.PI/Math.abs(Math.sin(Math.PI*z)))-lgamma(1-z);
  z-=1; let x=c[0]; for(let i=1;i<9;i++) x+=c[i]/(z+i);
  const t=z+7.5; return 0.5*Math.log(2*Math.PI)+(z+0.5)*Math.log(t)-t+Math.log(x);
}
function gammaP(a,x){
  if(x<=0) return 0;
  const lp=-x+a*Math.log(x)-lgamma(a);
  if(x<a+1){ let ap=a,sum=1/a,del=sum; for(let n=0;n<1000;n++){ap++;del*=x/ap;sum+=del;if(Math.abs(del)<Math.abs(sum)*1e-15)break;} return Math.min(1,sum*Math.exp(lp)); }
  let b=x+1-a,c=1e300,d=1/b,h=d;
  for(let i=1;i<1000;i++){const an=-i*(i-a);b+=2;d=an*d+b;if(Math.abs(d)<1e-300)d=1e-300;c=b+an/c;if(Math.abs(c)<1e-300)c=1e-300;d=1/d;const del=d*c;h*=del;if(Math.abs(del-1)<1e-15)break;}
  return Math.max(0,1-Math.exp(lp)*h);
}
function betacf(a,b,x){
  const F=1e-300; let qab=a+b,qap=a+1,qam=a-1,c=1,d=1-qab*x/qap; if(Math.abs(d)<F)d=F; d=1/d; let h=d;
  for(let m=1;m<=500;m++){const m2=2*m;let aa=m*(b-m)*x/((qam+m2)*(a+m2));d=1+aa*d;if(Math.abs(d)<F)d=F;c=1+aa/c;if(Math.abs(c)<F)c=F;d=1/d;h*=d*c;
    aa=-(a+m)*(qab+m)*x/((a+m2)*(qap+m2));d=1+aa*d;if(Math.abs(d)<F)d=F;c=1+aa/c;if(Math.abs(c)<F)c=F;d=1/d;const del=d*c;h*=del;if(Math.abs(del-1)<1e-15)break;}
  return h;
}
function betaI(a,b,x){
  if(x<=0) return 0; if(x>=1) return 1;
  const bt=Math.exp(lgamma(a+b)-lgamma(a)-lgamma(b)+a*Math.log(x)+b*Math.log(1-x));
  return x<(a+1)/(a+b+2) ? bt*betacf(a,b,x)/a : 1-bt*betacf(b,a,1-x)/b;
}
const Phi=x=> x>=0 ? 0.5+0.5*gammaP(0.5,x*x/2) : 0.5-0.5*gammaP(0.5,x*x/2);
const phi=x=>Math.exp(-x*x/2)/Math.sqrt(2*Math.PI);
const tCdf=(t,v)=>{const ib=betaI(v/2,0.5,v/(v+t*t));return t>=0?1-0.5*ib:0.5*ib;};
const tPdf=(t,v)=>Math.exp(lgamma((v+1)/2)-lgamma(v/2)-0.5*Math.log(v*Math.PI)-(v+1)/2*Math.log(1+t*t/v));
const chiCdf=(x,k)=> x<=0?0:gammaP(k/2,x/2);
const chiPdf=(x,k)=> x<=0?0:Math.exp((k/2-1)*Math.log(x)-x/2-(k/2)*Math.log(2)-lgamma(k/2));
function invert(F,p,lo,hi){for(let i=0;i<200;i++){const mid=(lo+hi)/2;if(F(mid)<p)lo=mid;else hi=mid;}return (lo+hi)/2;}
const PhiInv=p=>invert(Phi,p,-12,12);
const tInv=(p,v)=>invert(t=>tCdf(t,v),p,-1e4,1e4);
const chiInv=(p,k)=>invert(x=>chiCdf(x,k),p,0,k+80*Math.sqrt(2*k)+200);
const lfact=n=>lgamma(n+1);
const poisPmf=(k,m)=>Math.exp(-m+k*Math.log(m)-lfact(k));
const binPmf=(k,n,p)=>Math.exp(lfact(n)-lfact(k)-lfact(n-k)+k*Math.log(p)+(n-k)*Math.log(1-p));

/* ---------- helpers ---------- */
const $=id=>document.getElementById(id);
const NS='http://www.w3.org/2000/svg';
function E(tag,a,p){const e=document.createElementNS(NS,tag);for(const k in a)e.setAttribute(k,a[k]);if(p)p.appendChild(e);return e;}
function T(p,x,y,s,cls,anc){const t=E('text',{x,y,class:cls||'ax-t','text-anchor':anc||'middle'},p);t.textContent=s;return t;}
const fx=(x,d=4)=>{if(!isFinite(x))return '—';const a=Math.abs(x);if(a!==0&&(a<1e-4||a>=1e7))return x.toExponential(3);return (+x.toFixed(d)).toString();};
const nums=s=>s.split(/[\s,;]+/).filter(Boolean).map(parseNum);
function parseNum(s){s=String(s).trim();if(s.includes('/')){const [a,b]=s.split('/');return Number(a)/Number(b);}return Number(s);}
function frac(x,maxD=2000){if(!isFinite(x))return '';for(let d=1;d<=maxD;d++){const n=Math.round(x*d);if(Math.abs(x*d-n)<1e-7*d){return d===1?String(n):`${n}/${d}`;}}return '';}
function cards(el,items){el.innerHTML=items.map(([k,v])=>`<div class="k"><span>${k}</span><b>${v}</b></div>`).join('');}
function verdict(el,cls,html){el.innerHTML=html?`<div class="verdict ${cls}">${html}</div>`:'';}
function niceTicks(lo,hi,n=5){const span=hi-lo||1;const step0=span/n;const p=Math.pow(10,Math.floor(Math.log10(step0)));const s=[1,2,2.5,5,10].map(v=>v*p).find(v=>v>=step0)||10*p;const out=[];for(let v=Math.ceil(lo/s)*s;v<=hi+1e-9;v+=s)out.push(+v.toFixed(10));return out;}

function axes(host,o){
  const W=o.W||640,H=o.H||240,m=Object.assign({l:44,r:18,t:18,b:34},o.m||{});
  if(!o.group){host.setAttribute('viewBox',`0 0 ${W} ${H}`);host.innerHTML='';}
  const y0=o.y0||0;
  const sx=x=>m.l+(x-o.x0)/(o.x1-o.x0)*(W-m.l-m.r), sy=y=>H-m.b-(y-y0)/(o.y1-y0)*(H-m.t-m.b);
  if(o.yt) for(const v of o.yt){E('line',{x1:m.l,x2:W-m.r,y1:sy(v),y2:sy(v),class:'gridl'},host);T(host,m.l-6,sy(v)+4,o.yf?o.yf(v):String(v),'ax-t','end');}
  const base=o.base!==undefined?o.base:y0;
  E('line',{x1:m.l,x2:W-m.r,y1:sy(base),y2:sy(base),class:'axis'},host);
  if(o.xt) for(const v of o.xt){E('line',{x1:sx(v),x2:sx(v),y1:sy(y0),y2:sy(y0)+4,class:'axis'},host);T(host,sx(v),H-m.b+18,o.xf?o.xf(v):String(v));}
  if(o.xl) T(host,W-m.r,H-4,o.xl,'ax-t','end');
  if(o.yl) T(host,m.l+4,m.t-4,o.yl,'ax-t','start');
  return {sx,sy,W,H,m,host};
}
function curve(ax,f,a,b,cls,N=300){let d='';for(let i=0;i<=N;i++){const x=a+(b-a)*i/N;d+=(i?'L':'M')+ax.sx(x).toFixed(1)+','+ax.sy(f(x)).toFixed(1);}return E('path',{d,class:cls||'curve'},ax.host);}
function area(ax,f,a,b,cls){let d=`M${ax.sx(a).toFixed(1)},${ax.sy(0).toFixed(1)}`;for(let i=0;i<=160;i++){const x=a+(b-a)*i/160;d+=`L${ax.sx(x).toFixed(1)},${ax.sy(f(x)).toFixed(1)}`;}d+=`L${ax.sx(b).toFixed(1)},${ax.sy(0).toFixed(1)}Z`;return E('path',{d,class:cls},ax.host);}
function vline(ax,x,ytop,cls,label,lcls,anc){E('line',{x1:ax.sx(x),x2:ax.sx(x),y1:ax.sy(0),y2:ax.sy(ytop),class:cls},ax.host);if(label)T(ax.host,ax.sx(x),ax.sy(ytop)-6,label,lcls||'lbl-t',anc);}

/* ---------- static figures ---------- */
function figHist(){
  const svg=$('fig-hist');if(!svg)return;
  const H=[28,133,228,260,200,105,36,8,2,0,0];
  const ax=axes(svg,{x0:-0.7,x1:10.7,y1:0.32,yt:[0,0.1,0.2,0.3],xt:[0,1,2,3,4,5,6,7,8,9,10],xl:'k',m:{l:44,r:18,t:30,b:34}});
  H.forEach((h,k)=>{const y=h/1000;E('rect',{x:ax.sx(k-0.35),y:ax.sy(y),width:ax.sx(0.35)-ax.sx(-0.35),height:ax.sy(0)-ax.sy(y),class:'bar'},svg);});
  for(let k=0;k<=10;k++){E('circle',{cx:ax.sx(k),cy:ax.sy(binPmf(k,10,0.2989)),r:4.5,class:'dot'},svg);}
  E('rect',{x:380,y:12,width:14,height:10,class:'bar'},svg);T(svg,400,21,'Hⱼ/n จาก data','lbl','start');
  E('circle',{cx:387,cy:36,r:4.5,class:'dot'},svg);T(svg,400,40,'pmf binomial(10, 0.2989)','lbl','start');
}
function figChi(){
  const svg=$('fig-chi');if(!svg)return;const k=7,z=14.067;
  const f=x=>chiPdf(x,k);const ymax=0.13;
  const ax=axes(svg,{x0:0,x1:24,y1:ymax,xt:[0,5,10,15,20],xl:'z',m:{l:18,r:18,t:30,b:34}});
  area(ax,f,z,24,'area-t');curve(ax,f,0.01,24,'curve');
  vline(ax,z,0.085,'mark','z_α = 14.067','lbl-t');
  vline(ax,7.01,0.125,'mark-a','Z = 7.01','lbl-a');
  T(svg,ax.sx(18.2),ax.sy(0.03),'α = 0.05','lbl-t');
  T(svg,ax.sx(4.5),ax.sy(0.03),'ยอมรับ','lbl');
}
function figCI(){
  const svg=$('fig-ci');if(!svg)return;const y=1.96;
  const ax=axes(svg,{x0:-3.8,x1:3.8,y1:0.47,xt:[-3,-2,-1,0,1,2,3],m:{l:18,r:18,t:30,b:34}});
  area(ax,phi,-y,y,'area-a');area(ax,phi,-3.8,-y,'area-t');area(ax,phi,y,3.8,'area-t');curve(ax,phi,-3.8,3.8,'curve');
  vline(ax,-y,0.2,'mark','−1.96','lbl-t');vline(ax,y,0.2,'mark','1.96','lbl-t');
  T(svg,ax.sx(0),ax.sy(0.17),'1 − α = 0.95','lbl-a');
  T(svg,ax.sx(-2.75),ax.sy(0.05),'α/2','lbl-t');T(svg,ax.sx(2.75),ax.sy(0.05),'α/2','lbl-t');
}
function figCIvar(){
  const svg=$('fig-civar');if(!svg)return;const k=99,l=77.046,u=123.225;const f=x=>chiPdf(x,k);
  const ax=axes(svg,{x0:55,x1:150,y1:0.036,xt:[60,80,100,120,140],m:{l:18,r:18,t:30,b:34}});
  area(ax,f,l,u,'area-a');area(ax,f,55,l,'area-t');area(ax,f,u,150,'area-t');curve(ax,f,55,150,'curve');
  vline(ax,l,0.024,'mark','ℓ = 77.046','lbl-t');vline(ax,u,0.024,'mark','u = 123.225','lbl-t');
  T(svg,ax.sx(99),ax.sy(0.012),'0.90','lbl-a');
  T(svg,ax.sx(66),ax.sy(0.004),'0.05','lbl-t');T(svg,ax.sx(134),ax.sy(0.004),'0.05','lbl-t');
}
function figTest(){
  const svg=$('fig-test');if(!svg)return;svg.setAttribute('viewBox','0 0 960 210');svg.innerHTML='';svg.style.minWidth='640px';
  const panels=[['แบบ 1 · H₁: m > m₀',[[1.645,3.8]],[[1.645,'y_α']]],['แบบ 2 · H₁: m ≤ m₀',[[-3.8,-1.645]],[[-1.645,'−y_α']]],['แบบ 3 · H₁: m ≠ m₀',[[-3.8,-1.96],[1.96,3.8]],[[-1.96,'−y_α/2'],[1.96,'y_α/2']]]];
  panels.forEach((p,i)=>{
    const g=E('g',{transform:`translate(${i*320},0)`},svg);
    const ax=axes(g,{group:true,W:320,H:210,x0:-3.8,x1:3.8,y1:0.47,xt:[-2,0,2],m:{l:12,r:12,t:34,b:30}});
    for(const [a,b] of p[1]) area(ax,phi,a,b,'area-t');
    curve(ax,phi,-3.8,3.8,'curve');
    for(const [x,lab] of p[2]) vline(ax,x,0.16,'mark',lab,'lbl-t');
    T(g,160,18,p[0],'lbl');
  });
}
function figCarrier(){
  const svg=$('fig-carrier');if(!svg)return;
  const ax=axes(svg,{x0:0,x1:2,y0:-1.25,y1:1.25,base:0,yt:[-1,0,1],xt:[0,0.5,1,1.5,2],xl:'t (f = 1)',m:{l:40,r:110,t:16,b:34}});
  [[1.5,'curve'],[-0.67,'curve2'],[-1.51,'curve3']].forEach(([th,c],i)=>{curve(ax,t=>Math.cos(2*Math.PI*t+th),0,2,c);T(svg,ax.sx(2)+10,ax.sy(Math.cos(4*Math.PI+th))+4+(i===2?10:0)-(i===0?4:0),'Θ = '+th,c==='curve'?'lbl':c==='curve2'?'lbl-a':'lbl-t','start');});
}
function figPois(){
  const svg=$('fig-pois');if(!svg)return;const J=[0.45,1.1,1.75,2.2,2.75,3.05,3.8,4.5];
  const ax=axes(svg,{x0:0,x1:5,y1:9,yt:[0,2,4,6,8],xt:[0,1,2,3,4,5],xl:'t',yl:'Nₜ',m:{l:40,r:18,t:22,b:34}});
  let d=`M${ax.sx(0)},${ax.sy(0)}`;let n=0;for(const t of J){d+=`L${ax.sx(t)},${ax.sy(n)}`;n++;d+=`L${ax.sx(t)},${ax.sy(n)}`;}d+=`L${ax.sx(5)},${ax.sy(n)}`;
  E('path',{d,class:'curve2'},svg);
  n=0;for(const t of J){E('circle',{cx:ax.sx(t),cy:ax.sy(n+1),r:3.5,class:'dot'},svg);n++;}
  ['T₁','T₂','T₃'].forEach((s,i)=>{E('line',{x1:ax.sx(J[i]),x2:ax.sx(J[i]),y1:ax.sy(0),y2:ax.sy(i+1),class:'mark'},svg);T(svg,ax.sx(J[i]),ax.sy(0)-6,s,'lbl-t');});
  T(svg,ax.sx(J[0]/2),ax.sy(0.35),'X₁','lbl-a');T(svg,ax.sx((J[0]+J[1])/2),ax.sy(1.35),'X₂','lbl-a');T(svg,ax.sx((J[1]+J[2])/2),ax.sy(2.35),'X₃','lbl-a');
}
function figNrho(){
  const svg=$('fig-nrho');if(!svg)return;
  const ax=axes(svg,{x0:0,x1:1,y1:20,yt:[0,5,10,15,20],xt:[0,0.2,0.4,0.6,0.8,1],xl:'ρ = λ/μ',yl:'N',m:{l:40,r:18,t:22,b:34}});
  curve(ax,r=>Math.min(r/(1-r),20),0,0.952,'curve2');
  [[0.8,4,'(a) ρ = 0.8, N = 4'],[8/9,8,'(b) ρ = 0.89, N = 8']].forEach(([r,N,s])=>{E('circle',{cx:ax.sx(r),cy:ax.sy(N),r:5,class:'dot'},svg);T(svg,ax.sx(r)-10,ax.sy(N)-4,s,'lbl-t','end');});
}

/* ---------- tools ---------- */
function toolCLT(){
  const n=Math.round(+$('cl-n').value),p=+$('cl-p').value,k=Math.round(+$('cl-k').value),s=Math.round(+$('cl-s').value);
  const out=[];
  if(n>0&&p>0&&p<1&&k>=0){
    const exact=k>n?0:binPmf(k,n,p);const sd=Math.sqrt(n*p*(1-p));const ap=phi((k-n*p)/sd)/sd;
    out.push(['ค่าจริง binomial',fx(exact,6)],['CLT (5.14)',fx(ap,6)],['error สัมพัทธ์',exact>0?fx(Math.abs(ap-exact)/exact*100,2)+'%':'ค่าจริง = 0'],['np (ตำแหน่งที่แม่นสุด)',fx(n*p,2)]);
  }
  if(s>=1&&s<=170){const ex=Math.exp(lfact(s));const st=Math.sqrt(2*Math.PI)*Math.exp((s+0.5)*Math.log(s)-s);out.push([s+'! จริง',s<=20?Math.round(ex).toLocaleString('en-US'):ex.toExponential(4)],['Stirling',s<=20?Math.round(st).toLocaleString('en-US'):st.toExponential(4)],['Stirling / จริง',fx(st/ex,5)]);}
  cards($('cl-out'),out);
}
function tab61(){
  const t=$('tab61');if(!t)return;let h='<thead><tr><th class="num">k</th><th class="num">z_α (α = 5%)</th><th class="num">z_α (α = 1%)</th><th class="num">k</th><th class="num">z_α (α = 5%)</th><th class="num">z_α (α = 1%)</th></tr></thead><tbody>';
  for(let k=1;k<=10;k++){const r=(kk)=>`<td class="num">${kk}</td><td class="num">${chiInv(.95,kk).toFixed(3)}</td><td class="num">${chiInv(.99,kk).toFixed(3)}</td>`;h+=`<tr${k===7?' class="hl"':''}>${r(k)}${r(k+10)}</tr>`;}
  t.innerHTML=h+'</tbody>';
}
function toolChi(){
  const H=nums($('chi-h').value);let P=nums($('chi-np').value);const r=Math.max(0,Math.round(+$('chi-r').value));const a=+$('chi-a').value;
  const tab=$('chi-tab'),out=$('chi-out'),v=$('chi-v');
  if(!H.length||H.length!==P.length||H.some(isNaN)||P.some(isNaN)){tab.innerHTML='';cards(out,[]);verdict(v,'no','จำนวนค่าใน Hⱼ กับ npⱼ ต้องเท่ากัน และเป็นตัวเลขทั้งหมด');return;}
  const n=H.reduce((s,x)=>s+x,0);const sp=P.reduce((s,x)=>s+x,0);const isP=Math.abs(sp-1)<0.02;if(isP)P=P.map(x=>x*n);
  let Z=0,rows='',low=0;H.forEach((h,j)=>{const e=P[j];const c=e>0?(h-e)**2/e:0;Z+=c;if(e<5)low++;rows+=`<tr${e<5?' class="hl"':''}><td class="num">${j}</td><td class="num">${h}</td><td class="num">${e.toFixed(3)}</td><td class="num">${(h-e).toFixed(3)}</td><td class="num">${c.toFixed(3)}</td></tr>`;});
  tab.innerHTML=`<thead><tr><th class="num">j</th><th class="num">Hⱼ</th><th class="num">npⱼ</th><th class="num">Hⱼ − npⱼ</th><th class="num">(Hⱼ − npⱼ)²/npⱼ</th></tr></thead><tbody>${rows}</tbody>`;
  const m=H.length,k=m-1-r;
  if(k<1||!(a>0&&a<1)){cards(out,[['Z',fx(Z,3)]]);verdict(v,'no','degrees of freedom ต้องมากกว่า 0 และ α ต้องอยู่ระหว่าง 0 กับ 1');return;}
  const za=chiInv(1-a,k);
  cards(out,[['n = ΣHⱼ',n],['m (bins)',m],['k = m − 1 − r',k],['Z',fx(Z,3)],['z_α',fx(za,3)]]);
  verdict(v,Z<=za?'ok':'no',(Z<=za?`Z = ${fx(Z,3)} ≤ z_α = ${fx(za,3)} ⇒ ยอมรับ hypothesis (fit ได้)`:`Z = ${fx(Z,3)} > z_α = ${fx(za,3)} ⇒ ปฏิเสธ hypothesis`)+(low?` · แถวไฮไลต์ ${low} bin มี npⱼ < 5 ควรรวม bin ก่อนสรุปจริง`:''));
}
let ciMode='m';
function toolCI(){
  const out=$('ci-out');
  if(ciMode==='m'){
    const cs=$('ci-case').value,Mn=+$('ci-mn').value,s=+$('ci-s').value,n=Math.round(+$('ci-n').value),c=+$('ci-c').value/100;
    if(!(n>=2&&s>=0&&c>0&&c<1)){cards(out,[['ข้อผิดพลาด','ตรวจ n ≥ 2 และ 1 − α']]);return;}
    const al=1-c;const y=cs==='t'?tInv(1-al/2,n-1):PhiInv(1-al/2);const d=s*y/Math.sqrt(n);
    cards(out,[['เปิดตาราง',cs==='t'?`t, ν = ${n-1}`:'normal Φ'],['y_α/2',fx(y,3)],['δ',fx(d,4)],['ขอบล่าง',fx(Mn-d,4)],['ขอบบน',fx(Mn+d,4)]]);
  }else{
    const cs=$('cv-case').value,s2=+$('cv-s2').value,n=Math.round(+$('cv-n').value),c=+$('cv-c').value/100;
    if(!(n>=2&&s2>=0&&c>0&&c<1)){cards(out,[['ข้อผิดพลาด','ตรวจ n ≥ 2 และ 1 − α']]);return;}
    const al=1-c,dof=cs==='known'?n:n-1;const l=chiInv(al/2,dof),u=chiInv(1-al/2,dof);const num=dof*s2;
    cards(out,[['χ² dof',dof],['ℓ',fx(l,3)],['u',fx(u,3)],['ขอบล่าง σ²',fx(num/u,4)],['ขอบบน σ²',fx(num/l,4)]]);
  }
}
function toolTest(){
  const ty=$('ht-type').value,Mn=+$('ht-mn').value,m0=+$('ht-m0').value,s=+$('ht-s').value,n=Math.round(+$('ht-n').value),a=+$('ht-a').value;
  const out=$('ht-out'),v=$('ht-v');
  if(!(s>0&&n>=2&&a>0&&a<1)){cards(out,[]);verdict(v,'no','ใส่ σ หรือ Sₙ มากกว่า 0, n ≥ 2 และ 0 < α < 1');return;}
  const Z=(Mn-m0)/(s/Math.sqrt(n));let y,acc,reg;
  if(ty==='1'){y=PhiInv(1-a);acc=Z<=y;reg=`(−∞, ${fx(y,3)}]`;}
  else if(ty==='2'){y=PhiInv(1-a);acc=Z>-y;reg=`(−${fx(y,3)}, ∞)`;}
  else{y=PhiInv(1-a/2);acc=Math.abs(Z)<=y;reg=`[−${fx(y,3)}, ${fx(y,3)}]`;}
  cards(out,[['Zₙ',fx(Z,3)],[ty==='3'?'y_α/2':'y_α',fx(y,3)],['acceptance region',reg]]);
  verdict(v,acc?'ok':'no',acc?'Zₙ อยู่ใน acceptance region ⇒ ยอมรับ H₀ (data ยังไม่ค้านพอ)':'Zₙ ตกใน critical region ⇒ ปฏิเสธ H₀ ⇒ เชื่อ H₁');
}
function toolReg(){
  const x=nums($('rg-x').value),Y=nums($('rg-y').value),out=$('rg-out'),svg=$('fig-reg');
  if(x.length<2||x.length!==Y.length||x.some(isNaN)||Y.some(isNaN)){cards(out,[['ข้อผิดพลาด','x กับ Y ต้องมีจำนวนเท่ากันอย่างน้อย 2 ค่า']]);svg.innerHTML='';return;}
  const n=x.length,xb=x.reduce((s,v)=>s+v,0)/n,Yb=Y.reduce((s,v)=>s+v,0)/n;let Sxx=0,SxY=0;x.forEach((xi,i)=>{Sxx+=(xi-xb)**2;SxY+=(xi-xb)*(Y[i]-Yb);});
  if(Sxx===0){cards(out,[['ข้อผิดพลาด','x ทุกค่าเท่ากัน หา slope ไม่ได้']]);return;}
  const a=SxY/Sxx,b=Yb-a*xb;let e=0;x.forEach((xi,i)=>{e+=(Y[i]-(a*xi+b))**2;});
  cards(out,[['x̄',fx(xb)],['Ȳ',fx(Yb)],['S_xx',fx(Sxx)],['S_xY',fx(SxY)],['a = S_xY/S_xx',fx(a)],['b = Ȳ − a x̄',fx(b)],['e(a, b)',fx(e)]]);
  const xmin=Math.min(...x),xmax=Math.max(...x),ymin=Math.min(...Y,a*xmin+b),ymax=Math.max(...Y,a*xmax+b);
  const padx=(xmax-xmin)*0.08||1,pady=(ymax-ymin)*0.1||1;const x0=xmin-padx,x1=xmax+padx,y0=ymin-pady,y1=ymax+pady;
  const ax=axes(svg,{x0,x1,y0,y1,base:y0,yt:niceTicks(y0,y1),xt:niceTicks(x0,x1,6),yf:v=>fx(v,2),xf:v=>fx(v,2),xl:'x',m:{l:50,r:18,t:22,b:34}});
  E('line',{x1:ax.sx(x0),y1:ax.sy(a*x0+b),x2:ax.sx(x1),y2:ax.sy(a*x1+b),class:'curve2'},svg);
  x.forEach((xi,i)=>{E('line',{x1:ax.sx(xi),x2:ax.sx(xi),y1:ax.sy(Y[i]),y2:ax.sy(a*xi+b),class:'mark'},svg);E('circle',{cx:ax.sx(xi),cy:ax.sy(Y[i]),r:4.5,class:'dot'},svg);});
  T(svg,ax.sx(x0)+8,ax.m.t+4,`ĝ(x) = ${fx(a,3)}x ${b<0?'−':'+'} ${fx(Math.abs(b),3)}`,'lbl-a','start');
}
function toolPois(){
  const l=+$('po-l').value,t=+$('po-t').value,k=Math.round(+$('po-k').value),out=$('po-out'),svg=$('fig-popmf');
  if(!(l>0&&t>0&&k>=0)){cards(out,[['ข้อผิดพลาด','λ, t ต้องมากกว่า 0 และ k ≥ 0']]);return;}
  const mu=l*t;let cdf=0;for(let i=0;i<=k;i++)cdf+=poisPmf(i,mu);const pk=poisPmf(k,mu);const ge=1-(cdf-pk);
  cards(out,[['mean = λ(t − s)',fx(mu,4)],[`P(N = ${k})`,fx(pk,5)],[`P(N ≤ ${k})`,fx(cdf,5)],[`P(N ≥ ${k})`,fx(ge,5)],[`[P(N ≥ ${k})]² · 2 ช่วงอิสระ`,fx(ge*ge,5)]]);
  const K=Math.max(k+2,Math.ceil(mu+4*Math.sqrt(mu))+1,6);let pm=0;const ps=[];for(let i=0;i<=K;i++){const v=poisPmf(i,mu);ps.push(v);pm=Math.max(pm,v);}
  const step=K>40?10:K>20?5:K>10?2:1;const xt=[];for(let i=0;i<=K;i+=step)xt.push(i);
  const ax=axes(svg,{x0:-0.7,x1:K+0.7,y1:pm*1.2,yt:niceTicks(0,pm*1.15,4),yf:v=>fx(v,3),xt,m:{l:50,r:18,t:22,b:34}});
  const w=Math.min(0.38,0.38);ps.forEach((v,i)=>{E('rect',{x:ax.sx(i-w),y:ax.sy(v),width:ax.sx(w)-ax.sx(-w),height:ax.sy(0)-ax.sy(v),class:i>=k?'area-t':'bar'},svg);});
  T(svg,ax.sx(K+0.7)-4,ax.m.t+2,`สีส้ม = P(N ≥ ${k})`,'lbl-t','end');
}
const mcDefault={2:[['7/10','3/10'],['2/5','3/5']],3:[['0','1/4','3/4'],['0','1/2','1/2'],['2/5','2/5','1/5']],4:[['1/2','1/2','0','0'],['1/4','1/2','1/4','0'],['0','1/4','1/2','1/4'],['0','0','1/2','1/2']]};
function mcBuild(){
  const n=+$('mc-size').value,g=$('mc-grid');g.style.gridTemplateColumns=`repeat(${n},auto)`;g.innerHTML='';
  for(let i=0;i<n;i++)for(let j=0;j<n;j++){const inp=document.createElement('input');inp.id=`mc-${i}-${j}`;inp.value=mcDefault[n][i][j];inp.setAttribute('aria-label',`p${i}${j}`);inp.addEventListener('input',toolMC);g.appendChild(inp);}
  toolMC();
}
function toolMC(){
  const n=+$('mc-size').value,v=$('mc-v'),out=$('mc-out'),pm=$('mc-pm');const P=[];
  for(let i=0;i<n;i++){P.push([]);for(let j=0;j<n;j++)P[i].push(parseNum($(`mc-${i}-${j}`).value));}
  if(P.flat().some(x=>isNaN(x)||x<0)){verdict(v,'no','ทุกช่องต้องเป็นตัวเลข ≥ 0');cards(out,[]);pm.textContent='';return;}
  const bad=P.map((r,i)=>[i,r.reduce((s,x)=>s+x,0)]).filter(([,s])=>Math.abs(s-1)>1e-6);
  if(bad.length){verdict(v,'no','แถว '+bad.map(([i,s])=>`${i} รวมได้ ${fx(s,4)}`).join(', ')+' ทุกแถวต้องรวมได้ 1');cards(out,[]);pm.textContent='';return;}
  const A=[],b=[];for(let j=0;j<n-1;j++){A.push(P.map((row,k)=>row[j]-(k===j?1:0)));b.push(0);}A.push(Array(n).fill(1));b.push(1);
  const M=A.map((r,i)=>[...r,b[i]]);let ok=true;
  for(let c=0;c<n;c++){let p=c;for(let r=c+1;r<n;r++)if(Math.abs(M[r][c])>Math.abs(M[p][c]))p=r;if(Math.abs(M[p][c])<1e-12){ok=false;break;}[M[c],M[p]]=[M[p],M[c]];for(let r=0;r<n;r++){if(r===c)continue;const f=M[r][c]/M[c][c];for(let q=c;q<=n;q++)M[r][q]-=f*M[c][q];}}
  if(ok){const pi=M.map((r,i)=>r[n]/r[i]);verdict(v,'ok','แก้ได้ stationary distribution หนึ่งชุด (π = πP, Σπⱼ = 1)');cards(out,pi.map((x,i)=>[`π${'₀₁₂₃'[i]}`,fx(x,5)+(frac(x)?`  (${frac(x)})`:'')]));}
  else{verdict(v,'no','สมการมีคำตอบไม่เดียว (chain อาจแยกเป็นหลายกลุ่มที่ไปหากันไม่ได้)');cards(out,[]);}
  const m=Math.max(0,Math.min(500,Math.round(+$('mc-m').value)));let R=P.map((r,i)=>r.map((_,j)=>i===j?1:0));
  for(let s=0;s<m;s++){R=R.map(r=>P[0].map((_,j)=>r.reduce((acc,x,k)=>acc+x*P[k][j],0)));}
  pm.textContent=`P^${m} =\n`+R.map(r=>'  '+r.map(x=>x.toFixed(4).padStart(8)).join(' ')).join('\n')+(m>=10?'\n\nเมื่อ m ใหญ่ ทุกแถวเข้าใกล้ π (ถ้า chain ลู่เข้า)':'');
}
function toolMM1(){
  const a=+$('mm-a').value,s=+$('mm-s').value,out=$('mm-out'),v=$('mm-v'),svg=$('fig-pn');
  if(!(a>0&&s>0)){cards(out,[]);verdict(v,'no','ใส่เวลาเฉลี่ยที่มากกว่า 0');return;}
  const l=1/a,m=1/s,r=l/m;
  if(r>=1){cards(out,[['λ',fx(l,4)],['μ',fx(m,4)],['ρ = λ/μ',fx(r,4)]]);verdict(v,'no','ρ ≥ 1: ลูกค้ามาเร็วกว่าที่บริการได้ คิวยาวขึ้นไม่สิ้นสุด ใช้สูตร M/M/1 ไม่ได้');svg.innerHTML='';return;}
  const N=r/(1-r),T_=1/(m-l),W=T_-1/m,NQ=l*W;
  cards(out,[['λ',fx(l,4)],['μ',fx(m,4)],['ρ = λ/μ',fx(r,4)],['N = ρ/(1−ρ)',fx(N,4)],['T = 1/(μ−λ)',fx(T_,4)],['W = T − 1/μ',fx(W,4)],['N_Q = λW',fx(NQ,4)],['p₀ = 1 − ρ',fx(1-r,4)]]);
  verdict(v,'info',`ลองตรวจ Little: λT = ${fx(l,4)} × ${fx(T_,4)} = ${fx(l*T_,4)} = N ✓`);
  const K=15,ps=[];for(let n=0;n<=K;n++)ps.push(Math.pow(r,n)*(1-r));const pmx=Math.max(...ps);
  const ax=axes(svg,{x0:-0.7,x1:K+0.7,y1:pmx*1.2,yt:niceTicks(0,pmx*1.15,4),yf:v=>fx(v,2),xt:[0,3,6,9,12,15],xl:'n',m:{l:46,r:18,t:20,b:34}});
  ps.forEach((p,n)=>{E('rect',{x:ax.sx(n-0.38),y:ax.sy(p),width:ax.sx(0.38)-ax.sx(-0.38),height:ax.sy(0)-ax.sy(p),class:'bar'},svg);});
}
function toolCrit(){
  const d=$('cr-d').value,k=Math.max(1,Math.round(+$('cr-k').value)),p=+$('cr-p').value,x=+$('cr-x').value;$('cr-k').disabled=d==='n';
  const F=d==='n'?Phi:d==='t'?(z=>tCdf(z,k)):(z=>chiCdf(z,k));const Fi=d==='n'?PhiInv:d==='t'?(q=>tInv(q,k)):(q=>chiInv(q,k));
  const name=d==='n'?'Φ':d==='t'?`t (ν = ${k})`:`χ² (k = ${k})`;const items=[['การแจกแจง',name]];
  if(p>0&&p<1)items.push([`x ที่ F(x) = ${p}`,fx(Fi(p),4)]);
  if(isFinite(x)){const Fx=F(x);items.push([`F(${x})`,fx(Fx,5)],[`1 − F(${x})`,fx(1-Fx,5)]);}
  cards($('cr-out'),items);
}
function checklist(){
  const KEY='stoch-ck-v1';let st={};try{st=JSON.parse(localStorage.getItem(KEY)||'{}');}catch(e){st={};}
  const boxes=[...document.querySelectorAll('#ck input')];
  const upd=()=>{const d=boxes.filter(b=>b.checked).length;$('ck-prog').textContent=`ทำได้แล้ว ${d}/${boxes.length} หัวข้อ`;};
  boxes.forEach(b=>{b.checked=!!st[b.id];b.addEventListener('change',()=>{st[b.id]=b.checked;try{localStorage.setItem(KEY,JSON.stringify(st));}catch(e){}upd();});});upd();
}
function nav(){
  const ul=$('toc'),slot=$('mtoc-slot');if(ul&&slot){const c=ul.cloneNode(true);c.removeAttribute('id');slot.appendChild(c);c.addEventListener('click',e=>{if(e.target.closest('a'))slot.closest('details').open=false;});}
  const links=[...document.querySelectorAll('.toc a')];const map=new Map();links.forEach(a=>{const id=a.getAttribute('href').slice(1);if(!map.has(id))map.set(id,[]);map.get(id).push(a);});
  if(!('IntersectionObserver' in window))return;
  const io=new IntersectionObserver(es=>{es.forEach(en=>{if(en.isIntersecting){links.forEach(a=>a.classList.remove('on'));(map.get(en.target.id)||[]).forEach(a=>a.classList.add('on'));}});},{rootMargin:'-20% 0px -70% 0px'});
  document.querySelectorAll('section.ch').forEach(s=>io.observe(s));
}

/* ---------- boot ---------- */
function bind(ids,fn){ids.forEach(id=>{const el=$(id);if(el){el.addEventListener('input',fn);el.addEventListener('change',fn);}});fn();}
function boot(){
  nav();
  [figHist,figChi,figCI,figCIvar,figTest,figCarrier,figPois,figNrho,tab61].forEach(f=>{try{f();}catch(e){console.error(e);}});
  bind(['cl-n','cl-p','cl-k','cl-s'],toolCLT);
  bind(['chi-h','chi-np','chi-r','chi-a'],toolChi);
  bind(['ci-case','ci-mn','ci-s','ci-n','ci-c','cv-case','cv-s2','cv-n','cv-c'],toolCI);
  const mb=$('ci-m-btn'),vb=$('ci-v-btn');
  const setMode=m=>{ciMode=m;mb.setAttribute('aria-pressed',m==='m');vb.setAttribute('aria-pressed',m==='v');$('ci-m').hidden=m!=='m';$('ci-v').hidden=m!=='v';toolCI();};
  mb.addEventListener('click',()=>setMode('m'));vb.addEventListener('click',()=>setMode('v'));
  bind(['ht-type','ht-mn','ht-m0','ht-s','ht-n','ht-a'],toolTest);
  bind(['rg-x','rg-y'],toolReg);
  bind(['po-l','po-t','po-k'],toolPois);
  $('mc-size').addEventListener('change',mcBuild);$('mc-m').addEventListener('input',toolMC);mcBuild();
  bind(['mm-a','mm-s'],toolMM1);
  bind(['cr-d','cr-k','cr-p','cr-x'],toolCrit);
  checklist();
}
if(document.readyState==='loading')document.addEventListener('DOMContentLoaded',boot);else boot();

/* ---------- past exam (Final 2564) ---------- */
const PIHAT=('14159 26535 89793 23846 26433 83279 51288 41971 69399 37512 58239 74944 59234 78164 56286 26899 86287 34825 34211 78679 82148').replace(/\s/g,'');
function examFromId(){
  const raw=$('ej-id').value,dg=raw.replace(/\D/g,''),v=$('ej-v');
  if(!raw.trim()){verdict(v,'info','ยังไม่ได้ใส่รหัส ใช้ j ที่เลือกไว้');return;}
  if(dg.length<2){verdict(v,'no','รหัสต้องมีตัวเลขอย่างน้อย 2 หลัก');return;}
  const x0=Number(dg.slice(-2));const d=(x0>=1&&x0<=105)?Number(PIHAT[x0-1]):5;const j=Math.pow(2,d)%5;
  $('ej-j').value=String(j);
  verdict(v,'ok',`x₀ = ${x0} ⇒ ${(x0>=1&&x0<=105)?'เลขโดดตำแหน่งที่ '+x0+' ของ π̂':'นอกช่วง 1–105 จึงใช้ค่า'} = ${d} ⇒ j = 2^${d} mod 5 = ${Math.pow(2,d)} mod 5 = ${j}`);
  renderExam();
}
function renderExam(){
  const j=Number($('ej-j').value);
  const tab={1:[0.02,2.326],2:[0.03,2.170],3:[0.04,2.054],4:[0.05,1.960]};const [a,y]=tab[j];
  const Mn=8.32+j/1000,d=y*Math.sqrt(2.1)/Math.sqrt(101);
  cards($('ex1-out'),[['α = αⱼ',a],['1 − α',(1-a).toFixed(2)],['y_α/2 (Table 6.2)',y.toFixed(3)],['Mₙ',Mn.toFixed(3)],['δ = √2.1·y/√101',d.toFixed(5)],['ช่วง (1 − α)',`[${(Mn-d).toFixed(5)}, ${(Mn+d).toFixed(5)}]`]]);
  const Z=1.4+j/100,ya=1.476;
  cards($('ex2-out'),[['Zₙ = 1.4 + j/100',Z.toFixed(2)],['y_α (α = 0.07)',ya],['(a) reject ถ้า',`Zₙ > ${ya}`],['(b) reject ถ้า',`Zₙ ≤ −${ya}`]]);
  verdict($('ex2-v'),'ok',`(a) Zₙ = ${Z.toFixed(2)} ≤ ${ya} ⇒ ยอมรับ H₀ ⇒ ยอมรับข้ออ้าง E[Xᵢ] ≤ p₀ · (b) Zₙ = ${Z.toFixed(2)} > −${ya} ⇒ ยอมรับ H₀: E[Xᵢ] > p₀ ⇒ นักศึกษาไม่ควรยอมรับข้ออ้างของผู้ให้บริการ`);
  $('ex3-ans').textContent=`j = ${j}: f(x, y) = y·e^(−y(x + ${j})), x, y > 0`;
  const w=Math.sqrt(5)/j;
  cards($('ex4-out'),[['ω₀ = √5π/j',`${w.toFixed(4)}π ≈ ${(w*Math.PI).toFixed(4)} rad/s`],['m_X(t)','0'],['R_X(t₁, t₂)',`½ cos(${j===1?'√5':'(√5/'+j+')'}π(t₁ − t₂))`]]);
  const T_=5+(20+j)/2;
  cards($('ex5-out'),[['λ','5 คน/นาที'],['T = 5 + (20 + j)/2',`${T_} นาที`],['N = λT',`${5*T_} คน`]]);
}
function bootExam(){
  if(!$('ej-j'))return;
  $('ej-j').addEventListener('change',()=>{$('ej-id').value='';verdict($('ej-v'),'info',`ใช้ j = ${$('ej-j').value}`);renderExam();});
  $('ej-id').addEventListener('input',examFromId);
  verdict($('ej-v'),'info','ใช้ j = 2 (ตรงกับชีทเฉลยลายมือ)');renderExam();
}
document.addEventListener('DOMContentLoaded',bootExam);if(document.readyState!=='loading')bootExam();

</script>
</body>
</html>
