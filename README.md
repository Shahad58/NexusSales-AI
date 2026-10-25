<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NexusSales AI | Executive Sales Intelligence</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;0,600;0,700;0,800;1,500;1,600;1,700&family=Outfit:wght@300;400;500;600;700;800&family=IBM+Plex+Sans+Arabic:wght@300;400;500;600;700&family=Amiri:wght@400;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
<script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
<style>
:root {
    --en-body: 'Outfit', sans-serif;
    --en-display: 'Playfair Display', Georgia, serif;
    --ar-body: 'IBM Plex Sans Arabic', 'Tajawal', sans-serif;
    --ar-display: 'Amiri', 'Noto Naskh Arabic', serif;
}
html { font-size: 90%; scroll-behavior: smooth; }
body {
    font-family: var(--en-body);
    background-color: #040711;
    color: #f8fafc;
    background-image:
        radial-gradient(at 10% 15%, hsla(250, 85%, 58%, 0.20) 0px, transparent 55%),
        radial-gradient(at 90% 10%, hsla(190, 85%, 50%, 0.16) 0px, transparent 55%),
        radial-gradient(at 50% 90%, hsla(320, 85%, 55%, 0.14) 0px, transparent 55%);
    background-attachment: fixed;
    min-height: 100vh;
    overflow-x: hidden;
    -webkit-font-smoothing: antialiased;
}
.material-symbols-outlined { font-variation-settings: 'FILL' 0, 'wght' 400, 'GRAD' 0, 'opsz' 24; vertical-align: middle; }
.font-playfair, .hero-display { font-family: var(--en-display); }
body.arabic-mode { font-family: var(--ar-body); }
body.arabic-mode .font-playfair,
body.arabic-mode .hero-display {
    font-family: var(--ar-display);
    font-weight: 700;
    line-height: 1.55 !important;
}
body.arabic-mode * { letter-spacing: 0 !important; text-transform: none !important; }
body.arabic-mode .italic { font-style: normal !important; }
body.arabic-mode .leading-relaxed { line-height: 2 !important; }
body.arabic-mode .insight-box {
    border-left: none; border-right: 4px solid #818cf8;
    border-radius: 12px 0 0 12px;
}
body.arabic-mode .feature-num { right: auto; left: -8px; }
.num { direction: ltr; unicode-bidi: isolate; display: inline-block; }
.js-plotly-plot, .q-slider { direction: ltr; }
body.arabic-mode .js-plotly-plot { direction: ltr; }

