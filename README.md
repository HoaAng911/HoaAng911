<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ngoc Hoang — Backend Developer</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

  :root {
    --bg: #050810;
    --surface: #0d1117;
    --accent: #00d4ff;
    --accent2: #7b61ff;
    --accent3: #ff6b35;
    --text: #e8eaf0;
    --muted: #6b7280;
    --card: rgba(13,17,23,0.85);
  }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    width: 12px; height: 12px;
    background: var(--accent);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.15s ease, opacity 0.15s ease;
    transform: translate(-50%, -50%);
    mix-blend-mode: screen;
  }
  .cursor-ring {
    width: 36px; height: 36px;
    border: 1px solid rgba(0,212,255,0.4);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9998;
    transition: transform 0.08s linear;
    transform: translate(-50%, -50%);
  }
  body:hover .cursor { opacity: 1; }

  /* Canvas */
  #canvas3d {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
  }

  /* Scrollable content */
  .content {
    position: relative;
    z-index: 10;
  }

  /* ─── HERO ─── */
  .hero {
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    padding: 0 8vw;
  }

  .hero-label {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.3em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 24px;
    opacity: 0;
    animation: fadeUp 0.8s ease 0.3s forwards;
  }

  .hero-name {
    font-size: clamp(52px, 9vw, 120px);
    font-weight: 800;
    line-height: 0.9;
    letter-spacing: -0.03em;
    margin-bottom: 24px;
    opacity: 0;
    animation: fadeUp 0.9s ease 0.5s forwards;
  }

  .hero-name .line2 {
    -webkit-text-stroke: 1.5px var(--accent);
    color: transparent;
  }

  .hero-sub {
    font-family: 'Space Mono', monospace;
    font-size: clamp(11px, 1.4vw, 14px);
    color: var(--muted);
    max-width: 440px;
    line-height: 1.8;
    margin-bottom: 48px;
    opacity: 0;
    animation: fadeUp 0.9s ease 0.7s forwards;
  }

  .hero-cta {
    display: flex;
    gap: 16px;
    opacity: 0;
    animation: fadeUp 0.9s ease 0.9s forwards;
  }

  .btn {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    text-decoration: none;
    padding: 14px 28px;
    border-radius: 2px;
    transition: all 0.25s ease;
    display: inline-block;
  }

  .btn-primary {
    background: var(--accent);
    color: #000;
    font-weight: 700;
  }
  .btn-primary:hover {
    background: #fff;
    transform: translateY(-2px);
    box-shadow: 0 0 30px rgba(0,212,255,0.4);
  }

  .btn-ghost {
    border: 1px solid rgba(255,255,255,0.15);
    color: var(--text);
  }
  .btn-ghost:hover {
    border-color: var(--accent);
    color: var(--accent);
    transform: translateY(-2px);
  }

  .scroll-hint {
    position: absolute;
    bottom: 40px;
    left: 8vw;
    display: flex;
    align-items: center;
    gap: 12px;
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
    opacity: 0;
    animation: fadeUp 1s ease 1.2s forwards;
  }

  .scroll-line {
    width: 40px;
    height: 1px;
    background: var(--muted);
    position: relative;
    overflow: hidden;
  }
  .scroll-line::after {
    content: '';
    position: absolute;
    left: -100%;
    top: 0;
    width: 100%;
    height: 100%;
    background: var(--accent);
    animation: scrollSlide 2s ease infinite;
  }
  @keyframes scrollSlide { 0%{left:-100%} 100%{left:100%} }

  /* ─── SECTIONS ─── */
  section {
    padding: 120px 8vw;
  }

  .section-tag {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.4em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-tag::before {
    content: '';
    width: 24px;
    height: 1px;
    background: var(--accent);
  }

  .section-title {
    font-size: clamp(32px, 5vw, 64px);
    font-weight: 800;
    letter-spacing: -0.02em;
    line-height: 1;
    margin-bottom: 64px;
  }

  /* ─── ABOUT ─── */
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: center;
  }

  .about-text p {
    font-size: 15px;
    line-height: 1.9;
    color: rgba(232,234,240,0.7);
    margin-bottom: 20px;
  }

  .about-text p strong {
    color: var(--text);
    font-weight: 600;
  }

  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
  }

  .stat-card {
    background: var(--card);
    border: 1px solid rgba(255,255,255,0.05);
    padding: 32px 28px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s ease;
    backdrop-filter: blur(10px);
  }

  .stat-card:hover { border-color: var(--accent); }

  .stat-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,212,255,0.03), transparent);
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  .stat-card:hover::before { opacity: 1; }

  .stat-num {
    font-size: 42px;
    font-weight: 800;
    letter-spacing: -0.04em;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 8px;
  }

  .stat-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
  }

  /* ─── SKILLS ─── */
  .skills-section { background: transparent; }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 2px;
  }

  .skill-item {
    background: var(--card);
    border: 1px solid rgba(255,255,255,0.05);
    padding: 24px;
    display: flex;
    align-items: center;
    gap: 16px;
    cursor: default;
    transition: all 0.25s ease;
    backdrop-filter: blur(10px);
    position: relative;
    overflow: hidden;
  }

  .skill-item:hover {
    border-color: rgba(0,212,255,0.3);
    transform: translateY(-3px);
  }

  .skill-icon {
    width: 36px;
    height: 36px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
  }

  .skill-name {
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.01em;
  }

  .skill-type {
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    color: var(--muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-top: 2px;
  }

  /* ─── PROJECTS ─── */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
    gap: 2px;
  }

  .project-card {
    background: var(--card);
    border: 1px solid rgba(255,255,255,0.05);
    padding: 40px;
    position: relative;
    overflow: hidden;
    cursor: default;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
  }

  .project-card:hover {
    border-color: rgba(0,212,255,0.25);
    transform: translateY(-6px);
  }

  .project-card::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0%;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    transition: width 0.4s ease;
  }
  .project-card:hover::after { width: 100%; }

  .project-number {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 0.2em;
    margin-bottom: 20px;
  }

  .project-name {
    font-size: 22px;
    font-weight: 800;
    letter-spacing: -0.02em;
    margin-bottom: 14px;
    line-height: 1.2;
  }

  .project-desc {
    font-size: 13px;
    line-height: 1.8;
    color: rgba(232,234,240,0.55);
    margin-bottom: 28px;
  }

  .project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .tag {
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    padding: 5px 10px;
    border: 1px solid rgba(255,255,255,0.1);
    color: var(--muted);
    border-radius: 2px;
    transition: all 0.2s ease;
  }

  .project-card:hover .tag {
    border-color: rgba(0,212,255,0.3);
    color: var(--accent);
  }

  /* ─── CONTACT ─── */
  .contact-section {
    min-height: 60vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .contact-big {
    font-size: clamp(36px, 6vw, 80px);
    font-weight: 800;
    letter-spacing: -0.03em;
    line-height: 1;
    margin-bottom: 40px;
  }

  .contact-big span {
    -webkit-text-stroke: 1.5px var(--accent);
    color: transparent;
  }

  .contact-links {
    display: flex;
    flex-wrap: wrap;
    gap: 24px;
    margin-bottom: 80px;
  }

  .contact-link {
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--text);
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 10px;
    transition: color 0.2s ease;
    padding-bottom: 4px;
    border-bottom: 1px solid rgba(255,255,255,0.15);
  }

  .contact-link:hover {
    color: var(--accent);
    border-bottom-color: var(--accent);
  }

  .contact-link::after {
    content: '↗';
    font-size: 14px;
  }

  /* ─── FOOTER ─── */
  footer {
    padding: 40px 8vw;
    border-top: 1px solid rgba(255,255,255,0.05);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  footer p {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
  }

  /* ─── DIVIDER ─── */
  .divider {
    width: 100%;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(0,212,255,0.2), transparent);
    margin: 0;
  }

  /* ─── ANIMATIONS ─── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .reveal {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ─── NOISE OVERLAY ─── */
  .noise {
    position: fixed;
    inset: 0;
    z-index: 1;
    pointer-events: none;
    opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='1'/%3E%3C/svg%3E");
  }

  /* ─── MOBILE ─── */
  @media (max-width: 768px) {
    .about-grid { grid-template-columns: 1fr; gap: 48px; }
    .hero-cta { flex-direction: column; }
    .contact-links { flex-direction: column; gap: 16px; }
    footer { flex-direction: column; gap: 16px; text-align: center; }
  }

  /* Glowing orbs */
  .orb {
    position: fixed;
    border-radius: 50%;
    pointer-events: none;
    filter: blur(80px);
    opacity: 0.12;
    z-index: 0;
    animation: orbFloat 8s ease-in-out infinite alternate;
  }
  .orb1 { width: 600px; height: 600px; background: var(--accent); top: -200px; right: -100px; }
  .orb2 { width: 400px; height: 400px; background: var(--accent2); bottom: 20%; left: -100px; animation-delay: -4s; }

  @keyframes orbFloat {
    from { transform: translate(0, 0) scale(1); }
    to { transform: translate(30px, 30px) scale(1.05); }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>
<div class="noise"></div>

<!-- Background orbs -->
<div class="orb orb1"></div>
<div class="orb orb2"></div>

<!-- 3D Canvas -->
<canvas id="canvas3d"></canvas>

<div class="content">

  <!-- HERO -->
  <section class="hero">
    <p class="hero-label">Available for internship · 2025</p>
    <h1 class="hero-name">
      Ngoc<br>
      <span class="line2">Hoang.</span>
    </h1>
    <p class="hero-sub">
      Backend Developer & Security Enthusiast.<br>
      Building scalable systems — NestJS, Node, Django, MongoDB.<br>
      3rd-year IT student based in Vietnam.
    </p>
    <div class="hero-cta">
      <a href="#projects" class="btn btn-primary">View Projects</a>
      <a href="#contact" class="btn btn-ghost">Get in touch</a>
    </div>
    <div class="scroll-hint">
      <div class="scroll-line"></div>
      scroll to explore
    </div>
  </section>

  <div class="divider"></div>

  <!-- ABOUT -->
  <section id="about">
    <div class="section-tag reveal">About me</div>
    <h2 class="section-title reveal">Who I am.</h2>

    <div class="about-grid">
      <div class="about-text reveal">
        <p>I'm a <strong>3rd-year IT student</strong> obsessed with backend architecture, clean APIs, and understanding how systems break under pressure.</p>
        <p>My focus is the intersection of <strong>web development and security</strong> — building systems that are not just functional, but resilient.</p>
        <p>Outside of code, I grind <strong>data structures and algorithms</strong> for sport and constantly study system design patterns to level up my engineering thinking.</p>
      </div>
      <div class="stats-grid reveal">
        <div class="stat-card">
          <div class="stat-num">100+</div>
          <div class="stat-label">DSA Problems</div>
        </div>
        <div class="stat-card">
          <div class="stat-num">3</div>
          <div class="stat-label">Projects Built</div>
        </div>
        <div class="stat-card">
          <div class="stat-num">6+</div>
          <div class="stat-label">Technologies</div>
        </div>
        <div class="stat-card">
          <div class="stat-num">∞</div>
          <div class="stat-label">Coffee Cups</div>
        </div>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- SKILLS -->
  <section id="skills">
    <div class="section-tag reveal">Tech Stack</div>
    <h2 class="section-title reveal">What I work with.</h2>

    <div class="skills-grid reveal">
      <div class="skill-item">
        <div class="skill-icon" style="background:#ED8B0015; color:#ED8B00">☕</div>
        <div><div class="skill-name">Java</div><div class="skill-type">Language</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#3776AB15; color:#3776AB">🐍</div>
        <div><div class="skill-name">Python</div><div class="skill-type">Language</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#F7DF1E15; color:#F7DF1E">JS</div>
        <div><div class="skill-name">JavaScript</div><div class="skill-type">Language</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#3178C615; color:#3178C6">TS</div>
        <div><div class="skill-name">TypeScript</div><div class="skill-type">Language</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#33993315; color:#339933">⬡</div>
        <div><div class="skill-name">Node.js</div><div class="skill-type">Runtime</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#E0234E15; color:#E0234E">N</div>
        <div><div class="skill-name">NestJS</div><div class="skill-type">Framework</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#00000030; color:#fff">Ex</div>
        <div><div class="skill-name">Express.js</div><div class="skill-type">Framework</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#092E2015; color:#44B78B">Dj</div>
        <div><div class="skill-name">Django</div><div class="skill-type">Framework</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#4EA94B15; color:#4EA94B">🍃</div>
        <div><div class="skill-name">MongoDB</div><div class="skill-type">Database</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#4479A115; color:#4479A1">SQL</div>
        <div><div class="skill-name">MySQL</div><div class="skill-type">Database</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#61DAFB15; color:#61DAFB">⚛</div>
        <div><div class="skill-name">React</div><div class="skill-type">Frontend</div></div>
      </div>
      <div class="skill-item">
        <div class="skill-icon" style="background:#06B6D415; color:#06B6D4">~</div>
        <div><div class="skill-name">Tailwind</div><div class="skill-type">Frontend</div></div>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- PROJECTS -->
  <section id="projects">
    <div class="section-tag reveal">Featured Work</div>
    <h2 class="section-title reveal">What I've built.</h2>

    <div class="projects-grid reveal">
      <div class="project-card">
        <div class="project-number">01 — Backend System</div>
        <div class="project-name">E-commerce Hub</div>
        <div class="project-desc">Full-featured e-commerce backend with user authentication, cart management, and complete payment flow. Designed for scalability with clean service architecture.</div>
        <div class="project-tags">
          <span class="tag">NestJS</span>
          <span class="tag">React</span>
          <span class="tag">MongoDB</span>
          <span class="tag">JWT</span>
        </div>
      </div>

      <div class="project-card">
        <div class="project-number">02 — Full Stack</div>
        <div class="project-name">Smart Task Manager</div>
        <div class="project-desc">Full-stack productivity app with JWT authentication, role-based access, and real-time CRUD operations. Clean UX with a solid backend foundation.</div>
        <div class="project-tags">
          <span class="tag">MongoDB</span>
          <span class="tag">Express</span>
          <span class="tag">React</span>
          <span class="tag">Node.js</span>
        </div>
      </div>

      <div class="project-card">
        <div class="project-number">03 — Algorithm Prep</div>
        <div class="project-name">DSA Mastery</div>
        <div class="project-desc">A curated collection of 100+ solved algorithm and data structure problems covering arrays, trees, graphs, and dynamic programming — built for interview readiness.</div>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag">Algorithms</span>
          <span class="tag">Data Structures</span>
        </div>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- CONTACT -->
  <section id="contact" class="contact-section">
    <div class="section-tag reveal">Contact</div>
    <h2 class="contact-big reveal">
      Let's build<br><span>something.</span>
    </h2>

    <div class="contact-links reveal">
      <a href="mailto:hoangdtntaluoi@gmail.com" class="contact-link">Email</a>
      <a href="https://linkedin.com/in/yourprofile" target="_blank" class="contact-link">LinkedIn</a>
      <a href="https://github.com/HoaAng911" target="_blank" class="contact-link">GitHub</a>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <p>© 2025 Ngoc Hoang</p>
    <p>Backend Developer · Vietnam</p>
  </footer>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ─── CURSOR ───
const cursor = document.getElementById('cursor');
const cursorRing = document.getElementById('cursorRing');
let mx = -100, my = -100, rx = -100, ry = -100;

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  cursor.style.left = mx + 'px';
  cursor.style.top = my + 'px';
});

