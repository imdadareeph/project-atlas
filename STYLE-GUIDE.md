# Project Atlas: Reusable Style Guide

Design system used by `index.html` and `repo-benchmarks/`. Copy the blocks below into any new page and it will match. Pure HTML + CSS, no build step, no framework, no font downloads.

**Look:** editorial, technical, calm. Warm amber accent on cool slate. Monospace for labels, tight-tracked sans for headlines, thin 1px borders, square corners (only pills and round buttons are rounded).

---

## 1. Quick start (copy-paste skeleton)

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Page title</title>
<!-- apply saved theme BEFORE paint, prevents flash -->
<script>try{if(localStorage.getItem('theme')==='light')document.documentElement.dataset.theme='light'}catch(e){}</script>
<style>/* paste sections 2 to 8 here */</style>
</head>
<body id="top">
  <nav class="nav">…</nav>
  <main>
    <header class="hero">…</header>
    <section id="one">…</section>
  </main>
  <footer>…</footer>
  <button class="to-top" id="to-top">↑</button>
  <script>/* paste section 9 here */</script>
</body>
</html>
```

For a page inside a sub-folder, image paths become `../img/...`.

---

## 2. Colour tokens

All colour lives in CSS variables on `:root`. Never hardcode hex in components; use the token. Dark is the default; light is opt-in via `data-theme="light"`.

| Token | Dark | Light | Use |
|---|---|---|---|
| `--bg` | `#0c1015` | `#f6f4ef` | page background |
| `--surface` | `#111820` | `#ffffff` | cards, buttons |
| `--surface2` | `#151e27` | `#faf8f4` | card hover, floating buttons |
| `--ink` | `#edf1f4` | `#171b20` | primary text |
| `--muted` | `#9ca9b5` | `#4d5965` | body copy, secondary text |
| `--faint` | `#65717d` | `#76818c` | meta, captions, footer |
| `--line` | `#29333d` | `#dcd7cd` | every 1px border / divider |
| `--accent` | `#e2a76f` | `#b0641a` | brand colour: kickers, links, active state, bars |
| `--accent2` | `#8ea9c1` | `#3d6485` | secondary: inline `code`, comparison bars |
| `--accent-line` | `#77604b` | `#d9b48a` | accent-tinted border on hover |
| `--glow` | `#17212a` | `#e9e2d3` | soft corner glow behind body |
| `--nav-bg` | `rgba(12,16,21,.88)` | `rgba(246,244,239,.88)` | translucent navbar |
| `--hover` | `rgba(255,255,255,.06)` | `rgba(0,0,0,.06)` | pill hover fill |
| `--hero-img` | `url(img/bg.webp)` | `url(img/bg-lite.webp)` | hero still (image-only variant / fallback, **placeholder**, see §6 and §10) |
| `--hero-shade` | `rgba(12,16,21,.55)` | `rgba(246,244,239,.5)` | left-side scrim so text stays readable |

```css
:root{
  --bg:#0c1015;--surface:#111820;--surface2:#151e27;--ink:#edf1f4;--muted:#9ca9b5;
  --faint:#65717d;--line:#29333d;--accent:#e2a76f;--accent2:#8ea9c1;--max:1100px;
  --sans:Inter,ui-sans-serif,system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
  --mono:"SFMono-Regular",Consolas,"Liberation Mono",monospace;
  --hero-img:url(img/bg.webp);--hero-shade:rgba(12,16,21,.55);--glow:#17212a;
  --nav-bg:rgba(12,16,21,.88);--hover:rgba(255,255,255,.06);--accent-line:#77604b;
  color-scheme:dark;
}
:root[data-theme=light]{
  --bg:#f6f4ef;--surface:#ffffff;--surface2:#faf8f4;--ink:#171b20;--muted:#4d5965;
  --faint:#76818c;--line:#dcd7cd;--accent:#b0641a;--accent2:#3d6485;
  --hero-img:url(img/bg-lite.webp);--hero-shade:rgba(246,244,239,.5);--glow:#e9e2d3;
  --nav-bg:rgba(246,244,239,.88);--hover:rgba(0,0,0,.06);--accent-line:#d9b48a;
  color-scheme:light;
}
```

**Rules**
- Accent is used sparingly: kickers, one highlighted word in the H1, active tab, primary bar, links. Never for large fills except bars and active buttons.
- Text on an accent fill uses `var(--bg)` (dark text on amber in both themes).
- Want a different brand? Change only `--accent`, `--accent2`, `--accent-line`. Everything follows.
- **Navbar stays dark in light theme.** It re-declares the dark tokens locally (see §5).

---

## 3. Typography

No web fonts. System stacks only, so pages load instantly and work offline.

| Role | Family | Token |
|---|---|---|
| Headlines, body | Inter, then system UI sans | `--sans` |
| Labels, kickers, meta, tabs, buttons, code | SF Mono / Consolas | `--mono` |

