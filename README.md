<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <meta name="theme-color" content="#f5f5f7" />
  <meta name="color-scheme" content="light" />
  <title>Three · 罗思维</title>
  <style>
    :root {
      --bg: #f5f5f7;
      --surface: rgba(255, 255, 255, 0.9);
      --surface-solid: #fff;
      --text: #1d1d1f;
      --secondary: #6e6e73;
      --tertiary: #86868b;
      --line: rgba(0, 0, 0, 0.08);
      --line-strong: rgba(0, 0, 0, 0.14);
      --blue: #0071e3;
      --green: #24a148;
      --purple: #6757d9;
      --danger: #d92d20;
      --shadow: 0 24px 70px rgba(0, 0, 0, 0.07);
      --shadow-soft: 0 12px 36px rgba(0, 0, 0, 0.045);
      --radius-xl: 30px;
      --radius-lg: 24px;
      --radius-md: 16px;
      --page: 1180px;
      --ease: cubic-bezier(.2, .8, .2, 1);
    }

    * { box-sizing: border-box; }

    html {
      min-width: 320px;
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

    body.modal-open { overflow: hidden; }
    body.monochrome { --blue: #1d1d1f; --green: #1d1d1f; --purple: #1d1d1f; }

    button, input, select, textarea { font: inherit; }
    button { border: 0; cursor: pointer; }
    button:focus-visible, input:focus-visible, select:focus-visible, textarea:focus-visible {
      outline: 3px solid rgba(0, 113, 227, .22);
      outline-offset: 2px;
    }

    [hidden] { display: none !important; }

    .container {
      width: min(calc(100% - 40px), var(--page));
      margin: 0 auto;
    }

    .app {
      opacity: 0;
      transform: translateY(8px);
      transition: opacity .5s var(--ease), transform .5s var(--ease);
    }

    .app.ready { opacity: 1; transform: none; }

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

    .top-actions { display: flex; align-items: center; gap: 9px; }

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

    .milestone-pill:hover { transform: translateY(-1px); background: white; }

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

    .icon-button:hover { transform: translateY(-1px); background: white; }
    .icon-button svg { width: 18px; height: 18px; fill: none; stroke: currentColor; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }

    main { padding: 66px 0 82px; }

    .hero { display: block; margin-bottom: 30px; }

    .hero-meta-line {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
      margin-bottom: 16px;
    }

    .eyebrow {
      margin: 0;
      color: var(--text);
      font-size: 15px;
      font-weight: 680;
    }

    .hero-date { color: var(--secondary); font-size: 12px; text-align: right; white-space: nowrap; }

    h1 {
      margin: 0;
      font-size: clamp(48px, 8vw, 84px);
      line-height: .96;
      letter-spacing: -.06em;
      font-weight: 720;
    }

    h1 span { color: var(--secondary); }

    .hero-copy {
      max-width: 660px;
      margin: 22px 0 0;
      color: var(--secondary);
      font-size: clamp(16px, 2vw, 20px);
      line-height: 1.58;
      letter-spacing: -.02em;
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

    .reward-title { font-size: 17px; font-weight: 680; letter-spacing: -.01em; }
    .reward-note { margin-top: 6px; color: var(--secondary); font-size: 13px; line-height: 1.5; }
    .reward-metrics { display: flex; align-items: center; gap: 24px; }
    .reward-metric { text-align: right; }
    .reward-metric strong { display: block; font-size: 30px; line-height: 1; letter-spacing: -.045em; }
    .reward-metric span { display: block; margin-top: 6px; color: var(--secondary); font-size: 11px; white-space: nowrap; }

    .reward-button {
      min-height: 40px;
      padding: 0 16px;
      border-radius: 999px;
      color: white;
      background: var(--text);
      font-size: 13px;
      font-weight: 680;
    }

    .cards-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 20px;
      margin-bottom: 64px;
    }

    .habit-card {
      position: relative;
      min-width: 0;
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
      user-select: none;
    }

    .habit-card:hover { transform: translateY(-4px); box-shadow: var(--shadow); }

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

    .card-head { display: flex; align-items: center; justify-content: space-between; gap: 18px; }
    .card-label { color: var(--secondary); font-size: 12px; font-weight: 760; letter-spacing: .12em; }

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

    .habit-card:hover .card-arrow { transform: translateX(2px); background: rgba(0, 0, 0, .075); }

    .primary-value {
      margin-top: 34px;
      font-size: clamp(46px, 5.2vw, 70px);
      line-height: .92;
      letter-spacing: -.065em;
      font-weight: 690;
      overflow-wrap: anywhere;
    }

    .primary-value small {
      display: block;
      margin-top: 12px;
      color: var(--secondary);
      font-size: 13px;
      line-height: 1.4;
      letter-spacing: 0;
      font-weight: 560;
    }

    .secondary-data { margin-top: 28px; padding-top: 20px; border-top: 1px solid var(--line); }
    .secondary-row { display: flex; justify-content: space-between; gap: 18px; align-items: baseline; font-size: 13px; }
    .secondary-row + .secondary-row { margin-top: 10px; }
    .secondary-row span:first-child { color: var(--secondary); }
    .secondary-row strong { min-width: 0; text-align: right; font-weight: 650; overflow-wrap: anywhere; }

    .progress-area { margin-top: auto; padding-top: 28px; }
    .progress-top { display: flex; justify-content: space-between; gap: 14px; margin-bottom: 10px; color: var(--secondary); font-size: 12px; }
    .progress-top strong { color: var(--text); font-weight: 680; }
    .progress-track { height: 8px; overflow: hidden; border-radius: 999px; background: rgba(0, 0, 0, .065); }
    .progress-fill { width: 0; height: 100%; border-radius: inherit; transition: width .6s var(--ease); }
    .ielts .progress-fill { background: var(--blue); }
    .weight .progress-fill { background: var(--green); }
    .sleep .progress-fill { background: var(--purple); }
    .progress-note { margin-top: 10px; color: var(--secondary); font-size: 12px; line-height: 1.45; }
    .mini-chart { width: 100%; height: 46px; display: block; margin-top: 14px; }

    .section { margin-bottom: 60px; scroll-margin-top: 90px; }
    .section-head { display: flex; justify-content: space-between; align-items: end; gap: 24px; margin-bottom: 22px; }
    .section-head h2 { margin: 0; font-size: clamp(34px, 5vw, 52px); line-height: 1; letter-spacing: -.052em; }
    .section-head p { max-width: 430px; margin: 0; color: var(--secondary); font-size: 14px; line-height: 1.55; text-align: right; }

    .overview-panel {
      padding: 30px;
      border-radius: var(--radius-xl);
      background: var(--surface);
      border: 1px solid rgba(255, 255, 255, .9);
      box-shadow: var(--shadow-soft);
    }

    .ledger-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 0; border-bottom: 1px solid var(--line); }
    .ledger-stat { padding: 4px 28px 28px; min-width: 0; }
    .ledger-stat + .ledger-stat { border-left: 1px solid var(--line); }
    .ledger-stat span { display: block; color: var(--secondary); font-size: 12px; }
    .ledger-stat strong { display: block; margin-top: 10px; font-size: 36px; line-height: 1; letter-spacing: -.045em; overflow-wrap: anywhere; }

    .recent-list { padding-top: 12px; }
    .recent-item { min-height: 72px; padding: 15px 2px; display: grid; grid-template-columns: 42px 1fr auto; align-items: center; gap: 14px; border-bottom: 1px solid var(--line); }
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
    .recent-main strong { display: block; font-size: 14px; }
    .recent-main span { display: block; margin-top: 5px; color: var(--secondary); font-size: 12px; line-height: 1.4; }
    .recent-date { color: var(--secondary); font-size: 12px; text-align: right; }
    .empty-state { padding: 24px 2px; color: var(--secondary); font-size: 13px; line-height: 1.65; }

    footer { padding: 18px 0 52px; color: var(--secondary); font-size: 12px; }
    .footer-inner { padding-top: 22px; display: flex; justify-content: space-between; gap: 18px; border-top: 1px solid var(--line); }

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

    .modal-layer.open { display: flex; animation: fadeIn .2s ease both; }

    .modal {
      width: min(100%, 720px);
      max-height: min(90vh, 860px);
      overflow: auto;
      overscroll-behavior: contain;
      border-radius: 28px;
      background: rgba(255, 255, 255, .97);
      border: 1px solid rgba(255, 255, 255, .95);
      box-shadow: 0 34px 110px rgba(0, 0, 0, .2);
      animation: modalIn .28s var(--ease) both;
    }

    .modal.wide { width: min(100%, 850px); }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    @keyframes modalIn { from { opacity: 0; transform: translateY(12px) scale(.985); } to { opacity: 1; transform: none; } }
    @keyframes mobileSheetIn { from { opacity: 0; transform: translateY(26px); } to { opacity: 1; transform: none; } }

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
      -webkit-backdrop-filter: blur(18px);
      border-bottom: 1px solid var(--line);
    }

    .modal-title { font-size: 21px; font-weight: 680; letter-spacing: -.03em; }
    .close-button { width: 34px; height: 34px; display: grid; place-items: center; border-radius: 50%; background: rgba(0, 0, 0, .055); color: var(--text); font-size: 20px; }
    .modal-body { padding: 26px; }
    .modal-section + .modal-section { margin-top: 28px; padding-top: 26px; border-top: 1px solid var(--line); }
    .modal-section-title { margin-bottom: 16px; font-size: 13px; font-weight: 730; }

    .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
    .field { display: grid; gap: 8px; min-width: 0; }
    .field.full { grid-column: 1 / -1; }
    .field label { color: var(--secondary); font-size: 12px; font-weight: 630; }

    .field input, .field select, .field textarea {
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

    .field textarea { min-height: 92px; resize: vertical; line-height: 1.55; }
    .field input:focus, .field select:focus, .field textarea:focus { background: white; border-color: rgba(0, 113, 227, .5); box-shadow: 0 0 0 4px rgba(0, 113, 227, .08); }

    .fixed-setting {
      width: 100%;
      min-height: 46px;
      padding: 11px 14px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      color: var(--text);
      background: #f7f7f9;
      border: 1px solid var(--line);
      border-radius: 13px;
    }

    .fixed-setting strong { font-size: 14px; font-weight: 680; }
    .fixed-setting span { flex: 0 0 auto; color: var(--secondary); font-size: 11px; }

    .choice-list { display: grid; gap: 10px; }
    .choice { position: relative; }
    .choice input { position: absolute; opacity: 0; pointer-events: none; }
    .choice label { min-height: 52px; padding: 0 15px; display: flex; align-items: center; gap: 12px; border-radius: 15px; background: #f7f7f9; border: 1px solid var(--line); color: var(--text); cursor: pointer; transition: .18s ease; }
    .choice-mark { width: 22px; height: 22px; flex: 0 0 auto; display: grid; place-items: center; border-radius: 7px; border: 1.5px solid var(--line-strong); color: white; font-size: 13px; }
    .choice input:checked + label { background: rgba(36, 161, 72, .08); border-color: rgba(36, 161, 72, .28); }
    .choice input:checked + label .choice-mark { background: var(--green); border-color: var(--green); }

    .summary-box { padding: 20px; display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 14px; border-radius: 18px; background: #f7f7f9; border: 1px solid var(--line); }
    .summary-box > div { min-width: 0; }
    .summary-box span { display: block; color: var(--secondary); font-size: 11px; }
    .summary-box strong { display: block; margin-top: 7px; font-size: 22px; line-height: 1; letter-spacing: -.035em; overflow-wrap: anywhere; }
    .detail-progress { margin-top: 18px; }
    .detail-progress + .detail-progress { margin-top: 22px; }
    .detail-progress .progress-track { height: 7px; }

    .chart-wrap { position: relative; height: 230px; margin-top: 14px; }
    .chart-wrap canvas { width: 100%; height: 100%; display: block; }
    .chart-empty { position: absolute; inset: 0; display: grid; place-items: center; color: var(--secondary); font-size: 13px; text-align: center; pointer-events: none; }

    .history-list { display: grid; gap: 0; }
    .history-row { padding: 14px 0; display: grid; grid-template-columns: 110px 1fr auto; gap: 16px; align-items: center; border-bottom: 1px solid var(--line); font-size: 13px; }
    .history-row:last-child { border-bottom: 0; }
    .history-row .date { color: var(--secondary); }
    .history-row .note { color: var(--secondary); line-height: 1.45; }
    .history-row .value { text-align: right; font-weight: 640; }

    .sleep-status { padding: 26px; border-radius: 20px; background: #f7f7f9; border: 1px solid var(--line); text-align: center; }
    .sleep-status strong { display: block; font-size: 48px; line-height: 1; letter-spacing: -.055em; }
    .sleep-status span { display: block; margin-top: 10px; color: var(--secondary); font-size: 13px; line-height: 1.5; }
    .checkin-button { width: 100%; min-height: 52px; margin-top: 16px; border-radius: 999px; color: white; background: var(--purple); font-weight: 690; }
    .checkin-button:disabled { cursor: not-allowed; color: var(--secondary); background: rgba(0, 0, 0, .065); }

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

    .button { min-height: 44px; padding: 0 18px; border-radius: 999px; font-size: 14px; font-weight: 680; transition: transform .18s var(--ease), opacity .18s ease; }
    .button:hover { transform: translateY(-1px); }
    .button.primary { color: white; background: var(--text); }
    .button.secondary { color: var(--text); background: rgba(0, 0, 0, .055); }
    .button.danger { color: var(--danger); background: rgba(217, 45, 32, .1); }
    .button:disabled { cursor: not-allowed; opacity: .45; transform: none; }

    .data-actions { display: flex; flex-wrap: wrap; gap: 10px; }
    .transfer-note { color: var(--secondary); font-size: 12px; line-height: 1.7; }
    .transfer-actions { margin-top: 16px; }

    .gift-card-view { border: 1px solid rgba(0, 0, 0, .08); border-radius: 24px; background: radial-gradient(circle at 86% 12%, rgba(255, 255, 255, .18), transparent 24%), linear-gradient(145deg, #1d1d1f, #35353a); color: white; padding: 34px; box-shadow: 0 24px 70px rgba(0, 0, 0, .2); }
    .gift-card-view .muted { color: rgba(255, 255, 255, .62); }
    .gift-title { margin-top: 44px; font-size: 46px; line-height: .95; letter-spacing: -.055em; font-weight: 700; }
    .gift-name { margin-top: 28px; font-size: 23px; font-weight: 650; }
    .gift-description { margin-top: 12px; max-width: 430px; color: rgba(255, 255, 255, .7); line-height: 1.55; font-size: 14px; }
    .gift-code { margin-top: 48px; padding-top: 18px; display: flex; justify-content: space-between; gap: 18px; border-top: 1px solid rgba(255, 255, 255, .14); font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; font-size: 13px; }
    .gift-list { display: grid; gap: 12px; margin-top: 18px; }
    .gift-record { padding: 16px; border-radius: 16px; background: #f7f7f9; border: 1px solid var(--line); }
    .gift-record-top { display: flex; justify-content: space-between; gap: 16px; align-items: center; }
    .gift-record strong { font-size: 14px; }
    .gift-record span { display: block; margin-top: 5px; color: var(--secondary); font-size: 12px; }
    .status-tag { min-height: 26px; padding: 0 10px; display: inline-flex; align-items: center; border-radius: 999px; color: var(--secondary); background: rgba(0, 0, 0, .055); font-size: 11px; white-space: nowrap; }
    .status-tag.claimed { color: var(--green); background: rgba(36, 161, 72, .1); }

    .celebration-card { padding: 40px 34px 32px; }
    .celebration-kicker { color: var(--secondary); font-size: 12px; font-weight: 760; letter-spacing: .15em; text-transform: uppercase; }
    .celebration-number { margin-top: 48px; font-size: clamp(54px, 10vw, 82px); line-height: .94; letter-spacing: -.065em; font-weight: 720; }
    .celebration-copy { margin-top: 22px; color: var(--secondary); font-size: 16px; line-height: 1.65; }
    .celebration-actions { margin-top: 28px; display: flex; flex-wrap: wrap; gap: 10px; }

    .toast {
      position: fixed;
      left: 50%;
      bottom: calc(28px + env(safe-area-inset-bottom));
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
      white-space: nowrap;
      max-width: calc(100% - 28px);
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .toast.show { opacity: 1; transform: translate(-50%, 0); }

    .sr-only { position: absolute !important; width: 1px !important; height: 1px !important; padding: 0 !important; margin: -1px !important; overflow: hidden !important; clip: rect(0, 0, 0, 0) !important; white-space: nowrap !important; border: 0 !important; }

    @media (max-width: 940px) {
      .cards-grid { grid-template-columns: 1fr; }
      .habit-card { min-height: 330px; }
      .reward-summary { grid-template-columns: 1fr; }
      .reward-metrics { justify-content: flex-start; }
      .reward-metric { text-align: left; }
    }

    @media (max-width: 720px) {
      .container { width: min(calc(100% - 24px), var(--page)); }
      main { padding: 38px 0 56px; }
      .topbar-inner { height: 60px; }
      .milestone-pill span { display: none; }
      .hero { margin-bottom: 22px; }
      .hero-meta-line { margin-bottom: 13px; }
      h1 { font-size: clamp(46px, 15vw, 64px); }
      .hero-copy { margin-top: 16px; font-size: 15px; line-height: 1.6; }
      .reward-summary { padding: 20px; border-radius: 24px; }
      .reward-metrics { flex-wrap: wrap; gap: 16px; }

      .cards-grid { gap: 14px; margin-bottom: 44px; }
      .habit-card { width: 100%; min-height: 0; padding: 21px; border-radius: 27px; }
      .habit-card:hover { transform: none; }
      .habit-card::after { right: -54px; bottom: -70px; width: 155px; height: 155px; opacity: .065; }
      .card-arrow { width: 36px; height: 36px; }
      .primary-value { margin-top: 23px; font-size: clamp(48px, 14vw, 62px); line-height: .96; }
      .primary-value small { margin-top: 10px; font-size: 13px; }
      .secondary-data { margin-top: 20px; padding-top: 16px; }
      .secondary-row { font-size: 14px; }
      .progress-area { margin-top: 0; padding-top: 22px; }
      .progress-track { height: 7px; }
      .mini-chart { height: 40px; }

      .section { margin-bottom: 44px; }
      .section-head { align-items: start; flex-direction: column; gap: 10px; margin-bottom: 16px; }
      .section-head h2 { font-size: 34px; }
      .section-head p { text-align: left; font-size: 13px; }
      .overview-panel { padding: 20px; border-radius: 25px; }
      .ledger-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); }
      .ledger-stat { padding: 4px 12px 20px; }
      .ledger-stat:first-child { padding-left: 0; }
      .ledger-stat:last-child { padding-right: 0; }
      .ledger-stat + .ledger-stat { border-top: 0; border-left: 1px solid var(--line); }
      .ledger-stat strong { font-size: 29px; }
      .recent-item { grid-template-columns: 38px minmax(0, 1fr); gap: 11px; }
      .recent-date { grid-column: 2; text-align: left; }

      .modal-layer { align-items: flex-end; padding: 0; }
      .modal { width: 100%; max-height: 92dvh; border-radius: 28px 28px 0 0; animation-name: mobileSheetIn; padding-bottom: env(safe-area-inset-bottom); }
      .modal.wide { width: 100%; }
      .modal-head { min-height: 64px; }
      .modal-body { padding: 20px; }
      .modal-head, .modal-actions { padding-left: 20px; padding-right: 20px; }
      .modal-actions { justify-content: stretch; }
      .modal-actions .button { flex: 1; }
      .form-grid { grid-template-columns: 1fr; }
      .field.full { grid-column: auto; }
      .summary-box { grid-template-columns: repeat(3, minmax(0, 1fr)); padding: 16px; gap: 8px; }
      .summary-box strong { font-size: 18px; }
      .history-row { grid-template-columns: 92px 1fr; gap: 12px; }
      .history-row .value { grid-column: 2; text-align: left; }
      .data-actions .button { width: 100%; }
      .footer-inner { flex-direction: column; }
      .gift-title { font-size: 40px; }
      .gift-code { flex-direction: column; }
    }

    @media (max-width: 380px) {
      .container { width: calc(100% - 18px); }
      .habit-card { padding: 18px; }
      .reward-summary { padding: 18px; }
      .ledger-stat { padding-left: 8px; padding-right: 8px; }
      .ledger-stat strong { font-size: 25px; }
      .summary-box { grid-template-columns: 1fr; }
      .summary-box strong { font-size: 21px; }
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
          <button class="milestone-pill" id="openLedger" type="button">
            <span>里程积分</span>
            <strong id="topBalance">0</strong>
          </button>

          <button class="icon-button" id="openSettings" type="button" aria-label="设置" title="设置">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="M12 15.5a3.5 3.5 0 1 0 0-7 3.5 3.5 0 0 0 0 7Z"/>
              <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06A1.65 1.65 0 0 0 15 19.4a1.65 1.65 0 0 0-1 .6 1.65 1.65 0 0 0-.4 1.08V21a2 2 0 1 1-4 0v-.09A1.65 1.65 0 0 0 8.6 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-.6-1 1.65 1.65 0 0 0-1.08-.4H3a2 2 0 1 1 0-4h.09A1.65 1.65 0 0 0 4.6 8.6a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-.6 1.65 1.65 0 0 0 .4-1.08V3a2 2 0 1 1 4 0v.09A1.65 1.65 0 0 0 15.4 4.6a1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9c.14.37.36.7.66.97.3.26.67.41 1.07.43H21a2 2 0 1 1 0 4h-.09a1.65 1.65 0 0 0-1.51.6Z"/>
            </svg>
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

        <section class="reward-summary" id="rewardSummary" aria-label="Milestone overview">
          <div>
            <div class="reward-title">Milestones</div>
            <div class="reward-note" id="rewardSummaryText">每两个里程积分，可以向董纪君兑换一份神秘礼品。</div>
          </div>
          <div class="reward-metrics">
            <div class="reward-metric">
              <strong id="rewardEarned">0</strong>
              <span>累计获得</span>
            </div>
            <div class="reward-metric">
              <strong id="rewardBalance">0</strong>
              <span>当前可用</span>
            </div>
            <button class="reward-button" id="redeemHome" type="button">兑换礼品</button>
          </div>
        </section>

        <section class="cards-grid" id="cardsGrid">
          <article class="habit-card ielts" data-card-type="ielts" data-open="ieltsModal" tabindex="0" role="button" aria-label="打开IELTS记录">
            <div class="card-head">
              <div class="card-label">01 · IELTS</div>
              <div class="card-arrow">→</div>
            </div>
            <div class="primary-value" id="ieltsPrimary">0
              <small id="ieltsPrimaryLabel">days to IELTS</small>
            </div>
            <div class="secondary-data">
              <div class="secondary-row"><span>Words remembered</span><strong id="totalWordsCard">0</strong></div>
              <div class="secondary-row"><span>Today</span><strong id="todayWordsCard">0 new</strong></div>
            </div>
            <div class="progress-area">
              <div class="progress-top"><span>Next 500 words</span><strong id="wordProgressText">0 / 500</strong></div>
              <div class="progress-track"><div class="progress-fill" id="wordProgressFill"></div></div>
              <div class="progress-note" id="wordProgressNote">500 words to the next milestone</div>
            </div>
          </article>

          <article class="habit-card weight" data-card-type="weight" data-open="weightModal" tabindex="0" role="button" aria-label="打开体重记录">
            <div class="card-head">
              <div class="card-label">02 · WEIGHT</div>
              <div class="card-arrow">→</div>
            </div>
            <div class="primary-value" id="weightPrimary">—
              <small id="weightPrimaryLabel">record today’s weight</small>
            </div>
            <div class="secondary-data">
              <div class="secondary-row"><span>Target</span><strong id="targetWeightCard">55.0 kg</strong></div>
              <div class="secondary-row"><span>Total change</span><strong id="weightChangeCard">—</strong></div>
              <canvas class="mini-chart" id="weightSparkline" aria-label="体重趋势"></canvas>
            </div>
            <div class="progress-area">
              <div class="progress-top"><span>Healthy choices</span><strong id="healthProgressText">0 / 50</strong></div>
              <div class="progress-track"><div class="progress-fill" id="healthProgressFill"></div></div>
              <div class="progress-note" id="healthProgressNote">50 choices to the next milestone</div>
            </div>
          </article>

          <article class="habit-card sleep" data-card-type="sleep" data-open="sleepModal" tabindex="0" role="button" aria-label="打开早睡打卡">
            <div class="card-head">
              <div class="card-label">03 · SLEEP</div>
              <div class="card-arrow">→</div>
            </div>
            <div class="primary-value" id="sleepPrimary">0 days
              <small id="sleepPrimaryLabel">current streak</small>
            </div>
            <div class="secondary-data">
              <div class="secondary-row"><span>Tonight</span><strong id="tonightStatus">Waiting</strong></div>
              <div class="secondary-row"><span>Best streak</span><strong id="bestSleepStreak">0 days</strong></div>
            </div>
            <div class="progress-area">
              <div class="progress-top"><span>Two quiet weeks</span><strong id="sleepProgressText">0 / 14</strong></div>
              <div class="progress-track"><div class="progress-fill" id="sleepProgressFill"></div></div>
              <div class="progress-note" id="sleepProgressNote">14 nights to the next milestone</div>
            </div>
          </article>
        </section>

        <section class="section" id="ledgerSection">
          <div class="section-head">
            <h2>Milestones</h2>
            <p>完成500个新单词、50次健康选择或连续早睡14晚，即可获得1个里程积分。</p>
          </div>
          <div class="overview-panel">
            <div class="ledger-grid">
              <div class="ledger-stat"><span>Earned</span><strong id="homeEarned">0</strong></div>
              <div class="ledger-stat"><span>Redeemed</span><strong id="homeRedeemed">0</strong></div>
              <div class="ledger-stat"><span>Available</span><strong id="homeAvailable">0</strong></div>
            </div>
            <div class="recent-list" id="recentList"></div>
          </div>
        </section>
      </div>
    </main>

    <footer>
      <div class="container footer-inner">
        <span>Three · made for 罗思维</span>
        <span>Data stays in this browser unless exported.</span>
      </div>
    </footer>
  </div>

  <!-- IELTS -->
  <div class="modal-layer" id="ieltsModal">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="ieltsModalTitle">
      <div class="modal-head">
        <div class="modal-title" id="ieltsModalTitle">IELTS · Words</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>
      <form id="ieltsForm">
        <div class="modal-body">
          <div class="modal-section">
            <div class="form-grid">
              <div class="field"><label for="examDateInput">IELTS考试日期</label><input id="examDateInput" type="date" /></div>
              <div class="field"><label for="todayNewWords">今日新记单词</label><input id="todayNewWords" type="number" min="0" max="10000" step="1" inputmode="numeric" /></div>
              <div class="field"><label for="todayReviewWords">今日复习单词</label><input id="todayReviewWords" type="number" min="0" max="10000" step="1" inputmode="numeric" /></div>
              <div class="field full"><label for="wordNote">今日备注</label><textarea id="wordNote" placeholder="写下一点今天的学习状态"></textarea></div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">当前进度</div>
            <div class="summary-box">
              <div><span>累计新词</span><strong id="ieltsTotalDetail">0词</strong></div>
              <div><span>已获里程</span><strong id="ieltsMilestonesDetail">0次</strong></div>
              <div><span>距离下一次</span><strong id="ieltsRemainingDetail">500词</strong></div>
            </div>
            <div class="detail-progress">
              <div class="progress-top"><span>下一组500词</span><strong id="ieltsDetailProgressText">0 / 500</strong></div>
              <div class="progress-track"><div class="progress-fill" id="ieltsDetailProgressFill" style="background:var(--blue)"></div></div>
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
        <div class="modal-title" id="weightModalTitle">Weight · Health</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>
      <form id="weightForm">
        <div class="modal-body">
          <div class="modal-section">
            <div class="form-grid">
              <div class="field"><label for="todayWeightInput">今日体重（kg）</label><input id="todayWeightInput" type="number" min="20" max="300" step="0.1" inputmode="decimal" /></div>
              <div class="field"><label for="targetWeightInput">目标体重（kg）</label><input id="targetWeightInput" type="number" min="20" max="300" step="0.1" inputmode="decimal" /></div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">今日健康选择</div>
            <div class="choice-list">
              <div class="choice"><input id="noLateSnack" type="checkbox" /><label for="noLateSnack"><span class="choice-mark">✓</span><span>今晚不吃夜宵</span></label></div>
              <div class="choice"><input id="noMilkTea" type="checkbox" /><label for="noMilkTea"><span class="choice-mark">✓</span><span>今天不喝奶茶或含糖饮料</span></label></div>
              <div class="choice"><input id="eightyFull" type="checkbox" /><label for="eightyFull"><span class="choice-mark">✓</span><span>正餐保持八分饱</span></label></div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">当前进度</div>
            <div class="summary-box">
              <div><span>初始体重</span><strong id="startWeightDetail">—</strong></div>
              <div><span>当前体重</span><strong id="currentWeightDetail">—</strong></div>
              <div><span>距离目标</span><strong id="weightRemainingDetail">—</strong></div>
            </div>
            <div class="detail-progress">
              <div class="progress-top"><span>体重目标</span><strong id="weightGoalText">尚无足够数据</strong></div>
              <div class="progress-track"><div class="progress-fill" id="weightGoalFill" style="background:var(--green)"></div></div>
            </div>
            <div class="detail-progress">
              <div class="progress-top"><span>下一组50次健康选择</span><strong id="healthDetailProgressText">0 / 50</strong></div>
              <div class="progress-track"><div class="progress-fill" id="healthDetailProgressFill" style="background:var(--green)"></div></div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">历史体重变化</div>
            <div class="chart-wrap">
              <canvas id="weightChart" aria-label="历史体重变化趋势"></canvas>
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
        <div class="modal-title" id="sleepModalTitle">Sleep · Check-in</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>
      <div class="modal-body">
        <div class="modal-section">
          <div class="sleep-status">
            <strong id="sleepModalStreak">0 days</strong>
            <span id="sleepWindowText">打卡开放时间为每天20:00至次日00:59。</span>
            <button class="checkin-button" id="sleepCheckinButton" type="button">完成今晚打卡</button>
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">当前进度</div>
          <div class="summary-box">
            <div><span>当前连续</span><strong id="sleepCurrentDetail">0天</strong></div>
            <div><span>历史最佳</span><strong id="sleepBestDetail">0天</strong></div>
            <div><span>已获里程</span><strong id="sleepMilestonesDetail">0次</strong></div>
          </div>
          <div class="detail-progress">
            <div class="progress-top"><span>下一次连续两周</span><strong id="sleepDetailProgressText">0 / 14</strong></div>
            <div class="progress-track"><div class="progress-fill" id="sleepDetailProgressFill" style="background:var(--purple)"></div></div>
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">规则</div>
          <div class="empty-state" style="padding:0">
            每天20:00至次日00:59可以打卡。00:00至00:59的打卡仍计入前一晚。凌晨01:00后未完成会中断当前连续天数，且不支持补签；已经获得的里程积分不会被取消。
          </div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">最近14晚</div>
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
        <div class="modal-title" id="ledgerModalTitle">Milestones & Gifts</div>
        <button class="close-button" type="button" data-close aria-label="关闭">×</button>
      </div>
      <div class="modal-body">
        <div class="modal-section">
          <div class="summary-box">
            <div><span>累计获得</span><strong id="ledgerEarned">0</strong></div>
            <div><span>已使用</span><strong id="ledgerSpent">0</strong></div>
            <div><span>当前可用</span><strong id="ledgerBalance">0</strong></div>
          </div>
          <div class="data-actions" style="margin-top:16px">
            <button class="button primary" id="redeemLedger" type="button">使用2个积分兑换</button>
          </div>
        </div>

        <div class="modal-section" id="latestGiftSection" hidden>
          <div class="modal-section-title">最新礼品卡</div>
          <div id="latestGiftCard"></div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">礼品兑换记录</div>
          <div class="gift-list" id="giftRecords"></div>
        </div>

        <div class="modal-section">
          <div class="modal-section-title">全部里程记录</div>
          <div class="recent-list" id="ledgerRecentList"></div>
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
            <div class="modal-section-title">固定信息</div>
            <div class="form-grid">
              <div class="field">
                <label>收礼人</label>
                <div class="fixed-setting"><strong>罗思维</strong><span>不可修改</span></div>
              </div>
              <div class="field">
                <label>赠送人</label>
                <div class="fixed-setting"><strong>董纪君</strong><span>不可修改</span></div>
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">目标设置</div>
            <div class="form-grid">
              <div class="field"><label for="settingsExamDate">IELTS考试日期</label><input id="settingsExamDate" type="date" /></div>
              <div class="field"><label for="settingsTargetWeight">目标体重（kg）</label><input id="settingsTargetWeight" type="number" min="20" max="300" step="0.1" inputmode="decimal" /></div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">首页显示</div>
            <div class="form-grid">
              <div class="field">
                <label for="cardOrderInput">卡片顺序</label>
                <select id="cardOrderInput">
                  <option value="ielts,weight,sleep">IELTS · 体重 · 早睡</option>
                  <option value="sleep,ielts,weight">早睡 · IELTS · 体重</option>
                  <option value="weight,sleep,ielts">体重 · 早睡 · IELTS</option>
                </select>
              </div>
              <div class="field">
                <label for="colorStyleInput">配色风格</label>
                <select id="colorStyleInput"><option value="color">彩色</option><option value="mono">黑白</option></select>
              </div>
              <div class="field">
                <label for="showRewardInput">里程积分概览</label>
                <select id="showRewardInput"><option value="show">在首页显示</option><option value="hide">在首页隐藏</option></select>
              </div>
              <div class="field">
                <label for="showHistoryInput">里程记录</label>
                <select id="showHistoryInput"><option value="show">显示在卡片下方</option><option value="hide">在首页隐藏</option></select>
              </div>
            </div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">早睡打卡时间</div>
            <div class="form-grid">
              <div class="field"><label>开始时间</label><div class="fixed-setting"><strong>20:00</strong><span>不可修改</span></div></div>
              <div class="field"><label>截止时间</label><div class="fixed-setting"><strong>次日 01:00</strong><span>不可修改</span></div></div>
            </div>
            <div class="transfer-note" style="margin-top:12px">每晚20:00开放打卡，次日凌晨01:00截止。凌晨00:00至00:59完成的打卡仍计入前一晚。</div>
          </div>

          <div class="modal-section">
            <div class="modal-section-title">数据管理</div>
            <div class="transfer-note">在旧设备导出备份文件并通过微信、文件或网盘发送；在新设备打开本网站后导入，即可恢复全部记录、积分和兑换卡。</div>
            <div class="data-actions transfer-actions">
              <button class="button primary" id="exportData" type="button">导出或分享数据</button>
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
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="celebrationTitle">
      <div id="celebrationContent"></div>
    </div>
  </div>

  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    (() => {
      "use strict";

      const STORAGE_KEY = "three_luosiwei_final_v1";
      const APP_VERSION = 2;
      const FIXED_USER = "罗思维";
      const FIXED_AUTHOR = "董纪君";
      const FIXED_SLEEP_START = "20:00";
      const FIXED_SLEEP_DEADLINE = "01:00";

      const defaultState = {
        version: APP_VERSION,
        settings: {
          userName: FIXED_USER,
          authorName: FIXED_AUTHOR,
          examDate: "",
          targetWeight: 55,
          sleepStart: FIXED_SLEEP_START,
          sleepDeadline: FIXED_SLEEP_DEADLINE,
          cardOrder: "ielts,weight,sleep",
          colorStyle: "color",
          showRewardSummary: true,
          showHistory: true
        },
        daily: {},
        sleepCheckins: {},
        milestones: [],
        redemptions: []
      };

      let state = loadState();
      let toastTimer = null;
      let popupQueue = [];
      let popupOpen = false;

      const $ = id => document.getElementById(id);
      const todayKey = () => dateKey(new Date());

      function cloneDefault() {
        return JSON.parse(JSON.stringify(defaultState));
      }

      function fixedSettings(settings = {}) {
        return {
          ...cloneDefault().settings,
          ...settings,
          userName: FIXED_USER,
          authorName: FIXED_AUTHOR,
          sleepStart: FIXED_SLEEP_START,
          sleepDeadline: FIXED_SLEEP_DEADLINE
        };
      }

      function loadState() {
        try {
          const raw = localStorage.getItem(STORAGE_KEY);
          if (!raw) return cloneDefault();
          const parsed = JSON.parse(raw);
          return {
            version: APP_VERSION,
            settings: fixedSettings(parsed.settings || {}),
            daily: parsed.daily && typeof parsed.daily === "object" ? parsed.daily : {},
            sleepCheckins: parsed.sleepCheckins && typeof parsed.sleepCheckins === "object" ? parsed.sleepCheckins : {},
            milestones: Array.isArray(parsed.milestones) ? parsed.milestones : [],
            redemptions: Array.isArray(parsed.redemptions) ? parsed.redemptions : []
          };
        } catch (error) {
          console.warn("Unable to load saved data:", error);
          return cloneDefault();
        }
      }

      function saveState() {
        state.settings = fixedSettings(state.settings);
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

      function safeNumber(value, fallback = 0) {
        const n = Number(value);
        return Number.isFinite(n) ? n : fallback;
      }

      function clamp(value, min, max) {
        return Math.min(max, Math.max(min, value));
      }

      function escapeHtml(value) {
        return String(value ?? "").replace(/[&<>"']/g, char => ({
          "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#039;"
        })[char]);
      }

      function formatShortDate(date) {
        return new Intl.DateTimeFormat("zh-CN", { month: "short", day: "numeric" }).format(date);
      }

      function formatDateTime(iso) {
        return new Intl.DateTimeFormat("zh-CN", {
          year: "numeric", month: "short", day: "numeric", hour: "2-digit", minute: "2-digit"
        }).format(new Date(iso));
      }

      function ensureDaily(key = todayKey()) {
        if (!state.daily[key]) {
          state.daily[key] = {
            words: { newWords: 0, reviewWords: 0, note: "" },
            weight: { value: "", noLateSnack: false, noMilkTea: false, eightyFull: false }
          };
        }
        state.daily[key].words = {
          newWords: 0, reviewWords: 0, note: "", ...(state.daily[key].words || {})
        };
        state.daily[key].weight = {
          value: "", noLateSnack: false, noMilkTea: false, eightyFull: false,
          ...(state.daily[key].weight || {})
        };
        return state.daily[key];
      }

      function totalWords() {
        return Object.values(state.daily).reduce((sum, entry) =>
          sum + Math.max(0, safeNumber(entry?.words?.newWords)), 0);
      }

      function healthPointsForEntry(entry) {
        const weight = entry?.weight || {};
        return [weight.noLateSnack, weight.noMilkTea, weight.eightyFull].filter(Boolean).length;
      }

      function totalHealthPoints() {
        return Object.values(state.daily).reduce((sum, entry) => sum + healthPointsForEntry(entry), 0);
      }

      function weightEntries() {
        return Object.keys(state.daily)
          .sort()
          .map(key => ({ date: dateFromKey(key), key, value: safeNumber(state.daily[key]?.weight?.value, NaN) }))
          .filter(item => Number.isFinite(item.value) && item.value > 0);
      }

      function firstWeight() { return weightEntries()[0] || null; }
      function latestWeight() { const list = weightEntries(); return list[list.length - 1] || null; }
      function currentRound(total, size) { return total % size; }
      function remainingToNext(total, size) { const r = total % size; return r === 0 && total > 0 ? size : size - r; }
      function milestonesFor(source) { return state.milestones.filter(item => item.source === source); }
      function spentPoints() { return state.redemptions.length * 2; }
      function milestoneBalance() { return Math.max(0, state.milestones.length - spentPoints()); }

      function getNightWindow(now = new Date()) {
        const hour = now.getHours();
        if (hour >= 20) {
          return { canCheckIn: true, nightKey: dateKey(now), message: "今晚打卡开放中，截止到次日凌晨01:00。" };
        }
        if (hour < 1) {
          const previous = new Date(now);
          previous.setDate(previous.getDate() - 1);
          return { canCheckIn: true, nightKey: dateKey(previous), message: "仍在昨晚的有效打卡时间内，凌晨01:00截止。" };
        }
        return { canCheckIn: false, nightKey: null, message: "打卡将在今晚20:00开放，次日凌晨01:00截止。" };
      }

      function sleepReferenceKey(now = new Date()) {
        const hour = now.getHours();
        const windowInfo = getNightWindow(now);
        if (windowInfo.canCheckIn && state.sleepCheckins[windowInfo.nightKey]) return windowInfo.nightKey;
        if (hour >= 20) return addDaysToKey(dateKey(now), -1);
        if (hour < 1) return addDaysToKey(windowInfo.nightKey, -1);
        return addDaysToKey(dateKey(now), -1);
      }

      function currentSleepRun(now = new Date()) {
        const reference = sleepReferenceKey(now);
        if (!reference || !state.sleepCheckins[reference]) return { streak: 0, startKey: null, endKey: reference };
        let streak = 0;
        let cursor = reference;
        while (state.sleepCheckins[cursor]) {
          streak += 1;
          cursor = addDaysToKey(cursor, -1);
        }
        return { streak, startKey: addDaysToKey(reference, -(streak - 1)), endKey: reference };
      }

      function bestSleepStreak() {
        const keys = Object.keys(state.sleepCheckins).sort();
        if (!keys.length) return 0;
        let best = 1;
        let current = 1;
        for (let i = 1; i < keys.length; i += 1) {
          if (dayDifference(keys[i - 1], keys[i]) === 1) {
            current += 1;
            best = Math.max(best, current);
          } else current = 1;
        }
        return best;
      }

      function createMilestone(source, level, title, detail, id, createdAt = new Date().toISOString()) {
        if (state.milestones.some(item => item.id === id)) return null;
        const milestone = { id, source, level, title, detail, createdAt };
        state.milestones.push(milestone);
        return milestone;
      }

      function syncWordMilestones() {
        const expected = Math.floor(totalWords() / 500);
        const added = [];
        for (let level = 1; level <= expected; level += 1) {
          const item = createMilestone("ielts", level, `${level * 500} Words`, `累计记住了${level * 500}个新单词。`, `ielts-${level}`);
          if (item) added.push(item);
        }
        return added;
      }

      function syncHealthMilestones() {
        const expected = Math.floor(totalHealthPoints() / 50);
        const added = [];
        for (let level = 1; level <= expected; level += 1) {
          const item = createMilestone("health", level, `${level * 50} Healthy Choices`, `累计完成了${level * 50}次健康选择。`, `health-${level}`);
          if (item) added.push(item);
        }
        return added;
      }

      function syncSleepMilestones() {
        const keys = Object.keys(state.sleepCheckins).sort();
        if (!keys.length) return [];
        const added = [];
        let currentStart = keys[0];
        let currentLength = 1;

        function commitRun(startKey, length) {
          const count = Math.floor(length / 14);
          for (let level = 1; level <= count; level += 1) {
            const endKey = addDaysToKey(startKey, level * 14 - 1);
            const id = `sleep-${startKey}-${level}`;
            const item = createMilestone("sleep", level, `${level * 14} Quiet Nights`, `从${formatShortDate(dateFromKey(startKey))}开始连续早睡${level * 14}晚。`, id, state.sleepCheckins[endKey] || new Date().toISOString());
            if (item) added.push(item);
          }
        }

        for (let i = 1; i < keys.length; i += 1) {
          if (dayDifference(keys[i - 1], keys[i]) === 1) currentLength += 1;
          else {
            commitRun(currentStart, currentLength);
            currentStart = keys[i];
            currentLength = 1;
          }
        }
        commitRun(currentStart, currentLength);
        return added;
      }

      function syncAllMilestones(showPopups = false) {
        const added = [...syncWordMilestones(), ...syncHealthMilestones(), ...syncSleepMilestones()];
        if (showPopups) added.forEach(item => enqueuePopup({ type: "milestone", item }));
        return added;
      }

      function showToast(message) {
        const toast = $("toast");
        toast.textContent = message;
        toast.classList.add("show");
        window.clearTimeout(toastTimer);
        toastTimer = window.setTimeout(() => toast.classList.remove("show"), 2300);
      }

      function openModal(id) {
        const layer = typeof id === "string" ? $(id) : id;
        if (!layer) return;
        if (layer.id === "ieltsModal") renderIeltsDetail();
        if (layer.id === "weightModal") renderWeightDetail();
        if (layer.id === "sleepModal") renderSleepDetail();
        if (layer.id === "ledgerModal") renderLedger();
        if (layer.id === "settingsModal") prefillSettings();
        layer.classList.add("open");
        document.body.classList.add("modal-open");
      }

      function closeModal(layer) {
        if (!layer) return;
        layer.classList.remove("open");
        if (!document.querySelector(".modal-layer.open")) document.body.classList.remove("modal-open");
      }

      function enqueuePopup(payload) {
        popupQueue.push(payload);
        showNextPopup();
      }

      function showNextPopup() {
        if (popupOpen || !popupQueue.length) return;
        popupOpen = true;
        const payload = popupQueue.shift();
        const content = $("celebrationContent");

        if (payload.type === "milestone") {
          const item = payload.item;
          content.innerHTML = `
            <div class="celebration-card">
              <div class="celebration-kicker">Milestone unlocked</div>
              <div class="celebration-number" id="celebrationTitle">+1</div>
              <div class="celebration-copy"><strong>${escapeHtml(item.title)}</strong><br>${escapeHtml(item.detail)}</div>
              <div class="celebration-actions"><button class="button primary" id="celebrationDone" type="button">继续</button></div>
            </div>`;
        } else {
          content.innerHTML = `
            <div class="celebration-card">
              <div class="celebration-kicker">Gift unlocked</div>
              <div class="celebration-number" id="celebrationTitle">Ready.</div>
              <div class="celebration-copy">现在已经有足够的里程积分，可以兑换一份由${FIXED_AUTHOR}准备的神秘礼品。</div>
              <div class="celebration-actions"><button class="button primary" id="celebrationRedeem" type="button">立即兑换</button><button class="button secondary" id="celebrationDone" type="button">稍后</button></div>
            </div>`;
        }

        openModal("celebrationLayer");
        $("celebrationDone")?.addEventListener("click", finishPopup);
        $("celebrationRedeem")?.addEventListener("click", () => {
          finishPopup();
          redeemGift(true);
        });
      }

      function finishPopup() {
        closeModal($("celebrationLayer"));
        popupOpen = false;
        window.setTimeout(showNextPopup, 150);
      }

      function examCountdown() {
        if (!state.settings.examDate) return { primary: "—", label: "set an IELTS date" };
        const exam = dateFromKey(state.settings.examDate);
        exam.setHours(23, 59, 59, 999);
        const diff = Math.ceil((exam - new Date()) / 86400000);
        if (diff > 0) return { primary: String(diff), label: "days to IELTS" };
        if (diff === 0) return { primary: "Today", label: "IELTS exam day" };
        return { primary: "Done", label: `exam date ${formatShortDate(exam)}` };
      }

      function tonightDisplayStatus() {
        const windowInfo = getNightWindow();
        if (windowInfo.canCheckIn) return state.sleepCheckins[windowInfo.nightKey] ? "Completed" : "Waiting";
        const lastNight = addDaysToKey(todayKey(), -1);
        return state.sleepCheckins[lastNight] ? "Completed" : "Paused";
      }

      function renderHeaderAndRewards() {
        $("greeting").textContent = `For ${FIXED_USER}`;
        $("heroDate").textContent = new Intl.DateTimeFormat("zh-CN", { year: "numeric", month: "long", day: "numeric", weekday: "short" }).format(new Date());
        const earned = state.milestones.length;
        const spent = spentPoints();
        const balance = milestoneBalance();
        $("topBalance").textContent = balance;
        $("rewardEarned").textContent = earned;
        $("rewardBalance").textContent = balance;
        $("homeEarned").textContent = earned;
        $("homeRedeemed").textContent = spent;
        $("homeAvailable").textContent = balance;
        $("rewardSummaryText").textContent = `每两个里程积分，可以向${FIXED_AUTHOR}兑换一份神秘礼品。`;
        $("redeemHome").disabled = balance < 2;
        $("redeemHome").textContent = balance >= 2 ? "兑换礼品" : "还需2个积分";
      }

      function renderIeltsCard() {
        const countdown = examCountdown();
        const total = totalWords();
        const today = ensureDaily();
        const progress = currentRound(total, 500);
        const remaining = remainingToNext(total, 500);
        $("ieltsPrimary").firstChild.nodeValue = countdown.primary;
        $("ieltsPrimaryLabel").textContent = countdown.label;
        $("totalWordsCard").textContent = total.toLocaleString();
        $("todayWordsCard").textContent = `${safeNumber(today.words.newWords)} new`;
        $("wordProgressText").textContent = `${progress} / 500`;
        $("wordProgressFill").style.width = `${progress / 500 * 100}%`;
        $("wordProgressNote").textContent = `${remaining} words to the next milestone`;
      }

      function renderWeightCard() {
        const first = firstWeight();
        const latest = latestWeight();
        const target = safeNumber(state.settings.targetWeight, 55);
        const points = totalHealthPoints();
        const progress = currentRound(points, 50);
        const remaining = remainingToNext(points, 50);

        if (latest) {
          $("weightPrimary").firstChild.nodeValue = latest.value.toFixed(1);
          $("weightPrimaryLabel").textContent = `kg · ${formatShortDate(latest.date)}`;
        } else {
          $("weightPrimary").firstChild.nodeValue = "—";
          $("weightPrimaryLabel").textContent = "record today’s weight";
        }
        $("targetWeightCard").textContent = `${target.toFixed(1)} kg`;
        if (first && latest) {
          const diff = latest.value - first.value;
          const sign = diff > 0 ? "+" : diff < 0 ? "−" : "";
          $("weightChangeCard").textContent = `${sign}${Math.abs(diff).toFixed(1)} kg`;
        } else $("weightChangeCard").textContent = "—";
        $("healthProgressText").textContent = `${progress} / 50`;
        $("healthProgressFill").style.width = `${progress / 50 * 100}%`;
        $("healthProgressNote").textContent = `${remaining} choices to the next milestone`;
        drawWeightSparkline();
      }

      function renderSleepCard() {
        const run = currentSleepRun();
        const best = bestSleepStreak();
        const progress = run.streak % 14;
        const remaining = progress === 0 && run.streak > 0 ? 14 : 14 - progress;
        $("sleepPrimary").firstChild.nodeValue = `${run.streak} ${run.streak === 1 ? "day" : "days"}`;
        $("sleepPrimaryLabel").textContent = "current streak";
        $("tonightStatus").textContent = tonightDisplayStatus();
        $("bestSleepStreak").textContent = `${best} ${best === 1 ? "day" : "days"}`;
        $("sleepProgressText").textContent = `${progress} / 14`;
        $("sleepProgressFill").style.width = `${progress / 14 * 100}%`;
        $("sleepProgressNote").textContent = `${remaining} nights to the next milestone`;
      }

      function sourceLabel(source) {
        return source === "ielts" ? "IELTS" : source === "health" ? "HEALTH" : "SLEEP";
      }

      function renderRecentItem(entry) {
        if (entry.type === "gift") {
          return `<div class="recent-item"><div class="source-dot gift">G</div><div class="recent-main"><strong>神秘礼品兑换卡</strong><span>${escapeHtml(entry.item.code)} · 使用2个积分</span></div><div class="recent-date">${escapeHtml(formatShortDate(new Date(entry.createdAt)))}</div></div>`;
        }
        const initial = sourceLabel(entry.item.source).charAt(0);
        return `<div class="recent-item"><div class="source-dot ${escapeHtml(entry.item.source)}">${initial}</div><div class="recent-main"><strong>${escapeHtml(entry.item.title)}</strong><span>${escapeHtml(entry.item.detail)} · +1积分</span></div><div class="recent-date">${escapeHtml(formatShortDate(new Date(entry.createdAt)))}</div></div>`;
      }

      function combinedLedger() {
        return [
          ...state.milestones.map(item => ({ type: "milestone", createdAt: item.createdAt, item })),
          ...state.redemptions.map(item => ({ type: "gift", createdAt: item.createdAt, item }))
        ].sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
      }

      function renderHomeLedger() {
        const combined = combinedLedger();
        $("recentList").innerHTML = combined.length
          ? combined.slice(0, 8).map(renderRecentItem).join("")
          : `<div class="empty-state">完成第一个500词、50次健康选择或连续14晚后，里程记录会出现在这里。</div>`;
      }

      function applyHomeSettings() {
        const order = String(state.settings.cardOrder || "ielts,weight,sleep").split(",");
        const grid = $("cardsGrid");
        order.forEach(type => {
          const card = grid.querySelector(`[data-card-type="${type}"]`);
          if (card) grid.appendChild(card);
        });
        document.body.classList.toggle("monochrome", state.settings.colorStyle === "mono");
        $("rewardSummary").hidden = state.settings.showRewardSummary === false;
        $("ledgerSection").hidden = state.settings.showHistory === false;
      }

      function renderAll() {
        applyHomeSettings();
        renderHeaderAndRewards();
        renderIeltsCard();
        renderWeightCard();
        renderSleepCard();
        renderHomeLedger();
      }

      function renderIeltsDetail() {
        const today = ensureDaily();
        const total = totalWords();
        const progress = currentRound(total, 500);
        $("examDateInput").value = state.settings.examDate || "";
        $("todayNewWords").value = safeNumber(today.words.newWords) || "";
        $("todayReviewWords").value = safeNumber(today.words.reviewWords) || "";
        $("wordNote").value = today.words.note || "";
        $("ieltsTotalDetail").textContent = `${total.toLocaleString()}词`;
        $("ieltsMilestonesDetail").textContent = `${milestonesFor("ielts").length}次`;
        $("ieltsRemainingDetail").textContent = `${remainingToNext(total, 500)}词`;
        $("ieltsDetailProgressText").textContent = `${progress} / 500`;
        $("ieltsDetailProgressFill").style.width = `${progress / 500 * 100}%`;

        const rows = Object.keys(state.daily).sort().reverse().filter(key => safeNumber(state.daily[key]?.words?.newWords) > 0 || safeNumber(state.daily[key]?.words?.reviewWords) > 0 || state.daily[key]?.words?.note).slice(0, 14).map(key => {
          const words = state.daily[key].words;
          const note = words.note ? escapeHtml(words.note) : `复习${safeNumber(words.reviewWords)}词`;
          return `<div class="history-row"><div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div><div class="note">${note}</div><div class="value">+${safeNumber(words.newWords)}词</div></div>`;
        });
        $("ieltsHistory").innerHTML = rows.length ? rows.join("") : `<div class="empty-state">还没有单词记录。</div>`;
      }

      function weightGoalProgress() {
        const first = firstWeight();
        const latest = latestWeight();
        const target = safeNumber(state.settings.targetWeight, 55);
        if (!first || !latest || first.value === target) return { ratio: 0, text: "尚无足够数据" };
        const direction = target < first.value ? -1 : 1;
        const totalDistance = Math.abs(target - first.value);
        const completed = direction === -1 ? first.value - latest.value : latest.value - first.value;
        const ratio = clamp(completed / totalDistance, 0, 1);
        return { ratio, text: `${Math.round(ratio * 100)}%` };
      }

      function renderWeightDetail() {
        const today = ensureDaily();
        const first = firstWeight();
        const latest = latestWeight();
        const target = safeNumber(state.settings.targetWeight, 55);
        const points = totalHealthPoints();
        const progress = currentRound(points, 50);
        const goal = weightGoalProgress();
        $("targetWeightInput").value = target;
        const todayValue = safeNumber(today.weight.value, NaN);
        $("todayWeightInput").value = Number.isFinite(todayValue) && todayValue > 0 ? todayValue : "";
        $("noLateSnack").checked = Boolean(today.weight.noLateSnack);
        $("noMilkTea").checked = Boolean(today.weight.noMilkTea);
        $("eightyFull").checked = Boolean(today.weight.eightyFull);
        $("startWeightDetail").textContent = first ? `${first.value.toFixed(1)} kg` : "—";
        $("currentWeightDetail").textContent = latest ? `${latest.value.toFixed(1)} kg` : "—";
        $("weightRemainingDetail").textContent = latest ? `${Math.abs(latest.value - target).toFixed(1)} kg` : "—";
        $("weightGoalText").textContent = goal.text;
        $("weightGoalFill").style.width = `${goal.ratio * 100}%`;
        $("healthDetailProgressText").textContent = `${progress} / 50`;
        $("healthDetailProgressFill").style.width = `${progress / 50 * 100}%`;

        const rows = Object.keys(state.daily).sort().reverse().filter(key => {
          const entry = state.daily[key];
          return safeNumber(entry?.weight?.value, NaN) > 0 || healthPointsForEntry(entry) > 0;
        }).slice(0, 14).map(key => {
          const entry = state.daily[key];
          const value = safeNumber(entry.weight.value, NaN);
          return `<div class="history-row"><div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div><div class="note">${healthPointsForEntry(entry)}个健康选择</div><div class="value">${Number.isFinite(value) && value > 0 ? `${value.toFixed(1)} kg` : "—"}</div></div>`;
        });
        $("weightHistory").innerHTML = rows.length ? rows.join("") : `<div class="empty-state">还没有体重或健康选择记录。</div>`;
        window.requestAnimationFrame(drawWeightChart);
      }

      function renderSleepDetail() {
        const run = currentSleepRun();
        const best = bestSleepStreak();
        const progress = run.streak % 14;
        const windowInfo = getNightWindow();
        const checked = windowInfo.canCheckIn && Boolean(state.sleepCheckins[windowInfo.nightKey]);
        $("sleepModalStreak").textContent = `${run.streak} ${run.streak === 1 ? "day" : "days"}`;
        $("sleepWindowText").textContent = checked
          ? `本晚已于${new Intl.DateTimeFormat("zh-CN", { hour: "2-digit", minute: "2-digit" }).format(new Date(state.sleepCheckins[windowInfo.nightKey]))}完成打卡。`
          : windowInfo.message;
        $("sleepCheckinButton").disabled = !windowInfo.canCheckIn || checked;
        $("sleepCheckinButton").textContent = checked ? "今晚已完成" : windowInfo.canCheckIn ? "完成今晚打卡" : "当前不在打卡时间";
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
          const value = checkedAt ? new Intl.DateTimeFormat("zh-CN", { hour: "2-digit", minute: "2-digit" }).format(new Date(checkedAt)) : "Missed";
          return `<div class="history-row"><div class="date">${escapeHtml(formatShortDate(dateFromKey(key)))}</div><div class="note">${checkedAt ? "Success" : "未完成"}</div><div class="value">${value}</div></div>`;
        }).join("");
      }

      function latestGiftMarkup(record) {
        return `<div class="gift-card-view"><div class="muted">THREE · MYSTERY GIFT</div><div class="gift-title">A quiet<br>reward.</div><div class="gift-name">For ${FIXED_USER}</div><div class="gift-description">凭此卡向${FIXED_AUTHOR}兑换一份神秘礼品。具体礼物由赠送人准备。</div><div class="gift-code"><span>${escapeHtml(record.code)}</span><span>${escapeHtml(formatShortDate(new Date(record.createdAt)))}</span></div></div>`;
      }

      function renderLedger() {
        const earned = state.milestones.length;
        const spent = spentPoints();
        const balance = milestoneBalance();
        $("ledgerEarned").textContent = earned;
        $("ledgerSpent").textContent = spent;
        $("ledgerBalance").textContent = balance;
        $("redeemLedger").disabled = balance < 2;
        $("redeemLedger").textContent = balance >= 2 ? "使用2个积分兑换" : "积分不足2个";

        const latest = [...state.redemptions].sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))[0];
        $("latestGiftSection").hidden = !latest;
        $("latestGiftCard").innerHTML = latest ? latestGiftMarkup(latest) : "";

        $("giftRecords").innerHTML = state.redemptions.length
          ? [...state.redemptions].sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt)).map(record => `
              <div class="gift-record"><div class="gift-record-top"><div><strong>${escapeHtml(record.code)}</strong><span>${escapeHtml(formatDateTime(record.createdAt))}</span></div><button class="status-tag ${record.status === "claimed" ? "claimed" : ""}" type="button" data-gift-status="${escapeHtml(record.id)}">${record.status === "claimed" ? "已领取" : "待领取"}</button></div></div>`).join("")
          : `<div class="empty-state">还没有生成礼品兑换卡。</div>`;

        $("ledgerRecentList").innerHTML = combinedLedger().length
          ? combinedLedger().map(renderRecentItem).join("")
          : `<div class="empty-state">还没有里程记录。</div>`;

        document.querySelectorAll("[data-gift-status]").forEach(button => {
          button.addEventListener("click", () => toggleGiftStatus(button.dataset.giftStatus));
        });
      }

      function prefillSettings() {
        $("settingsExamDate").value = state.settings.examDate || "";
        $("settingsTargetWeight").value = safeNumber(state.settings.targetWeight, 55);
        $("cardOrderInput").value = state.settings.cardOrder || "ielts,weight,sleep";
        $("colorStyleInput").value = state.settings.colorStyle || "color";
        $("showRewardInput").value = state.settings.showRewardSummary === false ? "hide" : "show";
        $("showHistoryInput").value = state.settings.showHistory === false ? "hide" : "show";
      }

      function submitIelts(event) {
        event.preventDefault();
        const entry = ensureDaily();
        entry.words.newWords = Math.max(0, safeNumber($("todayNewWords").value));
        entry.words.reviewWords = Math.max(0, safeNumber($("todayReviewWords").value));
        entry.words.note = $("wordNote").value.trim();
        state.settings.examDate = $("examDateInput").value;
        const balanceBefore = milestoneBalance();
        const added = syncWordMilestones();
        saveState();
        closeModal($("ieltsModal"));
        renderAll();
        showToast("单词记录已保存");
        added.forEach(item => enqueuePopup({ type: "milestone", item }));
        if (balanceBefore < 2 && milestoneBalance() >= 2) enqueuePopup({ type: "gift-unlocked" });
      }

      function submitWeight(event) {
        event.preventDefault();
        const entry = ensureDaily();
        const value = $("todayWeightInput").value;
        entry.weight.value = value === "" ? "" : safeNumber(value);
        entry.weight.noLateSnack = $("noLateSnack").checked;
        entry.weight.noMilkTea = $("noMilkTea").checked;
        entry.weight.eightyFull = $("eightyFull").checked;
        state.settings.targetWeight = safeNumber($("targetWeightInput").value, 55);
        const balanceBefore = milestoneBalance();
        const added = syncHealthMilestones();
        saveState();
        closeModal($("weightModal"));
        renderAll();
        showToast("体重与健康选择已保存");
        added.forEach(item => enqueuePopup({ type: "milestone", item }));
        if (balanceBefore < 2 && milestoneBalance() >= 2) enqueuePopup({ type: "gift-unlocked" });
      }

      function performSleepCheckin() {
        const windowInfo = getNightWindow();
        if (!windowInfo.canCheckIn) { showToast("当前不在20:00至00:59的打卡时间内"); return; }
        if (state.sleepCheckins[windowInfo.nightKey]) { showToast("今晚已经完成打卡"); return; }
        const balanceBefore = milestoneBalance();
        state.sleepCheckins[windowInfo.nightKey] = new Date().toISOString();
        const added = syncSleepMilestones();
        saveState();
        renderAll();
        renderSleepDetail();
        showToast("早睡打卡成功");
        added.forEach(item => enqueuePopup({ type: "milestone", item }));
        if (balanceBefore < 2 && milestoneBalance() >= 2) enqueuePopup({ type: "gift-unlocked" });
      }

      function submitSettings(event) {
        event.preventDefault();
        state.settings = fixedSettings({
          ...state.settings,
          examDate: $("settingsExamDate").value,
          targetWeight: safeNumber($("settingsTargetWeight").value, 55),
          cardOrder: $("cardOrderInput").value || "ielts,weight,sleep",
          colorStyle: $("colorStyleInput").value || "color",
          showRewardSummary: $("showRewardInput").value !== "hide",
          showHistory: $("showHistoryInput").value !== "hide"
        });
        saveState();
        closeModal($("settingsModal"));
        renderAll();
        showToast("设置已保存");
      }

      function redeemGift(askConfirm = true) {
        if (milestoneBalance() < 2) { showToast("当前里程积分不足2个"); return; }
        if (askConfirm && !window.confirm(`确认使用2个里程积分，向${FIXED_AUTHOR}兑换一份神秘礼品吗？`)) return;
        const record = {
          id: `gift-${Date.now()}-${Math.random().toString(36).slice(2, 7)}`,
          code: `THREE-${todayKey().replaceAll("-", "")}-${Math.random().toString(36).slice(2, 6).toUpperCase()}`,
          status: "pending",
          createdAt: new Date().toISOString(),
          claimedAt: null
        };
        state.redemptions.push(record);
        saveState();
        renderAll();
        openModal("ledgerModal");
        showToast("神秘礼品兑换卡已生成");
      }

      function toggleGiftStatus(id) {
        const record = state.redemptions.find(item => item.id === id);
        if (!record) return;
        if (record.status === "pending") {
          if (!window.confirm("确认这份神秘礼品已经领取了吗？")) return;
          record.status = "claimed";
          record.claimedAt = new Date().toISOString();
        } else {
          if (!window.confirm("要将状态恢复为待领取吗？")) return;
          record.status = "pending";
          record.claimedAt = null;
        }
        saveState();
        renderAll();
        renderLedger();
      }

      async function exportData() {
        const payload = { app: "Three", version: APP_VERSION, exportedAt: new Date().toISOString(), data: state };
        const filename = `three-${FIXED_USER}-${todayKey()}.json`;
        const content = JSON.stringify(payload, null, 2);
        const blob = new Blob([content], { type: "application/json" });
        try {
          const file = new File([blob], filename, { type: "application/json" });
          if (navigator.share && navigator.canShare && navigator.canShare({ files: [file] })) {
            await navigator.share({ title: "Three 数据备份", text: `${FIXED_USER}的Three打卡数据`, files: [file] });
            showToast("数据已打开分享面板");
            return;
          }
        } catch (error) {
          if (error?.name === "AbortError") return;
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
        showToast("JSON备份已导出");
      }

      function importData(file) {
        if (!file) return;
        const reader = new FileReader();
        reader.onload = () => {
          try {
            const parsed = JSON.parse(String(reader.result));
            const imported = parsed.data || parsed;
            if (!imported || typeof imported !== "object") throw new Error("Invalid backup");
            state = {
              version: APP_VERSION,
              settings: fixedSettings(imported.settings || {}),
              daily: imported.daily && typeof imported.daily === "object" ? imported.daily : {},
              sleepCheckins: imported.sleepCheckins && typeof imported.sleepCheckins === "object" ? imported.sleepCheckins : {},
              milestones: Array.isArray(imported.milestones) ? imported.milestones : [],
              redemptions: Array.isArray(imported.redemptions) ? imported.redemptions : []
            };
            syncAllMilestones(false);
            saveState();
            renderAll();
            prefillSettings();
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
        if (!window.confirm("确定要清空所有记录、里程积分和兑换卡吗？此操作无法撤销。")) return;
        if (!window.confirm("请再次确认：真的要清空全部数据吗？")) return;
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
        if (!canvas || canvas.offsetParent === null) return;
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
        if (max === min) { max += .5; min -= .5; }
        const x = index => index / (points.length - 1) * width;
        const y = value => 5 + (1 - (value - min) / (max - min)) * (height - 10);
        ctx.beginPath();
        points.forEach((point, index) => index === 0 ? ctx.moveTo(x(index), y(point.value)) : ctx.lineTo(x(index), y(point.value)));
        ctx.strokeStyle = "rgba(36,161,72,.72)";
        ctx.lineWidth = 2;
        ctx.lineCap = "round";
        ctx.lineJoin = "round";
        ctx.stroke();
      }

      function drawWeightChart() {
        const canvas = $("weightChart");
        if (!canvas || canvas.offsetParent === null) return;
        const points = weightEntries();
        const { ctx, width, height } = setupCanvas(canvas);
        ctx.clearRect(0, 0, width, height);
        $("weightChartEmpty").style.display = points.length >= 2 ? "none" : "grid";
        if (points.length < 2) return;
        const padding = { top: 24, right: 22, bottom: 34, left: 24 };
        const chartWidth = Math.max(1, width - padding.left - padding.right);
        const chartHeight = Math.max(1, height - padding.top - padding.bottom);
        const values = points.map(item => item.value);
        let min = Math.min(...values);
        let max = Math.max(...values);
        if (max === min) { max += .5; min -= .5; }
        const x = index => padding.left + index / (points.length - 1) * chartWidth;
        const y = value => padding.top + (1 - (value - min) / (max - min)) * chartHeight;
        ctx.strokeStyle = "rgba(0,0,0,.07)";
        ctx.lineWidth = 1;
        [0, .5, 1].forEach(ratio => {
          const gy = padding.top + ratio * chartHeight;
          ctx.beginPath(); ctx.moveTo(padding.left, gy); ctx.lineTo(width - padding.right, gy); ctx.stroke();
        });
        ctx.beginPath();
        points.forEach((point, index) => index === 0 ? ctx.moveTo(x(index), y(point.value)) : ctx.lineTo(x(index), y(point.value)));
        ctx.strokeStyle = "rgba(36,161,72,.9)";
        ctx.lineWidth = 2.5;
        ctx.lineCap = "round";
        ctx.lineJoin = "round";
        ctx.stroke();
        points.forEach((point, index) => {
          ctx.beginPath(); ctx.arc(x(index), y(point.value), index === points.length - 1 ? 4 : 2.5, 0, Math.PI * 2); ctx.fillStyle = "rgba(36,161,72,1)"; ctx.fill();
        });
        ctx.fillStyle = "rgba(0,0,0,.44)";
        ctx.font = "11px -apple-system, BlinkMacSystemFont, sans-serif";
        ctx.textAlign = "left";
        ctx.fillText(`${points[0].value.toFixed(1)} kg`, padding.left, 12);
        ctx.fillText(formatShortDate(points[0].date), padding.left, height - 8);
        ctx.textAlign = "right";
        ctx.fillText(`${points[points.length - 1].value.toFixed(1)} kg`, width - padding.right, 12);
        ctx.fillText(formatShortDate(points[points.length - 1].date), width - padding.right, height - 8);
      }

      function bindOpenableCards() {
        document.querySelectorAll("[data-open]").forEach(element => {
          const open = () => openModal(element.dataset.open);
          element.addEventListener("click", open);
          element.addEventListener("keydown", event => {
            if (event.key === "Enter" || event.key === " ") { event.preventDefault(); open(); }
          });
        });
      }

      function bindEvents() {
        bindOpenableCards();
        document.querySelectorAll("[data-close]").forEach(button => button.addEventListener("click", () => closeModal(button.closest(".modal-layer"))));
        document.querySelectorAll(".modal-layer:not(#celebrationLayer)").forEach(layer => layer.addEventListener("click", event => { if (event.target === layer) closeModal(layer); }));
        document.addEventListener("keydown", event => {
          if (event.key !== "Escape") return;
          const layer = [...document.querySelectorAll(".modal-layer.open")].find(item => item.id !== "celebrationLayer");
          if (layer) closeModal(layer);
        });

        $("openLedger").addEventListener("click", () => openModal("ledgerModal"));
        $("openSettings").addEventListener("click", () => openModal("settingsModal"));
        $("redeemHome").addEventListener("click", () => redeemGift(true));
        $("redeemLedger").addEventListener("click", () => redeemGift(true));
        $("ieltsForm").addEventListener("submit", submitIelts);
        $("weightForm").addEventListener("submit", submitWeight);
        $("settingsForm").addEventListener("submit", submitSettings);
        $("sleepCheckinButton").addEventListener("click", performSleepCheckin);
        $("exportData").addEventListener("click", exportData);
        $("importData").addEventListener("click", () => $("importFile").click());
        $("importFile").addEventListener("change", event => importData(event.target.files[0]));
        $("clearData").addEventListener("click", clearAllData);

        window.addEventListener("scroll", () => $("topbar").classList.toggle("scrolled", window.scrollY > 8), { passive: true });
        let resizeTimer;
        window.addEventListener("resize", () => {
          window.clearTimeout(resizeTimer);
          resizeTimer = window.setTimeout(() => { drawWeightSparkline(); if ($("weightModal").classList.contains("open")) drawWeightChart(); }, 120);
        });
      }

      function init() {
        ensureDaily();
        syncAllMilestones(false);
        saveState();
        bindEvents();
        renderAll();
        requestAnimationFrame(() => $("app").classList.add("ready"));
      }

      init();
    })();
  </script>
</body>
</html>
