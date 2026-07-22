# Code Analysis HTML — Design System

Design rules for rendering **code / architecture analysis results** as a single-file visual HTML dashboard.

> **Companion skin.** This shares the warm-editorial palette with the `make-html` report skill (family resemblance by design), but the layout discipline is different: dashboard measure (`1080px`), masthead opening, diagram-first sections. This document is **self-contained** — it does not require reading the make-html ruleset.

---

## When to Apply — **READ FIRST**

> Apply **only when the user explicitly requests a visual/HTML rendering of an analysis.** Never auto-apply.

### ✅ Apply (user explicitly requests)

- "이 분석 시각화해줘" / "visualize this analysis"
- "이미지화해서 보여줘" / "다이어그램으로 정리해줘"
- "이 분석 HTML(대시보드)로 만들어줘"
- continuing a visual analysis page started earlier in the conversation

### ❌ Do not apply

- **Prose report requests** — "HTML 보고서로", "리포트 형식으로" → that is `make-html`, not this skill.
- **Simple Q&A / summaries / read-only** — respond in Markdown.
- **Table-only needs** — a Markdown table is enough.
- **Another format specified** — PPT, DOCX, Mermaid source, etc.

### Decision rules

1. Did the user explicitly ask for a *visual / diagram / dashboard* rendering? → If NO, do not apply.
2. Is the content long-form reading (policy, PRD, guide)? → Use `make-html` instead.
3. If ambiguous, **ask**: "산문 리포트로 만들까요, 다이어그램 중심의 시각화 페이지로 만들까요?"

---

## 0. Principles

1. **Open with the subject, not a hero.** The masthead leads with the most characteristic structure of the analysis — a data pipeline for a storage system, a module stack for a library, a sparkline for a time-series topic. Never a generic centered hero.
2. **Diagram-first sections.** Every section = `number + serif H2 + 1–2 sentence lede + one visual`. The visual carries the content; prose stays in the lede.
3. **One accent = clay.** Terracotta `#D97757` is the only chromatic accent. It marks the *hot path* — active pipeline nodes, primary bars, key numbers, live dots. Everything else is the warm ink family. Never introduce a second accent hue.
4. **Alive but editorial.** Animated dash-flow on arrows, count-up numbers, scroll reveals, hover lifts — subtle vanilla motion. No parallax, no glassmorphism, no gradient headlines.
5. **Numbers must be crisp** — `font-feature-settings: "tnum"` on every numeric element.
6. **Self-contained** — one `.html` file, inline `<style>` + vanilla `<script>`, no chart/diagram libraries. Only external requests are the two font CDNs.

---

## 1. Color Tokens

> **Warm-only palette.** No pure white page background, no cool gray. Cards are `--paper` white sitting *on* the ivory canvas — that lift is the core contrast. The only chromatic accent is clay.

```css
:root{
  /* brand */
  --brand-slate:#141413;      /* warm near-black — dark surfaces, strong lines */
  --brand-ivory:#faf9f5;      /* the canvas — page background (never #ffffff) */
  --brand-clay:#D97757;       /* THE accent — terracotta */
  --brand-clay-deep:#C96442;  /* hover / label text on clay */
  --brand-manila:#EBDBBC;     /* warm sand — aside tint */
  --brand-kraft:#D4A27F;      /* warm secondary tone (secondary bar start, dashed asides) */
  --brand-red:#BF4D43;        /* danger / "no" marks (warmed) */
  --brand-green:#617A5B;      /* positive / "yes" marks (muted sage) */
  --brand-gray:#B0AEA5;       /* warm mid gray */

  /* ink (text hierarchy) */
  --ink:#141413;    /* headings / emphasis */
  --ink-2:#3D3929;  /* default body — warm brown-black */
  --ink-3:#605A4E;  /* secondary body / descriptions */
  --ink-4:#83786A;  /* captions / labels */
  --ink-5:#A8A093;  /* separators / disabled */
  --ink-6:#C9C3B6;

  /* surface / line */
  --surface:#faf9f5;     /* page background */
  --paper:#ffffff;       /* card fill — sits ON the ivory for lift */
  --surface-1:#F2F0E9;   /* hover fill */
  --surface-2:#EBE8DF;   /* tracks / thead / chip fill */
  --line:#141413;        /* strong divider (section top rule) */
  --line-soft:#E0DCD1;   /* card / table borders */
  --line-softer:#ECE9E0; /* row dividers */

  /* semantic */
  --accent:#D97757;      /* = brand-clay */
  --accent-deep:#C96442;
  --danger:#BF4D43;
  --success:#617A5B;

  /* radius */
  --radius:12px;
  --radius-sm:8px;
}
```

### Ambient background

A fixed, barely-there warm radial wash — layered, never neon, never animated blobs:

```css
body::before{
  content:"";position:fixed;inset:0;z-index:-1;pointer-events:none;
  background:
    radial-gradient(900px 500px at 85% -8%,  rgba(217,119,87,.07),  transparent 60%),
    radial-gradient(700px 460px at -10% 30%, rgba(212,162,127,.08), transparent 60%),
    radial-gradient(800px 600px at 60% 110%, rgba(235,219,188,.10), transparent 60%);
}
```

---

## 2. Typography

> **Serif display + Pretendard body** — the same pairing as make-html. Display = section H2, stat numbers, finding numbers. Everything else is Pretendard.

```css
:root{
  --font-sans:"Pretendard",sans-serif;
  --font-display:"Noto Serif KR",Georgia,serif;
}
```