Body: `line-height:1.6` (long-form article pages use `1.72`).

| Element | Style |
|---|---|
| **Hero H1** | `clamp(44px,7vw,82px)`, `line-height:.98`, `letter-spacing:-.055em`. One key word wrapped in `<span>` coloured `--accent` |
| **Hero dek** (subtitle) | `clamp(18px,2.2vw,24px)`, `--muted`, `max-width:640px` |
| **H2** | `clamp(26px,3.5vw,38px)`, `line-height:1.1`, `letter-spacing:-.03em` |
| **H3 (card title)** | `20px`, `line-height:1.25`, `letter-spacing:-.02em` |
| **Body paragraph** | `17px`, `--muted`, `max-width:720px` |
| **Kicker / eyebrow** | mono `600 11px`, `letter-spacing:.14em`, UPPERCASE, `--accent`. Format: `01 · ABOUT` |
| **Tag / chip** | mono `600 10px`, `letter-spacing:.12em` (tag) or mono `11px` (chip) |
| **Meta / footer** | mono `11px`, `--faint` |
| **Inline `code`** | mono `.9em`, `--accent2` |

Pattern: **big tight-tracked sans headline + tiny wide-tracked mono label above it.** That contrast is the signature look.

```css
.eyebrow,.kicker{font:600 11px/1.2 var(--mono);letter-spacing:.14em;color:var(--accent)}
.hero h1{font-size:clamp(44px,7vw,82px);line-height:.98;letter-spacing:-.055em;margin:24px 0}
.hero h1 span{color:var(--accent)}
.dek{font-size:clamp(18px,2.2vw,24px);color:var(--muted);max-width:640px;margin:0 0 32px}
h2{font-size:clamp(26px,3.5vw,38px);letter-spacing:-.03em;line-height:1.1;margin:12px 0 28px}
code{font-family:var(--mono);font-size:.9em;color:var(--accent2)}
```

### Long-form article block (report pages)
`repo-benchmarks/index.html` follows its visual sections with plain markdown-style HTML placed directly under `<main>`. It is styled by child selectors, not classes:
```css
body{line-height:1.72}                               /* article pages read looser than the 1.6 default */
main>h1{font-size:48px;line-height:1.05;letter-spacing:-.04em;margin-top:120px}
main>h2{font-size:32px;line-height:1.1;letter-spacing:-.03em;margin:80px 0 22px;padding-top:30px;border-top:1px solid var(--line)}
main>p{max-width:790px;color:var(--muted);font-size:17px}
main>p code,td code,pre code{font-family:var(--mono);color:var(--accent2)}
.table-wrap{overflow-x:auto;margin:28px 0 38px}
hr{border:0;border-top:1px solid var(--line);margin:80px 0}
```
(Source uses `#c3ccd3` for `main>p` and `#d9b992` for code; tokens above are the theme-safe equivalents.)

---

## 4. Layout & base

```css
*{box-sizing:border-box}
html{scroll-behavior:smooth;background:var(--bg)}
body{margin:0;overflow-x:clip;
  background:radial-gradient(circle at 80% 0%,var(--glow) 0,transparent 34%),var(--bg);
  color:var(--ink);font-family:var(--sans);line-height:1.6}
[id]{scroll-margin-top:72px}            /* anchors clear the sticky navbar */
main{max-width:var(--max);margin:auto;padding:0 28px 100px}
a{color:inherit;text-decoration:none}
section{padding:56px 0}
```

- Content column: `--max:1100px`, 28px side padding (16px on phones).
- Vertical rhythm: sections `56px` padding (visual/report sections use `105px 0 40px`).
- Body has a faint radial glow in the top-right corner. It is the only "gradient" decoration on the page.
- Section separators: `1px solid var(--line)`. No shadows anywhere.
- Section numbering convention: kicker `01 · ABOUT`, `02 · PROJECTS`, …

---

## 5. Navigation bar

Sticky, 56px, translucent with backdrop blur, 1px bottom border. Layout: `[theme button] [brand] [scrollable links…] ········ [handle on right]`.

```html
<nav class="nav" aria-label="Primary">
  <button class="theme-btn" id="theme-btn" aria-label="Switch theme"></button>
  <span class="nav-brand">PROJECT ATLAS</span>
  <div class="nav-links"><a href="#top">Home</a><a href="#about">About</a></div>
  <span class="nav-tag">@handle</span>
</nav>
```

