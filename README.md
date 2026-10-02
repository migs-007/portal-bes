<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>CidadãoAtivo — Denúncias Urbanas</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
/* ════════════════════════════════════════════
   VARIÁVEIS GLOBAIS
════════════════════════════════════════════ */
:root {
  --green:        #2ecc71;
  --green-dark:   #27ae60;
  --green-deep:   #1a7a45;
  --green-pale:   #d4f5e2;
  --green-faint:  #edfbf3;
  --teal:         #1abc9c;
  --blue:         #3498db;
  --amber:        #f39c12;
  --red:          #e74c3c;
  --purple:       #9b59b6;
  --bg:           #f2fdf6;
  --bg-sidebar:   #ffffff;
  --border:       rgba(39,174,96,0.18);
  --border-s:     rgba(39,174,96,0.45);
  --text-dark:    #162419;
  --text-mid:     #3a6349;
  --text-soft:    #6fa080;
  --text-faint:   #aeceba;
  --sh-sm:        0 2px 12px rgba(39,174,96,0.10);
  --sh-md:        0 8px 32px rgba(39,174,96,0.16);
  --sh-lg:        0 20px 60px rgba(39,174,96,0.22);
  --sidebar-w:    380px;
  --r:            14px;
  --ease:         cubic-bezier(.22,1,.36,1);
  --dur-fast:     .2s;
  --dur-med:      .32s;
}

/* ════════════════════════════════════════════
   RESET & BASE
════════════════════════════════════════════ */
*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
html { height:100%; }
body {
  height:100%;
  font-family:'DM Sans', sans-serif;
  background:var(--bg);
  color:var(--text-dark);
  -webkit-font-smoothing:antialiased;
  overflow:hidden;        /* Controlado por tela ativa */
}

/* ════════════════════════════════════════════
   TELA 1 — LANDING PAGE
════════════════════════════════════════════ */
#screen-menu {
  position:fixed; inset:0; z-index:500;
  display:flex; flex-direction:column;
  align-items:center; justify-content:flex-start;
  overflow-y:auto; overflow-x:hidden;
  background:
    radial-gradient(ellipse 90% 70% at 5% 15%,  rgba(46,204,113,.15) 0%,transparent 60%),
    radial-gradient(ellipse 70% 80% at 95% 85%,  rgba(26,188,156,.13) 0%,transparent 55%),
    radial-gradient(ellipse 50% 60% at 55%  5%,  rgba(52,152,219,.08) 0%,transparent 50%),
    linear-gradient(160deg, #e6f9ed 0%, #f2fdf6 45%, #eaf6ff 100%);
}

/* ── HERO (topo da landing) ── */
.hero {
  width:100%; max-width:700px;
  padding:3rem 1.5rem 2rem;
  display:flex; flex-direction:column;
  align-items:center; text-align:center;
  position:relative;
}

/* Ícones flutuantes */
.float-row {
  display:flex; gap:1rem; margin-bottom:1.8rem; justify-content:center;
}
.fi {
  width:52px; height:52px; border-radius:14px;
  display:flex; align-items:center; justify-content:center;
  font-size:1.4rem; background:white;
  box-shadow:var(--sh-md);
  animation:bobble 5.5s var(--ease) infinite;
}
.fi:nth-child(2){animation-delay:.4s}
.fi:nth-child(3){animation-delay:.8s}
.fi:nth-child(4){animation-delay:1.2s}
.fi:nth-child(5){animation-delay:1.6s}
@keyframes bobble { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-5px)} }

/* Chip de marca */
.chip {
  display:inline-flex; align-items:center; gap:.5rem;
  background:white; border:1px solid var(--border);
  border-radius:50px; padding:.4rem 1.1rem;
  font-size:.7rem; font-weight:600; letter-spacing:.12em;
  color:var(--green-dark); text-transform:uppercase;
  margin-bottom:1.1rem; box-shadow:var(--sh-sm);
}
.chip-dot {
  width:7px; height:7px; border-radius:50%;
  background:var(--green);
  animation:ripple 2s ease-in-out infinite;
}
@keyframes ripple {
  0%   { box-shadow:0 0 0 0   rgba(46,204,113,.6); }
  70%  { box-shadow:0 0 0 9px rgba(46,204,113,0); }
  100% { box-shadow:0 0 0 0   rgba(46,204,113,0); }
}

/* Título */
.hero-title {
  font-family:'Playfair Display', serif;
  font-size:clamp(2.4rem,8vw,4.2rem);
  font-weight:800; line-height:1.1;
  color:var(--text-dark); margin-bottom:.9rem;
}
.hl { color:var(--green-dark); position:relative; }
.hl::after {
  content:''; position:absolute;
  left:0; bottom:-3px;
  width:100%; height:3px;
  background:linear-gradient(90deg,var(--green),var(--teal));
  border-radius:2px;
}

/* Descrição */
.hero-desc {
  font-size:clamp(.88rem,2.5vw,1.02rem);
  color:var(--text-mid); line-height:1.8;
  max-width:520px; margin-bottom:2.2rem; font-weight:300;
}

/* Botão CTA */
.btn-cta {
  background:linear-gradient(135deg,var(--green-deep) 0%,var(--green) 55%,var(--teal) 100%);
  color:white; border:none; cursor:pointer;
  padding:1rem 2.8rem; border-radius:50px;
  font-family:'DM Sans', sans-serif;
  font-size:1.05rem; font-weight:600;
  display:inline-flex; align-items:center; gap:.65rem;
  box-shadow:0 8px 28px rgba(46,204,113,.45), 0 2px 8px rgba(0,0,0,.1);
  transition:all var(--dur-med) var(--ease);
  position:relative; overflow:hidden;
}
.btn-cta::before {
  content:''; position:absolute; inset:0;
  background:rgba(255,255,255,.15);
  opacity:0; transition:opacity var(--dur-fast) var(--ease);
}
.btn-cta:hover { transform:translateY(-2px); box-shadow:0 12px 34px rgba(46,204,113,.5); }
.btn-cta:hover::before { opacity:1; }
.btn-cta:active { transform:translateY(0); }
.btn-cta .arr { transition:transform var(--dur-fast) var(--ease); }
.btn-cta:hover .arr { transform:translateX(3px); }

/* Selos de confiança */
.trust-row {
  margin-top:1.8rem;
  display:flex; flex-wrap:wrap; justify-content:center;
  gap:.7rem 1.3rem;
  font-size:.76rem; color:var(--text-soft);
}
.trust-item { display:flex; align-items:center; gap:.35rem; }

/* ── SEÇÃO DE IMAGENS NA LANDING ── */
.landing-images {
  width:100%; max-width:900px;
  padding:0 1.5rem 1.5rem;
}

.img-section-title {
  text-align:center;
  font-size:1rem; font-weight:600;
  color:var(--text-mid); margin-bottom:1.2rem;
  display:flex; align-items:center; justify-content:center; gap:.5rem;
}
.img-section-title::before,
.img-section-title::after {
  content:''; flex:1; height:1px;
  background:var(--border); max-width:80px;
}

.img-grid {
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:.9rem;
}
.img-card {
  border-radius:var(--r); overflow:hidden;
  box-shadow:var(--sh-sm);
  transition:transform var(--dur-fast) var(--ease), box-shadow var(--dur-fast) var(--ease);
  position:relative;
  background:white;
  border:1px solid var(--border);
}
.img-card:hover { transform:translateY(-3px); box-shadow:var(--sh-md); }
.img-card img {
  width:100%; height:160px;
  object-fit:cover; display:block;
}
.img-card-label {
  padding:.65rem .8rem;
  font-size:.76rem; font-weight:600;
  color:var(--text-mid);
  display:flex; align-items:center; gap:.4rem;
  border-top:1px solid var(--border);
}

/* ── COMO FUNCIONA ── */
.how-section {
  width:100%; max-width:860px;
  padding:0 1.5rem 1.5rem;
}
.how-grid {
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
  gap:1rem;
}
.how-card {
  background:white; border:1px solid var(--border);
  border-radius:var(--r); padding:1.4rem 1.1rem;
  text-align:center; box-shadow:var(--sh-sm);
  transition:transform var(--dur-fast) var(--ease), box-shadow var(--dur-fast) var(--ease);
}
.how-card:hover { transform:translateY(-2px); box-shadow:var(--sh-md); }
.how-icon { font-size:2rem; margin-bottom:.7rem; }
.how-step {
  font-family:'DM Mono', monospace;
  font-size:.6rem; font-weight:500;
  letter-spacing:.1em; color:var(--green-dark);
  text-transform:uppercase; margin-bottom:.3rem;
}
.how-title { font-size:.9rem; font-weight:600; color:var(--text-dark); margin-bottom:.3rem; }
.how-desc  { font-size:.76rem; color:var(--text-soft); line-height:1.5; }

/* ── STATS ── */
.stats-section {
  width:100%; max-width:860px;
  padding:0 1.5rem 3rem;
}
.stats-grid {
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:1rem;
}
.stat-card {
  background:linear-gradient(135deg,var(--green-deep),var(--green-dark));
  border-radius:var(--r); padding:1.4rem 1rem;
  text-align:center; color:white;
  box-shadow:0 6px 20px rgba(39,174,96,.3);
}
.stat-val {
  font-family:'Playfair Display', serif;
  font-size:2rem; font-weight:800; line-height:1;
}
.stat-lbl { font-size:.76rem; opacity:.8; margin-top:.3rem; }

/* Animação entrada */
.anim { animation:fadeUp .55s var(--ease) both; }
.a1{animation-delay:.04s} .a2{animation-delay:.1s} .a3{animation-delay:.17s}
.a4{animation-delay:.24s} .a5{animation-delay:.31s} .a6{animation-delay:.38s}
@keyframes fadeUp {
  from { opacity:0; transform:translateY(12px); }
  to   { opacity:1; transform:translateY(0); }
}

/* ════════════════════════════════════════════
   TELA 2 — SISTEMA PRINCIPAL
════════════════════════════════════════════ */
#screen-main {
  display:none;
  position:fixed; inset:0; z-index:400;
}
#screen-main.active { display:flex; }

/* ── DESKTOP (≥ 769 px) ── */
@media (min-width:769px) {
  #screen-main.active {
    flex-direction:row;
    overflow:hidden; height:100%;
  }
  #sidebar {
    width:var(--sidebar-w); min-width:var(--sidebar-w);
    height:100%; overflow-y:auto; flex-shrink:0;
    display:flex; flex-direction:column;
  }
  #map-col {
    flex:1; position:relative;
    overflow:hidden; height:100%;
  }
  #map { width:100%; height:100%; }

  /* MIRA — position:absolute relativo a #map-col */
  .ch-line, #ch-dot { position:absolute; pointer-events:none; z-index:600; }
  #ch-h {
    width:32px; height:2px;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:rgba(39,174,96,.95);
    box-shadow:0 0 10px rgba(39,174,96,.7); border-radius:1px;
  }
  #ch-v {
    width:2px; height:32px;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:rgba(39,174,96,.95);
    box-shadow:0 0 10px rgba(39,174,96,.7); border-radius:1px;
  }
  #ch-dot {
    width:9px; height:9px; border-radius:50%;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:var(--green-dark);
    box-shadow:0 0 14px rgba(39,174,96,1),0 0 28px rgba(39,174,96,.45);
  }
  #btn-registrar {
    position:absolute; bottom:1.5rem; left:50%; transform:translateX(-50%);
    z-index:600;
  }
  #btn-registrar:hover { transform:translateX(-50%) translateY(-3px); }
}

/* ── MOBILE (≤ 768 px) ── */
@media (max-width:768px) {
  body { overflow:hidden; }

  #screen-main.active {
    flex-direction:column;
    overflow:hidden; height:100%;
  }
  #map-col {
    /* Ocupa porção fixa do alto, NÃO rola */
    flex:0 0 52vh;
    width:100%; position:relative; overflow:hidden;
  }
  #map { width:100%; height:100%; }

  /* MIRA mobile — igual desktop, relativo a #map-col */
  .ch-line, #ch-dot { position:absolute; pointer-events:none; z-index:600; }
  #ch-h {
    width:32px; height:2px;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:rgba(39,174,96,.95);
    box-shadow:0 0 10px rgba(39,174,96,.7); border-radius:1px;
  }
  #ch-v {
    width:2px; height:32px;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:rgba(39,174,96,.95);
    box-shadow:0 0 10px rgba(39,174,96,.7); border-radius:1px;
  }
  #ch-dot {
    width:9px; height:9px; border-radius:50%;
    top:50%; left:50%; transform:translate(-50%,-50%);
    background:var(--green-dark);
    box-shadow:0 0 14px rgba(39,174,96,1),0 0 28px rgba(39,174,96,.45);
  }

  /* Botão registrar mobile */
  #btn-registrar {
    position:absolute; bottom:.9rem; left:50%; transform:translateX(-50%);
    z-index:600;
  }
  #btn-registrar:hover { transform:translateX(-50%) translateY(-2px); }

  /* Sidebar ocupa o restante e rola */
  #sidebar {
    flex:1; width:100%; min-height:0;
    overflow-y:auto; -webkit-overflow-scrolling:touch;
    display:flex; flex-direction:column;
  }
}

/* ── BOTÃO REGISTRAR (partilhado) ── */
#btn-registrar {
  background:linear-gradient(135deg,var(--green-deep),var(--green));
  color:white; border:none; cursor:pointer;
  padding:.78rem 1.7rem; border-radius:50px;
  font-family:'DM Sans', sans-serif;
  font-size:.93rem; font-weight:600;
  display:flex; align-items:center; gap:.5rem;
  box-shadow:0 6px 24px rgba(39,174,96,.5), 0 2px 8px rgba(0,0,0,.15);
  transition:box-shadow .2s, transform .2s;
  white-space:nowrap; z-index:600;
}
#btn-registrar:active { transform:translateX(-50%) scale(.97) !important; }

/* Badge do mapa */
.map-badge {
  position:absolute; top:.9rem; left:50%; transform:translateX(-50%);
  z-index:600;
  background:rgba(255,255,255,.92); backdrop-filter:blur(10px);
  border:1px solid var(--border-s); border-radius:20px;
  padding:.3rem .95rem;
  font-size:.62rem; font-weight:600; letter-spacing:.1em;
  color:var(--green-deep); text-transform:uppercase;
  pointer-events:none; box-shadow:var(--sh-sm); white-space:nowrap;
}