CDN load (the only external dependencies allowed):

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Noto+Serif+KR:wght@500;600;700&display=swap">
```

| Role | Family | Size | Weight |
|---|---|---|---|
| Masthead title | serif display | `clamp(30px, 4.6vw, 46px)` | 700 |
| Section H2 | serif display | `clamp(21px, 2.6vw, 27px)` | 700 |
| Stat number | serif display | `clamp(26px, 3vw, 34px)` | 700 |
| Body / lede | Pretendard | 14.5–16px | 400 |
| Card title | Pretendard | 13.5–14.5px | 700 |
| Label / caption | Pretendard | 11–12.5px | 600 |
| Kicker | Pretendard | 12px, `letter-spacing:.14em`, uppercase | 700 |

- All numeric text gets `font-feature-settings:"tnum"` (`.tnum` utility class).
- Korean body: `word-break: keep-all`.
- Display headings get light negative tracking (`-0.01em`); labels/kickers get positive tracking.

---

## 3. Layout

```css
.wrap{max-width:1080px;margin:0 auto;padding:0 28px}   /* dashboard measure */
section{padding:56px 0 8px}
```

- Page = `masthead` (border-bottom `--line`) → `main.wrap` with numbered `section`s → `footer`.
- Each section head: `border-top:1px solid var(--line)` + a clay two-digit number + serif H2 on one baseline row.
- Generous whitespace: `56px` top padding per section; visuals breathe inside `--paper` cards.

---

## 4. Masthead

The opening. **Not a centered hero.** Structure: kicker (with live dot) → serif title → lede → meta row → optional subject visual (sparkline) → stat strip.

```html
<header class="masthead">
  <div class="wrap">
    <div class="kicker"><span class="live-dot"></span> Backend Architecture Analysis</div>
    <h1>InfluxDB, 백엔드에서 어떻게 쓰이고 있나</h1>
    <p class="lede">…1–2 sentences, the one-line verdict of the analysis…</p>
    <div class="meta tnum">
      <span>대상 <b>server/ 전 모듈</b></span>
      <span>버킷 <b>openstackit</b></span>
    </div>
    <!-- optional: subject visual, e.g. sparkline for time-series topics -->
    <div class="stats reveal"> …stat tiles (§5)… </div>
  </div>
</header>
```

### 4.1 Kicker + live dot

```css
.kicker{display:flex;align-items:center;gap:10px;font-size:12px;font-weight:700;
  letter-spacing:.14em;text-transform:uppercase;color:var(--accent-deep)}
.kicker .live-dot{width:8px;height:8px;border-radius:50%;background:var(--accent);
  box-shadow:0 0 0 0 rgba(217,119,87,.5);animation:pulse 2s infinite}
@keyframes pulse{0%{box-shadow:0 0 0 0 rgba(217,119,87,.45)}
  70%{box-shadow:0 0 0 9px rgba(217,119,87,0)}100%{box-shadow:0 0 0 0 rgba(217,119,87,0)}}
```

### 4.2 Sparkline (time-series signature, inline SVG)

```html
<svg viewBox="0 0 1000 64" preserveAspectRatio="none">
  <path class="spark-fill" d="M0,44 … L1000,64 0,64 Z"/>
  <path class="spark-line" d="M0,44 …"/>
  <circle class="spark-dot" cx="1000" cy="12" r="3.5"/>
</svg>
```

```css
.spark-line{fill:none;stroke:var(--accent);stroke-width:2;stroke-linecap:round;
  stroke-dasharray:1200;stroke-dashoffset:1200;animation:draw 2.4s .3s ease forwards}
.spark-fill{fill:rgba(217,119,87,.08);opacity:0;animation:fadein 1s 1.6s forwards}
@keyframes draw{to{stroke-dashoffset:0}}
@keyframes fadein{to{opacity:1}}
```

> Build the path points from the *real* shape of the subject when possible (e.g. an actual metric trend), not decorative noise.

---

## 5. Stat Strip (`.stats`)

Key numbers of the analysis, count-up on reveal. A hairline grid of `--paper` cells (gap trick: `gap:1px; background:--line-soft`).

```html
<div class="stats reveal">
  <div class="stat">
    <div class="num tnum" data-count="13">0</div>
    <div class="lab">measurements</div>
    <div class="sub">노드 7 · 인스턴스 6 · GPU · LB</div>
  </div>
  …
</div>
```

```css
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:1px;background:var(--line-soft);border:1px solid var(--line-soft);
  border-radius:var(--radius);overflow:hidden;margin:34px 0 8px}
.stat{background:var(--paper);padding:20px 18px 16px;transition:background .25s}
.stat:hover{background:var(--surface-1)}
.stat .num{font-family:var(--font-display);font-weight:700;color:var(--ink);
  font-size:clamp(26px,3vw,34px);line-height:1.1}
.stat .num small{font-size:.55em;font-weight:600;color:var(--accent-deep);margin-left:2px}
.stat .lab{font-size:12px;font-weight:600;color:var(--ink-3);margin-top:6px}
.stat .sub{font-size:11px;color:var(--ink-5);margin-top:2px}
```

Rules: 3–6 tiles. The number is the analysis fact (count-up animated via JS, §8). `small` holds a unit (`d`, `%`, `×`).

---

## 6. Section Head

```html
<section id="flow">
  <div class="sec-head reveal"><span class="no tnum">01</span><h2>데이터 흐름 — 수집은 에이전트, 백엔드는 조회</h2></div>
  <p class="sec-lede reveal">…1–2 sentences…</p>
  …one visual…
</section>
```

```css
.sec-head{display:flex;align-items:baseline;gap:14px;border-top:1px solid var(--line);
  padding-top:18px;margin-bottom:8px}
.sec-head .no{font-family:var(--font-display);font-weight:700;font-size:15px;
  color:var(--accent-deep);letter-spacing:.04em}
.sec-head h2{font-family:var(--font-display);font-weight:700;
  font-size:clamp(21px,2.6vw,27px);color:var(--ink);letter-spacing:-.01em}
.sec-lede{color:var(--ink-3);font-size:14.5px;max-width:760px;margin:6px 0 26px}
```

Rules: H2 titles are **declarative, with a dash clause** stating the takeaway ("데이터 흐름 — 수집은 에이전트, 백엔드는 조회"). No interrogative headings.

---

## 7. Visual Components

### 7.1 Flow diagram (`.flow`) — pipeline with animated arrows

A horizontal chain of `--paper` nodes joined by clay dashed arrows whose dashes *flow* (direction of data). The hot node (the subject of the analysis) gets `.hot` (clay-tinted icon chip).

```html
<div class="flow reveal">
  <div class="flow-node">
    <div class="ic">📡</div>
    <div class="role">COLLECT</div>
    <h4>telegraf / hsflowd</h4>
    <p>…what it does… <code>app.monitoring.agent</code></p>
  </div>
  <div class="flow-arrow"></div>
  <div class="flow-node hot">
    <div class="ic">🗄️</div><div class="role">STORE</div>
    <h4>InfluxDB v1.8</h4><p>…</p>
  </div>
  <div class="flow-arrow"></div>
  …
</div>
```

```css
.flow{display:flex;align-items:stretch;border:1px solid var(--line-soft);
  border-radius:var(--radius);background:var(--paper);overflow:hidden}
