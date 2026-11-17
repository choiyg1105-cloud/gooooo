<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>깃허브 주소 입력표 현황판</title>
  <style>
    :root {
      --canvas-top: #102133;
      --canvas-mid: #1a344d;
      --canvas-bot: #112438;
      --card: #ffffff;
      --card-soft: #f3f6fa;
      --ink: #1b2533;
      --muted: #707f90;
      --line: #e5ecf3;
      --mint: #16d1b6;
      --teal: #18b0c4;
      --indigo: #5658d6;
      --coral: #ef5757;
      --coral-soft: #fdeeee;
      --mint-soft: #e6faf6;
      --warn: #a35a12;
      --warn-soft: #fff4e5;
      --shadow: 0 14px 32px rgba(7, 19, 32, 0.28);
      --radius: 16px;
      --maxw: 1160px;
    }

    * { box-sizing: border-box; }
    body {
      margin: 0;
      min-height: 100vh;
      background: radial-gradient(1200px 600px at 20% 0%, #21415f 0%, transparent 60%),
                  linear-gradient(155deg, var(--canvas-top) 0%, var(--canvas-mid) 52%, var(--canvas-bot) 100%);
      color: var(--ink);
      font-family: "Pretendard", "Noto Sans KR", "Apple SD Gothic Neo", "Malgun Gothic", sans-serif;
      line-height: 1.55;
      -webkit-font-smoothing: antialiased;
    }

    .wrap {
      max-width: var(--maxw);
      margin: 0 auto;
      padding: 26px 20px 64px;
    }

    /* Top Bar on Dark Navy Canvas */
    .topbar {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      margin-bottom: 22px;
    }

    .mode-badge {
      display: inline-flex;
      align-items: center;
      gap: 7px;
      font-size: 0.8rem;
      font-weight: 700;
      padding: 6px 13px;
      border-radius: 999px;
      background: rgba(22, 209, 182, 0.14);
      color: #6ef0dc;
      border: 1px solid rgba(22, 209, 182, 0.35);
      backdrop-filter: blur(6px);
    }
    .mode-badge.local {
      background: rgba(255, 196, 107, 0.16);
      color: #ffd38a;
      border-color: rgba(255, 196, 107, 0.4);
    }

    .csv-actions {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      align-items: center;
    }

    .btn {
      appearance: none;
      border: 1px solid rgba(255, 255, 255, 0.22);
      background: rgba(255, 255, 255, 0.08);
      color: #eef5fc;
      padding: 8px 15px;
      border-radius: 999px;
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      transition: transform 0.12s ease, background 0.15s ease;
    }
    .btn:hover { background: rgba(255, 255, 255, 0.16); }
    .btn:active { transform: translateY(1px); }
    .btn-card {
      border: 1px solid var(--line);
      background: var(--card-soft);
      color: var(--ink);
    }
    .btn-card:hover { background: #e8eef5; }
    .btn-primary {
      border: none;
      background: linear-gradient(135deg, var(--mint), var(--teal));
      color: #09242b;
      font-weight: 700;
      box-shadow: 0 6px 16px rgba(22, 209, 182, 0.28);
    }
    .btn-primary:hover { filter: brightness(1.06); background: linear-gradient(135deg, var(--mint), var(--teal)); }
    .btn:focus-visible, input:focus-visible, select:focus-visible {
      outline: 2px solid var(--mint);
      outline-offset: 2px;
    }

    /* Hero */
    .hero { margin-bottom: 24px; }
    .eyebrow {
      font-size: 0.76rem;
      letter-spacing: 0.12em;
      color: #79e7d4;
      font-weight: 700;
      margin: 0 0 6px;
    }
    h1 {
      font-size: clamp(1.75rem, 3.4vw, 2.4rem);
      line-height: 1.24;
      letter-spacing: -0.02em;
      margin: 0 0 10px;
      color: #ffffff;
    }
    .lead {
      margin: 0;
      max-width: 68ch;
      color: #b8c9da;
      font-size: 0.96rem;
    }

    /* Bento Summary Grid */
    .bento-top {
      display: grid;
      grid-template-columns: 1.15fr 1.1fr 0.95fr;
      gap: 16px;
      margin-bottom: 14px;
    }

    .bento-card {
      background: var(--card);
      border-radius: var(--radius);
      padding: 20px 22px;
      box-shadow: var(--shadow);
    }

    .kpi-row {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 16px;
      align-items: center;
    }
    .kpi-numbers {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 10px;
    }
    .kpi-item-val {
      font-size: 1.85rem;
      font-weight: 300;
      letter-spacing: -0.03em;
      font-variant-numeric: tabular-nums;
      color: var(--ink);
      line-height: 1.15;
    }
    .kpi-item-val small {
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--muted);
      margin-left: 2px;
    }
    .kpi-item-lbl {
      font-size: 0.74rem;
      font-weight: 600;
      color: var(--muted);
      margin-top: 4px;
    }
    .ring-legend {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 14px;
      padding-top: 12px;
      border-top: 1px solid var(--line);
      font-size: 0.76rem;
      color: var(--muted);
    }
    .dot-swatch {
      display: inline-block;
      width: 9px;
      height: 9px;
      border-radius: 50%;
      margin-right: 5px;
      vertical-align: middle;
    }

    /* Slider-style Progress Bars (Top-Middle Bento Card) */
    .mini-bars {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }
    .mini-bar-head {
      display: flex;
      justify-content: space-between;
      font-size: 0.78rem;
      font-weight: 700;
      color: var(--ink);
      margin-bottom: 5px;
    }
    .mini-bar-head span:last-child {
      color: var(--muted);
      font-variant-numeric: tabular-nums;
    }
    .mini-track {
      height: 8px;
      border-radius: 999px;
      background: #eaf0f6;
      overflow: hidden;
    }
    .mini-fill {
      height: 100%;
      border-radius: 999px;
      background: linear-gradient(90deg, var(--mint), var(--teal));
    }
    .mini-fill.indigo { background: linear-gradient(90deg, var(--indigo), #8378f4); }
    .mini-fill.coral { background: linear-gradient(90deg, var(--coral), #f5826b); }

    .pill-strip {
      display: flex;
      flex-wrap: wrap;
      gap: 7px;
      margin-top: 14px;
    }
    .pill-tag {
      font-size: 0.75rem;
      font-weight: 700;
      padding: 4px 10px;
      border-radius: 999px;
      background: var(--mint-soft);
      color: #0e7c6d;
    }
    .pill-tag.coral {
      background: var(--coral-soft);
      color: #c23636;
    }

    /* Dot Matrix Card */
    .matrix-wrap {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
    }
    .dot-grid {
      display: grid;
      grid-template-columns: repeat(5, 16px);
      gap: 8px;
    }
    .resp-dot {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      border: none;
      padding: 0;
      cursor: pointer;
      transition: transform 0.15s ease;
    }
    .resp-dot:hover { transform: scale(1.22); }
    .matrix-stats {
      flex: 1;
      font-size: 0.8rem;
      color: var(--muted);
    }
    .matrix-big {
      font-size: 1.7rem;
      font-weight: 300;
      color: var(--ink);
      line-height: 1.1;
      margin-bottom: 6px;
      font-variant-numeric: tabular-nums;
    }
    .matrix-line {
      display: flex;
      justify-content: space-between;
      margin-top: 3px;
      font-variant-numeric: tabular-nums;
    }

    .criteria-box {
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid rgba(255, 255, 255, 0.14);
      color: #c7d6e6;
      border-radius: 12px;
      padding: 11px 16px;
      font-size: 0.82rem;
      margin-bottom: 24px;
    }

    /* Pill Tabs on Dark Navy */
    .tabs {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 20px;
    }
    .tab-btn {
      appearance: none;
      border: 1px solid rgba(255, 255, 255, 0.18);
      background: rgba(255, 255, 255, 0.07);
      color: #d6e4f2;
      padding: 9px 18px;
      border-radius: 999px;
      font-size: 0.9rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.15s ease;
    }
    .tab-btn:hover { background: rgba(255, 255, 255, 0.15); color: #ffffff; }
    .tab-btn[aria-selected="true"] {
      border-color: transparent;
      background: linear-gradient(135deg, var(--mint), var(--teal));
      color: #08232a;
      font-weight: 700;
      box-shadow: 0 6px 18px rgba(22, 209, 182, 0.28);
    }

    .panel { display: none; }
    .panel.active { display: block; }

    .split {
      display: grid;
      grid-template-columns: 1.2fr 0.95fr;
      gap: 18px;
      align-items: start;
    }
    .region-grid {
      display: grid;
      grid-template-columns: 1.05fr 1fr;
      gap: 18px;
      align-items: start;
    }
    .stack {
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .box {
      background: var(--card);
      border-radius: var(--radius);
      padding: 22px;
      box-shadow: var(--shadow);
    }
    .box h2 {
      font-size: 1.08rem;
      margin: 0 0 6px;
      color: var(--ink);
    }
    .box-desc {
      margin: 0 0 16px;
      font-size: 0.86rem;
      color: var(--muted);
    }

    /* Korea Province SVG Map */
    .map-legend {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 12px;
      font-size: 0.78rem;
      color: var(--muted);
      margin-bottom: 12px;
    }
    .legend-item {
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }
    .legend-swatch {
      width: 13px;
      height: 13px;
      border-radius: 4px;
      border: 1px solid #d5dee8;
      display: inline-block;
    }
    .korea-map-wrap {
      background: #f5f8fb;
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 12px;
      display: flex;
      justify-content: center;
    }
    svg.korea-map {
      width: 100%;
      max-width: 390px;
      height: auto;
      display: block;
    }
    .map-region { cursor: pointer; outline: none; }
    .map-region .region-shape {
      stroke: #b5c3d1;
      stroke-width: 1.25;
      stroke-linejoin: round;
      transition: fill 0.18s ease, stroke 0.15s ease, stroke-width 0.15s ease;
    }
    .map-region:hover .region-shape,
    .map-region:focus-visible .region-shape {
      stroke: #1b2533;
      stroke-width: 2.2;
    }
    .map-region.selected .region-shape {
      stroke: #102133;
      stroke-width: 2.7;
    }
    .map-label {
      font-size: 9.5px;
      font-weight: 700;
      text-anchor: middle;
      dominant-baseline: central;
      pointer-events: none;
      fill: #243242;
    }
    .map-label.on-dark { fill: #ffffff; }
    .map-subcount {
      font-size: 8.5px;
      font-weight: 700;
      text-anchor: middle;
      dominant-baseline: central;
      pointer-events: none;
      fill: #2d3e50;
    }
    .map-subcount.on-dark { fill: #e4f8f5; }

    .unplaced-row {
      margin-top: 12px;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      padding: 10px 13px;
      border-radius: 10px;
      border: 1px dashed #eab87c;
      background: var(--warn-soft);
      font-size: 0.83rem;
      color: var(--warn);
    }
    .unplaced-btn {
      appearance: none;
      border: 1px solid #d99748;
      background: #ffffff;
      color: var(--warn);
      font-weight: 700;
      font-size: 0.8rem;
      padding: 6px 12px;
      border-radius: 999px;
      cursor: pointer;
    }
    .unplaced-btn[aria-pressed="true"] {
      background: var(--warn);
      color: #ffffff;
      border-color: var(--warn);
    }

    /* Gradient Pill Bar Charts */
    .bar-list {
      display: flex;
      flex-direction: column;
      gap: 9px;
    }
    .bar-item {
      width: 100%;
      text-align: left;
      appearance: none;
      border: 1px solid var(--line);
      background: #ffffff;
      border-radius: 11px;
      padding: 10px 13px;
      cursor: pointer;
      transition: border-color 0.15s ease, background 0.15s ease, transform 0.1s ease;
    }
    .bar-item:hover { background: #f8fafd; border-color: #c4d1df; }
    .bar-item[aria-pressed="true"] {
      border-color: var(--teal);
      background: #f1fbf9;
      box-shadow: inset 0 0 0 1px rgba(24, 176, 196, 0.25);
    }
    .bar-meta {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      gap: 10px;
      font-size: 0.89rem;
      font-weight: 700;
      color: var(--ink);
      margin-bottom: 7px;
    }
    .bar-count {
      font-variant-numeric: tabular-nums;
      color: var(--muted);
      font-size: 0.83rem;
      font-weight: 600;
      white-space: nowrap;
    }
    .bar-track {
      width: 100%;
      height: 9px;
      background: #ebf0f5;
      border-radius: 999px;
      overflow: hidden;
    }
    .bar-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--indigo) 0%, var(--teal) 55%, var(--mint) 100%);
      border-radius: 999px;
      transition: width 0.25s ease;
    }

    .name-chips {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 12px;
    }
    .name-chip {
      appearance: none;
      border: 1px solid #d5e4ea;
      background: var(--card-soft);
      color: var(--ink);
      padding: 6px 13px;
      border-radius: 999px;
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.15s ease;
    }
    .name-chip:hover {
      border-color: var(--teal);
      background: var(--mint-soft);
      color: #0b6d61;
    }

    .notice {
      margin-top: 14px;
      padding: 11px 14px;
      border-radius: 10px;
      font-size: 0.83rem;
      background: var(--warn-soft);
      color: var(--warn);
      border: 1px solid #f0d7b1;
    }
    .notice-muted {
      background: var(--card-soft);
      color: var(--muted);
      border-color: var(--line);
    }

    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 14px;
    }
    .input, .select {
      border: 1px solid var(--line);
      background: var(--card-soft);
      color: var(--ink);
      border-radius: 9px;
      padding: 8px 12px;
      font-size: 0.87rem;
      font-family: inherit;
    }
    .input { flex: 1 1 180px; }

    .roster-list {
      display: flex;
      flex-direction: column;
      gap: 6px;
      max-height: 460px;
      overflow-y: auto;
      padding-right: 4px;
    }
    .roster-btn {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
      text-align: left;
      border: 1px solid var(--line);
      background: #ffffff;
      border-radius: 10px;
      padding: 10px 13px;
      cursor: pointer;
    }
    .roster-btn:hover { background: var(--card-soft); }
    .roster-btn[aria-pressed="true"] {
      border-color: var(--teal);
      background: var(--mint-soft);
    }
    .roster-sub {
      font-size: 0.79rem;
      color: var(--muted);
    }

    dl.profile-dl {
      margin: 0;
      display: grid;
      grid-template-columns: 120px 1fr;
      gap: 11px 14px;
      font-size: 0.9rem;
    }
    dl.profile-dl dt {
      color: var(--muted);
      font-weight: 600;
    }
    dl.profile-dl dd {
      margin: 0;
      word-break: break-word;
    }
    .tag {
      display: inline-block;
      font-size: 0.75rem;
      padding: 2px 8px;
      border-radius: 999px;
      background: var(--card-soft);
      color: var(--muted);
      border: 1px solid var(--line);
      margin-left: 4px;
      vertical-align: middle;
    }
    .tag-warn {
      background: var(--coral-soft);
      color: #c23636;
      border-color: #f4c6c6;
    }
    a.ext-link {
      color: #2f4dd0;
      text-decoration: underline;
      text-underline-offset: 3px;
      font-weight: 600;
    }

    table.audit-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.89rem;
    }
    table.audit-table th, table.audit-table td {
      text-align: left;
      padding: 10px 8px;
      border-bottom: 1px solid var(--line);
    }
    table.audit-table th {
      color: var(--muted);
      font-weight: 700;
      font-size: 0.8rem;
    }
    .num { font-variant-numeric: tabular-nums; }

    @media (max-width: 960px) {
      .bento-top { grid-template-columns: 1fr; }
    }
    @media (max-width: 860px) {
      .split, .region-grid { grid-template-columns: 1fr; }
      dl.profile-dl { grid-template-columns: 1fr; gap: 4px; }
      dl.profile-dl dd { margin-bottom: 10px; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header class="topbar">
      <div id="modeBadge" class="mode-badge">공개 안전 모드 · 기본 내장 데이터</div>
      <div class="csv-actions">
        <input id="csvFileInput" type="file" accept=".csv,text/csv" hidden />
        <button id="openCsvBtn" class="btn btn-primary" type="button">CSV 열기</button>
        <button id="resetCsvBtn" class="btn" type="button" hidden>기본 데이터로 되돌리기</button>
      </div>
    </header>

    <section class="hero">
      <p class="eyebrow">DATA DASHBOARD · GITHUB &amp; PAGES</p>
      <h1>깃허브 주소 입력표 현황판</h1>
      <p id="heroLead" class="lead"></p>
    </section>

    <!-- Bento Summary Cards -->
    <section class="bento-top" aria-label="핵심 요약 지표">
      <!-- Card 1: 핵심 수치 + 동심원 링 게이지 -->
      <article class="bento-card">
        <div class="kpi-row">
          <div>
            <div class="kpi-numbers">
              <div>
                <div id="statTotal" class="kpi-item-val">-</div>
                <div class="kpi-item-lbl">전체 응답</div>
              </div>
              <div>
                <div id="statGithub" class="kpi-item-val">-</div>
                <div class="kpi-item-lbl">GitHub 입력</div>
              </div>
              <div>
                <div id="statPages" class="kpi-item-val">-</div>
                <div class="kpi-item-lbl">제출 페이지</div>
              </div>
            </div>
            <div class="ring-legend">
              <span><span class="dot-swatch" style="background:var(--mint)"></span>GitHub <strong id="ringPctGh">-</strong></span>
              <span><span class="dot-swatch" style="background:var(--teal)"></span>제출 페이지 <strong id="ringPctPg">-</strong></span>
              <span><span class="dot-swatch" style="background:var(--indigo)"></span>둘 다 <strong id="ringPctBoth">-</strong></span>
            </div>
          </div>
          <svg id="summaryRingSvg" width="96" height="96" viewBox="0 0 96 96" role="img" aria-label="입력 비율 동심원 게이지"></svg>
        </div>
      </article>

      <!-- Card 2: 입력 상태 & 형식 체크 바 -->
      <article class="bento-card">
        <div class="mini-bars">
          <div>
            <div class="mini-bar-head">
              <span>둘 다 입력</span>
              <span id="miniBothTxt">-</span>
            </div>
            <div class="mini-track"><div id="miniBothBar" class="mini-fill" style="width:0%"></div></div>
          </div>
          <div>
            <div class="mini-bar-head">
              <span>GitHub만 입력</span>
              <span id="miniGhOnlyTxt">-</span>
            </div>
            <div class="mini-track"><div id="miniGhOnlyBar" class="mini-fill indigo" style="width:0%"></div></div>
          </div>
          <div>
            <div class="mini-bar-head">
              <span>URL 형식 확인 필요 (전체 칸)</span>
              <span id="miniWarnTxt">-</span>
            </div>
            <div class="mini-track"><div id="miniWarnBar" class="mini-fill coral" style="width:0%"></div></div>
          </div>
        </div>
        <div class="pill-strip" id="summaryPills"></div>
      </article>

      <!-- Card 3: 응답자 권역 도트 매트릭스 -->
      <article class="bento-card">
        <div class="matrix-wrap">
          <div id="dotMatrix" class="dot-grid" aria-label="응답자 권역별 도트 분포"></div>
          <div class="matrix-stats">
            <div id="metroBig" class="matrix-big">-</div>
            <div style="font-weight:700; color:var(--ink); margin-bottom:6px;">수도권 거주 비중</div>
            <div class="matrix-line"><span><span class="dot-swatch" style="background:var(--indigo)"></span>경기도</span><strong id="dotStatGg">-</strong></div>
            <div class="matrix-line"><span><span class="dot-swatch" style="background:var(--mint)"></span>서울특별시</span><strong id="dotStatSl">-</strong></div>
            <div class="matrix-line"><span><span class="dot-swatch" style="background:var(--teal)"></span>비수도권 명시</span><strong id="dotStatEtc">-</strong></div>
            <div class="matrix-line"><span><span class="dot-swatch" style="background:var(--coral)"></span>확인 필요</span><strong id="dotStatUnk">-</strong></div>
          </div>
        </div>
      </article>
    </section>

    <div id="criteriaSummary" class="criteria-box"></div>

    <nav class="tabs" role="tablist" aria-label="데이터 분석 항목">
      <button class="tab-btn" role="tab" id="tab-submission" aria-controls="panel-submission" aria-selected="true" data-tab="submission">제출 현황</button>
      <button class="tab-btn" role="tab" id="tab-region" aria-controls="panel-region" aria-selected="false" data-tab="region">거주 지역 · 지도</button>
      <button class="tab-btn" role="tab" id="tab-hobby" aria-controls="panel-hobby" aria-selected="false" data-tab="hobby">취미</button>
      <button class="tab-btn" role="tab" id="tab-profile" aria-controls="panel-profile" aria-selected="false" data-tab="profile">이름별 프로필</button>
      <button class="tab-btn" role="tab" id="tab-audit" aria-controls="panel-audit" aria-selected="false" data-tab="audit">입력 확인</button>
    </nav>

    <!-- 1. 제출 현황 -->
    <section id="panel-submission" class="panel active" role="tabpanel" aria-labelledby="tab-submission">
      <div class="split">
        <div class="box">
          <h2>GitHub · 제출 페이지 입력 조합</h2>
          <p class="box-desc">막대 항목을 누르거나 키보드로 선택하면 오른쪽에서 해당 응답자 명단을 확인할 수 있습니다.</p>
          <div id="submissionBars" class="bar-list"></div>
          <div class="notice">
            입력 여부는 <strong>칸이 비어 있지 않은지</strong>만 집계합니다. 링크의 실제 HTTP 접속·정상 작동·과제 완료 여부는 확인하지 않았습니다.
          </div>
        </div>
        <div class="box">
          <h2 id="submissionDetailTitle">선택한 분류 명단</h2>
          <p id="submissionDetailDesc" class="box-desc"></p>
          <div id="submissionDetailList" class="name-chips"></div>
        </div>
      </div>
    </section>

    <!-- 2. 거주 지역 (지도 + 막대그래프 + 명단) -->
    <section id="panel-region" class="panel" role="tabpanel" aria-labelledby="tab-region">
      <div class="region-grid">
        <div class="box">
          <h2>시·도 단위 거주 분포 지도</h2>
          <p class="box-desc">어디에 많이 거주하는지 시·도 단위로 색 농도로 나타냈습니다. 지역을 클릭하거나 탭해 선택할 수 있습니다.</p>
          <div class="map-legend" aria-label="거주 인원 색상 범례">
            <span class="legend-item"><span class="legend-swatch" style="background:#edf2f7"></span>0명</span>
            <span class="legend-item"><span class="legend-swatch" style="background:#a8ede3"></span>1~2명</span>
            <span class="legend-item"><span class="legend-swatch" style="background:#37d1bf"></span>3~5명</span>
            <span class="legend-item"><span class="legend-swatch" style="background:#5658d6"></span>6명 이상</span>
          </div>
          <div class="korea-map-wrap">
            <svg id="koreaMapSvg" class="korea-map" viewBox="0 0 400 500" role="img" aria-label="대한민국 시·도별 거주 인원 지도"></svg>
          </div>
          <div id="unplacedRegionRow" class="unplaced-row" hidden></div>
          <div class="notice notice-muted">
            세부 동·구 주소와 개인별 좌표·이동 거리는 생성하거나 표시하지 않으며, 시·도 단위 집계만 지도에 반영합니다.
          </div>
        </div>

        <div class="stack">
          <div class="box">
            <h2>시·도별 인원 비교</h2>
            <p class="box-desc">지도와 연동됩니다. 시·도명이 생략된 값은 임의로 추측하지 않고 ‘분류 확인 필요’로 묶었습니다.</p>
            <div id="regionBars" class="bar-list"></div>
          </div>
          <div class="box">
            <h2 id="regionDetailTitle">선택한 지역 명단</h2>
            <p id="regionDetailDesc" class="box-desc"></p>
            <div id="regionDetailList" class="name-chips"></div>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. 취미 -->
    <section id="panel-hobby" class="panel" role="tabpanel" aria-labelledby="tab-hobby">
      <div class="split">
        <div class="box">
          <h2>취미 분포 (원문 표기 유지)</h2>
          <p class="box-desc">쉼표(,) 기준으로 나누고 1인당 중복은 1회만 계산했습니다. 비슷한 표현을 임의로 합치지 않았습니다.</p>
          <div class="controls">
            <input id="hobbySearch" class="input" type="search" placeholder="취미 검색 (예: 기타, 음악, 게임)" aria-label="취미 검색" />
            <button id="hobbyTopToggle" class="btn btn-card" type="button" aria-pressed="false">전체 보기</button>
          </div>
          <div id="hobbyBars" class="bar-list"></div>
        </div>
        <div class="box">
          <h2 id="hobbyDetailTitle">선택한 취미 입력자</h2>
          <p id="hobbyDetailDesc" class="box-desc"></p>
          <div id="hobbyDetailList" class="name-chips"></div>
        </div>
      </div>
    </section>

    <!-- 4. 이름별 프로필 -->
    <section id="panel-profile" class="panel" role="tabpanel" aria-labelledby="tab-profile">
      <div class="split">
        <div class="box">
          <h2>응답자 명단 검색 및 필터</h2>
          <p class="box-desc">이름을 선택하면 상세 입력 내역을 확인합니다.</p>
          <div class="controls">
            <input id="profileSearch" class="input" type="search" placeholder="이름 검색" aria-label="이름 검색" />
            <select id="profileStatusFilter" class="select" aria-label="제출 현황 필터">
              <option value="ALL">제출 현황: 전체</option>
              <option value="둘 다 입력">둘 다 입력</option>
              <option value="GitHub만 입력">GitHub만 입력</option>
              <option value="제출 페이지만 입력">제출 페이지만 입력</option>
              <option value="둘 다 미입력">둘 다 미입력</option>
            </select>
            <select id="profileRegionFilter" class="select" aria-label="거주 지역 필터">
              <option value="ALL">거주 지역: 전체</option>
            </select>
          </div>
          <div id="rosterList" class="roster-list"></div>
        </div>
        <div class="box">
          <h2 id="profileDetailName">프로필 상세</h2>
          <p id="profileDetailSub" class="box-desc"></p>
          <div id="profileDetailBody"></div>
          <div id="profilePrivacyNotice" class="notice notice-muted"></div>
        </div>
      </div>
    </section>

    <!-- 5. 입력 확인 -->
    <section id="panel-audit" class="panel" role="tabpanel" aria-labelledby="tab-audit">
      <div class="split">
        <div class="box">
          <h2>주요 항목별 누락 건수</h2>
          <p class="box-desc">원본 값을 임의로 고치거나 빈 칸을 추정해 채우지 않고, 있는 그대로 집계했습니다.</p>
          <table class="audit-table">
            <thead>
              <tr>
                <th>항목(열)</th>
                <th>입력됨</th>
                <th>누락(빈 칸)</th>
                <th>비고</th>
              </tr>
            </thead>
            <tbody id="auditTableBody"></tbody>
          </table>
        </div>
        <div class="box">
          <h2>집계 제외 행 및 비일관 컬럼 안내</h2>
          <div id="auditNotes"></div>
        </div>
      </div>
    </section>
  </div>

  <script>
    const PUBLIC_EMBEDDED_DATA = {
      meta: {
        preHeaderRows: 1,
        blankRows: 11,
        missingNameRows: 0,
        sourceLabel: "공개 안전 모드 · 기본 내장 데이터"
      },
      records: [
        { name: "박종석", github: "https://github.com/John02-design", pages: "https://john02-design.github.io/1234/", instagram: "I_am_jongseok", hobby: "일렉기타, 영화감상", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: false },
        { name: "박준영", github: "https://github.com/alex60796-bot", pages: "https://alex60796-bot.github.io/dkssudgktpdy/", instagram: "jxn.y0xn9", hobby: "피파", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "강민서", github: "https://github.com/rlverms", pages: "https://rlverms.github.io/rlverms/", instagram: "rlverms", hobby: "쿠리만쥬 인형 구매, 영화 시청, 음악 감상", regionProvince: "서울특별시", addressRaw: null, favorite: null, genderPresent: true },
        { name: "박정빈", github: "https://github.com/ImSAKUYAmyfriendisRYO", pages: "https://imsakuyamyfriendisryo.github.io/-/", instagram: "@upfstpu.2", hobby: "노래듣기", regionProvince: "분류 확인 필요", addressRaw: null, favorite: null, genderPresent: true },
        { name: "김나림", github: "github.com/nalimgim947", pages: "", instagram: "@thisisimrimthis", hobby: "그림, 음악, 운동, 애니", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "김보라", github: "https://github.com/KIMBORA050411", pages: "https://kimbora050411.github.io/050411/", instagram: "@b0.ra.4", hobby: "공포 보기", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "김승기", github: "https://github.com/Seung7l", pages: "https://seung7l.github.io/DoSiMoonWhaSulGye/", instagram: "@_seung7l", hobby: "네줄기타", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "문민희", github: "https://github.com/massiag41-art", pages: "https://massiag41-art.github.io/1233/", instagram: "@mini3061", hobby: "게임", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "민제희", github: "https://github.com/alfmekfn5", pages: "https://alfmekfn5.github.io/555/", instagram: "@mirdar.u_", hobby: "게임, 운동, 기깔나게 자기", regionProvince: "강원도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "윤수아", github: "https://github.com/tndk0512-ux", pages: "https://tndk0512-ux.github.io/index/", instagram: "tnd_0_5", hobby: "기타,애니,음악듣기", regionProvince: "서울특별시", addressRaw: null, favorite: null, genderPresent: true },
        { name: "이지민", github: "https://github.com/leejimin0", pages: "https://leejimin0.github.io/ezym/", instagram: "@tonmozza", hobby: "독서", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "정유림", github: "https://github.com/yourrime0502-afk", pages: "https://yourrime0502-afk.github.io/kdu1/", instagram: "https://www.instagram.com/nollimmm?mdxt=MWNnNHBwZm5kNWI4NQ==", hobby: "콘서트 관람", regionProvince: "분류 확인 필요", addressRaw: null, favorite: null, genderPresent: true },
        { name: "최윤영", github: "https://github.com/usagi030dd", pages: "https://usagi030dd.github.io/samples/ 입니다.", instagram: "@usagi030dd", hobby: "음악 감상 및 연주(통기타&일렉)", regionProvince: "서울특별시", addressRaw: null, favorite: null, genderPresent: true },
        { name: "최윤지", github: "https://github.com/choiyg1105-cloud", pages: "https://choiyg1105-cloud.github.io/save/", instagram: "yum._.crochet", hobby: "우쿨렐레, 뜨개질", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true },
        { name: "신단하", github: "https://github.com/danhaaaa", pages: "https://danhaaaa.github.io/sakuya/", instagram: "@danha_naa", hobby: "위시 사랑하기", regionProvince: "경기도", addressRaw: null, favorite: null, genderPresent: true }
      ]
    };

    const PROVINCE_PREFIXES = [
      "서울특별시", "부산광역시", "대구광역시", "인천광역시", "광주광역시", "대전광역시", "울산광역시", "세종특별자치시",
      "경기도", "강원도", "강원특별자치도", "충청북도", "충청남도", "전라북도", "전북특별자치도", "전라남도",
      "경상북도", "경상남도", "제주특별자치도"
    ];

    const KOREA_MAP_REGIONS = [
      { id: "강원", fullName: "강원도", aliases: ["강원도", "강원특별자치도"], points: "148,54 240,18 286,95 322,152 292,168 224,158 180,152 170,96", lx: 232, ly: 96 },
      { id: "경기", fullName: "경기도", aliases: ["경기도"], points: "92,66 148,54 170,96 180,152 162,172 136,180 102,172 86,152 84,126 76,102", lx: 126, ly: 152 },
      { id: "인천", fullName: "인천광역시", aliases: ["인천광역시"], points: "54,98 78,96 86,128 62,134 50,118", lx: 68, ly: 114 },
      { id: "서울", fullName: "서울특별시", aliases: ["서울특별시"], points: "102,106 134,104 140,124 128,132 104,130 98,118", lx: 119, ly: 117 },
      { id: "충남", fullName: "충청남도", aliases: ["충청남도"], points: "92,172 136,180 144,202 164,238 156,262 88,266 68,228 46,192", lx: 95, ly: 222 },
      { id: "충북", fullName: "충청북도", aliases: ["충청북도"], points: "152,172 180,152 224,158 246,190 214,220 194,258 182,262 164,238 144,202", lx: 192, ly: 198 },
      { id: "세종", fullName: "세종특별자치시", aliases: ["세종특별자치시"], points: "128,196 146,196 150,218 130,218", lx: 139, ly: 207 },
      { id: "대전", fullName: "대전광역시", aliases: ["대전광역시"], points: "138,224 162,224 164,246 140,246", lx: 151, ly: 235 },
      { id: "경북", fullName: "경상북도", aliases: ["경상북도"], points: "224,158 292,168 324,174 338,266 328,300 274,304 196,284 186,262 214,220 246,190", lx: 266, ly: 222 },
      { id: "대구", fullName: "대구광역시", aliases: ["대구광역시"], points: "238,262 270,262 272,288 238,288", lx: 255, ly: 275 },
      { id: "전북", fullName: "전라북도", aliases: ["전라북도", "전북특별자치도"], points: "88,266 156,262 186,262 170,324 112,328 68,324 76,294", lx: 124, ly: 294 },
      { id: "전남", fullName: "전라남도", aliases: ["전라남도"], points: "68,324 112,328 170,324 184,344 184,392 138,420 68,428 50,380", lx: 122, ly: 388 },
      { id: "광주", fullName: "광주광역시", aliases: ["광주광역시"], points: "92,336 126,336 128,360 92,360", lx: 110, ly: 348 },
      { id: "경남", fullName: "경상남도", aliases: ["경상남도"], points: "186,262 196,284 274,304 300,304 298,334 282,354 238,382 184,392 184,344 170,324", lx: 232, ly: 336 },
      { id: "울산", fullName: "울산광역시", aliases: ["울산광역시"], points: "298,298 330,298 328,328 298,328", lx: 314, ly: 313 },
      { id: "부산", fullName: "부산광역시", aliases: ["부산광역시"], points: "282,336 318,336 314,364 278,364", lx: 298, ly: 350 },
      { id: "제주", fullName: "제주특별자치도", aliases: ["제주특별자치도"], points: "68,452 128,448 136,466 116,478 66,474 58,462", lx: 98, ly: 463 }
    ];

    const state = {
      isLocalCsv: false,
      meta: PUBLIC_EMBEDDED_DATA.meta,
      records: PUBLIC_EMBEDDED_DATA.records,
      selectedSubmission: "둘 다 입력",
      selectedRegion: null,
      selectedHobby: null,
      hobbyShowAll: false,
      selectedProfileIndex: 0
    };

    function escapeHtml(str) {
      return String(str ?? "")
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#39;");
    }

    function isValidHttpUrl(value) {
      const trimmed = String(value || "").trim();
      if (!trimmed || /\s/.test(trimmed)) return false;
      if (!/^https?:\/\//i.test(trimmed)) return false;
      try {
        const u = new URL(trimmed);
        return (u.protocol === "http:" || u.protocol === "https:") && Boolean(u.hostname) && u.hostname.includes(".");
      } catch {
        return false;
      }
    }

    function classifyProvince(rawAddress) {
      const trimmed = String(rawAddress || "").trim();
      if (!trimmed) return "미입력";
      const first = trimmed.split(/\s+/)[0];
      const matched = PROVINCE_PREFIXES.find(p => first === p || trimmed.startsWith(p + " "));
      return matched || "분류 확인 필요";
    }

    function getSubmissionCategory(rec) {
      const hasGh = Boolean(String(rec.github || "").trim());
      const hasPg = Boolean(String(rec.pages || "").trim());
      if (hasGh && hasPg) return "둘 다 입력";
      if (hasGh && !hasPg) return "GitHub만 입력";
      if (!hasGh && hasPg) return "제출 페이지만 입력";
      return "둘 다 미입력";
    }

    function extractHobbies(hobbyStr) {
      const raw = String(hobbyStr || "").trim();
      if (!raw) return [];
      const seen = new Set();
      const result = [];
      for (const part of raw.split(",")) {
        const item = part.trim();
        if (item && !seen.has(item)) {
          seen.add(item);
          result.push(item);
        }
      }
      return result;
    }

    function parseCsvRows(text) {
      const clean = text.replace(/^\uFEFF/, "");
      const rows = [];
      let row = [];
      let field = "";
      let inQuotes = false;

      for (let i = 0; i < clean.length; i++) {
        const ch = clean[i];
        const next = clean[i + 1];
        if (inQuotes) {
          if (ch === '"' && next === '"') {
            field += '"';
            i++;
          } else if (ch === '"') {
            inQuotes = false;
          } else {
            field += ch;
          }
        } else {
          if (ch === '"') {
            inQuotes = true;
          } else if (ch === ",") {
            row.push(field);
            field = "";
          } else if (ch === "\r" && next === "\n") {
            row.push(field);
            rows.push(row);
            row = [];
            field = "";
            i++;
          } else if (ch === "\n" || ch === "\r") {
            row.push(field);
            rows.push(row);
            row = [];
            field = "";
          } else {
            field += ch;
          }
        }
      }
      if (field.length > 0 || row.length > 0) {
        row.push(field);
        rows.push(row);
      }
      return rows;
    }

    function parseUploadedCsv(text, filename) {
      const rows = parseCsvRows(text);
      const isBlankRow = r => r.every(cell => String(cell || "").trim() === "");

      const headerIdx = rows.findIndex(r => {
        const c0 = String(r[0] || "").trim();
        return c0 === "이름" || r.some(c => String(c || "").includes("GitHub 주소"));
      });

      if (headerIdx === -1) {
        throw new Error("‘이름’ 헤더 행을 찾을 수 없습니다. 첨부된 입력표 형식의 CSV인지 확인해 주세요.");
      }

      const header = rows[headerIdx].map(c => String(c || "").trim());
      const findCol = (keywords, fallback) => {
        const idx = header.findIndex(h => keywords.some(k => h.includes(k)));
        return idx !== -1 ? idx : fallback;
      };

      const colName = findCol(["이름"], 0);
      const colGithub = findCol(["GitHub"], 1);
      const colPages = findCol(["제출 페이지", "Pages"], 2);
      const colInsta = findCol(["인스타"], 3);
      const colHobby = findCol(["취미"], 4);
      const colAddr = findCol(["집주소", "주소", "거주"], 5);
      const colFav = findCol(["아끼는"], 6);
      const colGender = findCol(["성별"], 7);

      let preHeaderRows = 0;
      let blankRows = 0;
      let missingNameRows = 0;

      for (let i = 0; i < headerIdx; i++) {
        if (isBlankRow(rows[i])) blankRows++;
        else preHeaderRows++;
      }

      const records = [];
      for (let i = headerIdx + 1; i < rows.length; i++) {
        const r = rows[i];
        if (isBlankRow(r)) {
          blankRows++;
          continue;
        }
        const name = String(r[colName] || "").trim();
        if (!name) {
          missingNameRows++;
          continue;
        }
        const addrRaw = String(r[colAddr] || "").trim();
        records.push({
          name,
          github: String(r[colGithub] || "").trim(),
          pages: String(r[colPages] || "").trim(),
          instagram: String(r[colInsta] || "").trim(),
          hobby: String(r[colHobby] || "").trim(),
          regionProvince: classifyProvince(addrRaw),
          addressRaw: addrRaw,
          favorite: String(r[colFav] || "").trim(),
          genderPresent: Boolean(String(r[colGender] || "").trim())
        });
      }

      return {
        meta: {
          preHeaderRows,
          blankRows,
          missingNameRows,
          sourceLabel: `로컬 CSV 미리보기 · ${filename}`
        },
        records
      };
    }

    function renderSummaryRings(ghRatio, pgRatio, bothRatio) {
      const svg = document.getElementById("summaryRingSvg");
      const rings = [
        { r: 41, ratio: ghRatio, color: "#16d1b6" },
        { r: 30, ratio: pgRatio, color: "#18b0c4" },
        { r: 19, ratio: bothRatio, color: "#5658d6" }
      ];
      svg.innerHTML = rings.map(item => {
        const c = 2 * Math.PI * item.r;
        const dash = c * Math.max(0, Math.min(1, item.ratio));
        return `
          <circle cx="48" cy="48" r="${item.r}" fill="none" stroke="#e9eff5" stroke-width="6.5" />
          <circle cx="48" cy="48" r="${item.r}" fill="none" stroke="${item.color}" stroke-width="6.5"
            stroke-linecap="round" stroke-dasharray="${dash.toFixed(1)} ${c.toFixed(1)}"
            transform="rotate(-90 48 48)" />
        `;
      }).join("");
    }

    function renderDotMatrix(records) {
      const container = document.getElementById("dotMatrix");
      container.innerHTML = "";
      let gg = 0, sl = 0, etc = 0, unk = 0;

      records.forEach((rec, idx) => {
        const p = rec.regionProvince;
        let color = "#18b0c4";
        if (p === "경기도") { color = "#5658d6"; gg++; }
        else if (p === "서울특별시") { color = "#16d1b6"; sl++; }
        else if (p === "분류 확인 필요" || p === "미입력") { color = "#ef5757"; unk++; }
        else { etc++; }

        const dot = document.createElement("button");
        dot.type = "button";
        dot.className = "resp-dot";
        dot.style.background = color;
        dot.title = `${rec.name} (${p})`;
        dot.setAttribute("aria-label", `${rec.name} 프로필 보기 (${p})`);
        dot.addEventListener("click", () => {
          state.selectedProfileIndex = idx;
          activateTab("profile");
          renderProfile();
        });
        container.appendChild(dot);
      });

      const total = records.length || 1;
      const metroPct = (((gg + sl) / total) * 100).toFixed(1);
      document.getElementById("metroBig").textContent = `${metroPct}%`;
      document.getElementById("dotStatGg").textContent = `${gg}명`;
      document.getElementById("dotStatSl").textContent = `${sl}명`;
      document.getElementById("dotStatEtc").textContent = `${etc}명`;
      document.getElementById("dotStatUnk").textContent = `${unk}명`;
    }

    function renderAll() {
      const records = state.records;
      const total = records.length;
      const ghCount = records.filter(r => String(r.github || "").trim() !== "").length;
      const pgCount = records.filter(r => String(r.pages || "").trim() !== "").length;
      const bothCount = records.filter(r => getSubmissionCategory(r) === "둘 다 입력").length;
      const ghOnlyCount = records.filter(r => getSubmissionCategory(r) === "GitHub만 입력").length;
      const ghInvalidUrl = records.filter(r => String(r.github || "").trim() !== "" && !isValidHttpUrl(r.github)).length;
      const pgInvalidUrl = records.filter(r => String(r.pages || "").trim() !== "" && !isValidHttpUrl(r.pages)).length;
      const warnUrlCount = ghInvalidUrl + pgInvalidUrl;

      const badge = document.getElementById("modeBadge");
      badge.textContent = state.meta.sourceLabel;
      badge.classList.toggle("local", state.isLocalCsv);
      document.getElementById("resetCsvBtn").hidden = !state.isLocalCsv;

      document.getElementById("heroLead").textContent =
        `유효 응답자 ${total}명 중 GitHub 주소는 ${ghCount}명, 제출 페이지 주소는 ${pgCount}명이 입력했습니다. ` +
        `입력 현황·거주 시·도 지도·취미 분포·이름별 프로필·누락 점검을 한 화면에서 탐색하세요.`;

      document.getElementById("statTotal").innerHTML = `${total}<small>명</small>`;
      document.getElementById("statGithub").innerHTML = `${ghCount}<small>명</small>`;
      document.getElementById("statPages").innerHTML = `${pgCount}<small>명</small>`;

      const ghRatio = total ? ghCount / total : 0;
      const pgRatio = total ? pgCount / total : 0;
      const bothRatio = total ? bothCount / total : 0;
      document.getElementById("ringPctGh").textContent = `${(ghRatio * 100).toFixed(1)}%`;
      document.getElementById("ringPctPg").textContent = `${(pgRatio * 100).toFixed(1)}%`;
      document.getElementById("ringPctBoth").textContent = `${(bothRatio * 100).toFixed(1)}%`;
      renderSummaryRings(ghRatio, pgRatio, bothRatio);

      // Card 2 bars
      document.getElementById("miniBothTxt").textContent = `${bothCount}명 / ${total}명`;
      document.getElementById("miniBothBar").style.width = `${total ? (bothCount / total) * 100 : 0}%`;
      document.getElementById("miniGhOnlyTxt").textContent = `${ghOnlyCount}명 / ${total}명`;
      document.getElementById("miniGhOnlyBar").style.width = `${total ? (ghOnlyCount / total) * 100 : 0}%`;
      document.getElementById("miniWarnTxt").textContent = `${warnUrlCount}건`;
      document.getElementById("miniWarnBar").style.width = `${total ? Math.min(100, (warnUrlCount / total) * 100) : 0}%`;

      document.getElementById("summaryPills").innerHTML = `
        <span class="pill-tag">정상 URL 형식 ${ghCount + pgCount - warnUrlCount}건</span>
        <span class="pill-tag coral">형식 확인 필요 ${warnUrlCount}건</span>
      `;

      renderDotMatrix(records);

      document.getElementById("criteriaSummary").textContent =
        `집계 기준: 헤더 이후 ‘이름’이 비어 있지 않은 ${total}개 행 집계 (헤더 이전 안내 ${state.meta.preHeaderRows}행 · 빈/공백 행 ${state.meta.blankRows}행 · 이름 누락 ${state.meta.missingNameRows}행 제외). ` +
        `입력 여부만 집계하며 실제 링크 작동 여부는 확인하지 않았습니다.`;

      renderSubmission();
      renderRegion();
      renderHobby();
      renderProfileFilters();
      renderProfile();
      renderAudit();
    }

    function renderBars(container, items, totalBase, selectedKey, onSelect) {
      container.innerHTML = "";
      if (!items.length) {
        container.innerHTML = `<p class="box-desc">표시할 항목이 없습니다.</p>`;
        return;
      }
      const maxCount = Math.max(...items.map(it => it.count), 1);
      for (const item of items) {
        const btn = document.createElement("button");
        btn.type = "button";
        btn.className = "bar-item";
        btn.setAttribute("aria-pressed", String(item.key === selectedKey));
        const pct = totalBase ? ((item.count / totalBase) * 100).toFixed(1) : "0.0";
        const widthPct = Math.max((item.count / maxCount) * 100, item.count > 0 ? 4 : 0);
        btn.innerHTML = `
          <div class="bar-meta">
            <span>${escapeHtml(item.key)}</span>
            <span class="bar-count">${item.count}명 (${pct}%)</span>
          </div>
          <div class="bar-track" aria-hidden="true">
            <div class="bar-fill" style="width: ${widthPct}%"></div>
          </div>
        `;
        btn.addEventListener("click", () => onSelect(item.key));
        container.appendChild(btn);
      }
    }

    function renderNameChips(container, names) {
      container.innerHTML = "";
      if (!names.length) {
        container.innerHTML = `<p class="box-desc">해당하는 응답자가 없습니다.</p>`;
        return;
      }
      for (const name of names) {
        const chip = document.createElement("button");
        chip.type = "button";
        chip.className = "name-chip";
        chip.textContent = name;
        chip.title = `${name} 프로필 보기`;
        chip.addEventListener("click", () => {
          const idx = state.records.findIndex(r => r.name === name);
          if (idx !== -1) {
            state.selectedProfileIndex = idx;
            document.getElementById("profileSearch").value = "";
            document.getElementById("profileStatusFilter").value = "ALL";
            document.getElementById("profileRegionFilter").value = "ALL";
            activateTab("profile");
            renderProfile();
          }
        });
        container.appendChild(chip);
      }
    }

    function renderSubmission() {
      const order = ["둘 다 입력", "GitHub만 입력", "제출 페이지만 입력", "둘 다 미입력"];
      const groups = new Map(order.map(k => [k, []]));
      for (const rec of state.records) {
        groups.get(getSubmissionCategory(rec)).push(rec.name);
      }
      const items = order.map(k => ({ key: k, count: groups.get(k).length }));
      if (!order.includes(state.selectedSubmission)) state.selectedSubmission = order[0];

      renderBars(
        document.getElementById("submissionBars"),
        items,
        state.records.length,
        state.selectedSubmission,
        key => { state.selectedSubmission = key; renderSubmission(); }
      );

      const selectedNames = groups.get(state.selectedSubmission) || [];
      document.getElementById("submissionDetailTitle").textContent = `‘${state.selectedSubmission}’ 명단 (${selectedNames.length}명)`;
      document.getElementById("submissionDetailDesc").textContent = "이름을 누르면 해당 응답자의 프로필 탭으로 이동합니다.";
      renderNameChips(document.getElementById("submissionDetailList"), selectedNames);
    }

    function getMapFillColor(count) {
      if (count >= 6) return "#5658d6";
      if (count >= 3) return "#37d1bf";
      if (count >= 1) return "#a8ede3";
      return "#edf2f7";
    }

    function renderKoreaMap(groups) {
      const svg = document.getElementById("koreaMapSvg");
      svg.innerHTML = "";
      const ns = "http://www.w3.org/2000/svg";

      for (const reg of KOREA_MAP_REGIONS) {
        const matchedAliases = reg.aliases.filter(a => groups.has(a));
        const names = matchedAliases.flatMap(a => groups.get(a));
        const count = names.length;
        const activeKey = matchedAliases[0] || reg.fullName;
        const isSelected = reg.aliases.includes(state.selectedRegion) || state.selectedRegion === reg.fullName;

        const g = document.createElementNS(ns, "g");
        g.setAttribute("class", `map-region${isSelected ? " selected" : ""}`);
        g.setAttribute("role", "button");
        g.setAttribute("tabindex", "0");
        g.setAttribute("aria-pressed", String(isSelected));
        g.setAttribute("aria-label", `${reg.fullName} ${count}명`);

        const titleEl = document.createElementNS(ns, "title");
        titleEl.textContent = `${reg.fullName}: ${count}명`;
        g.appendChild(titleEl);

        const poly = document.createElementNS(ns, "polygon");
        poly.setAttribute("class", "region-shape");
        poly.setAttribute("points", reg.points);
        poly.setAttribute("fill", getMapFillColor(count));
        g.appendChild(poly);

        const isDark = count >= 6;
        const label = document.createElementNS(ns, "text");
        label.setAttribute("class", `map-label${isDark ? " on-dark" : ""}`);
        label.setAttribute("x", String(reg.lx));
        label.setAttribute("y", String(count > 0 ? reg.ly - 5 : reg.ly));
        label.textContent = reg.id;
        g.appendChild(label);

        if (count > 0) {
          const sub = document.createElementNS(ns, "text");
          sub.setAttribute("class", `map-subcount${isDark ? " on-dark" : ""}`);
          sub.setAttribute("x", String(reg.lx));
          sub.setAttribute("y", String(reg.ly + 6));
          sub.textContent = `${count}명`;
          g.appendChild(sub);
        }

        const selectThis = () => {
          state.selectedRegion = activeKey;
          renderRegion();
        };
        g.addEventListener("click", selectThis);
        g.addEventListener("keydown", e => {
          if (e.key === "Enter" || e.key === " ") {
            e.preventDefault();
            selectThis();
          }
        });

        svg.appendChild(g);
      }

      const unplacedRow = document.getElementById("unplacedRegionRow");
      const unplacedNames = groups.get("분류 확인 필요") || [];
      if (unplacedNames.length > 0) {
        unplacedRow.hidden = false;
        const isSel = state.selectedRegion === "분류 확인 필요";
        unplacedRow.innerHTML = `
          <span>시·도 미기재로 지도에 임의 배치하지 않은 인원</span>
          <button type="button" class="unplaced-btn" aria-pressed="${isSel}">
            분류 확인 필요 (${unplacedNames.length}명)
          </button>
        `;
        unplacedRow.querySelector("button").addEventListener("click", () => {
          state.selectedRegion = "분류 확인 필요";
          renderRegion();
        });
      } else {
        unplacedRow.hidden = true;
      }
    }

    function renderRegion() {
      const groups = new Map();
      for (const rec of state.records) {
        const key = rec.regionProvince || "미입력";
        if (!groups.has(key)) groups.set(key, []);
        groups.get(key).push(rec.name);
      }
      const items = [...groups.entries()]
        .map(([key, names]) => ({ key, count: names.length, names }))
        .sort((a, b) => b.count - a.count || a.key.localeCompare(b.key, "ko"));

      if (!state.selectedRegion) {
        state.selectedRegion = items[0]?.key || null;
      }

      renderKoreaMap(groups);
      renderBars(
        document.getElementById("regionBars"),
        items,
        state.records.length,
        state.selectedRegion,
        key => { state.selectedRegion = key; renderRegion(); }
      );

      const selectedNames = groups.get(state.selectedRegion) || [];
      document.getElementById("regionDetailTitle").textContent = `‘${state.selectedRegion || "-"}’ 명단 (${selectedNames.length}명)`;
      document.getElementById("regionDetailDesc").textContent =
        state.selectedRegion === "분류 확인 필요"
          ? "시·도명이 생략되어 지도에 임의 배치하지 않고 별도 분류한 인원입니다."
          : selectedNames.length === 0
            ? "해당 시·도에는 입력된 응답자가 없습니다."
            : "세부 동·구 주소는 공개 화면에 표시하지 않습니다.";
      renderNameChips(document.getElementById("regionDetailList"), selectedNames);
    }

    function renderHobby() {
      const groups = new Map();
      for (const rec of state.records) {
        for (const h of extractHobbies(rec.hobby)) {
          if (!groups.has(h)) groups.set(h, []);
          groups.get(h).push(rec.name);
        }
      }

      const query = document.getElementById("hobbySearch").value.trim().toLowerCase();
      let items = [...groups.entries()]
        .map(([key, names]) => ({ key, count: names.length, names }))
        .sort((a, b) => b.count - a.count || a.key.localeCompare(b.key, "ko"));

      if (query) {
        items = items.filter(it => it.key.toLowerCase().includes(query));
      }

      const toggleBtn = document.getElementById("hobbyTopToggle");
      toggleBtn.textContent = state.hobbyShowAll ? "상위 10개만 보기" : `전체 보기 (${items.length}개)`;
      toggleBtn.setAttribute("aria-pressed", String(state.hobbyShowAll));

      const visibleItems = (!query && !state.hobbyShowAll) ? items.slice(0, 10) : items;

      if (!state.selectedHobby || !groups.has(state.selectedHobby)) {
        state.selectedHobby = visibleItems[0]?.key || items[0]?.key || null;
      }

      renderBars(
        document.getElementById("hobbyBars"),
        visibleItems,
        state.records.length,
        state.selectedHobby,
        key => { state.selectedHobby = key; renderHobby(); }
      );

      const selectedNames = groups.get(state.selectedHobby) || [];
      document.getElementById("hobbyDetailTitle").textContent =
        state.selectedHobby ? `‘${state.selectedHobby}’ 입력자 (${selectedNames.length}명)` : "선택된 취미 없음";
      document.getElementById("hobbyDetailDesc").textContent = "응답자 전체 인원 대비 해당 취미를 입력한 인원입니다.";
      renderNameChips(document.getElementById("hobbyDetailList"), selectedNames);
    }

    function renderProfileFilters() {
      const regionSel = document.getElementById("profileRegionFilter");
      const prev = regionSel.value;
      const provinces = [...new Set(state.records.map(r => r.regionProvince))].sort((a, b) => a.localeCompare(b, "ko"));
      regionSel.innerHTML = `<option value="ALL">거주 지역: 전체</option>` +
        provinces.map(p => `<option value="${escapeHtml(p)}">${escapeHtml(p)}</option>`).join("");
      if (provinces.includes(prev)) regionSel.value = prev;
    }

    function renderFieldWithOptionalUrl(rawValue) {
      const val = String(rawValue || "").trim();
      if (!val) return `<span class="tag">미입력</span>`;
      if (isValidHttpUrl(val)) {
        const safe = escapeHtml(val);
        return `<a class="ext-link" href="${safe}" target="_blank" rel="noopener noreferrer">${safe}</a>`;
      }
      if (/github\.com|https?:\/\//i.test(val)) {
        return `${escapeHtml(val)} <span class="tag tag-warn">URL 형식 확인 필요 · 링크 비활성</span>`;
      }
      return `${escapeHtml(val)} <span class="tag">일반 텍스트</span>`;
    }

    function renderProfile() {
      const q = document.getElementById("profileSearch").value.trim().toLowerCase();
      const statusF = document.getElementById("profileStatusFilter").value;
      const regionF = document.getElementById("profileRegionFilter").value;

      const indexed = state.records
        .map((rec, idx) => ({ rec, idx }))
        .filter(({ rec }) => {
          if (q && !rec.name.toLowerCase().includes(q)) return false;
          if (statusF !== "ALL" && getSubmissionCategory(rec) !== statusF) return false;
          if (regionF !== "ALL" && rec.regionProvince !== regionF) return false;
          return true;
        });

      const roster = document.getElementById("rosterList");
      roster.innerHTML = "";
      if (!indexed.length) {
        roster.innerHTML = `<p class="box-desc">검색 조건에 맞는 이름이 없습니다.</p>`;
      } else {
        if (!indexed.some(item => item.idx === state.selectedProfileIndex)) {
          state.selectedProfileIndex = indexed[0].idx;
        }
        for (const { rec, idx } of indexed) {
          const btn = document.createElement("button");
          btn.type = "button";
          btn.className = "roster-btn";
          btn.setAttribute("aria-pressed", String(idx === state.selectedProfileIndex));
          btn.innerHTML = `
            <strong>${escapeHtml(rec.name)}</strong>
            <span class="roster-sub">${escapeHtml(getSubmissionCategory(rec))} · ${escapeHtml(rec.regionProvince)}</span>
          `;
          btn.addEventListener("click", () => {
            state.selectedProfileIndex = idx;
            renderProfile();
          });
          roster.appendChild(btn);
        }
      }

      const selected = state.records[state.selectedProfileIndex];
      const nameEl = document.getElementById("profileDetailName");
      const subEl = document.getElementById("profileDetailSub");
      const bodyEl = document.getElementById("profileDetailBody");
      const noticeEl = document.getElementById("profilePrivacyNotice");

      if (!selected) {
        nameEl.textContent = "프로필 상세";
        subEl.textContent = "선택된 응답자가 없습니다.";
        bodyEl.innerHTML = "";
        noticeEl.textContent = "";
        return;
      }

      nameEl.textContent = selected.name;
      subEl.textContent = `${getSubmissionCategory(selected)} · URL 형식이 확인된 값만 링크로 열립니다.`;

      const regionDisplay = state.isLocalCsv
        ? `${escapeHtml(selected.addressRaw || "미입력")} <span class="tag">${escapeHtml(selected.regionProvince)}</span>`
        : `${escapeHtml(selected.regionProvince)} <span class="tag">세부 동·구 공개 비노출</span>`;

      const favoriteDisplay = state.isLocalCsv
        ? (selected.favorite ? escapeHtml(selected.favorite) : `<span class="tag">미입력</span>`)
        : `<span class="tag">공개 화면 비노출 (사적 답변)</span>`;

      bodyEl.innerHTML = `
        <dl class="profile-dl">
          <dt>GitHub 주소</dt>
          <dd>${renderFieldWithOptionalUrl(selected.github)}</dd>
          <dt>제출 페이지</dt>
          <dd>${renderFieldWithOptionalUrl(selected.pages)}</dd>
          <dt>인스타그램</dt>
          <dd>${renderFieldWithOptionalUrl(selected.instagram)}</dd>
          <dt>취미</dt>
          <dd>${selected.hobby ? escapeHtml(selected.hobby) : `<span class="tag">미입력</span>`}</dd>
          <dt>거주지</dt>
          <dd>${regionDisplay}</dd>
          <dt>가장 아끼는 것</dt>
          <dd>${favoriteDisplay}</dd>
        </dl>
      `;

      noticeEl.textContent = state.isLocalCsv
        ? "현재 브라우저에서 직접 연 로컬 CSV 데이터를 표시 중입니다(외부 서버 전송 없음). 공개 게시용 화면에서는 상세 주소와 사적 답변이 제외됩니다."
        : "공개 게시 안전 모드입니다. 세부 동·구 주소와 사적 답변(‘가장 아끼는 것’ 등)은 공개 HTML 소스에 포함되지 않으며, 상단 ‘CSV 열기’로 로컬 파일을 열면 본인 화면에서만 전체 내용을 확인할 수 있습니다.";
    }

    function renderAudit() {
      const n = state.records.length;
      const countPresent = fn => state.records.filter(fn).length;

      const rows = [
        { label: "이름", present: countPresent(r => Boolean(r.name)), note: "필수 식별 기준" },
        { label: "GitHub 주소", present: countPresent(r => Boolean(r.github)), note: "스킴 누락 등 형식 주의 포함" },
        { label: "제출 페이지 주소", present: countPresent(r => Boolean(r.pages)), note: "공란 1건, 문장 포함 URL 1건" },
        { label: "인스타그램", present: countPresent(r => Boolean(r.instagram)), note: "아이디 문자열·URL 혼재" },
        { label: "취미", present: countPresent(r => Boolean(r.hobby)), note: "쉼표 구분 다중 입력" },
        { label: "집주소 (동까지)", present: countPresent(r => r.regionProvince !== "미입력"), note: "시·도 미기재 2건" },
        { label: "가장 아끼는 것", present: n, note: "공개 모드에서는 내용 비노출" },
        { label: "성별", present: countPresent(r => r.genderPresent), note: "자유 서술 포함 · 통계 제외" }
      ];

      document.getElementById("auditTableBody").innerHTML = rows.map(r => `
        <tr>
          <td>${escapeHtml(r.label)}</td>
          <td class="num">${r.present}명</td>
          <td class="num">${n - r.present}건</td>
          <td>${escapeHtml(r.note)}</td>
        </tr>
      `).join("");

      document.getElementById("auditNotes").innerHTML = `
        <p class="box-desc"><strong>1. 집계 제외 행</strong></p>
        <ul style="margin:0 0 14px; padding-left:18px; color:var(--muted); font-size:0.88rem;">
          <li>헤더(‘이름’) 이전 안내 행: ${state.meta.preHeaderRows}행</li>
          <li>모든 칸이 비어 있거나 공백만 있는 행: ${state.meta.blankRows}행</li>
          <li>이름이 누락된 데이터 행: ${state.meta.missingNameRows}행</li>
        </ul>
        <p class="box-desc"><strong>2. 통계화하지 않은 컬럼</strong></p>
        <p class="box-desc" style="margin-bottom:0;">
          ‘성별’ 컬럼에는 표준 성별 표기 외에 자유 서술·별칭 값(예: 동물, 별명 등)이 섞여 있어 일관된 분류가 불가능하므로 성별 분포 차트를 만들지 않았습니다.
        </p>
      `;
    }

    const tabButtons = [...document.querySelectorAll(".tab-btn")];
    function activateTab(tabKey) {
      for (const btn of tabButtons) {
        const active = btn.dataset.tab === tabKey;
        btn.setAttribute("aria-selected", String(active));
        document.getElementById(`panel-${btn.dataset.tab}`).classList.toggle("active", active);
      }
    }
    tabButtons.forEach((btn, idx) => {
      btn.addEventListener("click", () => activateTab(btn.dataset.tab));
      btn.addEventListener("keydown", e => {
        if (e.key === "ArrowRight") {
          e.preventDefault();
          const next = tabButtons[(idx + 1) % tabButtons.length];
          next.focus();
          activateTab(next.dataset.tab);
        } else if (e.key === "ArrowLeft") {
          e.preventDefault();
          const prev = tabButtons[(idx - 1 + tabButtons.length) % tabButtons.length];
          prev.focus();
          activateTab(prev.dataset.tab);
        }
      });
    });

    document.getElementById("hobbySearch").addEventListener("input", renderHobby);
    document.getElementById("hobbyTopToggle").addEventListener("click", () => {
      state.hobbyShowAll = !state.hobbyShowAll;
      renderHobby();
    });
    document.getElementById("profileSearch").addEventListener("input", renderProfile);
    document.getElementById("profileStatusFilter").addEventListener("change", renderProfile);
    document.getElementById("profileRegionFilter").addEventListener("change", renderProfile);

    const fileInput = document.getElementById("csvFileInput");
    document.getElementById("openCsvBtn").addEventListener("click", () => fileInput.click());
    fileInput.addEventListener("change", async e => {
      const file = e.target.files?.[0];
      if (!file) return;
      try {
        const text = await file.text();
        const parsed = parseUploadedCsv(text, file.name);
        state.isLocalCsv = true;
        state.meta = parsed.meta;
        state.records = parsed.records;
        state.selectedRegion = null;
        state.selectedHobby = null;
        state.selectedProfileIndex = 0;
        renderAll();
      } catch (err) {
        alert(err.message || "CSV 파일을 읽는 중 오류가 발생했습니다.");
      } finally {
        fileInput.value = "";
      }
    });

    document.getElementById("resetCsvBtn").addEventListener("click", () => {
      state.isLocalCsv = false;
      state.meta = PUBLIC_EMBEDDED_DATA.meta;
      state.records = PUBLIC_EMBEDDED_DATA.records;
      state.selectedRegion = null;
      state.selectedHobby = null;
      state.selectedProfileIndex = 0;
      renderAll();
    });

    renderAll();
  </script>
</body>
</html>