body::before {
    content: ''; position: fixed; inset: 0; z-index: -3; pointer-events: none;
    background-image:
        radial-gradient(1.5px 1.5px at 100px 200px, rgba(255,255,255,0.95), transparent),
        radial-gradient(1px 1px at 300px 100px, rgba(200,220,255,0.8), transparent),
        radial-gradient(2px 2px at 500px 300px, rgba(255,255,255,0.9), transparent),
        radial-gradient(1px 1px at 700px 150px, rgba(255,255,255,0.7), transparent),
        radial-gradient(1.5px 1.5px at 200px 500px, rgba(180,200,255,0.85), transparent),
        radial-gradient(1px 1px at 800px 400px, rgba(255,255,255,0.75), transparent),
        radial-gradient(2px 2px at 450px 600px, rgba(255,255,255,0.6), transparent),
        radial-gradient(1px 1px at 650px 700px, rgba(255,255,255,0.8), transparent),
        radial-gradient(1.5px 1.5px at 950px 200px, rgba(200,220,255,0.7), transparent),
        radial-gradient(1px 1px at 50px 800px, rgba(255,255,255,0.85), transparent);
    background-size: 1300px 900px; background-repeat: repeat;
    animation: nexusTwinkle 8s ease-in-out infinite alternate;
}
body::after {
    content: ''; position: fixed; inset: 0; z-index: -1; pointer-events: none;
    background-image:
        linear-gradient(rgba(99,102,241,0.06) 1px, transparent 1px),
        linear-gradient(90deg, rgba(99,102,241,0.06) 1px, transparent 1px);
    background-size: 55px 55px;
    mask-image: radial-gradient(ellipse at 50% 45%, black 15%, transparent 75%);
    -webkit-mask-image: radial-gradient(ellipse at 50% 45%, black 15%, transparent 75%);
}
@keyframes nexusTwinkle { 0%{opacity:.35;} 100%{opacity:1;} }
#fx-canvas { position: fixed; inset: 0; z-index: -2; pointer-events: none; opacity: .85; }
.orb { position: fixed; border-radius: 50%; filter: blur(90px); pointer-events: none; z-index: -2; opacity: .5; }
.orb-1 { width: 420px; height: 420px; top: -120px; left: -100px; background: #4f46e5; animation: orbA 22s ease-in-out infinite alternate; }
.orb-2 { width: 360px; height: 360px; top: 30%; right: -120px; background: #0891b2; animation: orbB 26s ease-in-out infinite alternate; }
.orb-3 { width: 380px; height: 380px; bottom: -140px; left: 30%; background: #be185d; opacity: .35; animation: orbA 30s ease-in-out infinite alternate-reverse; }
@keyframes orbA { to { transform: translate(120px, 90px) scale(1.15); } }
@keyframes orbB { to { transform: translate(-110px, -80px) scale(1.2); } }

.reveal { animation: revealUp .85s cubic-bezier(.2,.7,.2,1) var(--d, 0s) backwards; }
@keyframes revealUp { from { opacity: 0; transform: translateY(26px); } to { opacity: 1; transform: none; } }
@keyframes floatY { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
@keyframes spin { to { transform: rotate(360deg); } }
@keyframes spinRev { to { transform: rotate(-360deg); } }
@keyframes gradShift { to { background-position: 200% center; } }
@keyframes pulseDot { 0%,100% { box-shadow: 0 0 0 0 rgba(52,211,153,.6); } 70% { box-shadow: 0 0 0 9px rgba(52,211,153,0); } }

.glass-nav {
    background: rgba(4, 7, 17, 0.82);
    backdrop-filter: blur(22px);
    border-bottom: 1px solid rgba(255,255,255,0.08);
}
.brand-tile {
    width: 40px; height: 40px; border-radius: 12px;
    background: linear-gradient(135deg, #6366f1, #a855f7 55%, #ec4899);
    display: flex; align-items: center; justify-content: center;
    box-shadow: 0 8px 22px -6px rgba(139,92,246,.8);
}
.nav-link {
    color: #cbd5e1; font-weight: 600; font-size: .88rem;
    padding: .45rem .9rem; border-radius: 999px; cursor: pointer;
    transition: all .25s ease; white-space: nowrap;
}
.nav-link:hover { color: #fff; background: rgba(129,140,248,.14); }
.nav-link.active {
    color: #fff;
    background: linear-gradient(135deg, rgba(99,102,241,.38), rgba(168,85,247,.26));
    box-shadow: inset 0 0 0 1px rgba(129,140,248,.5);
}
.lang-toggle { border: 1px solid rgba(255,255,255,.16); border-radius: 999px; overflow: hidden; display: flex; }
.lang-btn {
    padding: 5px 15px; background: transparent; border: none;
    color: #94a3b8; font-weight: 700; font-size: 13px; cursor: pointer;
    transition: all .2s; font-family: inherit;
}
.lang-btn.active { background: #6366f1; color: white; }
.lang-btn:hover:not(.active) { color: white; }

.glass-card {
    background: rgba(18, 26, 45, 0.7);
    backdrop-filter: blur(16px);
    border: 1px solid rgba(255,255,255,0.08);
    transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
    color: white;
}
.glass-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 25px 45px -12px rgba(99, 102, 241, 0.35);
    border-color: rgba(99, 102, 241, 0.5);
}
.hero-text {
    background: linear-gradient(110deg, #818cf8 0%, #22d3ee 25%, #c084fc 50%, #f472b6 75%, #818cf8 100%);
    background-size: 200% auto;
    -webkit-background-clip: text; background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: gradShift 7s linear infinite;
}
.insight-box {
    background: linear-gradient(135deg, rgba(99, 102, 241, 0.12) 0%, rgba(192, 132, 252, 0.05) 100%);
    border-left: 4px solid #818cf8; border-radius: 0 12px 12px 0;
}
.glow-line {
    height: 2px; width: 100%;
    background: linear-gradient(90deg,transparent,#818cf8,#22d3ee,#c084fc,transparent);
    background-size: 200% 100%; animation: glowMove 5s linear infinite; opacity: .8;
}
@keyframes glowMove { to { background-position: 200% 0; } }
.live-dot { width: 9px; height: 9px; border-radius: 50%; background: #34d399; display: inline-block; animation: pulseDot 2s infinite; }
.kpi-icon {
    width: 44px; height: 44px; border-radius: 14px; display: flex; align-items: center; justify-content: center;
    background: rgba(99,102,241,.14); border: 1px solid rgba(129,140,248,.25);
}

.hero-visual { position: relative; width: 100%; max-width: 760px; margin: 6px auto 0; }
.hero-visual svg { width: 100%; height: auto; display: block; filter: drop-shadow(0 30px 60px rgba(79,70,229,.35)); }
.vbar { transform-box: fill-box; transform-origin: 50% 100%; animation: barGrow 1.1s cubic-bezier(.2,.8,.2,1) backwards; }
@keyframes barGrow { from { transform: scaleY(0); opacity: 0; } to { transform: scaleY(1); opacity: 1; } }
.draw-line { stroke-dasharray: 1; stroke-dashoffset: 0; animation: drawLine 2.6s ease-out .5s backwards; }
@keyframes drawLine { from { stroke-dashoffset: 1; } to { stroke-dashoffset: 0; } }
.fade-in { animation: fadeIn 1.6s ease 1.4s backwards; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
.donut-seg { transform-box: fill-box; transform-origin: center; animation: donutIn 1.6s ease-out .6s backwards; }
@keyframes donutIn { from { opacity: 0; transform: rotate(-120deg); } to { opacity: 1; transform: none; } }
.float-chip {
    position: absolute; display: flex; align-items: center; gap: 10px;
    padding: 10px 16px 10px 10px; border-radius: 16px; z-index: 3;
    background: rgba(15,23,42,.8); backdrop-filter: blur(14px);
    border: 1px solid rgba(255,255,255,.13);
    box-shadow: 0 18px 40px -14px rgba(0,0,0,.75);
    animation: floatY 5.5s ease-in-out infinite; text-align: start;
}
.chip-1 { top: 6%;  left: -2%;  animation-delay: 0s; }
.chip-2 { top: -3%; right: 0%;  animation-delay: -1.8s; }
.chip-3 { bottom: 4%; right: -3%; animation-delay: -3.2s; }
.chip-icon { width: 34px; height: 34px; border-radius: 11px; display: flex; align-items: center; justify-content: center; }
.chip-label { font-size: 10.5px; color: #94a3b8; font-weight: 600; line-height: 1.2; }
.chip-value { font-size: 15px; color: #fff; font-weight: 800; line-height: 1.3; max-width: 130px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

.upload-bubble-wrap { display: flex; flex-direction: column; align-items: center; gap: 14px; padding: 28px 0 18px; position: relative; }
.upload-bubble-wrap::before {
    content: ''; position: absolute; width: 260px; height: 260px; top: -8px; left: 50%;
    transform: translateX(-50%); border-radius: 50%;
    background: radial-gradient(circle, rgba(168,85,247,.30) 0%, transparent 70%);
    pointer-events: none; animation: bubblePulse 3.4s ease-in-out infinite;
}
@keyframes bubblePulse {
    0%,100% { transform: translateX(-50%) scale(1); opacity: .6; }
    50% { transform: translateX(-50%) scale(1.18); opacity: .95; }
}
.ring { position: absolute; left: 50%; top: 28px; border-radius: 50%; pointer-events: none; }
.ring-1 { width: 252px; height: 252px; margin: -31px 0 0 -126px; border: 1px dashed rgba(165,180,252,.5); animation: spin 22s linear infinite; }
.ring-2 { width: 312px; height: 312px; margin: -61px 0 0 -156px; border: 1px dotted rgba(34,211,238,.45); animation: spinRev 34s linear infinite; }
.ring-1::after, .ring-2::after {
    content: ''; position: absolute; top: -4px; left: 50%; width: 8px; height: 8px; border-radius: 50%;
    background: #22d3ee; box-shadow: 0 0 14px 3px rgba(34,211,238,.8);
}
.ring-2::after { background: #f472b6; box-shadow: 0 0 14px 3px rgba(244,114,182,.8); }
.upload-bubble {
    width: 190px; height: 190px; border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, #a5b4fc 0%, #6366f1 40%, #7c3aed 70%, #be185d 100%);
    display: flex; align-items: center; justify-content: center; cursor: pointer;
    box-shadow: 0 28px 60px -12px rgba(139, 92, 246, 0.7),
                inset 0 -10px 22px rgba(0, 0, 0, 0.28), inset 0 8px 20px rgba(255, 255, 255, 0.28);
    transition: all .35s cubic-bezier(.4,0,.2,1);
    animation: bubbleFloat 4s ease-in-out infinite; position: relative; z-index: 1;
}
.upload-bubble:hover { transform: scale(1.08); box-shadow: 0 35px 80px -12px rgba(139, 92, 246, 0.95), inset 0 -10px 22px rgba(0,0,0,.28), inset 0 8px 20px rgba(255,255,255,.35); }
.upload-bubble:active { transform: scale(0.97); }
@keyframes bubbleFloat { 0%,100% { transform: translateY(0) rotate(0deg); } 50% { transform: translateY(-10px) rotate(2deg); } }
.upload-bubble-label { font-size: 16px; font-weight: 700; color: #e0e7ff; text-align: center; z-index: 1; }
.upload-bubble-sub { font-size: 11px; color: #94a3b8; letter-spacing: .8px; font-weight: 600; z-index: 1; }

.feature-card {
    background: rgba(18,26,45,.7); backdrop-filter: blur(16px);
    border: 1px solid rgba(255,255,255,.08); border-radius: 24px;
    position: relative; overflow: hidden; transition: all .35s cubic-bezier(.4,0,.2,1);
    display: flex; flex-direction: column;
}
.feature-card::after {
    content: ''; position: absolute; inset: 0; border-radius: inherit; pointer-events: none;
    background: radial-gradient(500px circle at 20% -10%, rgba(129,140,248,.14), transparent 60%);
}
.feature-card:hover { transform: translateY(-6px) scale(1.015); border-color: rgba(129,140,248,.55); box-shadow: 0 25px 50px -12px rgba(99,102,241,.4); }
.feature-num {
    position: absolute; top: -22px; right: -8px; font-size: 6.5rem; font-weight: 800; line-height: 1;
    color: rgba(129,140,248,.08); font-family: var(--en-display); pointer-events: none;
}
.feature-icon {
    width: 58px; height: 58px; border-radius: 18px; display: flex; align-items: center; justify-content: center;
    box-shadow: 0 10px 24px -6px rgba(99,102,241,.45); margin-bottom: 16px;
}
.step-card {
    background: rgba(18,26,45,.65); border: 1px solid rgba(255,255,255,.08); border-radius: 22px;
    position: relative; overflow: hidden; transition: all .35s ease;
    display: flex; flex-direction: column;
}
.step-card:hover { border-color: rgba(129,140,248,.5); transform: translateY(-4px); }
.step-num-badge {
    width: 44px; height: 44px; border-radius: 12px; background: linear-gradient(135deg,#4338ca,#7c3aed);
    display: flex; align-items: center; justify-content: center; font-weight: 800; color: #fff;
    box-shadow: 0 8px 18px -4px rgba(99,102,241,.5);
}
.rec-num {
    width: 36px; height: 36px; flex: none; border-radius: 11px; display: flex; align-items: center; justify-content: center;
    background: linear-gradient(135deg, rgba(99,102,241,.35), rgba(168,85,247,.25));
    border: 1px solid rgba(129,140,248,.4);
}
.ripple-btn { position: relative; overflow: hidden; transition: transform 0.3s cubic-bezier(0.25, 0.8, 0.25, 1), box-shadow 0.3s; }
.ripple-btn:hover { transform: translateY(-2px); box-shadow: 0 18px 40px rgba(99, 102, 241, 0.55); }

.chat-fab {
    position: fixed; bottom: 25px; right: 25px; width: 56px; height: 56px;
    border-radius: 50%; background: #6366f1; display: flex; align-items: center; justify-content: center;
    cursor: pointer; box-shadow: 0 18px 40px rgba(99,102,241,.5); z-index: 100;
    transition: transform .25s;
}
.chat-fab:hover { transform: scale(1.1); }
.chat-dialog {
    position: fixed; bottom: 95px; right: 25px; width: 420px; height: 580px;
    background: #111827; border: 1px solid #374151; border-radius: 16px;
    display: flex; flex-direction: column; overflow: hidden;
    box-shadow: 0 25px 50px rgba(0,0,0,.5); z-index: 100;
}
.chat-header {
    padding: 16px; background: linear-gradient(to right, #4f46e5, #9333ea);
    color: white; display: flex; align-items: center; gap: 12px;
}
.chat-messages {
    flex: 1; overflow-y: auto; padding: 16px;
    display: flex; flex-direction: column; gap: 8px;
}
.chat-input-row {
    padding: 12px; background: #1f2937; border-top: 1px solid #374151;
    display: flex; gap: 8px; align-items: center;
}
#chat-input {
    flex: 1; background: #111827; border: 1px solid #4b5563; color: white;
    padding: 8px 12px; border-radius: 8px; outline: none; font-family: inherit;
}
.chat-send {
    background: #6366f1; border: none; color: white; width: 40px; height: 40px;
    border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center;
}
.chat-msg {
    padding: 12px 16px; border-radius: 16px; max-width: 85%;
    font-size: 14px; line-height: 1.5; word-wrap: break-word;
}
.chat-msg.user { background: #4f46e5; color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
.chat-msg.ai { background: #1f2937; color: #f3f4f6; align-self: flex-start; border-bottom-left-radius: 4px; }

.toast {
    position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
    padding: 12px 24px; border-radius: 10px; color: white; font-weight: 600;
    z-index: 200; box-shadow: 0 10px 30px rgba(0,0,0,.4); animation: toastIn .3s ease;
    max-width: 90vw;
}
.toast-positive { background: #10b981; }
.toast-negative { background: #ef4444; }
.toast-warning { background: #f59e0b; }
@keyframes toastIn {
    from { opacity: 0; transform: translateX(-50%) translateY(-20px); }
    to { opacity: 1; transform: translateX(-50%) translateY(0); }
}

@media (max-width: 900px) {
    html { font-size: 100%; }
    .glass-nav { padding-left: 10px !important; padding-right: 10px !important; }
    .nav-links { overflow-x: auto; max-width: 100%; }
    .upload-bubble { width: 155px; height: 155px; }
    .ring { display: none; }
    .float-chip { transform: scale(.8); }
    .chip-3 { display: none; }
}
@media (max-width: 500px) {
    .chat-dialog { width: calc(100vw - 20px); right: 10px; height: 70vh; bottom: 90px; }
}
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { animation-duration: .01ms !important; animation-iteration-count: 1 !important; }
}
</style>
</head>
<body>

<header class="glass-nav py-3 px-6 lg:px-10 flex justify-between items-center sticky top-0 z-50 gap-3">
    <div class="flex items-center gap-3 cursor-pointer" style="white-space:nowrap" onclick="setPage('home')">
        <div class="brand-tile">
            <span class="material-symbols-outlined text-white" style="font-size:1.4rem">insights</span>
        </div>
        <span class="text-xl font-playfair font-bold text-white" style="letter-spacing:-.02em">NexusSales AI</span>
    </div>
    <nav id="nav-links" class="flex gap-1 items-center nav-links" style="flex-wrap:nowrap"></nav>
    <div class="lang-toggle">
        <button id="lang-en" onclick="changeLanguage('en')" class="lang-btn active">EN</button>
        <button id="lang-ar" onclick="changeLanguage('ar')" class="lang-btn">عربي</button>
    </div>
</header>

<main id="content" class="w-full pb-12"></main>

<footer class="w-full flex justify-center py-8 mt-12 border-t border-gray-800 text-gray-400 text-sm items-center gap-1">
    <span class="font-medium" id="footer-text">Created by SHODO</span>
    <span style="color:#ef4444;font-size:1.1rem">❤️</span>
</footer>

<div id="chat-fab" onclick="toggleChat()" class="chat-fab">
    <span class="material-symbols-outlined text-white" style="font-size:1.8rem">psychology</span>
</div>
<div id="chat-dialog" class="chat-dialog" style="display:none">
    <div class="chat-header">
        <span class="material-symbols-outlined text-white" style="font-size:2rem">psychology</span>
        <div>
            <div id="chat-title-text" style="font-size:16px;font-weight:700;line-height:1.2">Executive Sales Assistant</div>
            <div id="chat-sub-text" style="font-size:12px;color:#e0e7ff">Powered by NexusSales AI</div>
        </div>
        <button onclick="toggleChat()" style="margin-left:auto;color:white;background:transparent;border:none;cursor:pointer">
            <span class="material-symbols-outlined">close</span>
        </button>
    </div>
    <div id="chat-messages" class="chat-messages"></div>
    <div class="chat-input-row">
        <input id="chat-input" placeholder="Ask about sales metrics..." onkeydown="if(event.key==='Enter')handleChat()">
        <button onclick="handleChat()" class="chat-send">
            <span class="material-symbols-outlined">send</span>
        </button>
    </div>
</div>

<input type="file" id="file-input" accept=".csv,.xlsx,.xls" style="position:absolute;width:1px;height:1px;opacity:0;pointer-events:none;z-index:-1">

<script>
// ============================================
// STATE
// ============================================
const state = {
    df: [],
    page: 'home',
    dataset_mode: null,
    currency_code: 'USD',
    currency_symbol: '$',
    language: 'en',
    explorerPage: 0
};

const CURRENCY_SYMBOLS = { USD:'$', GBP:'£', EUR:'€', SAR:'﷼', AED:'د.إ', EGP:'E£', JPY:'¥', INR:'₹', KWD:'KD', QAR:'QR', BHD:'BD', OMR:'RO' };
const CURRENCY_PATTERNS = [['$','USD'],['£','GBP'],['€','EUR'],['ر.س','SAR'],['د.إ','AED'],['﷼','SAR'],['E£','EGP'],['¥','JPY'],['₹','INR'],['AED','AED'],['SAR','SAR'],['USD','USD'],['GBP','GBP']];

// ============================================
// TRANSLATIONS
// ============================================
const TEXT = {
    app_name: ["NexusSales AI", "NexusSales AI"],
    app_subtitle: ["Executive Sales Intelligence & Decision Engine", "منصة الذكاء التنفيذي للمبيعات ودعم القرار"],
    nav_home: ["Home", "الرئيسية"],
    nav_dashboard: ["Dashboard", "لوحة المؤشرات"],
    nav_forecasts: ["Forecasts", "التنبؤات"],
    nav_insights: ["Insights", "الرؤى التحليلية"],
    nav_summary: ["Summary", "الملخص"],
    nav_explorer: ["Data Explorer", "مستكشف البيانات"],
    hero_title: ["Turn Sales Data Into Your Next Big Decision.", "حوِّل بيانات مبيعاتك إلى قرارك الاستراتيجي التالي"],
    hero_subtitle: ["Predict revenue, evaluate trends, and act with confidence.", "تنبّأ بالإيرادات، وحلِّل الاتجاهات، وتصرّف بثقة"],
    hero_desc: ["Understand your sales performance, forecast future revenue, and uncover growth opportunities — all in one intelligent executive platform.", "افهم أداء مبيعاتك، وتنبّأ بإيراداتك المستقبلية، واكتشف فرص النمو، عبر منصة تنفيذية ذكية ومتكاملة."],
    chip_growth: ["Monthly growth", "النمو الشهري"],
    chip_market: ["Leading market", "السوق الأول"],
    chip_orders: ["Orders processed", "الطلبات المعالجة"],
    upload_bubble_title: ["Upload Your Data", "ارفع بياناتك"],
    upload_bubble_subtitle: ["CSV · XLSX · XLS", "CSV · XLSX · XLS"],
    upload_success: ["Data imported successfully ({n:,} rows).", "تم استيراد البيانات بنجاح ({n:,} سجل)."],
    upload_error: ["Could not import file: {e}", "تعذّر استيراد الملف: {e}"],
    err_read: ["Could not read the file.", "تعذّرت قراءة الملف."],
    err_empty: ["The file is empty.", "الملف فارغ."],
    err_type: ["Only CSV, XLSX and XLS files are supported.", "الملفات المدعومة هي CSV وXLSX وXLS فقط."],
    err_nodata: ["The file contains no data.", "لا يحتوي الملف على بيانات."],
    err_norows: ["No valid rows were found in the file.", "لم يتم العثور على سجلات صالحة في الملف."],
    preview_title: ["Sales Overview & Executive Snapshot", "نظرة عامة على المبيعات والمؤشرات التنفيذية"],
    preview_hint: ["Click the chart below to open the full interactive dashboard", "انقر على الرسم البياني أدناه لفتح لوحة المؤشرات التفاعلية الكاملة"],
    preview_badge: ["Demo Preview Mode", "وضع المعاينة التجريبية"],
    load_demo: ["Load Demo Data", "تحميل البيانات التجريبية"],
    demo_dataset: ["Enterprise Demo Data", "بيانات تجريبية مؤسسية"],
    demo_loaded: ["Enterprise demo data loaded successfully.", "تم تحميل البيانات التجريبية بنجاح."],
    total_revenue: ["Total Revenue", "إجمالي الإيرادات"],
    active_period: ["Active reporting period", "الفترة المشمولة بالتقرير"],
    projected_forecast: ["Projected Forecast", "الإيرادات المتوقعة"],
    ai_projection: ["AI-powered projection", "تقدير مدعوم بالذكاء الاصطناعي"],
    top_product_line: ["Top Product Line", "المنتج الأعلى مبيعًا"],
    highest_rev_contribution: ["Highest revenue contribution", "الأعلى مساهمةً في الإيرادات"],
    preview_click_hint: ["→ Click anywhere here to open the full dashboard", "← انقر في أي موضع هنا لفتح لوحة المؤشرات الكاملة"],
    benefit_1_tag: ["Sales Forecasting", "التنبؤ بالمبيعات"],
    benefit_1_title: ["Know What's Coming Next", "استشرف ما هو قادم"],
    benefit_1_desc: ["Predict future sales and revenue using historical performance to support smarter business planning.", "تنبّأ بالمبيعات والإيرادات المستقبلية اعتمادًا على الأداء التاريخي، لدعم تخطيط أعمالك بدقة أكبر."],
    benefit_2_tag: ["Performance Insights", "تحليل الأداء"],
    benefit_2_title: ["Understand What Drives Results", "افهم محرّكات النتائج"],
    benefit_2_desc: ["Identify best-performing product lines, sales trends, and regions needing immediate attention.", "حدِّد المنتجات الأفضل أداءً، واتجاهات المبيعات، والمناطق التي تتطلب اهتمامًا عاجلًا."],
    benefit_3_tag: ["Smarter Decisions", "قرارات أذكى"],
    benefit_3_title: ["Turn Insights Into Action", "حوِّل الرؤى إلى إجراءات"],
    benefit_3_desc: ["Get clear, data-driven recommendations that help you prioritize growth opportunities.", "احصل على توصيات واضحة مبنية على البيانات تساعدك على ترتيب أولويات فرص النمو."],
    steps_title: ["From Sales Data to Business Clarity", "من بيانات المبيعات إلى وضوح الرؤية"],
    steps_subtitle: ["A seamless 3-step decision flow designed for busy sales leaders.", "مسار قرار سلس من ثلاث خطوات، صُمّم لقادة المبيعات المنشغلين."],
    step_1_title: ["Upload Your Data", "ارفع بياناتك"],
    step_1_desc: ["Import your sales spreadsheets directly from Excel or CSV files with zero manual mapping required.", "استورد جداول مبيعاتك مباشرةً من ملفات Excel أو CSV دون الحاجة إلى أي ربط يدوي للأعمدة."],
    step_2_title: ["Discover What Matters", "اكتشف ما يهم"],
    step_2_desc: ["Analyze underlying revenue drivers, product lines, and regional growth patterns.", "حلِّل محرّكات الإيرادات وخطوط المنتجات وأنماط النمو في كل منطقة."],
    step_3_title: ["Make Better Decisions", "اتخذ قرارات أفضل"],
    step_3_desc: ["Leverage automated forecasts and executive recommendations to steer your business strategy.", "استفد من التنبؤات الآلية والتوصيات التنفيذية في توجيه استراتيجية أعمالك."],
    active_context: ["Active Context", "مصدر البيانات الحالي"],
    reset_demo: ["Reset Demo Data", "إعادة البيانات التجريبية"],
    currency: ["Currency", "العملة"],
    total_orders: ["Total Orders", "إجمالي الطلبات"],
    avg_order_value: ["Avg Order Value", "متوسط قيمة الطلب"],
    active_markets: ["Active Markets", "الأسواق النشطة"],
    revenue_over_time: ["Revenue Performance Over Time", "أداء الإيرادات عبر الزمن"],
    executive_takeaway: ["Executive Strategy Takeaway", "الخلاصة الاستراتيجية"],
    top_markets: ["Top Revenue Markets", "الأسواق الأعلى إيرادًا"],
    market_recommendation: ["Market Share Recommendation", "توصية السوق"],
    takeaway_msg: ["Temporal trends indicate healthy momentum peaking in {m}. Seasonal surges provide a clear baseline for inventory planning and team capacity allocation.", "بلغت مبيعاتك ذروتها في {m}. وتمنحك الارتفاعات الموسمية مرجعًا واضحًا لتخطيط المخزون وتوزيع طاقة الفريق."],
    market_msg: ["\"{c}\" remains your highest revenue driver. Consider increasing localized marketing efforts to secure market leadership.", "يظل سوق «{c}» المحرّك الأول لإيراداتك. يُنصح بتعزيز الحملات التسويقية المحلية فيه للحفاظ على الريادة."],
    na: ["N/A", "غير متاح"],
    forecast_page_title: ["Sales Forecasts & Revenue Projections", "التنبؤ بالمبيعات وتوقعات الإيرادات"],
    forecast_horizon: ["Forecast Horizon: {n} Days", "أفق التنبؤ: {n} يومًا"],
    generate_forecast: ["Generate Projections", "إنشاء التوقعات"],
    mae_label: ["Mean Absolute Variance (MAE)", "متوسط الخطأ المطلق (MAE)"],
    rmse_label: ["Root Mean Deviation (RMSE)", "جذر متوسط مربع الخطأ (RMSE)"],
    historical_sales: ["Historical Sales", "المبيعات التاريخية"],
    projected_revenue: ["Projected Revenue", "الإيرادات المتوقعة"],
    forecast_evaluation: ["Forecast Evaluation", "تقييم التنبؤ"],
    forecast_insufficient: ["Insufficient historical data for forecasting.", "البيانات التاريخية غير كافية لإجراء التنبؤ."],
    forecast_msg: ["Projections anticipate consistent revenue performance across the next {h}-day horizon with an estimated variance margin of {mae}.", "تشير التوقعات إلى أداء مستقر نسبيًا للإيرادات خلال الأيام الـ{h} القادمة، بهامش خطأ تقديري يبلغ نحو {mae}."],
    insights_title: ["Executive Insights & Business Opportunities", "رؤى تنفيذية وفر
