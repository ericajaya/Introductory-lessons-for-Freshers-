<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kotlin Coroutines — A Beginner's Guide</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Sora:wght@300;400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #1e2530;
    --border: #30363d;
    --kotlin: #7c52ff;
    --kotlin-light: #a98aff;
    --teal: #39d0c9;
    --amber: #f0a500;
    --rose: #f05070;
    --green: #3dd68c;
    --text: #e6edf3;
    --muted: #8b949e;
    --code-bg: #0d1117;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Sora', sans-serif;
    font-size: 16px;
    line-height: 1.75;
    min-height: 100vh;
  }

  /* ─── HEADER ─── */
  header {
    background: linear-gradient(135deg, #1a0a3d 0%, #0d1117 60%);
    border-bottom: 1px solid var(--kotlin);
    padding: 60px 40px 50px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse 70% 60% at 50% 0%, rgba(124,82,255,0.18) 0%, transparent 70%);
    pointer-events: none;
  }
  .header-badge {
    display: inline-block;
    background: rgba(124,82,255,0.15);
    border: 1px solid var(--kotlin);
    color: var(--kotlin-light);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 5px 14px;
    border-radius: 999px;
    margin-bottom: 22px;
  }
  header h1 {
    font-size: clamp(2.2rem, 5vw, 3.6rem);
    font-weight: 800;
    letter-spacing: -0.02em;
    line-height: 1.1;
    background: linear-gradient(90deg, #fff 30%, var(--kotlin-light));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 16px;
  }
  header p {
    color: var(--muted);
    font-size: 1.05rem;
    max-width: 560px;
    margin: 0 auto;
  }

  /* ─── PROGRESS BAR ─── */
  .progress-nav {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 14px 40px;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .progress-nav a {
    color: var(--muted);
    text-decoration: none;
    font-size: 0.78rem;
    font-family: 'JetBrains Mono', monospace;
    padding: 4px 12px;
    border-radius: 4px;
    border: 1px solid transparent;
    transition: all .2s;
  }
  .progress-nav a:hover {
    color: var(--kotlin-light);
    border-color: var(--kotlin);
    background: rgba(124,82,255,0.1);
  }

  /* ─── LAYOUT ─── */
  main {
    max-width: 900px;
    margin: 0 auto;
    padding: 60px 28px 100px;
  }

  /* ─── SECTION ─── */
  section {
    margin-bottom: 80px;
    animation: fadeUp .5s ease both;
  }
  @keyframes fadeUp {
    from { opacity:0; transform:translateY(20px); }
    to   { opacity:1; transform:translateY(0); }
  }

  .section-number {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    color: var(--kotlin);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  h2 {
    font-size: 1.9rem;
    font-weight: 700;
    letter-spacing: -0.02em;
    margin-bottom: 20px;
    color: #fff;
  }
  h3 {
    font-size: 1.15rem;
    font-weight: 600;
    color: var(--teal);
    margin: 32px 0 10px;
  }
  p { color: #c9d1d9; margin-bottom: 14px; }

  /* ─── CALLOUT ─── */
  .callout {
    border-left: 3px solid var(--kotlin);
    background: rgba(124,82,255,0.07);
    border-radius: 0 8px 8px 0;
    padding: 16px 20px;
    margin: 24px 0;
    font-size: 0.95rem;
    color: var(--text);
  }
  .callout.tip { border-color: var(--teal); background: rgba(57,208,201,0.07); }
  .callout.warn { border-color: var(--amber); background: rgba(240,165,0,0.07); }
  .callout strong { color: var(--kotlin-light); }
  .callout.tip strong { color: var(--teal); }
  .callout.warn strong { color: var(--amber); }

  /* ─── CODE ─── */
  .code-block {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    margin: 20px 0;
    font-family: 'JetBrains Mono', monospace;
  }
  .code-header {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 8px 16px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.75rem;
    color: var(--muted);
  }
  .code-lang {
    color: var(--kotlin-light);
    font-weight: 600;
  }
  .code-dots { display: flex; gap: 6px; }
  .code-dots span {
    width: 10px; height: 10px; border-radius: 50%;
  }
  .code-dots .r { background: #f05070; }
  .code-dots .y { background: #f0a500; }
  .code-dots .g { background: #3dd68c; }
  pre {
    padding: 22px 20px;
    overflow-x: auto;
    font-size: 0.87rem;
    line-height: 1.65;
  }
  /* syntax colours */
  .kw  { color: #c792ea; font-weight: 600; }
  .fn  { color: #82aaff; }
  .str { color: #c3e88d; }
  .cmt { color: #546e7a; font-style: italic; }
  .num { color: #f78c6c; }
  .cls { color: #ffcb6b; }
  .ann { color: #f07178; }

  /* ─── DIAGRAM ─── */
  .diagram-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 30px 20px;
    margin: 28px 0;
    overflow-x: auto;
  }
  .diagram-title {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    text-transform: uppercase;
    letter-spacing: .1em;
    color: var(--muted);
    margin-bottom: 20px;
    text-align: center;
  }
  svg text { font-family: 'JetBrains Mono', monospace; }

  /* ─── COMPARISON TABLE ─── */
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
    font-size: 0.9rem;
  }
  th {
    background: var(--surface2);
    color: var(--kotlin-light);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: .08em;
    padding: 12px 16px;
    text-align: left;
    border-bottom: 2px solid var(--kotlin);
  }
  td {
    padding: 11px 16px;
    border-bottom: 1px solid var(--border);
    color: #c9d1d9;
    vertical-align: top;
  }
  tr:hover td { background: rgba(124,82,255,0.04); }
  .tick { color: var(--green); }
  .cross { color: var(--rose); }

  /* ─── STEP LIST ─── */
  .steps { list-style: none; counter-reset: step; }
  .steps li {
    counter-increment: step;
    position: relative;
    padding: 18px 20px 18px 64px;
    margin-bottom: 14px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    transition: border-color .2s;
  }
  .steps li:hover { border-color: var(--kotlin); }
  .steps li::before {
    content: counter(step);
    position: absolute;
    left: 18px;
    top: 50%;
    transform: translateY(-50%);
    width: 32px; height: 32px;
    background: linear-gradient(135deg, var(--kotlin), #5030cc);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-family: 'JetBrains Mono', monospace;
    font-weight: 700;
    font-size: 0.85rem;
    color: #fff;
    line-height: 32px;
    text-align: center;
  }
  .steps li strong { color: #fff; display: block; margin-bottom: 4px; }

  /* ─── CONCEPT CARDS ─── */
  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
    margin: 24px 0;
  }
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px;
    transition: border-color .2s, transform .2s;
  }
  .card:hover { border-color: var(--kotlin); transform: translateY(-3px); }
  .card-icon { font-size: 1.6rem; margin-bottom: 10px; }
  .card h4 { color: #fff; font-size: 1rem; margin-bottom: 6px; }
  .card p  { font-size: 0.85rem; margin: 0; }

  /* ─── QUIZ ─── */
  .quiz {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px;
    margin: 28px 0;
  }
  .quiz h3 { color: var(--amber); margin: 0 0 16px; font-size: 1rem; }
  .quiz-q { margin-bottom: 18px; }
  .quiz-q p { font-weight: 600; color: #fff; margin-bottom: 8px; }
  .quiz-opt {
    display: block;
    padding: 9px 14px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 6px;
    margin: 6px 0;
    cursor: pointer;
    font-size: 0.88rem;
    transition: all .15s;
    user-select: none;
  }
  .quiz-opt:hover { border-color: var(--kotlin); color: var(--kotlin-light); }
  .quiz-opt.correct { border-color: var(--green); background: rgba(61,214,140,0.1); color: var(--green); }
  .quiz-opt.wrong   { border-color: var(--rose);  background: rgba(240,80,112,0.1);  color: var(--rose);  }
  .quiz-feedback { font-size: 0.83rem; margin-top: 6px; color: var(--muted); min-height: 1.2em; }

  /* ─── FOOTER ─── */
  footer {
    text-align: center;
    padding: 40px;
    border-top: 1px solid var(--border);
    color: var(--muted);
    font-size: 0.85rem;
  }

  /* ─── INLINE CODE ─── */
  code {
    background: rgba(124,82,255,0.15);
    color: var(--kotlin-light);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.83em;
    padding: 2px 6px;
    border-radius: 4px;
  }

  /* ─── RESPONSIVE ─── */
  @media (max-width: 600px) {
    header { padding: 40px 20px 36px; }
    main { padding: 40px 16px 80px; }
    h2 { font-size: 1.5rem; }
  }
</style>
</head>
<body>

<header>
  <div class="header-badge">First-Year Degree Level · Kotlin</div>
  <h1>Kotlin Coroutines</h1>
  <p>A hands-on beginner's guide to writing asynchronous code the Kotlin way — without the headaches.</p>
</header>

<nav class="progress-nav">
  <a href="#s1">1 · What are Coroutines?</a>
  <a href="#s2">2 · Setup</a>
  <a href="#s3">3 · Your First Coroutine</a>
  <a href="#s4">4 · Suspend Functions</a>
  <a href="#s5">5 · Builders</a>
  <a href="#s6">6 · Scope &amp; Lifecycle</a>
  <a href="#s7">7 · Dispatchers</a>
  <a href="#s8">8 · Structured Concurrency</a>
  <a href="#s9">9 · Error Handling</a>
  <a href="#s10">10 · Knowledge Check</a>
</nav>

<main>

<!-- ══════════════════════════════════════════════════════
     SECTION 1 — WHAT ARE COROUTINES?
══════════════════════════════════════════════════════ -->
<section id="s1">
  <div class="section-number">01 / Concept</div>
  <h2>What are Coroutines?</h2>

  <p>When a program needs to do something that takes time — like downloading a file, reading from a database, or waiting for user input — the simplest approach is to <strong>block</strong>: sit and do nothing until it's done. This wastes CPU resources and freezes UIs.</p>

  <p>A <strong>coroutine</strong> is a piece of code that can <em>pause itself</em> (suspend) and let other work happen in the meantime, then resume exactly where it left off — all without blocking the thread it's running on.</p>

  <div class="callout">
    <strong>Key idea:</strong> Coroutines give you the simplicity of writing sequential code while secretly doing things concurrently behind the scenes.
  </div>

  <!-- DIAGRAM 1: Blocking vs Coroutine -->
  <div class="diagram-wrap">
    <div class="diagram-title">Diagram 1 — Blocking Thread vs. Coroutine</div>
    <svg viewBox="0 0 780 220" width="100%" fill="none" xmlns="http://www.w3.org/2000/svg">
      <!-- labels -->
      <text x="10" y="30" fill="#8b949e" font-size="11" font-weight="600">BLOCKING</text>
      <text x="10" y="130" fill="#8b949e" font-size="11" font-weight="600">COROUTINE</text>

      <!-- Blocking row -->
      <rect x="10" y="40" width="120" height="28" rx="5" fill="#7c52ff" opacity=".9"/>
      <text x="70" y="58" fill="white" font-size="10" text-anchor="middle">Task A work</text>

      <rect x="130" y="40" width="140" height="28" rx="5" fill="#30363d"/>
      <text x="200" y="58" fill="#546e7a" font-size="10" text-anchor="middle">⏸ waiting (blocked)</text>

      <rect x="270" y="40" width="80" height="28" rx="5" fill="#7c52ff" opacity=".9"/>
      <text x="310" y="58" fill="white" font-size="10" text-anchor="middle">Task A done</text>

      <rect x="350" y="40" width="120" height="28" rx="5" fill="#f05070" opacity=".8"/>
      <text x="410" y="58" fill="white" font-size="10" text-anchor="middle">Task B work</text>

      <rect x="470" y="40" width="140" height="28" rx="5" fill="#30363d"/>
      <text x="540" y="58" fill="#546e7a" font-size="10" text-anchor="middle">⏸ waiting (blocked)</text>

      <rect x="610" y="40" width="80" height="28" rx="5" fill="#f05070" opacity=".8"/>
      <text x="650" y="58" fill="white" font-size="10" text-anchor="middle">Task B done</text>

      <!-- timeline arrow -->
      <line x1="10" y1="80" x2="700" y2="80" stroke="#30363d" stroke-width="1" stroke-dasharray="4"/>
      <text x="710" y="84" fill="#30363d" font-size="10">▶ time</text>

      <!-- Coroutine row -->
      <rect x="10" y="140" width="120" height="28" rx="5" fill="#7c52ff" opacity=".9"/>
      <text x="70" y="158" fill="white" font-size="10" text-anchor="middle">Coroutine A</text>

      <!-- A suspended -->
      <rect x="130" y="140" width="2" height="28" rx="1" fill="#7c52ff" opacity=".5"/>
      <line x1="131" y1="154" x2="269" y2="154" stroke="#7c52ff" stroke-width="1.5" stroke-dasharray="5,4" opacity=".4"/>

      <!-- B runs during A's suspension -->
      <rect x="133" y="140" width="135" height="28" rx="5" fill="#f05070" opacity=".8"/>
      <text x="200" y="158" fill="white" font-size="10" text-anchor="middle">Coroutine B runs</text>

      <!-- A resumes -->
      <rect x="269" y="140" width="80" height="28" rx="5" fill="#7c52ff" opacity=".9"/>
      <text x="309" y="158" fill="white" font-size="10" text-anchor="middle">A resumes</text>

      <!-- B continues -->
      <rect x="350" y="140" width="100" height="28" rx="5" fill="#f05070" opacity=".8"/>
      <text x="400" y="158" fill="white" font-size="10" text-anchor="middle">B continues</text>

      <!-- finish -->
      <rect x="451" y="140" width="70" height="28" rx="5" fill="#3dd68c" opacity=".85"/>
      <text x="486" y="158" fill="#0d1117" font-size="10" text-anchor="middle" font-weight="600">A done ✓</text>
      <rect x="522" y="140" width="70" height="28" rx="5" fill="#3dd68c" opacity=".85"/>
      <text x="557" y="158" fill="#0d1117" font-size="10" text-anchor="middle" font-weight="600">B done ✓</text>

      <!-- legend -->
      <rect x="10" y="200" width="12" height="10" rx="2" fill="#7c52ff"/>
      <text x="26" y="210" fill="#8b949e" font-size="10">Coroutine A</text>
      <rect x="110" y="200" width="12" height="10" rx="2" fill="#f05070"/>
      <text x="126" y="210" fill="#8b949e" font-size="10">Coroutine B</text>
      <rect x="210" y="200" width="12" height="10" rx="2" fill="#3dd68c"/>
      <text x="226" y="210" fill="#8b949e" font-size="10">Completed</text>
      <rect x="310" y="200" width="12" height="10" rx="2" fill="#30363d"/>
      <text x="326" y="210" fill="#8b949e" font-size="10">Blocked / idle</text>
    </svg>
  </div>

  <p>Notice how with coroutines, while Coroutine A is waiting (suspended), the thread is free to run Coroutine B. The total elapsed time is <em>much shorter</em>.</p>

  <div class="card-grid">
    <div class="card">
      <div class="card-icon">🪶</div>
      <h4>Lightweight</h4>
      <p>You can run thousands of coroutines with very little memory — unlike OS threads.</p>
    </div>
    <div class="card">
      <div class="card-icon">📖</div>
      <h4>Sequential-looking</h4>
      <p>Code reads top-to-bottom like normal, even when it pauses and resumes.</p>
    </div>
    <div class="card">
      <div class="card-icon">🛡️</div>
      <h4>Structured</h4>
      <p>Coroutines have scopes. If a parent is cancelled, children are too — no leaks.</p>
    </div>
    <div class="card">
      <div class="card-icon">🔀</div>
      <h4>Cancellable</h4>
      <p>Any coroutine can be cancelled cleanly at any suspension point.</p>
    </div>
  </div>
</section>

<!-- ══════════════════════════════════════════════════════
     SECTION 2 — SETUP
══════════════════════════════════════════════════════ -->
<section id="s2">
  <div class="section-number">02 / Setup</div>
  <h2>Project Setup</h2>

  <p>Coroutines are not built into the standard library — you add them as a dependency. Here's how for both Gradle project types:</p>

  <h3>Gradle (Kotlin DSL — <code>build.gradle.kts</code>)</h3>
  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="r"></span><span class="y"></span><span class="g"></span></div>
      <span class="code-lang">Kotlin</span>
      <span>build.gradle.kts</span>
    </div>
<pre><span class="cmt">// Add to your dependencies block</span>
<span class="fn">dependencies</span> {
    <span class="fn">implementation</span>(<span class="str">"org.jetbrains.kotlinx:kotlinx-coroutines-core:1.8.0"</span>)

    <span class="cmt">// For Android projects also add:</span>
    <span class="cmt">// implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.8.0")</span>
}</pre>
  </div>

  <h3>Gradle (Groovy DSL — <code>build.gradle</code>)</h3>
  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="r"></span><span class="y"></span><span class="g"></span></div>
      <span class="code-lang">Groovy</span>
      <span>build.gradle</span>
    </div>
<pre><span class="fn">dependencies</span> {
    implementation <span class="str">'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.8.0'</span>
}</pre>
  </div>

  <div class="callout tip">
    <strong>Tip:</strong> If you're following along with a plain Kotlin project (e.g. IntelliJ IDEA), choose <em>New Project → Kotlin → Gradle → Kotlin DSL</em> and add the dependency above.
  </div>
</section>

<!-- ══════════════════════════════════════════════════════
     SECTION 3 — YOUR FIRST COROUTINE
══════════════════════════════════════════════════════ -->
<section id="s3">
  <div class="section-number">03 / Hello World</div>
  <h2>Your First Coroutine</h2>

  <p>Let's write the simplest possible coroutine and understand every part of it.</p>

  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="r"></span><span class="y"></span><span class="g"></span></div>
      <span class="code-lang">Kotlin</span>
      <span>Main.kt</span>
    </div>
<pre><span class="kw">import</span> kotlinx.coroutines.*

<span class="kw">fun</span> <span class="fn">main</span>() = <span class="fn">runBlocking</span> {   <span class="cmt">// ① starts a coroutine scope, blocks the thread until done</span>
    <span class="fn">launch</span> {                  <span class="cmt">// ② launches a new coroutine (fire-and-forget)</span>
        <span class="fn">delay</span>(<span class="num">1000L</span>)           <span class="cmt">// ③ suspends (not blocks) for 1 second</span>
        println(<span class="str">"World!"</span>)      <span class="cmt">// ④ resumes and prints</span>
    }
    println(<span class="str">"Hello,"</span>)         <span class="cmt">// ⑤ prints immediately</span>
}</pre>
  </div>

  <p><strong>Output:</strong></p>
  <div class="code-block">
    <div class="code-header">
      <div class="code-dots"><span class="r"></span><span class="y"></span><span class="g"></span></div>
      <span class="code-lang">Output</span>
    </div>
<pre>Hello,
World!</pre>
  </div>

  <!-- DIAGRAM 2: execution timeline -->
  <div class="diagram-wrap">
    <div class="diagram-title">Diagram 2 — Execution Timeline of the Hello World Example</div>
    <svg viewBox="0 0 760 160" width="100%" xmlns="http://www.w3.org/2000/svg">
      <!-- time axis -->
      <line x1="10" y1="140" x2="750" y2="140" stroke="#30363d" stroke-width="1.5"/>
      <text x="755" y="144" fill="#30363d" font-size="10">▶ time</text>
      <!-- ticks -->
      <l# Introductory-lessons-for-Freshers-