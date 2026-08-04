
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="color-scheme" content="light" />
  <meta name="theme-color" content="#f5f6f8" />
  <title>Three · 罗思维</title>
  <style>
    :root {
      --bg: #f5f5f7;
      --surface: rgba(255, 255, 255, 0.9);
      --surface-solid: #ffffff;
      --text: #1d1d1f;
      --secondary: #6e6e73;
      --tertiary: #86868b;
      --line: rgba(0, 0, 0, 0.08);
      --line-strong: rgba(0, 0, 0, 0.14);
      --blue: #0071e3;
      --green: #24a148;
      --purple: #6757d9;
      --gold: #9a6a12;
      --danger: #d92d20;
      --shadow: 0 24px 70px rgba(0, 0, 0, 0.07);
      --shadow-soft: 0 12px 36px rgba(0, 0, 0, 0.045);
      --radius-xl: 30px;
      --radius-lg: 24px;
      --radius-md: 16px;
      --radius-sm: 12px;
      --page: 1180px;
      --ease: cubic-bezier(.2, .8, .2, 1);
    }

    * { box-sizing: border-box; }

    html {
      background: var(--bg);
      scroll-behavior: smooth;
    }

    body {
      margin: 0;
      min-height: 100vh;
      color: var(--text);
      background:
        radial-gradient(circle at 5% -10%, rgba(0, 113, 227, .07), transparent 28%),
        radial-gradient(circle at 96% 4%, rgba(103, 87, 217, .065), transparent 24%),
        var(--bg);
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text",
        "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
      -webkit-font-smoothing: antialiased;
      text-rendering: optimizeLegibility;
    }

    button, input, select, textarea { font: inherit; }

    button {
      border: 0;
      cursor: pointer;
    }

    .container {
      width: min(calc(100% - 40px), var(--page));
      margin: 0 auto;
    }

    .app {
      opacity: 0;
      transform: translateY(8px);
      transition: opacity .5s var(--ease), transform .5s var(--ease);
    }

    .app.ready {
      opacity: 1;
      transform: none;
    }

    .topbar {
      position: sticky;
      top: 0;
      z-index: 40;
      background: rgba(245, 245, 247, .78);
      backdrop-filter: blur(24px) saturate(160%);
      -webkit-backdrop-filter: blur(24px) saturate(160%);
      border-bottom: 1px solid transparent;
      transition: border-color .2s ease, box-shadow .2s ease;
    }

    .topbar.scrolled {
      border-bottom-color: var(--line);
      box-shadow: 0 8px 28px rgba(0, 0, 0, .025);
    }

    .topbar-inner {
      height: 68px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 20px;
    }

    .brand {
      display: inline-flex;
      align-items: center;
      gap: 11px;
      font-size: 13px;
      font-weight: 760;
      letter-spacing: .13em;
      text-transform: uppercase;
    }

    .brand-mark {
      width: 24px;
      height: 24px;
      border-radius: 8px;
      background: linear-gradient(135deg, var(--blue) 0 33%, var(--green) 33% 66%, var(--purple) 66%);
      box-shadow: inset 0 0 0 1px rgba(255, 255, 255, .5);
    }

    .top-actions {
      display: flex;
      align-items: center;
      gap: 9px;
    }

    .milestone-pill {
      min-height: 38px;
      padding: 0 14px;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      border-radius: 999px;
      color: var(--text);
      background: rgba(255, 255, 255, .8);
      border: 1px solid var(--line);
      font-size: 13px;
      font-weight: 650;
      transition: transform .2s var(--ease), background .2s ease;
    }

    .milestone-pill:hover {
      transform: translateY(-1px);
      background: white;
    }

    .milestone-pill strong {
      min-width: 22px;
      height: 22px;
      padding: 0 7px;
      display: inline-grid;
      place-items: center;
      border-radius: 999px;
      color: white;
      background: var(--text);
      font-size: 11px;
    }

    .icon-button {
      width: 38px;
      height: 38px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      background: rgba(255, 255, 255, .8);
      border: 1px solid var(--line);
      color: var(--text);
      transition: transform .2s var(--ease), background .2s ease;
    }

    .icon-button:hover {
      transform: translateY(-1px);
      background: white;
    }

    .icon-button svg {
      width: 18px;
      height: 18px;
      fill: none;
      stroke: currentColor;
      stroke-width: 1.8;
      stroke-linecap: round;
      stroke-linejoin: round;
    }

    main { padding: 74px 0 88px; }

    .hero {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 36px;
      align-items: end;
      margin-bottom: 42px;
    }

    .eyebrow {
      margin-bottom: 13px;
      color: var(--secondary);
      font-size: 13px;
      font-weight: 700;
      letter-spacing: .1em;
      text-transform: uppercase;
    }

    h1 {
      margin: 0;
      font-size: clamp(48px, 8vw, 84px);
      line-height: .96;
      letter-spacing: -.06em;
      font-weight: 720;
    }

    .hero-copy {
      max-width: 660px;
      margin: 22px 0 0;
      color: var(--secondary);
      font-size: clamp(17px, 2.2vw, 21px);
      line-height: 1.58;
      letter-spacing: -.02em;
    }

    .hero-date {
      padding-bottom: 5px;
      color: var(--secondary);
      font-size: 14px;
      text-align: right;
      white-space: nowrap;
    }

    .reward-summary {
      margin-bottom: 22px;
      padding: 24px 28px;
      display: grid;
      grid-template-columns: 1fr auto;
      align-items: center;
      gap: 26px;
      border-radius: var(--radius-lg);
      background: var(--surface);
      border: 1px solid rgba(255, 255, 255, .9);
      box-shadow: var(--shadow-soft);
      backdrop-filter: blur(22px) saturate(140%);
      -webkit-backdrop-filter: blur(22px) saturate(140%);
    }

    .reward-title {
      font-size: 15px;
      font-weight: 680;
      letter-spacing: -.01em;
    }

    .reward-note {
      margin-top: 6px;
      color: var(--secondary);
      font-size: 13px;
      line-height: 1.45;
    }

    .reward-metrics {
      display: flex;
      align-items: center;
      gap: 26px;
    }

    .reward-metric {
      text-align: right;
    }

    .reward-metric strong {
      display: block;
      font-size: 30px;
      line-height: 1;
      letter-spacing: -.045em;
    }

    .reward-metric span {
      display: block;
      margin-top: 6px;
      color: var(--secondary);
      font-size: 11px;
      white-space: nowrap;
    }

    .reward-button {
      min-height: 40px;
      padding: 0 16px;
      border-radius: 999px;
      color: white;
      background: var(--text);
      font-size: 13px;
      font-weight: 680;
    }

    .reward-button[hidden] { display: none; }

    .cards-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
      margin-bottom: 64px;
    }

    .habit-card {
      position: relative;
      min-height: 390px;
      padding: 28px;
      display: flex;
      flex-direction: column;
      overflow: hidden;
      border-radius: var(--radius-xl);
      background: var(--surface);
      border: 1px solid rgba(255, 255, 255, .92);
      box-shadow: var(--shadow-soft);
      backdrop-filter: blur(24px) saturate(145%);
      -webkit-backdrop-filter: blur(24px) saturate(145%);
      transition: transform .25s var(--ease), box-shadow .25s ease;
    }

    .habit-card:hover {
      transform: translateY(-4px);
      box-shadow: var(--shadow);
    }

    .habit-card::after {
      content: "";
      position: absolute;
      right: -70px;
      bottom: -95px;
      width: 210px;
      height: 210px;
      border-radius: 50%;
      opacity: .075;
      pointer-events: none;
    }

    .habit-card.ielts::after { background: var(--blue); }
    .habit-card.weight::after { background: var(--green); }
    .habit-card.sleep::after { background: var(--purple); }

    .card-head {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
    }

    .card-label {
      color: var(--secondary);
      font-size: 12px;
      font-weight: 760;
      letter-spacing: .12em;
    }

    .card-arrow {
      width: 34px;
      height: 34px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      color: var(--text);
      background: rgba(0, 0, 0, .045);
      font-size: 18px;
      transition: transform .2s var(--ease), background .2s ease;
    }

    .habit-card:hover .card-arrow {
      transform: translateX(2px);
      background: rgba(0, 0, 0, .075);
    }

    .primary-value {
      margin-top: 34px;
      font-size: clamp(50px, 6vw, 70px);
      line-height: .92;
      letter-spacing: -.065em;
      font-weight: 690;
    }

    .primary-value small {
      display: block;
      margin-top: 12px;
      color: var(--secondary);
      font-size: 13px;
      line-height: 1.3;
      letter-spacing: 0;
      font-weight: 560;
    }

    .secondary-data {
      margin-top: 28px;
      padding-top: 20px;
      border-top: 1px solid var(--line);
    }

    .secondary-row {
      display: flex;
      justify-content: space-between;
      gap: 18px;
      align-items: baseline;
      font-size: 13px;
    }

    .secondary-row + .secondary-row { margin-top: 10px; }

    .secondary-row span:first-child { color: var(--secondary); }

    .secondary-row strong {
      text-align: right;
      font-weight: 650;
    }

    .progress-area { margin-top: auto; padding-top: 28px; }

    .progress-top {
      display: flex;
      justify-content: space-between;
      gap: 14px;
      margin-bottom: 10px;
      color: var(--secondary);
      font-size: 12px;
    }

    .progress-top strong {
      color: var(--text);
      font-weight: 680;
    }

    .progress-track {
      height: 8px;
      overflow: hidden;
      border-radius: 999px;
      background: rgba(0, 0, 0, .065);
    }

    .progress-fill {
      width: 0;
      height: 100%;
      border-radius: inherit;
      transition: width .6s var(--ease);
    }

    .ielts .progress-fill { background: var(--blue); }
    .weight .progress-fill { background: var(--green); }
    .sleep .progress-fill { background: var(--purple); }

    .progress-note {
      margin-top: 10px;
      color: var(--secondary);
      font-size: 12px;
      line-height: 1.4;
    }

    .mini-chart {
      width: 100%;
      height: 46px;
      display: block;
      margin-top: 14px;
    }

    .section {
      margin-bottom: 60px;
      scroll-margin-top: 96px;
    }

    .section-head {
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 24px;
      margin-bottom: 22px;
    }

    .section-head h2 {
      margin: 0;
      font-size: clamp(34px, 5vw, 52px);
      line-height: 1;
      letter-spacing: -.052em;
    }

    .section-head p {
      max-width: 430px;
      margin: 0;
      color: var(--secondary);
      font-size: 14px;
      line-height: 1.55;
      text-align: right;
    }

    .overview-panel {
      padding: 30px;
      border-radius: var(--radius-xl);
      background: var(--surface);
      border: 1px solid rgba(255, 255, 255, .9);
      box-shadow: var(--shadow-soft);
    }

    .ledger-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 0;
      border-bottom: 1px solid var(--line);
    }

    .ledger-stat {
      padding: 4px 28px 28px;
    }

    .ledger-stat + .ledger-stat { border-left: 1px solid var(--line); }

    .ledger-stat span {
      display: block;
      color: var(--secondary);
      font-size: 12px;
    }

    .ledger-stat strong {
      display: block;
      margin-top: 10px;
      font-size: 36px;
      line-height: 1;
      letter-spacing: -.045em;
    }

    .recent-list { padding-top: 12px; }

    .recent-item {
      min-height: 72px;
      padding: 15px 2px;
      display: grid;
      grid-template-columns: 42px 1fr auto;
      align-items: center;
      gap: 14px;
      border-bottom: 1px solid var(--line);
    }

    .recent-item:last-child { border-bottom: 0; }

    .source-dot {
      width: 34px;
      height: 34px;
      display: grid;
      place-items: center;
      border-radius: 11px;
      color: white;
      font-size: 11px;
      font-weight: 760;
      letter-spacing: .04em;
    }

    .source-dot.ielts { background: var(--blue); }
    .source-dot.health { background: var(--green); }
    .source-dot.sleep { background: var(--purple); }
    .source-dot.gift { background: var(--text); }

    .recent-main strong {
      display: block;
      font-size: 14px;
    }

    .recent-main span {
      display: block;
      margin-top: 5px;
      color: var(--secondary);
      font-size: 12px;
      line-height: 1.4;
    }

    .recent-date {
      color: var(--secondary);
      font-size: 12px;
      text-align: right;
    }

    .empty-state {
      padding: 34px 4px 16px;
      color: var(--secondary);
      font-size: 13px;
      line-height: 1.6;
    }

    footer {
      padding: 18px 0 52px;
      color: var(--secondary);
      font-size: 12px;
    }

    .footer-inner {
      padding-top: 22px;
      display: flex;
      justify-content: space-between;
      gap: 18px;
      border-top: 1px solid var(--line);
    }

    .modal-layer {
      position: fixed;
      inset: 0;
      z-index: 100;
      display: none;
      align-items: center;
      justify-content: center;
      padding: 22px;
      background: rgba(0, 0, 0, .28);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
    }

    .modal-layer.open {
      display: flex;
      animation: fadeIn .2s ease both;
    }

    .modal {
      width: min(100%, 720px);
      max-height: min(90vh, 860px);
      overflow: auto;
      border-radius: 28px;
      background: rgba(255, 255, 255, .97);
      border: 1px solid rgba(255, 255, 255, .95);
      box-shadow: 0 34px 110px rgba(0, 0, 0, .2);
      animation: modalIn .28s var(--ease) both;
    }

    .modal.wide { width: min(100%, 850px); }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    @keyframes modalIn {
      from { opacity: 0; transform: translateY(12px) scale(.985); }
      to { opacity: 1; transform: none; }
    }

    .modal-head {
      position: sticky;
      top: 0;
      z-index: 5;
      min-height: 72px;
      padding: 18px 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
      background: rgba(255, 255, 255, .92);
      backdrop-filter: blur(18px);
      border-bottom: 1px solid var(--line);
    }

    .modal-title {
      font-size: 21px;
      font-weight: 680;
      letter-spacing: -.03em;
    }

    .close-button {
      width: 34px;
      height: 34px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      background: rgba(0, 0, 0, .055);
      color: var(--text);
      font-size: 20px;
    }

    .modal-body { padding: 26px; }

    .modal-section + .modal-section {
      margin-top: 28px;
      padding-top: 26px;
      border-top: 1px solid var(--line);
    }

    .modal-section-title {
      margin-bottom: 16px;
      font-size: 13px;
      font-weight: 730;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
    }

    .field {
      display: grid;
      gap: 8px;
    }

    .field.full { grid-column: 1 / -1; }

    .field label {
      color: var(--secondary);
      font-size: 12px;
      font-weight: 630;
    }

    .field input,
    .field select,
    .field textarea {
      width: 100%;
      min-height: 46px;
      padding: 12px 14px;
      color: var(--text);
      background: #f7f7f9;
      border: 1px solid var(--line);
      border-radius: 13px;
      outline: 0;
      transition: border-color .18s ease, box-shadow .18s ease, background .18s ease;
    }

    .field textarea {
      min-height: 92px;
      resize: vertical;
      line-height: 1.55;
    }

    .field input:focus,
    .field select:focus,
    .field textarea:focus {
      background: white;
      border-color: rgba(0, 113, 227, .5);
      box-shadow: 0 0 0 4px rgba(0, 113, 227, .08);
    }

    .choice-list {
      display: grid;
      gap: 10px;
    }

    .choice {
      position: relative;
    }

    .choice input {
      position: absolute;
      opacity: 0;
      pointer-events: none;
    }

    .choice label {
      min-height: 52px;
      padding: 0 15px;
      display: flex;
      align-items: center;
      gap: 12px;
      border-radius: 15px;
      background: #f7f7f9;
      border: 1px solid var(--line);
      color: var(--text);
      cursor: pointer;
      transition: .18s ease;
    }

    .choice-mark {
      width: 22px;
      height: 22px;
      flex: 0 0 auto;
      display: grid;
      place-items: center;
      border-radius: 7px;
      border: 1.5px solid var(--line-strong);
      color: white;
      font-size: 13px;
    }

    .choice input:checked + label {
      background: rgba(36, 161, 72, .08);
      border-color: rgba(36, 161, 72, .28);
    }

    .choice input:checked + label .choice-mark {
      background: var(--green);
      border-color: var(--green);
    }

    .summary-box {
      padding: 20px;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 14px;
      border-radius: 18px;
      background: #f7f7f9;
      border: 1px solid var(--line);
    }

    .summary-box > div { min-width: 0; }

    .summary-box span {
      display: block;
      color: var(--secondary);
      font-size: 11px;
    }

    .summary-box strong {
      display: block;
      margin-top: 7px;
      font-size: 22px;
      line-height: 1;
      letter-spacing: -.035em;
      overflow-wrap: anywhere;
    }

    .detail-progress {
      margin-top: 18px;
    }

    .detail-progress + .detail-progress { margin-top: 22px; }

    .detail-progress .progress-track { height: 7px; }

    .chart-wrap {
      position: relative;
      height: 230px;
      margin-top: 14px;
    }

    .chart-wrap canvas {
      width: 100%;
      height: 100%;
      display: block;
    }

    .chart-empty {
      position: absolute;
      inset: 0;
      display: grid;
      place-items: center;
      color: var(--secondary);
      font-size: 13px;
      text-align: center;
      pointer-events: none;
    }

    .history-list {
      display: grid;
      gap: 0;
    }

    .history-row {
      padding: 14px 0;
      display: grid;
      grid-template-columns: 110px 1fr auto;
      gap: 16px;
      align-items: center;
      border-bottom: 1px solid var(--line);
      font-size: 13px;
    }

    .history-row:last-child { border-bottom: 0; }

    .history-row .date { color: var(--secondary); }
    .history-row .note { color: var(--secondary); line-height: 1.45; }
    .history-row .value { text-align: right; font-weight: 640; }

    .sleep-status {
      padding: 26px;
      border-radius: 20px;
      background: #f7f7f9;
      border: 1px solid var(--line);
      text-align: center;
    }

    .sleep-status strong {
      display: block;
      font-size: 48px;
      line-height: 1;
      letter-spacing: -.055em;
    }

    .sleep-status span {
      display: block;
      margin-top: 10px;
      color: var(--secondary);
      font-size: 13px;
      line-height: 1.5;
    }

    .checkin-button {
      width: 100%;
      min-height: 52px;
      margin-top: 16px;
      border-radius: 999px;
      color: white;
      background: var(--purple);
      font-weight: 690;
    }

    .checkin-button:disabled {
      cursor: not-allowed;
      color: var(--secondary);
      background: rgba(0, 0, 0, .065);
    }

    .modal-actions {
      position: sticky;
      bottom: 0;
      z-index: 4;
      padding: 18px 24px 22px;
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      background: linear-gradient(to top, white 74%, rgba(255, 255, 255, .8));
    }

    .button {
      min-height: 44px;
      padding: 0 18px;
      border-radius: 999px;
      font-size: 14px;
      font-weight: 680;
      transition: transform .18s var(--ease), opacity .18s ease;
    }

    .button:hover { transform: translateY(-1px); }

    .button.primary {
      color: white;
      background: var(--text);
    }

    .button.secondary {
      color: var(--text);
      background: rgba(0, 0, 0, .055);
    }

    .button.danger {
      color: var(--danger);
      background: rgba(217, 45, 32, .1);
    }

    .data-actions {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }

    .celebration-modal {
      width: min(100%, 580px);
      overflow: hidden;
    }

    .celebration-card {
      position: relative;
      min-height: 480px;
      padding: 44px 38px 34px;
      display: flex;
      flex-direction: column;
      overflow: hidden;
      background:
        radial-gradient(circle at 100% 0%, rgba(154, 106, 18, .12), transparent 32%),
        #fff;
    }

    .celebration-card::after {
      content: "";
      position: absolute;
      right: -110px;
      bottom: -120px;
      width: 300px;
      height: 300px;
      border-radius: 50%;
      background: rgba(0, 0, 0, .035);
    }

    .celebration-kicker {
      position: relative;
      z-index: 1;
      color: var(--secondary);
      font-size: 12px;
      font-weight: 760;
      letter-spacing: .16em;
      text-transform: uppercase;
    }

    .celebration-number {
      position: relative;
      z-index: 1;
      margin-top: 62px;
      font-size: clamp(56px, 10vw, 86px);
      line-height: .92;
      letter-spacing: -.065em;
      font-weight: 720;
    }

    .celebration-copy {
      position: relative;
      z-index: 1;
      max-width: 430px;
      margin-top: 24px;
      color: var(--secondary);
      font-size: 16px;
      line-height: 1.6;
    }

    .celebration-points {
      position: relative;
      z-index: 1;
      margin-top: auto;
      padding-top: 40px;
      font-size: 24px;
      font-weight: 700;
      letter-spacing: -.03em;
    }

    .celebration-meta {
      position: relative;
      z-index: 1;
      margin-top: 9px;
      color: var(--secondary);
      font-size: 12px;
    }

    .celebration-actions {
      position: relative;
      z-index: 1;
      margin-top: 28px;
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .gift-card-view {
      border: 1px solid rgba(0, 0, 0, .08);
      border-radius: 24px;
      background:
        radial-gradient(circle at 86% 12%, rgba(255, 255, 255, .18), transparent 24%),
        linear-gradient(145deg, #1d1d1f, #35353a);
      color: white;
      padding: 34px;
      box-shadow: 0 24px 70px rgba(0, 0, 0, .2);
    }

    .gift-card-view .muted { color: rgba(255, 255, 255, .62); }

    .gift-title {
      margin-top: 52px;
      font-size: 48px;
      line-height: .95;
      letter-spacing: -.055em;
      font-weight: 700;
    }

    .gift-name {
      margin-top: 28px;
      font-size: 23px;
      font-weight: 650;
    }

    .gift-description {
      margin-top: 12px;
      max-width: 380px;
      color: rgba(255, 255, 255, .7);
      line-height: 1.55;
      font-size: 14px;
    }

    .gift-code {
      margin-top: 54px;
      padding-top: 18px;
      display: flex;
      justify-content: space-between;
      gap: 18px;
      border-top: 1px solid rgba(255, 255, 255, .14);
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
      font-size: 13px;
    }

    .gift-list {
      display: grid;
      gap: 12px;
      margin-top: 18px;
    }

    .gift-record {
      padding: 16px;
      border-radius: 16px;
      background: #f7f7f9;
      border: 1px solid var(--line);
    }

    .gift-record-top {
      display: flex;
      justify-content: space-between;
      gap: 16px;
      align-items: center;
    }

    .gift-record strong { font-size: 14px; }

    .gift-record span {
      display: block;
      margin-top: 5px;
      color: var(--secondary);
      font-size: 12px;
    }

    .status-tag {
      min-height: 26px;
      padding: 0 10px;
      display: inline-flex;
      align-items: center;
      border-radius: 999px;
      color: var(--secondary);
      background: rgba(0, 0, 0, .055);
      font-size: 11px;
      white-space: nowrap;
    }

    .status-tag.claimed {
      color: var(--green);
      background: rgba(36, 161, 72, .1);
    }

    .toast {
      position: fixed;
      left: 50%;
      bottom: 28px;
      z-index: 250;
      padding: 12px 16px;
      transform: translate(-50%, 18px);
      opacity: 0;
      pointer-events: none;
      border-radius: 999px;
      color: white;
      background: rgba(29, 29, 31, .94);
      box-shadow: 0 14px 36px rgba(0, 0, 0, .2);
      font-size: 13px;
      transition: .24s var(--ease);
    }

    .toast.show {
      opacity: 1;
      transform: translate(-50%, 0);
    }

    .sr-only {
      position: absolute !important;
      width: 1px !important;
      height: 1px !important;
      padding: 0 !important;
      margin: -1px !important;
      overflow: hidden !important;
      clip: rect(0, 0, 0, 0) !important;
      white-space: nowrap !important;
      border: 0 !important;
    }

    @media (max-width: 940px) {
      .cards-grid { grid-template-columns: 1fr; }
      .habit-card { min-height: 330px; }
      .reward-summary { grid-template-columns: 1fr; }
      .reward-metrics { justify-content: flex-start; }
      .reward-metric { text-align: left; }
    }

    @media (max-width: 720px) {
      .container { width: min(calc(100% - 24px), var(--page)); }
      main { padding-top: 44px; }
      .topbar-inner { height: 60px; }
      .milestone-pill span { display: none; }
      .hero { grid-template-columns: 1fr; gap: 24px; }
      .hero-date { text-align: left; }
      .reward-summary { padding: 21px; }
      .reward-metrics { flex-wrap: wrap; gap: 18px; }
      .section-head { align-items: start; flex-direction: column; }
      .section-head p { text-align: left; }
      .ledger-grid { grid-template-columns: 1fr; }
      .ledger-stat { padding: 18px 0; }
      .ledger-stat + .ledger-stat {
        border-left: 0;
        border-top: 1px solid var(--line);
      }
      .overview-panel { padding: 22px; }
      .form-grid { grid-template-columns: 1fr; }
      .field.full { grid-column: auto; }
      .summary-box { grid-template-columns: 1fr; }
      .history-row { grid-template-columns: 92px 1fr; }
      .history-row .value { grid-column: 2; text-align: left; }
      .modal-body { padding: 20px; }
      .modal-head, .modal-actions { padding-left: 20px; padding-right: 20px; }
      .modal-actions { justify-content: stretch; }
      .modal-actions .button { flex: 1; }
      .celebration-card { padding: 34px 26px 28px; }
      .celebration-actions .button { flex: 1; }
      .footer-inner { flex-direction: column; }
    }
  
    /* -------------------------------------------------------------
       Mobile-first refinement · Chinese reading hierarchy
       ------------------------------------------------------------- */
    .hero {
      display: block;
      margin-bottom: 30px;
    }

    .hero-meta-line {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
      margin-bottom: 16px;
    }

    .hero-meta-line .eyebrow {
      margin: 0;
      color: var(--text);
      font-size: 15px;
      font-weight: 680;
      letter-spacing: 0;
      text-transform: none;
    }

    .hero h1 span {
      color: var(--secondary);
    }

    .hero-date {
      padding: 0;
      text-align: right;
      font-size: 12px;
    }

    .reward-title {
      display: flex;
      align-items: baseline;
      gap: 9px;
      font-size: 17px;
      font-weight: 720;
    }

    .reward-title small {
      color: var(--secondary);
      font-size: 9px;
      font-weight: 760;
      letter-spacing: .12em;
    }

    .card-heading {
      min-width: 0;
      display: flex;
      align-items: center;
      gap: 13px;
    }

    .card-icon {
      width: 46px;
      height: 46px;
      flex: 0 0 auto;
      display: grid;
      place-items: center;
      border-radius: 14px;
    }

    .card-icon svg {
      width: 23px;
      height: 23px;
      fill: none;
      stroke: currentColor;
      stroke-width: 1.7;
      stroke-linecap: round;
      stroke-linejoin: round;
    }

    .ielts-icon {
      color: var(--blue);
      background: rgba(0, 113, 227, .10);
    }

    .weight-icon {
      color: var(--green);
      background: rgba(36, 161, 72, .10);
    }

    .sleep-icon {
      color: var(--purple);
      background: rgba(103, 87, 217, .10);
    }

    .card-title-cn {
      color: var(--text);
      font-size: 18px;
      line-height: 1.25;
      font-weight: 720;
      letter-spacing: -.02em;
    }

    .card-heading .card-label {
      margin-top: 4px;
      font-size: 9px;
      letter-spacing: .13em;
    }

    .transfer-note {
      margin-bottom: 16px;
      padding: 15px 16px;
      color: var(--secondary);
      background: #f7f7f9;
      border: 1px solid var(--line);
      border-radius: 14px;
      font-size: 13px;
      line-height: 1.65;
    }

    .quick-data-button {
      color: var(--blue);
    }

    @media (max-width: 940px) {
      .cards-grid {
        display: grid;
        grid-template-columns: minmax(0, 1fr);
      }
    }

    @media (max-width: 720px) {
      :root {
        --radius-xl: 27px;
      }

      body {
        background:
          radial-gradient(circle at 0% -4%, rgba(0, 113, 227, .055), transparent 24%),
          radial-gradient(circle at 100% 5%, rgba(103, 87, 217, .045), transparent 22%),
          #f5f5f7;
      }

      .container {
        width: 100%;
        max-width: var(--page);
        padding-left: max(14px, env(safe-area-inset-left));
        padding-right: max(14px, env(safe-area-inset-right));
      }

      .topbar {
        background: rgba(245, 245, 247, .84);
      }

      .topbar-inner {
        height: 58px;
        padding-top: env(safe-area-inset-top);
      }

      .brand {
        gap: 9px;
        font-size: 12px;
        letter-spacing: .12em;
      }

      .brand-mark {
        width: 22px;
        height: 22px;
        border-radius: 7px;
      }

      .top-actions {
        gap: 7px;
      }

      .icon-button,
      .milestone-pill {
        min-width: 36px;
        width: 36px;
        height: 36px;
        min-height: 36px;
        padding: 0;
        justify-content: center;
      }

      .milestone-pill span {
        display: none;
      }

      .milestone-pill strong {
        width: 24px;
        min-width: 24px;
        height: 24px;
        padding: 0;
      }

      main {
        padding: 24px 0 max(56px, env(safe-area-inset-bottom));
      }

      .hero {
        margin-bottom: 22px;
      }

      .hero-meta-line {
        margin-bottom: 13px;
      }

      .hero h1 {
        font-size: clamp(38px, 11vw, 48px);
        line-height: 1.07;
        letter-spacing: -.055em;
      }

      .hero-copy {
        max-width: 100%;
        margin-top: 16px;
        font-size: 15px;
        line-height: 1.72;
        letter-spacing: 0;
      }

      .hero-date {
        white-space: nowrap;
        font-size: 11px;
      }

      .reward-summary {
        margin-bottom: 14px;
        padding: 20px;
        gap: 18px;
        border-radius: 24px;
      }

      .reward-note {
        margin-top: 7px;
        font-size: 12px;
        line-height: 1.6;
      }

      .reward-metrics {
        width: 100%;
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 0;
        border-top: 1px solid var(--line);
        padding-top: 17px;
      }

      .reward-metric {
        min-width: 0;
        padding: 0 12px;
        text-align: left;
      }

      .reward-metric:first-child { padding-left: 0; }
      .reward-metric:last-of-type { padding-right: 0; }

      .reward-metric + .reward-metric {
        border-left: 1px solid var(--line);
      }

      .reward-metric strong {
        font-size: 27px;
      }

      .reward-metric span {
        margin-top: 5px;
        font-size: 10px;
        white-space: normal;
        line-height: 1.3;
      }

      .reward-button {
        grid-column: 1 / -1;
        width: 100%;
        margin-top: 16px;
      }

      .cards-grid {
        width: 100%;
        grid-template-columns: minmax(0, 1fr);
        gap: 14px;
        margin-bottom: 48px;
      }

      .habit-card {
        width: 100%;
        min-width: 0;
        min-height: 0;
        padding: 21px;
        border-radius: 27px;
        box-shadow: 0 12px 36px rgba(0, 0, 0, .045);
      }

      .habit-card:hover {
        transform: none;
      }

      .habit-card::after {
        right: -54px;
        bottom: -70px;
        width: 155px;
        height: 155px;
        opacity: .065;
      }

      .card-head {
        align-items: center;
      }

      .card-arrow {
        width: 36px;
        height: 36px;
        flex: 0 0 auto;
        font-size: 17px;
      }

      .primary-value {
        margin-top: 25px;
        font-size: clamp(48px, 14vw, 62px);
        line-height: .96;
      }

      .primary-value small {
        margin-top: 10px;
        font-size: 13px;
        line-height: 1.45;
      }

      .secondary-data {
        margin-top: 21px;
        padding-top: 17px;
      }

      .secondary-row {
        font-size: 14px;
        line-height: 1.45;
      }

      .secondary-row + .secondary-row {
        margin-top: 11px;
      }

      .progress-area {
        margin-top: 0;
        padding-top: 23px;
      }

      .progress-top {
        margin-bottom: 9px;
        font-size: 12px;
      }

      .progress-track {
        height: 7px;
      }

      .progress-note {
        margin-top: 9px;
        font-size: 12px;
        line-height: 1.5;
      }

      .mini-chart {
        height: 40px;
        margin-top: 13px;
      }

      .section {
        margin-bottom: 45px;
      }

      .section-head {
        margin-bottom: 16px;
        gap: 10px;
      }

      .section-head h2 {
        font-size: 34px;
      }

      .section-head p {
        font-size: 13px;
        line-height: 1.65;
      }

      .overview-panel {
        padding: 20px;
        border-radius: 25px;
      }

      .ledger-grid {
        grid-template-columns: repeat(3, minmax(0, 1fr));
      }

      .ledger-stat {
        min-width: 0;
        padding: 4px 12px 20px;
      }

      .ledger-stat:first-child { padding-left: 0; }
      .ledger-stat:last-child { padding-right: 0; }

      .ledger-stat + .ledger-stat {
        border-top: 0;
        border-left: 1px solid var(--line);
      }

      .ledger-stat strong {
        font-size: 29px;
      }

      .recent-item {
        grid-template-columns: 38px minmax(0, 1fr);
        gap: 11px;
      }

      .recent-date {
        grid-column: 2;
        text-align: left;
      }

      .modal-layer {
        align-items: flex-end;
        padding: 0;
      }

      .modal {
        width: 100%;
        max-height: 92dvh;
        border-radius: 28px 28px 0 0;
        animation-name: mobileSheetIn;
      }

      @keyframes mobileSheetIn {
        from { opacity: 0; transform: translateY(36px); }
        to { opacity: 1; transform: translateY(0); }
      }

      .modal-head {
        min-height: 64px;
        padding: 15px 18px;
      }

      .modal-title {
        font-size: 19px;
      }

      .modal-body {
        padding: 20px 18px;
      }

      .modal-actions {
        padding: 14px 18px max(18px, env(safe-area-inset-bottom));
      }

      .field input,
      .field select,
      .field textarea {
        font-size: 16px;
      }

      .choice label {
        min-height: 56px;
      }

      .summary-box {
        padding: 17px;
        gap: 16px;
      }

      .transfer-actions {
        display: grid;
        grid-template-columns: 1fr 1fr;
      }

      .transfer-actions .danger {
        grid-column: 1 / -1;
      }

      .toast {
        bottom: max(22px, calc(env(safe-area-inset-bottom) + 12px));
        max-width: calc(100% - 32px);
        text-align: center;
      }

      footer {
        padding-bottom: max(30px, env(safe-area-inset-bottom));
      }
    }

    @media (max-width: 390px) {
      .container {
        padding-left: max(12px, env(safe-area-inset-left));
        padding-right: max(12px, env(safe-area-inset-right));
      }

      .hero h1 {
        font-size: 39px;
      }

      .reward-summary,
      .habit-card {
        border-radius: 24px;
      }

      .habit-card {
        padding: 19px;
      }

      .card-icon {
        width: 43px;
        height: 43px;
      }

      .card-title-cn {
        font-size: 17px;
      }

      .primary-value {
        font-size: 50px;
      }
    }

  
    /* -------------------------------------------------------------
       Compact mobile cards v2
       The mobile interface is not a scaled desktop dashboard.
       ------------------------------------------------------------- */
    .compact-main {
      display: grid;
      grid-template-columns: minmax(0, 1fr) minmax(118px, .72fr);
      gap: 20px;
      align-items: end;
      margin-top: 24px;
    }

    .compact-facts {
      min-width: 0;
      display: grid;
      gap: 13px;
      padding-left: 18px;
      border-left: 1px solid var(--line);
    }

    .compact-fact {
      min-width: 0;
    }

    .compact-fact span {
      display: block;
      color: var(--secondary);
      font-size: 11px;
      line-height: 1.3;
    }

    .compact-fact strong {
      display: block;
      margin-top: 5px;
      color: var(--text);
      font-size: 15px;
      line-height: 1.25;
      font-weight: 680;
      text-align: left;
      overflow-wrap: anywhere;
    }

    @media (max-width: 720px) {
      .hero-copy {
        display: none;
      }

      .hero {
        margin-bottom: 18px;
      }

      .hero h1 {
        font-size: 31px;
        line-height: 1.16;
        letter-spacing: -.045em;
      }

      .hero h1 br {
        display: none;
      }

      .hero h1 span {
        margin-left: .15em;
      }

      .reward-summary {
        padding: 17px 18px;
      }

      .reward-note {
        display: none;
      }

      .reward-metrics {
        margin-top: 0;
      }

      .cards-grid {
        gap: 12px;
      }

      .habit-card {
        min-height: 0;
        padding: 18px;
        border-radius: 24px;
      }

      .habit-card::after {
        display: none;
      }

      .card-icon {
        width: 40px;
        height: 40px;
        border-radius: 12px;
      }

      .card-icon svg {
        width: 21px;
        height: 21px;
      }

      .card-title-cn {
        font-size: 17px;
      }

      .card-heading .card-label {
        margin-top: 2px;
        font-size: 8px;
      }

      .card-arrow {
        width: 32px;
        height: 32px;
        font-size: 25px;
        font-weight: 300;
      }

      .compact-main {
        grid-template-columns: minmax(0, 1fr) 118px;
        gap: 15px;
        margin-top: 19px;
      }

      .compact-facts {
        gap: 11px;
        padding-left: 14px;
      }

      .compact-fact strong {
        font-size: 14px;
      }

      .primary-value {
        min-width: 0;
        margin: 0;
        font-size: 36px;
        line-height: 1;
        letter-spacing: -.05em;
        white-space: normal;
        overflow-wrap: anywhere;
      }

      .primary-value small {
        margin-top: 7px;
        font-size: 11px;
        line-height: 1.45;
        letter-spacing: 0;
      }

      .mini-chart {
        height: 26px;
        margin: 13px 0 -2px;
      }

      .progress-area {
        padding-top: 18px;
      }

      .progress-top {
        margin-bottom: 8px;
      }

      .progress-note {
        margin-top: 8px;
        font-size: 11px;
      }

      .section {
        margin-top: 48px;
      }
    }

    @media (max-width: 374px) {
      .compact-main {
        grid-template-columns: minmax(0, 1fr) 105px;
        gap: 11px;
      }

      .compact-facts {
        padding-left: 11px;
      }

      .primary-value {
        font-size: 32px;
      }
    }

  
    /* =============================================================
       Final mobile-first layout
       ============================================================= */
    .card-title-en { color: var(--text); font-size: 18px; line-height: 1.15; font-weight: 760; letter-spacing: .035em; }
    .monochrome .ielts-icon, .monochrome .weight-icon, .monochrome .sleep-icon { color: var(--text); background: rgba(0,0,0,.065); }
    .monochrome .brand-mark { background: var(--text); }
    .monochrome .progress-fill, .monochrome .ielts .progress-fill, .monochrome .weight .progress-fill, .monochrome .sleep .progress-fill { background: var(--text); }

    @media (max-width: 720px) {
      body { min-height: 100dvh; padding-bottom: calc(74px + env(safe-area-inset-bottom)); }
      .topbar { position: fixed; top: auto; left: max(12px, env(safe-area-inset-left)); right: max(12px, env(safe-area-inset-right)); bottom: max(10px, env(safe-area-inset-bottom)); z-index: 80; border: 1px solid rgba(255,255,255,.82); border-radius: 22px; background: rgba(255,255,255,.84); box-shadow: 0 12px 42px rgba(0,0,0,.12); backdrop-filter: blur(28px) saturate(180%); -webkit-backdrop-filter: blur(28px) saturate(180%); }
      .topbar.scrolled { border-bottom-color: rgba(255,255,255,.82); box-shadow: 0 12px 42px rgba(0,0,0,.12); }
      .topbar-inner { height: 54px; padding: 0 10px 0 14px; }
      .brand { gap: 8px; font-size: 11px; }
      .brand-mark { width: 20px; height: 20px; }
      .top-actions { gap: 6px; }
      .icon-button, .milestone-pill { width: 34px; min-width: 34px; height: 34px; min-height: 34px; background: rgba(245,245,247,.88); }
      main { min-height: 100dvh; padding: 18px 0 16px; }
      .hero { margin-bottom: 12px; }
      .hero-meta-line { margin-bottom: 8px; }
      .hero-meta-line .eyebrow { color: var(--secondary); font-size: 11px; font-weight: 700; letter-spacing: .1em; text-transform: uppercase; }
      .hero-date { color: var(--secondary); font-size: 10px; }
      .hero h1 { font-size: 31px; line-height: 1.02; letter-spacing: -.055em; }
      .hero h1 br { display: none; }
      .hero h1 span { margin-left: .14em; }
      .hero-copy { display: block; margin-top: 8px; color: var(--secondary); font-size: 11px; line-height: 1.35; }
      .reward-summary { min-height: 48px; margin-bottom: 10px; padding: 11px 14px; display: flex; align-items: center; justify-content: space-between; border-radius: 18px; }
      .reward-title { font-size: 13px; }
      .reward-note { display: none; }
      .reward-metrics { width: auto; display: flex; gap: 0; padding: 0; border: 0; }
      .reward-metric { min-width: 48px; padding: 0 9px; text-align: center; }
      .reward-metric + .reward-metric { border-left: 1px solid var(--line); }
      .reward-metric strong { font-size: 18px; }
      .reward-metric span { display: none; }
      .reward-button { width: auto; min-height: 32px; margin: 0 0 0 8px; padding: 0 12px; font-size: 11px; }
      .cards-grid { height: min(490px, calc(100dvh - 228px)); min-height: 432px; max-height: 500px; display: grid; grid-template-columns: minmax(0,1fr); grid-template-rows: repeat(3,minmax(0,1fr)); gap: 9px; margin-bottom: 22px; }
      .habit-card { min-height: 0; height: 100%; padding: 13px 15px 12px; border-radius: 21px; overflow: hidden; }
      .habit-card::after { display: none; }
      .card-head { min-height: 31px; }
      .card-heading { gap: 9px; }
      .card-icon { width: 31px; height: 31px; border-radius: 9px; }
      .card-icon svg { width: 17px; height: 17px; }
      .card-title-en { font-size: 14px; }
      .card-heading .card-label { margin-top: 1px; font-size: 7px; }
      .card-arrow { width: 27px; height: 27px; font-size: 22px; background: rgba(0,0,0,.035); }
      .compact-main { grid-template-columns: minmax(0,1fr) 112px; gap: 12px; align-items: center; margin-top: 8px; }
      .primary-value { margin: 0; font-size: 28px; line-height: .96; white-space: normal; }
      .primary-value small { margin-top: 5px; font-size: 9px; line-height: 1.2; }
      .compact-facts { gap: 7px; padding-left: 11px; }
      .compact-fact span { font-size: 8px; }
      .compact-fact strong { margin-top: 2px; font-size: 11px; }
      .mini-chart { display: none; }
      .progress-area { margin-top: auto; padding-top: 8px; }
      .progress-top { margin-bottom: 5px; font-size: 8px; }
      .progress-track { height: 5px; }
      .progress-note { margin-top: 5px; font-size: 8px; line-height: 1.2; }
      .section { margin-top: 24px; margin-bottom: 20px; }
      .section-head { margin-bottom: 12px; }
      .section-head h2 { font-size: 28px; }
      .section-head p { font-size: 11px; line-height: 1.5; }
      .overview-panel { padding: 17px; border-radius: 22px; }
      footer { padding-bottom: 14px; }
    }

    @media (max-width: 390px) {
      .cards-grid { height: min(470px, calc(100dvh - 220px)); min-height: 420px; }
      .habit-card { padding: 12px 14px 11px; }
      .compact-main { grid-template-columns: minmax(0,1fr) 104px; }
      .primary-value { font-size: 26px; }
    }

    @media (max-height: 700px) and (max-width: 720px) {
      .hero-copy, .reward-summary { display: none; }
      .cards-grid { height: calc(100dvh - 142px); min-height: 410px; }
    }

  

    /* =============================================================
       Three · interaction refinement v3
       ============================================================= */
    .progress-number-only {
      justify-content: flex-end;
      margin-bottom: 8px;
    }

    .progress-number-only strong {
      font-size: 12px;
    }

    .habit-card .progress-area.compact-progress {
      padding-top: 18px;
    }

    .wheel-field {
      display: grid;
      gap: 10px;
    }

    .wheel-field > label,
    .field-caption {
      color: var(--secondary);
      font-size: 12px;
      font-weight: 630;
    }

    .wheel-shell {
      --wheel-row: 46px;
      position: relative;
      min-height: 154px;
      display: grid;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      border: 1px solid var(--line);
      border-radius: 18px;
      background: #f7f7f9;
    }

    .wheel-shell::before,
    .wheel-shell::after {
      content: "";
      position: absolute;
      z-index: 3;
      left: 0;
      right: 0;
      height: 47px;
      pointer-events: none;
    }

    .wheel-shell::before {
      top: 0;
      background: linear-gradient(#f7f7f9 24%, rgba(247,247,249,0));
    }

    .wheel-shell::after {
      bottom: 0;
      background: linear-gradient(rgba(247,247,249,0), #f7f7f9 76%);
    }

    .wheel-focus {
      position: absolute;
      z-index: 1;
      top: 50%;
      left: 12px;
      right: 12px;
      height: var(--wheel-row);
      transform: translateY(-50%);
      border-top: 1px solid var(--line);
      border-bottom: 1px solid var(--line);
      background: rgba(255,255,255,.64);
      pointer-events: none;
    }

    .single-wheel-layout {
      position: relative;
      z-index: 2;
      display: grid;
      grid-template-columns: 128px auto;
      align-items: center;
      gap: 12px;
    }

    .dual-wheel-layout {
      position: relative;
      z-index: 2;
      display: grid;
      grid-template-columns: 82px 16px 82px auto;
      align-items: center;
      justify-content: center;
      gap: 2px;
    }

    .wheel-column {
      height: calc(var(--wheel-row) * 3);
      overflow-x: hidden;
      overflow-y: auto;
      padding-block: var(--wheel-row);
      scroll-snap-type: y mandatory;
      overscroll-behavior: contain;
      scrollbar-width: none;
      -webkit-overflow-scrolling: touch;
    }

    .wheel-column::-webkit-scrollbar { display: none; }

    .wheel-option {
      height: var(--wheel-row);
      display: grid;
      place-items: center;
      scroll-snap-align: center;
      color: var(--secondary);
      font-size: 22px;
      font-weight: 600;
      letter-spacing: -.025em;
      opacity: .38;
      transform: scale(.9);
      transition: opacity .12s ease, transform .12s ease, color .12s ease;
      cursor: pointer;
      user-select: none;
    }

    .wheel-option.selected {
      color: var(--text);
      opacity: 1;
      transform: scale(1);
      font-weight: 700;
    }

    .wheel-dot {
      color: var(--text);
      font-size: 28px;
      font-weight: 650;
      text-align: center;
    }

    .wheel-unit {
      color: var(--secondary);
      font-size: 13px;
      font-weight: 650;
      white-space: nowrap;
    }

    .picker-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    .choice-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }

    .choice-grid .choice label {
      min-height: 58px;
      justify-content: space-between;
      padding: 0 16px;
    }

    .choice-content {
      min-width: 0;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .choice-emoji {
      flex: 0 0 auto;
      font-size: 21px;
      line-height: 1;
    }

    .choice-title {
      font-size: 14px;
      font-weight: 650;
    }

    .choice-bonus {
      color: var(--secondary);
      font-size: 10px;
      white-space: nowrap;
    }

    .ielts-choice input:checked + label {
      color: var(--blue);
      background: rgba(0,113,227,.08);
      border-color: rgba(0,113,227,.25);
    }

    .ielts-choice input:checked + label .choice-mark {
      background: var(--blue);
      border-color: var(--blue);
    }

    .draft-summary {
      margin-top: 16px;
      padding: 16px 18px;
      display: grid;
      grid-template-columns: 1fr auto;
      align-items: center;
      gap: 16px;
      border-radius: 16px;
      background: #f7f7f9;
      border: 1px solid var(--line);
    }

    .draft-summary-title {
      font-size: 13px;
      font-weight: 680;
    }

    .draft-formula {
      margin-top: 6px;
      color: var(--secondary);
      font-size: 11px;
      line-height: 1.5;
    }

    .draft-summary strong {
      font-size: 30px;
      line-height: 1;
      letter-spacing: -.045em;
    }

    .health-score {
      margin-top: 14px;
    }

    .weekend-banner {
      margin-bottom: 14px;
      padding: 16px 17px;
      display: grid;
      grid-template-columns: 36px 1fr;
      gap: 12px;
      align-items: start;
      border-radius: 17px;
      color: #4f43b8;
      background: rgba(103,87,217,.09);
      border: 1px solid rgba(103,87,217,.18);
    }

    .weekend-banner[hidden] { display: none; }

    .weekend-banner-icon {
      width: 36px;
      height: 36px;
      display: grid;
      place-items: center;
      border-radius: 11px;
      background: rgba(103,87,217,.12);
      font-size: 19px;
    }

    .weekend-banner strong {
      display: block;
      font-size: 14px;
    }

    .weekend-banner span {
      display: block;
      margin-top: 5px;
      color: var(--secondary);
      font-size: 12px;
      line-height: 1.55;
    }

    .backup-panel {
      padding: 17px;
      border-radius: 17px;
      background: #f7f7f9;
      border: 1px solid var(--line);
    }

    .backup-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 0;
    }

    .backup-stat {
      min-width: 0;
      padding: 2px 14px;
    }

    .backup-stat:first-child { padding-left: 0; }
    .backup-stat + .backup-stat { border-left: 1px solid var(--line); }

    .backup-stat span {
      display: block;
      color: var(--secondary);
      font-size: 11px;
    }

    .backup-stat strong {
      display: block;
      margin-top: 7px;
      font-size: 17px;
      line-height: 1.25;
      overflow-wrap: anywhere;
    }

    .backup-advice {
      margin-top: 15px;
      padding-top: 13px;
      color: var(--secondary);
      border-top: 1px solid var(--line);
      font-size: 12px;
      line-height: 1.55;
    }

    @media (max-width: 720px) {
      .picker-grid { grid-template-columns: 1fr; }
      .choice-grid { gap: 9px; }
      .choice-grid .choice label { min-height: 55px; padding: 0 12px; }
      .choice-title { font-size: 13px; }
      .choice-bonus { display: none; }
      .wheel-shell { min-height: 148px; }
      .draft-summary strong { font-size: 27px; }
      .habit-card .progress-area.compact-progress { padding-top: 7px; }
      .progress-number-only { margin-bottom: 5px; }
    }

    @media (max-width: 370px) {
      .choice-grid { grid-template-columns: 1fr; }
      .dual-wheel-layout { grid-template-columns: 72px 14px 72px auto; }
    }

  
    /* =============================================================
       Refined Apple-inspired color system
       Low saturation, clean white surfaces, soft semantic accents.
       ============================================================= */
    :root {
      --bg: #f5f6f8;
      --surface: rgba(255, 255, 255, .925);
      --surface-solid: #ffffff;
      --text: #1d1d1f;
      --secondary: #6b6b73;
      --tertiary: #888890;
      --line: rgba(29, 29, 31, .075);
      --line-strong: rgba(29, 29, 31, .135);

      --blue: #3e8fea;
      --blue-soft: #eaf4ff;
      --blue-glow: rgba(62, 143, 234, .085);

      --green: #35a96b;
      --green-soft: #eaf8f0;
      --green-glow: rgba(53, 169, 107, .08);

      --purple: #7968d8;
      --purple-soft: #f0edff;
      --purple-glow: rgba(121, 104, 216, .085);

      --gold: #a97828;
      --gold-soft: #fff7e8;
      --gold-accent: #c4933f;

      --danger: #d94b43;
      --shadow: 0 24px 70px rgba(38, 45, 62, .075);
      --shadow-soft: 0 12px 36px rgba(38, 45, 62, .05);
    }

    html {
      background: var(--bg);
    }

    body {
      background:
        radial-gradient(circle at 4% -9%, rgba(89, 154, 255, .085), transparent 29%),
        radial-gradient(circle at 97% 2%, rgba(140, 117, 232, .07), transparent 25%),
        radial-gradient(circle at 52% 108%, rgba(215, 188, 145, .065), transparent 31%),
        var(--bg);
    }

    .topbar {
      background: rgba(245, 246, 248, .80);
    }

    .brand-mark {
      background: linear-gradient(
        135deg,
        #4a9bf0 0 33%,
        #47b779 33% 66%,
        #8674df 66%
      );
      box-shadow:
        inset 0 0 0 1px rgba(255, 255, 255, .62),
        0 5px 14px rgba(62, 143, 234, .13);
    }

    .habit-card {
      background: rgba(255, 255, 255, .93);
      border-color: rgba(255, 255, 255, .94);
    }

    .habit-card.ielts {
      background:
        radial-gradient(circle at 105% 112%, var(--blue-glow), transparent 37%),
        rgba(255, 255, 255, .93);
    }

    .habit-card.weight {
      background:
        radial-gradient(circle at 105% 112%, var(--green-glow), transparent 37%),
        rgba(255, 255, 255, .93);
    }

    .habit-card.sleep {
      background:
        radial-gradient(circle at 105% 112%, var(--purple-glow), transparent 37%),
        rgba(255, 255, 255, .93);
    }

    .habit-card::after {
      opacity: .055;
    }

    .ielts-icon {
      color: var(--blue);
      background: var(--blue-soft);
      box-shadow: inset 0 0 0 1px rgba(62, 143, 234, .045);
    }

    .weight-icon {
      color: var(--green);
      background: var(--green-soft);
      box-shadow: inset 0 0 0 1px rgba(53, 169, 107, .045);
    }

    .sleep-icon {
      color: var(--purple);
      background: var(--purple-soft);
      box-shadow: inset 0 0 0 1px rgba(121, 104, 216, .045);
    }

    .ielts .progress-fill,
    #ieltsDetailProgressFill {
      background: linear-gradient(90deg, #6db4ff, var(--blue)) !important;
    }

    .weight .progress-fill,
    #healthDetailProgressFill {
      background: linear-gradient(90deg, #65c990, var(--green)) !important;
    }

    .sleep .progress-fill,
    #sleepDetailProgressFill {
      background: linear-gradient(90deg, #a091f0, var(--purple)) !important;
    }

    .source-dot.ielts {
      background: linear-gradient(145deg, #65adf6, var(--blue));
    }

    .source-dot.health {
      background: linear-gradient(145deg, #61c78c, var(--green));
    }

    .source-dot.sleep {
      background: linear-gradient(145deg, #9b8bea, var(--purple));
    }

    .source-dot.gift {
      background: linear-gradient(145deg, var(--gold-accent), var(--gold));
    }

    .ielts-choice input:checked + label {
      color: #276ead;
      background: var(--blue-soft);
      border-color: rgba(62, 143, 234, .25);
    }

    .ielts-choice input:checked + label .choice-mark {
      background: var(--blue);
      border-color: var(--blue);
    }

    .choice:not(.ielts-choice) input:checked + label {
      color: #247b4d;
      background: var(--green-soft);
      border-color: rgba(53, 169, 107, .25);
    }

    .choice:not(.ielts-choice) input:checked + label .choice-mark {
      background: var(--green);
      border-color: var(--green);
    }

    .weekend-banner {
      color: #5748b3;
      background: var(--purple-soft);
      border-color: rgba(121, 104, 216, .18);
    }

    .weekend-banner-icon {
      color: var(--purple);
      background: rgba(121, 104, 216, .11);
    }

    .reward-summary {
      background:
        radial-gradient(circle at 92% -30%, rgba(196, 147, 63, .12), transparent 46%),
        rgba(255, 255, 255, .925);
      border-color: rgba(196, 147, 63, .11);
    }

    .reward-title {
      color: #6f4d18;
    }

    .reward-metric strong {
      color: var(--gold);
    }

    .milestone-pill {
      border-color: rgba(169, 120, 40, .14);
      background: rgba(255, 247, 232, .84);
    }

    .milestone-pill strong {
      background: linear-gradient(145deg, var(--gold-accent), var(--gold));
    }

    .gift-card-view {
      background:
        radial-gradient(circle at 86% 12%, rgba(255, 244, 216, .16), transparent 25%),
        linear-gradient(145deg, #24211c, #3e3528);
    }

    .celebration-card {
      background:
        radial-gradient(circle at 100% 0%, rgba(196, 147, 63, .15), transparent 34%),
        #fff;
    }

    .backup-panel,
    .draft-summary,
    .sleep-status,
    .summary-box,
    .gift-record,
    .transfer-note {
      background: rgba(247, 248, 250, .92);
    }

    .quick-data-button {
      color: var(--blue);
    }

    @media (max-width: 720px) {
      body {
        background:
          radial-gradient(circle at -5% -4%, rgba(89, 154, 255, .075), transparent 27%),
          radial-gradient(circle at 105% 6%, rgba(140, 117, 232, .06), transparent 24%),
          radial-gradient(circle at 48% 105%, rgba(215, 188, 145, .055), transparent 26%),
          var(--bg);
      }

      .topbar {
        background: rgba(255, 255, 255, .86);
        border-color: rgba(255, 255, 255, .88);
        box-shadow: 0 14px 46px rgba(45, 51, 67, .12);
      }

      .habit-card {
        box-shadow: 0 12px 34px rgba(45, 51, 67, .052);
      }

      .reward-summary {
        box-shadow: 0 10px 30px rgba(82, 61, 24, .045);
      }
    }

  
    /* =============================================================
       Gift-on-demand banner & compact target weight
       ============================================================= */
    .reward-summary[hidden] {
      display: none !important;
    }

    .gift-unlocked-banner {
      min-height: 74px;
      grid-template-columns: 1fr auto;
      padding: 17px 20px;
      border-color: rgba(196, 147, 63, .20);
      background:
        radial-gradient(circle at 88% -45%, rgba(196, 147, 63, .20), transparent 48%),
        linear-gradient(145deg, rgba(255, 250, 240, .97), rgba(255, 255, 255, .94));
      box-shadow: 0 12px 32px rgba(117, 82, 25, .075);
    }

    .gift-unlocked-copy {
      min-width: 0;
      display: flex;
      align-items: center;
      gap: 13px;
    }

    .gift-unlocked-icon {
      width: 40px;
      height: 40px;
      flex: 0 0 auto;
      display: grid;
      place-items: center;
      border-radius: 13px;
      color: var(--gold);
      background: var(--gold-soft);
      box-shadow: inset 0 0 0 1px rgba(169, 120, 40, .09);
      font-size: 20px;
    }

    .gift-unlocked-banner .reward-title {
      color: #76531d;
      font-size: 16px;
    }

    .gift-unlocked-banner .reward-note {
      margin-top: 4px;
      font-size: 12px;
    }

    .gift-unlocked-banner .reward-button {
      min-width: 92px;
      background: linear-gradient(145deg, var(--gold-accent), var(--gold));
      box-shadow: 0 8px 20px rgba(169, 120, 40, .17);
    }

    .target-weight-compact {
      min-height: 82px;
      padding: 15px 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      align-self: start;
      border-radius: 17px;
      background: rgba(247, 248, 250, .94);
      border: 1px solid var(--line);
    }

    .target-weight-copy {
      min-width: 0;
    }

    .target-weight-copy label {
      display: block;
      color: var(--text);
      font-size: 13px;
      font-weight: 680;
    }

    .target-weight-copy span {
      display: block;
      margin-top: 5px;
      color: var(--secondary);
      font-size: 10px;
      line-height: 1.35;
    }

    .target-weight-input-wrap {
      flex: 0 0 auto;
      min-height: 46px;
      padding: 0 12px;
      display: flex;
      align-items: center;
      gap: 7px;
      border-radius: 13px;
      background: #fff;
      border: 1px solid var(--line-strong);
    }

    .target-weight-input-wrap:focus-within {
      border-color: rgba(53, 169, 107, .42);
      box-shadow: 0 0 0 4px rgba(53, 169, 107, .08);
    }

    .target-weight-input-wrap input {
      width: 76px;
      padding: 0;
      border: 0;
      outline: 0;
      color: var(--text);
      background: transparent;
      font-size: 20px;
      font-weight: 690;
      letter-spacing: -.025em;
      text-align: right;
      appearance: textfield;
      -moz-appearance: textfield;
    }

    .target-weight-input-wrap input::-webkit-outer-spin-button,
    .target-weight-input-wrap input::-webkit-inner-spin-button {
      margin: 0;
      -webkit-appearance: none;
    }

    .target-weight-input-wrap span {
      color: var(--secondary);
      font-size: 12px;
      font-weight: 650;
    }

    @media (max-width: 720px) {
      .gift-unlocked-banner {
        min-height: 64px;
        padding: 12px 13px;
        display: grid;
        grid-template-columns: minmax(0, 1fr) auto;
        gap: 10px;
      }

      .gift-unlocked-icon {
        width: 34px;
        height: 34px;
        border-radius: 11px;
        font-size: 17px;
      }

      .gift-unlocked-banner .reward-title {
        font-size: 13px;
      }

      .gift-unlocked-banner .reward-note {
        font-size: 10px;
        line-height: 1.35;
      }

      .gift-unlocked-banner .reward-button {
        width: auto;
        min-width: 70px;
        min-height: 34px;
        margin: 0;
        padding: 0 12px;
        font-size: 11px;
      }

      .target-weight-compact {
        min-height: 72px;
        padding: 12px 13px;
      }

      .target-weight-input-wrap {
        min-height: 42px;
        padding: 0 10px;
      }

      .target-weight-input-wrap input {
        width: 70px;
        font-size: 18px;
      }
    }

  
    /* =============================================================
       Neutral reward palette
       Removes champagne-gold accents while preserving semantic colors.
       ============================================================= */
    :root {
      --gold: var(--text);
      --gold-soft: #f3f4f6;
      --gold-accent: #36363a;
    }

    .reward-summary,
    .gift-unlocked-banner {
      color: var(--text);
      background:
        radial-gradient(circle at 92% -35%, rgba(62, 143, 234, .09), transparent 45%),
        rgba(255, 255, 255, .94);
      border-color: rgba(29, 29, 31, .08);
      box-shadow: 0 12px 32px rgba(38, 45, 62, .055);
    }

    .reward-title,
    .gift-unlocked-banner .reward-title {
      color: var(--text);
    }

    .reward-metric strong {
      color: var(--text);
    }

    .milestone-pill {
      color: var(--text);
      background: rgba(255, 255, 255, .86);
      border-color: var(--line);
    }

    .milestone-pill strong {
      color: #fff;
      background: var(--text);
    }

    .source-dot.gift {
      background: var(--text);
    }

    .gift-unlocked-icon {
      color: var(--blue);
      background: var(--blue-soft);
      box-shadow: inset 0 0 0 1px rgba(62, 143, 234, .08);
    }

    .gift-unlocked-banner .reward-button {
      color: #fff;
      background: var(--text);
      box-shadow: 0 8px 20px rgba(29, 29, 31, .13);
    }

    .celebration-card {
      background:
        radial-gradient(circle at 100% 0%, rgba(62, 143, 234, .10), transparent 34%),
        radial-gradient(circle at 0% 100%, rgba(121, 104, 216, .075), transparent 32%),
        #fff;
    }

    .gift-card-view {
      background:
        radial-gradient(circle at 86% 12%, rgba(255, 255, 255, .13), transparent 24%),
        linear-gradient(145deg, #1d1d1f, #35353a);
    }

    .target-weight-copy span {
      display: none;
    }

    .target-weight-compact {
      min-height: 68px;
    }

    @media (max-width: 720px) {
      .target-weight-compact {
        min-height: 62px;
      }
    }

  </style>
</head>

<body>
  <div class="app" id="app">
    <header class="topbar" id="topbar">
      <div class="container topbar-inner">
        <div class="brand">
          <span class="brand-mark" aria-hidden="true"></span>
          <span>Three</span>
        </div>

        <div class="top-actions">
          <button class="icon-button quick-data-button" id="quickExport" type="button" aria-label="导出或分享数据" title="导出数据">
            <svg viewBox="0 0 24 24">
              <path d="M12 3v11"></path>
              <path d="m8 10 4 4 4-4"></path>
              <path d="M5 15v4h14v-4"></path>
            </svg>
          </button>

          <button class="milestone-pill" id="openLedger" type="button">
            <span>里程积分</span>
            <strong id="topBalance">0</strong>
          </button>

          <button class="icon-button" id="openSettings" type="button" aria-label="设置" title="设置">
            <svg viewBox="0 0 24 24"><path d="M12 15.5a3.5 3.5 0 1 0 0-7 3.5 3.5 0 0 0 0 7Z"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06A1.65 1.65 0 0 0 15 19.4a1.65 1.65 0 0 0-1 .6 1.65 1.65 0 0 0-.4 1.08V21a2 2 0 1 1-4 0v-.09A1.65 1.65 0 0 0 8.6 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-.6-1 1.65 1.65 0 0 0-1.08-.4H3a2 2 0 1 1 0-4h.09A1.65 1.65 0 0 0 4.6 8.6a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-.6 1.65 1.65 0 0 0 .4-1.08V3a2 2 0 1 1 4 0v.09A1.65 1.65 0 0 0 15.4 4.6a1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9c.14.37.36.7.66.97.3.26.67.41 1.07.43H21a2 2 0 1 1 0 4h-.09a1.65 1.65 0 0 0-1.51.6Z"/></svg>
          </button>
        </div>
      </div>
    </header>

    <main>
      <div class="container">
        <section class="hero">
          <div class="hero-meta-line">
            <div class="eyebrow" id="greeting">For 罗思维</div>
            <div class="hero-date" id="heroDate"></div>
          </div>
          <h1>Keep going,<br><span>quietly.</span></h1>
          <p class="hero-copy">Study. Sleep. Shape. Three quiet habits, moving in one direction.</p>
        </section>

        <section class="reward-summary gift-unlocked-banner" id="rewardSummary" aria-label="神秘礼品已解锁" hidden>
          <div class="gift-unlocked-copy">
            <div class="gift-unlocked-icon" aria-hidden="true">✦</div>
            <div>
              <div class="reward-title">Gift unlocked</div>
              <div class="reward-note" id="rewardSummaryText">已经可以兑换一份神秘礼品。</div>
            </div>
          </div>
          <button class="reward-button" id="redeemHome" type="button">Redeem</button>
        </section>

        <section class="cards-grid" aria-label="Three daily goals">
          <article class="habit-card ielts" data-card-type="ielts" data-open="ieltsModal" tabindex="0" role="button" aria-label="Open IELTS">
            <div class="card-head">
              <div class="card-heading">
                <div class="card-icon ielts-icon" aria-hidden="true">
                  <svg viewBox="0 0 24 24"><path d="M5 4.8A2.8 2.8 0 0 1 7.8 2H20v17H7.8A2.8 2.8 0 0 0 5 21.8Z"></path><path d="M5 4.8v17"></path><path d="M9 7h7"></path><path d="M9 11h6"></path></svg>
                </div>
                <div class="card-title-en">IELTS</div>
              </div>
              <div class="card-arrow" aria-hidden="true">›</div>
            </div>
            <div class="compact-main">
              <div class="primary-value" id="ieltsPrimary">SET<small id="ieltsPrimaryLabel">Exam date</small></div>
              <div class="compact-facts">
                <div class="compact-fact"><span>Today</span><strong id="todayWords">0 words</strong></div>
                <div class="compact-fact"><span>Total</span><strong id="totalWords">0 words</strong></div>
              </div>
            </div>
            <div class="progress-area compact-progress">
              <div class="progress-top progress-number-only"><strong id="wordProgressText">0 / 500</strong></div>
              <div class="progress-track"><div class="progress-fill" id="wordProgressFill"></div></div>
            </div>
          </article>

          <article class="habit-card weight" data-card-type="weight" data-open="weightModal" tabindex="0" role="button" aria-label="Open Weight">
            <div class="card-head">
              <div class="card-heading">
                <div class="card-icon weight-icon" aria-hidden="true">
                  <svg viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="16" rx="4"></rect><path d="M8 10a4 4 0 0 1 8 0"></path><path d="m12 10 2-2"></path></svg>
                </div>
                <div class="card-title-en">WEIGHT</div>
              </div>
              <div class="card-arrow" aria-hidden="true">›</div>
            </div>
            <div class="compact-main">
              <div class="primary-value" id="weightPrimary">—<small id="weightPrimaryLabel">Today</small></div>
              <div class="compact-facts">
                <div class="compact-fact"><span>Target</span><strong id="targetWeightCard">55.00 kg</strong></div>
                <div class="compact-fact"><span>Change</span><strong id="weightChangeCard">—</strong></div>
              </div>
            </div>
            <canvas class="mini-chart" id="weightSparkline" aria-label="Weight trend"></canvas>
            <div class="progress-area compact-progress">
              <div class="progress-top progress-number-only"><strong id="healthProgressText">0 / 50</strong></div>
              <div class="progress-track"><div class="progress-fill" id="healthProgressFill"></div></div>
            </div>
          </article>

          <article class="habit-card sleep" data-card-type="sleep" data-open="sleepModal" tabindex="0" role="button" aria-label="Open Sleep">
            <div class="card-head">
              <div class="card-heading">
                <div class="card-icon sleep-icon" aria-hidden="true">
                  <svg viewBox="0 0 24 24"><path d="M20 15.2A8.5 8.5 0 0 1 8.8 4 8.5 8.5 0 1 0 20 15.2Z"></path><path d="M16.8 4.2v3.2"></path><path d="M15.2 5.8h3.2"></path></svg>
                </div>
                <div class="card-title-en">SLEEP</div>
              </div>
              <div class="card-arrow" aria-hidden="true">›</div>
            </div>
            <div class="compact-main">
              <div class="primary-value" id="sleepPrimary">0<small id="sleepPrimaryLabel">Current streak</small></div>
              <div class="compact-facts">
                <div class="compact-fact"><span>Tonight</span><strong id="tonightStatus">Open later</strong></div>
                <div class="compact-fact"><span>Best</span><strong id="bestSleepStreak">0 days</strong></div>
              </div>
            </div>
            <div class="progress-area compact-progress">
              <div class="progress-top progress-number-only"><strong id="sleepProgressText">0 / 14</strong></div>
              <div class="progress-track"><div class="progress-fill" id="sleepProgressFill"></div></div>
            </div>
          </article>
        </section>

        <section class="section" id="ledgerSection">
          <div class="section-head">
            <h2>Milestones</h2>
            <p>Every completed stage is recorded. Earned milestones remain available even after a later interruption.</p>
          </div>

          <div class="overview-panel">
            <div class="ledger-grid">
              <div class="ledger-stat">
                <span>Earned</span>
                <strong id="ledgerEarned">0</strong>
              </div>
              <div class="ledger-stat">
                <span>Redeemed</span>
                <strong id="ledgerRedeemed">0</strong>
              </div>
              <div class="ledger-stat">
                <span>Available</span>
                <strong id="ledgerAvailable">0</strong>
              </div>
            </div>

            <div class="recent-list" id="recentList"></div>
          </div>
        </section>

        <footer>
          <div class="footer-inner">
            <span>Made for 罗思维 by 董纪君.</span>
            <span>所有数据仅保存在此浏览器。</span>
          </div>
        </footer>
      </div>
    </main>
  </div>

  <!-- IELTS -->
  <div class="modal-layer" id="ieltsModal">
    <div class="modal wide" role="dialog" aria-modal="true" aria-labelledby="ieltsModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="ieltsModalTitle">IELTS · 今日学习</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>

      <form id="ieltsForm">
        <div class="modal-body">
          <div class="modal-section">
            <div class="modal-section-title">目标</div>
            <div class="field">
              <label for="examDateInput">IELTS考试日期</label>
              <input id="examDateInput" type="date" />
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">今日新增单词</div>
            <div class="wheel-field">
              <div class="wheel-shell" aria-label="选择今天新增记住的单词数量">
                <div class="wheel-focus" aria-hidden="true"></div>
                <div class="single-wheel-layout">
                  <div class="wheel-column" id="wordWheelColumn" role="listbox" aria-label="单词数量"></div>
                  <div class="wheel-unit">words</div>
                </div>
              </div>
              <input id="todayNewWords" type="hidden" value="0" />
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">今日专项学习</div>
            <div class="choice-grid">
              <div class="choice ielts-choice">
                <input id="skillListening" type="checkbox" />
                <label for="skillListening">
                  <span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">听力</span></span>
                  <span class="choice-bonus">+20词</span>
                </label>
              </div>
              <div class="choice ielts-choice">
                <input id="skillReading" type="checkbox" />
                <label for="skillReading">
                  <span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">阅读</span></span>
                  <span class="choice-bonus">+20词</span>
                </label>
              </div>
              <div class="choice ielts-choice">
                <input id="skillWriting" type="checkbox" />
                <label for="skillWriting">
                  <span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">写作</span></span>
                  <span class="choice-bonus">+20词</span>
                </label>
              </div>
              <div class="choice ielts-choice">
                <input id="skillSpeaking" type="checkbox" />
                <label for="skillSpeaking">
                  <span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">口语</span></span>
                  <span class="choice-bonus">+20词</span>
                </label>
              </div>
            </div>

            <div class="draft-summary">
              <div>
                <div class="draft-summary-title">今日计入进度</div>
                <div class="draft-formula"><span id="ieltsDraftActual">0</span>个新增 + <span id="ieltsDraftSkill">0</span>个专项折算</div>
              </div>
              <strong id="ieltsDraftTotal">0</strong>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">累计进度</div>
            <div class="summary-box">
              <div><span>累计计入</span><strong id="ieltsTotalDetail">0</strong></div>
              <div><span>已完成里程</span><strong id="ieltsMilestonesDetail">0</strong></div>
              <div><span>下一目标</span><strong id="ieltsRemainingDetail">500</strong></div>
            </div>

            <div class="detail-progress">
              <div class="progress-top">
                <span>本轮进度</span>
                <strong id="ieltsDetailProgressText">0 / 500</strong>
              </div>
              <div class="progress-track">
                <div class="progress-fill" id="ieltsDetailProgressFill" style="background:var(--blue)"></div>
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">最近记录</div>
            <div class="history-list" id="ieltsHistory"></div>
          </div>
        </div>

        <div class="modal-actions">
          <button class="button secondary" type="button" data-close>取消</button>
          <button class="button primary" type="submit">保存</button>
        </div>
      </form>
    </div>
  </div>

  <!-- WEIGHT -->
  <div class="modal-layer" id="weightModal">
    <div class="modal wide" role="dialog" aria-modal="true" aria-labelledby="weightModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="weightModalTitle">Weight · 今日记录</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>

      <form id="weightForm">
        <div class="modal-body">
          <div class="modal-section">
            <div class="modal-section-title">体重</div>
            <div class="picker-grid">
              <div class="target-weight-compact">
                <div class="target-weight-copy">
                  <label for="targetWeightInput">目标体重</label>
                </div>
                <div class="target-weight-input-wrap">
                  <input
                    id="targetWeightInput"
                    type="number"
                    inputmode="decimal"
                    min="40"
                    max="60"
                    step="0.01"
                    value="55.00"
                    aria-label="目标体重"
                  />
                  <span>kg</span>
                </div>
              </div>

              <div class="wheel-field">
                <label>今日体重</label>
                <div class="wheel-shell" aria-label="选择今日体重">
                  <div class="wheel-focus" aria-hidden="true"></div>
                  <div class="dual-wheel-layout">
                    <div class="wheel-column" id="todayWeightWholeWheel" role="listbox" aria-label="今日体重整数"></div>
                    <div class="wheel-dot">.</div>
                    <div class="wheel-column" id="todayWeightDecimalWheel" role="listbox" aria-label="今日体重小数"></div>
                    <div class="wheel-unit">kg</div>
                  </div>
                </div>
                <input id="todayWeightInput" type="hidden" value="55.00" />
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">今天完成的健康选择</div>
            <div class="choice-grid">
              <div class="choice">
                <input id="noLateSnack" type="checkbox" />
                <label for="noLateSnack"><span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">无宵夜</span></span><span class="choice-emoji">🍟</span></label>
              </div>
              <div class="choice">
                <input id="noMilkTea" type="checkbox" />
                <label for="noMilkTea"><span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">无奶茶</span></span><span class="choice-emoji">🧋</span></label>
              </div>
              <div class="choice">
                <input id="eightyFull" type="checkbox" />
                <label for="eightyFull"><span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">八分饱</span></span><span class="choice-emoji">🍚</span></label>
              </div>
              <div class="choice">
                <input id="exerciseDone" type="checkbox" />
                <label for="exerciseDone"><span class="choice-content"><span class="choice-mark">✓</span><span class="choice-title">运动</span></span><span class="choice-emoji">🏋️</span></label>
              </div>
            </div>

          </div>

          <div class="modal-section">
            <div class="modal-section-title">历史体重变化</div>
            <div class="chart-wrap">
              <canvas id="weightChart" aria-label="历史体重变化趋势图"></canvas>
              <div class="chart-empty" id="weightChartEmpty">至少记录两次体重后显示趋势。</div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">最近记录</div>
            <div class="history-list" id="weightHistory"></div>
          </div>
        </div>

        <div class="modal-actions">
          <button class="button secondary" type="button" data-close>取消</button>
          <button class="button primary" type="submit">保存</button>
        </div>
      </form>
    </div>
  </div>

  <!-- SLEEP -->
  <div class="modal-layer" id="sleepModal">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="sleepModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="sleepModalTitle">Sleep · 早睡打卡</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>

      <div class="modal-body">
        <div class="modal-section">
          <div class="weekend-banner" id="weekendStatusBanner" hidden>
            <div class="weekend-banner-icon">☾</div>
            <div>
              <strong>周末放松日</strong>
              <span>今晚不打卡不会中断连续记录，也不会增加天数；主动打卡仍可增加1天。</span>
            </div>
          </div>

          <div class="sleep-status">
            <strong id="sleepModalStreak">0 days</strong>
            <span id="sleepWindowText">打卡开放时间为每天20:00至次日00:59。</span>
            <button class="checkin-button" id="sleepCheckinButton" type="button">完成今晚打卡</button>
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">当前进度</div>
          <div class="summary-box">
            <div><span>当前连续</span><strong id="sleepCurrentDetail">0</strong></div>
            <div><span>历史最佳</span><strong id="sleepBestDetail">0</strong></div>
            <div><span>已完成里程</span><strong id="sleepMilestonesDetail">0</strong></div>
          </div>

          <div class="detail-progress">
            <div class="progress-top">
              <span>下一次连续两周</span>
              <strong id="sleepDetailProgressText">0 / 14</strong>
            </div>
            <div class="progress-track">
              <div class="progress-fill" id="sleepDetailProgressFill" style="background:var(--purple)"></div>
            </div>
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">规则</div>
          <div class="empty-state" style="padding:0">
            普通夜晚需在20:00至次日00:59完成打卡。周五和周六晚上为放松日：未打卡不会中断记录，也不会增加累计天数；主动打卡则正常增加1天。跨午夜的打卡归入前一晚，其他日期不支持补签。
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">最近记录</div>
          <div class="history-list" id="sleepHistory"></div>
        </div>
      </div>

      <div class="modal-actions">
        <button class="button primary" type="button" data-close>完成</button>
      </div>
    </div>
  </div>

  <!-- LEDGER -->
  <div class="modal-layer" id="ledgerModal">
    <div class="modal wide" role="dialog" aria-modal="true" aria-labelledby="ledgerModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="ledgerModalTitle">里程与礼品</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>

      <div class="modal-body">
        <div class="modal-section">
          <div class="summary-box">
            <div><span>累计获得</span><strong id="modalEarned">0</strong></div>
            <div><span>已经使用</span><strong id="modalUsed">0</strong></div>
            <div><span>当前可用</span><strong id="modalAvailable">0</strong></div>
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">里程记录</div>
          <div class="history-list" id="milestoneHistory"></div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">神秘礼品兑换卡</div>
          <button class="button primary" id="redeemLedger" type="button" style="margin-bottom:16px">使用2个积分兑换</button>
          <div class="gift-list" id="giftRecords"></div>
        </div>
      </div>

      <div class="modal-actions">
        <button class="button primary" type="button" data-close>完成</button>
      </div>
    </div>
  </div>

  <!-- SETTINGS -->
  <div class="modal-layer" id="settingsModal">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="settingsModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="settingsModalTitle">设置</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>

      <form id="settingsForm">
        <div class="modal-body">
          <div class="modal-section">
            <div class="modal-section-title">目标设置</div>
            <div class="form-grid">
              <div class="field">
                <label for="settingsExamDate">雅思考试日期</label>
                <input id="settingsExamDate" type="date" />
              </div>
              <div class="field">
                <label for="settingsTargetWeight">目标体重（kg）</label>
                <input id="settingsTargetWeight" type="number" min="40" max="60" step="0.01" />
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">首页显示</div>
            <div class="form-grid">
              <div class="field">
                <label for="cardOrderInput">卡片排列顺序</label>
                <select id="cardOrderInput">
                  <option value="ielts,weight,sleep">雅思 · 体重 · 早睡</option>
                  <option value="sleep,ielts,weight">早睡 · 雅思 · 体重</option>
                  <option value="weight,sleep,ielts">体重 · 早睡 · 雅思</option>
                </select>
              </div>
              <div class="field">
                <label for="colorStyleInput">界面强调色</label>
                <select id="colorStyleInput">
                  <option value="color">彩色</option>
                  <option value="mono">黑白</option>
                </select>
              </div>
              <div class="field">
                <label for="showHistoryInput">里程记录</label>
                <select id="showHistoryInput">
                  <option value="show">在卡片下方显示</option>
                  <option value="hide">不在首页显示</option>
                </select>
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">固定规则</div>
            <div class="transfer-note">
              使用者固定为罗思维，神秘礼品提供者固定为董纪君。早睡打卡时间固定为每天20:00至次日00:59；周五、周六晚上未打卡不会中断连续记录。
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">最近备份</div>
            <div class="backup-panel">
              <div class="backup-grid">
                <div class="backup-stat">
                  <span>上次导出</span>
                  <strong id="lastBackupText">从未备份</strong>
                </div>
                <div class="backup-stat">
                  <span>已有记录</span>
                  <strong id="recordedDaysText">0天</strong>
                </div>
              </div>
              <div class="backup-advice" id="backupAdviceText">建议首次记录后导出一份备份。</div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">数据迁移</div>
            <div class="transfer-note">
              在旧设备导出JSON备份，并通过微信、文件或网盘发送；在新设备打开本网站后导入，即可恢复单词、体重、早睡、积分和礼品兑换记录。
            </div>
            <div class="data-actions transfer-actions">
              <button class="button primary" id="exportData" type="button">导出 / 分享数据</button>
              <button class="button secondary" id="importData" type="button">导入备份</button>
              <button class="button danger" id="clearData" type="button">清空全部数据</button>
              <input class="sr-only" id="importFile" type="file" accept="application/json,.json" />
            </div>
          </div>
        </div>

        <div class="modal-actions">
          <button class="button secondary" type="button" data-close>取消</button>
          <button class="button primary" type="submit">保存设置</button>
        </div>
      </form>
    </div>
  </div>

  <!-- CELEBRATION -->
  <div class="modal-layer" id="celebrationLayer">
    <div class="modal celebration-modal" role="dialog" aria-modal="true" aria-labelledby="celebrationTitle">
      <div id="celebrationContent"></div>
    </div>
  </div>

  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    (() => {
      "use strict";

      const STORAGE_KEY = "three_luosiwei_final_v1";
      const APP_VERSION = 2;

      const defaultState = {
        version: APP_VERSION,
        settings: {
          userName: "罗思维",
          authorName: "董纪君",
          examDate: "",
          targetWeight: 55,
          sleepStart: "20:00",
          sleepDeadline: "01:00",
          cardOrder: "ielts,weight,sleep",
          colorStyle: "color",
          showRewardSummary: true,
          showHistory: true,
          lastBackupAt: null
        },
        daily: {},
        sleepCheckins: {},
        milestones: [],
        redemptions: []
      };

      let state = loadState();
      let popupQueue = [];
      let popupOpen = false;
      let toastTimer = null;

      const $ = id => document.getElementById(id);
      const todayKey = () => dateKey(new Date());

      function cloneDefault() {
        return JSON.parse(JSON.stringify(defaultState));
      }

      function loadState() {
        try {
          const raw = localStorage.getItem(STORAGE_KEY);
          if (!raw) return cloneDefault();

          const parsed = JSON.parse(raw);
          return {
            version: APP_VERSION,
            settings: {
              ...cloneDefault().settings,
              ...(parsed.settings || {}),
              userName: "罗思维",
              authorName: "董纪君",
              sleepStart: "20:00",
              sleepDeadline: "01:00"
            },
            daily: parsed.daily || {},
            sleepCheckins: parsed.sleepCheckins || {},
            milestones: Array.isArray(parsed.milestones) ? parsed.milestones : [],
            redemptions: Array.isArray(parsed.redemptions) ? parsed.redemptions : []
          };
        } catch (error) {
          console.warn("Unable to load saved data:", error);
          return cloneDefault();
        }
      }

      function saveState() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      }

      function dateKey(date) {
        const y = date.getFullYear();
        const m = String(date.getMonth() + 1).padStart(2, "0");
        const d = String(date.getDate()).padStart(2, "0");
        return `${y}-${m}-${d}`;
      }

      function dateFromKey(key) {
        const [y, m, d] = String(key).split("-").map(Number);
        return new Date(y, m - 1, d);
      }

      function addDaysToKey(key, amount) {
        const date = dateFromKey(key);
        date.setDate(date.getDate() + amount);
        return dateKey(date);
      }

      function dayDifference(aKey, bKey) {
        const a = dateFromKey(aKey);
        const b = dateFromKey(bKey);
        a.setHours(12, 0, 0, 0);
        b.setHours(12, 0, 0, 0);
        return Math.round((b - a) / 86400000);
      }

      function formatDate(date, options = {}) {
        return new Intl.DateTimeFormat("zh-CN", {
          year: options.year ?? "numeric",
          month: options.month ?? "long",
          day: options.day ?? "numeric",
          weekday: options.weekday
        }).format(date);
      }

      function formatShortDate(date) {
        return new Intl.DateTimeFormat("zh-CN", {
          month: "short",
          day: "numeric"
        }).format(date);
      }

      function formatDateTime(iso) {
        const date = new Date(iso);
        return new Intl.DateTimeFormat("zh-CN", {
          year: "numeric",
          month: "short",
          day: "numeric",
          hour: "2-digit",
          minute: "2-digit"
        }).format(date);
      }

      function safeNumber(value, fallback = 0) {
        const number = Number(value);
        return Number.isFinite(number) ? number : fallback;
      }

      function clamp(value, min, max) {
        return Math.min(max, Math.max(min, value));
      }

      function escapeHtml(value) {
        return String(value ?? "").replace(/[&<>"']/g, char => ({
          "&": "&amp;",
          "<": "&lt;",
          ">": "&gt;",
          '"': "&quot;",
          "'": "&#039;"
        })[char]);
      }

      const IELTS_SKILLS = [
        { key: "listening", label: "听力" },
        { key: "reading", label: "阅读" },
        { key: "writing", label: "写作" },
        { key: "speaking", label: "口语" }
      ];

      const HEALTH_CHOICES = [
        { key: "noLateSnack", label: "无宵夜" },
        { key: "noMilkTea", label: "无奶茶" },
        { key: "eightyFull", label: "八分饱" },
        { key: "exerciseDone", label: "运动" }
      ];

      function ensureDaily(key = todayKey()) {
        if (!state.daily[key]) state.daily[key] = {};

        const existingWords = state.daily[key].words || {};
        const existingSkills = existingWords.skills || {};
        state.daily[key].words = {
          newWords: 0,
          reviewWords: 0,
          note: "",
          ...existingWords,
          skills: {
            listening: false,
            reading: false,
            writing: false,
            speaking: false,
            ...existingSkills
          }
        };

        const existingWeight = state.daily[key].weight || {};
        state.daily[key].weight = {
          value: "",
          noLateSnack: false,
          noMilkTea: false,
          eightyFull: false,
          exerciseDone: false,
          ...existingWeight
        };

        return state.daily[key];
      }

      function selectedIeltsSkills(words) {
        const skills = words?.skills || {};
        return IELTS_SKILLS.filter(item => Boolean(skills[item.key]));
      }

      function effectiveWordsForEntry(entry) {
        const words = entry?.words || {};
        const actual = Math.max(0, Math.floor(safeNumber(words.newWords)));
        return actual + selectedIeltsSkills(words).length * 20;
      }

      function totalWords() {
        return Object.values(state.daily).reduce(
          (sum, entry) => sum + effectiveWordsForEntry(entry),
          0
        );
      }

      function healthPointsForEntry(entry) {
        const weight = entry?.weight || {};
        return HEALTH_CHOICES.filter(item => Boolean(weight[item.key])).length;
      }

      function totalHealthPoints() {
        return Object.values(state.daily).reduce(
          (sum, entry) => sum + healthPointsForEntry(entry),
          0
        );
      }

      function milestonesFor(source) {
        return state.milestones.filter(item => item.source === source);
      }

      function milestoneBalance() {
        return Math.max(0, state.milestones.length - state.redemptions.length * 2);
      }

      function availableGiftCount() {
        return Math.floor(milestoneBalance() / 2);
      }

      function weightEntries() {
        return Object.keys(state.daily)
          .sort()
          .map(key => ({
            key,
            date: dateFromKey(key),
            value: safeNumber(state.daily[key]?.weight?.value, NaN),
            points: healthPointsForEntry(state.daily[key])
          }))
          .filter(item => Number.isFinite(item.value) && item.value > 0);
      }

      function firstWeight() {
        return weightEntries()[0] || null;
      }

      function latestWeight() {
        const entries = weightEntries();
        return entries.length ? entries[entries.length - 1] : null;
      }

      function currentRound(total, threshold) {
        if (total <= 0) return 0;
        return total % threshold;
      }

      function remainingToNext(total, threshold) {
        const progress = currentRound(total, threshold);
        return progress === 0 && total > 0 ? threshold : threshold - progress;
      }

      function getExamCountdown() {
        if (!state.settings.examDate) {
          return { primary: "SET", label: "Exam date" };
        }

        const exam = dateFromKey(state.settings.examDate);
        const diff = dayDifference(todayKey(), state.settings.examDate);

        if (diff > 0) {
          return { primary: String(diff), label: `days · ${formatShortDate(exam)}` };
        }

        if (diff === 0) {
          return { primary: "TODAY", label: "IELTS exam" };
        }

        return { primary: "DONE", label: formatShortDate(exam) };
      }

      function timeToMinutes(value, fallback) {
        const source = /^\d{2}:\d{2}$/.test(String(value || "")) ? value : fallback;
        const [hours, minutes] = source.split(":").map(Number);
        return hours * 60 + minutes;
      }

      function getNightWindow(now = new Date()) {
        const current = now.getHours() * 60 + now.getMinutes();
        const start = timeToMinutes(state.settings.sleepStart, "20:00");
        const deadline = timeToMinutes(state.settings.sleepDeadline, "01:00");
        const crossesMidnight = start >= deadline;

        if (crossesMidnight) {
          if (current >= start) {
            return { canCheckIn: true, nightKey: dateKey(now), message: `Check-in is open until ${state.settings.sleepDeadline || "01:00"}.` };
          }
          if (current < deadline) {
            const previous = new Date(now);
            previous.setDate(previous.getDate() - 1);
            return { canCheckIn: true, nightKey: dateKey(previous), message: "This check-in still belongs to last night." };
          }
        } else if (current >= start && current < deadline) {
          return { canCheckIn: true, nightKey: dateKey(now), message: `Check-in is open until ${state.settings.sleepDeadline || "01:00"}.` };
        }

        return { canCheckIn: false, nightKey: null, message: `Opens at ${state.settings.sleepStart || "20:00"}.` };
      }

      function latestConcludedNightKey(now = new Date()) {
        const current = now.getHours() * 60 + now.getMinutes();
        const start = timeToMinutes(state.settings.sleepStart, "20:00");
        const deadline = timeToMinutes(state.settings.sleepDeadline, "01:00");
        const crossesMidnight = start >= deadline;

        if (crossesMidnight) {
          if (current < deadline) return addDaysToKey(dateKey(now), -2);
          return addDaysToKey(dateKey(now), -1);
        }

        if (current >= deadline) return dateKey(now);
        return addDaysToKey(dateKey(now), -1);
      }

      function sleepReferenceKey(now = new Date()) {
        const windowInfo = getNightWindow(now);
        if (windowInfo.canCheckIn) {
          if (state.sleepCheckins[windowInfo.nightKey]) return windowInfo.nightKey;
          return addDaysToKey(windowInfo.nightKey, -1);
        }
        return latestConcludedNightKey(now);
      }

      function isRelaxNightKey(key) {
        if (!key) return false;
        const day = dateFromKey(key).getDay();
        return day === 5 || day === 6;
      }

      function tonightNightKey(now = new Date()) {
        const current = now.getHours() * 60 + now.getMinutes();
        const deadline = timeToMinutes(state.settings.sleepDeadline, "01:00");
        if (current < deadline) {
          const previous = new Date(now);
          previous.setDate(previous.getDate() - 1);
          return dateKey(previous);
        }
        return dateKey(now);
      }

      function currentSleepRun(now = new Date()) {
        const reference = sleepReferenceKey(now);
        if (!reference) return { streak: 0, startKey: null, endKey: null };

        let streak = 0;
        let cursor = reference;
        let startKey = null;
        let endKey = null;
        let guard = 0;

        while (guard < 5000) {
          if (state.sleepCheckins[cursor]) {
            streak += 1;
            if (!endKey) endKey = cursor;
            startKey = cursor;
          } else if (!isRelaxNightKey(cursor)) {
            break;
          }

          cursor = addDaysToKey(cursor, -1);
          guard += 1;
        }

        return { streak, startKey, endKey: endKey || reference };
      }

      function bestSleepStreak() {
        const keys = Object.keys(state.sleepCheckins).sort();
        if (!keys.length) return 0;

        let cursor = keys[0];
        const end = keys[keys.length - 1];
        let current = 0;
        let best = 0;
        let guard = 0;

        while (cursor <= end && guard < 10000) {
          if (state.sleepCheckins[cursor]) {
            current += 1;
            best = Math.max(best, current);
          } else if (!isRelaxNightKey(cursor)) {
            current = 0;
          }

          cursor = addDaysToKey(cursor, 1);
          guard += 1;
        }

        return best;
      }

      function tonightDisplayStatus() {
        const windowInfo = getNightWindow();
        const nightKey = windowInfo.canCheckIn ? windowInfo.nightKey : tonightNightKey();

        if (windowInfo.canCheckIn && state.sleepCheckins[nightKey]) return "Done";
        if (isRelaxNightKey(nightKey)) return "RELAX DAY";
        return windowInfo.canCheckIn ? "Open now" : "Open later";
      }

      function createMilestone(source, level, title, detail, id) {
        if (state.milestones.some(item => item.id === id)) return null;

        const milestone = {
          id,
          source,
          level,
          title,
          detail,
          createdAt: new Date().toISOString()
        };

        state.milestones.push(milestone);
        return milestone;
      }

      function syncWordMilestones() {
        const expected = Math.floor(totalWords() / 500);
        const added = [];

        for (let level = 1; level <= expected; level += 1) {
          const milestone = createMilestone(
            "ielts",
            level,
            `${level * 500} Words`,
            `累计完成了${level * 500}个雅思学习进度（含专项折算）。`,
            `ielts-${level}`
          );

          if (milestone) added.push(milestone);
        }

        return added;
      }

      function syncHealthMilestones() {
        const expected = Math.floor(totalHealthPoints() / 50);
        const added = [];

        for (let level = 1; level <= expected; level += 1) {
          const milestone = createMilestone(
            "health",
            level,
            `${level * 50} Healthy Choices`,
            `累计完成了${level * 50}次健康选择。`,
            `health-${level}`
          );

          if (milestone) added.push(milestone);
        }

        return added;
      }

      function syncSleepMilestones() {
        const run = currentSleepRun();
        if (!run.startKey || run.streak < 14) return [];

        const expected = Math.floor(run.streak / 14);
        const added = [];

        for (let level = 1; level <= expected; level += 1) {
          const id = `sleep-${run.startKey}-${level}`;
          const milestone = createMilestone(
            "sleep",
            level,
            `${level * 14} Nights`,
            `连续${level * 14}晚在凌晨1点前完成打卡。`,
            id
          );

          if (milestone) added.push(milestone);
        }

        return added;
      }

      function syncAllMilestones(showPopups = false) {
        const beforeBalance = milestoneBalance();
        const added = [
          ...syncWordMilestones(),
          ...syncHealthMilestones(),
          ...syncSleepMilestones()
        ];

        if (added.length) {
          saveState();

          if (showPopups) {
            added.forEach(item => enqueuePopup({ type: "milestone", item }));

            const afterBalance = milestoneBalance();
            if (Math.floor(afterBalance / 2) > Math.floor(beforeBalance / 2)) {
              enqueuePopup({ type: "gift-unlocked" });
            }
          }
        }

        return added;
      }

      function enqueuePopup(item) {
        popupQueue.push(item);
        showNextPopup();
      }

      function showNextPopup() {
        if (popupOpen || popupQueue.length === 0) return;

        popupOpen = true;
        const popup = popupQueue.shift();
        renderCelebration(popup);
        $("celebrationLayer").classList.add("open");
        document.body.style.overflow = "hidden";
      }

      function closeCelebration() {
        $("celebrationLayer").classList.remove("open");
        popupOpen = false;

        if (!document.querySelector(".modal-layer.open")) {
          document.body.style.overflow = "";
        }

        window.setTimeout(showNextPopup, 180);
      }

      function sourceLabel(source) {
        return {
          ielts: "IELTS",
          health: "WEIGHT",
          sleep: "SLEEP"
        }[source] || "MILESTONE";
      }

      function renderCelebration(popup) {
        const container = $("celebrationContent");

        if (popup.type === "milestone") {
          const item = popup.item;
          container.innerHTML = `
            <div class="celebration-card">
              <div class="celebration-kicker">${sourceLabel(item.source)} · Milestone ${String(item.level).padStart(2, "0")}</div>
              <div class="celebration-number" id="celebrationTitle">${escapeHtml(item.title)}</div>
              <div class="celebration-copy">${escapeHtml(item.detail)}</div>
              <div class="celebration-points">+1 里程积分</div>
              <div class="celebration-meta">${escapeHtml(formatDateTime(item.createdAt))}</div>
              <div class="celebration-actions">
                <button class="button primary" id="celebrationContinue" type="button">完成</button>
              </div>
            </div>
          `;

          $("celebrationContinue").addEventListener("click", closeCelebration);
          return;
        }

        if (popup.type === "gift-unlocked") {
          const author = escapeHtml(state.settings.authorName || "董纪君");

          container.innerHTML = `
            <div class="celebration-card">
              <div class="celebration-kicker">Mystery Gift</div>
              <div class="celebration-number" id="celebrationTitle">Reward<br>unlocked.</div>
              <div class="celebration-copy">
                已经拥有足够的里程积分，可以向${author}兑换一份神秘礼品。
              </div>
              <div class="celebration-points">2 Milestones = 1 Gift</div>
              <div class="celebration-meta">当前可兑换 ${availableGiftCount()} 次</div>
              <div class="celebration-actions">
                <button class="button secondary" id="giftLater" type="button">稍后兑换</button>
                <button class="button primary" id="giftNow" type="button">生成兑换卡</button>
              </div>
            </div>
          `;

          $("giftLater").addEventListener("click", closeCelebration);
          $("giftNow").addEventListener("click", () => {
            closeCelebration();
            redeemGift(true);
          });
          return;
        }

        if (popup.type === "gift-card") {
          const record = popup.record;
          const user = escapeHtml(state.settings.userName || "罗思维");
          const author = escapeHtml(state.settings.authorName || "董纪君");

          container.innerHTML = `
            <div style="padding:24px">
              <div class="gift-card-view">
                <div class="celebration-kicker muted">THREE · MYSTERY GIFT</div>
                <div class="gift-title" id="celebrationTitle">Reward<br>Card</div>
                <div class="gift-name">${user}</div>
                <div class="gift-description">可凭此卡向${author}兑换一份神秘礼品。本卡已使用2个里程积分生成。</div>
                <div class="gift-code">
                  <span>${escapeHtml(record.code)}</span>
                  <span>${escapeHtml(formatShortDate(new Date(record.createdAt)))}</span>
                </div>
              </div>
              <div class="celebration-actions" style="margin-top:18px">
                <button class="button secondary" id="copyGiftCode" type="button">复制兑换码</button>
                <button class="button primary" id="giftCardDone" type="button">完成</button>
              </div>
            </div>
          `;

          $("copyGiftCode").addEventListener("click", async () => {
            try {
              await navigator.clipboard.writeText(record.code);
              showToast("兑换码已复制");
            } catch {
              window.prompt("请复制兑换码：", record.code);
            }
          });

          $("giftCardDone").addEventListener("click", closeCelebration);
        }
      }

      function redeemGift(showCard = true) {
        if (milestoneBalance() < 2) {
          showToast("当前积分不足2个");
          return;
        }

        const index = state.redemptions.length + 1;
        const initials = "LSW";
        const compactDate = todayKey().replaceAll("-", "");
        const code = `${initials}-${compactDate}-${String(index).padStart(2, "0")}`;

        const record = {
          id: `gift-${Date.now()}-${index}`,
          code,
          cost: 2,
          status: "pending",
          createdAt: new Date().toISOString(),
          claimedAt: null
        };

        state.redemptions.push(record);
        saveState();
        renderAll();

        if (showCard) enqueuePopup({ type: "gift-card", record });
      }

      function showToast(message) {
        const toast = $("toast");
        toast.textContent = message;
        toast.classList.add("show");
        window.clearTimeout(toastTimer);
        toastTimer = window.setTimeout(() => toast.classList.remove("show"), 2200);
      }

      function openModal(id) {
        prefillModal(id);
        $(id).classList.add("open");
        document.body.style.overflow = "hidden";
      }

      function closeModal(layer) {
        layer.classList.remove("open");
        if (!document.querySelector(".modal-layer.open")) {
          document.body.style.overflow = "";
        }
      }

      const wheelRegistry = new Map();
      const WHEEL_ROW_HEIGHT = 46;

      function buildWheel(id, min, max, formatter, onChange) {
        const element = $(id);
        const values = [];
        for (let value = min; value <= max; value += 1) values.push(value);

        element.innerHTML = values.map(value => `
          <div class="wheel-option" role="option" data-value="${value}">${formatter(value)}</div>
        `).join("");

        const registry = { element, min, max, formatter, onChange, value: min, timer: null };
        wheelRegistry.set(id, registry);

        const commit = () => {
          const index = clamp(Math.round(element.scrollTop / WHEEL_ROW_HEIGHT), 0, values.length - 1);
          const value = values[index];
          registry.value = value;
          element.querySelectorAll(".wheel-option").forEach((option, optionIndex) => {
            const selected = optionIndex === index;
            option.classList.toggle("selected", selected);
            option.setAttribute("aria-selected", selected ? "true" : "false");
          });
          if (typeof onChange === "function") onChange(value);
        };

        element.addEventListener("scroll", () => {
          window.clearTimeout(registry.timer);
          registry.timer = window.setTimeout(commit, 70);
        }, { passive: true });

        element.addEventListener("click", event => {
          const option = event.target.closest(".wheel-option");
          if (!option) return;
          setWheelValue(id, safeNumber(option.dataset.value), true);
        });

        commit();
      }

      function setWheelValue(id, requestedValue, smooth = false) {
        const registry = wheelRegistry.get(id);
        if (!registry) return;
        const value = clamp(Math.round(safeNumber(requestedValue, registry.min)), registry.min, registry.max);
        const index = value - registry.min;
        registry.value = value;
        registry.element.scrollTo({
          top: index * WHEEL_ROW_HEIGHT,
          behavior: smooth ? "smooth" : "auto"
        });

        registry.element.querySelectorAll(".wheel-option").forEach((option, optionIndex) => {
          const selected = optionIndex === index;
          option.classList.toggle("selected", selected);
          option.setAttribute("aria-selected", selected ? "true" : "false");
        });
        if (typeof registry.onChange === "function") registry.onChange(value);

        requestAnimationFrame(() => {
          if (Math.abs(registry.element.scrollTop - index * WHEEL_ROW_HEIGHT) > 1) {
            registry.element.scrollTop = index * WHEEL_ROW_HEIGHT;
          }
        });
      }

      function wheelValue(id) {
        return wheelRegistry.get(id)?.value ?? 0;
      }

      function weightFromWheels(prefix) {
        return clamp(
          wheelValue(`${prefix}WeightWholeWheel`) + wheelValue(`${prefix}WeightDecimalWheel`) / 100,
          40,
          60
        );
      }

      function updateWeightHidden(prefix, inputId) {
        $(inputId).value = weightFromWheels(prefix).toFixed(2);
      }

      function setWeightWheels(prefix, value) {
        let numeric = clamp(safeNumber(value, 55), 40, 60);
        let whole = Math.floor(numeric);
        let decimal = Math.round((numeric - whole) * 100);
        if (decimal >= 100) {
          whole += 1;
          decimal = 0;
        }
        whole = clamp(whole, 40, 60);
        if (whole === 60) decimal = 0;
        setWheelValue(`${prefix}WeightWholeWheel`, whole);
        setWheelValue(`${prefix}WeightDecimalWheel`, decimal);
        updateWeightHidden(prefix, `${prefix}WeightInput`);
      }

      function renderIeltsDraftSummary() {
        if (!$('ieltsDraftTotal')) return;
        const actual = clamp(Math.floor(safeNumber($("todayNewWords").value)), 0, 200);
        const selectedCount = ["skillListening", "skillReading", "skillWriting", "skillSpeaking"]
          .filter(id => $(id).checked).length;
        const bonus = selectedCount * 20;
        $("ieltsDraftActual").textContent = actual;
        $("ieltsDraftSkill").textContent = bonus;
        $("ieltsDraftTotal").textContent = actual + bonus;
      }

      function renderWeightDraftSummary() {
        if (!$('healthTodayScore')) return;
        const score = ["noLateSnack", "noMilkTea", "eightyFull", "exerciseDone"]
          .filter(id => $(id).checked).length;
        $("healthTodayScore").textContent = `${score} / 4`;
      }

      function initializeWheelPickers() {
        buildWheel("wordWheelColumn", 0, 200, value => String(value), value => {
          $("todayNewWords").value = value;
          renderIeltsDraftSummary();
        });

        const prefix = "today";
        buildWheel(`${prefix}WeightWholeWheel`, 40, 60, value => String(value), value => {
          if (value === 60 && wheelValue(`${prefix}WeightDecimalWheel`) > 0) {
            setWheelValue(`${prefix}WeightDecimalWheel`, 0);
          }
          updateWeightHidden(prefix, `${prefix}WeightInput`);
        });
        buildWheel(`${prefix}WeightDecimalWheel`, 0, 99, value => String(value).padStart(2, "0"), () => {
          if (wheelValue(`${prefix}WeightWholeWheel`) === 60 && wheelValue(`${prefix}WeightDecimalWheel`) > 0) {
            setWheelValue(`${prefix}WeightDecimalWheel`, 0);
          }
          updateWeightHidden(prefix, `${prefix}WeightInput`);
        });
      }

      function renderHeaderAndRewards() {
        const user = state.settings.userName || "罗思维";
        const author = state.settings.authorName || "董纪君";
        const balance = milestoneBalance();
        const gifts = availableGiftCount();

                $("greeting").textContent = `For ${user}`;
        $("heroDate").textContent = formatDate(new Date(), { weekday: "long" });

        $("topBalance").textContent = balance;

        $("ledgerEarned").textContent = state.milestones.length;
        $("ledgerRedeemed").textContent = state.redemptions.length;
        $("ledgerAvailable").textContent = balance;

        $("modalEarned").textContent = state.milestones.length;
        $("modalUsed").textContent = state.redemptions.length * 2;
        $("modalAvailable").textContent = balance;

        const rewardSummary = $("rewardSummary");
        if (gifts > 0) {
          rewardSummary.hidden = false;
          $("rewardSummaryText").textContent =
            gifts === 1
              ? `已经可以向${author}兑换一份神秘礼品。`
              : `已经可以向${author}兑换${gifts}份神秘礼品。`;
          $("redeemHome").textContent = gifts === 1 ? "Redeem" : `Redeem × ${gifts}`;
        } else {
          rewardSummary.hidden = true;
        }

        document.querySelector("footer .footer-inner span:first-child").textContent =
          `Made for ${user} by ${author}.`;
      }

      function renderIeltsCard() {
        const countdown = getExamCountdown();
        const today = ensureDaily();
        const todayEffective = effectiveWordsForEntry(today);
        const total = totalWords();
        const progress = currentRound(total, 500);

        $("ieltsPrimary").firstChild.nodeValue = countdown.primary;
        $("ieltsPrimaryLabel").textContent = countdown.label;
        $("todayWords").textContent = `${todayEffective} words`;
        $("totalWords").textContent = `${total.toLocaleString()} words`;
        $("wordProgressText").textContent = `${progress} / 500`;
        $("wordProgressFill").style.width = `${progress / 500 * 100}%`;
      }

      function renderWeightCard() {
        const first = firstWeight();
        const latest = latestWeight();
        const target = clamp(safeNumber(state.settings.targetWeight, 55), 40, 60);
        const points = totalHealthPoints();
        const progress = currentRound(points, 50);

        if (latest) {
          $("weightPrimary").firstChild.nodeValue = latest.value.toFixed(2);
          $("weightPrimaryLabel").textContent = `kg · ${formatShortDate(latest.date)}`;
        } else {
          $("weightPrimary").firstChild.nodeValue = "—";
          $("weightPrimaryLabel").textContent = "Today";
        }

        $("targetWeightCard").textContent = `${target.toFixed(2)} kg`;

        if (first && latest) {
          const diff = latest.value - first.value;
          const sign = diff > 0 ? "+" : diff < 0 ? "−" : "";
          $("weightChangeCard").textContent = `${sign}${Math.abs(diff).toFixed(2)} kg`;
        } else {
          $("weightChangeCard").textContent = "—";
        }

        $("healthProgressText").textContent = `${progress} / 50`;
        $("healthProgressFill").style.width = `${progress / 50 * 100}%`;
        drawWeightSparkline();
      }

      function renderSleepCard() {
        const run = currentSleepRun();
        const best = bestSleepStreak();
        const progress = run.streak % 14;

        $("sleepPrimary").firstChild.nodeValue = String(run.streak);
        $("sleepPrimaryLabel").textContent = "Current streak";
        $("tonightStatus").textContent = tonightDisplayStatus();
        $("bestSleepStreak").textContent = `${best} days`;
        $("sleepProgressText").textContent = `${progress} / 14`;
        $("sleepProgressFill").style.width = `${progress / 14 * 100}%`;
      }

      function renderIeltsDetail() {
        const today = ensureDaily();
        const total = totalWords();
        const progress = currentRound(total, 500);
        const actual = clamp(Math.floor(safeNumber(today.words.newWords)), 0, 200);

        $("examDateInput").value = state.settings.examDate || "";
        setWheelValue("wordWheelColumn", actual);
        $("todayNewWords").value = actual;
        $("skillListening").checked = Boolean(today.words.skills.listening);
        $("skillReading").checked = Boolean(today.words.skills.reading);
        $("skillWriting").checked = Boolean(today.words.skills.writing);
        $("skillSpeaking").checked = Boolean(today.words.skills.speaking);
        renderIeltsDraftSummary();

        $("ieltsTotalDetail").textContent = `${total.toLocaleString()}词`;
        $("ieltsMilestonesDetail").textContent = `${milestonesFor("ielts").length}次`;
        $("ieltsRemainingDetail").textContent = `${remainingToNext(total, 500)}词`;
        $("ieltsDetailProgressText").textContent = `${progress} / 500`;
        $("ieltsDetailProgressFill").style.width = `${progress / 500 * 100}%`;

        const rows = Object.keys(state.daily)
          .sort()
          .reverse()
          .filter(key => effectiveWordsForEntry(state.daily[key]) > 0)
          .slice(0, 12)
          .map(key => {
            const words = state.daily[key].words || {};
            const actualWords = Math.max(0, Math.floor(safeNumber(words.newWords)));
            const skillLabels = selectedIeltsSkills(words).map(item => item.label);
            const bonus = skillLabels.length * 20;
            const totalForDay = actualWords + bonus;
            const details = [];
            if (actualWords > 0) details.push(`新增${actualWords}词`);
            if (skillLabels.length) details.push(skillLabels.join(" · "));
            if (bonus > 0) details.push(`专项折算${bonus}词`);

            return `
              <div class="history-row">
                <div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div>
                <div class="note">${escapeHtml(details.join(" · ") || "雅思学习")}</div>
                <div class="value">+${totalForDay}</div>
              </div>
            `;
          });

        $("ieltsHistory").innerHTML = rows.length
          ? rows.join("")
          : `<div class="empty-state">还没有雅思学习记录。</div>`;
      }

      function weightGoalProgress() {
        const first = firstWeight();
        const latest = latestWeight();
        const target = clamp(safeNumber(state.settings.targetWeight, 55), 40, 60);

        if (!first || !latest || first.value === target) {
          return { ratio: 0, text: "尚无足够数据" };
        }

        const direction = target < first.value ? -1 : 1;
        const totalDistance = Math.abs(target - first.value);
        const completed = direction === -1
          ? first.value - latest.value
          : latest.value - first.value;

        const ratio = clamp(completed / totalDistance, 0, 1);
        return {
          ratio,
          text: `${Math.round(ratio * 100)}%`
        };
      }

      function renderWeightDetail() {
        const today = ensureDaily();
        const latest = latestWeight();
        const target = clamp(safeNumber(state.settings.targetWeight, 55), 40, 60);
        const todayValue = safeNumber(today.weight.value, NaN);
        const pickerValue = Number.isFinite(todayValue) && todayValue >= 40 && todayValue <= 60
          ? todayValue
          : latest?.value || target;

        $("targetWeightInput").value = target.toFixed(2);
        setWeightWheels("today", pickerValue);

        $("noLateSnack").checked = Boolean(today.weight.noLateSnack);
        $("noMilkTea").checked = Boolean(today.weight.noMilkTea);
        $("eightyFull").checked = Boolean(today.weight.eightyFull);
        $("exerciseDone").checked = Boolean(today.weight.exerciseDone);
        renderWeightDraftSummary();

        const rows = Object.keys(state.daily)
          .sort()
          .reverse()
          .filter(key => {
            const entry = state.daily[key];
            return safeNumber(entry?.weight?.value, NaN) > 0 || healthPointsForEntry(entry) > 0;
          })
          .slice(0, 14)
          .map(key => {
            const entry = state.daily[key];
            const value = safeNumber(entry.weight.value, NaN);
            const choices = HEALTH_CHOICES
              .filter(item => Boolean(entry.weight?.[item.key]))
              .map(item => item.label);
            const note = choices.length === HEALTH_CHOICES.length
              ? "完美的一天！"
              : choices.length
                ? `${choices.join(" · ")} · ${choices.length}分`
                : "未勾选健康项目";

            return `
              <div class="history-row">
                <div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div>
                <div class="note">${escapeHtml(note)}</div>
                <div class="value">${Number.isFinite(value) && value > 0 ? `${value.toFixed(2)} kg` : "—"}</div>
              </div>
            `;
          });

        $("weightHistory").innerHTML = rows.length
          ? rows.join("")
          : `<div class="empty-state">还没有体重或健康选择记录。</div>`;

        drawWeightChart();
      }

      function renderSleepDetail() {
        const run = currentSleepRun();
        const best = bestSleepStreak();
        const progress = run.streak % 14;
        const windowInfo = getNightWindow();
        const nightKey = windowInfo.canCheckIn ? windowInfo.nightKey : tonightNightKey();
        const relaxNight = isRelaxNightKey(nightKey);
        const checked = windowInfo.canCheckIn && Boolean(state.sleepCheckins[windowInfo.nightKey]);

        $("weekendStatusBanner").hidden = !relaxNight;
        $("sleepModalStreak").textContent = `${run.streak} ${run.streak === 1 ? "day" : "days"}`;

        if (checked) {
          $("sleepWindowText").textContent = `本晚已于${new Intl.DateTimeFormat("zh-CN", { hour: "2-digit", minute: "2-digit" }).format(new Date(state.sleepCheckins[windowInfo.nightKey]))}完成打卡。`;
        } else if (relaxNight) {
          $("sleepWindowText").textContent = windowInfo.canCheckIn
            ? "今晚可以放松，也可以主动打卡增加1天。"
            : "今晚是周末放松日，20:00后可以自愿打卡。";
        } else {
          $("sleepWindowText").textContent = windowInfo.message;
        }

        $("sleepCheckinButton").disabled = !windowInfo.canCheckIn || checked;
        $("sleepCheckinButton").textContent = checked
          ? "今晚已完成"
          : windowInfo.canCheckIn
            ? relaxNight ? "今晚仍要打卡" : "完成今晚打卡"
            : relaxNight ? "今晚可放松" : "当前不在打卡时间";

        $("sleepCurrentDetail").textContent = `${run.streak}天`;
        $("sleepBestDetail").textContent = `${best}天`;
        $("sleepMilestonesDetail").textContent = `${milestonesFor("sleep").length}次`;
        $("sleepDetailProgressText").textContent = `${progress} / 14`;
        $("sleepDetailProgressFill").style.width = `${progress / 14 * 100}%`;

        const reference = sleepReferenceKey();
        const recentKeys = [];
        for (let i = 0; i < 14; i += 1) recentKeys.push(addDaysToKey(reference, -i));

        $("sleepHistory").innerHTML = recentKeys.map(key => {
          const checkedAt = state.sleepCheckins[key];
          const relax = isRelaxNightKey(key);
          let value = "Missed";
          let note = "未完成";

          if (checkedAt) {
            value = new Intl.DateTimeFormat("zh-CN", { hour: "2-digit", minute: "2-digit" }).format(new Date(checkedAt));
            note = relax ? "Relax · 已打卡" : "Success";
          } else if (relax) {
            value = "Relax";
            note = "周末放松 · 不影响连续";
          }

          return `
            <div class="history-row">
              <div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div>
              <div class="note">${escapeHtml(note)}</div>
              <div class="value">${escapeHtml(value)}</div>
            </div>
          `;
        }).join("");
      }

      function renderLedger() {
        const combined = [
          ...state.milestones.map(item => ({
            type: "milestone",
            createdAt: item.createdAt,
            item
          })),
          ...state.redemptions.map(item => ({
            type: "gift",
            createdAt: item.createdAt,
            item
          }))
        ].sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));

        $("recentList").innerHTML = combined.length
          ? combined.slice(0, 8).map(renderRecentItem).join("")
          : `<div class="empty-state">完成第一个500词、50个健康选择或连续14晚后，里程记录会出现在这里。</div>`;

        $("milestoneHistory").innerHTML = state.milestones.length
          ? [...state.milestones]
              .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
              .map(item => `
                <div class="history-row">
                  <div class="date">${escapeHtml(formatShortDate(new Date(item.createdAt)))}</div>
                  <div class="note">${escapeHtml(sourceLabel(item.source))} · ${escapeHtml(item.detail)}</div>
                  <div class="value">+1</div>
                </div>
              `).join("")
          : `<div class="empty-state">还没有获得里程积分。</div>`;

        $("redeemLedger").disabled = milestoneBalance() < 2;
        $("redeemLedger").textContent = milestoneBalance() >= 2
          ? "使用2个积分兑换"
          : "积分不足2个";

        $("giftRecords").innerHTML = state.redemptions.length
          ? [...state.redemptions]
              .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
              .map(record => `
                <div class="gift-record">
                  <div class="gift-record-top">
                    <div>
                      <strong>${escapeHtml(record.code)}</strong>
                      <span>${escapeHtml(formatDateTime(record.createdAt))}</span>
                    </div>
                    <button class="status-tag ${record.status === "claimed" ? "claimed" : ""}"
                      type="button" data-gift-status="${escapeHtml(record.id)}">
                      ${record.status === "claimed" ? "已领取" : "待领取"}
                    </button>
                  </div>
                </div>
              `).join("")
          : `<div class="empty-state">还没有生成礼品兑换卡。</div>`;

        document.querySelectorAll("[data-gift-status]").forEach(button => {
          button.addEventListener("click", () => toggleGiftStatus(button.dataset.giftStatus));
        });
      }

      function renderRecentItem(entry) {
        if (entry.type === "gift") {
          return `
            <div class="recent-item">
              <div class="source-dot gift">G</div>
              <div class="recent-main">
                <strong>神秘礼品兑换卡</strong>
                <span>${escapeHtml(entry.item.code)} · 使用2个积分</span>
              </div>
              <div class="recent-date">${escapeHtml(formatShortDate(new Date(entry.createdAt)))}</div>
            </div>
          `;
        }

        const sourceClass = entry.item.source;
        const initial = sourceLabel(entry.item.source).charAt(0);

        return `
          <div class="recent-item">
            <div class="source-dot ${sourceClass}">${initial}</div>
            <div class="recent-main">
              <strong>${escapeHtml(entry.item.title)}</strong>
              <span>${escapeHtml(entry.item.detail)} · +1积分</span>
            </div>
            <div class="recent-date">${escapeHtml(formatShortDate(new Date(entry.createdAt)))}</div>
          </div>
        `;
      }

      function recordedDayCount() {
        const keys = new Set();
        Object.entries(state.daily).forEach(([key, entry]) => {
          const hasWords = effectiveWordsForEntry(entry) > 0;
          const hasWeight = safeNumber(entry?.weight?.value, NaN) > 0;
          const hasHealth = healthPointsForEntry(entry) > 0;
          if (hasWords || hasWeight || hasHealth) keys.add(key);
        });
        Object.keys(state.sleepCheckins).forEach(key => keys.add(key));
        return keys.size;
      }

      function renderBackupStatus() {
        const last = state.settings.lastBackupAt ? new Date(state.settings.lastBackupAt) : null;
        $("lastBackupText").textContent = last && !Number.isNaN(last.getTime())
          ? formatDateTime(last.toISOString())
          : "从未备份";
        $("recordedDaysText").textContent = `${recordedDayCount()}天`;

        if (!last || Number.isNaN(last.getTime())) {
          $("backupAdviceText").textContent = "建议首次记录后导出一份备份，避免更换设备或清理浏览器后丢失数据。";
          return;
        }

        const days = Math.floor((Date.now() - last.getTime()) / 86400000);
        $("backupAdviceText").textContent = days >= 30
          ? `距离上次备份已${days}天，建议现在重新导出。`
          : `备份状态良好，最近一次导出距今${Math.max(0, days)}天。`;
      }

      function prefillSettings() {
        $("settingsExamDate").value = state.settings.examDate || "";
        $("settingsTargetWeight").value = clamp(safeNumber(state.settings.targetWeight, 55), 40, 60).toFixed(2);
        $("cardOrderInput").value = state.settings.cardOrder || "ielts,weight,sleep";
        $("colorStyleInput").value = state.settings.colorStyle || "color";
        $("showHistoryInput").value = state.settings.showHistory === false ? "hide" : "show";
        renderBackupStatus();
      }

      function prefillModal(id) {
        if (id === "ieltsModal") renderIeltsDetail();
        if (id === "weightModal") renderWeightDetail();
        if (id === "sleepModal") renderSleepDetail();
        if (id === "ledgerModal") renderLedger();
        if (id === "settingsModal") prefillSettings();
      }

      function submitIelts(event) {
        event.preventDefault();

        const entry = ensureDaily();
        entry.words.newWords = clamp(Math.floor(safeNumber($("todayNewWords").value)), 0, 200);
        entry.words.reviewWords = 0;
        entry.words.note = "";
        entry.words.skills = {
          listening: $("skillListening").checked,
          reading: $("skillReading").checked,
          writing: $("skillWriting").checked,
          speaking: $("skillSpeaking").checked
        };
        state.settings.examDate = $("examDateInput").value;

        saveState();
        const added = syncWordMilestones();
        saveState();
        closeModal($("ieltsModal"));
        renderAll();
        showToast("雅思学习记录已保存");

        if (added.length) {
          added.forEach(item => enqueuePopup({ type: "milestone", item }));
          const balanceBeforeAdded = milestoneBalance() - added.length;
          if (Math.floor(milestoneBalance() / 2) > Math.floor(balanceBeforeAdded / 2)) {
            enqueuePopup({ type: "gift-unlocked" });
          }
        }
      }

      function submitWeight(event) {
        event.preventDefault();

        const entry = ensureDaily();
        entry.weight.value = Number(clamp(safeNumber($("todayWeightInput").value, 55), 40, 60).toFixed(2));
        entry.weight.noLateSnack = $("noLateSnack").checked;
        entry.weight.noMilkTea = $("noMilkTea").checked;
        entry.weight.eightyFull = $("eightyFull").checked;
        entry.weight.exerciseDone = $("exerciseDone").checked;
        state.settings.targetWeight = Number(clamp(safeNumber($("targetWeightInput").value, 55), 40, 60).toFixed(2));

        saveState();
        const added = syncHealthMilestones();
        saveState();
        closeModal($("weightModal"));
        renderAll();
        showToast("体重与健康选择已保存");

        if (added.length) {
          added.forEach(item => enqueuePopup({ type: "milestone", item }));
          const balanceBeforeAdded = milestoneBalance() - added.length;
          if (Math.floor(milestoneBalance() / 2) > Math.floor(balanceBeforeAdded / 2)) {
            enqueuePopup({ type: "gift-unlocked" });
          }
        }
      }

      function performSleepCheckin() {
        const windowInfo = getNightWindow();

        if (!windowInfo.canCheckIn) {
          showToast("早睡打卡时间为每天20:00至次日00:59");
          return;
        }

        if (state.sleepCheckins[windowInfo.nightKey]) {
          showToast("今晚已经完成打卡");
          return;
        }

        state.sleepCheckins[windowInfo.nightKey] = new Date().toISOString();
        saveState();

        const added = syncSleepMilestones();
        saveState();
        renderAll();
        renderSleepDetail();
        showToast("早睡打卡成功");

        if (added.length) {
          added.forEach(item => enqueuePopup({ type: "milestone", item }));

          const balanceBeforeAdded = milestoneBalance() - added.length;
          if (Math.floor(milestoneBalance() / 2) > Math.floor(balanceBeforeAdded / 2)) {
            enqueuePopup({ type: "gift-unlocked" });
          }
        }
      }

      function submitSettings(event) {
        event.preventDefault();

        state.settings.userName = "罗思维";
        state.settings.authorName = "董纪君";
        state.settings.sleepStart = "20:00";
        state.settings.sleepDeadline = "01:00";
        state.settings.examDate = $("settingsExamDate").value;
        state.settings.targetWeight = Number(clamp(safeNumber($("settingsTargetWeight").value, 55), 40, 60).toFixed(2));
        state.settings.cardOrder = $("cardOrderInput").value || "ielts,weight,sleep";
        state.settings.colorStyle = $("colorStyleInput").value || "color";
        state.settings.showHistory = $("showHistoryInput").value !== "hide";

        saveState();
        closeModal($("settingsModal"));
        renderAll();
        showToast("设置已保存");
      }

      function toggleGiftStatus(id) {
        const record = state.redemptions.find(item => item.id === id);
        if (!record) return;

        if (record.status === "pending") {
          const confirmed = window.confirm("确认这份神秘礼品已经领取了吗？");
          if (!confirmed) return;
          record.status = "claimed";
          record.claimedAt = new Date().toISOString();
        } else {
          const confirmed = window.confirm("要将状态恢复为待领取吗？");
          if (!confirmed) return;
          record.status = "pending";
          record.claimedAt = null;
        }

        saveState();
        renderAll();
        renderLedger();
      }

      async function exportData() {
        const payload = {
          app: "Three",
          version: APP_VERSION,
          exportedAt: new Date().toISOString(),
          data: state
        };

        const filename = `three-${state.settings.userName || "data"}-${todayKey()}.json`;
        const content = JSON.stringify(payload, null, 2);
        const blob = new Blob([content], { type: "application/json" });

        try {
          const file = new File([blob], filename, { type: "application/json" });

          if (
            navigator.share &&
            navigator.canShare &&
            navigator.canShare({ files: [file] })
          ) {
            await navigator.share({
              title: "Three 数据备份",
              text: `${state.settings.userName || "罗思维"}的Three打卡数据`,
              files: [file]
            });
            state.settings.lastBackupAt = new Date().toISOString();
            saveState();
            if ($("settingsModal").classList.contains("open")) renderBackupStatus();
            showToast("数据已成功分享");
            return;
          }
        } catch (error) {
          if (error && error.name === "AbortError") return;
          console.warn("Native share unavailable:", error);
        }

        const url = URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.href = url;
        link.download = filename;
        document.body.appendChild(link);
        link.click();
        link.remove();
        window.setTimeout(() => URL.revokeObjectURL(url), 1000);
        state.settings.lastBackupAt = new Date().toISOString();
        saveState();
        if ($("settingsModal").classList.contains("open")) renderBackupStatus();
        showToast("JSON备份已导出");
      }

      function importData(file) {
        if (!file) return;

        const reader = new FileReader();

        reader.onload = () => {
          try {
            const parsed = JSON.parse(String(reader.result));
            const imported = parsed.data || parsed;

            if (!imported || typeof imported !== "object") {
              throw new Error("Invalid backup");
            }

            state = {
              version: APP_VERSION,
              settings: {
                ...cloneDefault().settings,
                ...(imported.settings || {}),
                userName: "罗思维",
                authorName: "董纪君",
                sleepStart: "20:00",
                sleepDeadline: "01:00"
              },
              daily: imported.daily || {},
              sleepCheckins: imported.sleepCheckins || {},
              milestones: Array.isArray(imported.milestones) ? imported.milestones : [],
              redemptions: Array.isArray(imported.redemptions) ? imported.redemptions : []
            };

            syncAllMilestones(false);
            saveState();
            renderAll();
            showToast("备份已导入");
          } catch (error) {
            console.error(error);
            window.alert("无法读取该文件。请选择由本网站导出的JSON备份。");
          } finally {
            $("importFile").value = "";
          }
        };

        reader.readAsText(file);
      }

      function clearAllData() {
        const confirmed = window.confirm("确定要清空所有记录、里程积分和兑换卡吗？此操作无法撤销。");
        if (!confirmed) return;

        const secondConfirmed = window.confirm("请再次确认：真的要清空全部数据吗？");
        if (!secondConfirmed) return;

        state = cloneDefault();
        saveState();
        closeModal($("settingsModal"));
        renderAll();
        showToast("全部数据已清空");
      }

      function setupCanvas(canvas) {
        const rect = canvas.getBoundingClientRect();
        const dpr = Math.max(1, window.devicePixelRatio || 1);

        canvas.width = Math.max(1, Math.floor(rect.width * dpr));
        canvas.height = Math.max(1, Math.floor(rect.height * dpr));

        const ctx = canvas.getContext("2d");
        ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

        return { ctx, width: rect.width, height: rect.height };
      }

      function drawWeightSparkline() {
        const canvas = $("weightSparkline");
        const points = weightEntries().slice(-12);
        const { ctx, width, height } = setupCanvas(canvas);

        ctx.clearRect(0, 0, width, height);

        if (points.length < 2) {
          ctx.strokeStyle = "rgba(0,0,0,.08)";
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(0, height / 2);
          ctx.lineTo(width, height / 2);
          ctx.stroke();
          return;
        }

        const values = points.map(item => item.value);
        let min = Math.min(...values);
        let max = Math.max(...values);

        if (max === min) {
          max += .5;
          min -= .5;
        }

        const x = index => index / (points.length - 1) * width;
        const y = value => 5 + (1 - (value - min) / (max - min)) * (height - 10);

        ctx.beginPath();
        points.forEach((point, index) => {
          if (index === 0) ctx.moveTo(x(index), y(point.value));
          else ctx.lineTo(x(index), y(point.value));
        });
        ctx.strokeStyle = "rgba(36,161,72,.65)";
        ctx.lineWidth = 2;
        ctx.lineCap = "round";
        ctx.lineJoin = "round";
        ctx.stroke();
      }

      function drawWeightChart() {
        const canvas = $("weightChart");
        const points = weightEntries();
        const { ctx, width, height } = setupCanvas(canvas);
        ctx.clearRect(0, 0, width, height);

        $("weightChartEmpty").style.display = points.length >= 2 ? "none" : "grid";
        if (points.length < 2) return;

        const padding = { top: 24, right: 22, bottom: 34, left: 24 };
        const chartWidth = width - padding.left - padding.right;
        const chartHeight = height - padding.top - padding.bottom;
        const values = points.map(item => item.value);

        let min = Math.min(...values);
        let max = Math.max(...values);

        if (max === min) {
          max += .5;
          min -= .5;
        }

        const buffer = Math.max(.3, (max - min) * .2);
        min -= buffer;
        max += buffer;

        const x = index => padding.left + index / (points.length - 1) * chartWidth;
        const y = value => padding.top + (1 - (value - min) / (max - min)) * chartHeight;

        ctx.strokeStyle = "rgba(0,0,0,.06)";
        ctx.lineWidth = 1;

        for (let i = 0; i < 3; i += 1) {
          const gridY = padding.top + i * (chartHeight / 2);
          ctx.beginPath();
          ctx.moveTo(padding.left, gridY);
          ctx.lineTo(width - padding.right, gridY);
          ctx.stroke();
        }

        const gradient = ctx.createLinearGradient(0, padding.top, 0, height - padding.bottom);
        gradient.addColorStop(0, "rgba(36,161,72,.20)");
        gradient.addColorStop(1, "rgba(36,161,72,0)");

        ctx.beginPath();
        points.forEach((point, index) => {
          if (index === 0) ctx.moveTo(x(index), y(point.value));
          else ctx.lineTo(x(index), y(point.value));
        });
        ctx.lineTo(x(points.length - 1), height - padding.bottom);
        ctx.lineTo(x(0), height - padding.bottom);
        ctx.closePath();
        ctx.fillStyle = gradient;
        ctx.fill();

        ctx.beginPath();
        points.forEach((point, index) => {
          if (index === 0) ctx.moveTo(x(index), y(point.value));
          else ctx.lineTo(x(index), y(point.value));
        });
        ctx.strokeStyle = "rgba(36,161,72,1)";
        ctx.lineWidth = 2.5;
        ctx.lineCap = "round";
        ctx.lineJoin = "round";
        ctx.stroke();

        points.forEach((point, index) => {
          ctx.beginPath();
          ctx.arc(x(index), y(point.value), index === points.length - 1 ? 4 : 2.5, 0, Math.PI * 2);
          ctx.fillStyle = "rgba(36,161,72,1)";
          ctx.fill();
        });

        ctx.fillStyle = "rgba(0,0,0,.44)";
        ctx.font = "11px -apple-system, BlinkMacSystemFont, sans-serif";

        ctx.textAlign = "left";
        ctx.fillText(`${points[0].value.toFixed(2)} kg`, padding.left, 12);
        ctx.fillText(formatShortDate(points[0].date), padding.left, height - 8);

        ctx.textAlign = "right";
        ctx.fillText(`${points[points.length - 1].value.toFixed(2)} kg`, width - padding.right, 12);
        ctx.fillText(formatShortDate(points[points.length - 1].date), width - padding.right, height - 8);
      }

      function applyHomeSettings() {
        const order = String(state.settings.cardOrder || "ielts,weight,sleep").split(",");
        const grid = document.querySelector(".cards-grid");
        order.forEach(type => {
          const card = grid?.querySelector(`[data-card-type="${type}"]`);
          if (card) grid.appendChild(card);
        });
        document.body.classList.toggle("monochrome", state.settings.colorStyle === "mono");
        $("ledgerSection").hidden = state.settings.showHistory === false;
      }

      function renderAll() {
        applyHomeSettings();
        renderHeaderAndRewards();
        renderIeltsCard();
        renderWeightCard();
        renderSleepCard();
        renderLedger();
      }

      function bindOpenableCards() {
        document.querySelectorAll("[data-open]").forEach(element => {
          const open = () => openModal(element.dataset.open);

          element.addEventListener("click", open);
          element.addEventListener("keydown", event => {
            if (event.key === "Enter" || event.key === " ") {
              event.preventDefault();
              open();
            }
          });
        });
      }

      function bindEvents() {
        bindOpenableCards();

        document.querySelectorAll("[data-close]").forEach(button => {
          button.addEventListener("click", () => {
            const layer = button.closest(".modal-layer");
            if (layer) closeModal(layer);
          });
        });

        document.querySelectorAll(".modal-layer:not(#celebrationLayer)").forEach(layer => {
          layer.addEventListener("click", event => {
            if (event.target === layer) closeModal(layer);
          });
        });

        document.addEventListener("keydown", event => {
          if (event.key !== "Escape") return;

          const openLayer = [...document.querySelectorAll(".modal-layer.open")]
            .find(layer => layer.id !== "celebrationLayer");

          if (openLayer) closeModal(openLayer);
        });

        $("openLedger").addEventListener("click", () => openModal("ledgerModal"));
        $("openSettings").addEventListener("click", () => openModal("settingsModal"));
        $("redeemHome").addEventListener("click", () => redeemGift(true));
        $("redeemLedger").addEventListener("click", () => redeemGift(true));

        $("ieltsForm").addEventListener("submit", submitIelts);
        $("weightForm").addEventListener("submit", submitWeight);
        $("settingsForm").addEventListener("submit", submitSettings);
        $("sleepCheckinButton").addEventListener("click", performSleepCheckin);

        ["skillListening", "skillReading", "skillWriting", "skillSpeaking"]
          .forEach(id => $(id).addEventListener("change", renderIeltsDraftSummary));
        ["noLateSnack", "noMilkTea", "eightyFull", "exerciseDone"]
          .forEach(id => $(id).addEventListener("change", renderWeightDraftSummary));

        $("quickExport").addEventListener("click", exportData);
        $("exportData").addEventListener("click", exportData);
        $("importData").addEventListener("click", () => $("importFile").click());
        $("importFile").addEventListener("change", event => importData(event.target.files[0]));
        $("clearData").addEventListener("click", clearAllData);

        window.addEventListener("scroll", () => {
          $("topbar").classList.toggle("scrolled", window.scrollY > 8);
        }, { passive: true });

        let resizeTimer = null;
        window.addEventListener("resize", () => {
          window.clearTimeout(resizeTimer);
          resizeTimer = window.setTimeout(() => {
            drawWeightSparkline();
            if ($("weightModal").classList.contains("open")) drawWeightChart();
          }, 120);
        });
      }

      function init() {
        initializeWheelPickers();
        ensureDaily();
        syncAllMilestones(false);
        saveState();
        bindEvents();
        renderAll();

        requestAnimationFrame(() => {
          $("app").classList.add("ready");
        });
      }

      init();
    })();
  </script>
</body>
</html>