```css
.nav{position:sticky;top:0;z-index:10;display:flex;align-items:center;gap:20px;height:56px;padding:0 28px;
  background:var(--nav-bg);backdrop-filter:blur(12px);border-bottom:1px solid var(--line)}
/* navbar stays dark in light theme: re-declare dark tokens locally */
:root[data-theme=light] .nav{--nav-bg:rgba(12,16,21,.9);--ink:#edf1f4;--muted:#9ca9b5;--line:#29333d;
  --surface:#111820;--accent:#e2a76f;--hover:rgba(255,255,255,.08);--accent-line:#77604b}
.nav-brand{font:600 12px var(--mono);letter-spacing:.08em;color:var(--accent);white-space:nowrap}
.nav-tag{margin-left:auto;flex:none;font:600 12px var(--mono);letter-spacing:.04em;color:var(--accent);white-space:nowrap}
.nav-links{min-width:0;display:flex;gap:4px;overflow-x:auto;scrollbar-width:none}
.nav-links a{color:var(--muted);font-size:13px;padding:6px 12px;border-radius:99px;white-space:nowrap}
.nav-links a:hover{color:var(--ink);background:var(--hover)}
.nav-links a.ext{color:var(--accent);border:1px solid var(--accent-line)}  /* optional: link to another page */
.theme-btn{width:34px;height:34px;flex:none;border-radius:50%;border:1px solid var(--line);
  background:var(--surface);color:var(--accent);font-size:16px;line-height:1;cursor:pointer}
.theme-btn:hover{border-color:var(--accent-line)}
```

Links are **pills** (`border-radius:99px`), the only rounded text element. Links scroll horizontally on small screens with the scrollbar hidden. On phones, hide `.nav-brand`.

---

## 6. Hero (looping video background, image fallback)

Full-bleed **looping video** behind a left-aligned text block. The `<video>` and the `::before` scrim both break out of the 1100px column to span the full viewport width. An image-only version is described at the end of this section.

```html
<header class="hero">
  <video class="hero-video" id="hero-video" muted loop playsinline preload="auto" aria-hidden="true"></video>
  <div class="eyebrow">CATEGORY · SUBTITLE</div>
  <h1>Page <span>Title</span></h1>
  <p class="dek">One-sentence description.</p>
  <div class="hero-meta"><span>Fact one</span><span>Fact two</span><span>Fact three</span></div>
</header>
```

```css
.hero{position:relative;isolation:isolate;min-height:520px;display:flex;flex-direction:column;justify-content:center;padding:80px 0}
.hero::before{content:"";position:absolute;inset:0 calc(50% - 50vw);z-index:-1;   /* scrim only, no image */
  background:
    linear-gradient(90deg,var(--hero-shade),transparent 60%),  /* left scrim for legibility */
    linear-gradient(180deg,transparent 70%,var(--bg))}          /* bottom fade into page */
.hero-video{position:absolute;top:0;left:50%;width:100vw;height:100%;transform:translateX(-50%);
  object-fit:cover;z-index:-2;background:var(--bg)}             /* sits under the scrim */
.hero-meta{display:flex;gap:10px;flex-wrap:wrap;color:var(--faint);font:11px var(--mono)}
```

```js
// One <video>, source swapped per theme. Poster = still image shown until the video loads.
const hv=document.getElementById('hero-video');
const heroSrc=()=>{const l=document.documentElement.dataset.theme==='light',n=l?'lite':'dark';
  hv.poster=l?'img/bg-lite.webp':'img/bg.webp';hv.src='videos/'+n+'-loop.mp4';
  if(!matchMedia('(prefers-reduced-motion:reduce)').matches)hv.play().catch(()=>{})};
heroSrc();                       // on load
// and call heroSrc() at the end of the theme-toggle click handler
```

Layers, bottom to top: **video, left scrim, bottom fade to `--bg`, text**. The scrim and fade keep text readable over any footage and blend the hero into the page.

**Why the JS:** browsers only autoplay `muted` video, `poster` gives an instant still, and visitors with reduced-motion set get the poster only (no `play()`).

**Image-only variant** (no video, no JS): drop the `<video>`, and put the image back as the last layer in `::before`:
```css
.hero::before{background:linear-gradient(90deg,var(--hero-shade),transparent 60%),linear-gradient(180deg,transparent 70%,var(--bg)),var(--hero-img) center/cover no-repeat}
```

---

## 7. Components

### Card (with hover lift)
```css
.card{display:flex;flex-direction:column;border:1px solid var(--line);background:var(--surface);padding:22px;min-height:230px;transition:.2s}
.card:hover{border-color:var(--accent-line);background:var(--surface2);transform:translateY(-3px)}
.card .tag{font:600 10px var(--mono);letter-spacing:.12em;color:var(--accent)}
.card h3{font-size:20px;line-height:1.25;letter-spacing:-.02em;margin:12px 0 10px}
.card p{color:var(--muted);font-size:14px;margin:0 0 18px;flex:1}
.card .chips{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:14px}
.card .chips span{border:1px solid var(--line);padding:3px 8px;font:11px var(--mono);color:var(--faint)}
.card .open{font:600 12px var(--mono);color:var(--accent)}
/* placeholder / "coming soon" card: dashed, transparent, no hover motion */
.card.soon{border-style:dashed;background:transparent;justify-content:center;align-items:center;text-align:center;color:var(--faint);font:12px var(--mono)}
.card.soon:hover{transform:none;border-color:var(--line);background:transparent}
```
Grid: `repeat(3,1fr)` with `gap:18px`; 2 columns under 900px; 1 column under 600px.