function animCursor() {
  rx += (mx - rx) * 0.12;
  ry += (my - ry) * 0.12;
  cursorRing.style.left = rx + 'px';
  cursorRing.style.top = ry + 'px';
  requestAnimationFrame(animCursor);
}
animCursor();

document.querySelectorAll('a, button').forEach(el => {
  el.addEventListener('mouseenter', () => {
    cursor.style.transform = 'translate(-50%,-50%) scale(2.5)';
    cursor.style.opacity = '0.5';
  });
  el.addEventListener('mouseleave', () => {
    cursor.style.transform = 'translate(-50%,-50%) scale(1)';
    cursor.style.opacity = '1';
  });
});

// ─── THREE.JS ───
const canvas = document.getElementById('canvas3d');
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setClearColor(0x000000, 0);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 30;

// Particles
const particleCount = 1800;
const positions = new Float32Array(particleCount * 3);
const sizes = new Float32Array(particleCount);
const colors = new Float32Array(particleCount * 3);

for (let i = 0; i < particleCount; i++) {
  positions[i * 3] = (Math.random() - 0.5) * 120;
  positions[i * 3 + 1] = (Math.random() - 0.5) * 120;
  positions[i * 3 + 2] = (Math.random() - 0.5) * 60;
  sizes[i] = Math.random() * 2 + 0.5;

  const t = Math.random();
  if (t < 0.33) {
    colors[i * 3] = 0; colors[i * 3 + 1] = 0.83; colors[i * 3 + 2] = 1; // cyan
  } else if (t < 0.66) {
    colors[i * 3] = 0.48; colors[i * 3 + 1] = 0.38; colors[i * 3 + 2] = 1; // purple
  } else {
    colors[i * 3] = 1; colors[i * 3 + 1] = 0.42; colors[i * 3 + 2] = 0.21; // orange
  }
}

