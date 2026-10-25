<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NexusSales AI | Executive Sales Intelligence</title>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;0,600;0,700;0,800;1,500;1,600;1,700&family=Outfit:wght@300;400;500;600;700;800&family=IBM+Plex+Sans+Arabic:wght@300;400;500;600;700&family=Amiri:wght@400;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.plot.ly/plotly-2.27.0.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>

<style>
:root{
  --en-body:'Outfit',sans-serif; --en-display:'Playfair Display',Georgia,serif;
  --ar-body:'IBM Plex Sans Arabic','Tajawal',sans-serif; --ar-display:'Amiri','Noto Naskh Arabic',serif;
}
html{font-size:90%;scroll-behavior:smooth;}
body{
  font-family:var(--en-body);background-color:#040711;color:#f8fafc;
  background-image:
    radial-gradient(at 10% 15%,hsla(250,85%,58%,.20) 0,transparent 55%),
    radial-gradient(at 90% 10%,hsla(190,85%,50%,.16) 0,transparent 55%),
    radial-gradient(at 50% 90%,hsla(320,85%,55%,.14) 0,transparent 55%);
  background-attachment:fixed;min-height:100vh;overflow-x:hidden;-webkit-font-smoothing:antialiased;
}
.font-playfair,.hero-display{font-family:var(--en-display);}
body.arabic-mode{font-family:var(--ar-body);}
body.arabic-mode .font-playfair,body.arabic-mode .hero-display{font-family:var(--ar-display);font-weight:700;line-height:1.55!important;}
body.arabic-mode *{letter-spacing:0!important;text-transform:none!important;}
body.arabic-mode .italic{font-style:normal!important;}
body.arabic-mode .leading-relaxed{line-height:2!important;}
body.arabic-mode .insight-box{border-left:none;border-right:4px solid #818cf8;border-radius:12px 0 0 12px;}
.num{direction:ltr;unicode-bidi:isolate;display:inline-block;}
.js-plotly-plot,.q-slider{direction:ltr;}

body::before{
  content:'';position:fixed;inset:0;z-index:-3;pointer-events:none;
  background-image:
    radial-gradient(1.5px 1.5px at 100px 200px,rgba(255,255,255,.95),transparent),
    radial-gradient(1px 1px at 300px 100px,rgba(200,220,255,.8),transparent),
    radial-gradient(2px 2px at 500px 300px,rgba(255,255,255,.9),transparent),
    radial-gradient(1px 1px at 700px 150px,rgba(255,255,255,.7),transparent),
    radial-gradient(1.5px 1.5px at 200px 500px,rgba(180,200,255,.85),transparent),
    radial-gradient(1px 1px at 800px 400px,rgba(255,255,255,.75),transparent),
    radial-gradient(2px 2px at 450px 600px,rgba(255,255,255,.6),transparent),
    radial-gradient(1px 1px at 650px 700px,rgba(255,255,255,.8),transparent);
  background-size:1300px 900px;background-repeat:repeat;animation:nexusTwinkle 8s ease-in-out infinite alternate;
}
body::after{
  content:'';position:fixed;inset:0;z-index:-1;pointer-events:none;
  background-image:linear-gradient(rgba(99,102,241,.06) 1px,transparent 1px),linear-gradient(90deg,rgba(99,102,241,.06) 1px,transparent 1px);
  background-size:55px 55px;
  mask-image:radial-gradient(ellipse at 50% 45%,black 15%,transparent 75%);
  -webkit-mask-image:radial-gradient(ellipse at 50% 45%,black 15%,transparent 75%);
}
@keyframes nexusTwinkle{0%{opacity:.35}100%{opacity:1}}
#fx-canvas{position:fixed;inset:0;z-index:-2;pointer-events:none;opacity:.85;}
.orb{position:fixed;border-radius:50%;filter:blur(90px);pointer-events:none;z-index:-2;opacity:.5;}
.orb-1{width:420px;height:420px;top:-120px;left:-100px;background:#4f46e5;animation:orbA 22s ease-in-out infinite alternate;}
.orb-2{width:360px;height:360px;top:30%;right:-120px;background:#0891b2;animation:orbB 26s ease-in-out infinite alternate;}
.orb-3{width:380px;height:380px;bottom:-140px;left:30%;background:#be185d;opacity:.35;animation:orbA 30s ease-in-out infinite alternate-reverse;}
@keyframes orbA{to{transform:translate(120px,90px) scale(1.15);}}
@keyframes orbB{to{transform:translate(-110px,-80px) scale(1.2);}}

.reveal{animation:revealUp .85s cubic-bezier(.2,.7,.2,1) var(--d,0s) backwards;}
@keyframes revealUp{from{opacity:0;transform:translateY(26px);}to{opacity:1;transform:none;}}
@keyframes floatY{0%,100%{transform:translateY(0);}50%{transform:translateY(-10px);}}
@keyframes spin{to{transform:rotate(360deg);}}
@keyframes spinRev{to{transform:rotate(-360deg);}}
@keyframes gradShift{to{background-position:200% center;}}
@keyframes pulseDot{0%,100%{box-shadow:0 0 0 0 rgba(52,211,153,.6);}70%{box-shadow:0 0 0 9px rgba(52,211,153,0);}}

.glass-nav{background:rgba(4,7,17,.82);backdrop-filter:blur(22px);border-bottom:1px solid rgba(255,255,255,.08);}
.brand-tile{width:40px;height:40px;border-radius:12px;background:linear-gradient(135deg,#6366f1,#a855f7 55%,#ec4899);display:flex;align-items:center;justify-content:center;box-shadow:0 8px 22px -6px rgba(139,92,246,.8);}
.nav-link{color:#cbd5e1;font-weight:600;font-size:.88rem;padding:.45rem .9rem;border-radius:999px;cursor:pointer;transition:all .25s ease;white-space:nowrap;}
.nav-link:hover{color:#fff;background:rgba(129,140,248,.14);}
.nav-link.active{color:#fff;background:linear-gradient(135deg,rgba(99,102,241,.38),rgba(168,85,247,.26));box-shadow:inset 0 0 0 1px rgba(129,140,248,.5);}
.lang-toggle{border:1px solid rgba(255,255,255,.16);border-radius:999px;overflow:hidden;}
.lang-toggle button{padding:.4rem .9rem;font-weight:600;font-size:.8rem;color:#cbd5e1;background:transparent;transition:all .2s;}
.lang-toggle button.active{background:#6366f1;color:#fff;}

.glass-card{background:rgba(18,26,45,.7);backdrop-filter:blur(16px);border:1px solid rgba(255,255,255,.08);transition:all .35s cubic-bezier(.4,0,.2,1);color:#fff;}
.glass-card:hover{transform:translateY(-4px);box-shadow:0 25px 45px -12px rgba(99,102,241,.35);border-color:rgba(99,102,241,.5);}
.hero-text{background:linear-gradient(110deg,#818cf8 0,#22d3ee 25%,#c084fc 50%,#f472b6 75%,#818cf8 100%);background-size:200% auto;-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;animation:gradShift 7s linear infinite;}
.insight-box{background:linear-gradient(135deg,rgba(99,102,241,.12) 0,rgba(192,132,252,.05) 100%);border-left:4px solid #818cf8;border-radius:0 12px 12px 0;}
.glow-line{height:2px;width:100%;background:linear-gradient(90deg,transparent,#818cf8,#22d3ee,#c084fc,transparent);background-size:200% 100%;animation:glowMove 5s linear infinite;opacity:.8;}
@keyframes glowMove{to{background-position:200% 0;}}
.live-dot{width:9px;height:9px;border-radius:50%;background:#34d399;display:inline-block;animation:pulseDot 2s infinite;}
.kpi-icon{width:44px;height:44px;border-radius:14px;display:flex;align-items:center;justify-content:center;background:rgba(99,102,241,.14);border:1px solid rgba(129,140,248,.25);}

.hero-visual{position:relative;width:100%;max-width:760px;margin:6px auto 0;}
.hero-visual svg{width:100%;height:auto;display:block;filter:drop-shadow(0 30px 60px rgba(79,70,229,.35));}
.vbar{transform-box:fill-box;transform-origin:50% 100%;animation:barGrow 1.1s cubic-bezier(.2,.8,.2,1) backwards;}
@keyframes barGrow{from{transform:scaleY(0);opacity:0;}to{transform:scaleY(1);opacity:1;}}
.draw-line{stroke-dasharray:1;stroke-dashoffset:0;animation:drawLine 2.6s ease-out .5s backwards;}
@keyframes drawLine{from{stroke-dashoffset:1;}to{stroke-dashoffset:0;}}
.fade-in{animation:fadeIn 1.6s ease 1.4s backwards;}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
.donut-seg{transform-box:fill-box;transform-origin:center;animation:donutIn 1.6s ease-out .6s backwards;}
@keyframes donutIn{from{opacity:0;transform:rotate(-120deg);}to{opacity:1;transform:none;}}
.float-chip{position:absolute;display:flex;align-items:center;gap:10px;padding:10px 16px 10px 10px;border-radius:16px;z-index:3;background:rgba(15,23,42,.8);backdrop-filter:blur(14px);border:1px solid rgba(255,255,255,.13);box-shadow:0 18px 40px -14px rgba(0,0,0,.75);animation:floatY 5.5s ease-in-out infinite;text-align:start;}
.chip-1{top:6%;left:-2%;animation-delay:0s;}
.chip-2{top:-3%;right:0%;animation-delay:-1.8s;}
.chip-3{bottom:4%;right:-3%;animation-delay:-3.2s;}
.chip-icon{width:34px;height:34px;border-radius:11px;display:flex;align-items:center;justify-content:center;}
.chip-label{font-size:10.5px;color:#94a3b8;font-weight:600;line-height:1.2;}
.chip-value{font-size:15px;color:#fff;font-weight:800;line-height:1.3;max-width:130px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}

.hidden-uploader{position:absolute!important;width:1px!important;height:1px!important;opacity:0!important;pointer-events:none!important;overflow:hidden!important;z-index:-1!important;}
.upload-bubble-wrap{display:flex;flex-direction:column;align-items:center;gap:14px;padding:28px 0 18px;position:relative;}
.upload-bubble-wrap::before{content:'';position:absolute;width:260px;height:260px;top:-8px;left:50%;transform:translateX(-50%);border-radius:50%;background:radial-gradient(circle,rgba(168,85,247,.30) 0,transparent 70%);pointer-events:none;animation:bubblePulse 3.4s ease-in-out infinite;}
@keyframes bubblePulse{0%,100%{transform:translateX(-50%) scale(1);opacity:.6;}50%{transform:translateX(-50%) scale(1.18);opacity:.95;}}
.ring{position:absolute;left:50%;top:28px;border-radius:50%;pointer-events:none;}
.ring-1{width:252px;height:252px;margin:-31px 0 0 -126px;border:1px dashed rgba(165,180,252,.5);animation:spin 22s linear infinite;}
.ring-2{width:312px;height:312px;margin:-61px 0 0 -156px;border:1px dotted rgba(34,211,238,.45);animation:spinRev 34s linear infinite;}
.ring-1::after,.ring-2::after{content:'';position:absolute;top:-4px;left:50%;width:8px;height:8px;border-radius:50%;background:#22d3ee;box-shadow:0 0 14px 3px rgba(34,211,238,.8);}
.ring-2::after{background:#f472b6;box-shadow:0 0 14px 3px rgba(244,114,182,.8);}
.upload-bubble{width:190px;height:190px;border-radius:50%;background:radial-gradient(circle at 30% 30%,#a5b4fc 0,#6366f1 40%,#7c3aed 70%,#be185d 100%);display:flex;align-items:center;justify-content:center;cursor:pointer;box-shadow:0 28px 60px -12px rgba(139,92,246,.7),inset 0 -10px 22px rgba(0,0,0,.28),inset 0 8px 20px rgba(255,255,255,.28);transition:all .35s cubic-bezier(.4,0,.2,1);animation:bubbleFloat 4s ease-in-out infinite;position:relative;z-index:1;}
.upload-bubble:hover{transform:scale(1.08);box-shadow:0 35px 80px -12px rgba(139,92,246,.95),inset 0 -10px 22px rgba(0,0,0,.28),inset 0 8px 20px rgba(255,255,255,.35);}
.upload-bubble:active{transform:scale(.97);}
@keyframes bubbleFloat{0%,100%{transform:translateY(0) rotate(0);}50%{transform:translateY(-10px) rotate(2deg);}}
.upload-bubble-label{font-size:16px;font-weight:700;color:#e0e7ff;text-align:center;z-index:1;}
.upload-bubble-sub{font-size:11px;color:#94a3b8;letter-spacing:.8px;font-weight:600;z-index:1;}

.feature-card{background:rgba(18,26,45,.7);backdrop-filter:blur(16px);border:1px solid rgba(255,255,255,.08);border-radius:24px;position:relative;overflow:hidden;transition:all .35s cubic-bezier(.4,0,.2,1);}
.feature-card::after{content:'';position:absolute;inset:0;border-radius:inherit;pointer-events:none;background:radial-gradient(500px circle at 20% -10%,rgba(129,140,248,.14),transparent 60%);}
.feature-card:hover{transform:translateY(-6px) scale(1.015);border-color:rgba(129,140,248,.55);box-shadow:0 25px 50px -12px rgba(99,102,241,.4);}
.feature-num{position:absolute;top:-22px;right:-8px;font-size:6.5rem;font-weight:800;line-height:1;color:rgba(129,140,248,.08);font-family:var(--en-display);pointer-events:none;}
.feature-icon{width:58px;height:58px;border-radius:18px;display:flex;align-items:center;justify-content:center;box-shadow:0 10px 24px -6px rgba(99,102,241,.45);margin-bottom:16px;}
.step-card{background:rgba(18,26,45,.65);border:1px solid rgba(255,255,255,.08);border-radius:22px;position:relative;overflow:hidden;transition:all .35s ease;}
.step-card:hover{border-color:rgba(129,140,248,.5);transform:translateY(-4px);}
.step-num-badge{width:44px;height:44px;border-radius:12px;background:linear-gradient(135deg,#4338ca,#7c3aed);display:flex;align-items:center;justify-content:center;font-weight:800;color:#fff;box-shadow:0 8px 18px -4px rgba(99,102,241,.5);}
.rec-num{width:36px;height:36px;flex:none;border-radius:11px;display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg,rgba(99,102,241,.35),rgba(168,85,247,.25));border:1px solid rgba(129,140,248,.4);}
.ripple-btn{position:relative;overflow:hidden;transition:transform .3s cubic-bezier(.25,.8,.25,1),box-shadow .3s;}
.ripple-btn:hover{transform:translateY(-2px);box-shadow:0 18px 40px rgba(99,102,241,.55);}

#toast-container{position:fixed;top:80px;left:50%;transform:translateX(-50%);z-index:9999;display:flex;flex-direction:column;gap:8px;}
.toast{padding:.75rem 1.25rem;border-radius:.75rem;background:#1e293b;color:#fff;border:1px solid #334155;box-shadow:0 10px 30px -10px rgba(0,0,0,.6);animation:slideDown .3s ease;max-width:min(90vw,500px);}
.toast.positive{border-color:#10b981;background:#064e3b;}
.toast.negative{border-color:#ef4444;background:#7f1d1d;}
.toast.warning{border-color:#f59e0b;background:#78350f;}
@keyframes slideDown{from{transform:translateY(-20px);opacity:0;}to{transform:none;opacity:1;}}

.spinner{width:48px;height:48px;border:4px solid rgba(129,140,248,.2);border-top-color:#6366f1;border-radius:50%;animation:spin 1s linear infinite;margin:2.5rem auto;}

.chat-fab{position:fixed;bottom:25px;right:25px;width:64px;height:64px;border-radius:50%;background:#6366f1;color:#fff;border:none;cursor:pointer;box-shadow:0 15px 35px -10px rgba(99,102,241,.8);z-index:9998;display:flex;align-items:center;justify-content:center;transition:all .3s;}
.chat-fab:hover{transform:scale(1.1);}
.chat-dialog{position:fixed;bottom:100px;right:25px;width:420px;height:580px;max-width:calc(100vw - 50px);max-height:calc(100vh - 120px);background:#0f172a;border:1px solid #334155;border-radius:1rem;display:none;flex-direction:column;overflow:hidden;z-index:9997;box-shadow:0 25px 60px -15px rgba(0,0,0,.8);}
.chat-dialog.open{display:flex;}
.chat-message-row{display:flex;margin-bottom:.5rem;width:100%;}
.chat-message-row.user{justify-content:flex-end;}
.chat-message{padding:.6rem 1rem;border-radius:1rem;max-width:80%;font-size:.9rem;line-height:1.4;}
.chat-message.user{background:#6366f1;color:#fff;border-bottom-right-radius:.25rem;}
.chat-message.ai{background:#1e293b;color:#e2e8f0;border-bottom-left-radius:.25rem;}
.chat-message-name{font-size:.7rem;font-weight:700;color:#94a3b8;margin-bottom:.2rem;}

.data-table{width:100%;border-collapse:collapse;font-size:.85rem;}
.data-table th{background:#1e293b;color:#e2e8f0;padding:.6rem;text-align:start;position:sticky;top:0;border-bottom:1px solid #334155;}
.data-table td{padding:.5rem .6rem;border-bottom:1px solid #1e293b;color:#cbd5e1;}
.data-table tr:hover td{background:#1e293b;}

@media (max-width:900px){
  html{font-size:100%;}
  .glass-nav{padding-left:10px!important;padding-right:10px!important;}
  .nav-links{overflow-x:auto;max-width:100%;flex-wrap:nowrap!important;}
  .upload-bubble{width:155px;height:155px;}
  .ring{display:none;}
  .float-chip{transform:scale(.8);}
  .chip-3{display:none;}
}
@media (prefers-reduced-motion:reduce){*,*::before,*::after{animation-duration:.01ms!important;animation-iteration-count:1!important;}}
</style>
</head>
<body>

<header class="glass-nav py-3 px-6 lg:px-10 flex justify-between items-center sticky top-0 z-50 gap-3 flex-wrap">
  <div class="items-center gap-3 cursor-pointer flex flex-nowrap" onclick="setPage('home')">
    <div class="brand-tile"><i class="material-icons text-white" style="font-size:1.4rem">insights</i></div>
    <span class="text-xl font-playfair font-bold text-white tracking-tight">NexusSales AI</span>
  </div>
  <nav id="nav-links" class="gap-1 items-center flex flex-nowrap nav-links"></nav>
  <div class="lang-toggle">
    <button data-lang="en" onclick="changeLanguage('en')">EN</button>
    <button data-lang="ar" onclick="changeLanguage('ar')">عربي</button>
  </div>
</header>

<div id="content-container" class="w-full pb-12"></div>

<footer id="main-footer" class="w-full justify-center py-8 mt-12 border-t border-gray-800 text-gray-400 text-sm items-center gap-1 flex"></footer>

<div id="toast-container"></div>

<input type="file" id="file-uploader" accept=".csv,.xlsx,.xls" style="display:none">

<button class="chat-fab" onclick="toggleChat()">
  <i class="material-icons" style="font-size:1.8rem">psychology</i>
</button>

<div id="chat-dialog" class="chat-dialog">
  <div class="w-full bg-gradient-to-r from-indigo-600 to-purple-600 p-4 text-white flex items-center gap-3">
    <i class="material-icons" style="font-size:2rem">psychology</i>
    <div class="flex flex-col">
      <span id="chat-title" class="text-base font-bold leading-tight">Executive Sales Assistant</span>
      <span id="chat-subtitle" class="text-xs text-indigo-100">Powered by NexusSales AI</span>
    </div>
    <button onclick="toggleChat()" class="ms-auto text-white/80 hover:text-white"><i class="material-icons">close</i></button>
  </div>
  <div id="chat-messages" class="flex-1 w-full overflow-y-auto p-4 bg-gray-900"></div>
  <div class="w-full p-3 bg-gray-800 flex items-center gap-2 border-t border-gray-700">
    <input id="chat-input" type="text" placeholder="Ask about sales metrics..."
           class="flex-1 px-3 py-2 rounded-lg bg-gray-900 text-white border border-gray-700 focus:outline-none focus:border-indigo-500"
           onkeydown="if(event.key==='Enter')sendChat()">
    <button onclick="sendChat()" class="w-10 h-10 rounded-full bg-indigo-600 text-white flex items-center justify-center hover:bg-indigo-500">
      <i class="material-icons" style="font-size:1.3rem">send</i>
    </button>
  </div>
</div>

<script>
/* =========================================================
   1. CURRENCY & TRANSLATIONS
   ========================================================= */
const CURRENCY_SYMBOLS = {'USD':'$','GBP':'£','EUR':'€','SAR':'﷼','AED':'د.إ','EGP':'E£','JPY':'¥','INR':'₹','KWD':'KD','QAR':'QR','BHD':'BD','OMR':'RO'};
const CURRENCY_PATTERNS = [
  ['$','USD'],['£','GBP'],['€','EUR'],['ر.س','SAR'],['د.إ','AED'],['﷼','SAR'],
  ['E£','EGP'],['¥','JPY'],['₹','INR'],['AED','AED'],['SAR','SAR'],['USD','USD'],['GBP','GBP']
];

const TEXT = {
  app_name:["NexusSales AI","NexusSales AI"],
  app_subtitle:["Executive Sales Intelligence & Decision Engine","منصة الذكاء التنفيذي للمبيعات ودعم القرار"],
  nav_home:["Home","الرئيسية"], nav_dashboard:["Dashboard","لوحة المؤشرات"],
  nav_forecasts:["Forecasts","التنبؤات"], nav_insights:["Insights","الرؤى التحليلية"],
  nav_summary:["Summary","الملخص"], nav_explorer:["Data Explorer","مستكشف البيانات"],
  language:["Language","اللغة"],
  hero_title:["Turn Sales Data Into Your Next Big Decision.","حوِّل بيانات مبيعاتك إلى قرارك الاستراتيجي التالي"],
  hero_subtitle:["Predict revenue, evaluate trends, and act with confidence.","تنبّأ بالإيرادات، وحلِّل الاتجاهات، وتصرّف بثقة"],
  hero_desc:["Understand your sales performance, forecast future revenue, and uncover growth opportunities — all in one intelligent executive platform.","افهم أداء مبيعاتك، وتنبّأ بإيراداتك المستقبلية، واكتشف فرص النمو، عبر منصة تنفيذية ذكية ومتكاملة."],
  chip_growth:["Monthly growth","النمو الشهري"], chip_market:["Leading market","السوق الأول"],
  chip_orders:["Orders processed","الطلبات المعالجة"],
  upload_bubble_title:["Upload Your Data","ارفع بياناتك"],
  upload_bubble_subtitle:["CSV · XLSX · XLS","CSV · XLSX · XLS"],
  upload_success:["Data imported successfully ({n} rows).","تم استيراد البيانات بنجاح ({n} سجل)."],
  upload_error:["Could not import file: {e}","تعذّر استيراد الملف: {e}"],
  err_read:["Could not read the file.","تعذّرت قراءة الملف."],
  err_empty:["The file is empty.","الملف فارغ."],
  err_type:["Only CSV, XLSX and XLS files are supported.","الملفات المدعومة هي CSV وXLSX وXLS فقط."],
  err_nodata:["The file contains no data.","لا يحتوي الملف على بيانات."],
  err_norows:["No valid rows were found in the file.","لم يتم العثور على سجلات صالحة في الملف."],
  preview_title:["Sales Overview & Executive Snapshot","نظرة عامة على المبيعات والمؤشرات التنفيذية"],
  preview_hint:["Click the chart below to open the full interactive dashboard","انقر على الرسم البياني أدناه لفتح لوحة المؤشرات التفاعلية الكاملة"],
  preview_badge:["Demo Preview Mode","وضع المعاينة التجريبية"],
  load_demo:["Load Demo Data","تحميل البيانات التجريبية"],
  demo_dataset:["Enterprise Demo Data","بيانات تجريبية مؤسسية"],
  demo_loaded:["Enterprise demo data loaded successfully.","تم تحميل البيانات التجريبية بنجاح."],
  total_revenue:["Total Revenue","إجمالي الإيرادات"],
  active_period:["Active reporting period","الفترة المشمولة بالتقرير"],
  projected_forecast:["Projected Forecast","الإيرادات المتوقعة"],
  ai_projection:["AI-powered projection","تقدير مدعوم بالذكاء الاصطناعي"],
  top_product_line:["Top Product Line","المنتج الأعلى مبيعًا"],
  highest_rev_contribution:["Highest revenue contribution","الأعلى مساهمةً في الإيرادات"],
  preview_click_hint:["→ Click anywhere here to open the full dashboard","← انقر في أي موضع هنا لفتح لوحة المؤشرات الكاملة"],
  benefit_1_tag:["Sales Forecasting","التنبؤ بالمبيعات"], benefit_1_title:["Know What's Coming Next","استشرف ما هو قادم"],
  benefit_1_desc:["Predict future sales and revenue using historical performance to support smarter business planning.","تنبّأ بالمبيعات والإيرادات المستقبلية اعتمادًا على الأداء التاريخي، لدعم تخطيط أعمالك بدقة أكبر."],
  benefit_2_tag:["Performance Insights","تحليل الأداء"], benefit_2_title:["Understand What Drives Results","افهم محرّكات النتائج"],
  benefit_2_desc:["Identify best-performing product lines, sales trends, and regions needing immediate attention.","حدِّد المنتجات الأفضل أداءً، واتجاهات المبيعات، والمناطق التي تتطلب اهتمامًا عاجلًا."],
  benefit_3_tag:["Smarter Decisions","قرارات أذكى"], benefit_3_title:["Turn Insights Into Action","حوِّل الرؤى إلى إجراءات"],
  benefit_3_desc:["Get clear, data-driven recommendations that help you prioritize growth opportunities.","احصل على توصيات واضحة مبنية على البيانات تساعدك على ترتيب أولويات فرص النمو."],
  steps_title:["From Sales Data to Business Clarity","من بيانات المبيعات إلى وضوح الرؤية"],
  steps_subtitle:["A seamless 3-step decision flow designed for busy sales leaders.","مسار قرار سلس من ثلاث خطوات، صُمّم لقادة المبيعات المنشغلين."],
  step_1_title:["Upload Your Data","ارفع بياناتك"], step_1_desc:["Import your sales spreadsheets directly from Excel or CSV files with zero manual mapping required.","استورد جداول مبيعاتك مباشرةً من ملفات Excel أو CSV دون الحاجة إلى أي ربط يدوي للأعمدة."],
  step_2_title:["Discover What Matters","اكتشف ما يهم"], step_2_desc:["Analyze underlying revenue drivers, product lines, and regional growth patterns.","حلِّل محرّكات الإيرادات وخطوط المنتجات وأنماط النمو في كل منطقة."],
  step_3_title:["Make Better Decisions","اتخذ قرارات أفضل"], step_3_desc:["Leverage automated forecasts and executive recommendations to steer your business strategy.","استفد من التنبؤات الآلية والتوصيات التنفيذية في توجيه استراتيجية أعمالك."],
  active_context:["Active Context","مصدر البيانات الحالي"], reset_demo:["Reset Demo Data","إعادة البيانات التجريبية"],
  currency:["Currency","العملة"], total_orders:["Total Orders","إجمالي الطلبات"],
  avg_order_value:["Avg Order Value","متوسط قيمة الطلب"], active_markets:["Active Markets","الأسواق النشطة"],
  revenue_over_time:["Revenue Performance Over Time","أداء الإيرادات عبر الزمن"],
  executive_takeaway:["Executive Strategy Takeaway","الخلاصة الاستراتيجية"],
  top_markets:["Top Revenue Markets","الأسواق الأعلى إيرادًا"],
  market_recommendation:["Market Share Recommendation","توصية السوق"],
  takeaway_msg:["Temporal trends indicate healthy momentum peaking in {m}. Seasonal surges provide a clear baseline for inventory planning and team capacity allocation.","بلغت مبيعاتك ذروتها في {m}. وتمنحك الارتفاعات الموسمية مرجعًا واضحًا لتخطيط المخزون وتوزيع طاقة الفريق."],
  market_msg:["\"{c}\" remains your highest revenue driver. Consider increasing localized marketing efforts to secure market leadership.","يظل سوق «{c}» المحرّك الأول لإيراداتك. يُنصح بتعزيز الحملات التسويقية المحلية فيه للحفاظ على الريادة."],
  na:["N/A","غير متاح"],
  forecast_page_title:["Sales Forecasts & Revenue Projections","التنبؤ بالمبيعات وتوقعات الإيرادات"],
  forecast_horizon:["Forecast Horizon: {n} Days","أفق التنبؤ: {n} يومًا"],
  generate_forecast:["Generate Projections","إنشاء التوقعات"],
  mae_label:["Mean Absolute Variance (MAE)","متوسط الخطأ المطلق (MAE)"],
  rmse_label:["Root Mean Deviation (RMSE)","جذر متوسط مربع الخطأ (RMSE)"],
  historical_sales:["Historical Sales","المبيعات التاريخية"],
  projected_revenue:["Projected Revenue","الإيرادات المتوقعة"],
  forecast_evaluation:["Forecast Evaluation","تقييم التنبؤ"],
  forecast_insufficient:["Insufficient historical data for forecasting.","البيانات التاريخية غير كافية لإجراء التنبؤ."],
  forecast_msg:["Projections anticipate consistent revenue performance across the next {h}-day horizon with an estimated variance margin of {mae}.","تشير التوقعات إلى أداء مستقر نسبيًا للإيرادات خلال الأيام الـ{h} القادمة، بهامش خطأ تقديري يبلغ نحو {mae}."],
  insights_title:["Executive Insights & Business Opportunities","رؤى تنفيذية وفرص أعمال"],
  primary_driver:["Primary Revenue Driver","المحرّك الرئيس للإيرادات"],
  core_market:["Core Regional Market","السوق الإقليمي الأساسي"],
  strategic_plan:["Strategic Growth Plan","خطة النمو الاستراتيجية"],
  share_msg:["Generates {amt} ({pct} of total sales)","يسهم بمبلغ {amt} ({pct} من إجمالي المبيعات)"],
  rec_1:["Scale inventory for key product line '{name}' to eliminate potential stockout risks.","عزِّز مخزون المنتج «{name}» للحدّ من مخاطر نفاد الكميات."],
  rec_2:["Expand customer acquisition in top market '{name}' where response rates are highest.","وسِّع نطاق استقطاب العملاء في سوق «{name}» حيث تسجّل معدلات الاستجابة أعلى مستوياتها."],
  rec_3:["Utilize predictive forecasting prior to quarterly planning cycles to optimize cash flow.","استعن بالتنبؤات قبل دورات التخطيط الربع سنوية لتحسين التدفقات النقدية."],
  rec_4:["Monitor average order value metrics to identify cross-selling opportunities.","تابع مؤشر متوسط قيمة الطلب لاكتشاف فرص البيع المتقاطع."],
  summary_title:["Sales Summary","ملخص المبيعات"], orders_label:["Orders","الطلبات"],
  key_findings:["Key Findings","أبرز النتائج"], reporting_period:["Reporting period","فترة التقرير"],
  top_product_by_rev:["Top product by revenue","المنتج الأعلى إيرادًا"],
  top_market_by_rev:["Top market by revenue","السوق الأعلى إيرادًا"],
  rev_by_product:["Revenue by Product","الإيرادات حسب المنتج"],
  rev_by_market:["Revenue by Market","الإيرادات حسب السوق"],
  explorer_title:["Data Explorer & Inspection","مستكشف البيانات وفحصها"],
  explorer_subtitle:["Cleaned & Normalized Sales Dataset View","عرض مجموعة بيانات المبيعات بعد التنظيف والمعالجة"],
  chat_title:["Executive Sales Assistant","المساعد التنفيذي للمبيعات"],
  chat_subtitle:["Powered by NexusSales AI","مدعوم من NexusSales AI"],
  chat_welcome:["Hello! Ask me about revenue, products, or markets.","مرحبًا! يمكنك سؤالي عن الإيرادات أو المنتجات أو الأسواق."],
  chat_placeholder:["Ask about sales metrics...","اكتب سؤالك عن مؤشرات المبيعات..."],
  chat_user:["You","أنت"], chat_ai:["AI Assistant","المساعد الذكي"],
  bot_no_data:["Please upload or select a sales dataset first.","يرجى رفع ملف بيانات أو تحميل البيانات التجريبية أولًا."],
  bot_revenue:["Total registered revenue is {amt} across {orders} orders.","بلغ إجمالي إيراداتك {amt} من {orders} طلب."],
  bot_product:["Top revenue-generating product is '{name}'.","المنتج الأعلى إيرادًا هو «{name}»."],
  bot_market:["Top-performing market is '{name}'.","السوق الأعلى أداءً هو «{name}»."],
  bot_default:["Active dataset total: {amt}. Ask about revenue, top products, or markets.","إجمالي الإيرادات في البيانات الحالية: {amt}. يمكنك سؤالي عن الإيرادات أو أفضل المنتجات أو الأسواق."],
  footer:["Created by SHODO","تطوير SHODO"],
  language_updated:["Language updated","تم تغيير اللغة"]
};

/* =========================================================
   2. STATE
   ========================================================= */
const state = {
  df: [], page:'home', datasetMode:null,
  currencyCode:'USD', currencySymbol:'$',
  language:'en', chatHistory:[]
};

const EN_FONT = "Outfit, sans-serif";
const AR_FONT = "IBM Plex Sans Arabic, Tajawal, sans-serif";

function t(key, params){ const p = TEXT[key] || [key,key]; let s = p[state.language==='en'?0:1]; if(params) for(const k in params) s = s.replace(new RegExp('\\{'+k+'\\}','g'), params[k]); return s; }
function iso(x){ const s = String(x); return state.language==='ar' ? '\u2068'+s+'\u2069' : s; }
function money(v, dec=2){ const n = Number(v)||0; return state.currencySymbol + n.toLocaleString('en-US',{minimumFractionDigits:dec,maximumFractionDigits:dec}); }
function fmtDate(d){ if(!(d instanceof Date)) return String(d||''); return state.language==='ar' ? d.toLocaleDateString('en-GB') : d.toLocaleDateString('en-GB',{day:'2-digit',month:'short',year:'numeric'}); }
function datasetLabel(){ return state.datasetMode==null ? t('demo_dataset') : state.datasetMode; }
function notify(msg, type='positive', timeout=3500){
  const c = document.getElementById('toast-container');
  const el = document.createElement('div');
  el.className = 'toast '+type; el.textContent = msg; c.appendChild(el);
  setTimeout(()=>el.remove(), timeout);
}

/* =========================================================
   3. DATA HELPERS
   ========================================================= */
function parseDate(val){
  if(val==null || val==='') return null;
  if(val instanceof Date) return isNaN(val)?null:val;
  const s = String(val).trim(); if(!s) return null;
  let m = s.match(/^(\d{4})[\/\-](\d{1,2})[\/\-](\d{1,2})/);
  if(m){ const d = new Date(+m[1], +m[2]-1, +m[3]); return isNaN(d)?null:d; }
  m = s.match(/^(\d{1,2})[\/\-\.](\d{1,2})[\/\-\.](\d{2,4})/);
  if(m){ let y=+m[3]; if(y<100)y+=2000; const d = new Date(y, +m[2]-1, +m[1]); return isNaN(d)?null:d; }
  const d = new Date(s); return isNaN(d)?null:d;
}
function toNum(v){
  if(v==null) return NaN;
  const n = parseFloat(String(v).replace(/[^\d.\-]/g,''));
  return isNaN(n)?NaN:n;
}
function isGarbageCode(text){
  const s = String(text).trim();
  return s.length>8 && /^[0-9a-zA-Z]{10,}$/.test(s) && !/\s/.test(s);
}
function dedupeColumns(cols){
  const seen = {}; return cols.map(c=>{
    let s = c!=null ? String(c).trim() : '';
    if(!s || /^(nan|none|unnamed|__empty)$/i.test(s)) s = 'Column';
    if(seen[s]!=null){ seen[s]++; return s+'_'+seen[s]; }
    seen[s]=0; return s;
  });
}
function detectCurrency(rows){
  if(!rows.length) return 'USD';
  const cols = Object.keys(rows[0]);
  for(const col of cols){
    const vals = rows.slice(0,200).map(r=>r[col]).filter(v=>v!=null&&v!=='');
    if(!vals.length) continue;
    const text = vals.map(v=>String(v)).join(' ').toUpperCase();
    for(const [sym,code] of CURRENCY_PATTERNS){ if(text.includes(sym.toUpperCase())) return code; }
  }
  return 'USD';
}

function cleanData(rawRows){
  if(!rawRows || !rawRows.length) return [];
  // Detect header row when first row has many empty cells
  let rows = rawRows.slice();
  const cols0 = Object.keys(rows[0]||{});
  const emptyCount = cols0.filter(c=>/^(Column|__EMPTY|Unnamed)/i.test(c)).length;
  if(emptyCount > cols0.length/2 && rows.length > 1){
    rows = rows.slice(1);
  }
  // Dedupe columns
  const oldCols = Object.keys(rows[0]||{});
  const newCols = dedupeColumns(oldCols);
  const colMap0 = {}; oldCols.forEach((c,i)=>colMap0[c]=newCols[i]);
  rows = rows.map(r=>{ const o={}; oldCols.forEach(c=>{o[colMap0[c]] = r[c];}); return o; });

  // Keyword mapping
  const renameMap = {};
  const assigned = new Set();
  for(const col of Object.keys(rows[0]||{})){
    const l = String(col).toLowerCase(); let target = null;
    if(/(date|time|day|invoice_date|التاريخ|الوقت)/.test(l)) target='InvoiceDate';
    else if(/(qty|quantity|units|الكمية|العدد|count)/.test(l)) target='Quantity';
    else if(/(price|unit_price|rate|cost|السعر|التكلفة|amount)/.test(l)) target='UnitPrice';
    else if(/(product|item|desc|title|name|المنتج|الصنف)/.test(l) && !isGarbageCode(col)) target='Description';
    else if(/(country|region|market|location|state|الدولة|المنطقة)/.test(l)) target='Country';
    else if(/(invoice|order_id|bill|code|رقم)/.test(l)) target='InvoiceNo';
    else if(/(revenue|sales|total_sales|total|الإيرادات|المبيعات)/.test(l)) target='DirectRevenue';
    if(target && !assigned.has(target)){ renameMap[col] = target; assigned.add(target); }
  }
  rows = rows.map(r=>{ const o={}; for(const k in r){ o[renameMap[k]||k] = r[k]; } return o; });

  // Process
  const out = [];
  for(const r of rows){
    const dt = parseDate(r.InvoiceDate);
    const qty = r.Quantity!=null ? (toNum(r.Quantity)||1) : 1;
    const price = r.UnitPrice!=null ? (toNum(r.UnitPrice)||100) : 100;
    const directRev = r.DirectRevenue!=null ? toNum(r.DirectRevenue) : NaN;
    const revenue = !isNaN(directRev) && directRev>0 ? directRev : qty*price;
    let desc = r.Description!=null ? String(r.Description).trim() : '';
    if(desc && isGarbageCode(desc)) continue;
    if(!desc || desc==='nan' || desc==='None') desc = 'Standard Product Line';
    const country = r.Country!=null && String(r.Country).trim() ? String(r.Country).trim() : 'Global Market';
    const invoiceNo = r.InvoiceNo!=null && String(r.InvoiceNo).trim() ? String(r.InvoiceNo).trim() : 'INV-'+Math.floor(Math.random()*1e6);
    out.push({
      InvoiceDate: dt || new Date(),
      Date: dt ? new Date(dt.getFullYear(),dt.getMonth(),dt.getDate()) : new Date(),
      Description: desc,
      Quantity: qty,
      UnitPrice: price,
      Revenue: revenue,
      Country: country,
      InvoiceNo: invoiceNo
    });
  }
  return out;
}

/* =========================================================
   4. DEMO DATA
   ========================================================= */
function getDemoData(){
  const products = [
    ['Enterprise Suite Pro',2500],['Cloud Analytics License',1200],
    ['Hardware Gateway X',450],['Consulting Package',1800],
    ['AI Add-On Module',650],['Premium Maintenance',200]
  ];
  const countries = [
    ['United States',0.38],['United Kingdom',0.22],['Germany',0.15],
    ['Saudi Arabia',0.12],['UAE',0.08],['Canada',0.05]
  ];
  const prodWeights = [0.12,0.30,0.20,0.10,0.18,0.10];
  const rows = [];
  const start = new Date(2025,0,1).getTime();
  const end = new Date(2026,11,31).getTime();
  const range = end - start;
  for(let i=0;i<12000;i++){
    const d = new Date(start + Math.random()*range);
    // weighted product
    let r=Math.random(), acc=0, pIdx=0;
    for(let j=0;j<prodWeights.length;j++){ acc+=prodWeights[j]; if(r<=acc){pIdx=j;break;} }
    const [pname, basePrice] = products[pIdx];
    const price = +(basePrice*(0.95+Math.random()*0.10)).toFixed(2);
    const qtyArr = [1,2,3,5,10], qtyW=[0.50,0.28,0.12,0.07,0.03];
    let qr=Math.random(), qacc=0, qty=1;
    for(let j=0;j<qtyW.length;j++){ qacc+=qtyW[j]; if(qr<=qacc){qty=qtyArr[j];break;} }
    const cArr = countries, cW = [0.38,0.22,0.15,0.12,0.08,0.05];
    let cr=Math.random(), cacc=0, cIdx=0;
    for(let j=0;j<cW.length;j++){ cacc+=cW[j]; if(cr<=cacc){cIdx=j;break;} }
    rows.push({
      InvoiceNo:'INV-'+(500000+i),
      Description:pname, Quantity:qty,
      InvoiceDate:d, Date:new Date(d.getFullYear(),d.getMonth(),d.getDate()),
      UnitPrice:price, Country:cArr[cIdx][0], Revenue:price*qty
    });
  }
  return rows;
}

/* =========================================================
   5. FORECAST
   ========================================================= */
function runForecast(df, horizon){
  // Aggregate daily
  const dailyMap = {};
  for(const r of df){
    if(!r.Date) continue;
    const k = r.Date.getFullYear()+'-'+String(r.Date.getMonth()+1).padStart(2,'0')+'-'+String(r.Date.getDate()).padStart(2,'0');
    dailyMap[k] = (dailyMap[k]||0) + r.Revenue;
  }
  const allDates = Object.keys(dailyMap).sort();
  const values = allDates.map(d=>dailyMap[d]);

  if(allDates.length < 10){
    const lastDate = allDates.length ? new Date(allDates[allDates.length-1]) : new Date();
    const mean = values.length ? values.reduce((a,b)=>a+b,0)/values.length : 1500;
    const futureDates=[], futurePreds=[];
    for(let i=1;i<=horizon;i++){
      futureDates.push(new Date(lastDate.getTime()+i*86400000));
      futurePreds.push(mean*(1+(Math.random()-0.5)*0.15));
    }
    return {historicalDates:[],historicalValues:[],futureDates,futurePreds,
      mae:mean*0.04,rmse:mean*0.07,insufficient:true};
  }

  const n = allDates.length;
  function fitModel(vals, dts){
    const m = vals.length;
    const yMean = vals.reduce((a,b)=>a+b,0)/m;
    const xMean = (m-1)/2;
    let num=0, den=0;
    for(let i=0;i<m;i++){ num+=(i-xMean)*(vals[i]-yMean); den+=(i-xMean)**2; }
    const slope = den ? num/den : 0;
    const intercept = yMean - slope*xMean;
    const dowS=Array(7).fill(0), dowC=Array(7).fill(0);
    const monS=Array(12).fill(0), monC=Array(12).fill(0);
    dts.forEach((ds,i)=>{
      const d=new Date(ds);
      dowS[d.getDay()]+=vals[i]; dowC[d.getDay()]++;
      monS[d.getMonth()]+=vals[i]; monC[d.getMonth()]++;
    });
    const dowF = dowS.map((s,i)=>dowC[i]?(s/dowC[i])/yMean:1);
    const monF = monS.map((s,i)=>monC[i]?(s/monC[i])/yMean:1);
    return {slope,intercept,dowF,monF};
  }
  function predict(m, date, idx){
    return Math.max(0, (m.intercept + m.slope*idx) * m.dowF[date.getDay()] * m.monF[date.getMonth()]);
  }

  // Holdout validation
  const testN = Math.min(horizon, Math.max(1, Math.floor(n/4)));
  const trainN = n - testN;
  const trainModel = fitModel(values.slice(0,trainN), allDates.slice(0,trainN));
  let maeSum=0, rmseSum=0;
  for(let i=trainN;i<n;i++){
    const p = predict(trainModel, new Date(allDates[i]), i);
    maeSum += Math.abs(values[i]-p);
    rmseSum += (values[i]-p)**2;
  }
  const mae = maeSum/testN, rmse = Math.sqrt(rmseSum/testN);

  // Full fit for future
  const fullModel = fitModel(values, allDates);
  const lastDate = new Date(allDates[n-1]);
  const futureDates=[], futurePreds=[];
  for(let i=1;i<=horizon;i++){
    const d = new Date(lastDate.getTime()+i*86400000);
    futureDates.push(d);
    futurePreds.push(predict(fullModel, d, n-1+i));
  }
  const histLen = Math.min(horizon, n);
  const historicalDates = allDates.slice(-histLen).map(d=>new Date(d));
  const historicalValues = values.slice(-histLen);

  return {historicalDates,historicalValues,futureDates,futurePreds,mae,rmse,insufficient:false};
}

/* =========================================================
   6. HELPERS
   ========================================================= */
function styleFig(fig, height=320, legend=false){
  const fam = state.language==='ar' ? AR_FONT : EN_FONT;
  return {...fig, layout:{
    ...fig.layout,
    margin:{l:0,r:0,t:10,b:0}, height,
    paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
    font:{family:fam, color:'#94a3b8'},
    hoverlabel:{bgcolor:'#0f172a', bordercolor:'#6366f1', font:{family:fam,color:'#e2e8f0'}},
    showlegend: legend
  }};
}
function iconHTML(name, size='1.2rem', cls=''){
  return `<i class="material-icons ${cls}" style="font-size:${size};line-height:1;vertical-align:middle">${name}</i>`;
}
function setCurrency(code){
  state.currencyCode = code;
  state.currencySymbol = CURRENCY_SYMBOLS[code] || '$';
  setPage(state.page, false);
}
function loadDemoData(){
  state.df = getDemoData();
  state.datasetMode = null;
  state.currencyCode = 'USD'; state.currencySymbol = '$';
  notify(t('demo_loaded'),'positive');
  setPage('dashboard');
}

function heroSVG(){
  const xs = [78,130,182,234,286,338,390,442];
  const hs = [60,85,70,110,95,140,125,170];
  const ys = [235,210,225,185,200,155,170,125];
  function smooth(pts){
    let d = `M${pts[0][0]} ${pts[0][1]}`;
    for(let i=0;i<pts.length-1;i++){
      const [x0,y0]=pts[i],[x1,y1]=pts[i+1];
      const dx=(x1-x0)*0.5;
      d += ` C${x0+dx} ${y0} ${x1-dx} ${y1} ${x1} ${y1}`;
    }
    return d;
  }
  const pts = xs.map((x,i)=>[x,ys[i]]);
  const line = smooth(pts);
  const area = `${line} L${xs[xs.length-1]} 320 L${xs[0]} 320 Z`;
  const fline = smooth([[xs[xs.length-1],ys[ys.length-1]],[494,108],[546,92]]);
  let bars = '';
  xs.forEach((x,i)=>{ bars += `<rect class="vbar" x="${x-18}" y="${320-hs[i]}" width="36" height="${hs[i]}" rx="8" fill="url(#gBar)" style="animation-delay:${0.12*i}s"/>`; });
  let grid='';
  [120,170,220,270,320].forEach(y=>{ grid += `<line x1="50" y1="${y}" x2="570" y2="${y}" stroke="rgba(255,255,255,.06)" stroke-width="1"/>`; });
  return `<svg viewBox="0 0 720 380" xmlns="http://www.w3.org/2000/svg" role="img" aria-hidden="true">
    <defs>
      <linearGradient id="gBar" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#818cf8" stop-opacity=".95"/><stop offset="1" stop-color="#4338ca" stop-opacity=".35"/></linearGradient>
      <linearGradient id="gArea" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#22d3ee" stop-opacity=".35"/><stop offset="1" stop-color="#22d3ee" stop-opacity="0"/></linearGradient>
      <linearGradient id="gLine" x1="0" y1="0" x2="1" y2="0"><stop offset="0" stop-color="#818cf8"/><stop offset=".55" stop-color="#22d3ee"/><stop offset="1" stop-color="#c084fc"/></linearGradient>
      <linearGradient id="gPanel" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#1e293b" stop-opacity=".92"/><stop offset="1" stop-color="#0f172a" stop-opacity=".92"/></linearGradient>
    </defs>
    <rect x="20" y="20" width="680" height="340" rx="26" fill="url(#gPanel)" stroke="rgba(255,255,255,.12)"/>
    <circle cx="52" cy="50" r="5" fill="#f87171"/><circle cx="72" cy="50" r="5" fill="#fbbf24"/><circle cx="92" cy="50" r="5" fill="#34d399"/>
    <rect x="130" y="44" width="150" height="12" rx="6" fill="rgba(255,255,255,.08)"/>
    <rect x="560" y="44" width="110" height="12" rx="6" fill="rgba(129,140,248,.25)"/>
    ${grid}${bars}
    <path d="${area}" fill="url(#gArea)" class="fade-in"/>
    <path d="M442 125 L546 68 L546 122 Z" fill="rgba(34,211,238,.13)" class="fade-in"/>
    <path d="${line}" fill="none" stroke="url(#gLine)" stroke-width="4" stroke-linecap="round" pathLength="1" class="draw-line"/>
    <path d="${fline}" fill="none" stroke="#22d3ee" stroke-width="3" stroke-linecap="round" stroke-dasharray="7 7" class="fade-in"/>
    <circle r="11" fill="#22d3ee" opacity=".25"><animateMotion dur="6s" repeatCount="indefinite" path="${line}"/></circle>
    <circle r="5" fill="#22d3ee"><animateMotion dur="6s" repeatCount="indefinite" path="${line}"/></circle>
    <g transform="rotate(-90 628 170)" fill="none" stroke-width="14" stroke-linecap="round">
      <circle cx="628" cy="170" r="42" stroke="rgba(255,255,255,.07)"/>
      <circle class="donut-seg" cx="628" cy="170" r="42" stroke="#818cf8" stroke-dasharray="118 264"/>
      <circle class="donut-seg" cx="628" cy="170" r="42" stroke="#22d3ee" stroke-dasharray="66 264" stroke-dashoffset="-128"/>
      <circle class="donut-seg" cx="628" cy="170" r="42" stroke="#f472b6" stroke-dasharray="38 264" stroke-dashoffset="-204"/>
    </g>
    <text x="628" y="177" text-anchor="middle" fill="#e0e7ff" font-size="20" font-weight="800">AI</text>
    <g fill="rgba(255,255,255,.12)">
      <circle cx="590" cy="250" r="5" fill="#818cf8"/><rect x="604" y="246" width="80" height="8" rx="4"/>
      <circle cx="590" cy="274" r="5" fill="#22d3ee"/><rect x="604" y="270" width="62" height="8" rx="4"/>
      <circle cx="590" cy="298" r="5" fill="#f472b6"/><rect x="604" y="294" width="48" height="8" rx="4"/>
    </g>
  </svg>`;
}

function computeChipStats(df){
  let growth='—', topMarket='—', orders=0;
  try{
    const monthly = {};
    df.forEach(r=>{
      if(!r.InvoiceDate) return;
      const k = r.InvoiceDate.getFullYear()+'-'+String(r.InvoiceDate.getMonth()+1).padStart(2,'0');
      monthly[k] = (monthly[k]||0) + r.Revenue;
    });
    const keys = Object.keys(monthly).sort();
    if(keys.length>=2 && monthly[keys[keys.length-2]]){
      growth = ((monthly[keys[keys.length-1]]/monthly[keys[keys.length-2]]-1)*100).toFixed(1);
      growth = (parseFloat(growth)>=0?'+':'')+growth+'%';
    }
    const byCountry = {};
    df.forEach(r=>{ byCountry[r.Country]=(byCountry[r.Country]||0)+r.Revenue; });
    let maxV=-1; for(const k in byCountry){ if(byCountry[k]>maxV){maxV=byCountry[k]; topMarket=k;} }
    const invSet = new Set(df.map(r=>r.InvoiceNo));
    orders = invSet.size;
  }catch(e){}
  return {growth, topMarket, orders};
}

/* =========================================================
   7. PAGES
   ========================================================= */
const contentContainer = document.getElementById('content-container');

function renderHome(){
  const df = state.df;
  const {growth, topMarket, orders} = computeChipStats(df);

  let totalRev = 0; df.forEach(r=>totalRev+=r.Revenue);
  const forecastRev = totalRev*1.15;
  const byProd = {};
  df.forEach(r=>{ byProd[r.Description]=(byProd[r.Description]||0)+r.Revenue; });
  let topProd='Enterprise Suite Pro', maxP=-1;
  for(const k in byProd){ if(byProd[k]>maxP){maxP=byProd[k]; topProd=k;} }

  const monthly = {};
  df.forEach(r=>{
    if(!r.InvoiceDate) return;
    const k = r.InvoiceDate.getFullYear()+'-'+String(r.InvoiceDate.getMonth()+1).padStart(2,'0');
    monthly[k] = (monthly[k]||0) + r.Revenue;
  });
  const mKeys = Object.keys(monthly).sort();

  contentContainer.innerHTML = `
    <div class="w-full max-w-6xl mx-auto mt-8 px-6 items-center text-center flex flex-col">
      <div class="glass-card px-5 py-2 rounded-full items-center gap-3 mb-6 reveal inline-flex">
        <span class="live-dot"></span>
        <span class="text-sm font-semibold text-gray-200">${t('app_subtitle')}</span>
      </div>
      <div class="gap-3 mb-4 max-w-4xl items-center justify-center w-full reveal flex flex-col" style="--d:.1s">
        <h1 class="hero-display text-4xl md:text-6xl font-playfair font-bold text-white tracking-tight leading-tight text-center w-full">${t('hero_title')}</h1>
        <h2 class="hero-display text-xl md:text-3xl font-playfair font-bold italic hero-text tracking-tight text-center w-full">${t('hero_subtitle')}</h2>
      </div>
      <div class="glow-line max-w-xl mx-auto mb-5"></div>
      <p class="text-base md:text-lg text-gray-300 max-w-2xl mb-6 leading-relaxed text-center w-full reveal" style="--d:.2s">${t('hero_desc')}</p>

      <div class="hero-visual reveal w-full" style="--d:.3s">
        ${heroSVG()}
        <div class="float-chip chip-1">
          <div class="chip-icon" style="background:linear-gradient(135deg,#10b981,#059669)">${iconHTML('trending_up','1.2rem','text-white')}</div>
          <div class="flex flex-col items-start">
            <div class="chip-label">${t('chip_growth')}</div>
            <div class="chip-value num count-up">${growth}</div>
          </div>
        </div>
        <div class="float-chip chip-2">
          <div class="chip-icon" style="background:linear-gradient(135deg,#6366f1,#4f46e5)">${iconHTML('shopping_cart','1.2rem','text-white')}</div>
          <div class="flex flex-col items-start">
            <div class="chip-label">${t('chip_orders')}</div>
            <div class="chip-value num count-up">${orders.toLocaleString()}</div>
          </div>
        </div>
        <div class="float-chip chip-3">
          <div class="chip-icon" style="background:linear-gradient(135deg,#ec4899,#be185d)">${iconHTML('public','1.2rem','text-white')}</div>
          <div class="flex flex-col items-start">
            <div class="chip-label">${t('chip_market')}</div>
            <div class="chip-value">${topMarket}</div>
          </div>
        </div>
      </div>

      <div class="upload-bubble-wrap reveal w-full flex flex-col items-center" style="--d:.4s">
        <div class="ring ring-1"></div><div class="ring ring-2"></div>
        <div class="upload-bubble" onclick="document.getElementById('file-uploader').click()">
          ${iconHTML('cloud_upload','3.5rem','text-white')}
        </div>
        <div class="upload-bubble-label">${t('upload_bubble_title')}</div>
        <div class="upload-bubble-sub">${t('upload_bubble_subtitle')}</div>
      </div>

      <div class="glow-line max-w-xl mx-auto my-8"></div>

      <div class="w-full glass-card p-6 md:p-8 rounded-3xl border border-indigo-500/30 mb-16 text-left reveal flex flex-col">
        <div class="w-full justify-between items-center mb-4 flex flex-wrap gap-3">
          <div class="flex flex-col">
            <div class="text-2xl font-playfair font-bold text-white">${t('preview_title')}</div>
            <div class="text-xs text-gray-400 italic">${t('preview_hint')}</div>
          </div>
          <div class="flex items-center gap-3 bg-indigo-500/15 px-3 py-1.5 rounded-xl border border-indigo-500/30">
            ${iconHTML('info','1.1rem','text-indigo-300')}
            <span class="text-xs font-bold text-indigo-200">${t('preview_badge')}</span>
            <button onclick="event.stopPropagation();loadDemoData()" class="text-xs font-bold text-white bg-indigo-600 px-3 py-1 rounded-lg ripple-btn">${t('load_demo')}</button>
          </div>
        </div>

        <div class="w-full cursor-pointer flex flex-col" onclick="setPage('dashboard')">
          <div class="w-full gap-4 mb-5 flex flex-wrap">
            <div class="glass-card flex-1 min-w-[200px] p-4 rounded-xl">
              <div class="text-xs text-gray-400 font-bold uppercase">${t('total_revenue')}</div>
              <div class="text-2xl font-extrabold text-white mt-1 num count-up">${money(totalRev,0)}</div>
              <div class="text-xs text-emerald-400 mt-1 font-medium">${t('active_period')}</div>
            </div>
            <div class="glass-card flex-1 min-w-[200px] p-4 rounded-xl">
              <div class="text-xs text-gray-400 font-bold uppercase">${t('projected_forecast')}</div>
              <div class="text-2xl font-extrabold text-indigo-300 mt-1 num count-up">${money(forecastRev,0)}</div>
              <div class="text-xs text-indigo-400 mt-1 font-medium">${t('ai_projection')}</div>
            </div>
            <div class="glass-card flex-1 min-w-[200px] p-4 rounded-xl">
              <div class="text-xs text-gray-400 font-bold uppercase">${t('top_product_line')}</div>
              <div class="text-base font-bold text-white mt-1 truncate">${topProd}</div>
              <div class="text-xs text-gray-400 mt-1">${t('highest_rev_contribution')}</div>
            </div>
          </div>
          <div id="chart-home-preview" class="w-full"></div>
          <div class="text-center text-xs text-indigo-300 mt-3 font-semibold w-full">${t('preview_click_hint')}</div>
        </div>
      </div>

      <div class="w-full gap-6 justify-center mb-16 flex flex-wrap">
        ${[
          ['01',t('benefit_1_tag'),t('benefit_1_title'),t('benefit_1_desc'),'insights','from-indigo-500 to-blue-600'],
          ['02',t('benefit_2_tag'),t('benefit_2_title'),t('benefit_2_desc'),'analytics','from-purple-500 to-pink-600'],
          ['03',t('benefit_3_tag'),t('benefit_3_title'),t('benefit_3_desc'),'auto_awesome','from-cyan-500 to-emerald-500']
        ].map(([num,tag,title,desc,ic,grad],i)=>`
          <div class="feature-card flex-1 min-w-[260px] p-6 items-start text-left flex flex-col reveal" style="--d:${i*0.15}s">
            <div class="feature-num">${num}</div>
            <div class="feature-icon bg-gradient-to-br ${grad}">${iconHTML(ic,'1.8rem','text-white')}</div>
            <div class="text-[11px] font-bold text-purple-300 uppercase tracking-widest">${tag}</div>
            <div class="text-xl font-bold text-white font-playfair mt-1">${title}</div>
            <div class="text-sm text-gray-300 leading-relaxed mt-2">${desc}</div>
          </div>
        `).join('')}
      </div>

      <div class="w-full text-center my-10 items-center flex flex-col">
        <div class="text-2xl md:text-4xl font-playfair font-bold text-white mb-3">${t('steps_title')}</div>
        <div class="text-sm md:text-base text-gray-400 max-w-2xl mb-10">${t('steps_subtitle')}</div>
        <div class="w-full gap-6 justify-center flex flex-wrap">
          ${[
            ['1',t('step_1_title'),t('step_1_desc'),'cloud_upload'],
            ['2',t('step_2_title'),t('step_2_desc'),'travel_explore'],
            ['3',t('step_3_title'),t('step_3_desc'),'rocket_launch']
          ].map(([idx,title,desc,ic],i)=>`
            <div class="step-card flex-1 min-w-[240px] p-6 text-left items-start flex flex-col reveal" style="--d:${i*0.15}s">
              <div class="items-center gap-3 mb-3 flex">
                <div class="step-num-badge text-base">0${idx}</div>
                ${iconHTML(ic,'1.5rem','text-indigo-300')}
              </div>
              <div class="text-base font-bold text-white mb-2">${title}</div>
              <div class="text-sm text-gray-400 leading-relaxed">${desc}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;

  // Draw chart
  const mData = mKeys.map(k=>monthly[k]);
  Plotly.newPlot('chart-home-preview', [{
    x: mKeys, y: mData, name:t('historical_sales'), mode:'lines+markers',
    fill:'tozeroy', fillcolor:'rgba(129,140,248,0.12)',
    line:{color:'#818cf8', width:3, shape:'spline'},
    marker:{size:6, color:'#22d3ee'}
  }], {
    margin:{l:0,r:0,t:10,b:0}, height:260,
    paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
    font:{family:state.language==='ar'?AR_FONT:EN_FONT, color:'#94a3b8'},
    showlegend:false,
    xaxis:{showgrid:false, linecolor:'#334155'},
    yaxis:{showgrid:true, gridcolor:'#1e293b'}
  }, {displayModeBar:false, responsive:true});

  runCountUp();
}

function renderDashboard(){
  const df = state.df;
  let totalRev=0; df.forEach(r=>totalRev+=r.Revenue);
  const invSet = new Set(df.map(r=>r.InvoiceNo));
  const orders = invSet.size || df.length;
  const aov = orders ? totalRev/orders : 0;
  const countries = new Set(df.map(r=>r.Country)).size || 1;

  const monthly = {};
  df.forEach(r=>{ if(!r.InvoiceDate) return; const k = r.InvoiceDate.getFullYear()+'-'+String(r.InvoiceDate.getMonth()+1).padStart(2,'0'); monthly[k]=(monthly[k]||0)+r.Revenue; });
  const mKeys = Object.keys(monthly).sort();
  const mData = mKeys.map(k=>monthly[k]);
  const maxMonthKey = mKeys[mKeys.length-1]||'—';

  const byCountry = {};
  df.forEach(r=>{ byCountry[r.Country]=(byCountry[r.Country]||0)+r.Revenue; });
  const cArr = Object.entries(byCountry).sort((a,b)=>b[1]-a[1]).slice(0,5);
  const topCountry = cArr[0]?cArr[0][0]:t('na');

  const currencyOpts = Object.keys(CURRENCY_SYMBOLS).map(c=>`<option value="${c}" ${c===state.currencyCode?'selected':''}>${c}</option>`).join('');

  contentContainer.innerHTML = `
    <div class="w-full max-w-7xl mx-auto px-6">
      <div class="w-full justify-between items-center glass-card px-6 py-4 rounded-2xl mb-6 gap-4 flex flex-wrap reveal">
        <div class="items-center gap-3 flex">
          ${iconHTML('dataset','1.5rem','text-indigo-400')}
          <div class="text-sm font-semibold text-gray-200">${t('active_context')}: ${datasetLabel()}</div>
        </div>
        <div class="gap-3 items-center flex">
          <select onchange="setCurrency(this.value)" class="bg-[#0f172a] border border-indigo-500 rounded-lg px-3 py-1.5 text-sm text-white">
            ${currencyOpts}
          </select>
          <button onclick="loadDemoData()" class="text-xs font-bold ripple-btn px-3 py-2 rounded-lg border border-indigo-500 text-indigo-200 hover:bg-indigo-500/20">${t('reset_demo')}</button>
        </div>
      </div>

      <div class="w-full gap-5 mb-6 flex flex-wrap">
        ${[
          ['payments','text-indigo-400',t('total_revenue'),money(totalRev,0)],
          ['shopping_cart','text-emerald-400',t('total_orders'),orders.toLocaleString()],
          ['analytics','text-orange-400',t('avg_order_value'),money(aov)],
          ['public','text-blue-400',t('active_markets'),String(countries)]
        ].map(([ic,col,lbl,val],i)=>`
          <div class="glass-card flex-1 min-w-[200px] p-5 rounded-2xl reveal" style="--d:${i*0.1}s">
            <div class="w-full justify-between items-start flex">
              <div class="text-xs text-gray-400 font-bold uppercase tracking-wider">${lbl}</div>
              <div class="kpi-icon">${iconHTML(ic,'1.4rem',col)}</div>
            </div>
            <div class="text-3xl font-extrabold text-white mt-3 num count-up">${val}</div>
          </div>
        `).join('')}
      </div>

      <div class="w-full gap-6 mb-6 flex flex-wrap">
        <div class="glass-card w-full lg:w-2/3 p-6 rounded-2xl flex flex-col justify-between reveal">
          <div class="text-2xl font-bold mb-4 text-gray-100 font-playfair">${t('revenue_over_time')}</div>
          <div id="chart-dash-monthly" class="w-full"></div>
          <div class="w-full p-4 mt-4 rounded-xl insight-box flex flex-col">
            <div class="text-xs font-bold text-indigo-300 uppercase tracking-wide mb-1">${t('executive_takeaway')}</div>
            <div class="text-sm text-gray-300 leading-relaxed">${t('takeaway_msg',{m:iso(maxMonthKey)})}</div>
          </div>
        </div>
        <div class="glass-card w-full lg:w-1/3 p-6 rounded-2xl flex flex-col justify-between reveal" style="--d:.15s">
          <div class="text-2xl font-bold mb-4 text-gray-100 font-playfair">${t('top_markets')}</div>
          <div id="chart-dash-markets" class="w-full"></div>
          <div class="w-full p-4 mt-4 rounded-xl insight-box flex flex-col">
            <div class="text-xs font-bold text-purple-300 uppercase tracking-wide mb-1">${t('market_recommendation')}</div>
            <div class="text-sm text-gray-300 leading-relaxed">${t('market_msg',{c:iso(topCountry)})}</div>
          </div>
        </div>
      </div>
    </div>
  `;

  const fam = state.language==='ar'?AR_FONT:EN_FONT;
  Plotly.newPlot('chart-dash-monthly', [{
    x: mKeys, y: mData, type:'scatter', mode:'lines',
    line:{width:3, color:'#818cf8', shape:'spline'},
    fill:'tozeroy', fillcolor:'rgba(129,140,248,0.18)'
  }], {
    margin:{l:0,r:0,t:10,b:0}, height:320,
    paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
    font:{family:fam, color:'#94a3b8'},
    xaxis:{showgrid:false, linecolor:'#334155'},
    yaxis:{showgrid:true, gridcolor:'#1e293b'}
  }, {displayModeBar:false, responsive:true});

  const cY = cArr.map(x=>x[0]).reverse();
  const cX = cArr.map(x=>x[1]).reverse();
  Plotly.newPlot('chart-dash-markets', [{
    x: cX, y: cY, type:'bar', orientation:'h',
    marker:{color:'#c084fc'}
  }], {
    margin:{l:0,r:0,t:10,b:0}, height:320,
    paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
    font:{family:fam, color:'#94a3b8'},
    xaxis:{showgrid:true, gridcolor:'#1e293b', linecolor:'#334155'},
    yaxis:{showgrid:false}
  }, {displayModeBar:false, responsive:true});

  runCountUp();
}

function renderForecast(){
  contentContainer.innerHTML = `
    <div class="w-full max-w-7xl mx-auto px-6">
      <div class="text-4xl font-playfair font-bold mb-6 text-white mt-2 reveal">${t('forecast_page_title')}</div>
      <div class="items-center mb-6 glass-card p-5 rounded-2xl border border-gray-700 w-full justify-between flex flex-wrap gap-3 reveal">
        <div class="items-center gap-4 flex flex-wrap">
          <input id="horizon-slider" type="range" min="7" max="60" value="30" step="1" class="w-64 accent-indigo-500"
                 oninput="document.getElementById('horizon-label').textContent='${t('forecast_horizon',{n:'__N__'}).replace('__N__', this.value)}'">
          <span id="horizon-label" class="text-gray-200 font-semibold">${t('forecast_horizon',{n:30})}</span>
        </div>
        <button onclick="runForecastClick()" class="ripple-btn bg-indigo-600 text-white font-bold px-8 py-3 rounded-xl hover:bg-indigo-500">${t('generate_forecast')}</button>
      </div>
      <div id="forecast-output" class="w-full"></div>
    </div>
  `;
  runForecastClick();
}

function runForecastClick(){
  const h = parseInt(document.getElementById('horizon-slider').value);
  const out = document.getElementById('forecast-output');
  out.innerHTML = '<div class="spinner"></div>';

  setTimeout(()=>{
    const res = runForecast(state.df, h);
    if(!res){
      out.innerHTML = `<div class="glass-card p-6 rounded-2xl text-center text-gray-300">${t('forecast_insufficient')}</div>`;
      return;
    }
    const fmtD = d => d.toISOString().slice(0,10);
    const histX = res.historicalDates.map(fmtD);
    const futX = res.futureDates.map(fmtD);

    out.innerHTML = `
      <div class="w-full gap-5 mb-6 flex flex-wrap">
        <div class="p-5 glass-card rounded-2xl flex-1 min-w-[220px] border-l-4 border-indigo-500 reveal">
          <div class="text-xs text-gray-400 font-bold uppercase">${t('mae_label')}</div>
          <div class="text-3xl font-extrabold text-white mt-2 num">${money(res.mae)}</div>
        </div>
        <div class="p-5 glass-card rounded-2xl flex-1 min-w-[220px] border-l-4 border-pink-500 reveal" style="--d:.1s">
          <div class="text-xs text-gray-400 font-bold uppercase">${t('rmse_label')}</div>
          <div class="text-3xl font-extrabold text-white mt-2 num">${money(res.rmse)}</div>
        </div>
      </div>
      <div class="w-full p-6 glass-card rounded-2xl reveal" style="--d:.2s">
        <div id="chart-forecast" class="w-full"></div>
        <div class="w-full p-4 mt-4 rounded-xl insight-box flex flex-col">
          <div class="text-xs font-bold text-indigo-300 uppercase tracking-wide mb-1">${t('forecast_evaluation')}</div>
          <div class="text-sm text-gray-300 leading-relaxed">${t('forecast_msg',{h, mae:iso(money(res.mae))})}</div>
        </div>
      </div>
    `;

    const fam = state.language==='ar'?AR_FONT:EN_FONT;
    const traces = [];
    if(histX.length){
      traces.push({
        x: histX, y: res.historicalValues, name:t('historical_sales'),
        mode:'lines+markers', line:{color:'#94a3b8', width:2}
      });
    }
    traces.push({
      x: futX, y: res.futurePreds, name:t('projected_revenue'),
      mode:'lines', fill:'tozeroy', fillcolor:'rgba(129,140,248,0.10)',
      line:{color:'#818cf8', width:3, dash:'dash', shape:'spline'}
    });
    Plotly.newPlot('chart-forecast', traces, {
      margin:{l:0,r:0,t:10,b:0}, height:400,
      paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
      font:{family:fam, color:'#94a3b8'},
      hovermode:'x unified',
      showlegend:true,
      legend:{orientation:'h', y:1.08},
      xaxis:{showgrid:false, linecolor:'#334155'},
      yaxis:{showgrid:true, gridcolor:'#1e293b'}
    }, {displayModeBar:false, responsive:true});
  }, 50);
}

function renderInsights(){
  const df = state.df;
  let totalRev = 0; df.forEach(r=>totalRev+=r.Revenue);
  if(totalRev===0) totalRev = 1;

  const byProd = {}; df.forEach(r=>{ byProd[r.Description]=(byProd[r.Description]||0)+r.Revenue; });
  let topP='Standard Line', topPVal=0;
  for(const k in byProd){ if(byProd[k]>topPVal){topPVal=byProd[k];topP=k;} }

  const byCountry = {}; df.forEach(r=>{ byCountry[r.Country]=(byCountry[r.Country]||0)+r.Revenue; });
  let topC='Global Market', topCVal=0;
  for(const k in byCountry){ if(byCountry[k]>topCVal){topCVal=byCountry[k];topC=k;} }

  const pctP = (topPVal/totalRev*100);
  const pctC = (topCVal/totalRev*100);

  contentContainer.innerHTML = `
    <div class="w-full max-w-7xl mx-auto px-6">
      <div class="text-4xl font-playfair font-bold mb-6 text-white mt-2 reveal">${t('insights_title')}</div>

      <div class="w-full gap-6 mb-8 flex flex-wrap">
        <div class="glass-card flex-1 min-w-[260px] p-6 rounded-2xl border-l-4 border-emerald-500 reveal">
          ${iconHTML('verified','2rem','text-emerald-400')}
          <div class="text-xs font-bold text-gray-400 uppercase mt-2">${t('primary_driver')}</div>
          <div class="text-2xl font-bold text-white mt-1 font-playfair">${topP}</div>
          <div class="w-full bg-slate-800 rounded-full h-2 mt-3 overflow-hidden"><div class="h-full bg-emerald-400" style="width:${Math.min(pctP,100)}%"></div></div>
          <div class="text-xs text-emerald-400 mt-2">${t('share_msg',{amt:iso(money(topPVal)), pct:iso(pctP.toFixed(1)+'%')})}</div>
        </div>
        <div class="glass-card flex-1 min-w-[260px] p-6 rounded-2xl border-l-4 border-indigo-500 reveal" style="--d:.12s">
          ${iconHTML('public','2rem','text-indigo-400')}
          <div class="text-xs font-bold text-gray-400 uppercase mt-2">${t('core_market')}</div>
          <div class="text-2xl font-bold text-white mt-1 font-playfair">${topC}</div>
          <div class="w-full bg-slate-800 rounded-full h-2 mt-3 overflow-hidden"><div class="h-full bg-indigo-400" style="width:${Math.min(pctC,100)}%"></div></div>
          <div class="text-xs text-indigo-400 mt-2">${t('share_msg',{amt:iso(money(topCVal)), pct:iso(pctC.toFixed(1)+'%')})}</div>
        </div>
      </div>

      <div class="w-full glass-card p-8 rounded-2xl reveal" style="--d:.2s">
        <div class="text-2xl font-bold text-white font-playfair mb-5">${t('strategic_plan')}</div>
        ${[
          ['inventory_2', t('rec_1',{name:iso(topP)})],
          ['campaign', t('rec_2',{name:iso(topC)})],
          ['query_stats', t('rec_3')],
          ['shopping_basket', t('rec_4')]
        ].map(([ic,r])=>`
          <div class="w-full items-start gap-4 mb-4 flex flex-nowrap">
            <div class="rec-num">${iconHTML(ic,'1.2rem','text-indigo-200')}</div>
            <div class="text-base text-gray-300 leading-relaxed">${r}</div>
          </div>
        `).join('')}
      </div>
    </div>
  `;
}

function renderSummary(){
  const df = state.df;
  let total=0; df.forEach(r=>total+=r.Revenue);
  const invSet = new Set(df.map(r=>r.InvoiceNo));
  const orders = invSet.size || df.length;
  const avg = orders ? total/orders : 0;

  const byProd = {}; df.forEach(r=>{ byProd[r.Description]=(byProd[r.Description]||0)+r.Revenue; });
  const prodArr = Object.entries(byProd).sort((a,b)=>b[1]-a[1]);
  const byCountry = {}; df.forEach(r=>{ byCountry[r.Country]=(byCountry[r.Country]||0)+r.Revenue; });
  const countryArr = Object.entries(byCountry).sort((a,b)=>b[1]-a[1]);

  const dates = df.filter(r=>r.InvoiceDate).map(r=>r.InvoiceDate.getTime());
  const startD = dates.length ? new Date(Math.min(...dates)) : null;
  const endD = dates.length ? new Date(Math.max(...dates)) : null;

  const topProd = prodArr[0]?prodArr[0][0]:'—';
  const topProdPct = prodArr[0]&&total ? (prodArr[0][1]/total*100).toFixed(1) : '0';
  const topCountry = countryArr[0]?countryArr[0][0]:'—';
  const topCountryPct = countryArr[0]&&total ? (countryArr[0][1]/total*100).toFixed(1) : '0';

  contentContainer.innerHTML = `
    <div class="w-full max-w-7xl mx-auto px-6">
      <div class="text-4xl font-playfair font-bold mb-2 text-white mt-2 reveal">${t('summary_title')}</div>
      <div class="glow-line mb-6"></div>

      <div class="w-full gap-5 mb-6 flex flex-wrap">
        ${[
          [t('total_revenue'),money(total,0),'payments'],
          [t('orders_label'),orders.toLocaleString(),'shopping_cart'],
          [t('avg_order_value'),money(avg),'analytics']
        ].map(([lbl,val,ic],i)=>`
          <div class="glass-card flex-1 min-w-[200px] p-5 rounded-2xl reveal" style="--d:${i*0.1}s">
            ${iconHTML(ic,'1.5rem','text-indigo-400')}
            <div class="text-sm text-gray-300 mt-2">${lbl}</div>
            <div class="text-3xl font-extrabold text-white mt-2 num count-up">${val}</div>
          </div>
        `).join('')}
      </div>

      <div class="w-full glass-card p-6 rounded-2xl reveal">
        <div class="text-2xl font-bold text-white mb-4 font-playfair">${t('key_findings')}</div>
        ${[
          [t('reporting_period'), `${iso(startD?fmtDate(startD):'—')} – ${iso(endD?fmtDate(endD):'—')}`],
          [t('top_product_by_rev'), `${iso(topProd)} (${iso(topProdPct+'%')})`],
          [t('top_market_by_rev'), `${iso(topCountry)} (${iso(topCountryPct+'%')})`]
        ].map(([lbl,val])=>`
          <div class="w-full justify-between items-start gap-4 py-3 border-b border-gray-700 flex">
            <div class="text-sm text-gray-400">${lbl}</div>
            <div class="text-sm text-white font-semibold">${val}</div>
          </div>
        `).join('')}
      </div>

      <div class="w-full gap-5 mt-5 flex flex-wrap">
        <div class="glass-card flex-1 min-w-[280px] p-5 rounded-2xl reveal">
          <div class="text-xl font-bold text-white mb-3 font-playfair">${t('rev_by_product')}</div>
          <div id="chart-summary-products" class="w-full"></div>
        </div>
        <div class="glass-card flex-1 min-w-[280px] p-5 rounded-2xl reveal" style="--d:.12s">
          <div class="text-xl font-bold text-white mb-3 font-playfair">${t('rev_by_market')}</div>
          <div id="chart-summary-markets" class="w-full"></div>
        </div>
      </div>
    </div>
  `;

  const fam = state.language==='ar'?AR_FONT:EN_FONT;
  const top7Prod = prodArr.slice(0,7);
  Plotly.newPlot('chart-summary-products', [{
    labels: top7Prod.map(x=>x[0]), values: top7Prod.map(x=>x[1]),
    type:'pie', hole:0.62,
    marker:{colors:['#818cf8','#22d3ee','#c084fc','#f472b6','#34d399','#fbbf24','#60a5fa'], line:{color:'#0b1224', width:2}}
  }], {
    margin:{l:0,r:0,t:0,b:0}, height:330,
    paper_bgcolor:'rgba(0,0,0,0)',
    font:{family:fam, color:'#e2e8f0'},
    legend:{orientation:'h', y:-.1},
    showlegend:true
  }, {displayModeBar:false, responsive:true});

  const top7C = countryArr.slice(0,7).reverse();
  Plotly.newPlot('chart-summary-markets', [{
    x: top7C.map(x=>x[1]), y: top7C.map(x=>x[0]), type:'bar', orientation:'h',
    marker:{color: top7C.map((_,i)=>['#312e81','#4338ca','#6366f1','#818cf8','#a5b4fc','#22d3ee','#67e8f9'][i%7])}
  }], {
    margin:{l:0,r:0,t:0,b:0}, height:330,
    paper_bgcolor:'rgba(0,0,0,0)', plot_bgcolor:'rgba(0,0,0,0)',
    font:{family:fam, color:'#94a3b8'},
    xaxis:{showgrid:true, gridcolor:'#273449'},
    yaxis:{showgrid:false}
  }, {displayModeBar:false, responsive:true});

  runCountUp();
}

function renderExplorer(){
  const d = state.df.slice(0,500);
  const cols = d.length ? Object.keys(d[0]) : [];
  const fmtCell = v => {
    if(v instanceof Date) return v.toISOString().slice(0,10);
    if(typeof v==='number') return isFinite(v)?v.toLocaleString('en-US',{maximumFractionDigits:2}):'';
    return String(v==null?'':v);
  };

  contentContainer.innerHTML = `
    <div class="w-full max-w-7xl mx-auto px-6">
      <div class="text-4xl font-playfair font-bold mb-2 text-white mt-2 reveal">${t('explorer_title')}</div>
      <div class="text-sm text-gray-400 mb-6">${t('explorer_subtitle')}</div>
      <div class="w-full rounded-2xl border border-gray-700 shadow-2xl overflow-auto reveal" style="max-height:600px">
        <table class="data-table">
          <thead><tr>${cols.map(c=>`<th>${c}</th>`).join('')}</tr></thead>
          <tbody>
            ${d.slice(0,200).map(r=>`<tr>${cols.map(c=>`<td>${fmtCell(r[c])}</td>`).join('')}</tr>`).join('')}
          </tbody>
        </table>
      </div>
      <div class="text-center text-xs text-gray-500 mt-3">Showing ${Math.min(200,d.length)} of ${state.df.length.toLocaleString()} rows</div>
    </div>
  `;
}

function runCountUp(){
  document.querySelectorAll('.count-up').forEach(el=>{
    if(el.dataset.counted) return;
    el.dataset.counted = '1';
    const txt = el.textContent.trim();
    const m = txt.match(/^([^\d]*)([\d,]+(?:\.(\d+))?)(.*)$/);
    if(!m) return;
    const pre = m[1], num = parseFloat(m[2].replace(/,/g,'')), dec = m[3]?m[3].length:0, suf = m[4];
    if(!isFinite(num) || num===0){ return; }
    const dur=1500, t0=performance.now();
    function step(now){
      const p = Math.min(1,(now-t0)/dur), e = 1-Math.pow(1-p,3);
      el.textContent = pre + (num*e).toLocaleString('en-US',{minimumFractionDigits:dec, maximumFractionDigits:dec}) + suf;
      if(p<1) requestAnimationFrame(step); else el.textContent = txt;
    }
    requestAnimationFrame(step);
  });
}

/* =========================================================
   8. NAVIGATION
   ========================================================= */
const NAV_ITEMS = [
  ['nav_home','home'],['nav_dashboard','dashboard'],['nav_forecasts','forecast'],
  ['nav_insights','insights'],['nav_summary','summary'],['nav_explorer','explorer']
];

function buildNav(){
  const nav = document.getElementById('nav-links');
  nav.innerHTML = NAV_ITEMS.map(([lbl,pg])=>{
    const cls = state.page===pg ? 'nav-link active' : 'nav-link';
    return `<a class="${cls}" onclick="setPage('${pg}')">${t(lbl)}</a>`;
  }).join('');
}

function setPage(p, scroll=true){
  state.page = p;
  buildNav();
  if(p==='home') renderHome();
  else if(p==='dashboard') renderDashboard();
  else if(p==='forecast') renderForecast();
  else if(p==='insights') renderInsights();
  else if(p==='summary') renderSummary();
  else if(p==='explorer') renderExplorer();
  if(scroll) window.scrollTo({top:0,behavior:'smooth'});
}

/* =========================================================
   9. LANGUAGE / UPLOAD / CHAT
   ========================================================= */
function changeLanguage(lang){
  state.language = (lang==='en'||lang==='ar') ? lang : 'en';
  document.documentElement.setAttribute('dir', state.language==='ar'?'rtl':'ltr');
  document.documentElement.setAttribute('lang', state.language);
  document.body.classList.toggle('arabic-mode', state.language==='ar');
  document.querySelectorAll('.lang-toggle button').forEach(b=>{
    b.classList.toggle('active', b.dataset.lang===state.language);
  });
  // Update chat & footer & labels
  document.getElementById('chat-title').textContent = t('chat_title');
  document.getElementById('chat-subtitle').textContent = t('chat_subtitle');
  document.getElementById('chat-input').placeholder = t('chat_placeholder');
  buildFooter();
  // Reset chat history to welcome message
  state.chatHistory = [{role:'ai', text:t('chat_welcome')}];
  renderChatMessages();
  setPage(state.page, false);
  notify(t('language_updated'),'positive');
}

/* Upload */
document.getElementById('file-uploader').addEventListener('change', async (e)=>{
  const file = e.target.files[0]; if(!file) return;
  try{
    const lower = file.name.toLowerCase();
    let wb;
    if(lower.endsWith('.csv')){
      const text = await file.text();
      wb = XLSX.read(text, {type:'string', cellDates:true});
    } else if(lower.endsWith('.xlsx') || lower.endsWith('.xls')){
      const buf = await file.arrayBuffer();
      wb = XLSX.read(buf, {type:'array', cellDates:true});
    } else {
      throw new Error(t('err_type'));
    }
    const sheet = wb.Sheets[wb.SheetNames[0]];
    const rawRows = XLSX.utils.sheet_to_json(sheet, {defval:null, raw:true, dateNF:'yyyy-mm-dd'});
    if(!rawRows.length) throw new Error(t('err_nodata'));
    const currency = detectCurrency(rawRows);
    const cleaned = cleanData(rawRows);
    if(!cleaned.length) throw new Error(t('err_norows'));
    state.df = cleaned;
    state.datasetMode = file.name;
    state.currencyCode = currency || 'USD';
    state.currencySymbol = CURRENCY_SYMBOLS[state.currencyCode] || '$';
    notify(t('upload_success',{n:cleaned.length}),'positive',4000);
    setPage('dashboard');
  }catch(ex){
    console.error(ex);
    notify(t('upload_error',{e:ex.message}),'negative',5000);
  }
  e.target.value = '';
});

/* Chat */
function toggleChat(){
  document.getElementById('chat-dialog').classList.toggle('open');
}
function renderChatMessages(){
  const c = document.getElementById('chat-messages');
  c.innerHTML = state.chatHistory.map(m=>`
    <div class="chat-message-row ${m.role==='user'?'user':''}">
      <div class="flex flex-col">
        <div class="chat-message-name">${m.role==='user'?t('chat_user'):t('chat_ai')}</div>
        <div class="chat-message ${m.role==='user'?'user':'ai'}">${m.text}</div>
      </div>
    </div>
  `).join('');
  c.scrollTop = c.scrollHeight;
}
function sendChat(){
  const inp = document.getElementById('chat-input');
  const v = inp.value.trim(); if(!v) return;
  state.chatHistory.push({role:'user', text:v});
  const res = generateBotResponse(v);
  state.chatHistory.push({role:'ai', text:res});
  inp.value = '';
  renderChatMessages();
}
function generateBotResponse(msg){
  if(!state.df || !state.df.length) return t('bot_no_data');
  const q = msg.toLowerCase();
  const df = state.df;
  let totalRev=0; df.forEach(r=>totalRev+=r.Revenue);
  const invSet = new Set(df.map(r=>r.InvoiceNo));
  const orders = invSet.size || df.length;
  const amt = money(totalRev,2);

  if(/revenue|sales|income|إيراد|ايراد|مبيعات|المبيعات|دخل/.test(q))
    return t('bot_revenue',{amt:iso(amt), orders:orders.toLocaleString()});
  if(/best|product|item|أفضل|افضل|منتج|المنتج|منتجات/.test(q)){
    const by = {}; df.forEach(r=>{by[r.Description]=(by[r.Description]||0)+r.Revenue;});
    let top='—', mx=-1; for(const k in by){ if(by[k]>mx){mx=by[k]; top=k;} }
    return t('bot_product',{name:iso(top)});
  }
  if(/market|country|region|سوق|أسواق|اسواق|دولة|الدولة|منطقة/.test(q)){
    const by = {}; df.forEach(r=>{by[r.Country]=(by[r.Country]||0)+r.Revenue;});
    let top='—', mx=-1; for(const k in by){ if(by[k]>mx){mx=by[k]; top=k;} }
    return t('bot_market',{name:iso(top)});
  }
  return t('bot_default',{amt:iso(amt)});
}

/* Footer */
function buildFooter(){
  const f = document.getElementById('main-footer');
  f.innerHTML = `<span class="font-medium">${t('footer')}</span> <span style="color:#ef4444;font-size:1.1rem">❤️</span>`;
}

/* =========================================================
   10. BACKGROUND FX (Orbs + Particles)
   ========================================================= */
(function initFx(){
  ['orb-1','orb-2','orb-3'].forEach(c=>{
    const d=document.createElement('div');
    d.className='orb '+c;
    document.body.appendChild(d);
  });
  if(window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
  const cv = document.createElement('canvas');
  cv.id='fx-canvas'; document.body.appendChild(cv);
  const ctx = cv.getContext('2d');
  let W,H,pts=[],mouse={x:-999,y:-999};
  function resize(){
    W=cv.width=window.innerWidth; H=cv.height=window.innerHeight;
    const n = Math.min(80, Math.floor(W*H/24000));
    pts = Array.from({length:n},()=>({
      x:Math.random()*W, y:Math.random()*H,
      vx:(Math.random()-.5)*.35, vy:(Math.random()-.5)*.35,
      r:Math.random()*1.6+.6
    }));
  }
  window.addEventListener('resize', resize); resize();
  window.addEventListener('mousemove', e=>{ mouse.x=e.clientX; mouse.y=e.clientY; });
  (function draw(){
    if(!document.hidden){
      ctx.clearRect(0,0,W,H);
      for(let i=0;i<pts.length;i++){
        const p=pts[i]; p.x+=p.vx; p.y+=p.vy;
        if(p.x<0||p.x>W) p.vx*=-1;
        if(p.y<0||p.y>H) p.vy*=-1;
        const dx=p.x-mouse.x, dy=p.y-mouse.y, md=Math.hypot(dx,dy);
        if(md<110){ p.x+=dx/md*.9; p.y+=dy/md*.9; }
        ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,6.283);
        ctx.fillStyle='rgba(165,180,252,.75)'; ctx.fill();
        for(let j=i+1;j<pts.length;j++){
          const q=pts[j], d=Math.hypot(p.x-q.x,p.y-q.y);
          if(d<130){
            ctx.strokeStyle='rgba(129,140,248,'+(0.18*(1-d/130))+')';
            ctx.lineWidth=1; ctx.beginPath();
            ctx.moveTo(p.x,p.y); ctx.lineTo(q.x,q.y); ctx.stroke();
          }
        }
      }
    }
    requestAnimationFrame(draw);
  })();
})();

/* =========================================================
   11. INIT
   ========================================================= */
(function init(){
  // Init state with demo data
  state.df = getDemoData();
  state.chatHistory = [{role:'ai', text:t('chat_welcome')}];
  document.querySelectorAll('.lang-toggle button').forEach(b=>{
    if(b.dataset.lang==='en') b.classList.add('active');
  });
  buildNav();
  buildFooter();
  renderChatMessages();
  setPage('home', false);
})();
</script>
</body>
</html>