### Chart panel
```css
.chart-panel{margin:40px 0;padding:28px;border:1px solid var(--line);background:rgba(17,24,32,.72)}
.chart-head{display:flex;justify-content:space-between;gap:20px;align-items:center;margin-bottom:25px}
.label{font:600 11px/1.2 var(--mono);letter-spacing:.14em;color:var(--accent)}
.note{font:11px var(--mono);color:var(--faint);margin-left:15px}
.chart-note{font-size:12px;color:var(--faint);margin-top:22px}
```

### Flow / node chips (diagram boxes)
```css
.node{border:1px solid var(--line);background:rgba(255,255,255,.025);padding:11px 14px;font:600 13px var(--mono)}
.node.primary{border-color:var(--accent-line);background:rgba(226,167,111,.08);color:var(--accent)}  /* the highlighted one */
.arrow{font-size:24px;color:var(--faint)}
```

### Architecture diagram (3 columns with arrows)
Left/right columns list boxes; the centre column is the highlighted one (accent border + faint amber fill). Collapses to one column under 760px.
```css
.architecture{display:grid;grid-template-columns:1fr auto 1.3fr auto 1fr;gap:20px;align-items:center;margin:40px 0;padding:28px;border:1px solid var(--line);background:rgba(17,24,32,.72)}
.arch-column{display:flex;flex-direction:column;gap:8px}
.arch-title{font:600 11px/1.2 var(--mono);letter-spacing:.14em;color:var(--accent);margin-bottom:8px}
.arch-item{border:1px solid var(--line);background:rgba(255,255,255,.025);padding:11px 14px;font:600 12px var(--mono)}
.arch-column small{color:var(--faint);font:10px var(--mono);margin-top:5px}
.arch-connector{color:var(--accent);font-size:25px}
.arch-column.central{padding:22px;border:1px solid var(--accent-line);background:rgba(226,167,111,.04)}
```

### Two-sided boundary panel (before | after)
Two halves split by a 1px vertical line (horizontal on mobile). Right side shows the "future" item as a big accent word.
```css
.boundary{display:grid;grid-template-columns:1fr 1px 1fr;gap:35px;align-items:center;margin:40px 0;padding:28px;border:1px solid var(--line);background:rgba(17,24,32,.72)}
.boundary-line{height:100px;background:var(--line)}
.repoatlas{font-size:34px;letter-spacing:-.04em;margin-top:15px;color:var(--accent)}
```

### Table / code block
```css
table{width:100%;border-collapse:collapse;font-size:13px}
th{font:600 10px var(--mono);letter-spacing:.08em;text-transform:uppercase;color:var(--faint);text-align:left;border-bottom:1px solid var(--line);padding:11px 10px}
td{padding:11px 10px;border-bottom:1px solid var(--line);color:var(--muted)}
tr:hover td{background:var(--hover)}
.code{background:#080c10;border:1px solid var(--line);padding:20px;overflow:auto;color:#d5dde3;font:13px/1.65 var(--mono)}
```

---

## 8. Bars, tabs, pager, buttons

### Horizontal bar chart
Three columns: **label | track+fill | value**. Track is a flat dark strip; fill is solid accent and animates from the left. Fill width is set inline as a percentage.

In `repo-benchmarks/index.html` the bars are **JS-generated** from a data object. Each group is scaled to its own max (largest value = 100%, minimum 2% so tiny values stay visible), values print with `toFixed(3)` plus a unit suffix, and each group gets a mono kicker title (`COLD`, `WARM`…). Tabs rebuild the chart via `innerHTML`, so the grow animation replays on every tab click.

