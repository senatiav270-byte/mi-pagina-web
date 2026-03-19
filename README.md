<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Vicente Silvestre Jop</title>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Bebas+Neue&family=IBM+Plex+Mono:wght@300;400;500;700&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:      #020407;
      --panel:   #060c10;
      --card:    #091118;
      --line:    #0d2030;
      --neon:    #00ff88;
      --neon2:   #00cfff;
      --orange:  #ff7b35;
      --text:    #c8dce8;
      --dim:     #304a5c;
      --font:    'Space Grotesk', sans-serif;
      --display: 'Bebas Neue', cursive;
      --mono:    'IBM Plex Mono', monospace;
      --radius:  6px;
    }

    html, body { height: 100%; overflow: hidden; }
    body { background: var(--bg); color: var(--text); font-family: var(--font); cursor: none; }

    /* CURSOR */
    #cursor {
      position: fixed; width: 10px; height: 10px; border-radius: 50%;
      background: var(--neon); pointer-events: none; z-index: 9999;
      transform: translate(-50%,-50%); mix-blend-mode: exclusion;
    }
    #cursor-ring {
      position: fixed; width: 36px; height: 36px; border-radius: 50%;
      border: 1px solid rgba(0,255,136,0.4); pointer-events: none; z-index: 9998;
      transform: translate(-50%,-50%);
      transition: left 0.12s ease, top 0.12s ease;
    }

    /* SCANLINES */
    body::after {
      content: ''; position: fixed; inset: 0; z-index: 9990; pointer-events: none;
      background: repeating-linear-gradient(0deg, transparent, transparent 3px, rgba(0,0,0,0.04) 3px, rgba(0,0,0,0.04) 4px);
    }

    /* HORIZONTAL TRACK */
    .scroll-track {
      display: flex; width: 500vw; height: 100vh;
      transition: transform 0.7s cubic-bezier(0.77,0,0.18,1);
    }
    .slide {
      width: 100vw; height: 100vh; flex-shrink: 0;
      display: flex; flex-direction: column; justify-content: center; align-items: center;
      padding: 4.5rem 4vw 2rem; position: relative; overflow: hidden;
    }
    .slide::before {
      content: attr(data-num); position: absolute;
      font-family: var(--display); font-size: 30vw;
      color: rgba(0,255,136,0.025); right: -2vw; bottom: -4vh;
      line-height: 1; pointer-events: none; user-select: none;
    }

    /* TOP BAR */
    .topbar {
      position: fixed; top: 0; left: 0; right: 0; z-index: 500;
      height: 52px; display: flex; align-items: center; justify-content: space-between;
      padding: 0 2.5rem; border-bottom: 1px solid var(--line);
      background: rgba(2,4,7,0.95); backdrop-filter: blur(12px);
    }
    .topbar-id { display: flex; align-items: center; gap: 0.8rem; }
    .tb-badge {
      font-family: var(--display); font-size: 1.05rem; letter-spacing: 0.08em;
      color: var(--bg); background: var(--neon); padding: 0.15rem 0.6rem; border-radius: 3px;
    }
    .tb-name { font-family: var(--mono); font-size: 0.72rem; color: var(--dim); letter-spacing: 0.1em; }
    .blink { animation: blink 1s step-end infinite; }
    @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

    .topbar-nav { display: flex; }
    .tnav {
      font-family: var(--mono); font-size: 0.65rem; color: var(--dim);
      text-decoration: none; letter-spacing: 0.12em;
      padding: 0 1.1rem; height: 52px;
      display: flex; align-items: center; gap: 0.4rem;
      border-left: 1px solid var(--line); transition: all 0.2s; cursor: none;
    }
    .tnav:hover, .tnav.on { color: var(--neon); background: rgba(0,255,136,0.04); }
    .tn-num { color: rgba(0,255,136,0.3); font-size: 0.55rem; }
    .tnav.on .tn-num { color: var(--neon); }
    .topbar-progress { width: 80px; height: 2px; background: var(--line); border-radius: 2px; overflow: hidden; }
    .progress-fill { height: 100%; background: var(--neon); border-radius: 2px; transition: width 0.6s ease; }

    /* SIDE DOTS */
    .side-dots {
      position: fixed; right: 1.8rem; top: 50%; transform: translateY(-50%);
      z-index: 600; display: flex; flex-direction: column; gap: 0.6rem;
    }
    .sd {
      width: 4px; height: 4px; border-radius: 50%; background: var(--line);
      cursor: none; transition: all 0.3s; position: relative;
    }
    .sd::before {
      content: attr(data-label); position: absolute; right: 1.2rem; top: 50%;
      transform: translateY(-50%); font-family: var(--mono); font-size: 0.55rem;
      letter-spacing: 0.12em; color: var(--neon); white-space: nowrap;
      opacity: 0; transition: opacity 0.2s; pointer-events: none;
    }
    .sd:hover::before, .sd.on::before { opacity: 1; }
    .sd.on { background: var(--neon); box-shadow: 0 0 10px var(--neon); height: 20px; border-radius: 2px; }

    /* SECTION LABEL */
    .s-label {
      font-family: var(--mono); font-size: 0.6rem; color: var(--neon);
      letter-spacing: 0.25em; display: flex; align-items: center; gap: 0.7rem; margin-bottom: 1.5rem;
    }
    .s-label::before { content: ''; width: 20px; height: 1px; background: var(--neon); }

    /* ====== SLIDE 0 — HERO ====== */
    .hero-layout { width: 100%; max-width: 1100px; }
    .hero-top { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 3rem; }
    .hero-label {
      font-family: var(--mono); font-size: 0.62rem; color: var(--neon);
      letter-spacing: 0.25em; display: flex; align-items: center; gap: 0.6rem;
    }
    .hero-label::before { content: ''; width: 24px; height: 1px; background: var(--neon); }
    .hero-status { font-family: var(--mono); font-size: 0.62rem; color: var(--dim); display: flex; align-items: center; gap: 0.6rem; }
    .live-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--neon); animation: livepulse 1.5s ease-in-out infinite; }
    @keyframes livepulse { 0%,100%{box-shadow:0 0 0 0 rgba(0,255,136,0.6)} 50%{box-shadow:0 0 0 6px rgba(0,255,136,0)} }
    .hero-name { font-family: var(--display); font-size: clamp(5rem,11vw,9rem); line-height: 0.92; letter-spacing: 0.02em; margin-bottom: 0.6rem; }
    .hero-name .w1 { color: var(--text); display: block; }
    .hero-name .w2 { color: transparent; -webkit-text-stroke: 1.5px var(--neon); display: block; }
    .hero-name .w3 { color: var(--neon2); font-size: 0.55em; display: block; letter-spacing: 0.05em; }
    .hero-bottom { display: flex; align-items: flex-end; justify-content: space-between; margin-top: 2.5rem; }
    .hero-desc { max-width: 400px; font-size: 0.9rem; color: var(--dim); line-height: 1.75; border-left: 2px solid var(--line); padding-left: 1.2rem; }
    .hero-desc strong { color: var(--neon); font-weight: 600; }
    .hero-ctas { display: flex; gap: 0.7rem; }
    .cta-prim {
      font-family: var(--mono); font-size: 0.72rem; letter-spacing: 0.12em;
      background: var(--neon); color: var(--bg); border: none;
      padding: 0.75rem 1.6rem; border-radius: var(--radius); font-weight: 700; cursor: none; transition: all 0.2s;
    }
    .cta-prim:hover { box-shadow: 0 0 30px rgba(0,255,136,0.4); transform: translateY(-2px); }
    .cta-sec {
      font-family: var(--mono); font-size: 0.72rem; letter-spacing: 0.12em;
      background: transparent; color: var(--dim); border: 1px solid var(--line);
      padding: 0.75rem 1.6rem; border-radius: var(--radius); cursor: none; transition: all 0.2s;
    }
    .cta-sec:hover { color: var(--text); border-color: var(--dim); }

    /* ====== SLIDE 1 — SOBRE ====== */
    .sobre-layout { width: 100%; max-width: 1100px; display: grid; grid-template-columns: 1fr 1.4fr; gap: 5rem; align-items: center; }
    .slide-title { font-family: var(--display); font-size: clamp(3.5rem,6vw,5.5rem); line-height: 0.95; letter-spacing: 0.02em; margin-bottom: 2rem; }
    .slide-title .accent { color: var(--neon); }
    .slide-title .accent2 { color: var(--neon2); }
    .skills-wrap { display: grid; grid-template-columns: 1fr 1fr; gap: 0.45rem; }
    .skill-tag {
      font-family: var(--mono); font-size: 0.68rem; letter-spacing: 0.08em;
      padding: 0.6rem 0.9rem; border-radius: var(--radius);
      background: var(--card); border: 1px solid var(--line); color: var(--dim);
      display: flex; align-items: center; gap: 0.5rem; transition: all 0.2s;
    }
    .skill-tag:hover { color: var(--neon); border-color: rgba(0,255,136,0.25); }
    .skill-tag .dot { width: 4px; height: 4px; border-radius: 50%; background: var(--neon); flex-shrink: 0; }
    .bio-text { font-size: 0.9rem; color: var(--dim); line-height: 1.85; margin-bottom: 1.2rem; }
    .bio-text strong { color: var(--text); font-weight: 600; }
    .stats-row { display: flex; gap: 1.5rem; margin-top: 2rem; }
    .stat-box { text-align: center; }
    .stat-num { font-family: var(--display); font-size: 2.8rem; color: var(--neon); line-height: 1; }
    .stat-lbl { font-family: var(--mono); font-size: 0.58rem; color: var(--dim); letter-spacing: 0.15em; margin-top: 0.3rem; }
    .divider-v { width: 1px; background: var(--line); }

    /* ====== SLIDE 2 — PROYECTOS ====== */
    .proj-layout { width: 100%; max-width: 1100px; }
    .proj-header { display: flex; align-items: flex-end; justify-content: space-between; margin-bottom: 2rem; }
    .proj-card { background: var(--panel); border: 1px solid var(--line); border-radius: 12px; overflow: hidden; display: grid; grid-template-columns: 1fr 1fr; }
    .pc-left { padding: 2.5rem; border-right: 1px solid var(--line); }
    .pc-eyebrow { font-family: var(--mono); font-size: 0.6rem; color: var(--neon); letter-spacing: 0.2em; margin-bottom: 1rem; display: flex; align-items: center; gap: 0.6rem; }
    .pc-eyebrow::before { content: '//'; color: var(--dim); }
    .pc-title { font-family: var(--display); font-size: 2.8rem; line-height: 1; letter-spacing: 0.02em; margin-bottom: 1rem; color: var(--text); }
    .pc-title span { color: var(--neon); }
    .pc-desc { font-size: 0.85rem; color: var(--dim); line-height: 1.8; margin-bottom: 1.5rem; }
    .pc-desc strong { color: var(--text); }
    .pc-tags { display: flex; flex-wrap: wrap; gap: 0.35rem; margin-bottom: 1.8rem; }
    .pc-tags span {
      font-family: var(--mono); font-size: 0.58rem; padding: 0.22rem 0.65rem;
      border-radius: 3px; background: rgba(0,255,136,0.06);
      border: 1px solid rgba(0,255,136,0.15); color: rgba(0,255,136,0.7); letter-spacing: 0.06em;
    }
    .pc-btns { display: flex; gap: 0.6rem; }
    .pcb-main {
      font-family: var(--mono); font-size: 0.7rem; letter-spacing: 0.1em;
      background: var(--neon); color: var(--bg); border: none;
      padding: 0.7rem 1.4rem; border-radius: var(--radius); font-weight: 700; cursor: none; transition: all 0.2s;
    }
    .pcb-main:hover { box-shadow: 0 0 24px rgba(0,255,136,0.35); transform: translateY(-1px); }
    .pcb-sec {
      font-family: var(--mono); font-size: 0.7rem; letter-spacing: 0.08em;
      background: transparent; color: var(--dim); border: 1px solid var(--line);
      padding: 0.7rem 1.4rem; border-radius: var(--radius); cursor: none; transition: all 0.2s;
    }
    .pcb-sec:hover { color: var(--text); border-color: var(--dim); }
    .pc-right { padding: 2.5rem; display: flex; flex-direction: column; gap: 0.7rem; }
    .pc-info-row { display: flex; justify-content: space-between; align-items: center; padding: 0.7rem 0.9rem; background: var(--card); border: 1px solid var(--line); border-radius: var(--radius); }
    .pir-k { font-family: var(--mono); font-size: 0.6rem; color: var(--dim); letter-spacing: 0.12em; }
    .pir-v { font-family: var(--mono); font-size: 0.7rem; color: var(--text); font-weight: 700; }
    .pir-v.g { color: var(--neon); } .pir-v.b { color: var(--neon2); } .pir-v.o { color: var(--orange); }

    /* ====== SLIDE 3 — MEDIA ====== */
    .media-layout { width: 100%; max-width: 1100px; }
    .media-header { display: flex; align-items: flex-end; justify-content: space-between; margin-bottom: 1.5rem; }
    .media-grid { display: grid; grid-template-columns: 1.4fr 1fr; gap: 1.2rem; }

    /* VIDEO BOX */
    .video-box { background: var(--panel); border: 1px solid var(--line); border-radius: 10px; overflow: hidden; }
    .vbox-bar { display: flex; align-items: center; gap: 0.8rem; padding: 0.75rem 1.2rem; border-bottom: 1px solid var(--line); background: var(--card); }
    .vb-dots { display: flex; gap: 5px; }
    .vbd { width: 10px; height: 10px; border-radius: 50%; }
    .vbd.r { background: #ff5f57; } .vbd.y { background: #ffbd2e; } .vbd.g { background: #28c840; }
    .vbox-label { font-family: var(--mono); font-size: 0.65rem; color: var(--dim); letter-spacing: 0.08em; }
    .vbox-label span { color: var(--neon2); }
    #miVideo { width: 100%; display: block; background: #000; max-height: 280px; object-fit: contain; }
    .vbox-footer { padding: 0.75rem 1.2rem; border-top: 1px solid var(--line); background: var(--card); display: flex; align-items: center; gap: 0.8rem; }

    /* IMAGES BOX */
    .images-box { background: var(--panel); border: 1px solid var(--line); border-radius: 10px; overflow: hidden; display: flex; flex-direction: column; }
    .ibox-bar { display: flex; align-items: center; gap: 0.8rem; padding: 0.75rem 1.2rem; border-bottom: 1px solid var(--line); background: var(--card); flex-shrink: 0; }
    .ibox-label { font-family: var(--mono); font-size: 0.65rem; color: var(--dim); letter-spacing: 0.08em; }
    .ibox-label span { color: var(--neon); }
    .gallery-area { flex: 1; padding: 0.8rem; display: grid; grid-template-columns: 1fr 1fr; gap: 0.5rem; overflow-y: auto; min-height: 180px; max-height: 260px; }
    .gallery-thumb { aspect-ratio: 1; border-radius: 6px; overflow: hidden; border: 1px solid var(--line); cursor: none; position: relative; transition: transform 0.2s; }
    .gallery-thumb:hover { transform: scale(1.04); border-color: var(--neon); }
    .gallery-thumb img { width: 100%; height: 100%; object-fit: cover; display: block; }
    .ibox-footer { padding: 0.75rem 1.2rem; border-top: 1px solid var(--line); background: var(--card); display: flex; align-items: center; gap: 0.8rem; flex-shrink: 0; }

    /* UPLOAD BUTTON */
    .upload-btn {
      display: inline-flex; align-items: center; gap: 0.45rem;
      font-family: var(--mono); font-size: 0.65rem; letter-spacing: 0.1em;
      color: var(--dim); border: 1px dashed var(--dim);
      padding: 0.4rem 1rem; border-radius: var(--radius);
      cursor: pointer; transition: all 0.2s; background: none;
    }
    .upload-btn:hover { color: var(--neon); border-color: var(--neon); }
    .upload-btn input { display: none; }

    /* LIGHTBOX */
    .lightbox { position: fixed; inset: 0; z-index: 800; background: rgba(2,4,7,0.96); display: none; align-items: center; justify-content: center; }
    .lightbox.open { display: flex; }
    .lb-img { max-width: 88vw; max-height: 88vh; border-radius: 8px; border: 1px solid var(--line); }
    .lb-close { position: absolute; top: 1.5rem; right: 1.5rem; background: var(--card); border: 1px solid var(--line); color: var(--dim); font-family: var(--mono); font-size: 0.7rem; padding: 0.4rem 0.9rem; border-radius: var(--radius); cursor: pointer; transition: all 0.2s; }
    .lb-close:hover { color: var(--text); }

    /* ====== SLIDE 4 — CONTACTO ====== */
    .contact-layout { width: 100%; max-width: 1100px; }
    .contact-big { font-family: var(--display); font-size: clamp(4rem,8vw,7rem); line-height: 0.92; letter-spacing: 0.02em; margin-bottom: 3rem; }
    .contact-big .line2 { color: var(--neon); }
    .contact-big .line3 { color: transparent; -webkit-text-stroke: 1.5px var(--dim); font-size: 0.7em; }
    .contact-cards { display: grid; grid-template-columns: repeat(3,1fr); gap: 0.8rem; margin-bottom: 1.5rem; }
    .cc { background: var(--panel); border: 1px solid var(--line); border-radius: 10px; padding: 1.2rem 1.4rem; display: flex; align-items: center; gap: 0.9rem; text-decoration: none; transition: all 0.25s; cursor: none; position: relative; overflow: hidden; }
    .cc::after { content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 2px; background: var(--neon); transform: scaleX(0); transform-origin: left; transition: transform 0.3s; }
    .cc:hover::after { transform: scaleX(1); }
    .cc:hover { border-color: rgba(0,255,136,0.2); transform: translateY(-3px); }
    .cc-icon { font-size: 1.4rem; flex-shrink: 0; }
    .cc-info { min-width: 0; }
    .cc-name { font-weight: 700; font-size: 0.85rem; color: var(--text); margin-bottom: 0.15rem; }
    .cc-handle { font-family: var(--mono); font-size: 0.65rem; color: var(--dim); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .cc-arrow { margin-left: auto; color: var(--dim); font-size: 0.8rem; transition: all 0.2s; flex-shrink: 0; }
    .cc:hover .cc-arrow { transform: translate(3px,-3px); color: var(--neon); }
    .contact-footer { font-family: var(--mono); font-size: 0.62rem; color: var(--dim); letter-spacing: 0.12em; text-align: center; margin-top: 1.5rem; }
    .contact-footer span { color: var(--neon); }

    @media (max-width: 760px) {
      .sobre-layout { grid-template-columns: 1fr; gap: 2.5rem; }
      .proj-card { grid-template-columns: 1fr; }
      .pc-right { border-top: 1px solid var(--line); border-right: none; }
      .media-grid { grid-template-columns: 1fr; }
      .contact-cards { grid-template-columns: 1fr 1fr; }
      .side-dots { display: none; }
      .hero-name { font-size: 16vw; }
      .hero-bottom { flex-direction: column; gap: 1.5rem; align-items: flex-start; }
    }
  </style>
</head>
<body>

  <div id="cursor"></div>
  <div id="cursor-ring"></div>

  <!-- LIGHTBOX -->
  <div class="lightbox" id="lightbox">
    <img class="lb-img" id="lb-img" src="" alt=""/>
    <button class="lb-close" onclick="closeLightbox()">✕ CERRAR</button>
  </div>

  <!-- TOP BAR -->
  <nav class="topbar">
    <div class="topbar-id">
      <div class="tb-badge">VSJ</div>
      <span class="tb-name">vicente_silvestre_jop<span class="blink" style="color:var(--neon)">_</span></span>
    </div>
    <div class="topbar-nav">
      <a class="tnav on" id="tnav0" onclick="goTo(0);return false;" href="#"><span class="tn-num">00</span>INICIO</a>
      <a class="tnav"    id="tnav1" onclick="goTo(1);return false;" href="#"><span class="tn-num">01</span>SOBRE</a>
      <a class="tnav"    id="tnav2" onclick="goTo(2);return false;" href="#"><span class="tn-num">02</span>PROYECTOS</a>
      <a class="tnav"    id="tnav3" onclick="goTo(3);return false;" href="#"><span class="tn-num">03</span>MEDIA</a>
      <a class="tnav"    id="tnav4" onclick="goTo(4);return false;" href="#"><span class="tn-num">04</span>CONTACTO</a>
    </div>
    <div class="topbar-progress">
      <div class="progress-fill" id="progFill" style="width:0%"></div>
    </div>
  </nav>

  <!-- SIDE DOTS -->
  <div class="side-dots">
    <div class="sd on" data-label="INICIO"    onclick="goTo(0)"></div>
    <div class="sd"    data-label="SOBRE"     onclick="goTo(1)"></div>
    <div class="sd"    data-label="PROYECTOS" onclick="goTo(2)"></div>
    <div class="sd"    data-label="MEDIA"     onclick="goTo(3)"></div>
    <div class="sd"    data-label="CONTACTO"  onclick="goTo(4)"></div>
  </div>

  <div class="scroll-track" id="track">

    <!-- SLIDE 0 — HERO -->
    <div class="slide" data-num="00" id="slide0">
      <div class="hero-layout">
        <div class="hero-top">
          <div class="hero-label">PORTFOLIO · 2026</div>
          <div class="hero-status"><span class="live-dot"></span>SENATI — ING. SOFTWARE + IA</div>
        </div>
        <div class="hero-name">
          <span class="w1">VICENTE</span>
          <span class="w2">SILVESTRE</span>
          <span class="w3">JOP</span>
        </div>
        <div class="hero-bottom">
          <p class="hero-desc">
            Desarrollador de videojuegos 2D con <strong>Inteligencia Artificial</strong>.<br/>
            Especialista en <strong>Java + Greenfoot</strong>, enemigos inteligentes,<br/>
            pathfinding y sistemas de dificultad adaptativa.
          </p>
          <div class="hero-ctas">
            <button class="cta-prim" onclick="goTo(2)">VER PROYECTO →</button>
            <button class="cta-sec"  onclick="goTo(4)">CONTACTO</button>
          </div>
        </div>
      </div>
    </div>

    <!-- SLIDE 1 — SOBRE -->
    <div class="slide" data-num="01" id="slide1">
      <div class="sobre-layout">
        <div class="sobre-left">
          <div class="s-label">SOBRE MÍ</div>
          <div class="slide-title">ING.<br/><span class="accent">SOFT</span><br/><span class="accent2">&amp; IA</span></div>
          <div class="skills-wrap">
            <div class="skill-tag"><span class="dot"></span>Java</div>
            <div class="skill-tag"><span class="dot"></span>Greenfoot</div>
            <div class="skill-tag"><span class="dot"></span>OOP</div>
            <div class="skill-tag"><span class="dot"></span>IA Básica</div>
            <div class="skill-tag"><span class="dot"></span>HTML</div>
            <div class="skill-tag"><span class="dot"></span>CSS</div>
            <div class="skill-tag"><span class="dot"></span>Algoritmos</div>
            <div class="skill-tag"><span class="dot"></span>Game Dev</div>
          </div>
        </div>
        <div class="sobre-right">
          <p class="bio-text">Soy <strong>Vicente Silvestre Jop</strong>, estudiante de <strong>Ingeniería de Software con Inteligencia Artificial</strong> en <strong>SENATI</strong>. Me apasiona crear videojuegos 2D donde la inteligencia artificial da vida a los personajes.</p>
          <p class="bio-text">Combino <strong>Ingeniería de Software</strong> e <strong>IA</strong> para desarrollar juegos donde los enemigos aprenden, se adaptan y reaccionan al jugador dinámicamente. Mi lenguaje principal es <strong>Java</strong> con el entorno <strong>Greenfoot</strong>.</p>
          <p class="bio-text">En SENATI estudio materias como <strong>Algoritmos</strong>, <strong>Estructuras de datos</strong>, <strong>Machine Learning básico</strong> y <strong>Programación orientada a objetos</strong>.</p>
          <div class="stats-row">
            <div class="stat-box"><div class="stat-num">1</div><div class="stat-lbl">PROYECTO<br/>PRINCIPAL</div></div>
            <div class="divider-v"></div>
            <div class="stat-box"><div class="stat-num">2+</div><div class="stat-lbl">LENGUAJES<br/>DOMINADOS</div></div>
            <div class="divider-v"></div>
            <div class="stat-box"><div class="stat-num">∞</div><div class="stat-lbl">PASIÓN POR<br/>LA IA</div></div>
          </div>
        </div>
      </div>
    </div>

    <!-- SLIDE 2 — PROYECTOS -->
    <div class="slide" data-num="02" id="slide2">
      <div class="proj-layout">
        <div class="proj-header">
          <div class="s-label">PROYECTOS</div>
          <span style="font-family:var(--mono);font-size:0.62rem;color:var(--dim)">01 / 01</span>
        </div>
        <div class="proj-card">
          <div class="pc-left">
            <div class="pc-eyebrow">PROYECTO PRINCIPAL</div>
            <div class="pc-title">NAVE<br/><span>ESPACIAL</span></div>
            <p class="pc-desc">Juego de disparos espaciales en <strong>Greenfoot</strong> con <strong>IA aplicada</strong>. Pilotas una nave mientras los enemigos se mueven en patrones inteligentes, esquivan disparos y escalan en dificultad. Incluye pathfinding básico, sistema de colisiones y puntuación en tiempo real.</p>
            <div class="pc-tags">
              <span>Greenfoot</span><span>Java</span><span>IA</span>
              <span>2D Shooter</span><span>OOP</span><span>Pathfinding</span>
            </div>
            <div class="pc-btns">
              <button class="pcb-main" onclick="goTo(3)">▶ VER DEMO</button>
              <button class="pcb-sec">{ } CÓDIGO</button>
            </div>
          </div>
          <div class="pc-right">
            <div class="pc-info-row"><span class="pir-k">LENGUAJE</span><span class="pir-v g">Java</span></div>
            <div class="pc-info-row"><span class="pir-k">FRAMEWORK</span><span class="pir-v b">Greenfoot</span></div>
            <div class="pc-info-row"><span class="pir-k">TIPO</span><span class="pir-v">2D Shooter</span></div>
            <div class="pc-info-row"><span class="pir-k">IA APLICADA</span><span class="pir-v g">✓ SÍ</span></div>
            <div class="pc-info-row"><span class="pir-k">PATHFINDING</span><span class="pir-v g">✓ SÍ</span></div>
            <div class="pc-info-row"><span class="pir-k">DIFICULTAD ADAPTATIVA</span><span class="pir-v g">✓ SÍ</span></div>
            <div class="pc-info-row"><span class="pir-k">ESTADO</span><span class="pir-v b">COMPLETADO</span></div>
            <div class="pc-info-row"><span class="pir-k">AÑO</span><span class="pir-v o">2026</span></div>
          </div>
        </div>
      </div>
    </div>

    <!-- SLIDE 3 — MEDIA -->
    <div class="slide" data-num="03" id="slide3">
      <div class="media-layout">
        <div class="media-header">
          <div class="s-label">MEDIA — VIDEO &amp; IMÁGENES</div>
        </div>
        <div class="media-grid">

          <!-- VIDEO: referencia directa al archivo local -->
          <div class="video-box">
            <div class="vbox-bar">
              <div class="vb-dots">
                <span class="vbd r"></span><span class="vbd y"></span><span class="vbd g"></span>
              </div>
              <span class="vbox-label">~/nave-espacial/ <span>mi-video.mp4</span></span>
            </div>
            <video id="miVideo" controls style="width:100%;display:block;background:#000;max-height:280px;">
              <source src="mi-video.mp4" type="video/mp4"/>
              Tu navegador no soporta video HTML5.
            </video>
            <div class="vbox-footer">
              <span style="font-family:var(--mono);font-size:0.6rem;color:var(--dim)">MP4 · MOV · AVI</span>
            </div>
          </div>

          <!-- IMÁGENES: tarea1000.png cargada por defecto -->
          <div class="images-box">
            <div class="ibox-bar">
              <div class="vb-dots">
                <span class="vbd r"></span><span class="vbd y"></span><span class="vbd g"></span>
              </div>
              <span class="ibox-label">~/galeria/ <span id="galLabel">1 imagen</span></span>
            </div>
            <div class="gallery-area" id="galleryArea">
              <!-- Imagen local por defecto -->
              <div class="gallery-thumb" onclick="openLightbox('tarea1000.png')">
                <img src="tarea1000.png" alt="tarea1000"/>
              </div>
            </div>
            <div class="ibox-footer">
              <label class="upload-btn" for="fImgInput">
                ↑ AGREGAR IMÁGENES
                <input type="file" id="fImgInput" accept="image/*" multiple onchange="loadImages(event)"/>
              </label>
              <span id="img-count" style="font-family:var(--mono);font-size:0.6rem;color:var(--dim);margin-left:auto">1 / ∞</span>
            </div>
          </div>

        </div>
      </div>
    </div>

    <!-- SLIDE 4 — CONTACTO -->
    <div class="slide" data-num="04" id="slide4">
      <div class="contact-layout">
        <div class="s-label">CONTACTO</div>
        <div class="contact-big">
          <div>HABLEMOS</div>
          <div class="line2">CONMIGO</div>
          <div class="line3">AHORA.</div>
        </div>
        <div class="contact-cards">
          <a class="cc" href="https://github.com/vicentesilvestre" target="_blank">
            <span class="cc-icon">⌨️</span>
            <div class="cc-info"><div class="cc-name">GitHub</div><div class="cc-handle">github.com/vicentesilvestre</div></div>
            <span class="cc-arrow">↗</span>
          </a>
          <a class="cc" href="https://linkedin.com/in/vicentesilvestre" target="_blank">
            <span class="cc-icon">💼</span>
            <div class="cc-info"><div class="cc-name">LinkedIn</div><div class="cc-handle">linkedin.com/in/vicentesilvestre</div></div>
            <span class="cc-arrow">↗</span>
          </a>
          <a class="cc" href="mailto:vicente.silvestre@gmail.com">
            <span class="cc-icon">✉️</span>
            <div class="cc-info"><div class="cc-name">Email</div><div class="cc-handle">vicente.silvestre@gmail.com</div></div>
            <span class="cc-arrow">↗</span>
          </a>
          <a class="cc" href="https://twitter.com/vicentesjop" target="_blank">
            <span class="cc-icon">🐦</span>
            <div class="cc-info"><div class="cc-name">Twitter / X</div><div class="cc-handle">@vicentesjop</div></div>
            <span class="cc-arrow">↗</span>
          </a>
          <a class="cc" href="https://discord.com/users/vicentesjop" target="_blank">
            <span class="cc-icon">🎮</span>
            <div class="cc-info"><div class="cc-name">Discord</div><div class="cc-handle">vicentesjop#0000</div></div>
            <span class="cc-arrow">↗</span>
          </a>
          <a class="cc" href="https://youtube.com/@vicentesjop" target="_blank">
            <span class="cc-icon">▶️</span>
            <div class="cc-info"><div class="cc-name">YouTube</div><div class="cc-handle">@vicentesjop</div></div>
            <span class="cc-arrow">↗</span>
          </a>
        </div>
        <div class="contact-footer">
          DISEÑADO Y CONSTRUIDO POR <span>VICENTE SILVESTRE JOP</span> — 2026
        </div>
      </div>
    </div>

  </div><!-- /scroll-track -->

  <script>
    /* ===== CURSOR ===== */
    var cur  = document.getElementById('cursor');
    var ring = document.getElementById('cursor-ring');
    document.addEventListener('mousemove', function(e) {
      cur.style.left  = e.clientX + 'px';
      cur.style.top   = e.clientY + 'px';
      ring.style.left = e.clientX + 'px';
      ring.style.top  = e.clientY + 'px';
    });

    /* ===== NAVEGACIÓN ===== */
    var current = 0;
    var total   = 5;
    var track   = document.getElementById('track');

    function goTo(n) {
      current = Math.max(0, Math.min(n, total - 1));
      track.style.transform = 'translateX(' + (-current * 100) + 'vw)';
      document.querySelectorAll('.tnav').forEach(function(el, i) {
        el.classList.toggle('on', i === current);
      });
      document.querySelectorAll('.sd').forEach(function(el, i) {
        el.classList.toggle('on', i === current);
      });
      document.getElementById('progFill').style.width = (current / (total - 1) * 100) + '%';
    }

    document.addEventListener('keydown', function(e) {
      if (e.key === 'ArrowRight' || e.key === 'ArrowDown') goTo(current + 1);
      if (e.key === 'ArrowLeft'  || e.key === 'ArrowUp')   goTo(current - 1);
    });

    var wheelLock = false;
    document.addEventListener('wheel', function(e) {
      if (wheelLock) return;
      wheelLock = true;
      if (e.deltaY > 0 || e.deltaX > 0) goTo(current + 1);
      else goTo(current - 1);
      setTimeout(function() { wheelLock = false; }, 750);
    }, { passive: true });

    var touchStartX = 0;
    document.addEventListener('touchstart', function(e) { touchStartX = e.touches[0].clientX; });
    document.addEventListener('touchend', function(e) {
      var diff = touchStartX - e.changedTouches[0].clientX;
      if (diff >  50) goTo(current + 1);
      if (diff < -50) goTo(current - 1);
    });

    goTo(0);

    /* ===== CONTAR IMÁGENES INICIALES ===== */
    var imgCount = document.querySelectorAll('#galleryArea .gallery-thumb').length;

    /* ===== CAMBIAR VIDEO ===== */
    function loadVideo(e) {
      var f = e.target.files[0];
      if (!f) return;
      var video = document.getElementById('miVideo');
      video.src = URL.createObjectURL(f);
      document.querySelector('#miVideo + .vbox-footer .vbox-label span, .vbox-label span') && null;
      video.load();
    }

    /* ===== AGREGAR IMÁGENES ===== */
    function loadImages(e) {
      var files = Array.from(e.target.files);
      if (!files.length) return;

      var area = document.getElementById('galleryArea');

      files.forEach(function(f) {
        if (!f.type.startsWith('image/')) return;
        var url = URL.createObjectURL(f);

        var div = document.createElement('div');
        div.className = 'gallery-thumb';
        (function(u) { div.onclick = function() { openLightbox(u); }; })(url);

        var img = document.createElement('img');
        img.src = url;
        img.alt = f.name;

        div.appendChild(img);
        area.appendChild(div);
        imgCount++;
      });

      updateCount();
      e.target.value = '';
    }

    function updateCount() {
      var txt = imgCount === 1 ? '1 imagen' : imgCount + ' imágenes';
      document.getElementById('galLabel').textContent = txt;
      document.getElementById('img-count').textContent = imgCount + ' / ∞';
    }

    /* ===== LIGHTBOX ===== */
    function openLightbox(src) {
      document.getElementById('lb-img').src = src;
      document.getElementById('lightbox').classList.add('open');
    }
    function closeLightbox() {
      document.getElementById('lightbox').classList.remove('open');
    }
    document.getElementById('lightbox').addEventListener('click', function(e) {
      if (e.target === this) closeLightbox();
    });
  </script>
</body>
</html>