.flow-node{flex:1;min-width:0;padding:20px 16px;border-right:1px solid var(--line-softer);
  transition:background .25s}
.flow-node:last-child{border-right:none}
.flow-node:hover{background:var(--surface-1)}
.flow-node .ic{width:34px;height:34px;border-radius:var(--radius-sm);display:flex;
  align-items:center;justify-content:center;font-size:16px;margin-bottom:12px;
  background:var(--surface-2);border:1px solid var(--line-soft)}
.flow-node.hot .ic{background:rgba(217,119,87,.12);border-color:rgba(217,119,87,.35)}
.flow-node h4{font-size:14px;font-weight:700;color:var(--ink)}
.flow-node .role{font-size:11.5px;color:var(--ink-4);font-weight:600;letter-spacing:.05em;margin-top:2px}
.flow-node p{font-size:12.5px;color:var(--ink-3);margin-top:8px;line-height:1.6}
.flow-node code{font-size:11px;background:var(--surface-2);border-radius:4px;padding:1px 5px;color:var(--ink-2)}

.flow-arrow{flex:0 0 46px;align-self:center;position:relative;height:2px}
.flow-arrow::before{content:"";position:absolute;inset:0;
  background:repeating-linear-gradient(90deg,var(--accent) 0 6px,transparent 6px 12px);
  animation:flowdash 1.1s linear infinite}
.flow-arrow::after{content:"";position:absolute;right:-1px;top:50%;transform:translateY(-50%);
  border:5px solid transparent;border-left:7px solid var(--accent)}
@keyframes flowdash{to{background-position:12px 0}}
```

Rules: 3–5 nodes. Exactly **one** `.hot` node (the analysis subject). Arrows always point in the real data direction.

### 7.2 Fan-out (`.fanout`) — consumers of the last stage

```html
<div class="fanout reveal">
  <div class="fan"><span class="tag">SERVLET</span><b>MonitoringServiceImpl</b><span>실시간 메트릭 → UI</span></div>
  …
</div>
```

```css
.fanout{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:10px;margin-top:14px}
.fan{border:1px solid var(--line-soft);border-radius:var(--radius-sm);background:var(--paper);
  padding:13px 14px;transition:transform .22s,border-color .22s,box-shadow .22s}
.fan:hover{transform:translateY(-3px);border-color:var(--brand-kraft);box-shadow:0 8px 20px rgba(60,50,30,.07)}
.fan b{font-size:13px;color:var(--ink)}
.fan span{display:block;font-size:11.5px;color:var(--ink-4);margin-top:3px}
.fan .tag{display:inline-block;font-size:10px;font-weight:700;letter-spacing:.08em;
  color:var(--accent-deep);background:rgba(217,119,87,.1);border-radius:999px;padding:2px 8px;margin-bottom:7px}
```

### 7.3 Aside path (`.writepath`) — the exception worth calling out

A dashed kraft aside under a flow, for the minority/exceptional path (e.g. "the only write path").

```html
<div class="writepath reveal">
  <span class="badge">WRITE</span>
  <p><b>유일한 쓰기 경로 — LB 메트릭.</b> …explanation…</p>
</div>
```

```css
.writepath{margin-top:18px;border:1px dashed var(--brand-kraft);border-radius:var(--radius);
  background:rgba(235,219,188,.25);padding:16px 18px;display:flex;gap:14px;align-items:flex-start}
.writepath .badge{flex:0 0 auto;font-size:10.5px;font-weight:800;letter-spacing:.1em;color:#fff;
  background:var(--brand-kraft);border-radius:999px;padding:4px 11px;margin-top:2px}
.writepath p{font-size:13px;color:var(--ink-3)}
.writepath b{color:var(--ink)}
```

### 7.4 Layer stack (`.stack`) — dependency / module hierarchy

Top = highest level. Each layer has a depth badge, name + role, and a dependency pill. `.hot` marks the layers where the analyzed code lives.

```html
<div class="stack reveal">
  <div class="layer">
    <div class="depth tnum">4</div>
    <div class="body"><b><code>openstackit-servlet</code></b><span>…role…</span></div>
    <div class="dep">depends ↓</div>
  </div>
  <div class="layer hot"> … </div>
  …
</div>
```

```css
.stack{display:flex;flex-direction:column;max-width:760px}
.layer{display:flex;align-items:center;gap:16px;border:1px solid var(--line-soft);
  border-bottom:none;background:var(--paper);padding:16px 20px;
  transition:background .25s,padding-left .25s}
.layer:first-child{border-radius:var(--radius) var(--radius) 0 0}
.layer:last-child{border-bottom:1px solid var(--line-soft);border-radius:0 0 var(--radius) var(--radius)}
.layer:hover{background:var(--surface-1);padding-left:26px}
.layer .depth{flex:0 0 34px;height:34px;border-radius:50%;display:flex;align-items:center;
  justify-content:center;font-family:var(--font-display);font-weight:700;font-size:14px;
  background:var(--surface-2);color:var(--ink-3);border:1px solid var(--line-soft)}