```html
<div class="bar-row"><span>GitNexus</span><div class="bar-track"><div class="bar-fill" style="width:63%"></div></div><span class="bar-value">64.955 s</span></div>
```
```js
const data={cold:{GitNexus:64.955,Graphify:10.026,CodeGraph:9.913}};   // one object per chart
function drawBars(target,source,selected="all",suffix=" s"){
  const el=document.querySelector(target);el.innerHTML="";
  (selected==="all"?Object.keys(source):[selected]).forEach(group=>{
    const vals=source[group],max=Math.max(...Object.values(vals));
    el.insertAdjacentHTML("beforeend",`<div class="section-kicker" style="margin:22px 0 8px">${group.toUpperCase()}</div>`);
    Object.keys(vals).forEach(k=>{
      const w=Math.max(2,vals[k]/max*100);
      el.insertAdjacentHTML("beforeend",`<div class="bar-row"><span>${k}</span><div class="bar-track"><div class="bar-fill" style="width:${w}%"></div></div><span class="bar-value">${vals[k].toFixed(3)}${suffix}</span></div>`);
    });
  });
}
// tabs: <button class="tab" data-phase="cold"> → toggle .active, then drawBars("#chart",data,btn.dataset.phase)
```
```css
.bar-row{display:grid;grid-template-columns:105px 1fr 90px;gap:15px;align-items:center;margin:16px 0;font:12px var(--mono)}
.bar-track{height:20px;background:var(--line)}  /* source hardcodes #1a232c; use --track (below) if you want to match exactly */
.bar-fill{height:100%;background:var(--accent);transform-origin:left}
.bar-value{text-align:right;color:var(--ink)}
.compact .bar-row{grid-template-columns:85px 1fr 85px}          /* narrower variant for side-by-side panels */
@keyframes grow{from{transform:scaleX(0)}to{transform:scaleX(1)}}
```
Colour convention: **primary/winner or outlier = `--accent`**, everything else = `--accent2`.

**Scroll-triggered, staggered, replaying (used on both pages).** Bars sit at zero until their chart scrolls into view, grow left to right with a per-row stagger, and replay every time the chart re-enters (scrolling up or down). Ported from `imdadareeph.github.io`.
```html
<script>document.documentElement.classList.add('js')</script>   <!-- in <head>: gate so no-JS visitors still see full bars -->
<div id="chart" class="bars reveal repeat">…rows, each with style="--i:0", "--i:1"…</div>
```
```css
.js .bar-fill,.js .track i{transform:scaleX(0)}
.in .bar-fill,.in .track i{animation:grow .9s ease both;animation-delay:calc(var(--i,0)*70ms)}
.js .reveal{opacity:0;transform:translateY(16px);transition:opacity .6s ease,transform .6s ease}
.js .reveal.in{opacity:1;transform:none}
```
```js
const again=new IntersectionObserver(es=>es.forEach(e=>e.target.classList.toggle('in',e.isIntersecting)),{threshold:.2});
document.querySelectorAll('.repeat').forEach(el=>again.observe(el));
```
For JS-drawn bars set the stagger index as you build rows: `row.style.setProperty('--i', n++)`. Redrawing the chart (tabs) keeps `.in` on the container, so the animation replays on each tab click.

### Static scale bars (no animation)
Used for size comparisons (`.repo-row`). Label | track | bold value, fill width inline, **first row accent, the rest `--accent2`**. Slightly taller gap than `.bar-row`, thinner track (18px), no `grow` animation.
```css
.repo-row{display:grid;grid-template-columns:100px 1fr 110px;gap:18px;align-items:center;margin:22px 0;font:12px var(--mono)}
.track{height:18px;background:var(--line)}
.track i{display:block;height:100%;background:var(--accent2)}
.repo-row:first-child .track i{background:var(--accent)}
```
```html
<div class="repo-row"><span>GitNexus</span><div class="track"><i style="width:100%"></i></div><b>3.501M LOC</b></div>
```

### Thin "run" bars (comparison bars)
Number on top, thin bar underneath. The highlighted one is thicker and accent-coloured.
```css
.run span{display:block;font:10px var(--mono);color:var(--muted);margin-bottom:10px}
.run b{font-size:27px;letter-spacing:-.04em}
.run i{display:block;height:4px;background:var(--accent2);margin-top:13px}
.run.outlier i{background:var(--accent);height:10px}
.run.outlier b{color:var(--accent)}
```

### Ratio stats
```css
.ratio-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}
.ratio{padding:18px 10px;border-left:1px solid var(--line)}
.ratio strong{display:block;font-size:26px;letter-spacing:-.04em}
.ratio small{font:10px var(--mono);color:var(--faint)}
```

### Tabs (filter buttons)
Square, outlined, mono. Active/hover = filled accent.
```css
.tabs{display:flex;gap:4px;flex-wrap:wrap}
.tab{border:1px solid var(--line);background:none;color:var(--muted);padding:7px 10px;font:10px var(--mono);cursor:pointer}
.tab.active,.tab:hover{color:var(--bg);background:var(--accent);border-color:var(--accent)}
```

### Pager
```css
.pager{display:flex;justify-content:center;gap:6px;margin-top:32px}
.pager button{min-width:38px;height:38px;border:1px solid var(--line);background:var(--surface);color:var(--muted);font:600 13px var(--mono);cursor:pointer}
.pager button:hover:not(:disabled){color:var(--ink);border-color:var(--accent-line)}
.pager button.on{background:var(--accent);color:var(--bg);border-color:var(--accent)}
.pager button:disabled{opacity:.35;cursor:default}
```