/* Dica de uso da mira */
.map-hint {
  position:absolute; top:3rem; left:50%; transform:translateX(-50%);
  z-index:600;
  background:rgba(39,174,96,.9); color:white;
  border-radius:20px; padding:.28rem .85rem;
  font-size:.62rem; font-weight:500;
  pointer-events:none; white-space:nowrap;
  animation:fadeInOut 4s ease forwards;
}
@keyframes fadeInOut {
  0%   { opacity:0; }
  15%  { opacity:1; }
  75%  { opacity:1; }
  100% { opacity:0; }
}

/* ════════════════════════════════════════════
   SIDEBAR
════════════════════════════════════════════ */
#sidebar {
  background:var(--bg-sidebar);
  border-right:1px solid var(--border);
  box-shadow:3px 0 18px rgba(39,174,96,.07);
}

.sb-header {
  padding:1rem 1.2rem .9rem;
  border-bottom:1px solid var(--border);
  background:white; position:sticky; top:0; z-index:20;
  flex-shrink:0;
}
.sb-top {
  display:flex; align-items:center; justify-content:space-between;
  margin-bottom:.9rem;
}
.sb-brand { display:flex; align-items:center; gap:.6rem; }
.sb-mark {
  width:34px; height:34px; border-radius:10px; flex-shrink:0;
  background:linear-gradient(135deg,var(--green-deep),var(--green));
  display:flex; align-items:center; justify-content:center; font-size:1rem;
  box-shadow:0 3px 10px rgba(46,204,113,.3);
}
.sb-name { font-size:.87rem; font-weight:600; color:var(--text-dark); line-height:1.2; }
.sb-sub  { font-size:.62rem; color:var(--text-soft); margin-top:1px; }

.btn-back {
  background:var(--green-faint); border:1px solid var(--border);
  color:var(--green-dark); padding:.32rem .78rem;
  border-radius:8px; cursor:pointer;
  font-size:.73rem; font-weight:500;
  transition:all .2s; white-space:nowrap;
}
.btn-back:hover { background:var(--green-pale); }

/* Dashboard */
.dashboard {
  display:grid; grid-template-columns:repeat(3,1fr);
  gap:.55rem;
}
.dc {
  background:var(--bg); border:1px solid var(--border);
  border-radius:10px; padding:.6rem .4rem; text-align:center;
}
.dc-val {
  font-family:'DM Mono', monospace;
  font-size:1.65rem; font-weight:500; line-height:1;
}
.dc-lbl {
  font-size:.58rem; font-weight:500; color:var(--text-soft);
  letter-spacing:.07em; margin-top:.18rem; text-transform:uppercase;
}
.dc.total .dc-val { color:var(--green-dark); }
.dc.pend  .dc-val { color:var(--amber); }
.dc.res   .dc-val { color:var(--teal); }

/* Filtro de categoria */
.sb-filter {
  padding:.6rem 1.2rem .5rem;
  border-bottom:1px solid var(--border);
  background:white; position:sticky; top:0; z-index:19;
  flex-shrink:0;
}
.filter-scroll {
  display:flex; gap:.4rem; overflow-x:auto;
  padding-bottom:2px; scrollbar-width:none;
}
.filter-scroll::-webkit-scrollbar { display:none; }
.f-btn {
  flex-shrink:0; padding:.25rem .7rem;
  border-radius:20px; font-size:.68rem; font-weight:600;
  cursor:pointer; border:1.5px solid var(--border);
  background:var(--bg); color:var(--text-mid);
  transition:all .18s; white-space:nowrap;
}
.f-btn:hover { border-color:var(--green-dark); color:var(--green-dark); }
.f-btn.ativo { background:var(--green-pale); border-color:var(--green-dark); color:var(--green-deep); }

/* Feed label */
.feed-lbl {
  padding:.65rem 1.2rem .4rem;
  font-size:.63rem; font-weight:600;
  letter-spacing:.14em; color:var(--text-soft);
  text-transform:uppercase; border-bottom:1px solid var(--border);
  background:white; flex-shrink:0;
}

/* Feed list */
.feed-list {
  padding:.7rem;
  display:flex; flex-direction:column; gap:.55rem;
  min-height:60px;
}

/* Vazio */
.feed-empty { text-align:center; padding:2.5rem 1.5rem; }
.feed-empty .ei { font-size:2.6rem; display:block; margin-bottom:.8rem; }
.feed-empty p { font-size:.82rem; line-height:1.65; color:var(--text-soft); }

/* ════════════════════════════════════════════
   CARDS DO FEED
════════════════════════════════════════════ */
.den-card {
  background:white; border:1px solid var(--border);
  border-left:4px solid transparent;
  border-radius:var(--r); overflow:hidden;
  cursor:pointer;
  transition:transform var(--dur-fast) var(--ease), box-shadow var(--dur-fast) var(--ease);
  box-shadow:var(--sh-sm);
  animation:cardIn .32s var(--ease) both;
}
@keyframes cardIn {
  from { opacity:0; transform:translateY(6px); }
  to   { opacity:1; transform:translateY(0); }
}
.den-card:hover { transform:translateY(-2px); box-shadow:var(--sh-md); }

