<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Speed Check — Internet Speed Test</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>⚡️</text></svg>" />
<style>
  :root {
    --bg: #0a0a0f;
    --card: rgba(255,255,255,.04);
    --border: rgba(255,255,255,.08);
    --text: #e2e2e8;
    --dim: #6b6b8d;
    --purple: #a56bff;
    --blue: #4d7cff;
    --cyan: #33c9a4;
    --pink: #ff6eb4;
    --radius: 20px;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  @keyframes gradient { 0%{background-position:0% 50%} 50%{background-position:100% 50%} 100%{background-position:0% 50%} }
  @keyframes spin { to { transform: rotate(360deg) } }
  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    background: var(--bg); color: var(--text);
    min-height: 100vh; display: flex; flex-direction: column;
    align-items: center; justify-content: center; padding: 20px; overflow-x: hidden;
  }
  body::before, body::after {
    content: ''; position: fixed; width: 600px; height: 600px;
    border-radius: 50%; filter: blur(120px); opacity: .12; z-index: 0; pointer-events: none;
  }
  body::before { background: var(--purple); top: -200px; left: -200px; }
  body::after  { background: var(--blue);  bottom: -200px; right: -200px; }
  .container {
    position: relative; z-index: 1; max-width: 520px; width: 100%;
    display: flex; flex-direction: column; align-items: center; gap: 32px;
  }
  h1 {
    font-size: 2.2rem; font-weight: 800;
    background: linear-gradient(135deg, var(--purple), var(--blue), var(--cyan));
    background-size: 200% 200%; animation: gradient 6s ease infinite;
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .subtitle { color: var(--dim); font-size: .95rem; margin-top: -20px; }
  .gauge-wrap {
    position: relative; width: 280px; height: 280px;
    display: flex; align-items: center; justify-content: center;
  }
  .gauge-svg { width: 100%; height: 100%; }
  .gauge-center { position: absolute; display: flex; flex-direction: column; align-items: center; gap: 4px; }
  .gauge-value { font-size: 3.5rem; font-weight: 800; line-height: 1; }
  .gauge-unit { font-size: .9rem; color: var(--dim); text-transform: uppercase; letter-spacing: .15em; }
  .gauge-label { font-size: .85rem; color: var(--dim); margin-top: 4px; }
  .btn {
    width: 100%; max-width: 300px; padding: 16px 32px;
    background: linear-gradient(135deg, var(--purple), var(--blue));
    border: none; border-radius: 60px; color: #fff;
    font-size: 1.1rem; font-weight: 700; cursor: pointer;
    box-shadow: 0 0 30px rgba(165,107,255,.35); transition: transform .15s, box-shadow .3s;
  }
  .btn:hover { transform: translateY(-2px); box-shadow: 0 0 50px rgba(165,107,255,.55); }
  .btn:disabled { opacity: .5; cursor: not-allowed; }
  .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; width: 100%; }
  .stat-card {
    background: var(--card); border: 1px solid var(--border); border-radius: var(--radius);
    padding: 18px 14px; display: flex; flex-direction: column; align-items: center; gap: 8px;
    backdrop-filter: blur(12px);
  }
  .stat-label { font-size: .7rem; color: var(--dim); text-transform: uppercase; letter-spacing: .12em; }
  .stat-value { font-size: 1.4rem; font-weight: 700; }
  .stat-unit { font-size: .65rem; color: var(--dim); }
  .stat-download .stat-value { color: var(--cyan); }
  .stat-upload  .stat-value { color: var(--blue); }
  .stat-ping     .stat-value { color: var(--pink); }
  .server-info { display: flex; gap: 20px; flex-wrap: wrap; justify-content: center; font-size: .78rem; color: var(--dim); }
  .server-info span { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 6px 14px; }
  .btn.testing { pointer-events: none; }
  .btn.testing::before {
content: ''; position: absolute; left: -28px; top: 50%; transform: translateY(-50%);
    width: 20px; height: 20px; border: 2px solid rgba(255,255,255,.3);
    border-top-color: #fff; border-radius: 50%; animation: spin .6s linear infinite;
  }
  .progress-bar { width: 100%; height: 4px; border-radius: 4px; background: rgba(255,255,255,.06); overflow: hidden; }
  .progress-fill {
    height: 100%; width: 0%; border-radius: 4px;
    background: linear-gradient(90deg, var(--purple), var(--blue), var(--cyan));
    background-size: 200% 100%; animation: gradient 2s ease infinite; transition: width .3s;
  }
  footer { margin-top: 24px; font-size: .72rem; color: var(--dim); opacity: .5; }