### Back-to-top button
Round, fixed bottom-right, hidden until scrolled 400px, fades and slides up.
```css
.to-top{position:fixed;right:24px;bottom:24px;width:48px;height:48px;border-radius:50%;border:1px solid var(--line);
  background:var(--surface2);color:var(--accent);font-size:20px;cursor:pointer;
  opacity:0;visibility:hidden;transform:translateY(8px);transition:.2s;z-index:10}
.to-top.show{opacity:1;visibility:visible;transform:none}
.to-top:hover{background:var(--accent);color:var(--bg)}
```

### Footer
```css
footer{border-top:1px solid var(--line);padding:28px;text-align:center;color:var(--faint);font:11px var(--mono)}
footer b{color:var(--accent);font-weight:600}
```

---

## 9. Motion & behaviour

**Animation budget is tiny on purpose.** A handful of UI motions, the ambient looping hero video (still for reduced-motion visitors), one scroll-camera effect and, on the home page only, one pinned signature scene:

| Motion | Where | Spec |
|---|---|---|
| Bar grow | `.bar-fill` | `scaleX(0→1)` from left, `.9s ease`, 70ms stagger per row, replays on every scroll into view (§8) |
| Card lift | `.card:hover` | `translateY(-3px)` + border/background shift, `.2s` |
| Back-to-top | `.to-top` | fade + `translateY(8px→0)`, `.2s` |
| Smooth scroll | `html` | `scroll-behavior:smooth` |

Always include the reduced-motion guard:
```css
@media(prefers-reduced-motion:reduce){
  html{scroll-behavior:auto}
  *,*::before,*::after{animation-duration:.01ms!important;animation-iteration-count:1!important;transition-duration:.01ms!important}
}
```

**JS (theme toggle + back-to-top):**
```js
const tb=document.getElementById('theme-btn'),root=document.documentElement;
const paint=()=>{const l=root.dataset.theme==='light';tb.textContent=l?'☾':'☀';tb.title=l?'Switch to dark':'Switch to light'};
tb.onclick=()=>{const l=root.dataset.theme!=='light';root.dataset.theme=l?'light':'dark';
  try{localStorage.setItem('theme',l?'light':'dark')}catch(e){}paint()};
paint();
const t=document.getElementById('to-top');
addEventListener('scroll',()=>t.classList.toggle('show',scrollY>400),{passive:true});
t.onclick=()=>scrollTo({top:0,behavior:'smooth'});
```
The tiny `<script>` in `<head>` (see §1) restores the saved theme before first paint. Keep it there.

### Scroll camera: zoom, pan, dolly (Tier 1, CSS only)

Camera moves on a flat page: **zoom = scale**, **pan = translate**, **dolly = foreground moves faster than background** (parallax depth). Scroll-driven CSS animations tie them to scroll position with no JS. Works in Chromium/Edge; other browsers just see the static page (progressive enhancement). Use the individual `scale` / `translate` properties: they compose with any existing `transform`, so a `translateX(-50%)` centring trick is not overwritten.
```css
@supports (animation-timeline:scroll()){
  .hero{clip-path:inset(0 -100vw)}        /* video scales past the hero box: clip vertically, keep full-bleed width */
  .hero-video{animation:cam linear both;animation-timeline:scroll(root);animation-range:0px 80vh}
  .hero>:not(.hero-video){animation:fore linear both;animation-timeline:scroll(root);animation-range:0px 60vh}
  .card{animation:zin linear both;animation-timeline:view();animation-range:entry 0% cover 28%}   /* panels scale in on entry */
  @keyframes cam{to{scale:1.22;translate:-2% -5%}}        /* zoom + pan */
  @keyframes fore{to{translate:0 -60px;opacity:.15}}      /* dolly: text leaves faster than the video */
  @keyframes zin{from{scale:.96;opacity:.5}}
}
@media(prefers-reduced-motion:reduce){.hero-video,.hero>*,.card{animation:none!important}}
```
Notes: put `animation-timeline` **after** the `animation` shorthand (the shorthand resets it). The `.01ms` reduced-motion guard above is not enough for scroll-driven animations, so set `animation:none` explicitly as shown. Don't scale the video past about 1.25 (720p source softens).

### Pinned signature scene (Tier 2, GSAP + ScrollTrigger)

