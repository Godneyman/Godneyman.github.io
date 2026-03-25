<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI Agent 智能体教程（CQU刘学长版）</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700;900&family=JetBrains+Mono:wght@400;600&family=Noto+Sans+SC:wght@300;400;500&display=swap" rel="stylesheet" />
  <style>
    :root {
      --ink: #0d0d0d;
      --paper: #f5f0e8;
      --accent: #c0392b;
      --accent2: #1a237e;
      --gold: #b8860b;
      --muted: #6b6b6b;
      --border: #2a2a2a;
      --code-bg: #1a1a2e;
      --code-text: #e0e0ff;
      --highlight: #fff3cd;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background-color: var(--paper);
      color: var(--ink);
      font-family: 'Noto Sans SC', sans-serif;
      font-weight: 300;
      line-height: 1.8;
      overflow-x: hidden;
    }

    /* ── NOISE TEXTURE OVERLAY ── */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 9999;
      opacity: 0.4;
    }

    /* ── HEADER ── */
    header {
      border-bottom: 3px solid var(--border);
      padding: 0;
      position: sticky;
      top: 0;
      background: var(--paper);
      z-index: 100;
      display: flex;
      align-items: stretch;
    }

    .header-tag {
      background: var(--accent);
      color: #fff;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      letter-spacing: 0.12em;
      padding: 0 1.5rem;
      display: flex;
      align-items: center;
      writing-mode: horizontal-tb;
    }

    .header-title {
      padding: 1rem 2rem;
      font-family: 'Noto Serif SC', serif;
      font-weight: 900;
      font-size: 1.1rem;
      letter-spacing: 0.04em;
      flex: 1;
    }

    .header-nav {
      display: flex;
      align-items: center;
      gap: 0;
      border-left: 2px solid var(--border);
    }

    .header-nav a {
      display: flex;
      align-items: center;
      padding: 0 1.4rem;
      height: 100%;
      font-size: 0.8rem;
      color: var(--ink);
      text-decoration: none;
      border-right: 1px solid #ddd;
      transition: background 0.15s;
      letter-spacing: 0.06em;
    }

    .header-nav a:hover { background: var(--ink); color: var(--paper); }

    /* ── HERO ── */
    .hero {
      display: grid;
      grid-template-columns: 1fr 1fr;
      min-height: 72vh;
      border-bottom: 3px solid var(--border);
    }

    .hero-left {
      padding: 5rem 4rem;
      border-right: 3px solid var(--border);
      display: flex;
      flex-direction: column;
      justify-content: center;
      gap: 2rem;
      animation: fadeInLeft 0.8s ease both;
    }

    @keyframes fadeInLeft {
      from { opacity: 0; transform: translateX(-30px); }
      to { opacity: 1; transform: translateX(0); }
    }

    .hero-label {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.2em;
      color: var(--muted);
      text-transform: uppercase;
    }

    .hero-heading {
      font-family: 'Noto Serif SC', serif;
      font-weight: 900;
      font-size: clamp(2.8rem, 6vw, 5rem);
      line-height: 1.1;
      letter-spacing: -0.01em;
    }

    .hero-heading .accent { color: var(--accent); }

    .hero-sub {
      font-size: 1.05rem;
      color: var(--muted);
      max-width: 38ch;
      line-height: 1.7;
    }

    .hero-cta {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
    }

    .btn {
      display: inline-block;
      padding: 0.75rem 1.8rem;
      font-size: 0.85rem;
      letter-spacing: 0.08em;
      text-decoration: none;
      border: 2px solid var(--border);
      font-family: 'JetBrains Mono', monospace;
      transition: all 0.2s;
      cursor: pointer;
    }

    .btn-primary { background: var(--ink); color: var(--paper); }
    .btn-primary:hover { background: var(--accent); border-color: var(--accent); }
    .btn-secondary { background: transparent; color: var(--ink); }
    .btn-secondary:hover { background: var(--ink); color: var(--paper); }

    .hero-right {
      background: var(--ink);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 3rem;
      animation: fadeInRight 0.8s 0.2s ease both;
      position: relative;
      overflow: hidden;
    }

    @keyframes fadeInRight {
      from { opacity: 0; transform: translateX(30px); }
      to { opacity: 1; transform: translateX(0); }
    }

    .hero-right::before {
      content: 'AGENT';
      position: absolute;
      font-family: 'Noto Serif SC', serif;
      font-weight: 900;
      font-size: 12rem;
      color: rgba(255,255,255,0.03);
      letter-spacing: -0.05em;
      user-select: none;
    }

    .diagram {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.6rem;
      position: relative;
      z-index: 1;
      width: 100%;
      max-width: 340px;
    }

    .diag-node {
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.15);
      color: #fff;
      padding: 0.9rem 2rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      width: 100%;
      text-align: center;
      position: relative;
      transition: border-color 0.3s;
    }

    .diag-node:hover { border-color: var(--accent); }

    .diag-node.core {
      background: var(--accent);
      border-color: var(--accent);
      font-size: 0.95rem;
      font-weight: 600;
    }

    .diag-arrow {
      color: rgba(255,255,255,0.3);
      font-size: 1.3rem;
      line-height: 1;
    }

    .diag-row {
      display: flex;
      gap: 0.5rem;
      width: 100%;
    }

    .diag-row .diag-node { font-size: 0.7rem; padding: 0.7rem 0.5rem; }

    /* ── LAYOUT ── */
    .container {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 2rem;
    }

    /* ── SECTION HEADERS ── */
    .section-header {
      display: flex;
      align-items: baseline;
      gap: 1.5rem;
      margin-bottom: 3rem;
      padding-bottom: 1.2rem;
      border-bottom: 2px solid var(--border);
    }

    .section-num {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      color: var(--accent);
      letter-spacing: 0.15em;
    }

    .section-title {
      font-family: 'Noto Serif SC', serif;
      font-weight: 700;
      font-size: 1.9rem;
      letter-spacing: 0.02em;
    }

    /* ── WHAT IS AGENT ── */
    .section-what { padding: 6rem 0; }

    .two-col {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 4rem;
      align-items: start;
    }

    .prose p {
      margin-bottom: 1.2rem;
      font-size: 0.97rem;
      line-height: 1.9;
      color: #2a2a2a;
    }

    .pullquote {
      border-left: 4px solid var(--accent);
      padding: 1.2rem 1.8rem;
      background: rgba(192,57,43,0.04);
      font-family: 'Noto Serif SC', serif;
      font-size: 1.05rem;
      font-weight: 700;
      color: var(--accent);
      line-height: 1.6;
      margin: 2rem 0;
    }

    /* ── FEATURES GRID ── */
    .section-features { padding: 6rem 0; border-top: 3px solid var(--border); }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      border: 2px solid var(--border);
    }

    .feature-card {
      padding: 2.5rem 2rem;
      border-right: 2px solid var(--border);
      transition: background 0.2s;
      animation: fadeUp 0.5s ease both;
    }

    .feature-card:last-child { border-right: none; }
    .feature-card:hover { background: var(--ink); color: var(--paper); }
    .feature-card:hover .feature-icon { color: var(--accent); }
    .feature-card:hover .feature-desc { color: #ccc; }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .feature-icon {
      font-size: 2rem;
      margin-bottom: 1.2rem;
      color: var(--accent);
      display: block;
      transition: color 0.2s;
    }

    .feature-name {
      font-family: 'Noto Serif SC', serif;
      font-weight: 700;
      font-size: 1rem;
      margin-bottom: 0.8rem;
    }

    .feature-desc {
      font-size: 0.83rem;
      line-height: 1.7;
      color: var(--muted);
      transition: color 0.2s;
    }

    /* ── TYPES ── */
    .section-types { padding: 6rem 0; border-top: 3px solid var(--border); }

    .types-list { display: flex; flex-direction: column; gap: 1.5rem; }

    .type-item {
      display: grid;
      grid-template-columns: auto 1fr;
      gap: 2rem;
      border: 2px solid var(--border);
      padding: 2rem 2.5rem;
      align-items: start;
      transition: all 0.2s;
      cursor: default;
      animation: fadeUp 0.5s ease both;
    }

    .type-item:hover {
      border-color: var(--accent);
      transform: translateX(6px);
    }

    .type-num {
      font-family: 'JetBrains Mono', monospace;
      font-size: 2.5rem;
      font-weight: 600;
      color: #ddd;
      line-height: 1;
      min-width: 3rem;
    }

    .type-item:hover .type-num { color: var(--accent); }

    .type-name {
      font-family: 'Noto Serif SC', serif;
      font-weight: 700;
      font-size: 1.15rem;
      margin-bottom: 0.6rem;
    }

    .type-desc {
      font-size: 0.9rem;
      color: var(--muted);
      line-height: 1.7;
    }

    /* ── HOW IT WORKS ── */
    .section-how { padding: 6rem 0; border-top: 3px solid var(--border); }

    .steps {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 0;
      border: 2px solid var(--border);
      position: relative;
    }

    .step {
      padding: 2.5rem 1.5rem;
      border-right: 2px solid var(--border);
      text-align: center;
      position: relative;
      transition: background 0.2s;
    }

    .step:last-child { border-right: none; }
    .step:hover { background: var(--ink); color: var(--paper); }
    .step:hover .step-num { color: var(--accent); }
    .step:hover .step-label { color: #ccc; }

    .step-num {
      font-family: 'JetBrains Mono', monospace;
      font-size: 2.2rem;
      font-weight: 600;
      color: rgba(0,0,0,0.1);
      margin-bottom: 0.8rem;
      display: block;
    }

    .step-icon { font-size: 1.5rem; margin-bottom: 0.8rem; display: block; }

    .step-name {
      font-family: 'Noto Serif SC', serif;
      font-weight: 700;
      font-size: 0.95rem;
      margin-bottom: 0.6rem;
    }

    .step-label {
      font-size: 0.78rem;
      color: var(--muted);
      line-height: 1.6;
    }

    /* ── CODE SECTION ── */
    .section-code { padding: 6rem 0; border-top: 3px solid var(--border); }

    .code-block-wrap {
      background: var(--code-bg);
      border: 2px solid var(--border);
      overflow: hidden;
    }

    .code-header {
      display: flex;
      align-items: center;
      gap: 1rem;
      padding: 0.9rem 1.5rem;
      border-bottom: 1px solid rgba(255,255,255,0.08);
    }

    .code-dot { width: 12px; height: 12px; border-radius: 50%; }
    .dot-r { background: #ff5f57; }
    .dot-y { background: #febc2e; }
    .dot-g { background: #28c840; }

    .code-filename {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.78rem;
      color: rgba(255,255,255,0.4);
      margin-left: auto;
    }

    pre {
      padding: 2rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.82rem;
      line-height: 1.8;
      color: var(--code-text);
      overflow-x: auto;
    }

    .kw { color: #c792ea; }
    .fn { color: #82aaff; }
    .str { color: #c3e88d; }
    .cm { color: #546e7a; }
    .num { color: #f78c6c; }
    .cls { color: #ffcb6b; }

    /* ── USE CASES ── */
    .section-usecases { padding: 6rem 0; border-top: 3px solid var(--border); }

    .usecases-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1.5rem;
    }

    .usecase-card {
      border: 2px solid var(--border);
      padding: 2.5rem;
      transition: all 0.2s;
      animation: fadeUp 0.5s ease both;
      position: relative;
      overflow: hidden;
    }

    .usecase-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
      background: var(--accent);
      transform: scaleX(0);
      transition: transform 0.3s;
      transform-origin: left;
    }

    .usecase-card:hover::before { transform: scaleX(1); }
    .usecase-card:hover { transform: translateY(-4px); box-shadow: 0 12px 30px rgba(0,0,0,0.1); }

    .usecase-icon { font-size: 2rem; margin-bottom: 1.2rem; display: block; }

    .usecase-name {
      font-family: 'Noto Serif SC', serif;
      font-weight: 700;
      font-size: 1.05rem;
      margin-bottom: 0.6rem;
    }

    .usecase-desc { font-size: 0.85rem; color: var(--muted); line-height: 1.7; }

    /* ── COMPARE TABLE ── */
    .section-compare { padding: 6rem 0; border-top: 3px solid var(--border); }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.88rem;
    }

    th {
      background: var(--ink);
      color: var(--paper);
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.75rem;
      letter-spacing: 0.12em;
      padding: 1.1rem 1.5rem;
      text-align: left;
      border: 2px solid var(--border);
    }

    td {
      padding: 1rem 1.5rem;
      border: 1px solid #ddd;
      line-height: 1.6;
      vertical-align: top;
    }

    tr:hover td { background: rgba(192,57,43,0.03); }

    .tag {
      display: inline-block;
      padding: 0.2rem 0.6rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.08em;
    }

    .tag-yes { background: #e8f5e9; color: #2e7d32; border: 1px solid #a5d6a7; }
    .tag-no { background: #fce4ec; color: #c62828; border: 1px solid #ef9a9a; }
    .tag-partial { background: #fff3e0; color: #e65100; border: 1px solid #ffcc80; }

    /* ── FOOTER ── */
    footer {
      border-top: 3px solid var(--border);
      background: var(--ink);
      color: var(--paper);
      padding: 3rem 0;
    }

    .footer-inner {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 2rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .footer-brand {
      font-family: 'Noto Serif SC', serif;
      font-weight: 900;
      font-size: 1.3rem;
    }

    .footer-meta {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      color: rgba(255,255,255,0.4);
      letter-spacing: 0.08em;
    }

    /* ── SCROLL REVEAL ── */
    .reveal {
      opacity: 0;
      transform: translateY(24px);
      transition: opacity 0.6s ease, transform 0.6s ease;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* ── RESPONSIVE ── */
    @media (max-width: 900px) {
      .hero { grid-template-columns: 1fr; }
      .hero-right { display: none; }
      .two-col { grid-template-columns: 1fr; gap: 2rem; }
      .features-grid { grid-template-columns: 1fr 1fr; }
      .feature-card { border-bottom: 2px solid var(--border); }
      .steps { grid-template-columns: 1fr 1fr; }
      .step { border-bottom: 2px solid var(--border); }
      .usecases-grid { grid-template-columns: 1fr 1fr; }
      .types-list { gap: 1rem; }
      .header-nav { display: none; }
    }

    @media (max-width: 600px) {
      .hero-left { padding: 3rem 1.5rem; }
      .features-grid { grid-template-columns: 1fr; }
      .usecases-grid { grid-template-columns: 1fr; }
      .steps { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="header-tag">TUTORIAL</div>
  <div class="header-title">AI Agent 智能体教程</div>
  <nav class="header-nav">
    <a href="#what">概念</a>
    <a href="#features">特性</a>
    <a href="#types">类型</a>
    <a href="#how">原理</a>
    <a href="#code">代码</a>
  </nav>
</header>

<!-- HERO -->
<section class="hero">
  <div class="hero-left">
    <span class="hero-label">// AI Agent · 智能体 · 入门教程</span>
    <h1 class="hero-heading">
      AI<br><span class="accent">智能体</span><br>完全指南
    </h1>
    <p class="hero-sub">
      从零开始理解 AI Agent 的核心概念、工作原理、类型分类，以及如何在实际项目中构建和部署你的第一个智能体。
    </p>
    <div class="hero-cta">
      <a href="#what" class="btn btn-primary">开始学习 →</a>
      <a href="#code" class="btn btn-secondary">查看代码</a>
    </div>
  </div>
  <div class="hero-right">
    <div class="diagram">
      <div class="diag-node">用户输入 / 环境感知</div>
      <div class="diag-arrow">↓</div>
      <div class="diag-node core">🤖 AI Agent 智能体</div>
      <div class="diag-arrow">↓</div>
      <div class="diag-row">
        <div class="diag-node">🧠 推理</div>
        <div class="diag-node">📋 规划</div>
        <div class="diag-node">🛠️ 工具调用</div>
      </div>
      <div class="diag-arrow">↓</div>
      <div class="diag-node">执行动作 / 返回结果</div>
    </div>
  </div>
</section>

<!-- WHAT IS AGENT -->
<section class="section-what" id="what">
  <div class="container">
    <div class="section-header">
      <span class="section-num">01 —</span>
      <h2 class="section-title">什么是 AI Agent？</h2>
    </div>
    <div class="two-col">
      <div class="prose">
        <p>
          <strong>AI Agent（人工智能智能体）</strong> 是一种能够<strong>感知环境、做出决策并采取行动</strong>以实现特定目标的人工智能系统。与传统的 AI 模型（只能被动地响应单次输入）不同，AI Agent 具有主动性和持续性。
        </p>
        <p>
          简单来说，AI Agent 就像一个"自主工作的 AI 员工"——你给它一个目标，它会自己思考该怎么做、需要哪些工具、分几步完成，并且不断地执行、观察结果、调整策略。
        </p>
        <div class="pullquote">
          AI Agent = 感知 + 推理 + 规划 + 工具使用 + 执行 + 反馈循环
        </div>
        <p>
          区别于普通的大语言模型（LLM），AI Agent 能够调用外部工具（如搜索引擎、代码执行器、数据库），在多个步骤中完成复杂任务，并在过程中根据结果动态调整计划。
        </p>
      </div>
      <div>
        <div style="border: 2px solid var(--border); padding: 2.5rem; margin-bottom: 1.5rem;">
          <h3 style="font-family: 'Noto Serif SC'; font-weight:700; margin-bottom: 1.2rem; font-size:1rem;">LLM vs Agent 对比</h3>
          <div style="display:flex; flex-direction:column; gap:1rem;">
            <div style="padding:1rem; background:#f9f9f9; border-left: 3px solid #aaa;">
              <div style="font-size:0.78rem; color:var(--muted); font-family:monospace; margin-bottom:0.4rem;">普通 LLM</div>
              <div style="font-size:0.88rem;">输入问题 → 一次生成 → 输出答案。<br>无法主动使用工具，无状态记忆。</div>
            </div>
            <div style="padding:1rem; background:rgba(192,57,43,0.04); border-left: 3px solid var(--accent);">
              <div style="font-size:0.78rem; color:var(--accent); font-family:monospace; margin-bottom:0.4rem;">AI Agent</div>
              <div style="font-size:0.88rem;">接收目标 → 规划步骤 → 调用工具 → 观察结果 → 循环迭代 → 完成复杂任务。</div>
            </div>
          </div>
        </div>
        <div style="border: 2px solid var(--border); padding: 2rem; background: var(--ink); color: var(--paper);">
          <div style="font-family: 'JetBrains Mono'; font-size:0.72rem; color: rgba(255,255,255,0.4); margin-bottom:1rem;">// 经典定义</div>
          <p style="font-family: 'Noto Serif SC'; font-size:0.95rem; line-height:1.7;">"智能体是任何可以通过传感器感知环境，并通过执行器对环境做出响应的东西。"</p>
          <p style="font-size:0.78rem; color:rgba(255,255,255,0.4); margin-top:0.8rem;">— Stuart Russell & Peter Norvig,《人工智能：现代方法》</p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- FEATURES -->
<section class="section-features" id="features">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">02 —</span>
      <h2 class="section-title">核心特性</h2>
    </div>
    <div class="features-grid reveal">
      <div class="feature-card">
        <span class="feature-icon">🎯</span>
        <div class="feature-name">目标导向</div>
        <div class="feature-desc">智能体以目标为驱动，持续行动直到任务完成，而不仅仅是响应单条指令。</div>
      </div>
      <div class="feature-card">
        <span class="feature-icon">🔄</span>
        <div class="feature-name">自主决策</div>
        <div class="feature-desc">能够在没有人工干预的情况下，自主决定下一步行动，处理不确定性和异常情况。</div>
      </div>
      <div class="feature-card">
        <span class="feature-icon">🛠️</span>
        <div class="feature-name">工具调用</div>
        <div class="feature-desc">通过 API、插件、代码执行等工具与外部世界交互，突破 LLM 的知识边界。</div>
      </div>
      <div class="feature-card">
        <span class="feature-icon">💾</span>
        <div class="feature-name">记忆持久</div>
        <div class="feature-desc">拥有短期记忆（上下文）和长期记忆（向量数据库），能在会话间保持连贯性。</div>
      </div>
    </div>
  </div>
</section>

<!-- TYPES -->
<section class="section-types" id="types">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">03 —</span>
      <h2 class="section-title">智能体类型</h2>
    </div>
    <div class="types-list">
      <div class="type-item reveal">
        <span class="type-num">01</span>
        <div>
          <div class="type-name">反应型智能体（Reactive Agent）</div>
          <div class="type-desc">最简单的智能体类型。直接将感知映射到动作，没有内部状态或记忆。响应速度快，但无法处理需要历史信息的复杂任务。<br><br><strong>示例：</strong>简单的聊天机器人、规则引擎。</div>
        </div>
      </div>
      <div class="type-item reveal">
        <span class="type-num">02</span>
        <div>
          <div class="type-name">基于模型的智能体（Model-Based Agent）</div>
          <div class="type-desc">维护一个内部世界模型，记录环境状态，能处理部分可观察的环境。决策基于当前状态与历史信息的综合判断。<br><br><strong>示例：</strong>带记忆的对话助手、游戏 AI。</div>
        </div>
      </div>
      <div class="type-item reveal">
        <span class="type-num">03</span>
        <div>
          <div class="type-name">目标驱动型智能体（Goal-Based Agent）</div>
          <div class="type-desc">不仅维护世界模型，还明确定义目标状态。通过搜索和规划找到从当前状态到目标状态的行动路径。<br><br><strong>示例：</strong>路径规划、任务自动化智能体。</div>
        </div>
      </div>
      <div class="type-item reveal">
        <span class="type-num">04</span>
        <div>
          <div class="type-name">效用型智能体（Utility-Based Agent）</div>
          <div class="type-desc">在多个目标之间进行权衡，通过效用函数评估不同行动方案的优劣，选择最优解。适合处理有冲突目标的复杂场景。<br><br><strong>示例：</strong>推荐系统、资源调度优化。</div>
        </div>
      </div>
      <div class="type-item reveal">
        <span class="type-num">05</span>
        <div>
          <div class="type-name">学习型智能体（Learning Agent）</div>
          <div class="type-desc">能够从经验中学习，随着时间推移不断提高性能。包含学习元素、批评元素、执行元素和问题生成器四个组成部分。<br><br><strong>示例：</strong>强化学习 Agent、自适应个性化系统。</div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- HOW IT WORKS -->
<section class="section-how" id="how">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">04 —</span>
      <h2 class="section-title">工作原理</h2>
    </div>
    <div class="steps reveal">
      <div class="step">
        <span class="step-num">01</span>
        <span class="step-icon">👁️</span>
        <div class="step-name">感知</div>
        <div class="step-label">接收用户指令或环境信号，理解任务背景和约束条件</div>
      </div>
      <div class="step">
        <span class="step-num">02</span>
        <span class="step-icon">🧠</span>
        <div class="step-name">推理</div>
        <div class="step-label">利用大语言模型对任务进行深度分析，识别关键子目标</div>
      </div>
      <div class="step">
        <span class="step-num">03</span>
        <span class="step-icon">📋</span>
        <div class="step-name">规划</div>
        <div class="step-label">分解任务为可执行的步骤序列，确定工具调用顺序</div>
      </div>
      <div class="step">
        <span class="step-num">04</span>
        <span class="step-icon">⚡</span>
        <div class="step-name">执行</div>
        <div class="step-label">调用工具（搜索/代码/API）执行每个步骤，记录中间结果</div>
      </div>
      <div class="step">
        <span class="step-num">05</span>
        <span class="step-icon">🔁</span>
        <div class="step-name">反思</div>
        <div class="step-label">评估执行结果，判断是否达成目标，必要时调整策略循环迭代</div>
      </div>
    </div>
    <div style="margin-top:3rem; padding:2rem; border:2px solid var(--border); background:var(--highlight);" class="reveal">
      <strong style="font-family:'JetBrains Mono'; font-size:0.8rem; letter-spacing:0.1em;">💡 ReAct 框架</strong>
      <p style="margin-top:0.8rem; font-size:0.9rem; line-height:1.8;">现代 AI Agent 常采用 <strong>ReAct（Reasoning + Acting）</strong> 框架：交替进行"思考（Thought）→ 行动（Action）→ 观察（Observation）"三个步骤，形成推理-行动循环，直到得出最终答案。</p>
    </div>
  </div>
</section>

<!-- CODE EXAMPLE -->
<section class="section-code" id="code">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">05 —</span>
      <h2 class="section-title">代码示例</h2>
    </div>
    <div class="two-col">
      <div>
        <p style="font-size:0.92rem; color:var(--muted); margin-bottom:1.5rem; line-height:1.8;">以下示例展示如何使用 Python 构建一个简单的 AI Agent，它能够自主调用工具（搜索、计算）来完成用户任务。</p>
        <div style="display:flex; flex-direction:column; gap:1rem;">
          <div style="padding:1.2rem 1.5rem; border:2px solid var(--border);">
            <div style="font-family:'JetBrains Mono'; font-size:0.72rem; color:var(--accent); margin-bottom:0.5rem;">安装依赖</div>
            <code style="font-family:'JetBrains Mono'; font-size:0.82rem;">pip install openai langchain</code>
          </div>
          <div style="padding:1.2rem 1.5rem; border:2px solid var(--border);">
            <div style="font-family:'JetBrains Mono'; font-size:0.72rem; color:var(--accent); margin-bottom:0.5rem;">核心组件</div>
            <div style="font-size:0.85rem; line-height:1.8;">
              🧠 <strong>LLM</strong> — 推理大脑（GPT / Claude）<br>
              🛠️ <strong>Tools</strong> — 可调用的工具集合<br>
              💾 <strong>Memory</strong> — 短期 / 长期记忆<br>
              🔄 <strong>Executor</strong> — Agent 执行引擎
            </div>
          </div>
        </div>
      </div>
      <div class="code-block-wrap">
        <div class="code-header">
          <div class="code-dot dot-r"></div>
          <div class="code-dot dot-y"></div>
          <div class="code-dot dot-g"></div>
          <span class="code-filename">agent_demo.py</span>
        </div>
        <pre><span class="cm"># 使用 LangChain 构建简单 AI Agent</span>
<span class="kw">from</span> langchain.agents <span class="kw">import</span> AgentType, initialize_agent
<span class="kw">from</span> langchain.tools <span class="kw">import</span> Tool
<span class="kw">from</span> langchain.chat_models <span class="kw">import</span> ChatOpenAI

<span class="cm"># 1. 定义工具</span>
<span class="kw">def</span> <span class="fn">search_web</span>(query: <span class="cls">str</span>) -> <span class="cls">str</span>:
    <span class="str">"""搜索互联网获取信息"""</span>
    <span class="cm"># 实际项目中接入搜索 API</span>
    <span class="kw">return</span> <span class="str">f"搜索结果：关于 {query} 的信息..."</span>

<span class="kw">def</span> <span class="fn">calculate</span>(expression: <span class="cls">str</span>) -> <span class="cls">str</span>:
    <span class="str">"""执行数学计算"""</span>
    <span class="kw">return</span> <span class="cls">str</span>(<span class="fn">eval</span>(expression))

tools = [
    <span class="cls">Tool</span>(name=<span class="str">"搜索"</span>, func=search_web,
         description=<span class="str">"当需要查询实时信息时使用"</span>),
    <span class="cls">Tool</span>(name=<span class="str">"计算器"</span>, func=calculate,
         description=<span class="str">"当需要数学计算时使用"</span>),
]

<span class="cm"># 2. 初始化 LLM</span>
llm = <span class="cls">ChatOpenAI</span>(
    model=<span class="str">"gpt-4"</span>,
    temperature=<span class="num">0</span>
)

<span class="cm"># 3. 创建 Agent</span>
agent = <span class="fn">initialize_agent</span>(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=<span class="kw">True</span>,  <span class="cm"># 显示推理过程</span>
)

<span class="cm"># 4. 运行 Agent</span>
result = agent.<span class="fn">run</span>(
    <span class="str">"搜索今天的 AI 新闻，并统计共有多少条"</span>
)
<span class="fn">print</span>(result)</pre>
      </div>
    </div>
  </div>
</section>

<!-- USE CASES -->
<section class="section-usecases">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">06 —</span>
      <h2 class="section-title">应用场景</h2>
    </div>
    <div class="usecases-grid">
      <div class="usecase-card reveal">
        <span class="usecase-icon">💻</span>
        <div class="usecase-name">代码开发助手</div>
        <div class="usecase-desc">自动编写代码、调试错误、搜索文档、运行测试，全程无需人工干预。</div>
      </div>
      <div class="usecase-card reveal">
        <span class="usecase-icon">🔬</span>
        <div class="usecase-name">科研数据分析</div>
        <div class="usecase-desc">自动下载数据集、执行统计分析、生成可视化图表、撰写分析报告。</div>
      </div>
      <div class="usecase-card reveal">
        <span class="usecase-icon">🛒</span>
        <div class="usecase-name">电商自动化</div>
        <div class="usecase-desc">监控竞品价格、自动调价、处理客服对话、生成商品描述文案。</div>
      </div>
      <div class="usecase-card reveal">
        <span class="usecase-icon">📧</span>
        <div class="usecase-name">邮件与日程管理</div>
        <div class="usecase-desc">自动分类处理邮件、安排会议、设置提醒、起草回复内容。</div>
      </div>
      <div class="usecase-card reveal">
        <span class="usecase-icon">🏥</span>
        <div class="usecase-name">医疗辅助诊断</div>
        <div class="usecase-desc">分析患者症状、查阅医学文献、提供初步诊断建议（辅助医生）。</div>
      </div>
      <div class="usecase-card reveal">
        <span class="usecase-icon">💹</span>
        <div class="usecase-name">金融风控分析</div>
        <div class="usecase-desc">实时监控市场数据、分析风险指标、自动生成投资分析报告。</div>
      </div>
    </div>
  </div>
</section>

<!-- COMPARE TABLE -->
<section class="section-compare">
  <div class="container">
    <div class="section-header reveal">
      <span class="section-num">07 —</span>
      <h2 class="section-title">主流框架对比</h2>
    </div>
    <div style="overflow-x:auto;" class="reveal">
      <table>
        <thead>
          <tr>
            <th>框架</th>
            <th>开发语言</th>
            <th>多 Agent</th>
            <th>工具调用</th>
            <th>记忆管理</th>
            <th>适合场景</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><strong>LangChain</strong></td>
            <td>Python / JS</td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td><span class="tag tag-yes">✓ 丰富</span></td>
            <td><span class="tag tag-yes">✓ 完善</span></td>
            <td>通用场景，生态最丰富</td>
          </tr>
          <tr>
            <td><strong>AutoGPT</strong></td>
            <td>Python</td>
            <td><span class="tag tag-no">✗ 单体</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td><span class="tag tag-partial">△ 基础</span></td>
            <td>自主长任务执行</td>
          </tr>
          <tr>
            <td><strong>CrewAI</strong></td>
            <td>Python</td>
            <td><span class="tag tag-yes">✓ 核心</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td><span class="tag tag-partial">△ 有限</span></td>
            <td>多 Agent 协作流程</td>
          </tr>
          <tr>
            <td><strong>AutoGen</strong></td>
            <td>Python</td>
            <td><span class="tag tag-yes">✓ 强大</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td>多 Agent 对话与协作</td>
          </tr>
          <tr>
            <td><strong>Semantic Kernel</strong></td>
            <td>C# / Python</td>
            <td><span class="tag tag-partial">△ 有限</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td><span class="tag tag-yes">✓ 支持</span></td>
            <td>企业级 .NET 应用</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <div class="footer-brand">AI Agent 教程</div>
    <div class="footer-meta">CQU刘学长设计 · 2026</div>
  </div>
</footer>

<script>
  // Scroll reveal
  const reveals = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1, rootMargin: '0px 0px -40px 0px' });

  reveals.forEach(el => observer.observe(el));

  // Stagger feature cards
  document.querySelectorAll('.feature-card').forEach((card, i) => {
    card.style.animationDelay = `${i * 0.1}s`;
  });
</script>

</body>
</html>