</style>
</head>
<body>
<div class="container">
  <h1>⚡️ Speed Check</h1>
  <p class="subtitle">Real-time internet speed test, powered by Cloudflare</p>
  <div class="gauge-wrap">
    <svg class="gauge-svg" viewBox="0 0 280 280" id="gaugeSvg">
      <circle cx="140" cy="140" r="120" fill="none" stroke="rgba(255,255,255,.06)" stroke-width="10"
        stroke-dasharray="565.48 188.5" stroke-dashoffset="-94.25" stroke-linecap="round" transform="rotate(135 140 140)" />
      <circle id="gaugeArc" cx="140" cy="140" r="120" fill="none" stroke="url(#gaugeGradient)" stroke-width="10"
        stroke-dasharray="0 753.98" stroke-dashoffset="-94.25" stroke-linecap="round" transform="rotate(135 140 140)" />
      <g id="ticks"></g>
      <defs><linearGradient id="gaugeGradient" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#a56bff"/><stop offset="50%" stop-color="#4d7cff"/><stop offset="100%" stop-color="#33c9a4"/>
      </linearGradient></defs>
    </svg>
    <div class="gauge-center">
      <div class="gauge-value" id="gaugeValue">0</div>
      <div class="gauge-unit">Mbps</div>
      <div class="gauge-label" id="gaugeLabel">Ready</div>
    </div>
  </div>
  <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
  <div class="stats">
    <div class="stat-card stat-ping">
      <svg class="stat-icon" viewBox="0 0 32 32" fill="none"><path d="M16 4v4M16 24v4M4 16h4M24 16h4" stroke="#ff6eb4" stroke-width="2" stroke-linecap="round"/><circle cx="16" cy="16" r="8" stroke="#ff6eb4" stroke-width="2"/></svg>
      <span class="stat-label">Ping</span><span class="stat-value" id="pingValue">—</span><span class="stat-unit">ms</span>
    </div>
    <div class="stat-card stat-download">
      <svg class="stat-icon" viewBox="0 0 32 32" fill="none"><path d="M16 4v16M16 20l-5-5M16 20l5-5" stroke="#33c9a4" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/><path d="M6 26h20" stroke="#33c9a4" stroke-width="2" stroke-linecap="round"/></svg>
      <span class="stat-label">Download</span><span class="stat-value" id="dlValue">—</span><span class="stat-unit">Mbps</span>
    </div>
    <div class="stat-card stat-upload">
      <svg class="stat-icon" viewBox="0 0 32 32" fill="none"><path d="M16 28V12M16 12l5 5M16 12l-5 5" stroke="#4d7cff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/><path d="M6 6h20" stroke="#4d7cff" stroke-width="2" stroke-linecap="round"/></svg>
      <span class="stat-label">Upload</span><span class="stat-value" id="ulValue">—</span><span class="stat-unit">Mbps</span>
    </div>
  </div>
  <div class="server-info">
    <span id="serverName">Server: Cloudflare Edge</span><span id="ipInfo">IP: Loading…</span>
  </div>
  <button class="btn" id="startBtn" onclick="startTest()">Start Test</button>
</div>
<footer>Built with ❤️ — No installation required</footer>
<script>
const arc = document.getElementById('gaugeArc');
const gaugeV = document.getElementById('gaugeValue');
const gaugeL = document.getElementById('gaugeLabel');
const prog = document.getElementById('progressFill');
const btn = document.getElementById('startBtn');
const CIRC = 753.98; const ARC_LEN = 565.48;

function setGauge(p) { p = Math.max(0,Math.min(100,p)); arc.setAttribute('stroke-dasharray',`${(p/100)*ARC_LEN} ${CIRC-(p/100)*ARC_LEN}`); }