Use **once per site**, on the landing page. The hero pins, the camera dollies into the artwork (zoom + pan toward a focal point), the headline flies through and fades, and a caption resolves before the page releases. Everything is one scrubbed timeline, so scrolling back plays it in reverse.
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.13.0/dist/gsap.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.13.0/dist/ScrollTrigger.min.js" defer></script>
<!-- inside .hero, after the meta row: -->
<div class="hero-cap" aria-hidden="true"><b>THE ATLAS</b><span>Every folder, a place on the map.</span></div>
```
```css
.hero-video{left:calc(50% - 50vw)}      /* no transform: GSAP owns transform in the scene */
.hero-cap{display:none;position:absolute;left:0;bottom:16%;max-width:640px;pointer-events:none}
.gsap-scene .hero-cap{display:block}
.gsap-scene .hero{min-height:calc(100vh - 56px)}
/* Tier 1 hero rules become the fallback: */
html:not(.gsap-scene) .hero-video{animation:cam …}   /* and the same prefix on the .hero>:not(.hero-video) rule */
```
```js
addEventListener('DOMContentLoaded',()=>{
  if(!window.gsap||!window.ScrollTrigger)return;             // CDN failed → Tier 1 fallback
  gsap.registerPlugin(ScrollTrigger);
  gsap.matchMedia().add('(min-width:761px) and (prefers-reduced-motion:no-preference)',()=>{
    const hero=document.querySelector('.hero'),vid=hero.querySelector('.hero-video'),cap=hero.querySelector('.hero-cap'),
          text=gsap.utils.toArray('.hero > :not(.hero-video):not(.hero-cap)');
    document.documentElement.classList.add('gsap-scene');
    gsap.set(vid,{transformOrigin:'72% 42%'});                // focal point of the artwork
    gsap.timeline({defaults:{ease:'none'},scrollTrigger:{trigger:hero,start:'top 56px',end:()=>'+='+innerHeight*1.2,pin:true,scrub:.6,anticipatePin:1,invalidateOnRefresh:true}})
      .to(vid,{scale:1.75,xPercent:-7,yPercent:3,duration:1},0)                                   // zoom + pan into the focal point
      .to(text,{scale:1.25,y:-50,opacity:0,transformOrigin:'0% 50%',stagger:.04,duration:.5},0)   // dolly-through
      .fromTo(cap,{opacity:0,y:30},{opacity:1,y:0,duration:.35},.55);                             // arrival
    return()=>document.documentElement.classList.remove('gsap-scene');
  });
});
```
Rules: desktop and motion-OK only (`matchMedia`); avoid long pins on mobile; `start:'top 56px'` = below the 56px navbar; tune `transformOrigin` to wherever the subject sits in your image; the caption is decorative (`aria-hidden`) because it repeats page content. If a test browser has `scroll-behavior:smooth`, set it to `auto` when scripting scroll positions or readings will be off.

### Motion decision ladder

1. **CSS** (transitions, `view()` / `scroll()` timelines) for anything ordinary.
2. **GSAP + ScrollTrigger** only for a pinned or multi-step scrubbed scene, and only one per site.
3. **Three.js** only for genuine 3D (real dolly = `camera.position.z`, pan = `position.x/y`, zoom = `camera.fov` then `updateProjectionMatrix()`).

Tiers: micro 100 to 300ms (hover, buttons), UI 300 to 900ms (cards, bars, reveals), cinematic = scroll-controlled. Not every element should be cinematic.

---

## 10. Hero media (video and image placeholders)

The hero media is **placeholder**. Swap the files any time without touching CSS or HTML: keep the same filenames.

| File | Used by | Recommended | Notes |
|---|---|---|---|
| `videos/dark-loop.mp4` | hero video, **dark** theme | 1280×720+, 8 to 10s, under 1 MB | Seamless loop, no audio. Keep the left ~40% calm for text |
| `videos/lite-loop.mp4` | hero video, **light** theme | same | Pale variant of the same footage |
| `img/bg.webp` | video **poster** (dark) and image-only fallback | 1920×900+ | Dark, low-contrast still |
| `img/bg-lite.webp` | video **poster** (light) and image-only fallback | 1920×900+ | Light/pale still |
| `img/project-atlas.webp` | README / social preview | 1200×630 | Logo or banner |

### Preparing a looping video (ffmpeg)

Generated or stock clips rarely loop cleanly. Crossfade the last `X` seconds into the first `X` so the end joins the start with no jump, strip audio, and compress. Example for a 10s clip and `X=1.5` (output is 8.5s):

```bash
X=1.5; D=10   # X = crossfade seconds, D = clip duration
ffmpeg -y -i in.mp4 -an -filter_complex "
[0:v]split=3[a][m][t];
[a]trim=0:$X,setpts=PTS-STARTPTS[A];
[m]trim=$X:$(echo "$D-$X"|bc),setpts=PTS-STARTPTS[M];
[t]trim=$(echo "$D-$X"|bc):$D,setpts=PTS-STARTPTS[T];
[T][A]xfade=transition=fade:duration=$X:offset=0[X];
[X][M]concat=n=2:v=1[out]" \
 -map "[out]" -c:v libx264 -crf 26 -preset slow -pix_fmt yuv420p -movflags +faststart out-loop.mp4