const pGeo = new THREE.BufferGeometry();
pGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
pGeo.setAttribute('color', new THREE.BufferAttribute(colors, 3));

const pMat = new THREE.PointsMaterial({
  size: 0.15,
  vertexColors: true,
  transparent: true,
  opacity: 0.6,
  sizeAttenuation: true,
});

const particles = new THREE.Points(pGeo, pMat);
scene.add(particles);

// Geometric shapes - floating wireframes
function createFloatingShape(geo, color, x, y, z) {
  const mat = new THREE.MeshBasicMaterial({
    color,
    wireframe: true,
    transparent: true,
    opacity: 0.08,
  });
  const mesh = new THREE.Mesh(geo, mat);
  mesh.position.set(x, y, z);
  scene.add(mesh);
  return mesh;
}

const shapes = [
  createFloatingShape(new THREE.IcosahedronGeometry(8, 1), 0x00d4ff, 20, 5, -10),
  createFloatingShape(new THREE.OctahedronGeometry(5, 0), 0x7b61ff, -25, -8, -5),
  createFloatingShape(new THREE.TorusGeometry(6, 0.5, 8, 30), 0xff6b35, 0, 15, -20),
  createFloatingShape(new THREE.TetrahedronGeometry(4), 0x00d4ff, -15, 12, -8),
];