.card-img    { width:100%; height:115px; object-fit:cover; display:block; }
.card-body   { padding:.8rem .95rem; }
.card-top    { display:flex; align-items:center; justify-content:space-between; margin-bottom:.45rem; gap:.4rem; }
.cat-tag     { display:flex; align-items:center; gap:.35rem; font-size:.8rem; font-weight:600; flex-shrink:0; }
.sbadge {
  font-size:.58rem; font-weight:600;
  padding:.16rem .52rem; border-radius:20px;
  cursor:pointer; border:none;
  text-transform:uppercase;
  font-family:'DM Mono', monospace; white-space:nowrap; flex-shrink:0;
  transition:all .2s;
}
.sbadge.pendente  { background:#fff8e1; color:#b7860a; border:1px solid rgba(249,199,79,.3); }
.sbadge.andamento { background:#e3f2fd; color:#1565c0; border:1px solid rgba(41,121,255,.2); }
.sbadge.resolvido { background:#e8f5e9; color:#2e7d32; border:1px solid rgba(46,204,113,.25); }

.card-desc { font-size:.8rem; line-height:1.5; color:var(--text-mid); margin-bottom:.5rem; }
.card-ai {
  display:inline-flex; align-items:center; gap:.3rem;
  font-size:.63rem; font-weight:500;
  background:linear-gradient(90deg,var(--green-faint),#e3f2fd);
  color:var(--green-deep); padding:.13rem .55rem;
  border-radius:20px; border:1px solid var(--border); margin-bottom:.45rem;
}
.card-meta {
  display:flex; align-items:center; justify-content:space-between;
  font-size:.6rem; color:var(--text-soft);
  font-family:'DM Mono', monospace; gap:.3rem; flex-wrap:wrap;
}
.pbadge {
  font-size:.58rem; font-weight:600;
  padding:.1rem .42rem; border-radius:4px;
  text-transform:uppercase; letter-spacing:.03em; white-space:nowrap;
}
.pbadge.alta   { background:#fde8e8; color:#c0392b; }
.pbadge.normal { background:var(--green-faint); color:var(--green-deep); }
.card-coords {
  font-size:.6rem; color:var(--text-faint);
  font-family:'DM Mono', monospace; margin-top:.3rem;
}

/* ════════════════════════════════════════════
   LEAFLET OVERRIDES
════════════════════════════════════════════ */
.leaflet-container     { background:#d9f0e3; }
.leaflet-control-zoom  {
  border:1px solid var(--border) !important;
  border-radius:10px !important;
  overflow:hidden;
  box-shadow:var(--sh-sm) !important;
}
.leaflet-control-zoom a {
  background:white !important;
  color:var(--green-dark) !important;
  border-color:var(--border) !important;
}
.leaflet-control-zoom a:hover { background:var(--green-faint) !important; }
.leaflet-popup-content-wrapper {
  background:white;
  border:1px solid var(--border-s);
  border-radius:12px; box-shadow:var(--sh-md);
}
.leaflet-popup-tip  { background:white; }
.leaflet-popup-content {
  font-family:'DM Sans', sans-serif;
  font-size:.84rem; color:var(--text-dark); padding:2px 0;
}
@keyframes mPulse {
  0%   { transform:scale(.8);  opacity:1; }
  70%  { transform:scale(2.1); opacity:0; }
  100% { transform:scale(.8);  opacity:0; }
}

/* ════════════════════════════════════════════
   MODAL
════════════════════════════════════════════ */
#modal-ov, #auth-ov, #admin-ov {
  display:none; position:fixed; inset:0; z-index:1000;
  background:rgba(18,40,24,.48); backdrop-filter:blur(9px);
  align-items:center; justify-content:center; padding:1rem;
}
#modal-ov.open, #auth-ov.open, #admin-ov.open { display:flex; }

#modal, #auth-modal, #admin-modal {
  background:white; border:1px solid var(--border-s);
  border-radius:20px; width:min(480px,100%);
  padding:1.6rem;
  box-shadow:0 28px 70px rgba(39,174,96,.22), 0 4px 14px rgba(0,0,0,.1);
  animation:mIn .24s var(--ease);
  max-height:92vh; overflow-y:auto;
}
@keyframes mIn {
  from { opacity:0; transform:scale(.97) translateY(8px); }
  to   { opacity:1; transform:scale(1)   translateY(0); }
}

.modal-header {
  display:flex; align-items:center; gap:.75rem;
  margin-bottom:1.3rem; padding-bottom:.9rem;
  border-bottom:1px solid var(--border);
}
.modal-ico {
  width:44px; height:44px; border-radius:12px; flex-shrink:0;
  background:linear-gradient(135deg,var(--green-deep),var(--green));
  display:flex; align-items:center; justify-content:center; font-size:1.2rem;
  box-shadow:0 4px 14px rgba(46,204,113,.35);
}
.modal-ttl h2 { font-size:1.1rem; font-weight:700; color:var(--text-dark); }
.modal-ttl p  { font-size:.73rem; color:var(--text-soft); margin-top:.1rem; }

.fg   { margin-bottom:1.05rem; }
.flbl {
  display:block; font-size:.67rem; font-weight:600;
  letter-spacing:.1em; color:var(--text-soft);
  text-transform:uppercase; margin-bottom:.45rem;
}

/* Categorias */
.cat-grid {
  display:grid; grid-template-columns:repeat(3,1fr); gap:.45rem;
}
.cat-btn {
  background:var(--bg); border:1.5px solid var(--border);
  border-radius:10px; padding:.6rem .25rem; cursor:pointer;
  display:flex; flex-direction:column; align-items:center; gap:.28rem;
  font-size:.68rem; font-weight:500; color:var(--text-mid);
  transition:all .18s;
}
.cat-btn .ci { font-size:1.3rem; }
.cat-btn:hover { background:var(--green-faint); border-color:var(--border-s); }
.cat-btn.sel {
  border-color:var(--green-dark); background:var(--green-pale);
  color:var(--green-deep); font-weight:600;
  box-shadow:0 0 0 3px rgba(46,204,113,.15);
}

/* Upload de imagem */
.upload-zone {
  border:2px dashed var(--border-s); border-radius:12px;
  padding:1.15rem; text-align:center; cursor:pointer;
  transition:all .2s; background:var(--bg); position:relative;
}
.upload-zone:hover { border-color:var(--green-dark); background:var(--green-faint); }
.upload-zone input {
  position:absolute; inset:0; opacity:0; cursor:pointer;
  width:100%; height:100%;
}
.upload-zone .uico { font-size:1.75rem; margin-bottom:.38rem; }
.upload-zone p { font-size:.75rem; color:var(--text-soft); }
.upload-zone strong { color:var(--green-dark); }

#prev-wrap { display:none; margin-top:.65rem; position:relative; }
#prev-img  { width:100%; border-radius:9px; height:130px; object-fit:cover; border:1px solid var(--border); }
.rm-img {
  position:absolute; top:6px; right:6px;
  background:rgba(231,76,60,.85); color:white;
  border:none; border-radius:50%;
  width:24px; height:24px; cursor:pointer;
  font-size:.75rem; display:flex; align-items:center; justify-content:center;
  box-shadow:0 2px 6px rgba(0,0,0,.2);
}

#ai-box {
  display:none; margin-top:.5rem;
  background:linear-gradient(90deg,var(--green-faint),#e3f2fd);
  border:1px solid var(--border); border-radius:9px;
  padding:.6rem .8rem; font-size:.77rem; color:var(--green-deep);
  align-items:center; gap:.45rem;
}
#ai-box.show { display:flex; }

/* Textarea */
.ftxt {
  width:100%; background:var(--bg);
  border:1.5px solid var(--border); border-radius:10px;
  color:var(--text-dark); font-family:'DM Sans', sans-serif;
  font-size:.86rem; padding:.82rem;
  resize:vertical; min-height:78px;
  transition:border .2s; outline:none;
}
.ftxt:focus { border-color:var(--green-dark); background:white; }

/* Localização exibida */
#loc-display {
  display:flex; align-items:center; gap:.5rem;
  background:var(--green-faint); border:1px solid var(--border);
  border-radius:9px; padding:.6rem .8rem;
  font-family:'DM Mono', monospace; font-size:.72rem; color:var(--green-deep);
}

/* Anônimo */
.anon-row {
  display:flex; align-items:center; gap:.55rem;
  font-size:.82rem; color:var(--text-mid);
  cursor:pointer; user-select:none;
}
.anon-row input { width:16px; height:16px; accent-color:var(--green-dark); cursor:pointer; }

/* Botões do modal */
.mactions { display:flex; gap:.75rem; margin-top:1.2rem; }
.btn-ok {
  flex:1; background:linear-gradient(135deg,var(--green-deep),var(--green));
  color:white; border:none; cursor:pointer;
  padding:.84rem; border-radius:10px;
  font-family:'DM Sans', sans-serif;
  font-size:.93rem; font-weight:600;
  box-shadow:0 4px 14px rgba(39,174,96,.35);
  transition:all .2s;
}
.btn-ok:hover { filter:brightness(1.08); transform:translateY(-1px); }
.btn-no {
  padding:.84rem 1.1rem; background:transparent;
  border:1.5px solid var(--border); color:var(--text-mid);
  border-radius:10px; cursor:pointer;
  font-size:.86rem; transition:all .2s;
}
.btn-no:hover { border-color:var(--border-s); color:var(--text-dark); }

/* Toast de sucesso */
#toast {
  position:fixed; bottom:1.5rem; left:50%; transform:translateX(-50%);
  z-index:2000;
  background:var(--green-deep); color:white;
  padding:.7rem 1.4rem; border-radius:50px;
  font-size:.85rem; font-weight:500;
  box-shadow:0 8px 24px rgba(39,174,96,.4);
  display:none; align-items:center; gap:.5rem;
  animation:toastIn .26s var(--ease);
}
@keyframes toastIn { from{opacity:0;transform:translateX(-50%) translateY(6px)} to{opacity:1;transform:translateX(-50%) translateY(0)} }

/* ════════════════════════════════════════════
   AUTENTICAÇÃO — LOGIN / CRIAR CONTA
════════════════════════════════════════════ */
.landing-topbar {
  position:fixed; top:0; right:0; z-index:520;
  display:flex; align-items:center; gap:.55rem;
  padding:1rem 1.3rem; flex-wrap:wrap; justify-content:flex-end;
}
.ltb-user {
  display:flex; align-items:center; gap:.5rem;
  background:white; border:1px solid var(--border);
  border-radius:50px; padding:.3rem .3rem .3rem .9rem;
  font-size:.78rem; font-weight:600; color:var(--text-dark);
  box-shadow:var(--sh-sm);
}
.ltb-avatar {
  width:26px; height:26px; border-radius:50%; flex-shrink:0;
  background:linear-gradient(135deg,var(--green-deep),var(--green));
  color:white; display:flex; align-items:center; justify-content:center;
  font-size:.7rem; font-weight:700;
}

.auth-tabs {
  display:flex; gap:.4rem; margin-bottom:1.2rem;
  background:var(--bg); border:1px solid var(--border);
  border-radius:11px; padding:.28rem;
}
.auth-tab {
  flex:1; text-align:center; padding:.5rem; cursor:pointer;
  border:none; background:transparent; border-radius:8px;
  font-family:'DM Sans', sans-serif; font-size:.82rem; font-weight:600;
  color:var(--text-soft); transition:all .18s;
}
.auth-tab.ativo { background:white; color:var(--green-deep); box-shadow:var(--sh-sm); }

.auth-form { display:block; }
input.finp {
  width:100%; background:var(--bg);
  border:1.5px solid var(--border); border-radius:10px;
  color:var(--text-dark); font-family:'DM Sans', sans-serif;
  font-size:.86rem; padding:.72rem .82rem;
  outline:none; transition:border .2s;
}
input.finp:focus { border-color:var(--green-dark); background:white; }

.auth-erro {
  display:none; background:#fde8e8; border:1px solid rgba(231,76,60,.3);
  color:#c0392b; font-size:.78rem; font-weight:500;
  border-radius:9px; padding:.6rem .8rem; margin-bottom:1rem;
  align-items:center; gap:.4rem;
}
.auth-erro.show { display:flex; }
.mascote-mini { width:24px; height:24px; flex-shrink:0; }
.mascote-mini svg { width:100%; height:100%; display:block; }

.auth-close { width:100%; margin-top:.7rem; }

/* Badge de usuário na sidebar */
.sb-user {
  display:flex; align-items:center; gap:.55rem;
  background:var(--green-faint); border:1px solid var(--border);
  border-radius:12px; padding:.5rem .6rem; margin-bottom:.85rem;
}
.user-avatar {
  width:32px; height:32px; border-radius:50%; flex-shrink:0;
  background:linear-gradient(135deg,var(--green-deep),var(--green));
  color:white; display:flex; align-items:center; justify-content:center;
  font-size:.8rem; font-weight:700;
}
.user-info { flex:1; min-width:0; }
.user-name { font-size:.78rem; font-weight:700; color:var(--text-dark); line-height:1.2;
  white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
.user-sub  { font-size:.6rem; color:var(--text-soft); margin-top:1px; }
.btn-logout {
  background:white; border:1px solid var(--border); cursor:pointer;
  width:28px; height:28px; border-radius:8px; flex-shrink:0;
  font-size:.8rem; display:flex; align-items:center; justify-content:center;
  transition:all .18s;
}
.btn-logout:hover { background:#fde8e8; border-color:rgba(231,76,60,.3); }
.sb-guest { background:white; }
.guest-avatar { background:var(--bg); font-size:1rem; }

/* Botão de confirmação entre usuários */
.confirm-btn {
  display:inline-flex; align-items:center; gap:.3rem;
  font-size:.62rem; font-weight:600;
  padding:.2rem .55rem; border-radius:20px; cursor:pointer;
  background:var(--bg); border:1.5px solid var(--border);
  color:var(--text-mid); font-family:'DM Mono', monospace;
  transition:all .18s; white-space:nowrap;
}
.confirm-btn:hover  { border-color:var(--green-dark); color:var(--green-dark); }
.confirm-btn.ativo  { background:var(--green-pale); border-color:var(--green-dark); color:var(--green-deep); }

/* Badge de administrador */
.admin-tag {
  display:inline-flex; align-items:center;
  font-size:.56rem; font-weight:700; letter-spacing:.03em;
  background:#fff3cd; color:#8a6d00;
  border:1px solid rgba(138,109,0,.25);
  padding:.06rem .4rem; border-radius:8px; margin-left:.35rem;
  vertical-align:middle;
}

/* Nota sob o checkbox de anônimo */
.anon-note {
  font-size:.68rem; color:var(--text-soft);
  line-height:1.5; margin-top:.35rem; padding-left:1.6rem;
}
.guest-note {
  padding:.6rem .75rem; padding-left:.75rem;
  background:var(--green-faint); border:1px solid var(--border);
  border-radius:9px; margin-top:0;
}
.guest-note a { color:var(--green-deep); font-weight:700; text-decoration:underline; cursor:pointer; }

/* ════════════════════════════════════════════
   PAINEL ADMIN — IMAGENS DO SITE
════════════════════════════════════════════ */
.admin-img-grid {
  display:grid; grid-template-columns:repeat(2,1fr); gap:.7rem;
}
.admin-imgcard {
  border:1px solid var(--border); border-radius:12px;
  padding:.65rem; background:var(--bg); text-align:center;
}
.admin-imgcard-prev {
  width:100%; height:86px; border-radius:9px; overflow:hidden;
  background:white; border:1px solid var(--border);
  display:flex; align-items:center; justify-content:center;
  font-size:1.7rem; margin-bottom:.5rem;
}
.admin-imgcard-prev img { width:100%; height:100%; object-fit:cover; display:block; }
.admin-imgcard-lbl {
  font-size:.66rem; font-weight:600; color:var(--text-mid);
  margin-bottom:.5rem; line-height:1.3; min-height:2.1em;
}
.admin-imgcard-actions { display:flex; gap:.35rem; }
.btn-mini-upload, .btn-mini-reset {
  flex:1; font-size:.6rem; font-weight:600;
  padding:.36rem .2rem; border-radius:7px; cursor:pointer;
  border:1px solid var(--border); background:white; color:var(--text-mid);
  text-align:center; transition:all .15s; display:block;
}
.btn-mini-upload:hover { border-color:var(--green-dark); color:var(--green-dark); }
.btn-mini-reset:hover  { border-color:#e74c3c; color:#e74c3c; }
.admin-sec-divider { height:1px; background:var(--border); margin:1.15rem 0; }

.adm-stats-row { display:flex; gap:.6rem; margin-bottom:.9rem; }
.adm-stat {
  flex:1; text-align:center; background:var(--green-faint);
  border:1px solid var(--border); border-radius:12px; padding:.6rem .4rem;
}
.adm-stat-n {
  font-family:'DM Mono', monospace; font-size:1.25rem; font-weight:700;
  color:var(--green-deep); line-height:1;
}
.adm-stat-l {
  font-size:.6rem; color:var(--text-soft); margin-top:.25rem;
  text-transform:uppercase; letter-spacing:.03em;
}
.adm-user-row {
  display:flex; align-items:center; gap:.6rem;
  padding:.55rem .1rem; border-bottom:1px solid var(--border);
}
.adm-user-row:last-child { border-bottom:none; }
.adm-user-info { flex:1; min-width:0; }
.adm-user-nome { font-size:.78rem; font-weight:700; color:var(--text-dark); }
.adm-user-meta { font-size:.66rem; color:var(--text-soft); margin-top:1px; }
.admin-hint {
  font-size:.7rem; color:var(--text-soft); line-height:1.6;
  background:var(--green-faint); border:1px solid var(--border);
  border-radius:9px; padding:.6rem .75rem; margin-bottom:1rem;
}

/* ════════════════════════════════════════════
   MASCOTE — "Broto"
════════════════════════════════════════════ */
#mascote-wrap {
  position:fixed; left:1rem; bottom:1rem; z-index:850;
  display:flex; flex-direction:column; align-items:flex-start; gap:.55rem;
}
#mascote-avatar {
  width:54px; height:54px; border-radius:50%; flex-shrink:0;
  background:white; border:1px solid var(--border-s);
  box-shadow:var(--sh-md); padding:7px; cursor:pointer;
  display:flex; align-items:center; justify-content:center;
  transition:transform var(--dur-fast) var(--ease);
  animation:masBreath 3.8s ease-in-out infinite;
}
#mascote-avatar svg { width:100%; height:100%; display:block; }
#mascote-avatar:hover { transform:scale(1.07) rotate(-4deg); }
@keyframes masBreath { 0%,100%{ transform:translateY(0) } 50%{ transform:translateY(-3px) } }

.mascote-bubble {
  max-width:198px; background:white; border:1px solid var(--border-s);
  border-radius:14px 14px 14px 4px; padding:.6rem .55rem .6rem .8rem;
  font-size:.72rem; line-height:1.45; color:var(--text-dark);
  box-shadow:var(--sh-md); display:none; align-items:flex-start; gap:.4rem;
  opacity:0; transform:translateY(6px) scale(.96);
  transition:opacity var(--dur-med) var(--ease), transform var(--dur-med) var(--ease);
}
.mascote-bubble.show { display:flex; opacity:1; transform:translateY(0) scale(1); }
.mascote-bubble.aviso { border-color:rgba(231,76,60,.45); }
.mascote-close {
  background:none; border:none; cursor:pointer; color:var(--text-faint);
  font-size:.66rem; line-height:1; padding:2px; flex-shrink:0; margin-left:auto;
}
.mascote-close:hover { color:var(--text-soft); }

@media (max-width:480px) {
  #mascote-avatar { width:46px; height:46px; padding:6px; }
  .mascote-bubble { max-width:166px; font-size:.68rem; }
}

/* Scrollbars finas */
#sidebar::-webkit-scrollbar,
#modal::-webkit-scrollbar,
#auth-modal::-webkit-scrollbar,
#admin-modal::-webkit-scrollbar { width:4px; }
#sidebar::-webkit-scrollbar-thumb,
#modal::-webkit-scrollbar-thumb,
#auth-modal::-webkit-scrollbar-thumb,
#admin-modal::-webkit-scrollbar-thumb { background:var(--green-pale); border-radius:2px; }

/* ════════════════════════════════════════════
   RESPONSIVO EXTRA — MOBILE
════════════════════════════════════════════ */
@media (max-width:480px) {
  .hero-title { font-size:2rem; }
  .img-grid   { grid-template-columns:repeat(2,1fr); }
  .how-grid   { grid-template-columns:repeat(2,1fr); }
  .stats-grid { grid-template-columns:repeat(2,1fr); }
  .fi         { width:44px; height:44px; font-size:1.2rem; }
  .cat-grid   { grid-template-columns:repeat(3,1fr); }
  .mactions   { flex-direction:column; }
  #modal      { padding:1.2rem; }
}
</style>
</head>
<body>

<!-- Mascote "Broto" — definido uma vez, reutilizado via <use> -->
<svg style="position:absolute;width:0;height:0;overflow:hidden" aria-hidden="true">
  <symbol id="mascote-svg" viewBox="0 0 100 100">
    <ellipse cx="50" cy="59" rx="33" ry="31" fill="#2ecc71"/>
    <path d="M50,27 C40,19 28,21 26,11 C38,11 48,19 50,27Z" fill="#1a7a45"/>
    <path d="M50,27 C60,19 72,21 74,11 C62,11 52,19 50,27Z" fill="#27ae60"/>
    <ellipse cx="30" cy="63" rx="6" ry="4.3" fill="#ff9fb0" opacity=".55"/>
    <ellipse cx="70" cy="63" rx="6" ry="4.3" fill="#ff9fb0" opacity=".55"/>
    <circle cx="38" cy="57" r="6.3" fill="#fff"/>
    <circle cx="62" cy="57" r="6.3" fill="#fff"/>
    <circle cx="39.4" cy="58" r="3.1" fill="#16311f"/>
    <circle cx="63.4" cy="58" r="3.1" fill="#16311f"/>
    <circle cx="41" cy="56" r="1" fill="#fff"/>
    <circle cx="65" cy="56" r="1" fill="#fff"/>
    <path d="M42,69 Q50,76 58,69" stroke="#16311f" stroke-width="2.4" fill="none" stroke-linecap="round"/>
    <ellipse cx="35" cy="47" rx="8" ry="5" fill="#fff" opacity=".18"/>
  </symbol>
</svg>

<!-- ════════════════════════════════════════
     TELA 1 — LANDING PAGE COMPLETA
════════════════════════════════════════ -->
<div id="screen-menu">

  <!-- BARRA DE LOGIN / USUÁRIO -->
  <div class="landing-topbar">
    <div class="ltb-user" id="ltb-user" style="display:none">
      <div class="ltb-avatar" id="ltb-avatar">?</div>
      <span id="ltb-nome">—<span class="admin-tag" id="ltb-admin-tag" style="display:none">👑 ADM</span></span>
      <button class="btn-back" id="ltb-admin-btn" onclick="abrirAdminImagens()" style="display:none">🛠️ Painel Admin</button>
      <button class="btn-back" onclick="encerrarSessao()">Sair</button>
    </div>
    <button class="btn-back" id="ltb-entrar" onclick="abrirAuthModal('login')">👤 Entrar</button>
  </div>

  <!-- HERO -->
  <div class="hero">
    <div class="float-row anim a1">
      <div class="fi">🌿</div>
      <div class="fi">🗺️</div>
      <div class="fi">📍</div>
      <div class="fi">♻️</div>
      <div class="fi">🏙️</div>
    </div>

    <div class="chip anim a2">
      <div class="chip-dot"></div>
      CidadãoAtivo — Sistema de Denúncias
    </div>

    <h1 class="hero-title anim a3">
      Sua cidade<br>mais <span class="hl">limpa</span><br>e organizada
    </h1>

    <p class="hero-desc anim a4">
      Este sistema permite que qualquer cidadão registre problemas urbanos de forma rápida e prática,
      como lixo, entulho, buracos ou iluminação pública. As denúncias são marcadas diretamente no mapa
      e organizadas para facilitar a visualização e a resolução, promovendo uma cidade mais limpa,
      segura e bem cuidada.
    </p>

    <button class="btn-cta anim a5" onclick="tentarAbrirSistema()">
      <span>📢</span>
      Faça Sua Denúncia
      <span class="arr">→</span>
    </button>

    <div class="trust-row anim a6">
      <span class="trust-item">✅ Gratuito e anônimo</span>
      <span class="trust-item">📍 Localização precisa</span>
      <span class="trust-item">🔧 Acompanhe o status</span>
      <span class="trust-item">📸 Envie fotos</span>
      <span class="trust-item">🤖 IA classifica automaticamente</span>
    </div>
  </div>

  <!-- IMAGENS ILUSTRATIVAS -->
  <div class="landing-images anim a6">
    <div class="img-section-title">📸 Tipos de ocorrências mais comuns</div>
    <div class="img-grid">

      <div class="img-card" data-imgkey="lixo">
        <div class="img-card-media" id="media-lixo">
        <!-- SVG ilustrativo: Lixo urbano -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#f5f0e8"/>
          <rect x="0" y="110" width="320" height="50" fill="#c8b89a"/>
          <!-- calçada -->
          <rect x="0" y="108" width="320" height="6" fill="#b0a090"/>
          <!-- lixeira tombada -->
          <rect x="60" y="70" width="40" height="55" rx="4" fill="#607d8b" transform="rotate(-20,80,100)"/>
          <ellipse cx="80" cy="72" rx="22" ry="8" fill="#455a64" transform="rotate(-20,80,72)"/>
          <!-- lixo espalhado -->
          <circle cx="130" cy="115" r="8" fill="#8bc34a" opacity=".8"/>
          <rect x="145" y="108" width="15" height="10" rx="2" fill="#ff9800" opacity=".9"/>
          <circle cx="170" cy="118" r="6" fill="#f44336" opacity=".75"/>
          <rect x="185" y="110" width="20" height="8" rx="2" fill="#9c27b0" opacity=".7"/>
          <circle cx="215" cy="113" r="9" fill="#2196f3" opacity=".6"/>
          <!-- sacos plásticos -->
          <ellipse cx="240" cy="117" rx="18" ry="10" fill="#4caf50" opacity=".85"/>
          <ellipse cx="268" cy="112" rx="14" ry="9" fill="#ff5722" opacity=".8"/>
          <!-- chão rachado -->
          <line x1="0" y1="140" x2="80" y2="135" stroke="#a0907a" stroke-width="2"/>
          <line x1="200" y1="138" x2="320" y2="142" stroke="#a0907a" stroke-width="2"/>
          <!-- mosca -->
          <circle cx="115" cy="95" r="4" fill="#333"/>
          <ellipse cx="111" cy="93" rx="5" ry="3" fill="rgba(200,220,255,.7)"/>
          <ellipse cx="119" cy="93" rx="5" ry="3" fill="rgba(200,220,255,.7)"/>
          <!-- sol -->
          <circle cx="280" cy="28" r="18" fill="#ffd54f" opacity=".9"/>
          <line x1="280" y1="4"  x2="280" y2="0"  stroke="#ffd54f" stroke-width="2"/>
          <line x1="300" y1="10" x2="304" y2="6"  stroke="#ffd54f" stroke-width="2"/>
          <line x1="308" y1="28" x2="314" y2="28" stroke="#ffd54f" stroke-width="2"/>
          <line x1="300" y1="46" x2="304" y2="50" stroke="#ffd54f" stroke-width="2"/>
          <!-- nuvem -->
          <ellipse cx="50" cy="30" rx="30" ry="14" fill="white" opacity=".8"/>
          <ellipse cx="70" cy="24" rx="20" ry="14" fill="white" opacity=".8"/>
        </svg>
        </div>
        <div class="img-card-label">🗑️ Descarte irregular de lixo</div>
      </div>

      <div class="img-card" data-imgkey="buraco">
        <div class="img-card-media" id="media-buraco">
        <!-- SVG ilustrativo: Buraco na rua -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#e8e0d0"/>
          <!-- asfalto -->
          <rect x="0" y="60" width="320" height="100" fill="#546e7a"/>
          <!-- faixas -->
          <rect x="130" y="65" width="20" height="40" rx="2" fill="#ffffffaa"/>
          <rect x="170" y="65" width="20" height="40" rx="2" fill="#ffffffaa"/>
          <!-- buraco principal -->
          <ellipse cx="160" cy="120" rx="55" ry="28" fill="#1a1a1a"/>
          <ellipse cx="160" cy="120" rx="50" ry="24" fill="#111"/>
          <!-- bordas erodidas -->
          <path d="M108,118 Q125,100 145,112 Q158,95 175,110 Q190,100 210,115 Q218,125 210,130 Q190,145 160,148 Q130,145 108,130 Z" fill="#37474f"/>
          <!-- rachadura no asfalto -->
          <line x1="90" y1="90" x2="108" y2="118" stroke="#37474f" stroke-width="3"/>
          <line x1="108" y1="118" x2="95" y2="130" stroke="#37474f" stroke-width="2"/>
          <line x1="230" y1="88" x2="212" y2="116" stroke="#37474f" stroke-width="3"/>
          <!-- cone de sinalização -->
          <polygon points="265,90 255,140 275,140" fill="#ff5722"/>
          <rect x="250" y="140" width="30" height="5" rx="2" fill="#e64a19"/>
          <rect x="256" y="110" width="18" height="3" fill="white" opacity=".8"/>
          <rect x="258" y="120" width="14" height="3" fill="white" opacity=".8"/>
          <!-- céu -->
          <rect x="0" y="0" width="320" height="60" fill="#b3d9f0"/>
          <ellipse cx="80" cy="25" rx="40" ry="16" fill="white" opacity=".85"/>
          <ellipse cx="110" cy="18" rx="28" ry="16" fill="white" opacity=".85"/>
          <!-- predios -->
          <rect x="10" y="10" width="30" height="50" fill="#90a4ae" opacity=".7"/>
          <rect x="15" y="15" width="8" height="8" fill="#b0bec5"/>
          <rect x="27" y="15" width="8" height="8" fill="#b0bec5"/>
          <rect x="15" y="30" width="8" height="8" fill="#b0bec5"/>
        </svg>
        </div>
        <div class="img-card-label">🕳️ Buraco e pavimento danificado</div>
      </div>

      <div class="img-card" data-imgkey="iluminacao">
        <div class="img-card-media" id="media-iluminacao">
        <!-- SVG ilustrativo: Poste sem luz -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#1a237e"/>
          <!-- céu noturno -->
          <circle cx="40" cy="20" r="12" fill="#ffd54f" opacity=".9"/><!-- lua -->
          <circle cx="44" cy="16" r="10" fill="#1a237e"/><!-- sombra lua -->
          <!-- estrelas -->
          <circle cx="100" cy="15" r="1.5" fill="white" opacity=".8"/>
          <circle cx="160" cy="8" r="1.5" fill="white" opacity=".8"/>
          <circle cx="220" cy="20" r="1.5" fill="white" opacity=".8"/>
          <circle cx="280" cy="12" r="1" fill="white" opacity=".7"/>
          <circle cx="70" cy="35" r="1" fill="white" opacity=".6"/>
          <circle cx="250" cy="38" r="1" fill="white" opacity=".6"/>
          <!-- calçada noturna -->
          <rect x="0" y="110" width="320" height="50" fill="#263238"/>
          <rect x="0" y="108" width="320" height="5" fill="#1c2a30"/>
          <!-- árvores escuras -->
          <ellipse cx="40" cy="90" rx="25" ry="30" fill="#1b5e20" opacity=".7"/>
          <rect x="37" y="110" width="6" height="20" fill="#4e342e"/>
          <ellipse cx="290" cy="95" rx="22" ry="26" fill="#1b5e20" opacity=".7"/>
          <rect x="287" y="115" width="6" height="15" fill="#4e342e"/>
          <!-- poste apagado -->
          <rect x="155" y="20" width="10" height="95" fill="#78909c"/>
          <rect x="152" y="17" width="16" height="6" rx="3" fill="#90a4ae"/><!-- topo -->
          <!-- luminária apagada (cinza) -->
          <rect x="148" y="12" width="24" height="12" rx="4" fill="#455a64"/>
          <ellipse cx="160" cy="18" rx="10" ry="6" fill="#37474f"/>
          <!-- símbolo X no poste -->
          <line x1="154" y1="14" x2="166" y2="22" stroke="#ef5350" stroke-width="2.5"/>
          <line x1="166" y1="14" x2="154" y2="22" stroke="#ef5350" stroke-width="2.5"/>
          <!-- pessoas com lanterna (sugestão de escuridão) -->
          <circle cx="95" cy="105" r="7" fill="#ffb74d"/>
          <rect x="92" y="112" width="6" height="14" rx="2" fill="#1565c0"/>
          <!-- feixe de lanterna -->
          <polygon points="95,104 75,98 70,114 95,108" fill="rgba(255,235,59,.25)"/>
          <!-- outro pedestre -->
          <circle cx="230" cy="107" r="6" fill="#ef9a9a"/>
          <rect x="227" y="113" width="6" height="13" rx="2" fill="#880e4f"/>
        </svg>
        </div>
        <div class="img-card-label">💡 Iluminação pública com defeito</div>
      </div>

      <div class="img-card" data-imgkey="entulho">
        <div class="img-card-media" id="media-entulho">
        <!-- SVG ilustrativo: Entulho -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#f5f0e8"/>
          <rect x="0" y="115" width="320" height="45" fill="#c8b89a"/>
          <rect x="0" y="113" width="320" height="5" fill="#b0a090"/>
          <!-- pilha de entulho: tijolos, concreto -->
          <!-- base -->
          <rect x="50" y="100" width="220" height="20" rx="3" fill="#8d6e63"/>
          <!-- camada 2 -->
          <rect x="65" y="82"  width="185" height="22" rx="3" fill="#a1887f"/>
          <!-- tijolos individuais -->
          <rect x="75"  y="62" width="40" height="24" rx="2" fill="#c0392b"/>
          <rect x="120" y="58" width="55" height="28" rx="2" fill="#b71c1c"/>
          <rect x="180" y="65" width="48" height="21" rx="2" fill="#c0392b"/>
          <rect x="232" y="70" width="30" height="16" rx="2" fill="#e57373"/>
          <!-- placas de concreto -->
          <rect x="80"  y="46" width="70" height="20" rx="2" fill="#9e9e9e"/>
          <rect x="155" y="50" width="60" height="18" rx="2" fill="#bdbdbd"/>
          <!-- pó / areia -->
          <ellipse cx="160" cy="118" rx="120" ry="8" fill="#d4b896" opacity=".6"/>
          <!-- fita de obras -->
          <line x1="30" y1="80" x2="290" y2="80" stroke="#ffeb3b" stroke-width="4" stroke-dasharray="18,10"/>
          <!-- placa obra -->
          <rect x="270" y="50" width="36" height="30" rx="3" fill="#ff9800"/>
          <rect x="287" y="30" width="4" height="22" fill="#795548"/>
          <text x="276" y="68" font-size="8" fill="white" font-weight="bold">OBRA</text>
          <!-- céu -->
          <rect x="0" y="0" width="320" height="50" fill="#b3d9f0"/>
          <ellipse cx="60" cy="22" rx="36" ry="14" fill="white" opacity=".8"/>
        </svg>
        </div>
        <div class="img-card-label">🧱 Entulho de construção</div>
      </div>

      <div class="img-card" data-imgkey="animais">
        <div class="img-card-media" id="media-animais">
        <!-- SVG ilustrativo: Animal abandonado -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#e8f5e9"/>
          <!-- grama -->
          <rect x="0" y="110" width="320" height="50" fill="#66bb6a"/>
          <rect x="0" y="108" width="320" height="6" fill="#4caf50"/>
          <!-- graminha detalhe -->
          <line x1="20" y1="108" x2="20" y2="96" stroke="#388e3c" stroke-width="2"/>
          <line x1="35" y1="108" x2="38" y2="93" stroke="#388e3c" stroke-width="2"/>
          <line x1="290" y1="108" x2="286" y2="94" stroke="#388e3c" stroke-width="2"/>
          <line x1="305" y1="108" x2="305" y2="97" stroke="#388e3c" stroke-width="2"/>
          <!-- cachorro -->
          <!-- corpo -->
          <ellipse cx="160" cy="100" rx="45" ry="28" fill="#a1887f"/>
          <!-- cabeça -->
          <circle cx="200" cy="85" r="22" fill="#a1887f"/>
          <!-- orelhas -->
          <ellipse cx="193" cy="67" rx="9" ry="14" fill="#8d6e63" transform="rotate(-15,193,67)"/>
          <ellipse cx="215" cy="68" rx="9" ry="14" fill="#8d6e63" transform="rotate(15,215,68)"/>
          <!-- olhos -->
          <circle cx="195" cy="82" r="5" fill="#4e342e"/>
          <circle cx="210" cy="82" r="5" fill="#4e342e"/>
          <circle cx="196" cy="81" r="2" fill="white"/>
          <circle cx="211" cy="81" r="2" fill="white"/>
          <!-- nariz -->
          <ellipse cx="202" cy="91" rx="6" ry="4" fill="#5d4037"/>
          <!-- boca -->
          <path d="M197,95 Q202,101 207,95" stroke="#5d4037" stroke-width="1.5" fill="none"/>
          <!-- língua -->
          <ellipse cx="202" cy="100" rx="5" ry="4" fill="#e91e63"/>
          <!-- patas -->
          <ellipse cx="125" cy="122" rx="13" ry="9" fill="#8d6e63"/>
          <ellipse cx="150" cy="126" rx="13" ry="9" fill="#8d6e63"/>
          <ellipse cx="175" cy="126" rx="13" ry="9" fill="#8d6e63"/>
          <!-- rabo -->
          <path d="M120,95 Q90,70 100,55" stroke="#a1887f" stroke-width="10" fill="none" stroke-linecap="round"/>
          <!-- coração (amor ao animal) -->
          <path d="M240,50 C242,46 250,46 250,52 C250,46 258,46 260,50 C262,54 250,62 250,62 C250,62 238,54 240,50Z" fill="#ef5350"/>
          <!-- céu -->
          <rect x="0" y="0" width="320" height="70" fill="#b3e5fc"/>
          <ellipse cx="80" cy="25" rx="40" ry="16" fill="white" opacity=".9"/>
        </svg>
        </div>
        <div class="img-card-label">🐾 Animal abandonado / em risco</div>
      </div>

      <div class="img-card" data-imgkey="esgoto">
        <div class="img-card-media" id="media-esgoto">
        <!-- SVG ilustrativo: Esgoto / água -->
        <svg width="100%" height="160" viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg">
          <rect width="320" height="160" fill="#e3f2fd"/>
          <rect x="0" y="100" width="320" height="60" fill="#78909c"/>
          <rect x="0" y="98" width="320" height="6" fill="#607d8b"/>
          <!-- poça de água -->
          <ellipse cx="155" cy="105" rx="80" ry="20" fill="#0288d1" opacity=".55"/>
          <ellipse cx="155" cy="105" rx="60" ry="14" fill="#0277bd" opacity=".4"/>
          <!-- ondas na poça -->
          <ellipse cx="155" cy="105" rx="30" ry="6" fill="none" stroke="#4fc3f7" stroke-width="1.5" opacity=".6"/>
          <ellipse cx="155" cy="105" rx="50" ry="11" fill="none" stroke="#4fc3f7" stroke-width="1" opacity=".4"/>
          <!-- bueiro aberto -->
          <rect x="135" y="97" width="40" height="12" rx="4" fill="#37474f"/>
          <rect x="138" y="98" width="34" height="3" rx="1" fill="#263238"/>
          <rect x="138" y="103" width="34" height="3" rx="1" fill="#263238"/>
          <!-- jato de água saindo -->
          <path d="M155,97 Q160,80 158,60" stroke="#29b6f6" stroke-width="6" fill="none" stroke-linecap="round" opacity=".8"/>
          <path d="M155,97 Q148,78 152,58" stroke="#4fc3f7" stroke-width="3" fill="none" stroke-linecap="round" opacity=".6"/>
          <!-- gotas -->
          <ellipse cx="162" cy="55" rx="4" ry="6" fill="#29b6f6" opacity=".8"/>
          <ellipse cx="150" cy="52" rx="3" ry="5" fill="#4fc3f7" opacity=".7"/>
          <ellipse cx="170" cy="62" rx="3" ry="4" fill="#29b6f6" opacity=".6"/>
          <!-- calçada rachada -->
          <line x1="100" y1="110" x2="135" y2="105" stroke="#546e7a" stroke-width="2"/>
          <line x1="175" y1="105" x2="220" y2="112" stroke="#546e7a" stroke-width="2"/>
          <!-- céu -->
          <rect x="0" y="0" width="320" height="60" fill="#e1f5fe"/>
          <ellipse cx="260" cy="22" rx="38" ry="14" fill="white" opacity=".9"/>
          <ellipse cx="230" cy="15" rx="25" ry="14" fill="white" opacity=".9"/>
          <!-- predios ao fundo -->
          <rect x="20" y="20" width="25" height="80" fill="#b0bec5" opacity=".5"/>
          <rect x="50" y="30" width="20" height="70" fill="#b0bec5" opacity=".5"/>
        </svg>
        </div>
        <div class="img-card-label">💧 Esgoto e água acumulada</div>
      </div>

    </div>
  </div>

  <!-- COMO FUNCIONA -->
  <div class="how-section anim a6">
    <div class="img-section-title">🔄 Como funciona</div>
    <div class="how-grid">
      <div class="how-card">
        <div class="how-icon">🗺️</div>
        <div class="how-step">Passo 1</div>
        <div class="how-title">Abra o mapa</div>
        <div class="how-desc">Navegue até o local do problema usando o mapa interativo do Brasil.</div>
      </div>
      <div class="how-card">
        <div class="how-icon">🎯</div>
        <div class="how-step">Passo 2</div>
        <div class="how-title">Centralize a mira</div>
        <div class="how-desc">Mova o mapa até a mira verde ficar exatamente sobre o local.</div>
      </div>
      <div class="how-card">
        <div class="how-icon">📝</div>
        <div class="how-step">Passo 3</div>
        <div class="how-title">Preencha os dados</div>
        <div class="how-desc">Escolha a categoria, descreva o problema e adicione uma foto se quiser.</div>
      </div>
      <div class="how-card">
        <div class="how-icon">📍</div>
        <div class="how-step">Passo 4</div>
        <div class="how-title">Registre</div>
        <div class="how-desc">Sua denúncia aparece no mapa com marcador colorido e no painel lateral.</div>
      </div>
      <div class="how-card">
        <div class="how-icon">🔧</div>
        <div class="how-step">Passo 5</div>
        <div class="how-title">Acompanhe</div>
        <div class="how-desc">Clique no status do card para atualizar: Pendente → Em andamento → Resolvido.</div>
      </div>
    </div>
  </div>

  <!-- ESTATÍSTICAS -->
  <div class="stats-section anim a6">
    <div class="img-section-title">📊 Impacto do sistema</div>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-val">+500</div>
        <div class="stat-lbl">🏙️ Cidades atendidas</div>
      </div>
      <div class="stat-card" style="background:linear-gradient(135deg,#0e6655,#1abc9c)">
        <div class="stat-val">92%</div>
        <div class="stat-lbl">✅ Ocorrências resolvidas</div>
      </div>
      <div class="stat-card" style="background:linear-gradient(135deg,#1a5276,#3498db)">
        <div class="stat-val">24h</div>
        <div class="stat-lbl">⚡ Tempo médio de resposta</div>
      </div>
      <div class="stat-card" style="background:linear-gradient(135deg,#6c3483,#9b59b6)">
        <div class="stat-val">100%</div>
        <div class="stat-lbl">🔒 Gratuito e anônimo</div>
      </div>
    </div>
  </div>

</div><!-- /screen-menu -->

<!-- ════════════════════════════════════════
     TELA 2 — SISTEMA PRINCIPAL
════════════════════════════════════════ -->
<div id="screen-main">

  <!-- ── SIDEBAR ── -->
  <aside id="sidebar">

    <div class="sb-header">
      <div class="sb-top">
        <div class="sb-brand">
          <div class="sb-mark">🌿</div>
          <div>
            <div class="sb-name">CidadãoAtivo</div>
            <div class="sb-sub">Denúncias Urbanas</div>
          </div>
        </div>
        <button class="btn-back" onclick="voltarMenu()">← Início</button>
      </div>

      <div class="sb-user" id="sb-user" style="display:none">
        <div class="user-avatar" id="user-avatar">?</div>
        <div class="user-info">
          <div class="user-name" id="user-name">—<span class="admin-tag" id="user-admin-tag" style="display:none">👑 ADM</span></div>
          <div class="user-sub">Logado</div>
        </div>
        <button class="btn-logout" onclick="encerrarSessao()" title="Sair da conta">🚪</button>
      </div>
      <div class="sb-user sb-guest" id="sb-guest" style="display:none">
        <div class="user-avatar guest-avatar">🕶️</div>
        <div class="user-info">
          <div class="user-name">Modo visitante</div>
          <div class="user-sub">Denúncias vão como anônimas</div>
        </div>
        <button class="btn-back" onclick="abrirAuthModal('login')">Entrar</button>
      </div>
      <button class="btn-back" id="sb-admin-btn" onclick="abrirAdminImagens()"
        style="display:none;width:100%;margin-bottom:.85rem;text-align:center">🛠️ Painel Admin</button>

      <div class="dashboard">
        <div class="dc total">
          <div class="dc-val" id="d-total">0</div>
          <div class="dc-lbl">Total</div>
        </div>
        <div class="dc pend">
          <div class="dc-val" id="d-pend">0</div>
          <div class="dc-lbl">Pendentes</div>
        </div>
        <div class="dc res">
          <div class="dc-val" id="d-res">0</div>
          <div class="dc-lbl">Resolvidas</div>
        </div>
      </div>
    </div>

    <!-- Filtros rápidos -->
    <div class="sb-filter">
      <div class="filter-scroll" id="filtros">
        <button class="f-btn ativo" data-f="todos"      onclick="filtrar(this)">🗂️ Todos</button>
        <button class="f-btn"       data-f="_minhas"    onclick="filtrar(this)">👤 Minhas</button>
        <button class="f-btn"       data-f="lixo"       onclick="filtrar(this)">🗑️ Lixo</button>
        <button class="f-btn"       data-f="entulho"    onclick="filtrar(this)">🧱 Entulho</button>
        <button class="f-btn"       data-f="buraco"     onclick="filtrar(this)">🕳️ Buraco</button>
        <button class="f-btn"       data-f="iluminacao" onclick="filtrar(this)">💡 Iluminação</button>
        <button class="f-btn"       data-f="animais"    onclick="filtrar(this)">🐾 Animais</button>
        <button class="f-btn"       data-f="outro"      onclick="filtrar(this)">⚠️ Outro</button>
      </div>
    </div>

    <div class="feed-lbl">▸ Ocorrências Registradas</div>

    <div class="feed-list" id="feed-list">
      <div class="feed-empty">
        <span class="ei">🗺️</span>
        <p>Nenhuma ocorrência ainda.<br>Navegue no mapa, <strong>centralize a mira verde</strong> sobre o local e toque em <strong>Registrar no Local</strong>.</p>
      </div>
    </div>

  </aside>

  <!-- ── ÁREA DO MAPA ── -->
  <div id="map-col">
    <div class="map-badge">🇧🇷 Brasil — Mova o mapa</div>
    <div id="map"></div>

    <!-- MIRA: filhos diretos de #map-col → position:absolute funciona corretamente -->
    <div class="ch-line" id="ch-h"></div>
    <div class="ch-line" id="ch-v"></div>
    <div id="ch-dot"></div>

    <button id="btn-registrar" onclick="abrirModal()">
      <span>📍</span> Registrar no Local
    </button>
  </div>

</div><!-- /screen-main -->

<!-- ════════════════════════════════════════
     MODAL — NOVA DENÚNCIA
════════════════════════════════════════ -->
<div id="modal-ov">
  <div id="modal">

    <div class="modal-header">
      <div class="modal-ico">📌</div>
      <div class="modal-ttl">
        <h2>Nova Ocorrência</h2>
        <p>Preencha os dados e registre no mapa</p>
      </div>
    </div>

    <!-- Localização capturada -->
    <div class="fg">
      <label class="flbl">📍 Localização capturada</label>
      <div id="loc-display">
        <span>🎯</span> <span id="loc-txt">Aguardando…</span>
      </div>
    </div>

    <!-- Categoria -->
    <div class="fg">
      <label class="flbl">Categoria do Problema *</label>
      <div class="cat-grid">
        <button class="cat-btn" data-cat="lixo"       onclick="selecionarCat(this)"><span class="ci">🗑️</span><span>Lixo</span></button>
        <button class="cat-btn" data-cat="entulho"    onclick="selecionarCat(this)"><span class="ci">🧱</span><span>Entulho</span></button>
        <button class="cat-btn" data-cat="buraco"     onclick="selecionarCat(this)"><span class="ci">🕳️</span><span>Buraco</span></button>
        <button class="cat-btn" data-cat="iluminacao" onclick="selecionarCat(this)"><span class="ci">💡</span><span>Iluminação</span></button>
        <button class="cat-btn" data-cat="animais"    onclick="selecionarCat(this)"><span class="ci">🐾</span><span>Animais</span></button>
        <button class="cat-btn" data-cat="outro"      onclick="selecionarCat(this)"><span class="ci">⚠️</span><span>Outro</span></button>
      </div>
    </div>

    <!-- Foto -->
    <div class="fg">
      <label class="flbl">📸 Adicionar Foto (opcional)</label>
      <div class="upload-zone" id="upload-zone">
        <input type="file" id="img-input" accept="image/*" onchange="handleImagem(this)">
        <div class="uico">📷</div>
        <p><strong>Toque aqui para selecionar</strong></p>
        <p>JPG, PNG ou WEBP · câmera ou galeria</p>
      </div>
      <div id="prev-wrap">
        <img id="prev-img" src="" alt="Prévia da imagem">
        <button class="rm-img" onclick="removerImagem()" title="Remover foto">✕</button>
      </div>
      <div id="ai-box">
        <span>🤖</span><span id="ai-txt">Classificação automática: —</span>
      </div>
    </div>

    <!-- Descrição -->
    <div class="fg">
      <label class="flbl">Descrição do Problema *</label>
      <textarea class="ftxt" id="desc-input"
        placeholder="Ex: Há uma pilha de lixo grande na calçada há 3 dias, causando mau cheiro..."></textarea>
    </div>

    <!-- Anônimo -->
    <div class="fg" id="anon-row-logado">
      <label class="anon-row">
        <input type="checkbox" id="anon-check">
        Enviar como anônimo
      </label>
      <p class="anon-note">Seu nome não aparece para outras pessoas nem no mapa — a denúncia é publicada como "Anônimo".</p>
    </div>
    <div class="fg" id="anon-row-visitante" style="display:none">
      <p class="anon-note guest-note">
        🕶️ Você está no <strong>modo visitante</strong> — não precisa de conta para denunciar, e esta denúncia já será enviada como <strong>anônima</strong>.
        <br><a href="javascript:void(0)" onclick="irLoginDeDentroDoModal()">Entrar ou criar conta</a> se quiser acompanhar suas denúncias depois.
      </p>
    </div>

    <div class="auth-erro" id="den-aviso">
      <span class="mascote-mini"><svg viewBox="0 0 100 100"><use href="#mascote-svg"></use></svg></span>
      <span id="den-aviso-txt"></span>
    </div>

    <div class="mactions">
      <button class="btn-ok" onclick="confirmarRegistro()">Registrar Denúncia 📍</button>
      <button class="btn-no" onclick="fecharModal()">Cancelar</button>
    </div>

  </div>
</div>

<!-- ════════════════════════════════════════
     MODAL — LOGIN / CRIAR CONTA
════════════════════════════════════════ -->
<div id="auth-ov">
  <div id="auth-modal">

    <div class="modal-header">
      <div class="modal-ico">🔐</div>
      <div class="modal-ttl">
        <h2 id="auth-title">Entrar na sua conta</h2>
        <p id="auth-sub">Acesse para registrar e acompanhar denúncias</p>
      </div>
    </div>

    <div class="auth-tabs">
      <button class="auth-tab ativo" id="tab-login"    onclick="mudarAbaAuth('login')">Entrar</button>
      <button class="auth-tab"       id="tab-registro" onclick="mudarAbaAuth('registro')">Criar Conta</button>
    </div>

    <div class="auth-erro" id="auth-erro"><span>⚠️</span><span id="auth-erro-txt"></span></div>

    <!-- LOGIN -->
    <div class="auth-form" id="form-login">
      <div class="fg">
        <label class="flbl">Usuário</label>
        <input class="finp" id="log-user" placeholder="seu_usuario" autocomplete="username">
      </div>
      <div class="fg">
        <label class="flbl">Senha</label>
        <input class="finp" id="log-senha" type="password" placeholder="••••••••"
          autocomplete="current-password" onkeydown="if(event.key==='Enter') logar()">
      </div>
      <div class="mactions">
        <button class="btn-ok" onclick="logar()">Entrar 🔓</button>
      </div>
    </div>

    <!-- CRIAR CONTA -->
    <div class="auth-form" id="form-registro" style="display:none">
      <div class="fg">
        <label class="flbl">Nome completo</label>
        <input class="finp" id="reg-nome" placeholder="Seu nome">
      </div>
      <div class="fg">
        <label class="flbl">Usuário</label>
        <input class="finp" id="reg-user" placeholder="escolha_um_usuario" autocomplete="username">
      </div>
      <div class="fg">
        <label class="flbl">Senha</label>
        <input class="finp" id="reg-senha" type="password" placeholder="Mínimo 4 caracteres" autocomplete="new-password">
      </div>
      <div class="fg">
        <label class="flbl">Confirmar senha</label>
        <input class="finp" id="reg-senha2" type="password" placeholder="Repita a senha"
          autocomplete="new-password" onkeydown="if(event.key==='Enter') registrar()">
      </div>
      <div class="mactions">
        <button class="btn-ok" onclick="registrar()">Criar Conta 🎉</button>
      </div>
    </div>

    <button class="btn-no auth-close" onclick="fecharAuthModal()">Cancelar</button>

  </div>
</div>

<!-- ════════════════════════════════════════
     MODAL — ADMIN: PERSONALIZAR IMAGENS
════════════════════════════════════════ -->
<div id="admin-ov">
  <div id="admin-modal">

    <div class="modal-header">
      <div class="modal-ico">🛠️</div>
      <div class="modal-ttl">
        <h2>Painel do Administrador</h2>
        <p>Imagens do site e usuários cadastrados</p>
      </div>
    </div>

    <div class="auth-tabs">
      <button class="auth-tab ativo" id="adm-tab-imagens"  onclick="mudarAbaAdmin('imagens')">🎨 Imagens</button>
      <button class="auth-tab"       id="adm-tab-usuarios" onclick="mudarAbaAdmin('usuarios')">👥 Usuários</button>
    </div>

    <div id="adm-painel-imagens">
    <div class="admin-hint">💡 Envie uma foto para substituir o desenho de cada categoria. As imagens ficam salvas neste navegador.</div>

    <div class="admin-img-grid">

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-lixo"><span>🗑️</span></div>
        <div class="admin-imgcard-lbl">Descarte irregular de lixo</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('lixo', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('lixo')">↺ Padrão</button>
        </div>
      </div>

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-buraco"><span>🕳️</span></div>
        <div class="admin-imgcard-lbl">Buraco e pavimento danificado</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('buraco', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('buraco')">↺ Padrão</button>
        </div>
      </div>

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-iluminacao"><span>💡</span></div>
        <div class="admin-imgcard-lbl">Iluminação pública com defeito</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('iluminacao', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('iluminacao')">↺ Padrão</button>
        </div>
      </div>

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-entulho"><span>🧱</span></div>
        <div class="admin-imgcard-lbl">Entulho de construção</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('entulho', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('entulho')">↺ Padrão</button>
        </div>
      </div>

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-animais"><span>🐾</span></div>
        <div class="admin-imgcard-lbl">Animal abandonado / em risco</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('animais', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('animais')">↺ Padrão</button>
        </div>
      </div>

      <div class="admin-imgcard">
        <div class="admin-imgcard-prev" id="prev-admin-esgoto"><span>💧</span></div>
        <div class="admin-imgcard-lbl">Esgoto e água acumulada</div>
        <div class="admin-imgcard-actions">
          <label class="btn-mini-upload">📷 Trocar<input type="file" accept="image/*" style="display:none" onchange="trocarImagemSite('esgoto', this)"></label>
          <button class="btn-mini-reset" onclick="restaurarImagemSite('esgoto')">↺ Padrão</button>
        </div>
      </div>

    </div>
    </div>

    <div id="adm-painel-usuarios" style="display:none">
      <div class="adm-stats-row">
        <div class="adm-stat"><div class="adm-stat-n" id="adm-stat-usuarios">0</div><div class="adm-stat-l">Usuários</div></div>
        <div class="adm-stat"><div class="adm-stat-n" id="adm-stat-denuncias">0</div><div class="adm-stat-l">Denúncias</div></div>
        <div class="adm-stat"><div class="adm-stat-n" id="adm-stat-hoje">0</div><div class="adm-stat-l">Hoje</div></div>
      </div>
      <div class="admin-hint">👥 Ordenado por atividade mais recente. Denúncias enviadas como visitante (sem conta) não aparecem aqui.</div>
      <div id="adm-usuarios-lista"></div>
    </div>

    <div class="admin-sec-divider"></div>

    <div class="fg">
      <label class="flbl">🔑 Alterar minha senha</label>
      <input class="finp" id="adm-senha-nova" type="password" placeholder="Nova senha (mín. 4 caracteres)" style="margin-bottom:.6rem">
      <button class="btn-no" style="width:100%" onclick="trocarSenhaAdmin()">Salvar nova senha</button>
    </div>

    <button class="btn-no" style="width:100%;margin-top:.7rem" onclick="fecharAdminImagens()">Fechar</button>

  </div>
</div>

<!-- Mascote flutuante — dicas e avisos -->
<div id="mascote-wrap">
  <div class="mascote-bubble" id="mascote-bubble"></div>
  <button id="mascote-avatar" onclick="dicaAleatoria()" title="Toque para uma dica" aria-label="Mascote — dicas do site">
    <svg viewBox="0 0 100 100"><use href="#mascote-svg"></use></svg>
  </button>
</div>

<!-- Toast -->
<div id="toast">✅ Denúncia registrada com sucesso!</div>

<!-- ════════════════════════════════════════
     JAVASCRIPT
════════════════════════════════════════ -->
<script>
/* ── CONFIGURAÇÕES ── */
const CATS = {
  lixo:       { icon:'🗑️', label:'Lixo',        color:'#e67e22' },
  entulho:    { icon:'🧱', label:'Entulho',      color:'#795548' },
  buraco:     { icon:'🕳️', label:'Buraco',       color:'#f39c12' },
  iluminacao: { icon:'💡', label:'Iluminação',   color:'#e6b800' },
  animais:    { icon:'🐾', label:'Animais',      color:'#e74c3c' },
  outro:      { icon:'⚠️', label:'Outro',        color:'#9b59b6' },
};

/* Limites do Brasil */
const BRASIL_BOUNDS = [[-33.75, -73.99], [5.27, -34.79]];

/* Palavras-chave de prioridade ALTA */
const ALTA_KW = [
  'grande','perigo','grave','urgente','sério','crítico',
  'emergência','risco','bloqueio','fatal','enorme',
  'acidente','perigoso','vazamento','infestação'
];

/* Palavras-chave para classificação por nome de arquivo */
const AI_MAP = {
  lixo:       ['lixo','sujeira','sacola','lixeira','resíduo','bag','entulho_n'],
  entulho:    ['entulho','concreto','cimento','tijolo','obra','construção','debris'],
  buraco:     ['buraco','cratera','asfalto','pavimento','calçada','cava','rachado'],
  iluminacao: ['luz','poste','escuro','lamp','ilumina','noite'],
  animais:    ['animal','cachorro','gato','bicho','cao','felino','vira','dog','cat'],
};

function classificarImagem(nome) {
  const n = nome.toLowerCase();
  for (const [cat, kws] of Object.entries(AI_MAP)) {
    if (kws.some(k => n.includes(k))) return CATS[cat].label;
  }
  return 'Outros';
}

/* ── ESTADO ── */
let mapa       = null;
let denuncias  = [];
let lastId     = 0;
let catSel     = null;
let imgAtual   = null;
let filtroAtual = 'todos';
let usuarioAtual = null;   // { id, nome, user } — conta logada nesta sessão
let authDestino  = null;   // para onde ir após login/registro bem-sucedido

/* ── PERSISTÊNCIA (denúncias) ── */
function salvar() {
  try {
    localStorage.setItem('ca4_den',  JSON.stringify(denuncias));
    localStorage.setItem('ca4_lid',  String(lastId));
  } catch(e) {}
}
function carregar() {
  try {
    const d = localStorage.getItem('ca4_den');
    const i = localStorage.getItem('ca4_lid');
    if (d) denuncias = JSON.parse(d);
    if (i) lastId    = parseInt(i);
  } catch(e) {}
}

/* ════════════════════════════════════════════
   AUTENTICAÇÃO — contas de usuário
   Obs.: tudo é guardado no localStorage deste
   navegador (não há servidor). As senhas nunca
   são salvas em texto puro — apenas o hash
   SHA-256 (com salt aleatório por usuário).
════════════════════════════════════════════ */
function gerarSalt() {
  const arr = new Uint8Array(16);
  crypto.getRandomValues(arr);
  return Array.from(arr, b => b.toString(16).padStart(2,'0')).join('');
}
async function hashSenha(senha, salt) {
  const enc  = new TextEncoder().encode(salt + '::' + senha);
  const buf  = await crypto.subtle.digest('SHA-256', enc);
  return Array.from(new Uint8Array(buf), b => b.toString(16).padStart(2,'0')).join('');
}

function carregarUsuarios() {
  try {
    const u = localStorage.getItem('ca4_users');
    return u ? JSON.parse(u) : [];
  } catch(e) { return []; }
}
function salvarUsuarios(lista) {
  try { localStorage.setItem('ca4_users', JSON.stringify(lista)); } catch(e) {}
}

function carregarSessao() {
  try {
    const s = localStorage.getItem('ca4_sessao');
    if (s) usuarioAtual = JSON.parse(s);
  } catch(e) {}
}
function iniciarSessao(u) {
  usuarioAtual = { id: u.id, nome: u.nome, user: u.user, isAdmin: !!u.isAdmin };
  try { localStorage.setItem('ca4_sessao', JSON.stringify(usuarioAtual)); } catch(e) {}

  /* Registra a última atividade deste usuário (usado no painel admin) */
  const usuarios = carregarUsuarios();
  const idx = usuarios.findIndex(x => x.id === u.id);
  if (idx > -1) {
    usuarios[idx].ultimoLogin = new Date().toISOString();
    salvarUsuarios(usuarios);
  }

  atualizarUIUsuario();
}
function encerrarSessao() {
  usuarioAtual = null;
  try { localStorage.removeItem('ca4_sessao'); } catch(e) {}
  atualizarUIUsuario();
  voltarMenu();
  mostrarToast('👋 Sessão encerrada.');
}

function iniciais(nome) {
  return nome.trim().split(/\s+/).slice(0,2).map(p => p[0].toUpperCase()).join('') || '?';
}

/* ════════════════════════════════════════════
   MASCOTE "BROTO" — dicas, avisos e boas-vindas
════════════════════════════════════════════ */
let mascoteTimer = null;

const DICAS_MASCOTE = [
  'Toque em "Registrar no Local" para marcar um problema exatamente onde você está! 📍',
  'Não precisa de conta para denunciar — toda denúncia de visitante já sai anônima! 🕶️',
  'Você pode ver só as suas denúncias no filtro "👤 Minhas" (se tiver uma conta)! 🗂️',
  'Adicionar uma foto ajuda a equipe a entender melhor o problema! 📸',
  'Encontrou uma denúncia parecida? Toque em "✋ Confirmar" pra ajudar a validar! 🤝',
  'Prefere se identificar? Crie uma conta e acompanhe o status das suas denúncias! 🌱',
];

function dicaAleatoria() {
  const msg = DICAS_MASCOTE[Math.floor(Math.random() * DICAS_MASCOTE.length)];
  mostrarMascote(msg, 'dica', 6200);
}

function mostrarMascote(texto, tipo, duracao) {
  tipo = tipo || 'dica';
  duracao = duracao || 5500;
  const bubble = document.getElementById('mascote-bubble');
  bubble.innerHTML = `<span>${texto}</span><button class="mascote-close" onclick="fecharMascote()" aria-label="Fechar">✕</button>`;
  bubble.classList.toggle('aviso', tipo === 'aviso');
  bubble.classList.add('show');
  clearTimeout(mascoteTimer);
  mascoteTimer = setTimeout(fecharMascote, duracao);
}
function fecharMascote() {
  document.getElementById('mascote-bubble').classList.remove('show');
  clearTimeout(mascoteTimer);
}

/* Aviso do mascote dentro do modal de denúncia (substitui alert()) */
function mostrarAvisoDenuncia(msg) {
  document.getElementById('den-aviso-txt').textContent = msg;
  document.getElementById('den-aviso').classList.add('show');
}
function limparAvisoDenuncia() {
  document.getElementById('den-aviso').classList.remove('show');
}

/* Boas-vindas do mascote, uma única vez por navegador */
function talvezBoasVindasMascote() {
  try {
    if (localStorage.getItem('ca4_mascote_intro')) return;
    localStorage.setItem('ca4_mascote_intro', '1');
  } catch(e) {}
  setTimeout(() => {
    mostrarMascote('Oi! Eu sou o Broto 🌱 Toque em mim sempre que quiser uma dica!', 'dica', 7000);
  }, 1600);
}

function atualizarUIUsuario() {
  const logado = !!usuarioAtual;
  const admin  = logado && usuarioAtual.isAdmin;

  /* Barra da landing */
  document.getElementById('ltb-entrar').style.display = logado ? 'none' : 'inline-flex';
  document.getElementById('ltb-user').style.display    = logado ? 'flex' : 'none';
  document.getElementById('ltb-admin-btn').style.display = admin ? 'inline-flex' : 'none';
  document.getElementById('ltb-admin-tag').style.display = admin ? 'inline-flex' : 'none';
  if (logado) {
    document.getElementById('ltb-nome').firstChild.textContent = usuarioAtual.nome;
    document.getElementById('ltb-avatar').textContent = iniciais(usuarioAtual.nome);
  }

  /* Sidebar do sistema */
  const sbUser = document.getElementById('sb-user');
  if (sbUser) {
    sbUser.style.display = logado ? 'flex' : 'none';
    document.getElementById('sb-guest').style.display = logado ? 'none' : 'flex';
    document.getElementById('sb-admin-btn').style.display = admin ? 'inline-flex' : 'none';
    document.getElementById('user-admin-tag').style.display = admin ? 'inline-flex' : 'none';
    if (logado) {
      document.getElementById('user-avatar').textContent = iniciais(usuarioAtual.nome);
      document.getElementById('user-name').firstChild.textContent = usuarioAtual.nome;
    }
  }
}

/* ── MODAL DE AUTENTICAÇÃO ── */
function abrirAuthModal(aba) {
  mudarAbaAuth(aba || 'login');
  limparErroAuth();
  document.getElementById('log-user').value   = '';
  document.getElementById('log-senha').value  = '';
  document.getElementById('reg-nome').value   = '';
  document.getElementById('reg-user').value   = '';
  document.getElementById('reg-senha').value  = '';
  document.getElementById('reg-senha2').value = '';
  document.getElementById('auth-ov').classList.add('open');
}
function fecharAuthModal() {
  document.getElementById('auth-ov').classList.remove('open');
  authDestino = null;
}
function mudarAbaAuth(aba) {
  const login = aba === 'login';
  document.getElementById('tab-login').classList.toggle('ativo', login);
  document.getElementById('tab-registro').classList.toggle('ativo', !login);
  document.getElementById('form-login').style.display    = login ? 'block' : 'none';
  document.getElementById('form-registro').style.display = login ? 'none'  : 'block';
  document.getElementById('auth-title').textContent = login ? 'Entrar na sua conta' : 'Criar sua conta';
  document.getElementById('auth-sub').textContent   = login
    ? 'Acesse para registrar e acompanhar denúncias'
    : 'Leva menos de um minuto';
  limparErroAuth();
}
function mostrarErroAuth(msg) {
  const el = document.getElementById('auth-erro');
  document.getElementById('auth-erro-txt').textContent = msg;
  el.classList.add('show');
}
function limparErroAuth() {
  document.getElementById('auth-erro').classList.remove('show');
}

/* Chamado pelo botão principal da landing.
   Login NÃO é obrigatório: qualquer pessoa pode denunciar
   como visitante (a denúncia entra como anônima). */
function tentarAbrirSistema() {
  abrirSistema();
}

/* Usado dentro do modal de denúncia, quando o visitante
   decide entrar/criar conta no meio do fluxo */
function irLoginDeDentroDoModal() {
  fecharModal();
  abrirAuthModal('login');
}

async function registrar() {
  const nome   = document.getElementById('reg-nome').value.trim();
  const user   = document.getElementById('reg-user').value.trim().toLowerCase();
  const senha  = document.getElementById('reg-senha').value;
  const senha2 = document.getElementById('reg-senha2').value;

  if (!nome || !user || !senha) { mostrarErroAuth('Preencha todos os campos.'); return; }
  if (!/^[a-z0-9_.]{3,20}$/.test(user)) { mostrarErroAuth('Usuário: 3-20 caracteres (letras, números, _ ou .).'); return; }
  if (senha.length < 4) { mostrarErroAuth('A senha deve ter pelo menos 4 caracteres.'); return; }
  if (senha !== senha2) { mostrarErroAuth('As senhas não coincidem.'); return; }

  const usuarios = carregarUsuarios();
  if (usuarios.some(u => u.user === user)) { mostrarErroAuth('Esse usuário já existe. Tente entrar.'); return; }

  const salt = gerarSalt();
  const hash = await hashSenha(senha, salt);
  const novo = { id:'u'+Date.now(), nome, user, salt, hash, criadoEm:new Date().toISOString() };
  usuarios.push(novo);
  salvarUsuarios(usuarios);

  iniciarSessao(novo);
  fecharAuthModal();
  mostrarToast(`🎉 Conta criada! Bem-vindo(a), ${novo.nome}!`);
  if (authDestino === 'sistema') abrirSistema();
}

async function logar() {
  const user  = document.getElementById('log-user').value.trim().toLowerCase();
  const senha = document.getElementById('log-senha').value;
  if (!user || !senha) { mostrarErroAuth('Preencha usuário e senha.'); return; }

  const usuarios = carregarUsuarios();
  const u = usuarios.find(x => x.user === user);
  if (!u) { mostrarErroAuth('Usuário não encontrado.'); return; }

  const hash = await hashSenha(senha, u.salt);
  if (hash !== u.hash) { mostrarErroAuth('Senha incorreta.'); return; }

  iniciarSessao(u);
  fecharAuthModal();
  mostrarToast(`👋 Bem-vindo(a) de volta, ${u.nome}!`);
  if (authDestino === 'sistema') abrirSistema();
}

/* Garante que exista pelo menos uma conta ADM assim que o app carrega.
   Login padrão: usuário "admin" / senha "admin123" — troque assim que entrar! */
async function garantirContaAdmin() {
  const usuarios = carregarUsuarios();
  if (usuarios.some(u => u.isAdmin)) return;
  const salt = gerarSalt();
  const hash = await hashSenha('admin123', salt);
  usuarios.push({
    id: 'admin-' + Date.now(),
    nome: 'Administrador',
    user: 'admin',
    salt, hash,
    isAdmin: true,
    criadoEm: new Date().toISOString(),
  });
  salvarUsuarios(usuarios);
}

async function trocarSenhaAdmin() {
  if (!usuarioAtual) return;
  const campo = document.getElementById('adm-senha-nova');
  const nova  = campo.value;
  if (!nova || nova.length < 4) { mostrarToast('A senha deve ter pelo menos 4 caracteres.'); return; }

  const usuarios = carregarUsuarios();
  const u = usuarios.find(x => x.id === usuarioAtual.id);
  if (!u) return;

  const salt = gerarSalt();
  u.salt = salt;
  u.hash = await hashSenha(nova, salt);
  salvarUsuarios(usuarios);
  campo.value = '';
  mostrarToast('🔒 Senha atualizada com sucesso!');
}

/* ════════════════════════════════════════════
   PERSONALIZAÇÃO DAS IMAGENS DO SITE (admin)
   As imagens ilustrativas da landing page (seção
   "Tipos de ocorrências mais comuns") começam como
   desenhos SVG. Um administrador pode trocar cada
   uma por uma foto real, guardada no localStorage
   deste navegador, ou restaurar o desenho padrão.
════════════════════════════════════════════ */
const SITE_IMG_META = {
  lixo:       { icon:'🗑️', label:'Descarte irregular de lixo' },
  buraco:     { icon:'🕳️', label:'Buraco e pavimento danificado' },
  iluminacao: { icon:'💡', label:'Iluminação pública com defeito' },
  entulho:    { icon:'🧱', label:'Entulho de construção' },
  animais:    { icon:'🐾', label:'Animal abandonado / em risco' },
  esgoto:     { icon:'💧', label:'Esgoto e água acumulada' },
};
const SITE_IMG_KEYS = Object.keys(SITE_IMG_META);
let svgOriginais = {}; // guarda o markup SVG padrão de cada slot, na 1ª leitura

function carregarImagensSite() {
  try {
    const s = localStorage.getItem('ca4_site_imgs');
    return s ? JSON.parse(s) : {};
  } catch(e) { return {}; }
}
function salvarImagensSite(obj) {
  try { localStorage.setItem('ca4_site_imgs', JSON.stringify(obj)); } catch(e) {}
}

/* Aplica (ou reaplica) as imagens customizadas nos slots da landing */
function aplicarImagensSite() {
  const custom = carregarImagensSite();
  SITE_IMG_KEYS.forEach(key => {
    const el = document.getElementById('media-' + key);
    if (!el) return;
    if (!(key in svgOriginais)) svgOriginais[key] = el.innerHTML;
    el.innerHTML = custom[key]
      ? `<img src="${custom[key]}" alt="${SITE_IMG_META[key].label}" style="width:100%;height:160px;object-fit:cover;display:block">`
      : svgOriginais[key];
  });
}

function abrirAdminImagens() {
  if (!usuarioAtual || !usuarioAtual.isAdmin) { mostrarToast('Apenas administradores podem acessar isso.'); return; }
  atualizarPreviewsAdmin();
  mudarAbaAdmin('imagens');
  document.getElementById('admin-ov').classList.add('open');
}
function fecharAdminImagens() {
  document.getElementById('admin-ov').classList.remove('open');
}

function mudarAbaAdmin(aba) {
  const imgs = aba === 'imagens';
  document.getElementById('adm-tab-imagens').classList.toggle('ativo', imgs);
  document.getElementById('adm-tab-usuarios').classList.toggle('ativo', !imgs);
  document.getElementById('adm-painel-imagens').style.display  = imgs ? 'block' : 'none';
  document.getElementById('adm-painel-usuarios').style.display = imgs ? 'none'  : 'block';
  if (!imgs) renderAdminUsuarios();
}

function formatarDataHora(iso) {
  try {
    const d = new Date(iso);
    return d.toLocaleDateString('pt-BR',{day:'2-digit',month:'2-digit'}) + ' às ' +
           d.toLocaleTimeString('pt-BR',{hour:'2-digit',minute:'2-digit'});
  } catch(e) { return '—'; }
}

function renderAdminUsuarios() {
  const usuarios = carregarUsuarios().slice().sort((a, b) => {
    const da = a.ultimoLogin || a.criadoEm || '';
    const db = b.ultimoLogin || b.criadoEm || '';
    return db.localeCompare(da);
  });

  document.getElementById('adm-stat-usuarios').textContent  = usuarios.length;
  document.getElementById('adm-stat-denuncias').textContent = denuncias.length;
  const hoje = new Date().toLocaleDateString('pt-BR',{day:'2-digit',month:'2-digit'});
  document.getElementById('adm-stat-hoje').textContent = denuncias.filter(d => d.data === hoje).length;

  const lista = document.getElementById('adm-usuarios-lista');
  if (!usuarios.length) {
    lista.innerHTML = '<p class="admin-hint">Nenhum usuário cadastrado ainda.</p>';
    return;
  }
  lista.innerHTML = usuarios.map(u => {
    const nDen   = denuncias.filter(d => d.autorId === u.id).length;
    const ultima = u.ultimoLogin ? formatarDataHora(u.ultimoLogin) : formatarDataHora(u.criadoEm);
    return `
    <div class="adm-user-row">
      <div class="user-avatar" style="width:34px;height:34px;font-size:.72rem;flex-shrink:0">${iniciais(u.nome)}</div>
      <div class="adm-user-info">
        <div class="adm-user-nome">${u.nome}${u.isAdmin ? '<span class="admin-tag">👑 ADM</span>' : ''}</div>
        <div class="adm-user-meta">@${u.user} · ${nDen} denúncia${nDen === 1 ? '' : 's'} · última vez: ${ultima}</div>
      </div>
    </div>`;
  }).join('');
}

function atualizarPreviewsAdmin() {
  const custom = carregarImagensSite();
  SITE_IMG_KEYS.forEach(key => {
    const el = document.getElementById('prev-admin-' + key);
    if (!el) return;
    el.innerHTML = custom[key]
      ? `<img src="${custom[key]}" alt="">`
      : `<span>${SITE_IMG_META[key].icon}</span>`;
  });
}

function trocarImagemSite(key, input) {
  const file = input.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    const custom = carregarImagensSite();
    custom[key] = e.target.result;
    salvarImagensSite(custom);
    aplicarImagensSite();
    atualizarPreviewsAdmin();
    mostrarToast('🎨 Imagem atualizada!');
  };
  reader.readAsDataURL(file);
  input.value = '';
}

function restaurarImagemSite(key) {
  const custom = carregarImagensSite();
  if (!custom[key]) { mostrarToast('Essa imagem já está no padrão.'); return; }
  delete custom[key];
  salvarImagensSite(custom);
  aplicarImagensSite();
  atualizarPreviewsAdmin();
  mostrarToast('↺ Imagem padrão restaurada.');
}

/* ── NAVEGAÇÃO ── */
function abrirSistema() {
  document.getElementById('screen-menu').style.display = 'none';
  document.getElementById('screen-main').classList.add('active');

  if (!mapa) {
    /* Aguarda o layout ser calculado antes de inicializar o Leaflet */
    requestAnimationFrame(() => setTimeout(iniciarMapa, 80));
  } else {
    setTimeout(() => mapa.invalidateSize(), 120);
  }

  renderFeed();
  atualizarDash();
}

function voltarMenu() {
  document.getElementById('screen-menu').style.display = 'flex';
  document.getElementById('screen-main').classList.remove('active');
}

/* ── MAPA ── */
const BOA_ESPERANCA = [-21.9932, -48.3906];

function iniciarMapa() {
  /* Começa centralizado no Brasil enquanto tenta geolocalização */
  mapa = L.map('map', {
    center:  [-15.78, -47.93],
    zoom:    5,
    minZoom: 4,
    maxBounds: BRASIL_BOUNDS,
    maxBoundsViscosity: 0.9,
  });

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors',
    maxZoom: 19,
  }).addTo(mapa);

  mapa.zoomControl.setPosition('topright');
  denuncias.forEach(d => adicionarMarcador(d));

  /* ── GEOLOCALIZAÇÃO ── */
  atualizarBadge('📡 Obtendo sua localização…');

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      pos => {
        /* Sucesso: voa até a posição real do usuário */
        const lat = pos.coords.latitude;
        const lng = pos.coords.longitude;

        /* Verifica se está dentro dos limites do Brasil */
        const dentroNorte = lat <= 5.27   && lat >= -33.75;
        const dentroLeste = lng >= -73.99 && lng <= -34.79;
        const noBrasil    = dentroNorte && dentroLeste;

        if (noBrasil) {
          mapa.setView([lat, lng], 15, { animate: true });
          /* Marcador azul da posição atual */
          L.circleMarker([lat, lng], {
            radius:      10,
            color:       '#3498db',
            fillColor:   '#3498db',
            fillOpacity: 0.35,
            weight:      3,
          }).addTo(mapa)
            .bindPopup('<b>📍 Você está aqui</b>')
            .openPopup();
          atualizarBadge('📍 Sua localização');
        } else {
          /* Fora do Brasil: usa fallback */
          usarFallback();
        }
      },
      _err => {
        /* Permissão negada ou erro: usa Boa Esperança do Sul */
        usarFallback();
      },
      { timeout: 8000, maximumAge: 60000, enableHighAccuracy: true }
    );
  } else {
    /* Navegador sem suporte a geolocalização */
    usarFallback();
  }
}

function usarFallback() {
  mapa.setView(BOA_ESPERANCA, 15, { animate: true });
  atualizarBadge('📍 Boa Esperança do Sul — SP');
}

function atualizarBadge(texto) {
  const b = document.querySelector('.map-badge');
  if (b) b.textContent = texto;
}

function criarIcone(cat) {
  const c = CATS[cat] || CATS['outro'];
  return L.divIcon({
    html: `
      <div style="position:relative;width:40px;height:40px">
        <div style="
          position:absolute;inset:0;border-radius:50%;
          background:${c.color};opacity:.22;
          animation:mPulse 2.5s ease-in-out infinite">
        </div>
        <div style="
          position:absolute;inset:5px;border-radius:50%;
          background:${c.color};
          display:flex;align-items:center;justify-content:center;
          font-size:15px;
          box-shadow:0 2px 12px ${c.color}99;
          border:2.5px solid white">
          ${c.icon}
        </div>
      </div>`,
    className: '',
    iconSize:    [40, 40],
    iconAnchor:  [20, 20],
    popupAnchor: [0, -22],
  });
}

function adicionarMarcador(d) {
  const c = CATS[d.cat] || CATS['outro'];
  const imgHtml = d.img
    ? `<img src="${d.img}" style="width:100%;height:90px;object-fit:cover;border-radius:7px;margin-bottom:6px;border:1px solid #ddd">`
    : '';
  L.marker([d.lat, d.lng], { icon: criarIcone(d.cat) })
    .addTo(mapa)
    .bindPopup(`
      <div style="min-width:170px;font-family:'DM Sans',sans-serif">
        ${imgHtml}
        <div style="font-weight:700;color:${c.color};margin-bottom:3px;font-size:.9rem">${c.icon} ${c.label}</div>
        <div style="font-size:.8rem;color:#444;margin-bottom:5px;line-height:1.4">${d.desc}</div>
        <div style="font-size:.68rem;color:#888">${d.autor} · ${d.hora}</div>
        <div style="font-size:.62rem;color:#aaa;margin-top:2px">${d.lat.toFixed(4)}, ${d.lng.toFixed(4)}</div>
      </div>`);
}

/* ── IMAGEM ── */
function handleImagem(input) {
  const file = input.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    imgAtual = e.target.result;
    document.getElementById('prev-img').src  = imgAtual;
    document.getElementById('prev-wrap').style.display = 'block';
    document.getElementById('upload-zone').style.display = 'none';
    const cls = classificarImagem(file.name);
    document.getElementById('ai-txt').textContent = `Classificação automática: ${cls}`;
    document.getElementById('ai-box').classList.add('show');
  };
  reader.readAsDataURL(file);
}
function removerImagem() {
  imgAtual = null;
  document.getElementById('img-input').value = '';
  document.getElementById('prev-wrap').style.display  = 'none';
  document.getElementById('upload-zone').style.display = 'block';
  document.getElementById('ai-box').classList.remove('show');
}

/* ── MODAL ── */
function abrirModal() {
  catSel   = null;
  imgAtual = null;
  document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('sel'));
  document.getElementById('desc-input').value    = '';
  document.getElementById('anon-check').checked  = false;
  removerImagem();
  limparAvisoDenuncia();

  /* Visitante (sem conta) sempre envia como anônimo */
  document.getElementById('anon-row-logado').style.display    = usuarioAtual ? 'block' : 'none';
  document.getElementById('anon-row-visitante').style.display = usuarioAtual ? 'none'  : 'block';

  /* Mostra coordenadas capturadas */
  if (mapa) {
    const c = mapa.getCenter();
    document.getElementById('loc-txt').textContent =
      `${c.lat.toFixed(5)}, ${c.lng.toFixed(5)}`;
  }

  document.getElementById('modal-ov').classList.add('open');
}
function fecharModal() {
  document.getElementById('modal-ov').classList.remove('open');
}
function selecionarCat(el) {
  document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('sel'));
  el.classList.add('sel');
  catSel = el.dataset.cat;
}