.layer.hot .depth{background:var(--accent);color:#fff;border-color:var(--accent)}
.layer b{font-size:14.5px;color:var(--ink)}
.layer b code{font-size:12.5px;background:var(--surface-2);border-radius:4px;padding:1px 6px}
.layer span{display:block;font-size:12.5px;color:var(--ink-4);margin-top:2px}
.layer .dep{margin-left:auto;font-size:10.5px;font-weight:700;letter-spacing:.08em;color:var(--ink-4);
  border:1px solid var(--line-soft);border-radius:999px;padding:3px 10px;white-space:nowrap}
```

### 7.5 Bar chart (`.bars`) — ranked frequencies / amounts

Pure CSS bars, fill animated on reveal. Primary series = clay→kraft gradient; secondary = ink gradient (`.ink`).

```html
<div class="bars reveal">
  <div class="bar-row">
    <div class="bl">MonitoringServiceImpl<small>common · 실시간 조회</small></div>
    <div class="bar-track"><div class="bar-fill" data-w="100"></div></div>
    <div class="bv tnum">21</div>
  </div>
  …
</div>
<p class="bars-cap reveal tnum">▲ 파일당 Flux/API 참조 횟수</p>
```

```css
.bars{display:flex;flex-direction:column;gap:13px;max-width:760px}
.bar-row{display:grid;grid-template-columns:200px 1fr 46px;align-items:center;gap:14px}
.bar-row .bl{font-size:13px;color:var(--ink-2);font-weight:600;text-align:right}
.bar-row .bl small{display:block;font-weight:400;font-size:11px;color:var(--ink-5)}
.bar-track{height:22px;background:var(--surface-2);border-radius:6px;overflow:hidden}
.bar-fill{height:100%;width:0;border-radius:6px;
  background:linear-gradient(90deg,var(--brand-kraft),var(--accent));
  transition:width 1.1s cubic-bezier(.22,.8,.3,1)}
.bar-fill.ink{background:linear-gradient(90deg,var(--ink-5),var(--ink-3))}
.bar-row .bv{font-size:13px;font-weight:700;color:var(--ink);text-align:right}
.bars-cap{font-size:11.5px;color:var(--ink-5);margin-top:12px}
```

Rules: `data-w` is percent of the max row. Sort descending. Caption line starts with `▲` and states the unit.

### 7.6 Matrix table (`.table-wrap`) — configuration / option comparison

Reuses the warm table discipline: `--surface-2` thead, 1px `--line-softer` rows, hover tint, `.yes`/`.no` semantic marks.

```html
<div class="table-wrap reveal">
  <table>
    <thead><tr><th>항목</th><th>servlet</th><th>batch</th></tr></thead>
    <tbody>
      <tr><td><b>쓰기</b></td><td><span class="no">✕</span> 읽기 전용</td><td><span class="yes">○</span> <code>WriteApi</code></td></tr>
      …
    </tbody>
  </table>
</div>
```

```css
.table-wrap{border:1px solid var(--line-soft);border-radius:var(--radius);overflow:hidden;background:var(--paper)}
table{width:100%;border-collapse:collapse;font-size:13.5px}
thead th{background:var(--surface-2);color:var(--ink);font-weight:700;text-align:left;
  padding:12px 16px;border-bottom:1px solid var(--line-soft);font-size:12.5px;letter-spacing:.03em}
tbody td{padding:12px 16px;border-bottom:1px solid var(--line-softer);color:var(--ink-2);vertical-align:top}
tbody tr:last-child td{border-bottom:none}
tbody tr:hover{background:var(--surface-1)}
td .yes{color:var(--success);font-weight:700}
td .no{color:var(--danger);font-weight:700}
td code{font-size:11.5px;background:var(--surface-2);border-radius:4px;padding:1px 5px}
td .dim{color:var(--ink-4);font-size:12px;display:block;margin-top:2px}
```

### 7.7 Catalog grid (`.mgrid`) — inventories (measurements, endpoints, modules)

```html
<div class="mgrid reveal">
  <div class="mcard">
    <h4>노드 메트릭 <span class="cnt tnum">7</span></h4>
    <div class="chips"><span class="chip">cpu</span><span class="chip">mem</span>…</div>
  </div>
  …
</div>
```

```css
.mgrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:14px}
.mcard{border:1px solid var(--line-soft);border-radius:var(--radius);background:var(--paper);
  padding:18px;transition:transform .22s,box-shadow .22s,border-color .22s}
.mcard:hover{transform:translateY(-3px);box-shadow:0 10px 24px rgba(60,50,30,.08);border-color:var(--brand-kraft)}
.mcard h4{font-size:13.5px;font-weight:700;color:var(--ink);display:flex;align-items:center;gap:8px}
.mcard h4 .cnt{font-size:10.5px;font-weight:800;color:var(--accent-deep);
  background:rgba(217,119,87,.1);border-radius:999px;padding:2px 8px}
.chips{display:flex;flex-wrap:wrap;gap:6px;margin-top:12px}
.chip{font-size:11px;color:var(--ink-3);background:var(--surface-1);
  border:1px solid var(--line-softer);border-radius:999px;padding:3px 9px;
  transition:background .2s,color .2s,border-color .2s;cursor:default}