// Mouse interaction
let mouseX = 0, mouseY = 0;
document.addEventListener('mousemove', e => {
  mouseX = (e.clientX / window.innerWidth - 0.5) * 2;
  mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
});

// Scroll
let scrollY = 0;
window.addEventListener('scroll', () => { scrollY = window.scrollY; });

// Animate
const clock = new THREE.Clock();

function animate() {
  requestAnimationFrame(animate);
  const t = clock.getElapsedTime();

  particles.rotation.y = t * 0.015 + mouseX * 0.08;
  particles.rotation.x = mouseY * 0.04 + scrollY * 0.0003;

  shapes.forEach((s, i) => {
    s.rotation.x = t * (0.2 + i * 0.07);
    s.rotation.y = t * (0.15 + i * 0.05);
    s.position.y += Math.sin(t * 0.5 + i) * 0.005;
  });

  camera.position.x += (mouseX * 2 - camera.position.x) * 0.02;
  camera.position.y += (-mouseY * 2 - camera.position.y) * 0.02;
  camera.lookAt(scene.position);

  renderer.render(scene, camera);
}
animate();

// Resize
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

// ─── SCROLL REVEAL ───
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
    }
  });
}, { threshold: 0.1 });

reveals.forEach(el => observer.observe(el));
</script>
</body>
</html>