function confirmarRegistro() {
  if (!catSel) { mostrarAvisoDenuncia('Selecione uma categoria antes de continuar!'); return; }
  const desc = document.getElementById('desc-input').value.trim();
  if (!desc)  { mostrarAvisoDenuncia('Escreva uma breve descrição do problema!'); return; }

  /* Visitante (sem conta) sempre denuncia como anônimo */
  const anon   = usuarioAtual ? document.getElementById('anon-check').checked : true;
  const center = mapa.getCenter();
  const lc     = desc.toLowerCase();
  const alta   = ALTA_KW.some(w => lc.includes(w));
  const aiCls  = imgAtual
    ? document.getElementById('ai-txt').textContent.replace('Classificação automática: ','')
    : null;

  lastId++;
  const d = {
    id:        lastId,
    cat:       catSel,
    desc,
    lat:       center.lat,
    lng:       center.lng,
    autor:     (usuarioAtual && !anon) ? usuarioAtual.nome : 'Anônimo',
    autorId:   usuarioAtual ? usuarioAtual.id : null,
    anonimo:   anon,
    hora:      new Date().toLocaleTimeString('pt-BR',{hour:'2-digit',minute:'2-digit'}),
    data:      new Date().toLocaleDateString('pt-BR',{day:'2-digit',month:'2-digit'}),
    status:    'pendente',
    prioridade: alta ? 'alta' : 'normal',
    img:       imgAtual,
    aiCls,
    confirmacoes: [],
  };

  denuncias.unshift(d);
  salvar();
  adicionarMarcador(d);
  mapa.panTo([d.lat, d.lng], { animate:true, duration:.8 });
  fecharModal();
  renderFeed();
  atualizarDash();
  mostrarToast('✅ Denúncia registrada com sucesso!');
}