.chip:hover{background:var(--accent);color:#fff;border-color:var(--accent)}
```

### 7.8 Findings (`.finds`) — observations / improvement points

Numbered cards with a clay left rule; hover slides right.

```html
<div class="finds reveal">
  <div class="find">
    <div class="fn tnum">01</div>
    <div><b>v1/v2 혼재 잔재</b><p>…one-or-two-sentence detail with <code>inline code</code>…</p></div>
  </div>
  …
</div>
```

```css
.finds{display:flex;flex-direction:column;gap:12px}
.find{display:flex;gap:16px;align-items:flex-start;border:1px solid var(--line-soft);
  border-left:3px solid var(--accent);border-radius:var(--radius-sm);background:var(--paper);
  padding:16px 18px;transition:transform .22s,box-shadow .22s}
.find:hover{transform:translateX(4px);box-shadow:0 8px 20px rgba(60,50,30,.07)}
.find .fn{flex:0 0 auto;font-family:var(--font-display);font-weight:700;font-size:20px;
  color:var(--accent-deep);line-height:1.2;min-width:30px}
.find b{font-size:14px;color:var(--ink)}
.find p{font-size:13px;color:var(--ink-3);margin-top:4px}
.find code{font-size:11.5px;background:var(--surface-2);border-radius:4px;padding:1px 5px}
```

### 7.9 Program involvement table (`.prog`) — **mandatory in every analysis**

> Every analysis page **must** include a program/module involvement table answering *“which programs are related to this topic?”* Here **프로그램 = 파일 하나하나** (each individual source file is a “program”). One row per **module/group**; the last column lists that module’s individual **file chips** (never a `·`-separated string). Module-level involvement badges + per-file involvement dots. Most-involved rows get a clay left rule (`.hotrow`). Always include the legend caption.

```html
<div class="table-wrap reveal">
  <table class="prog">
    <thead><tr><th>모듈</th><th>역할</th><th>관여</th>
      <th>프로그램 파일 <span style="font-weight:500;color:var(--ink-4)">(각 파일이 하나의 프로그램)</span></th></tr></thead>
    <tbody>
      <tr class="hotrow">
        <td class="pname"><code>openstackit-common</code></td>
        <td>비즈니스 로직</td>
        <td><span class="pb read">READ</span><span class="pb write">WRITE</span></td>
        <td class="pfiles">
          <span class="file r w" title="실시간 조회 + LB 쓰기">MonitoringServiceImpl<span class="ext">.java</span></span>
          <span class="file r" title="리포팅 집계 조회">BatchReportingTelegraf<span class="ext">.java</span></span>
        </td>
      </tr>
      <tr>
        <td class="pname"><code>openstackit-openstack4j</code></td>
        <td>OpenStack API 래퍼</td>
        <td><span class="pb none">—</span></td>
        <td class="pfiles"><span style="font-size:11.5px;color:var(--ink-5)">관련 파일 없음</span></td>
      </tr>
    </tbody>
  </table>
</div>
<p class="prog-cap reveal">▲ 파일 칩 앞 점: <span class="file r" style="pointer-events:none">조회</span> <span class="file w" style="pointer-events:none">쓰기</span> <span class="file c" style="pointer-events:none">구성</span> <span class="file i" style="pointer-events:none">설치</span> · 모듈 배지는 해당 모듈의 전체 관여도</p>
```

```css
.prog .pname code{font-size:12.5px;font-weight:700;background:var(--surface-2);border-radius:4px;padding:2px 7px;color:var(--ink)}
.prog tr.hotrow td:first-child{box-shadow:inset 3px 0 0 var(--accent)}
/* file chips — one chip = one program file */
.pfiles{display:flex;flex-wrap:wrap;gap:6px;max-width:440px}
.file{
  display:inline-flex;align-items:center;gap:6px;cursor:default;
  font-family:var(--font-mono);font-size:11px;font-weight:600;color:var(--ink-2);
  background:var(--paper);border:1px solid var(--line-soft);border-radius:6px;
  padding:4px 9px 4px 8px;
  transition:transform .18s,border-color .18s,box-shadow .18s,background .18s;
}
.file::before{content:"";flex:0 0 auto;width:6px;height:6px;border-radius:50%}
.file.r::before{background:var(--accent)}
.file.w::before{background:var(--brand-clay-deep)}
.file.r.w::before{background:linear-gradient(135deg,var(--accent) 50%,var(--brand-clay-deep) 50%)}
.file.c::before{background:var(--ink-5)}
.file.i::before{background:var(--brand-kraft)}
.file:hover{transform:translateY(-2px);border-color:var(--accent);background:var(--surface-1);box-shadow:0 4px 10px rgba(60,50,30,.08)}
.file .ext{color:var(--ink-5);font-weight:400}
/* module-level badges */
.pb{display:inline-block;font-size:10px;font-weight:800;letter-spacing:.06em;border-radius:999px;
  padding:3px 9px;margin:1px 4px 1px 0;vertical-align:middle;white-space:nowrap}
.pb.read{color:var(--accent-deep);background:rgba(217,119,87,.12);border:1px solid rgba(217,119,87,.3)}
.pb.write{color:#fff;background:var(--accent);border:1px solid var(--accent)}
.pb.config{color:var(--ink-3);background:var(--surface-2);border:1px solid var(--line-soft)}
.pb.install{color:var(--ink-3);background:transparent;border:1px dashed var(--brand-kraft)}
.pb.none{color:var(--ink-5);background:transparent;border:1px dashed var(--line-soft)}
.prog-cap{font-size:11.5px;color:var(--ink-5);margin-top:10px}
```

`--font-mono` (add to `:root`): a system monospace stack so file names read as code — `--font-mono:"JetBrains Mono","SF Mono","Cascadia Code",Consolas,monospace;` (no extra CDN).

Per-file dot classes (combine as needed, e.g. `class="file r w"`):

| Class | Meaning | Dot |
|---|---|---|
| `.r` | reads/queries | clay |
| `.w` | writes/mutates | deep clay |
| `.c` | configures/wires | warm gray |
| `.i` | installs/deploys | kraft |

Module badge semantics (adapt labels to the domain, keep the visual grammar): `read` clay tint · `write` solid clay (strongest) · `config` neutral · `install` dashed kraft · `none` dashed dimmed.

Rules: sort rows by involvement (most-involved first); mark the top rows `.hotrow`; **every chip = one real file**, with a `title` tooltip describing its role; show the extension in a dimmed `.ext` span (or a `×N` count for an aggregate chip of many similar files, e.g. `Flux·Aggregate DTO ×9`); unrelated modules still get a “관련 파일 없음” row so coverage is visibly complete.

### 7.10 Dependency graph (`.depgraph`) — **mandatory file-level map**

> Every analysis page **must** also include a file-level dependency graph showing **all** involved files as nodes. It is a swimlane node-link diagram: one vertical lane per module/layer/package, each node = one file, edges = relationships. Rendered as inline SVG by a small **data-driven vanilla-JS template** — you fill in three arrays (`LANES`, `NODES`, `EDGES`) for the codebase being analyzed; the template computes layout, routes bezier edges, and wires hover-focus + click-pin.

Markup (the SVG is injected before the legend by the script):

```html
<div class="depgraph reveal" id="dg">
  <!-- SVG rendered by the template script -->
  <div class="dg-legend">
    <span class="li"><span class="swatch"></span>uses (컴파일 의존)</span>
    <span class="li"><span class="swatch bean"></span>bean 주입 / 런타임</span>
    <span class="li"><span class="swatch db"></span>외부 시스템 조회/쓰기</span>
    <span class="li"><span class="swatch install"></span>설치/배포</span>
    <span class="dg-hint">노드 호버 → 연결 강조 · 클릭 → 고정</span>
  </div>
</div>
```

```css
.depgraph{border:1px solid var(--line-soft);border-radius:var(--radius);background:var(--paper);overflow:hidden}
.depgraph svg{display:block;width:100%;height:auto}
.dg-lane{fill:var(--surface-1);opacity:.5}
.dg-lane-label{font-family:var(--font-sans);font-size:10.5px;font-weight:800;letter-spacing:.12em;fill:var(--ink-4)}
.dg-node{cursor:pointer;transition:opacity .25s}
.dg-node rect{fill:var(--paper);stroke:var(--line-soft);stroke-width:1;transition:stroke .2s,stroke-width .2s}
.dg-node:hover rect{stroke:var(--accent);stroke-width:1.5}
.dg-node .dg-name{font-family:var(--font-mono);font-size:10.5px;font-weight:600;fill:var(--ink-2)}
.dg-node .dg-sub{font-family:var(--font-sans);font-size:8.5px;fill:var(--ink-5)}
.dg-node.db rect{fill:rgba(217,119,87,.08);stroke:var(--brand-kraft)}
.dg-node.db .dg-name{fill:var(--accent-deep);font-weight:700}
/* edge casing: a paper-ivory halo under each line keeps crossings/overlaps legible */
.dg-edge-halo{fill:none;stroke:#FBFAF6;stroke-width:3.6;stroke-linecap:round;transition:opacity .25s}
.dg-edge{fill:none;stroke:var(--ink-5);stroke-width:1.5;transition:stroke .2s,opacity .25s,stroke-width .2s}
.dg-edge.bean{stroke-dasharray:5 4;stroke:var(--ink-4)}
.dg-edge.install{stroke-dasharray:5 4;stroke:var(--brand-kraft)}
.dg-edge.db{stroke:var(--brand-kraft);stroke-width:1.6}
.depgraph.focus .dg-node{opacity:.22}
.depgraph.focus .dg-node.on{opacity:1}
.depgraph.focus .dg-edge{opacity:.08}
.depgraph.focus .dg-edge-halo{opacity:.06}
.depgraph.focus .dg-edge.on{opacity:1;stroke:var(--accent);stroke-width:2.2}
.depgraph.focus .dg-edge-halo.on{opacity:1}
.depgraph.focus .dg-edge.on.db,.depgraph.focus .dg-edge.on.install{stroke:var(--brand-clay-deep)}
/* click-pin: the locked node keeps a clay ring; the hint switches to release instructions */
.dg-node.pinned rect{stroke:var(--accent-deep);stroke-width:2;fill:rgba(217,119,87,.06)}
.dg-node.pinned .dg-name{fill:var(--accent-deep)}
.depgraph.pinned .dg-hint{color:var(--accent-deep);font-weight:600}
.dg-legend{display:flex;flex-wrap:wrap;align-items:center;gap:8px 22px;padding:12px 16px;border-top:1px solid var(--line-softer);font-size:11.5px;color:var(--ink-4)}
.dg-legend .li{display:inline-flex;align-items:center;gap:7px}
.dg-legend .swatch{width:26px;height:0;border-top:2px solid var(--ink-6)}
.dg-legend .swatch.bean{border-top-style:dashed}
.dg-legend .swatch.db{border-top-color:var(--brand-kraft)}
.dg-legend .swatch.install{border-top:2px dashed var(--brand-kraft)}
.dg-hint{margin-left:auto;color:var(--ink-5);font-size:11px}
```

**The reusable renderer template.** Paste this `<script>` and replace only the `LANES` / `NODES` / `EDGES` arrays (and the container `id` if you change it). Everything else — layout, edge routing, arrows, hover-focus, click-pin — is generic.

```js
(function(){
  const wrap = document.getElementById('dg');
  if(!wrap) return;

  /* ── FILL THIS: one lane per module/layer/package, left→right in dependency order ── */
  const LANES = [
    {id:'core',   label:'CORE · 기반',   x:14,  w:186},
    {id:'common', label:'COMMON · 비즈니스', x:230, w:230},
    /* …one lane per group. Keep a ~30px GUTTER between lanes (next.x − (prev.x+prev.w) ≈ 30)
         so edges that cross the gap don't pile up; keep total width ≈ 1080… */
  ];
  const NODE_H=42, GAP=13, TOP=46;

  /* ── FILL THIS: one node per file. lane = a LANES.id; `db:true` marks the single
        external/system anchor node (auto-placed bottom-center). Aggregate many similar
        files (DTOs, migrations…) into one node with a `×N` sub. ── */
  const NODES = [
    {id:'c_prop', lane:'core',   name:'InfluxDB2Properties', sub:'influx.* 설정'},
    {id:'m_mon',  lane:'common', name:'MonitoringServiceImpl', sub:'실시간 조회'},
    {id:'db',     lane:'db',     name:'InfluxDB v1.8', sub:'시계열 저장소', db:true},
    /* …every involved file… */
  ];

  /* ── FILL THIS: type = 'use' (solid) | 'bean' (dashed, runtime/DI) |
        'db' (to the external anchor) | 'install' (dashed kraft) ── */
  const EDGES = [
    {from:'m_mon', to:'c_prop', type:'use'},
    {from:'m_mon', to:'db',     type:'db'},
    /* … */
  ];

  /* ── generic below — do not edit ── */
  const byId={}; NODES.forEach(n=>byId[n.id]=n);
  const byLane={}; NODES.forEach(n=>{ if(!n.db)(byLane[n.lane]=byLane[n.lane]||[]).push(n); });
  Object.keys(byLane).forEach(lid=>{
    const lane=LANES.find(l=>l.id===lid);
    byLane[lid].forEach((n,i)=>{ n.x=lane.x+6; n.w=lane.w-12; n.y=TOP+i*(NODE_H+GAP); n.h=NODE_H; });
  });
  const maxBottom=Math.max(...NODES.filter(n=>!n.db).map(n=>n.y+n.h));
  const db=NODES.find(n=>n.db);
  if(db){ db.w=300; db.h=48; db.x=(1080-db.w)/2; db.y=maxBottom+46; }
  const H=(db?db.y+db.h:maxBottom)+18;

  const NS='http://www.w3.org/2000/svg';
  const el=(t,a)=>{const e=document.createElementNS(NS,t);for(const k in a)e.setAttribute(k,a[k]);return e;};
  const svg=el('svg',{viewBox:`0 0 1080 ${H}`,role:'img','aria-label':'파일 의존성 그래프'});
  const defs=el('defs',{});
  const mk=(id,fill)=>{const m=el('marker',{id,viewBox:'0 0 10 10',refX:'8',refY:'5',markerWidth:'6',markerHeight:'6',orient:'auto-start-reverse'});m.appendChild(el('path',{d:'M0,0 L10,5 L0,10 z',fill}));defs.appendChild(m);};
  mk('arr-ink','#A8A093'); mk('arr-kraft','#D4A27F'); svg.appendChild(defs);

  LANES.forEach(l=>{
    svg.appendChild(el('rect',{class:'dg-lane',x:l.x,y:12,width:l.w,height:maxBottom-2,rx:8}));
    const t=el('text',{class:'dg-lane-label',x:l.x+10,y:30}); t.textContent=l.label; svg.appendChild(t);
  });

  /* ── anchor ports: spread edges along a node side so parallel lines fan out ── */
  EDGES.forEach(e=>{
    const s=byId[e.from], t=byId[e.to];
    if(t.db){ e.ss='bottom'; e.ts='top'; }
    else if(s.lane===t.lane){ e.ss='right'; e.ts='right'; }
    else if(s.x < t.x){ e.ss='right'; e.ts='left'; }
    else { e.ss='left'; e.ts='right'; }
  });
  function assignSlots(role){
    const groups={};
    EDGES.forEach(e=>{
      const id=role==='s'?e.from:e.to, side=role==='s'?e.ss:e.ts;
      (groups[id+'|'+side]=groups[id+'|'+side]||[]).push(e);
    });
    Object.values(groups).forEach(arr=>{
      arr.sort((a,b)=>{
        const o=role==='s'?byId[a.to]:byId[a.from], p=role==='s'?byId[b.to]:byId[b.from];
        return (o.x+o.y)-(p.x+p.y);
      });
      arr.forEach((e,i)=>{ e['_slot_'+role]=(i+1)/(arr.length+1); });
    });
  }
  assignSlots('s'); assignSlots('t');
  function anchor(n,side,slot){
    if(side==='left')  return {x:n.x,        y:n.y+n.h*slot};
    if(side==='right') return {x:n.x+n.w,    y:n.y+n.h*slot};
    if(side==='top')   return {x:n.x+n.w*slot,y:n.y};
    return {x:n.x+n.w*slot, y:n.y+n.h}; /* bottom */
  }
  /* bundle jitter: parallel edges between the same lane-pair get spread control points */
  const bc={};
  EDGES.forEach(e=>{ const k=e.ss+e.ts+byId[e.from].lane+byId[e.to].lane; e._bi=bc[k]||0; bc[k]=(bc[k]||0)+1; e._bk=k; });

  function pathFor(e){
    const s=byId[e.from], t=byId[e.to];
    const a=anchor(s,e.ss,e._slot_s), b=anchor(t,e.ts,e._slot_t);
    const jit=(e._bi-((bc[e._bk]-1)/2))*4;
    if(e.ss==='bottom'&&e.ts==='top'){
      const dy=(b.y-a.y)*0.5;
      return `M ${a.x} ${a.y} C ${a.x} ${a.y+dy}, ${b.x} ${b.y-dy}, ${b.x} ${b.y}`;
    }
    if(s.lane===t.lane){
      const lane=LANES.find(l=>l.id===s.lane);
      const peak=lane.x+lane.w+12+e._bi*8;   /* bow out into the right gutter */
      return `M ${a.x} ${a.y} C ${peak} ${a.y}, ${peak} ${b.y}, ${b.x} ${b.y}`;
    }
    const dx=Math.max(34,Math.abs(b.x-a.x)*0.42)+jit;
    const c1x=e.ss==='right'?a.x+dx:a.x-dx, c2x=e.ts==='right'?b.x+dx:b.x-dx;
    return `M ${a.x} ${a.y} C ${c1x} ${a.y}, ${c2x} ${b.y}, ${b.x} ${b.y}`;
  }

  /* edges — halo (casing) first, then the line, so crossings stay readable */
  EDGES.forEach(e=>{
    const d=pathFor(e);
    const h=el('path',{class:'dg-edge-halo',d}); h.dataset.from=e.from; h.dataset.to=e.to; svg.appendChild(h);
    const p=el('path',{class:'dg-edge '+e.type,d,
      'marker-end':(e.type==='db'||e.type==='install')?'url(#arr-kraft)':'url(#arr-ink)'});
    p.dataset.from=e.from; p.dataset.to=e.to; svg.appendChild(p);
  });

  NODES.forEach(n=>{
    const g=el('g',{class:'dg-node'+(n.db?' db':''),transform:`translate(${n.x},${n.y})`});
    g.dataset.id=n.id;
    g.appendChild(el('rect',{width:n.w,height:n.h,rx:7}));
    const nm=el('text',{class:'dg-name',x:9,y:17}); nm.textContent=n.name; g.appendChild(nm);
    const sb=el('text',{class:'dg-sub',x:9,y:31}); sb.textContent=n.sub; g.appendChild(sb);
    const tt=el('title',{}); tt.textContent=`${n.name} — ${n.sub}`; g.appendChild(tt);
    svg.appendChild(g);
  });

  wrap.insertBefore(svg, wrap.firstChild);

  /* hover = preview, click = pin (re-click / background click / Esc = release) */
  let pinned=null;
  const hint=wrap.querySelector('.dg-hint');
  const HINT_IDLE='노드 호버 → 연결 강조 · 클릭 → 고정';
  const HINT_PIN='고정됨 — 다시 클릭 · 빈 곳 클릭 · Esc 로 해제';
  const focus=id=>{
    wrap.classList.add('focus');
    const conn=new Set([id]);
    EDGES.forEach(e=>{ if(e.from===id||e.to===id){conn.add(e.from);conn.add(e.to);} });
    svg.querySelectorAll('.dg-node').forEach(nd=>nd.classList.toggle('on',conn.has(nd.dataset.id)));
    svg.querySelectorAll('.dg-edge, .dg-edge-halo').forEach(p=>p.classList.toggle('on',p.dataset.from===id||p.dataset.to===id));
  };
  const unfocus=()=>{ wrap.classList.remove('focus'); svg.querySelectorAll('.on').forEach(x=>x.classList.remove('on')); };
  const setPin=id=>{
    pinned=id;
    svg.querySelectorAll('.dg-node.pinned').forEach(x=>x.classList.remove('pinned'));
    if(id){
      svg.querySelector('.dg-node[data-id="'+id+'"]').classList.add('pinned');
      wrap.classList.add('pinned');
      if(hint)hint.textContent=HINT_PIN;
      focus(id);
    }else{
      wrap.classList.remove('pinned');
      if(hint)hint.textContent=HINT_IDLE;
      unfocus();
    }
  };
  svg.addEventListener('mouseover',e=>{const g=e.target.closest('.dg-node'); if(g)focus(g.dataset.id);});
  svg.addEventListener('mouseout', e=>{const g=e.target.closest('.dg-node'); if(g)(pinned?focus(pinned):unfocus());});
  svg.addEventListener('click',e=>{const g=e.target.closest('.dg-node'); setPin(g?(pinned===g.dataset.id?null:g.dataset.id):null);});
  document.addEventListener('keydown',e=>{ if(e.key==='Escape'&&pinned)setPin(null); });
})();
```

Authoring rules:

- **All involved files appear as nodes** — that is the point of this component. If a group is too numerous (DTOs, value objects, migrations), aggregate into one node with a `×N` sub rather than dropping them.
- Lanes run left→right in dependency order (most-depended-on left). One lane per module/layer/package; keep total width ≈ 1080.
- Node `name` = short file/class name (fits ~24 mono chars per 170px lane); put the full name + role in the `<title>` tooltip via `sub`.
- Keep the edge set meaningful (≈15–25): direct `use`, runtime/DI `bean`, edges to the external `db` anchor, and `install`. Too many edges → spaghetti; prefer aggregating targets.
- Exactly **one** `db:true` anchor (the external system / DB / service the code talks to). Omit it and the `db`/`install` edge types if the analysis has no external anchor.
- Hover-focus + click-pin is the readability mechanism — always keep it wired: hover previews a node's connections, **click locks (pins) the highlight**, re-click / background click / Esc releases; while pinned, hovering other nodes previews them and mouseout snaps back to the pin.
- **Legibility is built in — keep it.** Lanes must sit ~30px apart (next.x − (prev.x+prev.w) ≈ 30) so gap-crossing edges don't pile up. The renderer already (a) staggers anchor ports along each node side so parallel lines fan out instead of overlapping, (b) bows same-lane edges out into the right gutter, (c) spreads parallel control points with bundle jitter, and (d) draws an ivory casing halo under every line so crossings stay separable. Do not remove these; if a graph still looks tangled, **reduce edges by aggregating targets** (e.g. one `×N` DTO node) rather than thinning the casing.

---

## 8. Motion (vanilla JS)

Three reveal behaviors + two ambient loops. All optional-enhancement — the page must read fine with JS disabled.

```js
/* scroll reveal — add .in when 12% visible */
const io = new IntersectionObserver(es => es.forEach(e => {
  if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); }
}), { threshold: .12 });
document.querySelectorAll('.reveal').forEach(el => io.observe(el));

/* count-up stats — ease-out cubic, 1.1s */
const cio = new IntersectionObserver(es => es.forEach(e => {
  if (!e.isIntersecting) return;
  const el = e.target, end = +el.dataset.count, t0 = performance.now(), dur = 1100;
  (function tick(t){
    const p = Math.min((t - t0) / dur, 1), v = Math.round(end * (1 - Math.pow(1 - p, 3)));
    el.firstChild.nodeValue = v;
    if (p < 1) requestAnimationFrame(tick);
  })(t0);
  cio.unobserve(el);
}), { threshold: .5 });
document.querySelectorAll('.stat .num').forEach(el => cio.observe(el));

/* bar fills — staggered 90ms */
const bio = new IntersectionObserver(es => es.forEach(e => {
  if (!e.isIntersecting) return;
  e.target.querySelectorAll('.bar-fill').forEach((f, i) =>
    setTimeout(() => f.style.width = f.dataset.w + '%', i * 90));
  bio.unobserve(e.target);
}), { threshold: .3 });
document.querySelectorAll('.bars').forEach(el => bio.observe(el));
```

```css
.reveal{opacity:0;transform:translateY(18px);transition:opacity .7s ease,transform .7s ease}
.reveal.in{opacity:1;transform:none}
```

Ambient loops (CSS only, defined with their components): `pulse` (live dot, §4.1), `flowdash` (pipeline arrows, §7.1), `draw`/`fadein` (sparkline, §4.2).

Rules: transition only `opacity / transform / background / border-color / box-shadow / width`. No parallax, no scroll-jacking. `html{scroll-behavior:smooth}`.

---

## 9. Responsive

At `≤860px`:

- `.flow` collapses to a **vertical** column; `.flow-arrow` rotates (vertical dashes via `repeating-linear-gradient(180deg, …)` + `flowdashV` keyframes, downward arrowhead).
- `.bar-row` drops the label column (label spans full width above the track, left-aligned).
- `.layer .dep` pills hide.
- Grids (`auto-fit, minmax(...)`) reflow naturally.

```css
@media (max-width:860px){
  .flow{flex-direction:column}
  .flow-node{border-right:none;border-bottom:1px solid var(--line-softer)}
  .flow-node:last-child{border-bottom:none}
  .flow-arrow{flex:0 0 34px;align-self:center;width:2px;height:30px;margin:0 auto}
  .flow-arrow::before{background:repeating-linear-gradient(180deg,var(--accent) 0 6px,transparent 6px 12px);animation-name:flowdashV}
  .flow-arrow::after{right:auto;top:auto;bottom:-1px;left:50%;transform:translateX(-50%);border:5px solid transparent;border-top:7px solid var(--accent)}
  @keyframes flowdashV{to{background-position:0 12px}}
  .bar-row{grid-template-columns:1fr 46px}
  .bar-row .bl{grid-column:1/-1;text-align:left}
  .layer .dep{display:none}
}
```

---

## 10. Authoring Checklist

Check before delivering:

- [ ] `<html lang="ko">`; Pretendard + Noto Serif KR CDNs loaded (the only externals)
- [ ] Page background ivory `--surface` (never `#ffffff`); cards `--paper` on top; text warm ink (no cool gray)
- [ ] The **only** chromatic accent is clay — used for hot nodes, primary bars, key numbers, live dot, section numbers
- [ ] Masthead opens with the **subject's structure** (kicker → title → lede → meta → sparkline/stat strip), not a centered hero
- [ ] Every section = number + serif H2 (declarative, dash clause) + 1–2 sentence lede + **one visual**
- [ ] Exactly one `.hot` node per flow; arrows point in the real data direction
- [ ] All numeric elements have `.tnum`
- [ ] Stat numbers count up; bars fill on reveal; sections reveal on scroll
- [ ] No chart/diagram libraries — pure CSS + inline SVG + vanilla JS
- [ ] `≤860px` breakpoint handled (vertical flow, stacked bars)
- [ ] Findings use the numbered clay-rule cards, each ≤2 sentences
- [ ] **Program involvement table (§7.9) is present** — one row per module, each individual file as a `.file` chip (never a `·`-separated string) with involvement dot + `title` tooltip, module badges, `.hotrow` on the most-involved, legend caption, “관련 파일 없음” rows for unrelated modules
- [ ] **Dependency graph (§7.10) is present** — data-driven SVG with ALL involved files as nodes, swimlanes per module, edge types (use/bean/db/install), hover-focus + click-pin wired, legend
- [ ] Both §7.9 and §7.10 are filled from the *actual* files of the analyzed codebase (the template is generic — no InfluxDB-specific leftovers)
- [ ] Single self-contained `.html` file