```
- **Watermark** (e.g. a corner logo): add `delogo=x=…:y=…:w=…:h=…,` in front of `split=3` in the filter chain.
- **Check the seam:** compare the first and last frame with `ffmpeg -i first.png -i last.png -lavfi ssim -f null -`. About 0.95 or higher means no visible pop. The unprocessed dark clip scored 0.85 and popped visibly.
- Result is typically 0.4 to 0.7 MB for 8s at 720p.

### Placeholder options until real media arrives
- Leave the files as they are (current ones are placeholders).
- **No video:** skip the `<video>` and use the image-only variant from §6, or a pure-CSS stand-in via the `--hero-img` token:
  ```css
  --hero-img:linear-gradient(135deg,#17212a,#0c1015);            /* dark */
  :root[data-theme=light]{--hero-img:linear-gradient(135deg,#e9e2d3,#f6f4ef)}
  ```
- Or a placeholder service URL: `--hero-img:url(https://placehold.co/1920x900/0c1015/17212a)`

### When supplying real media
1. Video: MP4 (H.264), 720p is enough (the scene sits softly behind a scrim), **no audio track**, seamless loop, under about 1 MB each.
2. Stills (poster and fallback): **WebP**, quality about 80, under about 200 KB.
3. Same filenames means drop-in. Different names: edit the paths in `heroSrc()` (and the `--hero-img` tokens if you use the image-only variant).
4. Provide **both** a dark and a light version so contrast holds in each theme, or use one clip and lean on `--hero-shade` for legibility.
5. Sub-folder pages reference `../videos/…` and `../img/…`. Keep single shared `videos/` and `img/` folders at repo root.
6. Media is decorative: `aria-hidden="true"` on the video, no `alt` for CSS backgrounds. If you add an `<img>`, give it real `alt` text.
7. Only the active theme's video is requested, so the other costs nothing until the visitor toggles.

---

## 11. Responsive rules

```css
@media(max-width:900px){.grid{grid-template-columns:repeat(2,1fr)}}
@media(max-width:760px){                    /* report/chart pages */
  main{padding:0 18px 80px}
  .grid-2,.architecture,.outlier-panel,.boundary{grid-template-columns:1fr}
  .bar-row{grid-template-columns:82px 1fr 72px}
  .ratio-grid{grid-template-columns:1fr 1fr}
}
@media(max-width:600px){
  .grid{grid-template-columns:1fr}
  .nav{padding:0 14px}
  main{padding:0 16px 80px}
  .nav-brand{display:none}
  .nav-tag{font-size:10px}
}
```
Headings use `clamp()` so they scale without breakpoints. Tables sit in `.table-wrap{overflow-x:auto}`. Nav links scroll sideways.

---

## 12. Known gaps in the source pages

Verified against the actual files. Handle these when reusing.

- **`repo-benchmarks/index.html` is dark-only.** No `data-theme`, no toggle, no `--hero-img`/`--nav-bg` tokens. It hardcodes hex (`#1a232c` bar track, `#202932` table rules, `#c3ccd3` paragraphs, `#77604b` accent border, `rgba(17,24,32,.72)` panel fill, `url(../img/bg.webp)` hero). Everything in this guide is written with tokens, so a page built from it supports both themes; the source page does not yet.
- **Panels use translucent dark fills** (`rgba(17,24,32,.72)`, `rgba(255,255,255,.025)`). On the light theme use `var(--surface)` and `var(--hover)` instead, or they will look muddy.
- **Hero differences:** the report page's hero is taller (`min-height:720px`), has a `border-bottom`, and its `<h1>` spans are pale grey (`#d8e1e7`), not accent. The home page uses accent on one word.
- **Nav differences:** report page hides `.nav-tag` under 760px and shows its brand as a link back home; home page hides `.nav-brand` under 600px.
- **`.hero-flow`** (node → node → phase chips row) exists only on the report page; it wraps and stacks vertically on mobile with arrows rotated 90°.

Optional token to unify the bar track and scale track across themes:
```css
:root{--track:#1a232c}
:root[data-theme=light]{--track:#e7e2d8}
```

---

## 13. Checklist for a new page

- [ ] Paste tokens (§2), base (§4), and the components you need
- [ ] Theme-restore `<script>` in `<head>`, toggle JS at end of body
- [ ] Sticky nav with brand, links, theme button
- [ ] Hero: mono eyebrow → big H1 with one accent word → muted dek → mono meta row
- [ ] Sections: `kicker` (`01 · NAME`) → `h2` → content
- [ ] Only tokens for colour, no raw hex in new components
- [ ] Hero media paths correct for folder depth (`img/`, `videos/` vs `../img/`, `../videos/`); videos muted, no audio, seamless loop
- [ ] Reduced-motion block present (also `animation:none` for any scroll-driven / GSAP effect)
- [ ] Scroll effects verified scrolling both down and up; static fallback works with JS off and at phone width
- [ ] Test both themes and a 375px-wide viewport