/* ── TOAST ── */
function mostrarToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.display = 'flex';
  setTimeout(() => { t.style.display = 'none'; }, 3000);
}

/* ── FEED & STATUS ── */
const CICLO  = { pendente:'andamento', andamento:'resolvido', resolvido:'pendente' };
const SLABEL = {
  pendente:  '⏳ Pendente',
  andamento: '🔧 Em andamento',
  resolvido: '✅ Resolvido',
};

function ciclarStatus(id) {
  const d = denuncias.find(x => x.id === id);
  if (!d) return;
  d.status = CICLO[d.status];
  salvar();
  renderFeed();
  atualizarDash();
  mostrarToast('Status atualizado!');
}

function focarDenuncia(id) {
  const d = denuncias.find(x => x.id === id);
  if (!d || !mapa) return;
  mapa.setView([d.lat, d.lng], 16, { animate:true });
  /* No mobile, rola para o topo (onde fica o mapa) */
  if (window.innerWidth <= 768) {
    document.getElementById('map-col').scrollIntoView({ behavior:'smooth' });
  }
}

function filtrar(el) {
  document.querySelectorAll('.f-btn').forEach(b => b.classList.remove('ativo'));
  el.classList.add('ativo');
  filtroAtual = el.dataset.f;
  renderFeed();
}

