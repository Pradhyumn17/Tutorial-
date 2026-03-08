<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VISIONARY — Creative Portfolio</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Rajdhani:wght@300;400;500;600&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #c9a84c;
    --gold-light: #e8c97a;
    --gold-dark: #8a6820;
    --obsidian: #080808;
    --deep: #0e0e0f;
    --surface: #141416;
    --surface2: #1c1c1f;
    --white: #f5f0e8;
    --gray: #6b6b72;
    --gray-light: #9a9aa8;
    --accent: #ff6b35;
    --radius: 2px;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html {
    scroll-behavior: smooth;
    overflow-x: hidden;
  }

  body {
    background: var(--obsidian);
    color: var(--white);
    font-family: 'Rajdhani', sans-serif;
    overflow-x: hidden;
    cursor: none;
  }

  /* ─── CUSTOM CURSOR ─── */
  #cursor {
    position: fixed;
    width: 10px;
    height: 10px;
    background: var(--gold);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%,-50%);
    transition: transform 0.1s, width 0.3s, height 0.3s, background 0.3s;
    mix-blend-mode: difference;
  }
  #cursor-ring {
    position: fixed;
    width: 40px;
    height: 40px;
    border: 1px solid rgba(201,168,76,0.5);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%,-50%);
    transition: transform 0.15s ease-out, width 0.3s, height 0.3s, border-color 0.3s;
  }
  body.hovering #cursor { width: 16px; height: 16px; background: var(--gold-light); }
  body.hovering #cursor-ring { width: 60px; height: 60px; border-color: var(--gold); }

  /* ─── CANVAS (3D BG) ─── */
  #bg-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    pointer-events: none;
  }

  /* ─── LOADING SCREEN ─── */
  #loader {
    position: fixed;
    inset: 0;
    background: var(--obsidian);
    z-index: 10000;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    transition: opacity 0.8s ease, visibility 0.8s ease;
  }
  #loader.hidden { opacity: 0; visibility: hidden; }
  .loader-logo {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 300;
    letter-spacing: 0.5em;
    color: var(--gold);
    animation: pulse 2s ease-in-out infinite;
  }
  .loader-bar-wrap {
    margin-top: 2rem;
    width: 200px;
    height: 1px;
    background: rgba(201,168,76,0.2);
    position: relative;
    overflow: hidden;
  }
  .loader-bar {
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 100%;
    background: var(--gold);
    animation: loadBar 2s ease forwards;
  }
  .loader-percent {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    color: var(--gold);
    margin-top: 0.8rem;
    letter-spacing: 0.3em;
  }
  @keyframes loadBar { to { left: 0; } }
  @keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.5;} }

  /* ─── NAV ─── */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 1000;
    padding: 1.5rem 4vw;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: linear-gradient(to bottom, rgba(8,8,8,0.9), transparent);
    backdrop-filter: blur(4px);
    transition: padding 0.3s;
  }
  nav.scrolled {
    padding: 1rem 4vw;
    background: rgba(8,8,8,0.95);
    border-bottom: 1px solid rgba(201,168,76,0.1);
  }
  .nav-logo {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.4rem;
    font-weight: 600;
    letter-spacing: 0.3em;
    color: var(--gold);
    text-decoration: none;
  }
  .nav-links {
    display: flex;
    gap: 2.5rem;
    list-style: none;
  }
  .nav-links a {
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.8rem;
    font-weight: 500;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gray-light);
    text-decoration: none;
    transition: color 0.3s;
    position: relative;
  }
  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 0;
    width: 0; height: 1px;
    background: var(--gold);
    transition: width 0.3s;
  }
  .nav-links a:hover { color: var(--gold); }
  .nav-links a:hover::after { width: 100%; }

  /* Hamburger */
  .hamburger {
    display: none;
    flex-direction: column;
    gap: 5px;
    background: none;
    border: none;
    cursor: none;
    padding: 4px;
  }
  .hamburger span {
    display: block;
    width: 26px;
    height: 1px;
    background: var(--gold);
    transition: transform 0.3s, opacity 0.3s;
  }
  .hamburger.open span:nth-child(1) { transform: translateY(6px) rotate(45deg); }
  .hamburger.open span:nth-child(2) { opacity: 0; }
  .hamburger.open span:nth-child(3) { transform: translateY(-6px) rotate(-45deg); }

  /* Mobile menu */
  .mobile-menu {
    position: fixed;
    inset: 0;
    background: rgba(8,8,8,0.98);
    z-index: 999;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 3rem;
    transform: translateX(100%);
    transition: transform 0.5s cubic-bezier(0.77,0,0.175,1);
  }
  .mobile-menu.open { transform: translateX(0); }
  .mobile-menu a {
    font-family: 'Cormorant Garamond', serif;
    font-size: 2.5rem;
    font-weight: 300;
    color: var(--white);
    text-decoration: none;
    letter-spacing: 0.2em;
    transition: color 0.3s;
  }
  .mobile-menu a:hover { color: var(--gold); }

  /* ─── SECTIONS (General) ─── */
  section { position: relative; z-index: 1; }

  /* ─── HERO ─── */
  #hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 0 6vw;
    position: relative;
    overflow: hidden;
  }
  .hero-eyebrow {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.5em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 2rem;
    opacity: 0;
    transform: translateY(20px);
    animation: fadeUp 0.8s 2.3s forwards;
  }
  .hero-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(3.5rem, 10vw, 10rem);
    font-weight: 300;
    line-height: 0.9;
    letter-spacing: -0.02em;
    color: var(--white);
    opacity: 0;
    transform: translateY(30px);
    animation: fadeUp 1s 2.5s forwards;
    position: relative;
  }
  .hero-title em {
    font-style: italic;
    color: var(--gold);
    font-weight: 300;
  }
  .hero-subtitle {
    font-size: clamp(0.85rem, 1.5vw, 1rem);
    font-weight: 400;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--gray-light);
    margin-top: 2rem;
    opacity: 0;
    transform: translateY(20px);
    animation: fadeUp 0.8s 2.7s forwards;
  }
  .hero-cta {
    margin-top: 3.5rem;
    opacity: 0;
    animation: fadeUp 0.8s 2.9s forwards;
    display: flex;
    gap: 1.5rem;
    flex-wrap: wrap;
    justify-content: center;
  }
  .btn-primary {
    display: inline-flex;
    align-items: center;
    gap: 0.7rem;
    padding: 1rem 2.5rem;
    background: var(--gold);
    color: var(--obsidian);
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    text-decoration: none;
    border: none;
    cursor: none;
    transition: background 0.3s, transform 0.3s, box-shadow 0.3s;
    position: relative;
    overflow: hidden;
  }
  .btn-primary::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--white);
    transform: translateX(-101%);
    transition: transform 0.4s cubic-bezier(0.77,0,0.175,1);
  }
  .btn-primary:hover::before { transform: translateX(0); }
  .btn-primary:hover { box-shadow: 0 20px 60px rgba(201,168,76,0.3); }
  .btn-primary span { position: relative; z-index: 1; }
  .btn-ghost {
    display: inline-flex;
    align-items: center;
    gap: 0.7rem;
    padding: 1rem 2.5rem;
    background: transparent;
    color: var(--white);
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    text-decoration: none;
    border: 1px solid rgba(245,240,232,0.3);
    cursor: none;
    transition: border-color 0.3s, color 0.3s;
  }
  .btn-ghost:hover { border-color: var(--gold); color: var(--gold); }

  .hero-scroll {
    position: absolute;
    bottom: 2.5rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    opacity: 0;
    animation: fadeUp 0.8s 3.2s forwards;
  }
  .hero-scroll span {
    font-family: 'Space Mono', monospace;
    font-size: 0.55rem;
    letter-spacing: 0.3em;
    color: var(--gray);
    writing-mode: horizontal-tb;
  }
  .scroll-line {
    width: 1px;
    height: 60px;
    background: linear-gradient(to bottom, var(--gold), transparent);
    animation: scrollLine 2s ease-in-out infinite;
  }
  @keyframes scrollLine {
    0% { transform: scaleY(0); transform-origin: top; }
    50% { transform: scaleY(1); transform-origin: top; }
    51% { transform: scaleY(1); transform-origin: bottom; }
    100% { transform: scaleY(0); transform-origin: bottom; }
  }

  .hero-grid-lines {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba(201,168,76,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(201,168,76,0.03) 1px, transparent 1px);
    background-size: 80px 80px;
    pointer-events: none;
  }

  @keyframes fadeUp {
    to { opacity: 1; transform: translateY(0); }
  }

  /* ─── REEL / SHOWREEL ─── */
  #reel {
    padding: 8rem 4vw;
    position: relative;
  }
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem;
    letter-spacing: 0.5em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  .section-label::before {
    content: '';
    display: block;
    width: 40px;
    height: 1px;
    background: var(--gold);
  }
  .section-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(2.2rem, 5vw, 5rem);
    font-weight: 300;
    line-height: 1.05;
    margin-bottom: 3rem;
  }
  .section-title em { font-style: italic; color: var(--gold); }

  .reel-container {
    position: relative;
    width: 100%;
    aspect-ratio: 16/9;
    background: var(--surface);
    border: 1px solid rgba(201,168,76,0.15);
    overflow: hidden;
    cursor: none;
  }
  .reel-container video {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
  .reel-overlay {
    position: absolute;
    inset: 0;
    background: rgba(8,8,8,0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.4s;
  }
  .reel-container.playing .reel-overlay { background: transparent; }
  .play-btn {
    width: 80px;
    height: 80px;
    border: 1px solid rgba(201,168,76,0.6);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: none;
    transition: transform 0.3s, border-color 0.3s, background 0.3s;
    background: rgba(8,8,8,0.4);
    backdrop-filter: blur(8px);
  }
  .play-btn:hover { transform: scale(1.15); border-color: var(--gold); background: rgba(201,168,76,0.1); }
  .play-btn svg { margin-left: 4px; }
  .reel-container.playing .play-btn { opacity: 0; pointer-events: none; }

  /* Video Controls */
  .video-controls {
    position: absolute;
    bottom: 0; left: 0; right: 0;
    padding: 1.5rem 1.5rem 1rem;
    background: linear-gradient(transparent, rgba(8,8,8,0.9));
    display: flex;
    flex-direction: column;
    gap: 0.7rem;
    transform: translateY(100%);
    transition: transform 0.3s;
  }
  .reel-container:hover .video-controls { transform: translateY(0); }
  .progress-bar {
    width: 100%;
    height: 2px;
    background: rgba(255,255,255,0.2);
    cursor: none;
    position: relative;
  }
  .progress-fill {
    height: 100%;
    background: var(--gold);
    width: 0%;
    transition: width 0.1s linear;
  }
  .controls-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }
  .ctrl-btn {
    background: none;
    border: none;
    color: var(--white);
    cursor: none;
    padding: 4px;
    transition: color 0.2s;
  }
  .ctrl-btn:hover { color: var(--gold); }
  .time-display {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--gray-light);
    letter-spacing: 0.05em;
  }
  .vol-slider {
    -webkit-appearance: none;
    width: 70px;
    height: 2px;
    background: rgba(255,255,255,0.3);
    outline: none;
    cursor: none;
  }
  .vol-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: var(--gold);
    cursor: none;
  }

  /* Reel placeholder (demo video look) */
  .reel-placeholder {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
    background: linear-gradient(135deg, #0e0e0f 0%, #1c1218 50%, #0e0e0f 100%);
  }
  .reel-placeholder-text {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.2rem;
    color: rgba(201,168,76,0.4);
    letter-spacing: 0.3em;
    text-transform: uppercase;
  }
  .reel-placeholder-hint {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem;
    color: var(--gray);
    letter-spacing: 0.2em;
  }

  /* ─── ABOUT / STATS ─── */
  #about {
    padding: 8rem 4vw;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6rem;
    align-items: center;
  }
  .about-text p {
    font-size: 1.05rem;
    line-height: 1.85;
    color: var(--gray-light);
    margin-bottom: 1.5rem;
    font-weight: 300;
  }
  .about-text p strong { color: var(--white); font-weight: 500; }
  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
  }
  .stat-card {
    padding: 2rem;
    border: 1px solid rgba(201,168,76,0.15);
    background: rgba(20,20,22,0.6);
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, transform 0.3s;
  }
  .stat-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 2px;
    background: var(--gold);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s;
  }
  .stat-card:hover { border-color: rgba(201,168,76,0.4); transform: translateY(-4px); }
  .stat-card:hover::before { transform: scaleX(1); }
  .stat-number {
    font-family: 'Cormorant Garamond', serif;
    font-size: 3.5rem;
    font-weight: 300;
    color: var(--gold);
    line-height: 1;
    margin-bottom: 0.3rem;
  }
  .stat-label {
    font-size: 0.75rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gray);
  }

  /* ─── WORK / PORTFOLIO ─── */
  #work {
    padding: 8rem 4vw;
  }
  .work-header {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    margin-bottom: 4rem;
    flex-wrap: wrap;
    gap: 2rem;
  }

  /* Filter tabs */
  .filter-tabs {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
  }
  .filter-tab {
    padding: 0.5rem 1.2rem;
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.75rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    background: transparent;
    border: 1px solid rgba(255,255,255,0.1);
    color: var(--gray);
    cursor: none;
    transition: all 0.3s;
  }
  .filter-tab.active, .filter-tab:hover {
    border-color: var(--gold);
    color: var(--gold);
    background: rgba(201,168,76,0.05);
  }

  /* Portfolio grid */
  .portfolio-grid {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: 1.5rem;
  }
  .portfolio-item {
    position: relative;
    overflow: hidden;
    cursor: none;
    background: var(--surface);
    transition: transform 0.5s cubic-bezier(0.34,1.56,0.64,1);
  }
  .portfolio-item:nth-child(1) { grid-column: span 7; }
  .portfolio-item:nth-child(2) { grid-column: span 5; }
  .portfolio-item:nth-child(3) { grid-column: span 4; }
  .portfolio-item:nth-child(4) { grid-column: span 4; }
  .portfolio-item:nth-child(5) { grid-column: span 4; }
  .portfolio-item:nth-child(6) { grid-column: span 6; }
  .portfolio-item:nth-child(7) { grid-column: span 6; }
  .portfolio-item:hover { transform: scale(1.02); z-index: 2; }

  .portfolio-thumb {
    width: 100%;
    aspect-ratio: 16/10;
    position: relative;
    overflow: hidden;
  }
  .portfolio-thumb video,
  .portfolio-thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.6s ease;
    display: block;
  }
  .portfolio-item:hover .portfolio-thumb video,
  .portfolio-item:hover .portfolio-thumb img { transform: scale(1.08); }

  /* Gradient placeholder for demo */
  .thumb-placeholder {
    width: 100%;
    height: 100%;
    position: relative;
  }
  .portfolio-overlay {
    position: absolute;
    inset: 0;
    background: rgba(8,8,8,0);
    display: flex;
    align-items: flex-end;
    padding: 1.5rem;
    transition: background 0.4s;
  }
  .portfolio-item:hover .portfolio-overlay { background: rgba(8,8,8,0.6); }
  .portfolio-info {
    transform: translateY(10px);
    opacity: 0;
    transition: transform 0.4s, opacity 0.4s;
    width: 100%;
  }
  .portfolio-item:hover .portfolio-info { transform: translateY(0); opacity: 1; }
  .portfolio-info h3 {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.3rem;
    font-weight: 400;
    color: var(--white);
    margin-bottom: 0.3rem;
  }
  .portfolio-info p {
    font-size: 0.7rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gold);
  }
  .portfolio-play {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(0);
    width: 56px;
    height: 56px;
    border: 1px solid rgba(201,168,76,0.7);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(8,8,8,0.5);
    transition: transform 0.3s cubic-bezier(0.34,1.56,0.64,1);
  }
  .portfolio-item:hover .portfolio-play { transform: translate(-50%,-50%) scale(1); }

  /* ─── SERVICES ─── */
  #services {
    padding: 8rem 4vw;
    background: var(--deep);
  }
  .services-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2px;
    margin-top: 4rem;
    background: rgba(201,168,76,0.1);
  }
  .service-card {
    background: var(--obsidian);
    padding: 3rem 2.5rem;
    position: relative;
    overflow: hidden;
    transition: background 0.4s;
  }
  .service-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(201,168,76,0.08) 0%, transparent 60%);
    opacity: 0;
    transition: opacity 0.4s;
  }
  .service-card:hover { background: var(--surface); }
  .service-card:hover::before { opacity: 1; }
  .service-num {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--gold);
    letter-spacing: 0.3em;
    margin-bottom: 2rem;
  }
  .service-icon {
    width: 48px;
    height: 48px;
    margin-bottom: 1.5rem;
    color: var(--gold);
  }
  .service-card h3 {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.6rem;
    font-weight: 300;
    margin-bottom: 1rem;
    color: var(--white);
  }
  .service-card p {
    font-size: 0.9rem;
    line-height: 1.7;
    color: var(--gray);
    font-weight: 300;
  }
  .service-line {
    position: absolute;
    bottom: 0; left: 0;
    width: 0; height: 1px;
    background: var(--gold);
    transition: width 0.5s;
  }
  .service-card:hover 
