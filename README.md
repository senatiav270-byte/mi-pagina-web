<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ronaldo Aliaga Vicente — Dev Portfolio</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;700;800&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #080d16;
      --surface: #0d1422;
      --surface2: #111b2e;
      --border: #1c2d47;
      --accent: #38bdf8;
      --accent2: #818cf8;
      --green: #34d399;
      --text: #e2e8f0;
      --muted: #475569;
      --font: 'Plus Jakarta Sans', sans-serif;
      --mono: 'Fira Code', monospace;
    }

    html { scroll-behavior: smooth; }
    body { background: var(--bg); color: var(--text); font-family: var(--font); overflow-x: hidden; }
    #stars { position: fixed; inset: 0; z-index: 0; pointer-events: none; }

    .nav {
      position: fixed; top: 0; left: 0; right: 0; z-index: 100;
      display: flex; align-items: center; justify-content: space-between;
      padding: 0.6rem 1rem;
      background: rgba(8,13,22,0.9);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border);
    }
    .nav-logo { font-family: var(--mono); font-size: 0.85rem; font-weight: 500; color: var(--accent); letter-spacing: 0.05em; }
    .logo-blink { animation: blink 1s step-end infinite; }
    @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
    .nav-center { display: flex; gap: 0.7rem; }
    .nav-center a {
      font-family: var(--mono); font-size: 0.58rem; color: var(--muted);
      text-decoration: none; letter-spacing: 0.05em; transition: color 0.2s;
      padding-bottom: 2px; border-bottom: 1px solid transparent;
    }
    .nav-center a:hover, .nav-center a.active { color: var(--accent); border-bottom-color: var(--accent); }

    .page {
      position: fixed; inset: 0; z-index: 1;
      overflow-y: auto;
      display: flex; flex-direction: column;
      align-items: center; justify-content: flex-start;
      padding: 3.5rem 0.8rem 1.5rem;
      opacity: 0; pointer-events: none;
      transform: translateY(30px);
      transition: opacity 0.5s ease, transform 0.5s ease;
    }
    .page.active { opacity: 1; pointer-events: all; transform: translateY(0); }
    .page-inner { width: 100%; max-width: 860px; }

    .btn-solid { background: var(--accent); color: var(--bg); padding: 0.5rem 1rem; border-radius: 7px; font-weight: 700; font-size: 0.75rem; border: none; cursor: pointer; transition: all 0.2s; box-shadow: 0 0 16px rgba(56,189,248,0.25); }
    .btn-solid:hover { box-shadow: 0 0 24px rgba(56,189,248,0.45); transform: translateY(-1px); }
    .btn-outline { border: 1px solid var(--border); color: var(--text); padding: 0.5rem 1rem; border-radius: 7px; font-weight: 500; font-size: 0.75rem; background: none; cursor: pointer; transition: all 0.2s; }
    .btn-outline:hover { border-color: var(--accent); color: var(--accent); }

    .section-label { font-family: var(--mono); font-size: 0.58rem; color: var(--accent2); letter-spacing: 0.15em; margin-bottom: 1rem; }

    .portada-wrap { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; overflow: hidden; }
    .portada-top {
      background: linear-gradient(135deg, #0d1422, #111b2e);
      padding: 1rem; display: flex; align-items: center; gap: 0.9rem;
      border-bottom: 1px solid var(--border); position: relative; overflow: hidden;
    }
    .portada-top::before { content: ''; position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 50%, rgba(56,189,248,0.06), transparent 70%); }
    .portada-avatar {
      width: 52px; height: 52px; border-radius: 50%;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      display: flex; align-items: center; justify-content: center;
      font-family: var(--mono); font-size: 1.2rem; font-weight: 700;
      color: var(--bg); flex-shrink: 0;
      box-shadow: 0 0 16px rgba(56,189,248,0.3); position: relative; z-index: 1;
    }
    .portada-id { position: relative; z-index: 1; }
    .portada-nombre {
      font-size: clamp(0.85rem, 3.5vw, 1.2rem); font-weight: 800; letter-spacing: -0.02em;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 0.2rem;
    }
    .portada-rol { font-family: var(--mono); font-size: 0.58rem; color: var(--green); letter-spacing: 0.06em; margin-bottom: 0.25rem; }
    .portada-uni { font-size: 0.65rem; color: var(--muted); display: flex; align-items: center; gap: 0.3rem; }
    .portada-body { padding: 0.8rem; display: grid; grid-template-columns: 1fr 1fr; gap: 0.6rem; }
    .info-block { background: var(--surface2); border: 1px solid var(--border); border-radius: 9px; padding: 0.7rem 0.8rem; }
    .info-block-title { font-family: var(--mono); font-size: 0.54rem; color: var(--accent); letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 0.5rem; }
    .info-block p { font-size: 0.7rem; color: #94a3b8; line-height: 1.55; }
    .info-block p strong { color: var(--text); font-weight: 600; }
    .info-chips { display: flex; flex-wrap: wrap; gap: 0.25rem; margin-top: 0.4rem; }
    .info-chip { font-family: var(--mono); font-size: 0.52rem; padding: 0.15rem 0.5rem; border-radius: 4px; background: rgba(56,189,248,0.07); border: 1px solid rgba(56,189,248,0.18); color: var(--accent); }
    .info-chip.green { background: rgba(52,211,153,0.07); border-color: rgba(52,211,153,0.2); color: var(--green); }
    .info-chip.purple { background: rgba(129,140,248,0.07); border-color: rgba(129,140,248,0.2); color: var(--accent2); }

    .sobre-grid { display: grid; grid-template-columns: 1fr 1.4fr; gap: 1.5rem; align-items: start; }
    .sobre-text h2 { font-size: clamp(1.4rem, 4vw, 2.5rem); font-weight: 800; line-height: 1.15; letter-spacing: -0.03em; }
    .hl { color: var(--accent); } .hl2 { color: var(--accent2); }
    .sobre-right p { font-size: 0.78rem; color: #94a3b8; line-height: 1.7; margin-bottom: 1rem; }
    .sobre-right strong { color: var(--accent); font-weight: 600; }
    .tech-list { display: grid; grid-template-columns: 1fr 1fr; gap: 0.35rem; }
    .tech-item { display: flex; align-items: center; gap: 0.45rem; font-family: var(--mono); font-size: 0.65rem; color: #64748b; padding: 0.45rem 0.65rem; background: var(--surface); border: 1px solid var(--border); border-radius: 7px; transition: all 0.2s; }
    .tech-item:hover { color: var(--accent); border-color: rgba(56,189,248,0.3); }
    .tech-dot { width: 5px; height: 5px; background: var(--accent); border-radius: 50%; flex-shrink: 0; }

    .demo-wrap { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; overflow: hidden; }
    .demo-header { display: flex; align-items: center; gap: 0.5rem; padding: 0.6rem 0.9rem; border-bottom: 1px solid var(--border); background: var(--surface2); }
    .demo-dots { display: flex; gap: 4px; }
    .dd { width: 9px; height: 9px; border-radius: 50%; }
    .dd.red { background: #ff5f57; } .dd.yellow { background: #ffbd2e; } .dd.green { background: #28c840; }
    .demo-filename { font-family: var(--mono); font-size: 0.6rem; color: var(--muted); }
    .video-area { background: #000; }

    .proj-panel { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; overflow: hidden; transition: border-color 0.3s, transform 0.3s; }
    .proj-panel:hover { border-color: rgba(56,189,248,0.35); transform: translateY(-3px); }
    .pp-head { padding: 0.85rem 1rem; background: var(--surface2); border-bottom: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; }
    .pp-title { font-size: 0.82rem; font-weight: 700; display: flex; align-items: center; gap: 0.4rem; }
    .pp-status { font-family: var(--mono); font-size: 0.54rem; letter-spacing: 0.08em; padding: 0.18rem 0.55rem; border-radius: 99px; background: rgba(52,211,153,0.1); border: 1px solid rgba(52,211,153,0.3); color: var(--green); }
    .pp-body { padding: 1rem; }
    .pp-desc { font-size: 0.75rem; color: #64748b; line-height: 1.65; margin-bottom: 0.9rem; }
    .pp-tags { display: flex; flex-wrap: wrap; gap: 0.3rem; margin-bottom: 1rem; }
    .pp-tags span { font-family: var(--mono); font-size: 0.54rem; letter-spacing: 0.06em; padding: 0.18rem 0.55rem; border-radius: 5px; background: rgba(56,189,248,0.07); border: 1px solid rgba(56,189,248,0.18); color: var(--accent); }
    .pp-btns { display: flex; gap: 0.45rem; }
    .ppb-main { flex: 1; background: linear-gradient(135deg, var(--accent), var(--accent2)); color: var(--bg); border: none; padding: 0.5rem 0.8rem; border-radius: 7px; font-size: 0.72rem; font-weight: 700; cursor: pointer; transition: all 0.2s; }
    .ppb-main:hover { box-shadow: 0 4px 18px rgba(56,189,248,0.4); transform: translateY(-1px); }
    .ppb-sec { background: transparent; color: var(--muted); border: 1px solid var(--border); padding: 0.5rem 0.8rem; border-radius: 7px; font-family: var(--mono); font-size: 0.62rem; cursor: pointer; transition: all 0.2s; }
    .ppb-sec:hover { color: var(--text); border-color: #334155; }

    .contact-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 0.6rem; }
    .contact-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 0.8rem 0.9rem; display: flex; align-items: center; gap: 0.6rem; text-decoration: none; transition: all 0.25s; }
    .contact-card:hover { transform: translateY(-2px); border-color: rgba(56,189,248,0.4); box-shadow: 0 5px 16px rgba(0,0,0,0.3); }
    .contact-icon { width: 32px; height: 32px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 0.95rem; flex-shrink: 0; }
    .ic-gh { background: rgba(255,255,255,0.06); } .ic-li { background: rgba(10,102,194,0.15); }
    .ic-tw { background: rgba(29,161,242,0.12); } .ic-em { background: rgba(56,189,248,0.1); }
    .ic-dc { background: rgba(88,101,242,0.15); } .ic-yt { background: rgba(255,0,0,0.1); }
    .contact-info { display: flex; flex-direction: column; gap: 0.1rem; overflow: hidden; }
    .contact-name { font-weight: 600; font-size: 0.75rem; color: var(--text); }
    .contact-handle { font-family: var(--mono); font-size: 0.58rem; color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .contact-arrow { margin-left: auto; color: var(--muted); font-size: 0.8rem; flex-shrink: 0; transition: transform 0.2s; }
    .contact-card:hover .contact-arrow { transform: translateX(3px); color: var(--accent); }

    .page-dots { position: fixed; right: 0.6rem; top: 50%; transform: translateY(-50%); z-index: 200; display: flex; flex-direction: column; gap: 0.45rem; }
    .pdot { width: 5px; height: 5px; border-radius: 50%; background: var(--border); cursor: pointer; transition: all 0.3s; }
    .pdot.active { background: var(--accent); box-shadow: 0 0 6px var(--accent); height: 14px; border-radius: 3px; }

    .page-footer { margin-top: 1.2rem; text-align: center; font-family: var(--mono); font-size: 0.58rem; color: var(--muted); }
    .fc-hl { color: var(--accent); }

    @media (max-width: 480px) {
      .sobre-grid { grid-template-columns: 1fr; gap: 1rem; }
      .contact-grid { grid-template-columns: 1fr; }
      .portada-body { grid-template-columns: 1fr; }
      .page-dots { display: none; }
      .nav-center { gap: 0.5rem; }
    }
  </style>
</head>
<body>

  <canvas id="stars"></canvas>

  <nav class="nav">
    <div class="nav-logo">RN<span class="logo-blink">_</span></div>
    <div class="nav-center">
      <a href="#" onclick="goTo(0);return false;" id="nav0">Inicio</a>
      <a href="#" onclick="goTo(1);return false;" id="nav1">Sobre</a>
      <a href="#" onclick="goTo(2);return false;" id="nav2">Proyectos</a>
      <a href="#" onclick="goTo(3);return false;" id="nav3">Video</a>
      <a href="#" onclick="goTo(4);return false;" id="nav4">Contacto</a>
    </div>
  </nav>

  <div class="page-dots">
    <div class="pdot active" onclick="goTo(0)"></div>
    <div class="pdot" onclick="goTo(1)"></div>
    <div class="pdot" onclick="goTo(2)"></div>
    <div class="pdot" onclick="goTo(3)"></div>
    <div class="pdot" onclick="goTo(4)"></div>
  </div>

  <!-- PAGE 0 — PORTADA -->
  <div class="page active" id="page0">
    <div class="page-inner">
      <div class="section-label">// PORTADA</div>
      <div class="portada-wrap">
        <div class="portada-top">
          <div class="portada-avatar">R</div>
          <div class="portada-id">
            <div class="portada-nombre">Ronaldo Aliaga Vicente</div>
            <div class="portada-rol">&#9679; Ing. de Software con Inteligencia Artificial</div>
            <div class="portada-uni">🎓 SENATI — Ing. de Software con IA</div>
          </div>
        </div>
        <div class="portada-body">
          <div class="info-block">
            <div class="info-block-title">👤 Sobre mí</div>
            <p>Soy <strong>Ronaldo Aliaga Vicente</strong>, estudiante apasionado por la tecnología. Estudio <strong>Ing. de Software con IA</strong> en <strong>SENATI</strong> y creo videojuegos inteligentes.</p>
          </div>
          <div class="info-block">
            <div class="info-block-title">🤖 IA en videojuegos</div>
            <p>Aplico <strong>IA</strong> en mis juegos: enemigos inteligentes, pathfinding y dificultad adaptativa con <strong>Java</strong> y <strong>Greenfoot</strong>.</p>
            <div class="info-chips">
              <span class="info-chip">IA</span>
              <span class="info-chip">Pathfinding</span>
              <span class="info-chip">Greenfoot</span>
            </div>
          </div>
          <div class="info-block">
            <div class="info-block-title">🎓 Educación</div>
            <p><strong>SENATI</strong><br/>Ing. de Software con IA<br/>OOP, Algoritmos, Machine Learning.</p>
            <div class="info-chips">
              <span class="info-chip green">En curso</span>
              <span class="info-chip purple">ML</span>
              <span class="info-chip purple">Algoritmos</span>
            </div>
          </div>
          <div class="info-block">
            <div class="info-block-title">⚡ Habilidades</div>
            <p><strong>Java</strong>, <strong>HTML</strong>, <strong>CSS</strong>, videojuegos 2D con IA y diseño de niveles.</p>
            <div class="info-chips">
              <span class="info-chip">Java</span>
              <span class="info-chip">HTML</span>
              <span class="info-chip">CSS</span>
              <span class="info-chip green">IA</span>
              <span class="info-chip purple">2D</span>
            </div>
          </div>
        </div>
      </div>
      <div style="display:flex;gap:0.6rem;margin-top:0.9rem">
        <button class="btn-solid" onclick="goTo(2)">Ver proyectos</button>
        <button class="btn-outline" onclick="goTo(4)">Contacto</button>
      </div>
    </div>
  </div>

  <!-- PAGE 1 — SOBRE -->
  <div class="page" id="page1">
    <div class="page-inner">
      <div class="section-label">// 01 SOBRE MÍ</div>
      <div class="sobre-grid">
        <div class="sobre-text">
          <h2>Ing. de<br/><span class="hl">Software</span> &amp;<br/><span class="hl2">IA</span></h2>
        </div>
        <div class="sobre-right">
          <p>Soy <strong>Ronaldo Aliaga Vicente</strong>, estudiante de <strong>Ingeniería de Software con Inteligencia Artificial</strong> en <strong>SENATI</strong>. Me apasiona crear videojuegos 2D usando Java y Greenfoot, aplicando principios de IA para construir enemigos inteligentes y experiencias jugables únicas.</p>
          <p>La combinación de <strong>Ingeniería de Software</strong> e <strong>IA</strong> me permite desarrollar juegos donde los personajes aprenden, se adaptan y reaccionan al jugador de forma dinámica.</p>
          <div class="tech-list">
            <div class="tech-item"><span class="tech-dot"></span>Java</div>
            <div class="tech-item"><span class="tech-dot"></span>Greenfoot</div>
            <div class="tech-item"><span class="tech-dot"></span>OOP</div>
            <div class="tech-item"><span class="tech-dot"></span>IA Básica</div>
            <div class="tech-item"><span class="tech-dot"></span>HTML</div>
            <div class="tech-item"><span class="tech-dot"></span>CSS</div>
            <div class="tech-item"><span class="tech-dot"></span>Algoritmos</div>
            <div class="tech-item"><span class="tech-dot"></span>Game Dev</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- PAGE 2 — PROYECTOS -->
  <div class="page" id="page2">
    <div class="page-inner">
      <div class="section-label">// 02 PROYECTOS</div>
      <div class="proj-panel">
        <div class="pp-head">
          <div class="pp-title">🚀 Nave Espacial — Greenfoot + IA</div>
          <span class="pp-status">&#9679; Completado</span>
        </div>
        <div class="pp-body">
          <p class="pp-desc">
            Juego de disparos espaciales en <strong style="color:#38bdf8">Greenfoot</strong> con
            <strong style="color:#818cf8">IA</strong> aplicada. Pilotas una nave espacial en un universo
            donde los enemigos tienen comportamiento inteligente, se mueven en patrones y aumentan
            su dificultad. Incluye disparos, colisiones, pathfinding y puntuación en tiempo real.
          </p>
          <div class="pp-tags">
            <span>Greenfoot</span><span>Java</span><span>IA</span>
            <span>Shooter</span><span>OOP</span><span>2D</span><span>Pathfinding</span>
          </div>
          <div class="pp-btns">
            <button class="ppb-main" onclick="goTo(3)">&#9654; Ver video</button>
            <button class="ppb-sec">{ } Código</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- PAGE 3 — VIDEO -->
  <div class="page" id="page3">
    <div class="page-inner">
      <div class="section-label">// 03 VIDEO DEL PROYECTO</div>
      <div class="demo-wrap">
        <div class="demo-header">
          <div class="demo-dots">
            <span class="dd red"></span>
            <span class="dd yellow"></span>
            <span class="dd green"></span>
          </div>
          <span class="demo-filename">mi video.mp4</span>
        </div>
        <div class="video-area">
          <video controls style="width:100%;max-height:340px;display:block;">
            <source src="mi video.mp4" type="video/mp4"/>
          </video>
        </div>
      </div>
    </div>
  </div>

  <!-- PAGE 4 — CONTACTO -->
  <div class="page" id="page4">
    <div class="page-inner">
      <div class="section-label">// 04 CONTACTO</div>
      <div class="contact-grid">
        <a class="contact-card" href="https://github.com/ronaldo" target="_blank">
          <div class="contact-icon ic-gh">⌨️</div>
          <div class="contact-info"><span class="contact-name">GitHub</span><span class="contact-handle">github.com/ronaldo</span></div>
          <span class="contact-arrow">→</span>
        </a>
        <a class="contact-card" href="https://linkedin.com/in/ronaldo" target="_blank">
          <div class="contact-icon ic-li">💼</div>
          <div class="contact-info"><span class="contact-name">LinkedIn</span><span class="contact-handle">linkedin.com/in/ronaldo</span></div>
          <span class="contact-arrow">→</span>
        </a>
        <a class="contact-card" href="https://twitter.com/ronaldo" target="_blank">
          <div class="contact-icon ic-tw">🐦</div>
          <div class="contact-info"><span class="contact-name">Twitter / X</span><span class="contact-handle">@ronaldo</span></div>
          <span class="contact-arrow">→</span>
        </a>
        <a class="contact-card" href="mailto:ronaldo@gmail.com">
          <div class="contact-icon ic-em">✉️</div>
          <div class="contact-info"><span class="contact-name">Email</span><span class="contact-handle">ronaldo@gmail.com</span></div>
          <span class="contact-arrow">→</span>
        </a>
        <a class="contact-card" href="https://discord.com/users/ronaldo" target="_blank">
          <div class="contact-icon ic-dc">🎮</div>
          <div class="contact-info"><span class="contact-name">Discord</span><span class="contact-handle">ronaldo#0000</span></div>
          <span class="contact-arrow">→</span>
        </a>
        <a class="contact-card" href="https://youtube.com/@ronaldo" target="_blank">
          <div class="contact-icon ic-yt">▶️</div>
          <div class="contact-info"><span class="contact-name">YouTube</span><span class="contact-handle">@ronaldo</span></div>
          <span class="contact-arrow">→</span>
        </a>
      </div>
      <div class="page-footer">Creado por <span class="fc-hl">Ronaldo Aliaga Vicente</span> &mdash; 2026</div>
    </div>
  </div>

  <script>
    const c = document.getElementById('stars');
    const ctx = c.getContext('2d');
    c.width = window.innerWidth; c.height = window.innerHeight;
    const stars = Array.from({length:100}, () => ({
      x: Math.random()*c.width, y: Math.random()*c.height,
      r: Math.random()*1.1, a: Math.random()*Math.PI*2
    }));
    function drawStars() {
      ctx.clearRect(0,0,c.width,c.height);
      stars.forEach(s => {
        s.a += 0.004;
        ctx.beginPath(); ctx.arc(s.x, s.y, s.r, 0, Math.PI*2);
        ctx.fillStyle = `rgba(100,180,255,${0.3+0.3*Math.sin(s.a)})`; ctx.fill();
      });
      requestAnimationFrame(drawStars);
    }
    drawStars();
    window.addEventListener('resize', () => { c.width=window.innerWidth; c.height=window.innerHeight; });

    let current = 0;
    const total = 5;

    function goTo(n) {
      document.getElementById('page'+current).classList.remove('active');
      document.querySelectorAll('.pdot')[current].classList.remove('active');
      document.getElementById('nav'+current).classList.remove('active');
      current = n;
      document.getElementById('page'+current).classList.add('active');
      document.querySelectorAll('.pdot')[current].classList.add('active');
      document.getElementById('nav'+current).classList.add('active');
    }

    document.addEventListener('keydown', e => {
      if (e.key === 'ArrowRight' || e.key === 'ArrowDown') goTo(Math.min(current+1, total-1));
      if (e.key === 'ArrowLeft'  || e.key === 'ArrowUp')   goTo(Math.max(current-1, 0));
    });

    let tx = 0;
    document.addEventListener('touchstart', e => tx = e.touches[0].clientX);
    document.addEventListener('touchend', e => {
      const diff = tx - e.changedTouches[0].clientX;
      if (diff > 50) goTo(Math.min(current+1, total-1));
      if (diff < -50) goTo(Math.max(current-1, 0));
    });

    goTo(0);
  </script>
</body>
</html>