function renderFeed() {
  const list = document.getElementById('feed-list');

  const lista = filtroAtual === 'todos'   ? denuncias
    : filtroAtual === '_minhas'           ? denuncias.filter(d => usuarioAtual && d.autorId === usuarioAtual.id)
    : denuncias.filter(d => d.cat === filtroAtual);

  if (!lista.length) {
    const msgMinhas = usuarioAtual
      ? 'Você ainda não registrou nenhuma denúncia.<br>Toque em <strong>Registrar no Local</strong> para começar.'
      : 'Entre na sua conta para ver suas denúncias aqui.<br><a href="javascript:void(0)" onclick="abrirAuthModal(\'login\')" style="color:var(--green-deep);font-weight:700">Entrar ou criar conta</a>';
    list.innerHTML = `<div class="feed-empty">
      <span class="ei">${filtroAtual === 'todos' ? '🗺️' : filtroAtual === '_minhas' ? '👤' : CATS[filtroAtual]?.icon || '🔍'}</span>
      <p>${filtroAtual === 'todos'
        ? 'Nenhuma ocorrência ainda.<br>Navegue no mapa, <strong>centralize a mira verde</strong> sobre o local e toque em <strong>Registrar no Local</strong>.'
        : filtroAtual === '_minhas'
        ? msgMinhas
        : `Nenhuma ocorrência de <strong>${CATS[filtroAtual]?.label || filtroAtual}</strong> registrada.`
      }</p>
    </div>`;
    return;
  }

  list.innerHTML = lista.map(d => {
    const c = CATS[d.cat] || CATS['outro'];
    const imgHtml = d.img
      ? `<img class="card-img" src="${d.img}" alt="Foto da ocorrência">`
      : '';
    const aiHtml = d.aiCls
      ? `<div class="card-ai">🤖 ${d.aiCls}</div>`
      : '';
    const nConf = (d.confirmacoes || []).length;
    const jaConfirmou = !!(usuarioAtual && (d.confirmacoes || []).includes(usuarioAtual.id));
    const podeConfirmar = !usuarioAtual || d.autorId !== usuarioAtual.id;
    const confirmHtml = podeConfirmar
      ? `<button class="confirm-btn ${jaConfirmou ? 'ativo' : ''}"
           onclick="event.stopPropagation(); confirmarOcorrencia(${d.id})"
           title="${usuarioAtual ? 'Confirmar que também viu esse problema' : 'Entre para confirmar'}">
           ✋ ${nConf}
         </button>`
      : nConf > 0
      ? `<span class="confirm-btn">✋ ${nConf}</span>`
      : '';
    return `
    <div class="den-card" style="border-left-color:${c.color}" onclick="focarDenuncia(${d.id})">
      ${imgHtml}
      <div class="card-body">
        <div class="card-top">
          <div class="cat-tag" style="color:${c.color}">${c.icon} ${c.label}</div>
          <button
            class="sbadge ${d.status}"
            onclick="event.stopPropagation(); ciclarStatus(${d.id})"
            title="Clique para avançar o status">
            ${SLABEL[d.status]}
          </button>
        </div>
        ${aiHtml}
        <div class="card-desc">${d.desc}</div>
        <div class="card-meta">
          <span>👤 ${d.autor} · 📅 ${d.data || ''} ${d.hora}</span>
          <span class="pbadge ${d.prioridade}">
            ${d.prioridade === 'alta' ? '🔴 Alta' : '🟢 Normal'}
          </span>
        </div>
        <div class="card-meta" style="margin-top:.35rem">
          <span class="card-coords" style="margin-top:0">📍 ${d.lat.toFixed(4)}, ${d.lng.toFixed(4)}</span>
          ${confirmHtml}
        </div>
      </div>
    </div>`;
  }).join('');
}

/* ── ATIVIDADE ENTRE USUÁRIOS ── */
function confirmarOcorrencia(id) {
  if (!usuarioAtual) {
    mostrarMascote('Entre na sua conta para confirmar que também viu isso! 🤝', 'dica', 6000);
    abrirAuthModal('login');
    return;
  }
  const d = denuncias.find(x => x.id === id);
  if (!d) return;
  if (d.autorId === usuarioAtual.id) { mostrarToast('Você não pode confirmar sua própria denúncia.'); return; }

  if (!d.confirmacoes) d.confirmacoes = [];
  const idx = d.confirmacoes.indexOf(usuarioAtual.id);
  if (idx === -1) {
    d.confirmacoes.push(usuarioAtual.id);
    mostrarToast('✋ Confirmado — obrigado por ajudar!');
  } else {
    d.confirmacoes.splice(idx, 1);
  }
  salvar();
  renderFeed();
}

function atualizarDash() {
  document.getElementById('d-total').textContent = denuncias.length;
  document.getElementById('d-pend').textContent  = denuncias.filter(d => d.status === 'pendente').length;
  document.getElementById('d-res').textContent   = denuncias.filter(d => d.status === 'resolvido').length;
}

/* Fechar modal clicando fora */
document.getElementById('modal-ov').addEventListener('click', e => {
  if (e.target === document.getElementById('modal-ov')) fecharModal();
});
document.getElementById('auth-ov').addEventListener('click', e => {
  if (e.target === document.getElementById('auth-ov')) fecharAuthModal();
});
document.getElementById('admin-ov').addEventListener('click', e => {
  if (e.target === document.getElementById('admin-ov')) fecharAdminImagens();
});

/* ── INICIALIZAÇÃO ── */
carregar();
carregarSessao();
atualizarUIUsuario();
aplicarImagensSite();
garantirContaAdmin();
talvezBoasVindasMascote();
</script>
</body>
</html>
