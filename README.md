<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, maximum-scale=1, user-scalable=no">
<title>FitQuest Pro — Тренировки, Кейсы, Сумка и Дорога наград</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg-base: #0B0D14;
    --bg-surface: #141724;
    --bg-card: rgba(23, 27, 43, 0.88);
    --bg-card-hover: rgba(32, 38, 59, 0.95);
    --border: rgba(255, 255, 255, 0.08);
    --border-highlight: rgba(255, 255, 255, 0.18);
    --text-main: #FFFFFF;
    --text-muted: #8E94AA;
    --text-dim: #5B6178;

    /* Акцентные цвета */
    --primary: #FF3B5C;
    --primary-glow: rgba(255, 59, 92, 0.45);
    --primary-gradient: linear-gradient(135deg, #FF3B5C 0%, #FF6584 100%);
    --violet: #7B5CFA;
    --violet-glow: rgba(123, 92, 250, 0.45);
    --violet-gradient: linear-gradient(135deg, #7B5CFA 0%, #9C82FF 100%);
    --cyan: #00E5FF;
    --cyan-glow: rgba(0, 229, 255, 0.4);
    --gold: #FFB800;
    --gold-glow: rgba(255, 184, 0, 0.45);
    --gold-gradient: linear-gradient(135deg, #FFB800 0%, #FF8A00 100%);
    --emerald: #00E676;
    --emerald-glow: rgba(0, 230, 118, 0.4);
    --fire: #FF5722;
    --fire-glow: rgba(255, 87, 34, 0.4);
    --danger: #FF334B;

    /* Редкости */
    --rarity-common: #7E8B9B;
    --rarity-rare: #00B4D8;
    --rarity-epic: #9D4EDD;
    --rarity-legendary: #FFB703;
    --rarity-mythic: #FF007F;

    --radius-sm: 10px;
    --radius-md: 16px;
    --radius-lg: 22px;
    --radius-xl: 32px;
    --radius-pill: 999px;

    --shadow-card: 0 10px 30px rgba(0, 0, 0, 0.45);
    --shadow-glow-primary: 0 8px 24px var(--primary-glow);
    --shadow-glow-gold: 0 8px 24px var(--gold-glow);
    --shadow-glow-violet: 0 8px 24px var(--violet-glow);

    box-sizing: border-box;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

  html, body {
    height: 100%;
    width: 100%;
    overflow: hidden;
    background: #06070B;
    font-family: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    color: var(--text-main);
    user-select: none;
    -webkit-user-select: none;
  }

  body {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* Мобильная оболочка с внутренним холстом анимаций */
  #app-shell {
    position: relative;
    width: 100%;
    max-width: 480px;
    height: 100%;
    background: var(--bg-base);
    display: flex;
    flex-direction: column;
    overflow: hidden;
    z-index: 2;
    touch-action: pan-y;
  }

  @media (min-width: 500px) {
    #app-shell {
      height: 94vh;
      max-height: 920px;
      border-radius: var(--radius-xl);
      border: 1px solid rgba(255, 255, 255, 0.12);
      box-shadow: 0 25px 80px rgba(0, 0, 0, 0.8), 0 0 40px rgba(123, 92, 250, 0.15);
    }
  }

  /* Фоновый холст СТРОГО ВНУТРИ оболочки приложения */
  #ambient-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 2;
    border-radius: inherit;
  }

  /* Основная область просмотра контента */
  #view-container {
    flex: 1;
    position: relative;
    overflow: hidden;
    z-index: 5;
  }

  #view {
    width: 100%;
    height: 100%;
    overflow-y: auto;
    overflow-x: hidden;
    padding-bottom: 96px;
    scroll-behavior: smooth;
    -webkit-overflow-scrolling: touch;
  }

  #view::-webkit-scrollbar { width: 0; display: none; }

  /* Анимации смены экранов (свайп/перелистывание) */
  .slide-in-right { animation: slideInRight 0.28s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
  .slide-in-left { animation: slideInLeft 0.28s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
  .fade-in { animation: fadeIn 0.22s ease forwards; }

  @keyframes slideInRight {
    from { opacity: 0; transform: translateX(36px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes slideInLeft {
    from { opacity: 0; transform: translateX(-36px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* Индикатор жеста свайпа */
  .swipe-indicator-bar {
    position: absolute;
    top: 6px;
    left: 50%;
    transform: translateX(-50%);
    width: 38px;
    height: 4px;
    background: rgba(255, 255, 255, 0.15);
    border-radius: 4px;
    z-index: 10;
  }

  /* ---------------- КНОПКИ (PREMIUM BUTTONS) ---------------- */
  button {
    font-family: inherit;
    border: none;
    outline: none;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    transition: all 0.2s cubic-bezier(0.2, 0.8, 0.2, 1);
  }

  button:active:not(:disabled) { transform: scale(0.96) translateY(1px); }

  button:disabled {
    opacity: 0.45;
    cursor: not-allowed;
    filter: grayscale(0.5);
  }

  .btn-premium-primary {
    background: var(--primary-gradient);
    color: #FFFFFF;
    font-weight: 800;
    font-size: 14px;
    letter-spacing: 0.2px;
    padding: 14px 20px;
    border-radius: var(--radius-md);
    box-shadow: 0 6px 20px var(--primary-glow);
    border: 1px solid rgba(255, 255, 255, 0.25);
  }
  .btn-premium-primary::before {
    content: '';
    position: absolute;
    top: 0; left: -100%; width: 100%; height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
    animation: btnShine 4s infinite ease-in-out;
  }
  @keyframes btnShine {
    0%, 70% { left: -100%; }
    100% { left: 100%; }
  }

  .btn-premium-gold {
    background: var(--gold-gradient);
    color: #120A00;
    font-weight: 800;
    font-size: 14px;
    padding: 14px 20px;
    border-radius: var(--radius-md);
    box-shadow: 0 6px 22px var(--gold-glow);
    border: 1px solid rgba(255, 255, 255, 0.4);
  }

  .btn-premium-emerald {
    background: linear-gradient(135deg, #00E676 0%, #00B4D8 100%);
    color: #041E11;
    font-weight: 800;
    font-size: 14px;
    padding: 14px 20px;
    border-radius: var(--radius-md);
    box-shadow: 0 6px 22px var(--emerald-glow);
    border: 1px solid rgba(255, 255, 255, 0.4);
  }

  .btn-premium-violet {
    background: var(--violet-gradient);
    color: #FFFFFF;
    font-weight: 800;
    font-size: 14px;
    padding: 14px 20px;
    border-radius: var(--radius-md);
    box-shadow: 0 6px 20px var(--violet-glow);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .btn-premium-ghost {
    background: rgba(255, 255, 255, 0.06);
    color: var(--text-main);
    font-weight: 700;
    font-size: 13.5px;
    padding: 13px 18px;
    border-radius: var(--radius-md);
    border: 1px solid var(--border-highlight);
    backdrop-filter: blur(10px);
  }
  .btn-premium-ghost:hover { background: rgba(255, 255, 255, 0.1); }

  .btn-premium-danger {
    background: rgba(255, 51, 75, 0.15);
    color: var(--danger);
    font-weight: 700;
    font-size: 13px;
    padding: 12px 18px;
    border-radius: var(--radius-md);
    border: 1px solid rgba(255, 51, 75, 0.35);
  }

  .btn-chip {
    padding: 7px 14px;
    border-radius: var(--radius-pill);
    font-size: 11.5px;
    font-weight: 800;
    border: 1px solid var(--border);
    background: rgba(255, 255, 255, 0.05);
    color: var(--text-muted);
    flex-shrink: 0;
    white-space: nowrap;
  }
  .btn-chip.active {
    background: var(--primary);
    color: #FFF;
    border-color: var(--primary);
    box-shadow: 0 4px 14px var(--primary-glow);
  }

  /* Карточки */
  .card {
    background: var(--bg-card);
    border-radius: var(--radius-lg);
    border: 1px solid var(--border);
    backdrop-filter: blur(16px);
    box-shadow: var(--shadow-card);
    padding: 16px;
    position: relative;
    transition: transform 0.2s ease, border-color 0.2s ease;
  }
  .card:hover { border-color: var(--border-highlight); }

  /* Разметка экранов */
  .screen-pad { padding: 20px 18px; }
  .screen-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 18px;
    gap: 8px;
  }
  .screen-title {
    font-size: 22px;
    font-weight: 900;
    letter-spacing: -0.5px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .screen-sub {
    font-size: 12.5px;
    color: var(--text-muted);
    margin-top: 3px;
  }

  .coins-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(255, 184, 0, 0.12);
    border: 1px solid rgba(255, 184, 0, 0.35);
    padding: 8px 14px;
    border-radius: var(--radius-pill);
    color: var(--gold);
    font-weight: 800;
    font-size: 13.5px;
    box-shadow: 0 4px 14px rgba(255, 184, 0, 0.15);
    flex-shrink: 0;
  }

  /* Главный баннер уровня игрока */
  .hero-level-card {
    background: linear-gradient(135deg, #FF3B5C 0%, #7B5CFA 55%, #00E5FF 120%);
    border-radius: var(--radius-xl);
    padding: 22px 20px;
    color: #FFFFFF;
    position: relative;
    overflow: hidden;
    margin-bottom: 14px;
    box-shadow: 0 12px 36px rgba(123, 92, 250, 0.35);
  }
  .hero-level-card::after {
    content: '';
    position: absolute;
    right: -20px;
    top: -20px;
    width: 140px;
    height: 140px;
    background: radial-gradient(circle, rgba(255,255,255,0.2) 0%, transparent 70%);
    pointer-events: none;
  }
  .level-row-top { display: flex; justify-content: space-between; align-items: flex-start; }
  .level-num { font-size: 32px; font-weight: 900; line-height: 1; }
  .rank-badge {
    display: inline-block;
    padding: 4px 12px;
    background: rgba(0, 0, 0, 0.25);
    backdrop-filter: blur(8px);
    border-radius: var(--radius-pill);
    font-size: 11px;
    font-weight: 800;
    letter-spacing: 0.5px;
    margin-top: 6px;
    border: 1px solid rgba(255, 255, 255, 0.25);
  }
  .hero-char-avatar {
    width: 62px;
    height: 62px;
    border-radius: var(--radius-lg);
    background: rgba(255, 255, 255, 0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 32px;
    border: 2px solid rgba(255, 255, 255, 0.4);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
  }
  .hero-xp-track {
    height: 8px;
    background: rgba(0, 0, 0, 0.3);
    border-radius: 4px;
    overflow: hidden;
    margin-top: 16px;
  }
  .hero-xp-fill {
    height: 100%;
    background: #FFFFFF;
    border-radius: 4px;
    box-shadow: 0 0 10px #FFFFFF;
    transition: width 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
  }
  .hero-xp-meta {
    display: flex;
    justify-content: space-between;
    font-size: 11px;
    font-weight: 700;
    margin-top: 6px;
    opacity: 0.9;
  }
  .hero-bonus-line {
    margin-top: 10px;
    font-size: 11px;
    font-weight: 700;
    background: rgba(0,0,0,0.22);
    border-radius: var(--radius-sm);
    padding: 6px 10px;
    display: inline-block;
  }

  /* Быстрые действия на Главной */
  .quick-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    margin-bottom: 22px;
  }
  .quick-card {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 12px 10px;
    border-radius: var(--radius-md);
    color: #FFFFFF;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    border: 1px solid rgba(255, 255, 255, 0.15);
  }
  .quick-card.cases-btn {
    background: linear-gradient(135deg, #FF9500 0%, #FF3B5C 100%);
    box-shadow: 0 8px 20px rgba(255, 59, 92, 0.3);
  }
  .quick-card.clicker-btn {
    background: linear-gradient(135deg, #7B5CFA 0%, #00E5FF 100%);
    box-shadow: 0 8px 20px rgba(123, 92, 250, 0.3);
  }
  .quick-card.builder-btn {
    background: linear-gradient(135deg, #00E676 0%, #00B4D8 100%);
    box-shadow: 0 8px 20px rgba(0, 230, 118, 0.3);
  }
  .quick-card.bag-btn {
    background: linear-gradient(135deg, #FFB800 0%, #FF5722 100%);
    box-shadow: 0 8px 20px rgba(255, 140, 0, 0.3);
  }
  .quick-card .q-ic { font-size: 22px; margin-bottom: 5px; }
  .quick-card .q-title { font-weight: 800; font-size: 12px; }
  .quick-card .q-sub { font-size: 9.5px; opacity: 0.9; margin-top: 2px; text-align: left; }

  /* Секции */
  .section-title-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }
  .section-title {
    font-size: 15px;
    font-weight: 800;
    letter-spacing: -0.2px;
    color: var(--text-main);
  }
  .section-link {
    background: none;
    color: var(--primary);
    font-size: 12px;
    font-weight: 700;
  }

  /* ---------------- ШКАЛЫ ЭНЕРГИИ ---------------- */
  .stat-bars { display: grid; gap: 10px; margin-bottom: 20px; }
  .bar-card { padding: 12px 14px; border-radius: var(--radius-md); }
  .bar-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    font-weight: 800;
    margin-bottom: 8px;
  }
  .bar-track {
    height: 10px;
    background: rgba(255, 255, 255, 0.08);
    border-radius: 5px;
    overflow: hidden;
  }
  .bar-fill {
    height: 100%;
    border-radius: 5px;
    transition: width 0.35s ease;
  }
  .bar-fill.w { background: linear-gradient(90deg, #00E676, #00E5FF); box-shadow: 0 0 10px rgba(0,230,118,0.5); }
  .bar-fill.c { background: linear-gradient(90deg, var(--cyan), var(--violet)); box-shadow: 0 0 10px rgba(123,92,250,0.5); }
  .bar-fill.low { background: linear-gradient(90deg, #FF3B5C, #FF8A00); box-shadow: 0 0 10px rgba(255,59,92,0.5); }
  .bar-sub {
    display: flex;
    justify-content: space-between;
    font-size: 10.5px;
    color: var(--text-muted);
    margin-top: 7px;
    font-weight: 600;
  }
  .boost-row { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }
  .boost-pill {
    font-size: 10.5px;
    font-weight: 800;
    padding: 4px 10px;
    border-radius: var(--radius-pill);
    background: rgba(157, 78, 221, 0.2);
    border: 1px solid rgba(157, 78, 221, 0.5);
    color: #D9A8FF;
  }

  /* Квесты */
  .quest-row { display: flex; align-items: center; gap: 12px; padding: 12px 14px; margin-bottom: 8px; }
  .quest-icon-wrap {
    width: 44px;
    height: 44px;
    border-radius: var(--radius-md);
    background: rgba(255, 255, 255, 0.05);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 22px;
    flex-shrink: 0;
  }
  .quest-info { flex: 1; min-width: 0; }
  .quest-name { font-size: 12.5px; font-weight: 700; margin-bottom: 4px; }
  .quest-bar { height: 6px; background: rgba(255, 255, 255, 0.1); border-radius: 3px; overflow: hidden; margin-bottom: 4px; }
  .quest-fill { height: 100%; background: var(--emerald); border-radius: 3px; }
  .quest-meta { font-size: 10.5px; color: var(--gold); font-weight: 800; }

  /* Карточки тренировок */
  .workout-item { display: flex; align-items: center; gap: 14px; margin-bottom: 10px; padding: 14px; }
  .workout-icon-wrap {
    width: 52px;
    height: 52px;
    border-radius: var(--radius-md);
    background: rgba(255, 59, 92, 0.12);
    border: 1px solid rgba(255, 59, 92, 0.25);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 26px;
    flex-shrink: 0;
  }
  .workout-details { flex: 1; min-width: 0; }
  .workout-title { font-size: 14px; font-weight: 800; margin-bottom: 3px; }
  .workout-desc { font-size: 11px; color: var(--text-muted); line-height: 1.3; }
  .workout-rate-chips { display: flex; gap: 6px; margin-top: 6px; font-size: 10px; font-weight: 700; flex-wrap: wrap; }
  .rate-chip { padding: 2px 7px; border-radius: 6px; background: rgba(255, 255, 255, 0.05); color: var(--text-muted); }
  .rate-chip b { color: var(--text-main); }
  .rate-chip.fire { color: #FFA27B; background: rgba(255, 87, 34, 0.15); border: 1px solid rgba(255, 87, 34, 0.3); }
  .rate-chip.energy { color: #7DFFB5; background: rgba(0, 230, 118, 0.12); border: 1px solid rgba(0, 230, 118, 0.35); }
  .rate-chip.energy.bad { color: #FF8A9B; background: rgba(255, 51, 75, 0.14); border: 1px solid rgba(255, 51, 75, 0.4); }
  .cat-tag {
    display: inline-block;
    font-size: 9px;
    font-weight: 800;
    padding: 2px 8px;
    border-radius: var(--radius-pill);
    background: rgba(123, 92, 250, 0.2);
    color: #BFAEFF;
    margin-bottom: 4px;
    letter-spacing: 0.3px;
  }

  .cat-scroll {
    display: flex;
    gap: 8px;
    overflow-x: auto;
    margin: 0 -18px 14px;
    padding: 2px 18px 6px;
    -webkit-overflow-scrolling: touch;
  }
  .cat-scroll::-webkit-scrollbar { display: none; }

  /* ---------------- СЕТКА ПЕРСОНАЖЕЙ ---------------- */
  .char-filter-tabs { display: flex; gap: 8px; margin-bottom: 14px; overflow-x: auto; }
  .char-filter-tabs::-webkit-scrollbar { display: none; }
  .characters-grid { display: grid; grid-template-columns: 1fr; gap: 12px; }
  .char-box {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 14px;
    border-radius: var(--radius-lg);
    background: var(--bg-card);
    border: 1.5px solid var(--border);
    position: relative;
    overflow: hidden;
  }
  .char-box.rarity-mythic {
    border-color: var(--rarity-mythic);
    box-shadow: 0 0 20px rgba(255, 0, 127, 0.25);
    background: linear-gradient(135deg, rgba(255, 0, 127, 0.12) 0%, rgba(20, 23, 36, 0.95) 100%);
  }
  .char-box.rarity-legendary {
    border-color: var(--rarity-legendary);
    box-shadow: 0 0 18px rgba(255, 183, 3, 0.2);
    background: linear-gradient(135deg, rgba(255, 183, 3, 0.1) 0%, rgba(20, 23, 36, 0.95) 100%);
  }
  .char-box.rarity-epic { border-color: var(--rarity-epic); box-shadow: 0 0 15px rgba(157, 78, 221, 0.18); }
  .char-box.rarity-rare { border-color: var(--rarity-rare); }

  .char-avatar-frame {
    width: 60px;
    height: 60px;
    border-radius: var(--radius-md);
    background: rgba(255, 255, 255, 0.08);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    flex-shrink: 0;
    position: relative;
    border: 2px solid var(--border-highlight);
  }
  .char-avatar-frame.locked { filter: grayscale(1); opacity: 0.55; background: rgba(0,0,0,0.4); }
  .char-meta-col { flex: 1; min-width: 0; }
  .char-headline { display: flex; align-items: center; gap: 6px; flex-wrap: wrap; margin-bottom: 2px; }
  .char-name-txt { font-weight: 800; font-size: 14px; }
  .rarity-pill {
    font-size: 9px;
    font-weight: 800;
    padding: 2px 7px;
    border-radius: var(--radius-pill);
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: #FFF;
  }
  .rarity-pill.common { background: var(--rarity-common); }
  .rarity-pill.rare { background: var(--rarity-rare); }
  .rarity-pill.epic { background: var(--rarity-epic); }
  .rarity-pill.legendary { background: var(--rarity-legendary); color: #000; }
  .rarity-pill.mythic { background: var(--rarity-mythic); box-shadow: 0 0 8px var(--rarity-mythic); }

  .unlock-source-tag {
    font-size: 9.5px;
    font-weight: 800;
    padding: 2px 7px;
    border-radius: var(--radius-pill);
    background: rgba(255, 255, 255, 0.08);
    color: var(--cyan);
  }
  .unlock-source-tag.case { color: var(--gold); }

  .char-sport-lbl { font-size: 11px; color: var(--cyan); font-weight: 700; margin-bottom: 3px; }
  .char-perk-lbl { font-size: 10.5px; color: var(--text-muted); line-height: 1.3; }
  .char-btn-col { display: flex; flex-direction: column; gap: 6px; align-items: flex-end; flex-shrink: 0; }

  /* ---------------- КОНСТРУКТОР ТРЕНИРОВКИ ---------------- */
  .builder-exercise-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 18px;
    max-height: 44vh;
    overflow-y: auto;
    padding-right: 4px;
  }
  .builder-item-card {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px;
    border-radius: var(--radius-md);
    background: rgba(255, 255, 255, 0.04);
    border: 1.5px solid var(--border);
    transition: all 0.2s ease;
  }
  .builder-item-card.selected { border-color: var(--primary); background: rgba(255, 59, 92, 0.08); }
  .builder-item-check {
    width: 22px;
    height: 22px;
    border-radius: 6px;
    border: 2px solid var(--border-highlight);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 900;
    cursor: pointer;
    flex-shrink: 0;
    transition: all 0.15s ease;
  }
  .builder-item-card.selected .builder-item-check { background: var(--primary); border-color: var(--primary); color: #FFF; }
  .builder-item-icon {
    font-size: 24px;
    width: 38px;
    height: 38px;
    background: rgba(255,255,255,0.06);
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  .builder-item-info { flex: 1; min-width: 0; }
  .builder-item-name { font-size: 13px; font-weight: 800; margin-bottom: 2px; }
  .builder-item-muscle { font-size: 10px; color: var(--text-muted); }

  .builder-item-timectrl {
    display: flex;
    align-items: center;
    gap: 6px;
    background: rgba(0, 0, 0, 0.35);
    border: 1px solid var(--border);
    padding: 3px 6px;
    border-radius: var(--radius-pill);
    flex-shrink: 0;
  }
  .time-stepper-btn {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.1);
    color: #FFF;
    font-weight: 900;
    font-size: 14px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }
  .time-stepper-btn:active { background: var(--primary); }
  .time-stepper-val { font-size: 11.5px; font-weight: 800; min-width: 44px; text-align: center; color: var(--cyan); }

  /* ---------------- РУЛЕТКА КЕЙСОВ ---------------- */
  .case-banner-card { text-align: center; padding: 20px 16px; margin-bottom: 0; position: relative; overflow: hidden; }
  .case-banner-card.daily-card {
    background: linear-gradient(135deg, rgba(0, 230, 118, 0.15) 0%, rgba(20, 23, 36, 0.95) 100%);
    border-color: var(--emerald);
    box-shadow: 0 0 25px rgba(0, 230, 118, 0.2);
  }
  .case-banner-icon { font-size: 54px; margin-bottom: 6px; filter: drop-shadow(0 8px 16px rgba(255,184,0,0.3)); }
  .case-banner-title { font-size: 17px; font-weight: 900; margin-bottom: 4px; }
  .case-banner-desc { font-size: 11.5px; color: var(--text-muted); margin-bottom: 10px; }
  .case-loot-row { display: flex; justify-content: center; gap: 6px; flex-wrap: wrap; margin-bottom: 12px; font-size: 10.5px; font-weight: 800; color: var(--text-muted); }

  .roulette-container {
    position: relative;
    width: 100%;
    height: 125px;
    background: #090B12;
    border-radius: var(--radius-md);
    border: 2px solid var(--gold);
    box-shadow: 0 0 25px rgba(255, 184, 0, 0.25), inset 0 0 20px rgba(0,0,0,0.8);
    overflow: hidden;
    margin: 16px 0;
  }
  .roulette-pointer {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 4px;
    background: var(--primary);
    box-shadow: 0 0 16px var(--primary);
    z-index: 10;
  }
  .roulette-pointer::before, .roulette-pointer::after {
    content: '';
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 0; height: 0;
    border-left: 8px solid transparent;
    border-right: 8px solid transparent;
  }
  .roulette-pointer::before { top: 0; border-top: 10px solid var(--primary); }
  .roulette-pointer::after { bottom: 0; border-bottom: 10px solid var(--primary); }
  .roulette-track {
    display: flex;
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    transition: transform 4.5s cubic-bezier(0.12, 0.85, 0.1, 1);
  }
  .roulette-card-item {
    width: 96px;
    height: 100%;
    flex-shrink: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 4px;
    border-right: 1px solid rgba(255, 255, 255, 0.08);
    background: rgba(23, 27, 43, 0.95);
    padding: 6px;
    position: relative;
  }
  .roulette-card-item .item-ic { font-size: 32px; }
  .roulette-card-item .item-lbl { font-size: 10px; font-weight: 800; text-align: center; color: #FFF; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 86px; }
  .roulette-card-item .item-rarity-bar { position: absolute; bottom: 0; left: 0; right: 0; height: 3px; }

  /* ---------------- ТАПАЛКА / КЛИКЕР ---------------- */
  .clicker-container { display: flex; flex-direction: column; align-items: center; padding: 18px 16px; text-align: center; }
  .energy-pill-box {
    width: 100%;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 14px 16px;
    margin-bottom: 20px;
  }
  .energy-hdr { display: flex; justify-content: space-between; font-size: 12px; font-weight: 800; margin-bottom: 8px; }
  .energy-progress-bar { height: 10px; background: rgba(255,255,255,0.08); border-radius: 5px; overflow: hidden; }
  .energy-progress-fill { height: 100%; background: linear-gradient(90deg, var(--cyan), var(--violet)); border-radius: 5px; transition: width 0.15s ease; }
  .energy-sub { display: flex; justify-content: space-between; font-size: 10.5px; color: var(--text-muted); margin-top: 7px; font-weight: 600; }

  .punch-circle-btn {
    width: 190px;
    height: 190px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #FF5E7E 0%, #FF3B5C 50%, #7B5CFA 100%);
    box-shadow: 0 16px 45px rgba(255, 59, 92, 0.4), inset 0 -6px 14px rgba(0,0,0,0.4), inset 0 6px 14px rgba(255,255,255,0.4);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 72px;
    cursor: pointer;
    border: 4px solid rgba(255, 255, 255, 0.3);
    margin-bottom: 20px;
    transition: transform 0.08s cubic-bezier(0.2, 0.8, 0.2, 1);
  }
  .punch-circle-btn:active { transform: scale(0.91); box-shadow: 0 6px 20px rgba(255, 59, 92, 0.6); }

  .floating-punch-text {
    position: absolute;
    pointer-events: none;
    font-weight: 900;
    font-size: 16px;
    color: var(--gold);
    text-shadow: 0 2px 6px rgba(0,0,0,0.8);
    animation: floatPunch 0.75s ease-out forwards;
    z-index: 99;
  }
  .floating-punch-text.crit { color: #FF3B5C; font-size: 22px; }
  @keyframes floatPunch {
    0% { transform: translateY(0) scale(1); opacity: 1; }
    100% { transform: translateY(-60px) scale(1.2); opacity: 0; }
  }

  /* ---------------- НИЖНЯЯ ПАНЕЛЬ НАВИГАЦИИ ---------------- */
  #bottom-nav {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    height: 78px;
    background: rgba(14, 16, 26, 0.92);
    backdrop-filter: blur(20px);
    border-top: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-around;
    padding: 0 4px calc(env(safe-area-inset-bottom, 0px) + 6px);
    z-index: 30;
  }
  .tab-btn {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    background: none;
    border: none;
    color: var(--text-dim);
    font-size: 9.5px;
    font-weight: 700;
    padding: 8px 1px;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  .tab-btn .tab-ic { font-size: 19px; line-height: 1; transition: transform 0.2s cubic-bezier(0.3, 2, 0.4, 1); position: relative; }
  .tab-btn.active { color: #FFFFFF; }
  .tab-btn.active .tab-ic { transform: translateY(-2px) scale(1.15); filter: drop-shadow(0 0 8px var(--primary-glow)); }
  .tab-btn.active span:last-child { color: var(--primary); }
  .tab-dot {
    position: absolute;
    top: -3px;
    right: -7px;
    min-width: 14px;
    height: 14px;
    padding: 0 3px;
    border-radius: 7px;
    background: var(--primary);
    color: #FFF;
    font-size: 9px;
    font-weight: 900;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 0 8px var(--primary-glow);
  }

  /* ---------------- МОДАЛЬНЫЕ ОКНА ---------------- */
  .modal-overlay {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.78);
    backdrop-filter: blur(8px);
    display: flex;
    align-items: flex-end;
    z-index: 100;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.25s ease;
  }
  .modal-overlay.active { opacity: 1; pointer-events: all; }
  .modal-sheet {
    background: var(--bg-surface);
    width: 100%;
    border-top-left-radius: var(--radius-xl);
    border-top-right-radius: var(--radius-xl);
    border-top: 1px solid var(--border-highlight);
    box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.7);
    padding: 22px 20px calc(24px + env(safe-area-inset-bottom, 0px));
    max-height: 90%;
    overflow-y: auto;
    transform: translateY(100%);
    transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .modal-sheet::-webkit-scrollbar { width: 0; display: none; }
  .modal-overlay.active .modal-sheet { transform: translateY(0); }
  .modal-bar-handle { width: 40px; height: 4px; background: rgba(255, 255, 255, 0.2); border-radius: 2px; margin: 0 auto 16px; }

  .levelup-badge-anim { font-size: 64px; animation: levelUpBounce 0.9s cubic-bezier(0.3, 1.6, 0.4, 1) infinite alternate; }
  @keyframes levelUpBounce {
    from { transform: scale(0.9) translateY(0); }
    to { transform: scale(1.15) translateY(-8px); filter: drop-shadow(0 0 20px #FFB800); }
  }

  #toast {
    position: absolute;
    top: 24px;
    left: 20px;
    right: 20px;
    background: rgba(23, 27, 43, 0.95);
    border: 1px solid var(--border-highlight);
    box-shadow: 0 10px 30px rgba(0,0,0,0.6);
    backdrop-filter: blur(12px);
    color: #FFFFFF;
    padding: 13px 18px;
    border-radius: var(--radius-md);
    font-size: 13px;
    font-weight: 700;
    text-align: center;
    z-index: 200;
    opacity: 0;
    pointer-events: none;
    transform: translateY(-20px);
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  }
  #toast.show { opacity: 1; transform: translateY(0); }

  /* ---------------- СУМКА (ИНВЕНТАРЬ) ---------------- */
  .inv-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
  .inv-item {
    background: rgba(255, 255, 255, 0.04);
    border: 1.5px solid var(--border);
    border-radius: var(--radius-md);
    padding: 12px;
    display: flex;
    flex-direction: column;
    gap: 6px;
    position: relative;
  }
  .inv-item.rarity-rare { border-color: var(--rarity-rare); }
  .inv-item.rarity-epic { border-color: var(--rarity-epic); background: rgba(157, 78, 221, 0.08); }
  .inv-item.rarity-legendary { border-color: var(--rarity-legendary); background: rgba(255, 183, 3, 0.08); box-shadow: 0 0 14px rgba(255,183,3,0.18); }
  .inv-item.rarity-mythic { border-color: var(--rarity-mythic); background: rgba(255, 0, 127, 0.08); }
  .inv-item .inv-ic { font-size: 30px; }
  .inv-item .inv-name { font-size: 12.5px; font-weight: 800; }
  .inv-item .inv-desc { font-size: 10.5px; color: var(--text-muted); line-height: 1.3; flex: 1; }
  .inv-count {
    position: absolute;
    top: 8px;
    right: 8px;
    background: var(--primary);
    color: #FFF;
    font-size: 11px;
    font-weight: 900;
    padding: 2px 8px;
    border-radius: var(--radius-pill);
  }
  .inv-use-btn { padding: 8px 10px; font-size: 11.5px; border-radius: var(--radius-sm); }
  .inv-tabs { display: flex; gap: 8px; margin-bottom: 14px; }
  .inv-empty { text-align: center; padding: 26px 10px; color: var(--text-muted); font-size: 12px; }
  .trophy-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 12px;
    margin-bottom: 8px;
    border-radius: var(--radius-md);
    background: rgba(255, 184, 0, 0.06);
    border: 1px solid rgba(255, 184, 0, 0.25);
  }
  .trophy-row.equipped { background: rgba(255, 184, 0, 0.16); border-color: var(--gold); }
  .trophy-row .tr-ic { font-size: 26px; }
  .trophy-row .tr-info { flex: 1; min-width: 0; }
  .trophy-row .tr-name { font-size: 12.5px; font-weight: 800; }
  .trophy-row .tr-desc { font-size: 10.5px; color: var(--text-muted); }

  /* ---------------- ДОРОГА УРОВНЕЙ ---------------- */
  .road-summary { padding: 16px; margin-bottom: 18px; display: flex; align-items: center; gap: 14px; }
  .road-summary .rs-ic { font-size: 34px; }
  .road-summary .rs-main { flex: 1; min-width: 0; }
  .road-summary .rs-title { font-size: 14px; font-weight: 900; }
  .road-summary .rs-sub { font-size: 11px; color: var(--text-muted); margin-top: 2px; }
  .road-wrap { position: relative; padding: 4px 0 10px; }
  .road-line {
    position: absolute;
    left: 25px;
    top: 0;
    bottom: 0;
    width: 6px;
    border-radius: 3px;
    background: repeating-linear-gradient(180deg, rgba(255,255,255,0.18) 0 10px, transparent 10px 18px);
  }
  .road-row { position: relative; display: flex; align-items: center; gap: 14px; margin-bottom: 14px; }
  .road-node {
    width: 56px;
    height: 56px;
    border-radius: 50%;
    flex-shrink: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 22px;
    background: #1B2033;
    border: 3px solid var(--border-highlight);
    z-index: 2;
    line-height: 1;
  }
  .road-node small { font-size: 9px; font-weight: 900; margin-top: 2px; color: var(--text-muted); }
  .road-node.reached { border-color: var(--emerald); background: rgba(0, 230, 118, 0.14); }
  .road-node.claimable { border-color: var(--gold); background: rgba(255, 184, 0, 0.2); box-shadow: 0 0 20px var(--gold-glow); animation: roadPulse 1.4s infinite ease-in-out; }
  .road-node.claimed { border-color: var(--text-dim); background: rgba(255,255,255,0.05); opacity: 0.75; }
  @keyframes roadPulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.09); }
  }
  .road-card { flex: 1; min-width: 0; padding: 12px 14px; }
  .road-card.claimable { border-color: var(--gold); background: linear-gradient(135deg, rgba(255,184,0,0.14) 0%, rgba(20,23,36,0.95) 100%); }
  .road-card .rc-head { display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-bottom: 6px; }
  .road-card .rc-lvl { font-size: 12.5px; font-weight: 900; }
  .road-card .rc-state { font-size: 10px; font-weight: 800; color: var(--text-muted); }
  .road-card .rc-chips { display: flex; gap: 5px; flex-wrap: wrap; margin-bottom: 8px; }
  .road-dot-row { position: relative; display: flex; align-items: center; gap: 14px; margin-bottom: 10px; height: 18px; }
  .road-dot { width: 56px; display: flex; justify-content: center; z-index: 2; }
  .road-dot span { width: 14px; height: 14px; border-radius: 50%; background: #2A3047; border: 2px solid rgba(255,255,255,0.18); }
  .road-dot.reached span { background: var(--emerald); border-color: var(--emerald); box-shadow: 0 0 8px var(--emerald-glow); }
  .road-dot.current span { background: var(--primary); border-color: #FFF; width: 20px; height: 20px; box-shadow: 0 0 14px var(--primary-glow); }
  .road-dot-lbl { font-size: 10.5px; font-weight: 700; color: var(--text-dim); }
  .road-dot-lbl.current { color: var(--primary); font-weight: 900; }
  .road-end { text-align: center; padding: 18px; font-size: 12px; color: var(--text-muted); }

  /* ---------------- АДМИН-ПАНЕЛЬ ---------------- */
  .admin-badge-tag {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    padding: 3px 8px;
    border-radius: var(--radius-pill);
    background: rgba(255, 59, 92, 0.2);
    color: var(--primary);
    font-size: 10px;
    font-weight: 800;
  }
  .admin-badge-tag.dirty { background: rgba(255, 184, 0, 0.2); color: var(--gold); }
  .admin-badge-tag.clean { background: rgba(0, 230, 118, 0.18); color: var(--emerald); }
  .admin-section-box {
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid var(--border);
    border-radius: var(--radius-md);
    padding: 14px;
    margin-bottom: 14px;
  }
  .admin-sec-title { font-size: 13.5px; font-weight: 800; margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }
  .season-selector-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; margin-top: 8px; }
  .season-opt-btn {
    padding: 10px 12px;
    border-radius: var(--radius-md);
    background: rgba(255, 255, 255, 0.05);
    border: 1.5px solid var(--border);
    color: var(--text-muted);
    font-size: 12px;
    font-weight: 800;
    display: flex;
    align-items: center;
    gap: 8px;
    justify-content: flex-start;
  }
  .season-opt-btn.active {
    background: rgba(123, 92, 250, 0.2);
    border-color: var(--violet);
    color: #FFFFFF;
    box-shadow: 0 4px 14px var(--violet-glow);
  }
  .input-field {
    width: 100%;
    background: rgba(0, 0, 0, 0.35);
    border: 1.5px solid var(--border);
    border-radius: var(--radius-md);
    padding: 13px 14px;
    color: #FFF;
    font-size: 14px;
    font-family: inherit;
    margin-bottom: 12px;
    transition: border-color 0.2s;
    user-select: text;
    -webkit-user-select: text;
  }
  .input-field:focus { border-color: var(--primary); outline: none; }
  .input-field.sm { padding: 9px 10px; font-size: 13px; margin-bottom: 0; border-radius: var(--radius-sm); }
  .adm-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .adm-field label { display: block; font-size: 10.5px; font-weight: 700; color: var(--text-muted); margin-bottom: 4px; }
  .adm-list-row {
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }
  .adm-list-row:last-child { border-bottom: none; }
  .adm-list-head { display: flex; align-items: center; justify-content: space-between; gap: 8px; margin-bottom: 8px; font-size: 12.5px; font-weight: 800; }
  .adm-list-head .adm-name { display: flex; align-items: center; gap: 8px; min-width: 0; }
  .adm-list-head .adm-name span:last-child { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .adm-grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; }
  .switch { position: relative; width: 42px; height: 24px; flex-shrink: 0; }
  .switch input { opacity: 0; width: 0; height: 0; }
  .switch .slider {
    position: absolute;
    inset: 0;
    background: rgba(255,255,255,0.15);
    border-radius: 24px;
    transition: 0.2s;
    cursor: pointer;
  }
  .switch .slider::before {
    content: '';
    position: absolute;
    width: 18px;
    height: 18px;
    left: 3px;
    top: 3px;
    background: #FFF;
    border-radius: 50%;
    transition: 0.2s;
  }
  .switch input:checked + .slider { background: var(--emerald); }
  .switch input:checked + .slider::before { transform: translateX(18px); }
  .admin-savebar {
    position: sticky;
    bottom: 0;
    background: linear-gradient(180deg, rgba(20,23,36,0) 0%, var(--bg-surface) 28%);
    padding-top: 18px;
    margin-top: 6px;
    display: flex;
    gap: 10px;
    z-index: 5;
  }
  .admin-tabs { display: flex; gap: 6px; overflow-x: auto; margin-bottom: 14px; padding-bottom: 4px; }
  .admin-tabs::-webkit-scrollbar { display: none; }

  /* Живой таймер и калории */
  .live-timer-ring {
    font-size: 46px;
    font-weight: 900;
    font-family: monospace;
    letter-spacing: 2px;
    color: var(--primary);
    text-shadow: 0 0 20px var(--primary-glow);
    margin: 12px 0 6px;
  }
  .live-calorie-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(255, 87, 34, 0.15);
    border: 1.5px solid rgba(255, 87, 34, 0.4);
    color: #FF7043;
    padding: 6px 14px;
    border-radius: var(--radius-pill);
    font-size: 13px;
    font-weight: 800;
    margin-bottom: 16px;
    animation: firePulse 2s infinite ease-in-out;
  }
  @keyframes firePulse {
    0%, 100% { box-shadow: 0 0 8px rgba(255, 87, 34, 0.2); }
    50% { box-shadow: 0 0 18px rgba(255, 87, 34, 0.5); }
  }
  .session-progress { height: 6px; background: rgba(255,255,255,0.08); border-radius: 3px; overflow: hidden; margin: 0 0 14px; }
  .session-progress-fill { height: 100%; background: var(--primary-gradient); border-radius: 3px; transition: width 0.9s linear; }
</style>
</head>
<body>

<div id="app-shell">
  <!-- ВНУТРЕННИЙ КАНВАС ДЛЯ АНИМАЦИИ (Осень, Зима, Весна, Лето) -->
  <canvas id="ambient-canvas"></canvas>

  <div class="swipe-indicator-bar"></div>
  <div id="view-container">
    <div id="view"></div>
  </div>
  <nav id="bottom-nav"></nav>
  <div id="toast"></div>
  <div id="modal-root"></div>
</div>

<script>
/* =========================================================================
   1. ЗВУКОВОЙ СИНТЕЗАТОР (WEB AUDIO API)
   ========================================================================= */
const AudioCtx = window.AudioContext || window.webkitAudioContext;
let audioCtx = null;

function getAudioCtx() {
  if (!audioCtx) audioCtx = new AudioCtx();
  if (audioCtx.state === 'suspended') audioCtx.resume();
  return audioCtx;
}

const Sound = {
  click() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(650, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(140, ctx.currentTime + 0.04);
      gain.gain.setValueAtTime(0.12, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.04);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.04);
    } catch(e) {}
  },
  punch() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'triangle';
      osc.frequency.setValueAtTime(220, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(40, ctx.currentTime + 0.12);
      gain.gain.setValueAtTime(0.45, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.12);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.12);
    } catch(e) {}
  },
  tick() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(800, ctx.currentTime);
      gain.gain.setValueAtTime(0.05, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.025);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.025);
    } catch(e) {}
  },
  beep(freq = 880, duration = 0.1) {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(freq, ctx.currentTime);
      gain.gain.setValueAtTime(0.2, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + duration);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + duration);
    } catch(e) {}
  },
  rouletteTick() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'square';
      osc.frequency.setValueAtTime(520, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(180, ctx.currentTime + 0.03);
      gain.gain.setValueAtTime(0.09, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.03);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.03);
    } catch(e) {}
  },
  fanfare() {
    try {
      const ctx = getAudioCtx();
      const notes = [523.25, 659.25, 783.99, 1046.50, 1318.51];
      notes.forEach((freq, idx) => {
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.type = 'triangle';
        osc.frequency.value = freq;
        const startTime = ctx.currentTime + idx * 0.09;
        const dur = idx === notes.length - 1 ? 0.55 : 0.08;
        gain.gain.setValueAtTime(0.25, startTime);
        gain.gain.exponentialRampToValueAtTime(0.001, startTime + dur);
        osc.connect(gain); gain.connect(ctx.destination);
        osc.start(startTime); osc.stop(startTime + dur);
      });
    } catch(e) {}
  },
  levelUpFanfare() {
    try {
      const ctx = getAudioCtx();
      const chords = [
        [523.25, 659.25],
        [659.25, 783.99],
        [783.99, 1046.50],
        [1046.50, 1318.51, 1567.98]
      ];
      chords.forEach((chord, idx) => {
        chord.forEach(freq => {
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = 'triangle';
          osc.frequency.value = freq;
          const startTime = ctx.currentTime + idx * 0.12;
          const dur = idx === chords.length - 1 ? 0.75 : 0.11;
          gain.gain.setValueAtTime(0.18, startTime);
          gain.gain.exponentialRampToValueAtTime(0.001, startTime + dur);
          osc.connect(gain); gain.connect(ctx.destination);
          osc.start(startTime); osc.stop(startTime + dur);
        });
      });
    } catch(e) {}
  },
  /* Новый звук: применение предмета из сумки */
  useItem() {
    try {
      const ctx = getAudioCtx();
      [440, 660, 990].forEach((freq, idx) => {
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.type = 'sine';
        osc.frequency.value = freq;
        const st = ctx.currentTime + idx * 0.06;
        gain.gain.setValueAtTime(0.16, st);
        gain.gain.exponentialRampToValueAtTime(0.001, st + 0.14);
        osc.connect(gain); gain.connect(ctx.destination);
        osc.start(st); osc.stop(st + 0.14);
      });
    } catch(e) {}
  },
  /* Новый звук: крит в кликере */
  crit() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'sawtooth';
      osc.frequency.setValueAtTime(180, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(900, ctx.currentTime + 0.14);
      gain.gain.setValueAtTime(0.22, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.16);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.16);
    } catch(e) {}
  },
  /* Новый звук: ошибка / нет энергии */
  denied() {
    try {
      const ctx = getAudioCtx();
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'square';
      osc.frequency.setValueAtTime(220, ctx.currentTime);
      osc.frequency.setValueAtTime(150, ctx.currentTime + 0.08);
      gain.gain.setValueAtTime(0.12, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.18);
      osc.connect(gain); gain.connect(ctx.destination);
      osc.start(); osc.stop(ctx.currentTime + 0.18);
    } catch(e) {}
  }
};

/* =========================================================================
   2. СЕЗОННЫЙ АНИМАЦИОННЫЙ ДВИЖОК ВНУТРИ ПРИЛОЖЕНИЯ
   ========================================================================= */
const AmbientEngine = {
  canvas: null,
  ctx: null,
  particles: [],
  mode: 'autumn',
  animId: null,
  width: 0,
  height: 0,

  init() {
    this.canvas = document.getElementById('ambient-canvas');
    this.ctx = this.canvas.getContext('2d');
    this.resize();
    window.addEventListener('resize', () => this.resize());
    this.createParticles();
    this.start();
  },

  resize() {
    const shell = document.getElementById('app-shell');
    this.width = this.canvas.width = shell.clientWidth || 400;
    this.height = this.canvas.height = shell.clientHeight || 750;
  },

  setMode(newMode) {
    this.mode = newMode;
    this.particles = [];
    if (newMode !== 'off') {
      this.createParticles();
    }
  },

  createParticles() {
    const count = this.mode === 'winter' ? 50 : (this.mode === 'summer' ? 38 : 32);
    this.particles = [];
    for (let i = 0; i < count; i++) {
      this.particles.push(this.spawnParticle(true));
    }
  },

  spawnParticle(randomY = false) {
    const p = {
      x: Math.random() * this.width,
      y: randomY ? Math.random() * this.height : -25,
      size: 4 + Math.random() * 8,
      speedY: 0.8 + Math.random() * 1.5,
      speedX: -0.4 + Math.random() * 0.8,
      rotation: Math.random() * Math.PI * 2,
      rotationSpeed: -0.03 + Math.random() * 0.06,
      wobble: Math.random() * Math.PI * 2,
      wobbleSpeed: 0.02 + Math.random() * 0.03,
      alpha: 0.4 + Math.random() * 0.5,
      color: '#FFF'
    };

    if (this.mode === 'autumn') {
      const autumnColors = ['#D84315', '#E65100', '#F57C00', '#FFB300', '#BF360C', '#C2185B'];
      p.color = autumnColors[Math.floor(Math.random() * autumnColors.length)];
      p.size = 7 + Math.random() * 11;
      p.speedY = 0.9 + Math.random() * 1.4;
    } else if (this.mode === 'winter') {
      p.color = '#FFFFFF';
      p.size = 2.5 + Math.random() * 4;
      p.speedY = 0.7 + Math.random() * 1.6;
    } else if (this.mode === 'spring') {
      const springColors = ['#FF85A1', '#FFAFCC', '#FFC8DD', '#FFB7B2', '#F72585'];
      p.color = springColors[Math.floor(Math.random() * springColors.length)];
      p.size = 5 + Math.random() * 8;
      p.speedY = 0.8 + Math.random() * 1.3;
    } else if (this.mode === 'summer') {
      const summerColors = ['#FFE600', '#76FF03', '#00E5FF', '#FFB300', '#FFD54F'];
      p.color = summerColors[Math.floor(Math.random() * summerColors.length)];
      p.size = 3 + Math.random() * 5.5;
      p.y = randomY ? Math.random() * this.height : this.height + 20;
      p.speedY = -(0.5 + Math.random() * 1.2);
    }

    return p;
  },

  start() {
    const loop = () => {
      this.update();
      this.draw();
      this.animId = requestAnimationFrame(loop);
    };
    loop();
  },

  update() {
    if (this.mode === 'off') return;

    for (let i = 0; i < this.particles.length; i++) {
      const p = this.particles[i];
      p.wobble += p.wobbleSpeed;
      p.rotation += p.rotationSpeed;

      if (this.mode === 'summer') {
        p.y += p.speedY;
        p.x += Math.sin(p.wobble) * 0.7;
        if (p.y < -25) this.particles[i] = this.spawnParticle(false);
      } else {
        p.y += p.speedY;
        p.x += Math.sin(p.wobble) * 1.1 + p.speedX;
        if (p.y > this.height + 25) this.particles[i] = this.spawnParticle(false);
      }

      if (p.x > this.width + 25) p.x = -15;
      if (p.x < -25) p.x = this.width + 15;
    }
  },

  draw() {
    this.ctx.clearRect(0, 0, this.width, this.height);
    if (this.mode === 'off') return;

    for (const p of this.particles) {
      this.ctx.save();
      this.ctx.translate(p.x, p.y);
      this.ctx.rotate(p.rotation);
      this.ctx.globalAlpha = p.alpha;
      this.ctx.fillStyle = p.color;

      if (this.mode === 'autumn') {
        this.ctx.beginPath();
        this.ctx.ellipse(0, 0, p.size, p.size * 0.55, 0, 0, Math.PI * 2);
        this.ctx.fill();
      } else if (this.mode === 'winter') {
        this.ctx.beginPath();
        this.ctx.arc(0, 0, p.size, 0, Math.PI * 2);
        this.ctx.shadowColor = '#FFFFFF';
        this.ctx.shadowBlur = 6;
        this.ctx.fill();
      } else if (this.mode === 'spring') {
        this.ctx.beginPath();
        this.ctx.moveTo(0, -p.size);
        this.ctx.quadraticCurveTo(p.size, 0, 0, p.size);
        this.ctx.quadraticCurveTo(-p.size, 0, 0, -p.size);
        this.ctx.fill();
      } else if (this.mode === 'summer') {
        this.ctx.beginPath();
        this.ctx.arc(0, 0, p.size, 0, Math.PI * 2);
        this.ctx.shadowColor = p.color;
        this.ctx.shadowBlur = 12;
        this.ctx.fill();
      }
      this.ctx.restore();
    }
  }
};

/* =========================================================================
   3. КАТАЛОГИ ДАННЫХ
   ========================================================================= */
const STORAGE_KEY = 'fitquest_pro_state_v7';
const OLD_STORAGE_KEY = 'fitquest_pro_state_v6';
const SECRET_ADMIN_PASSWORD = 'Halid2007.';

const DEFAULT_SETTINGS = {
  ambientTheme: 'autumn',
  adminPassword: SECRET_ADMIN_PASSWORD,
  maxWorkoutEnergy: 100,     // максимум энергии для тренировок
  workoutRegenSec: 20,       // 1 единица энергии тренировок за N секунд
  energyCostFactor: 0.5,     // стоимость энергии = ккал × коэффициент
  clickerMax: 200,           // лимит кликера
  clickerRegenSec: 3,        // 1 клик за N секунд
  tapXp: 2,
  tapCoins: 2,
  critMultiplier: 3
};

const EXERCISE_CATEGORIES = [
  { id: 'all', label: 'Все' },
  { id: 'base', label: '💪 Базовые' },
  { id: 'box', label: '🥊 Бокс' },
  { id: 'mma', label: '🥋 ММА' },
  { id: 'grappling', label: '🤼 Грэпплинг' },
  { id: 'crossfit', label: '🔥 Кроссфит' }
];

const WORKOUT_CATEGORIES = [
  { id: 'all', label: 'Все' },
  { id: 'mma', label: '🥋 ММА' },
  { id: 'box', label: '🥊 Бокс' },
  { id: 'grappling', label: '🤼 Грэпплинг' },
  { id: 'crossfit', label: '🔥 Кроссфит' },
  { id: 'general', label: '💪 Общие' }
];

const CATEGORY_NAMES = { mma: 'ММА', box: 'БОКС', grappling: 'ГРЭПЛИНГ', crossfit: 'КРОССФИТ', general: 'ОБЩАЯ', base: 'БАЗА' };

// 51 упражнение с реальными коэффициентами расхода калорий (kcalPerMin)
const availableExercises = [
  // Разминка
  { id: 'ex-1', name: 'Суставная гимнастика', muscle: 'Все тело · Разминка', icon: '🧘', xpPerMin: 2, coinsPerMin: 1, kcalPerMin: 3.5, defaultSec: 180 },
  { id: 'ex-2', name: 'Вращения руками и корпусом', muscle: 'Плечи и позвоночник', icon: '🤸', xpPerMin: 2, coinsPerMin: 1, kcalPerMin: 4.0, defaultSec: 120 },
  { id: 'ex-3', name: 'Динамическая растяжка ног', muscle: 'Бедра и связки', icon: '🦵', xpPerMin: 3, coinsPerMin: 1, kcalPerMin: 4.2, defaultSec: 120 },

  // Грудь и руки
  { id: 'ex-4', name: 'Классические отжимания', muscle: 'Грудь, трицепс, плечи', icon: '💪', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 7.2, defaultSec: 90 },
  { id: 'ex-5', name: 'Широкие отжимания', muscle: 'Грудные мышцы акцент', icon: '🏋️', xpPerMin: 6, coinsPerMin: 3, kcalPerMin: 7.6, defaultSec: 90 },
  { id: 'ex-6', name: 'Алмазные отжимания', muscle: 'Трицепс и центр груди', icon: '💎', xpPerMin: 7, coinsPerMin: 4, kcalPerMin: 8.2, defaultSec: 60 },
  { id: 'ex-7', name: 'Отжимания на брусьях', muscle: 'Грудь и трицепс', icon: '⚡', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 9.0, defaultSec: 90 },
  { id: 'ex-8', name: 'Обратные отжимания от скамьи', muscle: 'Трицепс', icon: '🪑', xpPerMin: 4, coinsPerMin: 2, kcalPerMin: 6.0, defaultSec: 90 },

  // Спина и плечи
  { id: 'ex-9', name: 'Подтягивания широким хватом', muscle: 'Широчайшие мышцы', icon: '🧗', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 9.5, defaultSec: 90 },
  { id: 'ex-10', name: 'Австралийские подтягивания', muscle: 'Середина спины', icon: '🪵', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 6.8, defaultSec: 90 },
  { id: 'ex-11', name: 'Отжимания «Домиком»', muscle: 'Дельтовидные плечи', icon: '⛺', xpPerMin: 6, coinsPerMin: 3, kcalPerMin: 7.4, defaultSec: 60 },
  { id: 'ex-12', name: 'Лодочка на животе', muscle: 'Поясница и осанка', icon: '🛶', xpPerMin: 4, coinsPerMin: 2, kcalPerMin: 5.5, defaultSec: 60 },

  // Пресс и кор
  { id: 'ex-13', name: 'Скручивания на пресс', muscle: 'Прямая мышца живота', icon: '🎯', xpPerMin: 4, coinsPerMin: 2, kcalPerMin: 5.8, defaultSec: 60 },
  { id: 'ex-14', name: 'Планка на локтях', muscle: 'Глубокий кор', icon: '🧱', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 5.0, defaultSec: 60 },
  { id: 'ex-15', name: 'Боковая планка', muscle: 'Косые мышцы', icon: '📐', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 5.2, defaultSec: 60 },
  { id: 'ex-16', name: 'Велосипед на пресс', muscle: 'Косые и нижний пресс', icon: '🚴', xpPerMin: 6, coinsPerMin: 3, kcalPerMin: 7.5, defaultSec: 60 },
  { id: 'ex-17', name: 'Подъёмы ног лежа', muscle: 'Нижний пресс', icon: '🪜', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 6.2, defaultSec: 60 },
  { id: 'ex-18', name: 'Русский твист', muscle: 'Ротация кора', icon: '🌪️', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 6.8, defaultSec: 60 },

  // Ноги и ягодицы
  { id: 'ex-19', name: 'Взрывные приседания', muscle: 'Квадрицепс и ягодицы', icon: '💥', xpPerMin: 6, coinsPerMin: 4, kcalPerMin: 8.5, defaultSec: 90 },
  { id: 'ex-20', name: 'Выпады назад', muscle: 'Баланс и ягодицы', icon: '🚶', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 7.2, defaultSec: 90 },
  { id: 'ex-21', name: 'Болгарские сплит-приседы', muscle: 'Изоляция бедра', icon: '🔥', xpPerMin: 7, coinsPerMin: 4, kcalPerMin: 8.8, defaultSec: 90 },
  { id: 'ex-22', name: 'Ягодичный мостик', muscle: 'Задняя поверхность', icon: '🌉', xpPerMin: 4, coinsPerMin: 2, kcalPerMin: 5.4, defaultSec: 60 },

  // Кардио
  { id: 'ex-23', name: 'Бёрпи с прыжком', muscle: 'Все тело · Взрыв', icon: '💣', xpPerMin: 10, coinsPerMin: 6, kcalPerMin: 13.0, defaultSec: 60 },
  { id: 'ex-24', name: 'Джампинг Джек', muscle: 'Кардио выносливость', icon: '⭐', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 9.0, defaultSec: 90 },
  { id: 'ex-25', name: 'Скоростная скакалка', muscle: 'Икры и координация', icon: '🪢', xpPerMin: 7, coinsPerMin: 4, kcalPerMin: 11.5, defaultSec: 120 },
  { id: 'ex-26', name: 'Бой с тенью (комбинации)', muscle: 'Ударная скорость ММА', icon: '🥊', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.5, defaultSec: 120 },
  { id: 'ex-27', name: 'Заминка и растяжка шпагатов', muscle: 'Восстановление', icon: '🍃', xpPerMin: 3, coinsPerMin: 1, kcalPerMin: 3.2, defaultSec: 180 },

  /* ---------------- БОКС ---------------- */
  { id: 'ex-28', cat: 'box', name: 'Джеб–кросс на тени', muscle: 'Прямые удары и скорость', icon: '🥊', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.0, defaultSec: 120 },
  { id: 'ex-29', cat: 'box', name: 'Хуки и апперкоты по мешку', muscle: 'Боковые и нижние удары', icon: '👊', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 11.5, defaultSec: 120 },
  { id: 'ex-30', cat: 'box', name: 'Нырки и уклоны', muscle: 'Защита и реакция', icon: '🌀', xpPerMin: 7, coinsPerMin: 4, kcalPerMin: 9.0, defaultSec: 90 },
  { id: 'ex-31', cat: 'box', name: 'Боксёрский шаг и работа ног', muscle: 'Манёвр по рингу', icon: '👟', xpPerMin: 6, coinsPerMin: 3, kcalPerMin: 8.5, defaultSec: 90 },
  { id: 'ex-32', cat: 'box', name: 'Полный раунд на мешке (3 мин)', muscle: 'Выносливость бойца', icon: '🪨', xpPerMin: 10, coinsPerMin: 6, kcalPerMin: 12.5, defaultSec: 180 },

  /* ---------------- ММА ---------------- */
  { id: 'ex-33', cat: 'mma', name: 'Лоу-кики и мид-кики', muscle: 'Ударная техника ног', icon: '🦶', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.0, defaultSec: 90 },
  { id: 'ex-34', cat: 'mma', name: 'Колени в клинче', muscle: 'Клинч и ближний бой', icon: '🤺', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.5, defaultSec: 90 },
  { id: 'ex-35', cat: 'mma', name: 'Сбивание ног (тейкдаун-спринт)', muscle: 'Взрывной вход в ноги', icon: '🤼', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 12.0, defaultSec: 60 },
  { id: 'ex-36', cat: 'mma', name: 'Ground and pound по мешку', muscle: 'Удары сверху', icon: '🔨', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 11.8, defaultSec: 90 },
  { id: 'ex-37', cat: 'mma', name: 'Спрол и возврат в стойку', muscle: 'Защита от тейкдауна', icon: '🐊', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.8, defaultSec: 60 },
  { id: 'ex-38', cat: 'mma', name: 'Комбинации рука–нога', muscle: 'Связки ММА', icon: '🥋', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 11.2, defaultSec: 120 },

  /* ---------------- ГРЭПЛИНГ ---------------- */
  { id: 'ex-39', cat: 'grappling', name: 'Ползки: креветка и мост', muscle: 'Подвижность на ковре', icon: '🦐', xpPerMin: 6, coinsPerMin: 3, kcalPerMin: 7.5, defaultSec: 90 },
  { id: 'ex-40', cat: 'grappling', name: 'Мостики борца', muscle: 'Шея и спина', icon: '🏗️', xpPerMin: 5, coinsPerMin: 3, kcalPerMin: 6.5, defaultSec: 60 },
  { id: 'ex-41', cat: 'grappling', name: 'Свипы и пассы (дриллы)', muscle: 'Работа в гарде', icon: '♟️', xpPerMin: 6, coinsPerMin: 4, kcalPerMin: 7.8, defaultSec: 120 },
  { id: 'ex-42', cat: 'grappling', name: 'Захваты и подтягивание полотенца', muscle: 'Хват и предплечья', icon: '🧶', xpPerMin: 7, coinsPerMin: 4, kcalPerMin: 8.5, defaultSec: 90 },
  { id: 'ex-43', cat: 'grappling', name: 'Бросок через бедро (дрилл)', muscle: 'Техника бросков', icon: '🏯', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 10.0, defaultSec: 90 },
  { id: 'ex-44', cat: 'grappling', name: 'Контроль сбоку: обход по кругу', muscle: 'Давление и баланс', icon: '🛡️', xpPerMin: 6, coinsPerMin: 4, kcalPerMin: 7.2, defaultSec: 90 },

  /* ---------------- КРОССФИТ ---------------- */
  { id: 'ex-45', cat: 'crossfit', name: 'Трастеры с гантелями или рюкзаком', muscle: 'Ноги + жим', icon: '🏋️', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 12.5, defaultSec: 90 },
  { id: 'ex-46', cat: 'crossfit', name: 'Махи гирей или рюкзаком', muscle: 'Задняя цепь и кор', icon: '🔔', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 12.0, defaultSec: 90 },
  { id: 'ex-47', cat: 'crossfit', name: 'Прыжки на тумбу (box jump)', muscle: 'Взрывная сила ног', icon: '📦', xpPerMin: 8, coinsPerMin: 5, kcalPerMin: 11.0, defaultSec: 60 },
  { id: 'ex-48', cat: 'crossfit', name: 'Двойные прыжки на скакалке', muscle: 'Координация и кардио', icon: '🪂', xpPerMin: 9, coinsPerMin: 6, kcalPerMin: 13.0, defaultSec: 60 },
  { id: 'ex-49', cat: 'crossfit', name: 'Бёрпи + подтягивание', muscle: 'Все тело · Предел', icon: '🦾', xpPerMin: 11, coinsPerMin: 7, kcalPerMin: 14.0, defaultSec: 60 },
  { id: 'ex-50', cat: 'crossfit', name: 'Ходьба «медведем»', muscle: 'Плечи, кор, ноги', icon: '🐻', xpPerMin: 6, coinsPerMin: 4, kcalPerMin: 8.5, defaultSec: 60 },
  { id: 'ex-51', cat: 'crossfit', name: 'Табата: приседы с выпрыгиванием', muscle: '20 сек работа / 10 отдых', icon: '⏱️', xpPerMin: 10, coinsPerMin: 6, kcalPerMin: 13.5, defaultSec: 60 },
];

// 20 ПЕРСОНАЖЕЙ: 10 ЗА УРОВНИ И 10 ИЗ КЕЙСОВ. Поле bonus — реальные бонусы, которые работают у выбранного бойца
const initialCharacters = [
  { id: 'c-1', name: 'Дэн «Кобра» Северов', sport: 'Бокс & ММА', unlockType: 'level', unlockLevel: 1, description: 'Стартовый боец с молниеносной реакцией', perk: '+10% XP за все тренировки', bonus: { xp: 0.10 }, icon: '🥊', rarity: 'common', active: true },
  { id: 'c-2', name: 'Тимур Волков', sport: 'Самбо & Дзюдо', unlockType: 'level', unlockLevel: 1, description: 'Мастер бросков и железного захвата', perk: '+10% монет за тренировку', bonus: { coins: 0.10 }, icon: '🥋', rarity: 'common', active: true },
  { id: 'c-3', name: 'Марко «Торпедо»', sport: 'Плавание & Кросс', unlockType: 'level', unlockLevel: 3, description: 'Неутомимый атлет с бесконечным дыханием', perk: '+15% наград за удары в кликере', bonus: { tap: 0.15 }, icon: '🏊', rarity: 'common', active: true },
  { id: 'c-4', name: 'Влад «Скала»', sport: 'Воркаут', unlockType: 'level', unlockLevel: 5, description: 'Король турников и чистых подтягиваний', perk: '+12% XP за тренировки', bonus: { xp: 0.12 }, icon: '🧗', rarity: 'common', active: true },
  { id: 'c-5', name: 'Рикардо Феррейра', sport: 'Кикбоксинг', unlockType: 'level', unlockLevel: 7, description: 'Бразильская школа сокрушительных хай-киков', perk: '+15% монет за тренировки', bonus: { coins: 0.15 }, icon: '🦵', rarity: 'rare', active: true },
  { id: 'c-6', name: 'Никита Громов', sport: 'ММА · лёгкий вес', unlockType: 'level', unlockLevel: 10, description: 'Молниеносные контратаки и тайминг', perk: '+15% XP за тренировки', bonus: { xp: 0.15 }, icon: '⚡', rarity: 'rare', active: true },
  { id: 'c-7', name: 'Диего Марсело', sport: 'Футбольный атлетизм', unlockType: 'level', unlockLevel: 13, description: 'Взрывная скорость и координация ног', perk: '−20% затрат энергии на тренировки', bonus: { cost: 0.20 }, icon: '⚽', rarity: 'rare', active: true },
  { id: 'c-8', name: 'Леон «Снайпер»', sport: 'Стретчинг & Стрельба', unlockType: 'level', unlockLevel: 16, description: 'Филигранная точность каждого движения', perk: '+10% шанс крита в кликере (×3)', bonus: { crit: 0.10 }, icon: '🎯', rarity: 'rare', active: true },
  { id: 'c-9', name: 'Камила «Пантера»', sport: 'Муай-тай', unlockType: 'level', unlockLevel: 20, description: 'Грациозная и беспощадная чемпионка ринга', perk: '+20% XP за тренировки', bonus: { xp: 0.20 }, icon: '🐆', rarity: 'epic', active: true },
  { id: 'c-10', name: 'Руслан «Стальной Молот»', sport: 'Вольная борьба Pro', unlockType: 'level', unlockLevel: 25, description: 'Непобедимый чемпион ковра, сминающий защиту', perk: '+40% ко всем XP и монетам!', bonus: { xp: 0.40, coins: 0.40, tap: 0.40 }, icon: '🤼', rarity: 'legendary', active: true },

  { id: 'c-11', name: 'Арман «Тень»', sport: 'Грепплинг & BJJ', unlockType: 'case', unlockLevel: null, description: 'Секретный мастер удушающих приёмов', perk: '+25% монет из кейсов', bonus: { caseCoins: 0.25 }, icon: '🥷', rarity: 'rare', dropWeight: 25, inCase: true, active: true },
  { id: 'c-12', name: 'Баир «Феникс»', sport: 'Тайский бокс', unlockType: 'case', unlockLevel: null, description: 'Восстанавливается после самых жёстких боёв', perk: '−30% времени авто-тренировки', bonus: { autoTime: 0.30 }, icon: '🔥', rarity: 'rare', dropWeight: 25, inCase: true, active: true },
  { id: 'c-13', name: 'Заур «Титан»', sport: 'Пауэрлифтинг', unlockType: 'case', unlockLevel: null, description: 'Поднимает невероятные веса с ледяным спокойствием', perk: '+25% XP за тренировки', bonus: { xp: 0.25 }, icon: '🏋️', rarity: 'epic', dropWeight: 14, inCase: true, active: true },
  { id: 'c-14', name: 'Амир «Громовержец»', sport: 'Кроссфит Pro', unlockType: 'case', unlockLevel: null, description: 'Превращает тяжелейшие комплексы в искусство', perk: '−25% затрат энергии на тренировки', bonus: { cost: 0.25 }, icon: '⚡', rarity: 'epic', dropWeight: 14, inCase: true, active: true },
  { id: 'c-15', name: 'Эрик «Викинг»', sport: 'Стронгмен', unlockType: 'case', unlockLevel: null, description: 'Северная ярость и стальная несгибаемость', perk: '+35% монет за тренировки', bonus: { coins: 0.35 }, icon: '🪓', rarity: 'epic', dropWeight: 12, inCase: true, active: true },
  { id: 'c-16', name: 'Самурай Кэндзи', sport: 'Кэндо & Ниндзюцу', unlockType: 'case', unlockLevel: null, description: 'Ментальная концентрация абсолютного мастера', perk: '+40% к наградам за квесты', bonus: { quest: 0.40 }, icon: '⚔️', rarity: 'legendary', dropWeight: 4, inCase: true, active: true },
  { id: 'c-17', name: 'Кибер-Валькирия Астра', sport: 'Нео-Кроссфит 2077', unlockType: 'case', unlockLevel: null, description: 'Бионические рефлексы и футуристический темп', perk: 'Удваивает награды тапа кликера!', bonus: { tap: 1.0 }, icon: '🦾', rarity: 'legendary', dropWeight: 3.5, inCase: true, active: true },
  { id: 'c-18', name: 'Мастер Ли «Дракон»', sport: 'Шаолинь Кунг-фу', unlockType: 'case', unlockLevel: null, description: 'Сверхчеловеческий контроль дыхания и тела', perk: 'Авто-тренировка приносит ×2 приз', bonus: { auto: 1.0 }, icon: '🐉', rarity: 'legendary', dropWeight: 3, inCase: true, active: true },
  { id: 'c-19', name: 'Чингиз «Хан»', sport: 'Абсолютный Чемпион', unlockType: 'case', unlockLevel: null, description: 'Легенда арены, его имя внушает благоговейный трепет', perk: '👑 МИФИЧЕСКИЙ БУСТ: +100% ВСЕГО!', bonus: { xp: 1.0, coins: 1.0, tap: 1.0, quest: 1.0, caseCoins: 1.0 }, icon: '👑', rarity: 'mythic', dropWeight: 1.2, inCase: true, active: true },
  { id: 'c-20', name: 'Макс «Джаггернаут»', sport: 'Титаническая Сила', unlockType: 'case', unlockLevel: null, description: 'Неудержимая разрушительная сила вселенского масштаба', perk: '💎 Авто-сбор квестов + ×2 монеты', bonus: { coins: 1.0, autoClaim: 1 }, icon: '🪐', rarity: 'mythic', dropWeight: 0.8, inCase: true, active: true },
];

/* ---------------- ТРЕНИРОВКИ ---------------- */
function S(name, muscle, icon, duration, mult = 1) {
  return { name, muscle, icon, duration, mult };
}

function defaultSteps(icon) {
  return [
    S('Раунд 1 · Разогрев', 'Разогрев и вход в ритм', icon, 180, 1),
    S('Пиковый раунд', 'Максимальная интенсивность', icon, 240, 1.2),
    S('Финишный спурт', 'Выжимка сил до конца', '🏁', 180, 1.1)
  ];
}

function mkW(id, cat, name, description, icon, xpPerMin, coinsPerMin, kcalPerMin, steps) {
  return { id, cat, name, description, icon, xpPerMin, coinsPerMin, kcalPerMin, steps: steps || defaultSteps(icon), active: true };
}

const initialWorkouts = [
  /* --------- ОБЩИЕ --------- */
  mkW('w-1', 'general', 'Утренняя взрывная зарядка', 'Быстрый разгон метаболизма и заряд бодрости на весь день', '🌅', 5, 3, 6.5),
  mkW('w-2', 'general', 'Силовой воркаут «Грудь и Трицепс»', 'Отжимания разных стилей, упоры и планка', '💪', 7, 4, 7.8),
  mkW('w-3', 'general', 'ММА Кардио-штурм', 'Интенсивные раунды, удары и скоростные бёрпи', '🥊', 9, 6, 11.2),
  mkW('w-4', 'general', 'Стальной пресс & Кор', 'Комплекс для прокачки рельефных кубиков и осанки', '🧱', 6, 4, 6.8),
  mkW('w-5', 'general', 'Глубокая растяжка & Мобильность', 'Снятие зажимов, развитие гибкости суставов и связок', '🧘', 3, 2, 3.8),
  mkW('w-6', 'general', 'Взрывные ноги «Спринтер»', 'Плиометрика, выпрыгивания и мощные выпады', '🦵', 8, 5, 9.2),

  /* --------- БОКС --------- */
  mkW('w-7', 'box', 'Бокс: джеб–кросс–хук', 'Базовые связки на тени и мешке: скорость, дистанция, ритм', '🥊', 8, 5, 10.5, [
    S('Разминка на тени', 'Лёгкий бой с тенью', '🥊', 120, 0.7),
    S('Раунд: джеб–кросс', 'Прямые удары', '👊', 180, 1),
    S('Раунд: хуки', 'Боковые удары', '🌀', 180, 1.1),
    S('Раунд на мешке', 'Полная связка в темпе', '🪨', 180, 1.25),
    S('Заминка', 'Дыхание и растяжка плеч', '🍃', 120, 0.4)
  ]),
  mkW('w-8', 'box', 'Бокс: 6 раундов на мешке', 'Шесть трёхминутных раундов — настоящий боксёрский темп', '🪨', 10, 6, 12.0, [
    S('Раунд 1 · Разогрев', 'Спокойный темп', '🥊', 180, 0.8),
    S('Раунд 2 · Прямые', 'Джеб и кросс', '👊', 180, 1),
    S('Раунд 3 · Боковые', 'Хуки по корпусу', '🌀', 180, 1.1),
    S('Раунд 4 · Серии', 'Серии по 5 ударов', '⚡', 180, 1.2),
    S('Раунд 5 · Пик', 'Максимум интенсивности', '🔥', 180, 1.35),
    S('Раунд 6 · Финиш', 'Всё, что осталось', '🏁', 180, 1.25)
  ]),
  mkW('w-9', 'box', 'Бокс: защита и уклоны', 'Нырки, уклоны и контратаки — учимся не пропускать', '🌀', 7, 4, 9.0, [
    S('Работа ног', 'Боксёрский шаг', '👟', 150, 0.9),
    S('Нырки и уклоны', 'Защита корпуса', '🌀', 180, 1),
    S('Контратаки', 'Уклон и ответ', '🥊', 180, 1.15),
    S('Раунд с тенью', 'Свободный бой', '👊', 150, 1.1)
  ]),
  mkW('w-10', 'box', 'Бокс + скакалка (кардио)', 'Скакалка, тень и сильные серии на выносливость', '🪢', 8, 5, 11.0, [
    S('Скакалка · лёгкая', 'Разогрев икр', '🪢', 180, 0.9),
    S('Бой с тенью', 'Работа руками', '🥊', 180, 1),
    S('Скакалка · скоростная', 'Двойные пропуски', '⚡', 180, 1.25),
    S('Серии по мешку', 'Взрыв и отдых', '🪨', 180, 1.2)
  ]),

  /* --------- ММА --------- */
  mkW('w-11', 'mma', 'ММА: ударная техника', 'Руки, ноги, колени — связки для стойки', '🥋', 9, 5, 10.8, [
    S('Разминка', 'Суставы и динамика', '🧘', 120, 0.6),
    S('Руки: джеб–кросс–хук', 'Бокс в ММА', '🥊', 180, 1),
    S('Лоу-кики и мид-кики', 'Удары ногами', '🦶', 180, 1.1),
    S('Колени в клинче', 'Ближний бой', '🤺', 150, 1.15),
    S('Комбинации рука–нога', 'Связки ММА', '🥋', 180, 1.25)
  ]),
  mkW('w-12', 'mma', 'ММА: спарринг-раунды 5×3', 'Пять раундов по три минуты с переходами стойка–партер', '🥊', 10, 6, 12.5, [
    S('Раунд 1', 'Разведка и дистанция', '🥊', 180, 0.9),
    S('Раунд 2', 'Темп растёт', '👊', 180, 1.05),
    S('Раунд 3', 'Тейкдауны и контроль', '🤼', 180, 1.2),
    S('Раунд 4', 'Партер и удары сверху', '🔨', 180, 1.3),
    S('Раунд 5', 'Решающий', '🔥', 180, 1.4)
  ]),
  mkW('w-13', 'mma', 'ММА: клинч и тейкдауны', 'Заходы в ноги, клинч у сетки и защита от тейкдаунов', '🤼', 9, 6, 11.5, [
    S('Спринт-входы', 'Взрывной заход в ноги', '🤼', 150, 1.1),
    S('Клинч и колени', 'Давление у сетки', '🤺', 180, 1.1),
    S('Спрол и возврат', 'Защита от тейкдауна', '🐊', 150, 1.15),
    S('Ground and pound', 'Удары сверху по мешку', '🔨', 150, 1.25)
  ]),
  mkW('w-14', 'mma', 'ММА Fight Camp: финальный блок', 'Самая тяжёлая тренировка подготовки бойца — для сильных', '🔥', 11, 7, 13.0, [
    S('Разминка бойца', 'Динамика и тень', '🥊', 150, 0.7),
    S('Ударный блок', 'Руки и ноги', '👊', 180, 1.1),
    S('Борцовский блок', 'Тейкдауны и клинч', '🤼', 180, 1.25),
    S('Партер', 'Контроль и удары', '🔨', 180, 1.3),
    S('Раунд-финал', 'Всё вместе', '🔥', 180, 1.45),
    S('Заминка', 'Восстановление', '🍃', 120, 0.4)
  ]),

  /* --------- ГРЭПЛИНГ --------- */
  mkW('w-15', 'grappling', 'Грэпплинг: борцовский стартер', 'Стойки, захваты и броски — основа борьбы', '🤼', 8, 5, 9.5, [
    S('Разминка борца', 'Мостики, кувырки', '🏗️', 150, 0.7),
    S('Захваты и хват', 'Предплечья и кор', '🧶', 150, 1),
    S('Бросок через бедро', 'Дрилл на технику', '🏯', 180, 1.15),
    S('Контроль сбоку', 'Давление и баланс', '🛡️', 150, 1)
  ]),
  mkW('w-16', 'grappling', 'BJJ: гард и свипы (дриллы)', 'Работа в гарде, свипы и переходы на пассе', '🥋', 7, 4, 8.5, [
    S('Ползки: креветка', 'Подвижность', '🦐', 150, 0.9),
    S('Свипы (дрилл)', 'Работа в гарде', '♟️', 210, 1),
    S('Пассы (дрилл)', 'Прохождение гарда', '🛡️', 210, 1.05),
    S('Мостики и возврат', 'Побег из под контроля', '🏗️', 150, 0.9)
  ]),
  mkW('w-17', 'grappling', 'Грэпплинг: удержания и побеги', 'Дриллы на технику: контроль позиции и выход из неё', '🔒', 8, 5, 9.0, [
    S('Контроль сбоку', 'Давление корпусом', '🛡️', 180, 1),
    S('Побег: мост и креветка', 'Выход из-под контроля', '🦐', 180, 1.05),
    S('Переходы между позициями', 'Плавность и баланс', '♟️', 180, 1.1),
    S('Раунд на скорость', 'Лёгкий темп с партнёром', '🤼', 150, 1.2)
  ]),
  mkW('w-18', 'grappling', 'Грэпплинг: силовая для борца', 'Шея, хват, спина и кор — фундамент борца', '💪', 8, 5, 8.8, [
    S('Мостики борца', 'Шея и спина', '🏗️', 150, 0.8),
    S('Подтягивания с полотенцем', 'Хват и предплечья', '🧶', 150, 1.1),
    S('Ходьба «медведем»', 'Плечи и кор', '🐻', 150, 1),
    S('Выпрыгивания и бёрпи', 'Взрывная сила', '💣', 150, 1.3)
  ]),

  /* --------- КРОССФИТ --------- */
  mkW('w-19', 'crossfit', 'WOD «Огонь»: AMRAP 12', 'Максимум раундов за 12 минут: бёрпи, приседы, отжимания', '🔥', 10, 6, 12.5, [
    S('Разминка WOD', 'Суставы и пульс', '🧘', 120, 0.6),
    S('AMRAP · блок 1', 'Бёрпи / приседы / отжимания', '💣', 240, 1.15),
    S('AMRAP · блок 2', 'Продолжай без пауз', '🔥', 240, 1.25),
    S('AMRAP · блок 3', 'Финальный рывок', '🏁', 240, 1.35)
  ]),
  mkW('w-20', 'crossfit', 'EMOM 10: сила и взрыв', 'Каждую минуту — новое задание. Отдых — только остаток минуты', '⏱️', 9, 6, 11.5, [
    S('EMOM · минуты 1–3', 'Трастеры', '🏋️', 180, 1.1),
    S('EMOM · минуты 4–6', 'Махи и прыжки', '🔔', 180, 1.15),
    S('EMOM · минуты 7–10', 'Бёрпи + подтягивания', '🦾', 240, 1.3)
  ]),
  mkW('w-21', 'crossfit', 'Табата-ад 4×4', 'Четыре табаты подряд: 20 секунд работы, 10 отдыха', '⚡', 10, 7, 13.5, [
    S('Табата 1 · приседы', 'Приседы с выпрыгиванием', '⏱️', 240, 1.1),
    S('Табата 2 · бёрпи', 'Бёрпи с прыжком', '💣', 240, 1.25),
    S('Табата 3 · скакалка', 'Двойные прыжки', '🪂', 240, 1.2),
    S('Табата 4 · всё тело', 'Бёрпи + подтягивание', '🦾', 240, 1.35)
  ]),
  mkW('w-22', 'crossfit', 'WOD «Спартанец»', 'Тяжёлый комплекс на силу, кор и выносливость', '🏛️', 11, 7, 12.8, [
    S('Трастеры', 'Ноги + жим', '🏋️', 180, 1.1),
    S('Подтягивания и бёрпи', 'Спина и кардио', '🦾', 180, 1.3),
    S('Махи гирей', 'Задняя цепь', '🔔', 180, 1.2),
    S('Прыжки на тумбу', 'Взрыв ног', '📦', 150, 1.15),
    S('Ходьба «медведем»', 'Кор и плечи', '🐻', 150, 1)
  ]),
  mkW('w-23', 'crossfit', 'Кроссфит: ноги и кор', 'Приседы, прыжки и планки — ноги будут гореть', '🦵', 9, 5, 10.5, [
    S('Болгарские приседы', 'Изоляция бедра', '🔥', 180, 1.05),
    S('Прыжки на тумбу', 'Взрывная сила', '📦', 150, 1.15),
    S('Выпады с прыжком', 'Баланс и мощность', '🚶', 150, 1.1),
    S('Планки: фронт и бок', 'Глубокий кор', '🧱', 150, 0.8)
  ]),
];

/* ---------------- КЕЙСЫ ---------------- */
const initialCases = [
  { id: 'case-daily', name: 'Ежедневный бонусный кейс', icon: '🎁', isDaily: true, price: 0, minCoins: 40, maxCoins: 150, minXp: 25, maxXp: 70, rareDropChance: 0.18, itemRolls: 1, itemChance: 0.65, desc: 'Бесплатный кейс каждые 24 часа! Монеты, предметы и шанс на бойца.', active: true },
  { id: 'case-bronze', name: 'Стартовый бронзовый кейс', icon: '📦', isDaily: false, price: 75, minCoins: 25, maxCoins: 95, minXp: 15, maxXp: 40, rareDropChance: 0.15, itemRolls: 1, itemChance: 0.60, desc: 'Доступный бокс с монетами, энергетиками и редкими бойцами', active: true },
  { id: 'case-silver', name: 'Серебряный кейс Воина', icon: '🥈', isDaily: false, price: 220, minCoins: 75, maxCoins: 260, minXp: 45, maxXp: 110, rareDropChance: 0.30, itemRolls: 2, itemChance: 0.60, desc: 'Повышенный шанс на редких бойцов и ценные предметы', active: true },
  { id: 'case-gold', name: 'Золотой сундук Чемпиона', icon: '🥇', isDaily: false, price: 480, minCoins: 180, maxCoins: 600, minXp: 90, maxXp: 220, rareDropChance: 0.45, itemRolls: 2, itemChance: 0.75, desc: 'Высокий шанс на Эпических и Легендарных бойцов ринга', active: true },
  { id: 'case-cyber', name: 'Алмазный Кибер-Кейс', icon: '💎', isDaily: false, price: 850, minCoins: 350, maxCoins: 1200, minXp: 180, maxXp: 400, rareDropChance: 0.60, itemRolls: 3, itemChance: 0.75, desc: 'Кибернетические награды, бустеры и Легенды', active: true },
  { id: 'case-mythic', name: 'Мифический кейс Титанов', icon: '👑', isDaily: false, price: 1500, minCoins: 800, maxCoins: 2500, minXp: 400, maxXp: 900, rareDropChance: 0.85, itemRolls: 3, itemChance: 0.90, desc: 'Редчайший шанс выбить Мифических Чингиз Хана и Джаггернаута!', active: true },
];

/* ---------------- ПРЕДМЕТЫ ИЗ КЕЙСОВ (МОЖНО ИСПОЛЬЗОВАТЬ) ---------------- */
const ITEM_CATALOG = {
  energy_drink: { id: 'energy_drink', name: 'Энергетик', icon: '🥤', rarity: 'common', weight: 30, group: 'workout', desc: '+40 энергии для тренировок' },
  protein: { id: 'protein', name: 'Протеиновый шейкер', icon: '🥛', rarity: 'common', weight: 20, group: 'workout', desc: '+25 энергии и ×1.5 XP на следующую тренировку' },
  mega_energy: { id: 'mega_energy', name: 'Мега-энергетик', icon: '🧃', rarity: 'rare', weight: 11, group: 'workout', desc: 'Полностью восстанавливает энергию тренировок' },
  click_charge: { id: 'click_charge', name: 'Батарейка кликера', icon: '🔋', rarity: 'common', weight: 28, group: 'click', desc: '+50 ударов в кликере' },
  click_full: { id: 'click_full', name: 'Заряд Тайфуна', icon: '🌀', rarity: 'rare', weight: 10, group: 'click', desc: 'Полностью заряжает кликер' },
  coin_pouch: { id: 'coin_pouch', name: 'Мешочек монет', icon: '👝', rarity: 'common', weight: 24, group: 'other', desc: '+300 монет мгновенно' },
  xp_scroll: { id: 'xp_scroll', name: 'Свиток опыта', icon: '📜', rarity: 'rare', weight: 12, group: 'other', desc: '+150 XP мгновенно' },
  xp_boost: { id: 'xp_boost', name: 'Бустер XP ×2', icon: '🧪', rarity: 'epic', weight: 6, group: 'boost', desc: 'Удваивает XP за следующую тренировку' },
  coin_boost: { id: 'coin_boost', name: 'Золотой магнит', icon: '🧲', rarity: 'epic', weight: 6, group: 'boost', desc: 'Удваивает монеты за следующую тренировку' },
  voucher: { id: 'voucher', name: 'Золотой купон', icon: '🎟️', rarity: 'epic', weight: 5, group: 'other', desc: 'Бесплатно открывает любой платный кейс' },
  legend_elixir: { id: 'legend_elixir', name: 'Эликсир Чемпиона', icon: '🏆', rarity: 'legendary', weight: 1.6, group: 'boost', desc: 'Полная энергия + полный кликер + XP ×2 на тренировку' },
};

const RARITY_RANK = { common: 1, rare: 2, epic: 3, legendary: 4, mythic: 5 };

/* ---------------- ТРОФЕИ (МОЖНО НАДЕТЬ) ---------------- */
const TROPHY_CATALOG = {
  'tr-badge': { id: 'tr-badge', name: 'Значок Первопроходца', icon: '🏅', rarity: 'common', desc: '+3% монет за тренировки', coinMult: 0.03 },
  'tr-gloves': { id: 'tr-gloves', name: 'Золотые перчатки', icon: '🥊', rarity: 'rare', desc: '+5% XP за тренировки', xpMult: 0.05 },
  'tr-cup': { id: 'tr-cup', name: 'Кубок арены', icon: '🏆', rarity: 'epic', desc: '+8% монет за тренировки', coinMult: 0.08 },
  'tr-shaker': { id: 'tr-shaker', name: 'Платиновый шейкер', icon: '🥤', rarity: 'rare', desc: '−10% затрат энергии на тренировки', costMult: 0.10 },
  'tr-belt': { id: 'tr-belt', name: 'Пояс Чемпиона', icon: '👑', rarity: 'legendary', desc: '+10% XP и +10% монет', xpMult: 0.10, coinMult: 0.10 },
};
const TROPHY_DROP_ORDER = ['tr-gloves', 'tr-cup', 'tr-shaker', 'tr-belt'];

/* ---------------- ДОРОГА УРОВНЕЙ: награда каждые 3 уровня ---------------- */
const ROAD_STEP = 3;
const ROAD_MAX_LEVEL = 60;

function buildRoadRewards() {
  const list = [];
  for (let lvl = ROAD_STEP; lvl <= ROAD_MAX_LEVEL; lvl += ROAD_STEP) {
    const idx = lvl / ROAD_STEP;
    let give = {};
    let icon = '🎁';
    let title = 'Утешительный подарок';
    const kind = idx % 6;

    if (kind === 1) {
      give = { coins: 150 + lvl * 12 };
      icon = '🪙'; title = 'Мешок монет';
    } else if (kind === 2) {
      give = { energy: 35 + Math.min(40, lvl), items: { energy_drink: 1 } };
      icon = '🥤'; title = 'Заряд сил';
    } else if (kind === 3) {
      give = { clicks: 60 + lvl, items: { click_charge: 1 } };
      icon = '🔋'; title = 'Заряд для тапа';
    } else if (kind === 4) {
      give = { xp: 80 + lvl * 6, coins: 100 };
      icon = '📜'; title = 'Мудрость тренера';
    } else if (kind === 5) {
      give = { items: { protein: 1, coin_pouch: 1 }, coins: 120 };
      icon = '🥛'; title = 'Набор атлета';
    } else {
      give = { items: { voucher: 1, xp_boost: 1 }, coins: 250 + lvl * 8 };
      icon = '🎟️'; title = 'Большой приз';
    }

    if (lvl % 30 === 0) {
      give = { items: { voucher: 3, legend_elixir: 1, mega_energy: 2 }, coins: 2000 + lvl * 20 };
      icon = '🏆'; title = 'ЛЕГЕНДАРНАЯ НАГРАДА';
    }
    if (lvl === 15 || lvl === 45) {
      give = { items: { mega_energy: 1, click_full: 1, xp_boost: 1, coin_boost: 1 }, coins: 500 };
      icon = '💎'; title = 'Сундук вехи';
    }
    list.push({ level: lvl, icon, title, give });
  }
  return list;
}
const ROAD_REWARDS = buildRoadRewards();

/* ---------------- КВЕСТЫ ---------------- */
const initialQuests = [
  { id: 'q-1', title: 'Завершить любую тренировку', target: 1, current: 0, rewardCoins: 60, rewardXp: 40, icon: '🏋️', claimed: false, period: 'day' },
  { id: 'q-2', title: 'Сделать 40 ударов в кликере', target: 40, current: 0, rewardCoins: 50, rewardXp: 30, icon: '👊', claimed: false, period: 'day' },
  { id: 'q-3', title: 'Собрать свой комплекс в конструкторе', target: 1, current: 0, rewardCoins: 80, rewardXp: 50, icon: '🛠️', claimed: false, period: 'day' },
  { id: 'q-4', title: 'Открыть кейс с рулеткой', target: 1, current: 0, rewardCoins: 100, rewardXp: 60, icon: '🎁', claimed: false, period: 'day' },
  { id: 'q-6', title: 'Использовать предмет из сумки', target: 1, current: 0, rewardCoins: 70, rewardXp: 45, icon: '🎒', claimed: false, period: 'day' },
  { id: 'q-7', title: 'Забрать награду на Дороге уровней', target: 1, current: 0, rewardCoins: 90, rewardXp: 55, icon: '🛣️', claimed: false, period: 'week' },
  { id: 'q-5', title: 'Завершить 4 тренировки за неделю', target: 4, current: 0, rewardCoins: 300, rewardXp: 200, icon: '📅', claimed: false, period: 'week' },
];

const RANKS = [
  { min: 0, name: 'Новичок зала', color: '#7E8B9B' },
  { min: 3, name: 'Любитель спорта', color: '#00B4D8' },
  { min: 7, name: 'Атлет прогресса', color: '#00E676' },
  { min: 12, name: 'Профи тренировок', color: '#9D4EDD' },
  { min: 18, name: 'Мастер дисциплины', color: '#FF3B5C' },
  { min: 25, name: 'Гранд-Мастер', color: '#FFB800' },
  { min: 35, name: 'Живая Легенда', color: '#FF007F' },
];

/* =========================================================================
   4. УТИЛИТЫ, ХРАНИЛИЩЕ И СОСТОЯНИЕ
   ========================================================================= */
function calcLevel(xp) {
  let level = 1, need = 100, remaining = xp;
  while (remaining >= need) {
    remaining -= need;
    level++;
    need = Math.round(need * 1.25);
  }
  return { level, xpIntoLevel: remaining, xpForNextLevel: need };
}

function getRank(level) {
  let r = RANKS[0];
  for (const item of RANKS) {
    if (level >= item.min) r = item;
  }
  return r;
}

function genId(p) { return p + '-' + Date.now() + '-' + Math.floor(Math.random() * 10000); }
function esc(s) { return String(s == null ? '' : s).replace(/[&<>"']/g, c => ({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[c])); }
function clone(o) { return JSON.parse(JSON.stringify(o)); }
function pad2(n) { return String(n).padStart(2, '0'); }

function dayKeyNow() {
  const d = new Date();
  return d.getFullYear() + '-' + pad2(d.getMonth() + 1) + '-' + pad2(d.getDate());
}
function weekKeyNow() {
  const d = new Date();
  const day = (d.getDay() + 6) % 7; // понедельник = 0
  d.setDate(d.getDate() - day);
  return d.getFullYear() + '-' + pad2(d.getMonth() + 1) + '-' + pad2(d.getDate());
}

function loadRaw(key) {
  try {
    const raw = localStorage.getItem(key);
    if (raw) return JSON.parse(raw);
  } catch(e) {}
  return null;
}

// Загрузка с миграцией прогресса из старой версии (v6)
let saved = loadRaw(STORAGE_KEY);
let migratedFromV6 = false;
if (!saved) {
  const old = loadRaw(OLD_STORAGE_KEY);
  if (old) {
    migratedFromV6 = true;
    saved = {
      player: old.player,
      unlockedCharacters: old.unlockedCharacters,
      activeCharacterId: old.activeCharacterId,
      charTrainings: old.charTrainings,
      history: old.history,
      lastDailyCaseTimestamp: old.lastDailyCaseTimestamp,
      oldVouchers: old.freeCaseVouchers || 0,
      oldTrophies: old.trophies || [],
      settings: old.settings ? { ambientTheme: old.settings.ambientTheme, adminPassword: old.settings.adminPassword } : null
    };
  }
}

function migrateTrophies(oldList) {
  const out = ['tr-badge'];
  (oldList || []).forEach(name => {
    Object.values(TROPHY_CATALOG).forEach(t => {
      if (String(name).indexOf(t.name) !== -1 && !out.includes(t.id)) out.push(t.id);
    });
  });
  return out;
}

const state = {
  settings: Object.assign({}, DEFAULT_SETTINGS, (saved && saved.settings) || {}),

  player: (saved && saved.player) || { xp: 0, coins: 350 },
  characters: (saved && saved.characters) || clone(initialCharacters),
  unlockedCharacters: (saved && saved.unlockedCharacters) || ['c-1', 'c-2'],
  activeCharacterId: (saved && saved.activeCharacterId) || 'c-1',
  charTrainings: (saved && saved.charTrainings) || {},

  // Сумка: предметы (id → количество), трофеи (список id) и надетый трофей
  inventory: (saved && saved.inventory) || (migratedFromV6 && saved.oldVouchers ? { voucher: saved.oldVouchers } : {}),
  trophies: (saved && saved.trophies) || (migratedFromV6 ? migrateTrophies(saved.oldTrophies) : ['tr-badge']),
  equippedTrophy: (saved && saved.equippedTrophy) || null,
  boosts: (saved && saved.boosts) || { xpMult: 1, xpLeft: 0, coinMult: 1, coinLeft: 0 },
  roadClaimed: (saved && saved.roadClaimed) || [],
  lastDailyCaseTimestamp: (saved && saved.lastDailyCaseTimestamp) || 0,

  workouts: (saved && saved.workouts) || clone(initialWorkouts),
  exercises: (saved && saved.exercises) || clone(availableExercises),
  cases: (saved && saved.cases) || clone(initialCases),
  quests: (saved && saved.quests) || clone(initialQuests),
  questDayKey: (saved && saved.questDayKey) || dayKeyNow(),
  questWeekKey: (saved && saved.questWeekKey) || weekKeyNow(),
  history: (saved && saved.history) || [],

  // Конструктор: { exerciseId: durationInSeconds }
  builderSelection: (saved && saved.builderSelection) || { 'ex-1': 120, 'ex-28': 120, 'ex-33': 90, 'ex-14': 60, 'ex-27': 120 },
  builderCat: 'all',

  // Энергия тренировок и кликера (с отметкой времени для офлайн-восстановления)
  workoutEnergy: (saved && saved.workoutEnergy != null) ? saved.workoutEnergy : DEFAULT_SETTINGS.maxWorkoutEnergy,
  workoutTs: (saved && saved.workoutTs) || Date.now(),
  clickerEnergy: (saved && saved.clickerEnergy != null && !migratedFromV6) ? saved.clickerEnergy : DEFAULT_SETTINGS.clickerMax,
  clickerTs: (saved && saved.clickerTs && !migratedFromV6) ? saved.clickerTs : Date.now(),

  // Навигация
  currentTab: 'home',
  charSubTab: 'all',
  workoutCat: 'all',
  isAdmin: false,

  activeWorkoutSession: null
};

function persist() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify({
      settings: state.settings,
      player: state.player,
      characters: state.characters,
      unlockedCharacters: state.unlockedCharacters,
      activeCharacterId: state.activeCharacterId,
      charTrainings: state.charTrainings,
      inventory: state.inventory,
      trophies: state.trophies,
      equippedTrophy: state.equippedTrophy,
      boosts: state.boosts,
      roadClaimed: state.roadClaimed,
      lastDailyCaseTimestamp: state.lastDailyCaseTimestamp,
      workouts: state.workouts,
      exercises: state.exercises,
      cases: state.cases,
      quests: state.quests,
      questDayKey: state.questDayKey,
      questWeekKey: state.questWeekKey,
      history: state.history,
      builderSelection: state.builderSelection,
      workoutEnergy: state.workoutEnergy,
      workoutTs: state.workoutTs,
      clickerEnergy: state.clickerEnergy,
      clickerTs: state.clickerTs
    }));
  } catch(e) {}
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(showToast._t);
  showToast._t = setTimeout(() => t.classList.remove('show'), 2800);
}

function closeModal() {
  document.getElementById('modal-root').innerHTML = '';
}

/* =========================================================================
   5. БОНУСЫ ПЕРСОНАЖЕЙ И ТРОФЕЕВ
   ========================================================================= */
function getActiveChar() {
  return state.characters.find(c => c.id === state.activeCharacterId && state.unlockedCharacters.includes(c.id)) || state.characters[0];
}
function cb(key) {
  const c = getActiveChar();
  return (c && c.bonus && c.bonus[key]) || 0;
}
function trophyBonus(key) {
  const t = TROPHY_CATALOG[state.equippedTrophy];
  return (t && state.trophies.includes(t.id) && t[key]) || 0;
}
function bonusSummary() {
  const c = getActiveChar();
  const t = TROPHY_CATALOG[state.equippedTrophy];
  const parts = [];
  if (c) parts.push(c.icon + ' ' + c.perk);
  if (t && state.trophies.includes(t.id)) parts.push(t.icon + ' ' + t.desc);
  return parts;
}

/* =========================================================================
   6. ШКАЛЫ ЭНЕРГИИ: ТРЕНИРОВКИ И КЛИКЕР (С ВОССТАНОВЛЕНИЕМ ПО ВРЕМЕНИ)
   ========================================================================= */
function maxWE() { return state.settings.maxWorkoutEnergy; }
function maxCE() { return state.settings.clickerMax; }

function regenTick() {
  const now = Date.now();
  let changed = false;

  // Энергия тренировок
  const wMax = maxWE();
  const wInt = Math.max(1, state.settings.workoutRegenSec) * 1000;
  if (state.workoutEnergy >= wMax) {
    state.workoutEnergy = wMax;
    state.workoutTs = now;
  } else {
    const g = Math.floor((now - state.workoutTs) / wInt);
    if (g > 0) {
      state.workoutEnergy = Math.min(wMax, state.workoutEnergy + g);
      state.workoutTs = state.workoutEnergy >= wMax ? now : state.workoutTs + g * wInt;
      changed = true;
    }
  }

  // Клики: 1 клик каждые clickerRegenSec секунд (по умолчанию 3), лимит 200
  const cMax = maxCE();
  const cInt = Math.max(1, state.settings.clickerRegenSec) * 1000;
  if (state.clickerEnergy >= cMax) {
    state.clickerEnergy = cMax;
    state.clickerTs = now;
  } else {
    const g2 = Math.floor((now - state.clickerTs) / cInt);
    if (g2 > 0) {
      state.clickerEnergy = Math.min(cMax, state.clickerEnergy + g2);
      state.clickerTs = state.clickerEnergy >= cMax ? now : state.clickerTs + g2 * cInt;
      changed = true;
    }
  }

  if (changed) persist();
  return changed;
}

function spendWorkoutEnergy(n) {
  if (state.workoutEnergy >= maxWE()) state.workoutTs = Date.now();
  state.workoutEnergy = Math.max(0, state.workoutEnergy - n);
}
function gainWorkoutEnergy(n) {
  state.workoutEnergy = Math.min(maxWE(), state.workoutEnergy + n);
}
function spendClick() {
  if (state.clickerEnergy >= maxCE()) state.clickerTs = Date.now();
  state.clickerEnergy = Math.max(0, state.clickerEnergy - 1);
}
function gainClicks(n) {
  state.clickerEnergy = Math.min(maxCE(), state.clickerEnergy + n);
}

function secToNextWE() {
  if (state.workoutEnergy >= maxWE()) return 0;
  const wInt = Math.max(1, state.settings.workoutRegenSec) * 1000;
  return Math.max(0, Math.ceil((wInt - (Date.now() - state.workoutTs)) / 1000));
}
function secToNextCE() {
  if (state.clickerEnergy >= maxCE()) return 0;
  const cInt = Math.max(1, state.settings.clickerRegenSec) * 1000;
  return Math.max(0, Math.ceil((cInt - (Date.now() - state.clickerTs)) / 1000));
}
function secToFullWE() {
  const missing = maxWE() - Math.floor(state.workoutEnergy);
  if (missing <= 0) return 0;
  return secToNextWE() + (missing - 1) * state.settings.workoutRegenSec;
}
function fmtLong(sec) {
  if (sec <= 0) return '0с';
  const m = Math.floor(sec / 60);
  const s = sec % 60;
  return m > 0 ? `${m}м ${pad2(s)}с` : `${s}с`;
}

function energyBarHtml(kind) {
  if (kind === 'w') {
    const pct = Math.round((state.workoutEnergy / maxWE()) * 100);
    return `
      <div class="card bar-card">
        <div class="bar-head">
          <span>⚡ Энергия тренировок</span>
          <span data-we-val>${Math.floor(state.workoutEnergy)} / ${maxWE()}</span>
        </div>
        <div class="bar-track"><div class="bar-fill w ${pct < 25 ? 'low' : ''}" data-we-fill style="width:${pct}%;"></div></div>
        <div class="bar-sub">
          <span>+1 каждые ${state.settings.workoutRegenSec} сек</span>
          <span data-we-timer>${state.workoutEnergy >= maxWE() ? 'Полная ✓' : 'До полной: ' + fmtLong(secToFullWE())}</span>
        </div>
      </div>`;
  }
  const pct = Math.round((state.clickerEnergy / maxCE()) * 100);
  return `
    <div class="card bar-card">
      <div class="bar-head">
        <span>👊 Запас ударов кликера</span>
        <span data-ce-val>${Math.floor(state.clickerEnergy)} / ${maxCE()}</span>
      </div>
      <div class="bar-track"><div class="bar-fill c ${pct < 10 ? 'low' : ''}" data-ce-fill style="width:${pct}%;"></div></div>
      <div class="bar-sub">
        <span>+1 удар каждые ${state.settings.clickerRegenSec} сек</span>
        <span data-ce-timer>${state.clickerEnergy >= maxCE() ? 'Полный ✓' : 'Следующий через ' + secToNextCE() + 'с'}</span>
      </div>
    </div>`;
}

function boostPillsHtml() {
  const pills = [];
  if (state.boosts.xpLeft > 0) pills.push(`<span class="boost-pill">🧪 XP ×${state.boosts.xpMult} · ещё ${state.boosts.xpLeft} трен.</span>`);
  if (state.boosts.coinLeft > 0) pills.push(`<span class="boost-pill">🧲 Монеты ×${state.boosts.coinMult} · ещё ${state.boosts.coinLeft} трен.</span>`);
  return pills.length ? `<div class="boost-row">${pills.join('')}</div>` : '';
}

function updateEnergyUI() {
  const wPct = Math.round((state.workoutEnergy / maxWE()) * 100);
  document.querySelectorAll('[data-we-fill]').forEach(el => {
    el.style.width = wPct + '%';
    el.classList.toggle('low', wPct < 25);
  });
  document.querySelectorAll('[data-we-val]').forEach(el => { el.textContent = `${Math.floor(state.workoutEnergy)} / ${maxWE()}`; });
  document.querySelectorAll('[data-we-timer]').forEach(el => {
    el.textContent = state.workoutEnergy >= maxWE() ? 'Полная ✓' : 'До полной: ' + fmtLong(secToFullWE());
  });

  const cPct = Math.round((state.clickerEnergy / maxCE()) * 100);
  document.querySelectorAll('[data-ce-fill]').forEach(el => {
    el.style.width = cPct + '%';
    el.classList.toggle('low', cPct < 10);
  });
  document.querySelectorAll('[data-ce-val]').forEach(el => { el.textContent = `${Math.floor(state.clickerEnergy)} / ${maxCE()}`; });
  document.querySelectorAll('[data-ce-timer]').forEach(el => {
    el.textContent = state.clickerEnergy >= maxCE() ? 'Полный ✓' : 'Следующий через ' + secToNextCE() + 'с';
  });
}

function updateCoinsUI() {
  document.querySelectorAll('.js-coins').forEach(el => { el.textContent = `🪙 ${state.player.coins}`; });
}

// Общий тикер энергии — раз в полсекунды
setInterval(() => {
  regenTick();
  updateEnergyUI();
}, 500);

/* =========================================================================
   7. СУМКА: ПРЕДМЕТЫ ИЗ КЕЙСОВ, КОТОРЫЕ МОЖНО ПРИМЕНЯТЬ
   ========================================================================= */
function itemCount(id) { return state.inventory[id] || 0; }
function totalItems() { return Object.values(state.inventory).reduce((a, b) => a + b, 0); }

function addItem(id, n = 1) {
  state.inventory[id] = (state.inventory[id] || 0) + n;
}

function addBoost(kind, mult, workouts) {
  const b = state.boosts;
  if (kind === 'xp') {
    b.xpMult = b.xpLeft > 0 ? Math.max(b.xpMult, mult) : mult;
    b.xpLeft += workouts;
  } else {
    b.coinMult = b.coinLeft > 0 ? Math.max(b.coinMult, mult) : mult;
    b.coinLeft += workouts;
  }
}

// Применить предмет из сумки. Возвращает true, если предмет израсходован
function useItem(id) {
  if (!(itemCount(id) > 0)) return false;
  regenTick();
  let msg = '';

  switch (id) {
    case 'energy_drink':
      if (state.workoutEnergy >= maxWE()) { showToast('Энергия тренировок уже полная ⚡'); Sound.denied(); return false; }
      gainWorkoutEnergy(40);
      msg = '🥤 +40 энергии тренировок!';
      break;
    case 'protein':
      if (state.workoutEnergy >= maxWE() && state.boosts.xpLeft > 0) { showToast('Шейкер сейчас не нужен'); Sound.denied(); return false; }
      gainWorkoutEnergy(25);
      addBoost('xp', 1.5, 1);
      msg = '🥛 +25 энергии и XP ×1.5 на следующую тренировку!';
      break;
    case 'mega_energy':
      if (state.workoutEnergy >= maxWE()) { showToast('Энергия тренировок уже полная ⚡'); Sound.denied(); return false; }
      state.workoutEnergy = maxWE();
      msg = '🧃 Энергия тренировок восстановлена полностью!';
      break;
    case 'click_charge':
      if (state.clickerEnergy >= maxCE()) { showToast('Кликер уже полностью заряжен 👊'); Sound.denied(); return false; }
      gainClicks(50);
      msg = '🔋 +50 ударов в кликере!';
      break;
    case 'click_full':
      if (state.clickerEnergy >= maxCE()) { showToast('Кликер уже полностью заряжен 👊'); Sound.denied(); return false; }
      state.clickerEnergy = maxCE();
      msg = '🌀 Кликер заряжен до максимума!';
      break;
    case 'coin_pouch':
      state.player.coins += 300;
      msg = '👝 +300 монет!';
      break;
    case 'xp_scroll':
      msg = '📜 +150 XP!';
      break;
    case 'xp_boost':
      addBoost('xp', 2, 1);
      msg = '🧪 Следующая тренировка даст ×2 XP!';
      break;
    case 'coin_boost':
      addBoost('coin', 2, 1);
      msg = '🧲 Следующая тренировка даст ×2 монет!';
      break;
    case 'legend_elixir':
      state.workoutEnergy = maxWE();
      state.clickerEnergy = maxCE();
      addBoost('xp', 2, 1);
      msg = '🏆 Эликсир Чемпиона: энергия, кликер и ×2 XP!';
      break;
    case 'voucher':
      closeModal();
      switchTab('cases');
      showToast('🎟️ Выберите платный кейс — купон применится при открытии');
      return false;
    default:
      return false;
  }

  state.inventory[id]--;
  if (state.inventory[id] <= 0) delete state.inventory[id];
  Sound.useItem();
  updateQuestProgress('item', 1);
  persist();
  showToast(msg);
  if (id === 'xp_scroll') addPlayerXp(150);
  updateEnergyUI();
  updateCoinsUI();
  return true;
}

function equipTrophy(id) {
  if (!state.trophies.includes(id)) return;
  state.equippedTrophy = state.equippedTrophy === id ? null : id;
  Sound.click();
  persist();
}

function describeGive(give) {
  const out = [];
  if (give.coins) out.push(`🪙 +${give.coins}`);
  if (give.xp) out.push(`✨ +${give.xp} XP`);
  if (give.energy) out.push(`⚡ +${give.energy} энергии`);
  if (give.clicks) out.push(`👊 +${give.clicks} ударов`);
  if (give.items) {
    Object.keys(give.items).forEach(id => {
      const it = ITEM_CATALOG[id];
      if (it) out.push(`${it.icon} ${it.name} ×${give.items[id]}`);
    });
  }
  return out;
}

function applyGive(give) {
  if (give.coins) state.player.coins += give.coins;
  if (give.energy) gainWorkoutEnergy(give.energy);
  if (give.clicks) gainClicks(give.clicks);
  if (give.items) Object.keys(give.items).forEach(id => addItem(id, give.items[id]));
  persist();
  if (give.xp) addPlayerXp(give.xp);
}

/* =========================================================================
   8. ДОРОГА УРОВНЕЙ: УТЕШИТЕЛЬНАЯ НАГРАДА КАЖДЫЕ 3 УРОВНЯ
   ========================================================================= */
function roadClaimable() {
  const lvl = calcLevel(state.player.xp).level;
  return ROAD_REWARDS.filter(r => r.level <= lvl && !state.roadClaimed.includes(r.level));
}
function nextRoadReward() {
  const lvl = calcLevel(state.player.xp).level;
  return ROAD_REWARDS.find(r => r.level > lvl) || null;
}

function claimRoadReward(level) {
  const r = ROAD_REWARDS.find(x => x.level === level);
  if (!r || state.roadClaimed.includes(level)) return;
  if (calcLevel(state.player.xp).level < level) { showToast('Этот уровень ещё не достигнут'); return; }
  state.roadClaimed.push(level);
  Sound.fanfare();
  updateQuestProgress('road', 1);
  applyGive(r.give);
  showToast(`🛣️ Награда ур. ${level}: ${describeGive(r.give).join(' · ')}`);
  render();
}

function claimAllRoadRewards() {
  const list = roadClaimable();
  if (!list.length) return;
  Sound.fanfare();
  const merged = { coins: 0, xp: 0, energy: 0, clicks: 0, items: {} };
  list.forEach(r => {
    state.roadClaimed.push(r.level);
    const g = r.give;
    merged.coins += g.coins || 0;
    merged.xp += g.xp || 0;
    merged.energy += g.energy || 0;
    merged.clicks += g.clicks || 0;
    Object.keys(g.items || {}).forEach(id => { merged.items[id] = (merged.items[id] || 0) + g.items[id]; });
  });
  updateQuestProgress('road', list.length);
  applyGive(merged);
  showToast(`🛣️ Забрано наград: ${list.length}`);
  render();
}

/* =========================================================================
   9. КВЕСТЫ (СБРОС КАЖДЫЙ ДЕНЬ / НЕДЕЛЮ)
   ========================================================================= */
function resetQuestsIfNeeded() {
  const dk = dayKeyNow();
  const wk = weekKeyNow();
  let changed = false;
  state.quests.forEach(q => {
    const isWeek = q.period === 'week';
    if ((!isWeek && state.questDayKey !== dk) || (isWeek && state.questWeekKey !== wk)) {
      q.current = 0;
      q.claimed = false;
      changed = true;
    }
  });
  if (state.questDayKey !== dk) { state.questDayKey = dk; changed = true; }
  if (state.questWeekKey !== wk) { state.questWeekKey = wk; changed = true; }
  if (changed) persist();
}

function claimQuest(id, silent) {
  const q = state.quests.find(x => x.id === id);
  if (!q || q.claimed || q.current < q.target) return false;
  q.claimed = true;
  const mult = 1 + cb('quest');
  const coins = Math.round(q.rewardCoins * mult);
  const xp = Math.round(q.rewardXp * mult);
  state.player.coins += coins;
  persist();
  addPlayerXp(xp);
  if (!silent) showToast(`Квест сдан! +${coins}🪙 +${xp}XP`);
  return true;
}

function updateQuestProgress(actionKey, amount = 1) {
  const map = { workout: 'тренировк', click: 'клик', builder: 'конструктор', case: 'кейс', item: 'предмет', road: 'дорог' };
  const needle = map[actionKey];
  if (!needle) return;
  state.quests.forEach(q => {
    if (!q.claimed && q.title.toLowerCase().includes(needle)) {
      q.current = Math.min(q.target, q.current + amount);
    }
  });
  persist();
  // Бонус Джаггернаута: авто-сбор квестов
  if (cb('autoClaim')) {
    state.quests.forEach(q => { if (!q.claimed && q.current >= q.target) claimQuest(q.id, true); });
  }
}

/* =========================================================================
   10. УРОВНИ, РАЗБЛОКИРОВКА БОЙЦОВ, ПОВЫШЕНИЕ УРОВНЯ
   ========================================================================= */
function checkLevelUnlocks(level) {
  const newlyUnlocked = [];
  state.characters.forEach(c => {
    if (c.unlockType === 'level' && c.unlockLevel <= level && !state.unlockedCharacters.includes(c.id)) {
      state.unlockedCharacters.push(c.id);
      newlyUnlocked.push(c);
    }
  });
  if (newlyUnlocked.length > 0) persist();
  return newlyUnlocked;
}

function addPlayerXp(amount) {
  const oldLevel = calcLevel(state.player.xp).level;
  state.player.xp += amount;
  const newLevel = calcLevel(state.player.xp).level;

  if (newLevel > oldLevel) {
    const unlockedChars = checkLevelUnlocks(newLevel);
    triggerLevelUpModal(oldLevel, newLevel, unlockedChars);
  }
  persist();
}

function triggerLevelUpModal(oldLvl, newLvl, unlockedChars) {
  Sound.levelUpFanfare();
  const rank = getRank(newLvl);
  const bonusCoins = newLvl * 60;
  state.player.coins += bonusCoins;
  persist();

  const newRoad = ROAD_REWARDS.filter(r => r.level > oldLvl && r.level <= newLvl && !state.roadClaimed.includes(r.level));

  const root = document.getElementById('modal-root');
  root.innerHTML = `
    <div class="modal-overlay active" id="levelup-modal-ov">
      <div class="modal-sheet" style="text-align:center;">
        <div class="modal-bar-handle"></div>
        <div class="levelup-badge-anim">🏆</div>
        <div style="font-size:12px;font-weight:800;color:var(--gold);letter-spacing:1px;text-transform:uppercase;margin-top:6px;">ПОЗДРАВЛЯЕМ!</div>
        <h2 style="font-size:26px;font-weight:900;margin:4px 0 6px;">НОВЫЙ УРОВЕНЬ!</h2>

        <div style="display:flex;align-items:center;justify-content:center;gap:12px;margin:12px 0;">
          <span style="font-size:18px;font-weight:800;color:var(--text-muted);">Ур. ${oldLvl}</span>
          <span style="font-size:22px;color:var(--gold);">➔</span>
          <span style="font-size:26px;font-weight:900;color:var(--cyan);background:rgba(0,229,255,0.15);padding:4px 16px;border-radius:var(--radius-pill);border:1px solid var(--cyan);">Ур. ${newLvl}</span>
        </div>

        <div style="font-size:13px;font-weight:700;color:var(--violet);margin-bottom:14px;">Ранг: ${esc(rank.name)}</div>

        <div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:var(--radius-md);padding:12px;margin-bottom:18px;">
          <div style="font-size:11px;color:var(--text-muted);margin-bottom:6px;">Награды за достижение:</div>
          <div style="display:flex;justify-content:center;gap:8px;flex-wrap:wrap;">
            <span class="btn-chip" style="color:var(--gold);">🪙 +${bonusCoins} Монет</span>
            ${unlockedChars.map(c => `
              <span class="btn-chip" style="color:var(--cyan);border-color:var(--cyan);">🎖️ Разблокирован: ${c.icon} ${esc(c.name)}</span>
            `).join('')}
            ${newRoad.map(r => `
              <span class="btn-chip" style="color:#7DFFB5;border-color:rgba(0,230,118,0.5);">🛣️ Дорога ур. ${r.level}: ${r.icon} ${esc(r.title)}</span>
            `).join('')}
          </div>
        </div>

        <div style="display:flex;gap:10px;">
          ${newRoad.length ? `<button class="btn-premium-ghost" id="levelup-road-btn" style="flex:1;">🛣️ На дорогу</button>` : ''}
          <button class="btn-premium-gold" id="levelup-collect-btn" style="flex:1.5;">Забрать награды 🔥</button>
        </div>
      </div>
    </div>`;

  document.getElementById('levelup-collect-btn').onclick = () => {
    Sound.click();
    closeModal();
    render();
  };
  const roadBtn = document.getElementById('levelup-road-btn');
  if (roadBtn) roadBtn.onclick = () => {
    closeModal();
    switchTab('road');
  };
}

/* =========================================================================
   11. СВАЙП-НАВИГАЦИЯ МЕЖДУ ЭКРАНАМИ (ТАБАМИ)
   ========================================================================= */
const TABS_ORDER = ['home', 'workouts', 'cases', 'characters', 'road', 'profile'];

let touchStartX = 0;
let touchStartY = 0;
let touchEndX = 0;
let touchEndY = 0;
let touchBlocked = false;

function setupSwipeGestures() {
  const shell = document.getElementById('app-shell');

  shell.addEventListener('touchstart', (e) => {
    touchStartX = e.changedTouches[0].screenX;
    touchStartY = e.changedTouches[0].screenY;
    // Не листаем табы, если палец начал движение на горизонтальном списке
    touchBlocked = !!(e.target.closest && e.target.closest('.cat-scroll, .char-filter-tabs, .admin-tabs, .builder-exercise-list'));
  }, { passive: true });

  shell.addEventListener('touchend', (e) => {
    touchEndX = e.changedTouches[0].screenX;
    touchEndY = e.changedTouches[0].screenY;
    if (!touchBlocked) handleSwipeGesture();
  }, { passive: true });
}

function handleSwipeGesture() {
  const diffX = touchEndX - touchStartX;
  const diffY = touchEndY - touchStartY;

  if (Math.abs(diffY) > Math.abs(diffX) || Math.abs(diffX) < 55) return;
  if (document.querySelector('.modal-overlay.active')) return;

  const currentIdx = TABS_ORDER.indexOf(state.currentTab);
  if (currentIdx === -1) return;

  if (diffX < 0) {
    if (currentIdx < TABS_ORDER.length - 1) switchTab(TABS_ORDER[currentIdx + 1], 'right');
  } else {
    if (currentIdx > 0) switchTab(TABS_ORDER[currentIdx - 1], 'left');
  }
}

function switchTab(newTab, direction = null) {
  Sound.click();
  const currentIdx = TABS_ORDER.indexOf(state.currentTab);
  const newIdx = TABS_ORDER.indexOf(newTab);

  if (!direction) {
    direction = newIdx >= currentIdx ? 'right' : 'left';
  }

  state.currentTab = newTab;
  render(direction);
  const v = document.getElementById('view');
  v.style.scrollBehavior = 'auto';
  v.scrollTop = 0;
  v.style.scrollBehavior = '';
  if (newTab === 'road') scrollRoadToCurrent();
}

/* =========================================================================
   12. СТАТИСТИКА ТРЕНИРОВОК, ЭНЕРГОЗАТРАТЫ И МНОЖИТЕЛИ НАГРАД
   ========================================================================= */
function energyCost(totalKcal) {
  const base = totalKcal * state.settings.energyCostFactor;
  const reduction = Math.min(0.7, cb('cost') + trophyBonus('costMult'));
  return Math.min(maxWE(), Math.max(1, Math.round(base * (1 - reduction))));
}

function workoutStats(w) {
  const steps = w.steps && w.steps.length ? w.steps : defaultSteps(w.icon);
  let sec = 0, kcal = 0, xp = 0, coins = 0;
  steps.forEach(st => {
    sec += st.duration;
    kcal += w.kcalPerMin * st.mult * st.duration / 60;
    xp += w.xpPerMin * st.duration / 60;
    coins += w.coinsPerMin * st.duration / 60;
  });
  return {
    steps, sec,
    min: Math.max(1, Math.round(sec / 60)),
    kcal: Math.round(kcal),
    xp: Math.round(xp),
    coins: Math.round(coins),
    energy: energyCost(kcal)
  };
}

function xpMultNow() {
  let m = 1 + cb('xp') + trophyBonus('xpMult');
  if (state.boosts.xpLeft > 0) m *= state.boosts.xpMult;
  return m;
}
function coinMultNow() {
  let m = 1 + cb('coins') + trophyBonus('coinMult');
  if (state.boosts.coinLeft > 0) m *= state.boosts.coinMult;
  return m;
}

function formatSec(s) {
  if (s >= 60) {
    const m = Math.floor(s / 60);
    const sec = s % 60;
    return sec > 0 ? `${m}м ${sec}с` : `${m} мин`;
  }
  return `${s} сек`;
}

/* =========================================================================
   13. КОНСТРУКТОР ТРЕНИРОВКИ (С ФИЛЬТРОМ, КАЛОРИЯМИ И ЭНЕРГИЕЙ)
   ========================================================================= */
function idSelected(id) {
  return Object.prototype.hasOwnProperty.call(state.builderSelection, id);
}

function openBuilderModal() {
  Sound.click();
  renderBuilderSheet();
}

function renderBuilderSheet() {
  const root = document.getElementById('modal-root');
  const prevList = root.querySelector('.builder-exercise-list');
  const prevScroll = prevList ? prevList.scrollTop : 0;

  let totalSec = 0, totalXp = 0, totalCoins = 0, totalKcal = 0;

  const selectedExIds = Object.keys(state.builderSelection);
  selectedExIds.forEach(id => {
    const ex = state.exercises.find(x => x.id === id);
    if (ex) {
      const sec = state.builderSelection[id];
      totalSec += sec;
      totalXp += (ex.xpPerMin / 60) * sec;
      totalCoins += (ex.coinsPerMin / 60) * sec;
      totalKcal += (ex.kcalPerMin / 60) * sec;
    }
  });
  totalXp = Math.round(totalXp * xpMultNow());
  totalCoins = Math.round(totalCoins * coinMultNow());

  const totalMin = Math.max(1, Math.round(totalSec / 60));
  const cost = energyCost(totalKcal);
  const enough = state.workoutEnergy >= cost;

  const visible = state.exercises.filter(ex => state.builderCat === 'all' || (ex.cat || 'base') === state.builderCat);

  root.innerHTML = `
    <div class="modal-overlay active" id="builder-modal-ov">
      <div class="modal-sheet">
        <div class="modal-bar-handle"></div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
          <h2 style="font-size:18px;font-weight:900;">🛠️ Конструктор тренировки</h2>
          <span style="font-size:11px;font-weight:800;color:var(--primary);">${selectedExIds.length} выбрано</span>
        </div>
        <div class="screen-sub" style="margin-bottom:12px;">Выберите упражнения (бокс, ММА, грэпплинг, кроссфит) и настройте время:</div>

        <div class="cat-scroll" style="margin-bottom:10px;">
          ${EXERCISE_CATEGORIES.map(c => `<button class="btn-chip ${state.builderCat === c.id ? 'active' : ''}" data-b-cat="${c.id}">${c.label}</button>`).join('')}
        </div>

        <div class="builder-exercise-list">
          ${visible.map(ex => {
            const isSel = idSelected(ex.id);
            const currentSec = state.builderSelection[ex.id] || ex.defaultSec || 60;
            return `
              <div class="builder-item-card ${isSel ? 'selected' : ''}" data-item-id="${ex.id}">
                <div class="builder-item-check" data-toggle-ex="${ex.id}">${isSel ? '✓' : ''}</div>
                <div class="builder-item-icon">${ex.icon}</div>
                <div class="builder-item-info">
                  <div class="builder-item-name">${esc(ex.name)}</div>
                  <div class="builder-item-muscle">${esc(ex.muscle)} · 🔥 ${ex.kcalPerMin} ккал/мин</div>
                </div>
                ${isSel ? `
                  <div class="builder-item-timectrl">
                    <button class="time-stepper-btn" data-time-dec="${ex.id}">−</button>
                    <span class="time-stepper-val">${formatSec(currentSec)}</span>
                    <button class="time-stepper-btn" data-time-inc="${ex.id}">+</button>
                  </div>
                ` : `
                  <button class="btn-chip" data-toggle-ex="${ex.id}">Добавить</button>
                `}
              </div>`;
          }).join('')}
        </div>

        <div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:var(--radius-md);padding:12px 8px;display:grid;grid-template-columns:repeat(5,1fr);gap:4px;margin-bottom:12px;">
          <div style="text-align:center;"><div style="font-size:10px;color:var(--text-muted);">Время</div><div style="font-size:13px;font-weight:900;color:var(--cyan);">${totalMin} мин</div></div>
          <div style="text-align:center;"><div style="font-size:10px;color:var(--text-muted);">Калории</div><div style="font-size:13px;font-weight:900;color:#FF7043;">🔥 ${Math.round(totalKcal)}</div></div>
          <div style="text-align:center;"><div style="font-size:10px;color:var(--text-muted);">XP</div><div style="font-size:13px;font-weight:900;color:var(--violet);">✨ +${totalXp}</div></div>
          <div style="text-align:center;"><div style="font-size:10px;color:var(--text-muted);">Монеты</div><div style="font-size:13px;font-weight:900;color:var(--gold);">🪙 +${totalCoins}</div></div>
          <div style="text-align:center;"><div style="font-size:10px;color:var(--text-muted);">Энергия</div><div style="font-size:13px;font-weight:900;color:${enough ? '#7DFFB5' : '#FF8A9B'};">⚡ −${cost}</div></div>
        </div>
        <div style="font-size:11px;font-weight:700;text-align:center;margin-bottom:12px;color:${enough ? 'var(--text-muted)' : '#FF8A9B'};">
          ${enough ? `У вас ⚡ ${Math.floor(state.workoutEnergy)} из ${maxWE()}` : `Не хватает энергии: нужно ${cost}, есть ${Math.floor(state.workoutEnergy)}. Используйте энергетик из сумки или подождите.`}
        </div>

        <div style="display:flex;gap:10px;">
          <button class="btn-premium-ghost" id="builder-close-btn" style="flex:1;">Закрыть</button>
          <button class="btn-premium-primary" id="builder-start-btn" style="flex:2;" ${selectedExIds.length === 0 ? 'disabled' : ''}>🚀 Начать сет</button>
        </div>
      </div>
    </div>`;

  const newList = root.querySelector('.builder-exercise-list');
  if (newList) newList.scrollTop = prevScroll;

  document.getElementById('builder-close-btn').onclick = closeModal;
  document.getElementById('builder-modal-ov').onclick = (e) => {
    if (e.target.id === 'builder-modal-ov') closeModal();
  };

  root.querySelectorAll('[data-b-cat]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      state.builderCat = btn.dataset.bCat;
      renderBuilderSheet();
    };
  });

  root.querySelectorAll('[data-toggle-ex]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      const id = btn.dataset.toggleEx;
      if (idSelected(id)) {
        delete state.builderSelection[id];
      } else {
        const ex = state.exercises.find(x => x.id === id);
        state.builderSelection[id] = (ex && ex.defaultSec) || 60;
      }
      persist();
      renderBuilderSheet();
    };
  });

  root.querySelectorAll('[data-time-dec]').forEach(btn => {
    btn.onclick = (e) => {
      e.stopPropagation();
      Sound.click();
      const id = btn.dataset.timeDec;
      if (idSelected(id)) {
        state.builderSelection[id] = Math.max(15, state.builderSelection[id] - 15);
        persist();
        renderBuilderSheet();
      }
    };
  });

  root.querySelectorAll('[data-time-inc]').forEach(btn => {
    btn.onclick = (e) => {
      e.stopPropagation();
      Sound.click();
      const id = btn.dataset.timeInc;
      if (idSelected(id)) {
        state.builderSelection[id] = Math.min(600, state.builderSelection[id] + 15);
        persist();
        renderBuilderSheet();
      }
    };
  });

  document.getElementById('builder-start-btn').onclick = () => {
    startCustomBuilderWorkoutSession();
  };
}

/* =========================================================================
   14. ПЛЕЕР ТРЕНИРОВКИ: ТАЙМЕР, КАЛОРИИ, ЭНЕРГИЯ, БУСТЕРЫ
   ========================================================================= */
function beginSession(title, queue, isBuilder) {
  if (state.activeWorkoutSession) return false;
  regenTick();

  let totalKcal = 0, plannedSec = 0;
  queue.forEach(q => { totalKcal += q.kcalRate * q.duration / 60; plannedSec += q.duration; });
  const cost = energyCost(totalKcal);

  if (state.workoutEnergy < cost) {
    Sound.denied();
    showToast(`Не хватает энергии ⚡ Нужно ${cost}, есть ${Math.floor(state.workoutEnergy)}. Энергетик из сумки поможет!`);
    return false;
  }

  spendWorkoutEnergy(cost);
  persist();
  closeModal();
  Sound.fanfare();
  if (isBuilder) updateQuestProgress('builder', 1);

  state.activeWorkoutSession = {
    title,
    queue,
    currentIndex: 0,
    remainingSeconds: queue[0].duration,
    isPaused: false,
    timerId: null,
    totalElapsedSeconds: 0,
    plannedSeconds: plannedSec,
    energySpent: cost,
    xpMult: xpMultNow(),
    coinMult: coinMultNow(),
    totalXpEarned: 0,
    totalCoinsEarned: 0,
    totalCaloriesBurned: 0
  };

  renderWorkoutPlayerModal();
  runWorkoutTicker();
  return true;
}

function startCustomBuilderWorkoutSession() {
  const selectedExIds = Object.keys(state.builderSelection);
  const queue = [];

  selectedExIds.forEach(id => {
    const ex = state.exercises.find(x => x.id === id);
    if (ex) {
      queue.push({
        id: ex.id,
        name: ex.name,
        muscle: ex.muscle,
        icon: ex.icon,
        duration: state.builderSelection[id],
        xpRate: ex.xpPerMin,
        coinsRate: ex.coinsPerMin,
        kcalRate: ex.kcalPerMin
      });
    }
  });

  if (!queue.length) { showToast('Выберите хотя бы одно упражнение'); return; }
  beginSession('Мой кастомный комплекс', queue, true);
}

function startStandardWorkout(w) {
  const steps = w.steps && w.steps.length ? w.steps : defaultSteps(w.icon);
  const queue = steps.map((st, i) => ({
    id: w.id + '-s' + i,
    name: st.name,
    muscle: st.muscle,
    icon: st.icon,
    duration: st.duration,
    xpRate: w.xpPerMin,
    coinsRate: w.coinsPerMin,
    kcalRate: w.kcalPerMin * st.mult
  }));
  beginSession(w.name, queue, false);
}

function runWorkoutTicker() {
  if (state.activeWorkoutSession.timerId) clearInterval(state.activeWorkoutSession.timerId);

  state.activeWorkoutSession.timerId = setInterval(() => {
    const s = state.activeWorkoutSession;
    if (!s || s.isPaused) return;

    if (s.remainingSeconds > 0) {
      s.remainingSeconds--;
      s.totalElapsedSeconds++;

      if (s.remainingSeconds <= 3 && s.remainingSeconds > 0) {
        Sound.beep(700, 0.08);
      } else if (s.remainingSeconds === 0) {
        Sound.beep(1200, 0.25);
      } else {
        Sound.tick();
      }

      const currentEx = s.queue[s.currentIndex];
      s.totalXpEarned += (currentEx.xpRate / 60);
      s.totalCoinsEarned += (currentEx.coinsRate / 60);
      s.totalCaloriesBurned += (currentEx.kcalRate / 60);

      updateWorkoutPlayerUI();
    } else {
      if (s.currentIndex < s.queue.length - 1) {
        s.currentIndex++;
        s.remainingSeconds = s.queue[s.currentIndex].duration;
        Sound.fanfare();
        showToast(`Следующее: ${s.queue[s.currentIndex].name}!`);
        renderWorkoutPlayerModal();
      } else {
        finishWorkoutSession();
      }
    }
  }, 1000);
}

function updateWorkoutPlayerUI() {
  const s = state.activeWorkoutSession;
  if (!s) return;
  const timeEl = document.getElementById('live-player-timer');
  const kcalEl = document.getElementById('live-player-kcal');
  const xpEl = document.getElementById('live-player-xp');
  const coEl = document.getElementById('live-player-coins');
  const prEl = document.getElementById('live-player-progress');

  const m = Math.floor(s.remainingSeconds / 60);
  const sec = s.remainingSeconds % 60;
  const timeStr = `${pad2(m)}:${pad2(sec)}`;

  if (timeEl) timeEl.textContent = timeStr;
  if (kcalEl) kcalEl.textContent = `🔥 ${s.totalCaloriesBurned.toFixed(1)} ккал сожжено`;
  if (xpEl) xpEl.textContent = `✨ +${Math.round(s.totalXpEarned * s.xpMult)} XP`;
  if (coEl) coEl.textContent = `🪙 +${Math.round(s.totalCoinsEarned * s.coinMult)}`;
  if (prEl) prEl.style.width = Math.min(100, Math.round((s.totalElapsedSeconds / s.plannedSeconds) * 100)) + '%';
}

function renderWorkoutPlayerModal() {
  const s = state.activeWorkoutSession;
  if (!s) return;
  const currentEx = s.queue[s.currentIndex];
  const nextEx = s.queue[s.currentIndex + 1];
  const m = Math.floor(s.remainingSeconds / 60);
  const sec = s.remainingSeconds % 60;
  const timeStr = `${pad2(m)}:${pad2(sec)}`;
  const progress = Math.min(100, Math.round((s.totalElapsedSeconds / s.plannedSeconds) * 100));

  const root = document.getElementById('modal-root');
  root.innerHTML = `
    <div class="modal-overlay active" id="player-modal-ov">
      <div class="modal-sheet" style="text-align:center;">
        <div class="modal-bar-handle"></div>
        <div style="font-size:11px;font-weight:800;color:var(--primary);text-transform:uppercase;margin-bottom:4px;">${esc(s.title)} · Раунд ${s.currentIndex + 1} из ${s.queue.length}</div>
        <div style="font-size:48px;margin:6px 0;">${currentEx.icon}</div>
        <h2 style="font-size:20px;font-weight:900;">${esc(currentEx.name)}</h2>
        <div class="screen-sub" style="margin-top:2px;">${esc(currentEx.muscle)}</div>

        <div class="live-timer-ring" id="live-player-timer">${timeStr}</div>

        <div class="session-progress"><div class="session-progress-fill" id="live-player-progress" style="width:${progress}%;"></div></div>

        <div class="live-calorie-badge" id="live-player-kcal">
          🔥 ${s.totalCaloriesBurned.toFixed(1)} ккал сожжено
        </div>

        <div style="display:flex;justify-content:center;gap:12px;margin-bottom:12px;flex-wrap:wrap;">
          <div class="btn-chip" id="live-player-xp" style="color:var(--violet);">✨ +${Math.round(s.totalXpEarned * s.xpMult)} XP</div>
          <div class="btn-chip" id="live-player-coins" style="color:var(--gold);">🪙 +${Math.round(s.totalCoinsEarned * s.coinMult)}</div>
          <div class="btn-chip" style="color:#7DFFB5;">⚡ потрачено ${s.energySpent}</div>
        </div>

        ${(s.xpMult > 1 || s.coinMult > 1) ? `<div style="font-size:11px;font-weight:800;color:#D9A8FF;margin-bottom:12px;">Бонусы активны: XP ×${s.xpMult.toFixed(2)} · монеты ×${s.coinMult.toFixed(2)}</div>` : ''}

        ${nextEx ? `
          <div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:var(--radius-md);padding:8px 12px;font-size:11px;color:var(--text-muted);margin-bottom:16px;">
            Далее: <b>${nextEx.icon} ${esc(nextEx.name)}</b> (${formatSec(nextEx.duration)})
          </div>
        ` : ''}

        <div style="display:flex;gap:10px;">
          <button class="btn-premium-ghost" id="player-pause-btn" style="flex:1;">
            ${s.isPaused ? '▶️ Продолжить' : '⏸️ Пауза'}
          </button>
          <button class="btn-premium-ghost" id="player-skip-btn" style="flex:1;">
            ⏭️ Пропуск
          </button>
          <button class="btn-premium-primary" id="player-finish-btn" style="flex:1.4;">
            🏁 Завершить
          </button>
        </div>
      </div>
    </div>`;

  document.getElementById('player-pause-btn').onclick = () => {
    Sound.click();
    s.isPaused = !s.isPaused;
    renderWorkoutPlayerModal();
  };

  document.getElementById('player-skip-btn').onclick = () => {
    Sound.click();
    s.remainingSeconds = 0;
  };

  document.getElementById('player-finish-btn').onclick = () => {
    finishWorkoutSession();
  };
}

function finishWorkoutSession() {
  const s = state.activeWorkoutSession;
  if (!s) return;
  if (s.timerId) clearInterval(s.timerId);
  state.activeWorkoutSession = null;

  // Слишком рано — награды нет, энергия возвращается целиком
  if (s.totalElapsedSeconds < 10) {
    gainWorkoutEnergy(s.energySpent);
    persist();
    closeModal();
    showToast('Тренировка прервана слишком рано — энергия возвращена');
    render();
    return;
  }

  // Возврат энергии за непройденную часть
  const unusedShare = Math.max(0, 1 - s.totalElapsedSeconds / s.plannedSeconds);
  const refund = Math.round(s.energySpent * unusedShare);
  if (refund > 0) gainWorkoutEnergy(refund);

  const xpEarned = Math.max(1, Math.round(s.totalXpEarned * s.xpMult));
  const coinsEarned = Math.max(1, Math.round(s.totalCoinsEarned * s.coinMult));
  const caloriesBurned = Math.max(1, Math.round(s.totalCaloriesBurned));
  const elapsedMinutes = Math.max(1, Math.round(s.totalElapsedSeconds / 60));

  // Расходуем активные бустеры
  if (state.boosts.xpLeft > 0) { state.boosts.xpLeft--; if (state.boosts.xpLeft <= 0) state.boosts.xpMult = 1; }
  if (state.boosts.coinLeft > 0) { state.boosts.coinLeft--; if (state.boosts.coinLeft <= 0) state.boosts.coinMult = 1; }

  state.player.coins += coinsEarned;

  state.history.unshift({
    id: genId('hist'),
    name: s.title,
    date: new Date().toLocaleDateString('ru-RU', { hour: '2-digit', minute: '2-digit' }),
    duration: elapsedMinutes,
    calories: caloriesBurned,
    xp: xpEarned,
    coins: coinsEarned
  });
  if (state.history.length > 60) state.history.length = 60;

  if (s.totalElapsedSeconds >= 60) updateQuestProgress('workout', 1);
  persist();
  closeModal();
  Sound.fanfare();
  showToast(`🎉 Готово! +${xpEarned} XP · +${coinsEarned} 🪙 · 🔥 ${caloriesBurned} ккал${refund > 0 ? ' · ⚡ вернули ' + refund : ''}`);

  addPlayerXp(xpEarned);
  render();
}

/* =========================================================================
   15. СИСТЕМА КЕЙСОВ, РУЛЕТКА И ВЫПАДАЮЩИЕ ПРЕДМЕТЫ
   ========================================================================= */
function canClaimDailyCase() {
  const now = Date.now();
  const dayMs = 24 * 60 * 60 * 1000;
  return (now - state.lastDailyCaseTimestamp) >= dayMs;
}

function getDailyCaseTimeRemaining() {
  const now = Date.now();
  const dayMs = 24 * 60 * 60 * 1000;
  const diff = dayMs - (now - state.lastDailyCaseTimestamp);
  if (diff <= 0) return 'Готов к открытию!';
  const hours = Math.floor(diff / (1000 * 60 * 60));
  const mins = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
  return `${hours}ч ${mins}мин`;
}

// Выбор предмета с учётом редкости кейса
function rollItemFromCase(c) {
  const pool = Object.values(ITEM_CATALOG).map(it => {
    const rank = RARITY_RANK[it.rarity] || 1;
    const w = it.weight * (1 + (rank - 1) * c.rareDropChance * 2);
    return { it, w };
  });
  const total = pool.reduce((a, b) => a + b.w, 0);
  let r = Math.random() * total;
  for (const p of pool) {
    r -= p.w;
    if (r <= 0) return p.it;
  }
  return pool[0].it;
}

function rollTrophyFromCase(c) {
  const chance = 0.04 + c.rareDropChance * 0.12;
  if (Math.random() > chance) return null;
  const missing = TROPHY_DROP_ORDER.filter(id => !state.trophies.includes(id));
  if (!missing.length) return null;
  return missing[Math.floor(Math.random() * missing.length)];
}

function openCaseWithRoulette(c, payMode) {
  if (c.isDaily) {
    if (!canClaimDailyCase()) {
      Sound.denied();
      showToast(`Ежедневный кейс перезаряжается: ${getDailyCaseTimeRemaining()}`);
      return;
    }
    state.lastDailyCaseTimestamp = Date.now();
  } else {
    const useVoucher = payMode === 'voucher' || (payMode !== 'coins' && itemCount('voucher') > 0 && state.player.coins < c.price);
    if (useVoucher) {
      if (itemCount('voucher') <= 0) { showToast('Нет купонов в сумке'); return; }
      state.inventory.voucher--;
      if (state.inventory.voucher <= 0) delete state.inventory.voucher;
      showToast('Использован золотой купон на открытие!');
    } else if (state.player.coins < c.price) {
      Sound.denied();
      showToast('Недостаточно монет 🪙 для открытия кейса');
      return;
    } else {
      state.player.coins -= c.price;
    }
  }

  Sound.click();

  const wonCoins = Math.round((c.minCoins + Math.random() * (c.maxCoins - c.minCoins)) * (1 + cb('caseCoins')));
  const wonXp = Math.round(c.minXp + Math.random() * (c.maxXp - c.minXp));

  // Персонаж (только из пула кейсовых бойцов)
  let wonCharacter = null;
  const lockedCaseChars = state.characters.filter(ch => ch.unlockType === 'case' && ch.active && !state.unlockedCharacters.includes(ch.id));

  if (lockedCaseChars.length > 0 && Math.random() < c.rareDropChance) {
    const totalWeight = lockedCaseChars.reduce((sum, ch) => sum + (ch.dropWeight || 10), 0);
    let randWeight = Math.random() * totalWeight;
    for (const ch of lockedCaseChars) {
      randWeight -= (ch.dropWeight || 10);
      if (randWeight <= 0) {
        wonCharacter = ch;
        break;
      }
    }
    if (!wonCharacter) wonCharacter = lockedCaseChars[0];
    state.unlockedCharacters.push(wonCharacter.id);
  }

  // Предметы, которые потом можно использовать из сумки
  const wonItems = [];
  for (let i = 0; i < (c.itemRolls || 1); i++) {
    if (Math.random() < (c.itemChance != null ? c.itemChance : 0.6)) {
      const it = rollItemFromCase(c);
      addItem(it.id, 1);
      const ex = wonItems.find(x => x.id === it.id);
      if (ex) ex.count++; else wonItems.push({ id: it.id, count: 1 });
    }
  }

  const wonTrophyId = rollTrophyFromCase(c);
  if (wonTrophyId) state.trophies.push(wonTrophyId);

  state.player.coins += wonCoins;
  updateQuestProgress('case', 1);
  persist();

  startRouletteAnimation(c, wonCoins, wonXp, wonCharacter, wonItems, wonTrophyId);
}

function startRouletteAnimation(caseObj, wonCoins, wonXp, wonCharacter, wonItems, wonTrophyId) {
  const root = document.getElementById('modal-root');

  const dummyItems = [
    { icon: '🪙', label: '+60 Монет', rarity: 'common' },
    { icon: '✨', label: '+45 XP', rarity: 'common' },
    { icon: '🥤', label: 'Энергетик', rarity: 'common' },
    { icon: '🔋', label: 'Батарейка', rarity: 'common' },
    { icon: '🥷', label: 'Арман «Тень»', rarity: 'rare' },
    { icon: '🧃', label: 'Мега-энергетик', rarity: 'rare' },
    { icon: '🏆', label: 'Кубок Арены', rarity: 'rare' },
    { icon: '🏋️', label: 'Заур «Титан»', rarity: 'epic' },
    { icon: '🧪', label: 'Бустер XP ×2', rarity: 'epic' },
    { icon: '🎟️', label: 'Купон кейса', rarity: 'epic' },
    { icon: '⚔️', label: 'Самурай Кэндзи', rarity: 'legendary' },
    { icon: '👑', label: 'Чингиз Хан', rarity: 'mythic' },
  ];

  const trackList = [];
  for (let i = 0; i < 28; i++) {
    const dummy = dummyItems[Math.floor(Math.random() * dummyItems.length)];
    trackList.push({ ...dummy });
  }

  const winningIndex = 22;
  if (wonCharacter) {
    trackList[winningIndex] = { icon: wonCharacter.icon, label: wonCharacter.name, rarity: wonCharacter.rarity };
  } else if (wonItems.length) {
    const best = wonItems.map(w => ITEM_CATALOG[w.id]).sort((a, b) => RARITY_RANK[b.rarity] - RARITY_RANK[a.rarity])[0];
    trackList[winningIndex] = { icon: best.icon, label: best.name, rarity: best.rarity };
  } else if (wonTrophyId) {
    const t = TROPHY_CATALOG[wonTrophyId];
    trackList[winningIndex] = { icon: t.icon, label: t.name, rarity: t.rarity };
  } else {
    trackList[winningIndex] = { icon: '🪙', label: `+${wonCoins} 🪙`, rarity: 'rare' };
  }

  root.innerHTML = `
    <div class="modal-overlay active" id="roulette-modal-ov">
      <div class="modal-sheet" style="text-align:center;">
        <div class="modal-bar-handle"></div>
        <div style="font-size:12px;font-weight:800;color:var(--gold);">ОТКРЫВАЕТСЯ ${esc(caseObj.name.toUpperCase())}</div>
        <div class="roulette-container">
          <div class="roulette-pointer"></div>
          <div class="roulette-track" id="roulette-track-el">
            ${trackList.map(it => `
              <div class="roulette-card-item">
                <span class="item-ic">${it.icon}</span>
                <span class="item-lbl">${esc(it.label)}</span>
                <div class="item-rarity-bar" style="background:var(--rarity-${it.rarity});"></div>
              </div>
            `).join('')}
          </div>
        </div>
        <div class="screen-sub" id="roulette-status-txt">Крутим рулетку...</div>
      </div>
    </div>`;

  let tickDelay = 55;
  let elapsed = 0;
  const totalSpinTime = 4200;

  function doTick() {
    if (elapsed >= totalSpinTime) return;
    Sound.rouletteTick();
    elapsed += tickDelay;
    tickDelay *= 1.075;
    setTimeout(doTick, tickDelay);
  }
  doTick();

  setTimeout(() => {
    const trackEl = document.getElementById('roulette-track-el');
    if (trackEl) {
      const itemWidth = 96;
      const containerWidth = trackEl.parentElement.offsetWidth || 340;
      const targetOffset = (winningIndex * itemWidth) - (containerWidth / 2) + (itemWidth / 2);
      trackEl.style.transform = `translateX(-${targetOffset}px)`;
    }
  }, 50);

  setTimeout(() => {
    Sound.fanfare();
    showWinModal(caseObj, wonCoins, wonXp, wonCharacter, wonItems, wonTrophyId);
  }, 4500);
}

function showWinModal(caseObj, wonCoins, wonXp, wonCharacter, wonItems, wonTrophyId) {
  const root = document.getElementById('modal-root');
  const trophy = wonTrophyId ? TROPHY_CATALOG[wonTrophyId] : null;

  root.innerHTML = `
    <div class="modal-overlay active" id="win-modal-ov">
      <div class="modal-sheet" style="text-align:center;">
        <div class="modal-bar-handle"></div>
        <div style="font-size:54px;margin-bottom:8px;">${wonCharacter ? wonCharacter.icon : (caseObj.icon)}</div>
        <h2 style="font-size:22px;font-weight:900;margin-bottom:6px;">
          ${wonCharacter ? (wonCharacter.rarity === 'mythic' ? '👑 МИФИЧЕСКИЙ БОЕЦ!' : '🎉 ВЫПАЛ НОВЫЙ БОЕЦ!') : 'Награда получена!'}
        </h2>

        ${wonCharacter ? `
          <div style="display:inline-block;padding:6px 14px;border-radius:var(--radius-pill);background:var(--rarity-${wonCharacter.rarity});color:#FFF;font-weight:900;font-size:12px;margin-bottom:12px;">
            ${wonCharacter.rarity.toUpperCase()} · ${esc(wonCharacter.name)}
          </div>
          <div style="font-size:12px;color:var(--text-muted);margin-bottom:14px;">${esc(wonCharacter.perk)}</div>
        ` : ''}

        <div style="display:flex;justify-content:center;gap:10px;margin-bottom:12px;flex-wrap:wrap;">
          <div class="btn-chip" style="color:var(--gold);font-size:12px;padding:6px 14px;">🪙 +${wonCoins} монет</div>
          <div class="btn-chip" style="color:var(--violet);font-size:12px;padding:6px 14px;">✨ +${wonXp} XP</div>
        </div>

        ${(wonItems.length || trophy) ? `
          <div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:var(--radius-md);padding:12px;margin-bottom:16px;">
            <div style="font-size:11px;color:var(--text-muted);margin-bottom:8px;">🎒 Добавлено в сумку (можно использовать):</div>
            <div style="display:flex;justify-content:center;gap:8px;flex-wrap:wrap;">
              ${wonItems.map(w => {
                const it = ITEM_CATALOG[w.id];
                return `<span class="btn-chip" style="color:var(--rarity-${it.rarity});border-color:var(--rarity-${it.rarity});">${it.icon} ${esc(it.name)} ×${w.count}</span>`;
              }).join('')}
              ${trophy ? `<span class="btn-chip" style="color:var(--gold);border-color:var(--gold);">${trophy.icon} Трофей: ${esc(trophy.name)}</span>` : ''}
            </div>
          </div>
        ` : ''}

        <div style="display:flex;gap:10px;">
          ${(wonItems.length || trophy) ? `<button class="btn-premium-ghost" id="win-bag-btn" style="flex:1;">🎒 В сумку</button>` : ''}
          <button class="btn-premium-primary" id="win-collect-btn" style="flex:1.6;">Отлично, забрать!</button>
        </div>
      </div>
    </div>`;

  document.getElementById('win-collect-btn').onclick = () => {
    Sound.click();
    closeModal();
    addPlayerXp(wonXp);
    render();
  };
  const bagBtn = document.getElementById('win-bag-btn');
  if (bagBtn) bagBtn.onclick = () => {
    Sound.click();
    closeModal();
    addPlayerXp(wonXp);
    render();
    if (!document.getElementById('levelup-modal-ov')) openInventoryModal('items');
  };
}

/* =========================================================================
   16. СУМКА: ОКНО ИНВЕНТАРЯ
   ========================================================================= */
let inventoryTab = 'items';

function openInventoryModal(tab) {
  if (tab) inventoryTab = tab;
  Sound.click();
  renderInventoryModal();
}

function renderInventoryModal() {
  const root = document.getElementById('modal-root');
  const ids = Object.keys(state.inventory).filter(id => state.inventory[id] > 0 && ITEM_CATALOG[id])
    .sort((a, b) => RARITY_RANK[ITEM_CATALOG[b].rarity] - RARITY_RANK[ITEM_CATALOG[a].rarity]);

  root.innerHTML = `
    <div class="modal-overlay active" id="inv-modal-ov">
      <div class="modal-sheet">
        <div class="modal-bar-handle"></div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
          <h2 style="font-size:18px;font-weight:900;">🎒 Сумка</h2>
          <span class="btn-chip" style="color:var(--gold);">🪙 ${state.player.coins}</span>
        </div>
        <div class="screen-sub" style="margin-bottom:12px;">Всё, что выпало из кейсов и дороги наград, можно применить здесь</div>

        ${boostPillsHtml()}

        <div class="inv-tabs" style="margin-top:12px;">
          <button class="btn-chip ${inventoryTab === 'items' ? 'active' : ''}" data-inv-tab="items">🧪 Предметы (${totalItems()})</button>
          <button class="btn-chip ${inventoryTab === 'trophies' ? 'active' : ''}" data-inv-tab="trophies">🏆 Трофеи (${state.trophies.length})</button>
        </div>

        ${inventoryTab === 'items' ? (ids.length ? `
          <div class="inv-grid">
            ${ids.map(id => {
              const it = ITEM_CATALOG[id];
              return `
                <div class="inv-item rarity-${it.rarity}">
                  <span class="inv-count">×${state.inventory[id]}</span>
                  <div class="inv-ic">${it.icon}</div>
                  <div class="inv-name">${esc(it.name)}</div>
                  <div class="inv-desc">${esc(it.desc)}</div>
                  <button class="btn-premium-emerald inv-use-btn" data-use-item="${id}">${id === 'voucher' ? 'К кейсам' : 'Применить'}</button>
                </div>`;
            }).join('')}
          </div>
        ` : `
          <div class="inv-empty">🎒 Сумка пуста.<br>Открывайте кейсы и забирайте награды на Дороге уровней!</div>
        `) : `
          <div>
            ${state.trophies.map(tid => {
              const t = TROPHY_CATALOG[tid];
              if (!t) return '';
              const eq = state.equippedTrophy === tid;
              return `
                <div class="trophy-row ${eq ? 'equipped' : ''}">
                  <div class="tr-ic">${t.icon}</div>
                  <div class="tr-info">
                    <div class="tr-name">${esc(t.name)} ${eq ? '· надето' : ''}</div>
                    <div class="tr-desc">${esc(t.desc)}</div>
                  </div>
                  <button class="btn-chip ${eq ? 'active' : ''}" data-equip-trophy="${tid}">${eq ? 'Снять' : 'Надеть'}</button>
                </div>`;
            }).join('')}
          </div>
        `}

        <button class="btn-premium-ghost" id="inv-close-btn" style="width:100%;margin-top:6px;">Закрыть</button>
      </div>
    </div>`;

  document.getElementById('inv-close-btn').onclick = () => { closeModal(); render(); };
  document.getElementById('inv-modal-ov').onclick = (e) => { if (e.target.id === 'inv-modal-ov') { closeModal(); render(); } };

  root.querySelectorAll('[data-inv-tab]').forEach(btn => {
    btn.onclick = () => { Sound.click(); inventoryTab = btn.dataset.invTab; renderInventoryModal(); };
  });
  root.querySelectorAll('[data-use-item]').forEach(btn => {
    btn.onclick = () => {
      const ok = useItem(btn.dataset.useItem);
      if (ok && !document.getElementById('levelup-modal-ov')) {
        renderInventoryModal();
        render();
      }
    };
  });
  root.querySelectorAll('[data-equip-trophy]').forEach(btn => {
    btn.onclick = () => {
      equipTrophy(btn.dataset.equipTrophy);
      renderInventoryModal();
      render();
    };
  });
}

/* =========================================================================
   17. АВТО-ТРЕНИРОВКИ ПЕРСОНАЖЕЙ
   ========================================================================= */
function trainingDurationMs(ch) {
  const reduce = (ch.bonus && ch.bonus.autoTime) || 0;
  return Math.round(5 * 60 * 1000 * (1 - reduce));
}

function sendCharToTraining(charId) {
  Sound.click();
  const ch = state.characters.find(x => x.id === charId);
  const dur = trainingDurationMs(ch);
  state.charTrainings[charId] = Date.now() + dur;
  persist();
  render();
  showToast(`${ch.name} отправлен на авто-тренировку (${Math.round(dur / 60000 * 10) / 10} мин)!`);
}

function collectCharTrainingReward(charId) {
  Sound.fanfare();
  const ch = state.characters.find(x => x.id === charId);
  delete state.charTrainings[charId];

  let bonusCoins = 130;
  let bonusXp = 85;
  if (ch.rarity === 'epic') { bonusCoins = 240; bonusXp = 150; }
  else if (ch.rarity === 'legendary') { bonusCoins = 480; bonusXp = 300; }
  else if (ch.rarity === 'mythic') { bonusCoins = 900; bonusXp = 550; }

  const mult = 1 + ((ch.bonus && ch.bonus.auto) || 0);
  bonusCoins = Math.round(bonusCoins * mult);
  bonusXp = Math.round(bonusXp * mult);

  state.player.coins += bonusCoins;
  persist();
  addPlayerXp(bonusXp);
  render();
  showToast(`Добыча с тренировки ${ch.name}! +${bonusCoins}🪙 +${bonusXp}XP`);
}

/* =========================================================================
   18. КЛИКЕР-ТАПАЛКА: ЛИМИТ 200, +1 УДАР КАЖДЫЕ 3 СЕКУНДЫ
   ========================================================================= */
function tapPunch(e) {
  regenTick();
  if (state.clickerEnergy < 1) {
    Sound.denied();
    showToast(`Удары закончились! +1 каждые ${state.settings.clickerRegenSec} сек. Или примените 🔋 батарейку.`);
    return;
  }

  spendClick();

  const crit = Math.random() < cb('crit');
  const mult = (1 + cb('tap')) * (crit ? state.settings.critMultiplier : 1);
  const xpGain = Math.max(1, Math.round(state.settings.tapXp * mult));
  const coinsGain = Math.max(1, Math.round(state.settings.tapCoins * mult));

  if (crit) Sound.crit(); else Sound.punch();

  state.player.coins += coinsGain;
  updateQuestProgress('click', 1);
  addPlayerXp(xpGain);

  const pop = document.createElement('div');
  pop.className = 'floating-punch-text' + (crit ? ' crit' : '');
  pop.textContent = (crit ? 'КРИТ! ' : '') + `+${xpGain}XP +${coinsGain}🪙`;
  const target = e.currentTarget || e.target;
  const rect = target.getBoundingClientRect();
  const shellRect = document.getElementById('app-shell').getBoundingClientRect();
  pop.style.left = (rect.left - shellRect.left + rect.width / 2 - 40 + (Math.random() * 30 - 15)) + 'px';
  pop.style.top = (rect.top - shellRect.top + (Math.random() * 20 - 10)) + 'px';
  document.getElementById('app-shell').appendChild(pop);
  setTimeout(() => pop.remove(), 750);

  updateClickerUI();
}

function updateClickerUI() {
  updateEnergyUI();
  updateCoinsUI();
}

/* =========================================================================
   19. РЕНДЕР СТРАНИЦ ПРИЛОЖЕНИЯ
   ========================================================================= */
function renderNav() {
  const roadCount = roadClaimable().length;
  const tabs = [
    { id: 'home', label: 'Главная', ic: '🏠' },
    { id: 'workouts', label: 'Тренировки', ic: '🏋️' },
    { id: 'cases', label: 'Кейсы', ic: '🎁' },
    { id: 'characters', label: 'Бойцы', ic: '🎖️' },
    { id: 'road', label: 'Дорога', ic: '🛣️', dot: roadCount },
    { id: 'profile', label: 'Профиль', ic: '⚙️' },
  ];

  document.getElementById('bottom-nav').innerHTML = tabs.map(t => `
    <button class="tab-btn ${state.currentTab === t.id ? 'active' : ''}" data-tab="${t.id}">
      <span class="tab-ic">${t.ic}${t.dot ? `<span class="tab-dot">${t.dot}</span>` : ''}</span>
      <span>${t.label}</span>
    </button>
  `).join('');

  document.querySelectorAll('[data-tab]').forEach(btn => {
    btn.onclick = () => switchTab(btn.dataset.tab);
  });
}

function questRowHtml(q) {
  const isDone = q.current >= q.target;
  const pct = Math.min(100, Math.round((q.current / q.target) * 100));
  return `
    <div class="card quest-row">
      <div class="quest-icon-wrap">${q.icon}</div>
      <div class="quest-info">
        <div class="quest-name">${esc(q.title)} <span style="font-size:9px;color:var(--text-dim);">${q.period === 'week' ? '· неделя' : '· день'}</span></div>
        <div class="quest-bar"><div class="quest-fill" style="width:${pct}%;"></div></div>
        <div class="quest-meta">+${q.rewardCoins}🪙 +${q.rewardXp}XP (${q.current}/${q.target})</div>
      </div>
      ${q.claimed
        ? `<span style="font-size:11px;font-weight:800;color:var(--text-muted);">✓ Взято</span>`
        : `<button class="btn-premium-primary" data-claim-quest="${q.id}" style="padding:6px 12px;font-size:11px;" ${!isDone ? 'disabled' : ''}>Забрать</button>`
      }
    </div>`;
}

function workoutCardHtml(w, compact) {
  const st = workoutStats(w);
  const xpShown = Math.round(st.xp * xpMultNow());
  const coinsShown = Math.round(st.coins * coinMultNow());
  const lacks = state.workoutEnergy < st.energy;
  return `
    <div class="card workout-item">
      <div class="workout-icon-wrap">${w.icon}</div>
      <div class="workout-details">
        ${w.cat ? `<span class="cat-tag">${CATEGORY_NAMES[w.cat] || w.cat}</span>` : ''}
        <div class="workout-title">${esc(w.name)}</div>
        <div class="workout-desc">${esc(w.description)}</div>
        <div class="workout-rate-chips">
          <span class="rate-chip fire">🔥 <b>~${st.kcal} ккал</b></span>
          <span class="rate-chip">✨ <b>+${xpShown} XP</b></span>
          <span class="rate-chip">🪙 <b>+${coinsShown}</b></span>
          <span class="rate-chip">⏱️ ${st.min} мин · ${st.steps.length} раунд.</span>
          <span class="rate-chip energy ${lacks ? 'bad' : ''}">⚡ <b>−${st.energy}</b></span>
        </div>
      </div>
      <button class="btn-premium-primary" data-start-w="${w.id}" style="padding:9px 14px;font-size:12px;flex-shrink:0;">${compact ? 'Старт' : 'Начать'}</button>
    </div>`;
}

function quickItemsRow(ids) {
  const have = ids.filter(id => itemCount(id) > 0);
  if (!have.length) return '';
  return `
    <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px;">
      ${have.map(id => {
        const it = ITEM_CATALOG[id];
        return `<button class="btn-chip" style="color:var(--rarity-${it.rarity});border-color:var(--rarity-${it.rarity});" data-quick-use="${id}">${it.icon} ${esc(it.name)} ×${itemCount(id)}</button>`;
      }).join('')}
    </div>`;
}

function renderHome() {
  const lvl = calcLevel(state.player.xp);
  const rank = getRank(lvl.level);
  const xpPct = Math.round((lvl.xpIntoLevel / lvl.xpForNextLevel) * 100);
  const activeChar = getActiveChar();
  const claimable = roadClaimable();
  const next = nextRoadReward();
  const bonuses = bonusSummary();

  let roadSub = 'Все награды получены 🎉';
  if (claimable.length) roadSub = `Ждут тебя: ${claimable.length} — забирай! 🎁`;
  else if (next) roadSub = `Следующая награда на ур. ${next.level} (ещё ${next.level - lvl.level} ур.)`;

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <div>
          <div class="screen-sub">Твой путь к величию 💪</div>
          <h1 class="screen-title">FitQuest Pro 🔥</h1>
        </div>
        <div class="coins-badge js-coins">🪙 ${state.player.coins}</div>
      </div>

      <div class="hero-level-card">
        <div class="level-row-top">
          <div>
            <div class="level-num">Уровень ${lvl.level}</div>
            <div class="rank-badge">${esc(rank.name)}</div>
          </div>
          <div class="hero-char-avatar" title="${esc(activeChar.name)}">
            ${activeChar.icon}
          </div>
        </div>
        <div class="hero-xp-track">
          <div class="hero-xp-fill" style="width:${xpPct}%;"></div>
        </div>
        <div class="hero-xp-meta">
          <span>${lvl.xpIntoLevel} / ${lvl.xpForNextLevel} XP</span>
          <span>${xpPct}% до ${lvl.level + 1} ур.</span>
        </div>
        ${bonuses.length ? `<div class="hero-bonus-line">${esc(bonuses.join(' · '))}</div>` : ''}
      </div>

      <div class="stat-bars">
        ${energyBarHtml('w')}
        ${energyBarHtml('c')}
        ${boostPillsHtml()}
      </div>

      <div class="quick-grid">
        <button class="quick-card cases-btn" id="home-to-cases">
          <span class="q-ic">🎁</span>
          <span class="q-title">Кейсы</span>
          <span class="q-sub">${canClaimDailyCase() ? '⭐ Доступен!' : 'Рулетка'}</span>
        </button>
        <button class="quick-card clicker-btn" id="home-to-clicker">
          <span class="q-ic">👊</span>
          <span class="q-title">Кликер</span>
          <span class="q-sub">Тапай</span>
        </button>
        <button class="quick-card builder-btn" id="home-to-builder">
          <span class="q-ic">🛠️</span>
          <span class="q-title">Сборка</span>
          <span class="q-sub">Свой сет</span>
        </button>
        <button class="quick-card bag-btn" id="home-to-bag">
          <span class="q-ic">🎒</span>
          <span class="q-title">Сумка</span>
          <span class="q-sub">${totalItems()} предм.</span>
        </button>
      </div>

      <div class="card road-summary" id="home-to-road" style="cursor:pointer;">
        <div class="rs-ic">🛣️</div>
        <div class="rs-main">
          <div class="rs-title">Дорога уровней</div>
          <div class="rs-sub">${esc(roadSub)}</div>
        </div>
        <span class="btn-chip ${claimable.length ? 'active' : ''}">${claimable.length ? '🎁 ' + claimable.length : '→'}</span>
      </div>

      <div class="section-title-row">
        <h2 class="section-title">Квесты</h2>
      </div>
      <div style="margin-bottom:20px;">
        ${state.quests.map(questRowHtml).join('')}
      </div>

      <div class="section-title-row">
        <h2 class="section-title">Быстрый старт тренировки</h2>
        <button class="section-link" id="home-all-workouts-link">Все (${state.workouts.filter(w => w.active !== false).length}) →</button>
      </div>
      <div>
        ${state.workouts.filter(w => w.active !== false).slice(0, 3).map(w => workoutCardHtml(w, true)).join('')}
      </div>
    </div>`;
}

function renderWorkouts() {
  const list = state.workouts.filter(w => w.active !== false && (state.workoutCat === 'all' || w.cat === state.workoutCat));

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <div>
          <h1 class="screen-title">🏋️ Тренировки</h1>
          <div class="screen-sub">${state.workouts.filter(w => w.active !== false).length} программ: ММА, бокс, грэпплинг, кроссфит</div>
        </div>
        <button class="btn-premium-gold" id="btn-open-constructor" style="padding:9px 14px;font-size:12px;">🛠️ Конструктор</button>
      </div>

      <div class="stat-bars" style="margin-bottom:14px;">
        ${energyBarHtml('w')}
        ${boostPillsHtml()}
      </div>

      ${quickItemsRow(['energy_drink', 'protein', 'mega_energy', 'xp_boost', 'coin_boost'])}

      <div style="background:rgba(123,92,250,0.1);border:1px solid rgba(123,92,250,0.3);border-radius:var(--radius-md);padding:14px;margin-bottom:16px;display:flex;align-items:center;gap:12px;">
        <div style="font-size:28px;">⚙️</div>
        <div style="flex:1;">
          <div style="font-size:13px;font-weight:800;color:#FFF;">Индивидуальная тренировка</div>
          <div style="font-size:11px;color:var(--text-muted);">51 упражнение, точное время и расчёт калорий и энергии!</div>
        </div>
        <button class="btn-premium-violet" id="banner-constructor-btn" style="padding:8px 12px;font-size:11.5px;">Собрать</button>
      </div>

      <div class="section-title-row">
        <h2 class="section-title">Готовые комплексы</h2>
        <button class="section-link" id="workouts-open-bag">🎒 Сумка (${totalItems()})</button>
      </div>

      <div class="cat-scroll">
        ${WORKOUT_CATEGORIES.map(c => `<button class="btn-chip ${state.workoutCat === c.id ? 'active' : ''}" data-w-cat="${c.id}">${c.label}</button>`).join('')}
      </div>

      <div>
        ${list.length ? list.map(w => workoutCardHtml(w, false)).join('') : `<div class="inv-empty">В этой категории пока нет тренировок</div>`}
      </div>
    </div>`;
}

function renderCases() {
  const dailyReady = canClaimDailyCase();
  const dailyTime = getDailyCaseTimeRemaining();
  const vouchers = itemCount('voucher');

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <div>
          <h1 class="screen-title">🎁 Кейсы & Рулетка</h1>
          <div class="screen-sub">Бойцы, предметы для использования, трофеи и монеты!</div>
        </div>
        <div style="display:flex;gap:6px;align-items:center;">
          ${vouchers > 0 ? `<span class="btn-chip" style="color:var(--cyan);border-color:var(--cyan);">🎟️ ${vouchers}</span>` : ''}
          <div class="coins-badge js-coins">🪙 ${state.player.coins}</div>
        </div>
      </div>

      <div style="display:flex;gap:8px;margin-bottom:14px;">
        <button class="btn-premium-ghost" id="cases-open-bag" style="flex:1;padding:10px;font-size:12px;">🎒 Сумка (${totalItems()})</button>
        <button class="btn-premium-ghost" id="cases-open-trophies" style="flex:1;padding:10px;font-size:12px;">🏆 Трофеи (${state.trophies.length})</button>
      </div>

      <div style="display:flex;flex-direction:column;gap:14px;">
        ${state.cases.filter(c => c.active !== false).map(c => {
          const lootRow = `
            <div class="case-loot-row">
              <span>🎒 до ${c.itemRolls || 1} предм.</span>
              <span>🎖️ шанс бойца ${Math.round(c.rareDropChance * 100)}%</span>
            </div>`;

          if (c.isDaily) {
            return `
              <div class="card case-banner-card daily-card">
                <div class="case-banner-icon">${c.icon}</div>
                <div class="case-banner-title">${esc(c.name)}</div>
                <div class="case-banner-desc">${esc(c.desc)}</div>
                <div style="display:flex;justify-content:center;gap:12px;font-size:12px;font-weight:800;color:var(--emerald);margin-bottom:8px;">
                  <span>🪙 ${c.minCoins}–${c.maxCoins}</span>
                  <span>✨ ${c.minXp}–${c.maxXp} XP</span>
                </div>
                ${lootRow}
                <button class="btn-premium-emerald" data-open-case="${c.id}" style="width:100%;" ${!dailyReady ? 'disabled' : ''}>
                  ${dailyReady ? '🎁 Забрать бесплатно!' : `⏳ Перезарядка: ${dailyTime}`}
                </button>
              </div>`;
          }

          return `
            <div class="card case-banner-card">
              <div class="case-banner-icon">${c.icon}</div>
              <div class="case-banner-title">${esc(c.name)}</div>
              <div class="case-banner-desc">${esc(c.desc)}</div>
              <div style="display:flex;justify-content:center;gap:12px;font-size:12px;font-weight:800;color:var(--gold);margin-bottom:8px;">
                <span>🪙 ${c.minCoins}–${c.maxCoins}</span>
                <span>✨ ${c.minXp}–${c.maxXp} XP</span>
              </div>
              ${lootRow}
              ${vouchers > 0 ? `
                <div style="display:flex;gap:8px;">
                  <button class="btn-premium-emerald" data-open-case="${c.id}" data-pay="voucher" style="flex:1;font-size:12.5px;padding:12px 8px;">🎟️ По купону</button>
                  <button class="btn-premium-gold" data-open-case="${c.id}" data-pay="coins" style="flex:1;font-size:12.5px;padding:12px 8px;">За ${c.price} 🪙</button>
                </div>
              ` : `
                <button class="btn-premium-gold" data-open-case="${c.id}" data-pay="coins" style="width:100%;">
                  Открыть за ${c.price} 🪙
                </button>
              `}
            </div>`;
        }).join('')}
      </div>
    </div>`;
}

function renderCharacters() {
  const now = Date.now();
  const lvl = calcLevel(state.player.xp).level;

  let filteredChars = state.characters;
  if (state.charSubTab === 'level') filteredChars = state.characters.filter(c => c.unlockType === 'level');
  else if (state.charSubTab === 'case') filteredChars = state.characters.filter(c => c.unlockType === 'case');

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <div>
          <h1 class="screen-title">🎖️ Бойцы (${state.characters.length})</h1>
          <div class="screen-sub">Бонус выбранного бойца работает в тренировках, кликере и кейсах</div>
        </div>
        <div class="coins-badge js-coins">🪙 ${state.player.coins}</div>
      </div>

      <div class="char-filter-tabs">
        <button class="btn-chip ${state.charSubTab === 'all' ? 'active' : ''}" data-char-filter="all">Все (${state.characters.length})</button>
        <button class="btn-chip ${state.charSubTab === 'level' ? 'active' : ''}" data-char-filter="level">🏆 За Уровни</button>
        <button class="btn-chip ${state.charSubTab === 'case' ? 'active' : ''}" data-char-filter="case">🎁 Из Кейсов</button>
      </div>

      <div class="characters-grid">
        ${filteredChars.map(c => {
          const isUnlocked = state.unlockedCharacters.includes(c.id);
          const isSelected = state.activeCharacterId === c.id;
          const trainEnd = state.charTrainings[c.id];
          const isTraining = trainEnd && trainEnd > now;
          const trainDone = trainEnd && trainEnd <= now;
          const mins = Math.round(trainingDurationMs(c) / 60000 * 10) / 10;

          return `
            <div class="char-box rarity-${c.rarity}">
              <div class="char-avatar-frame ${isUnlocked ? '' : 'locked'}">
                ${isUnlocked ? c.icon : '🔒'}
              </div>
              <div class="char-meta-col">
                <div class="char-headline">
                  <span class="char-name-txt">${esc(c.name)}</span>
                  <span class="rarity-pill ${c.rarity}">${c.rarity}</span>
                  <span class="unlock-source-tag ${c.unlockType}">${c.unlockType === 'level' ? `🏆 Ур. ${c.unlockLevel}` : '🎁 Кейс'}</span>
                </div>
                <div class="char-sport-lbl">${esc(c.sport)}</div>
                <div class="char-perk-lbl">${isUnlocked ? esc(c.perk) : (c.unlockType === 'level' ? `🏆 Достигните ${c.unlockLevel} уровня` : '🎁 Выпадает из кейсов в рулетке')}</div>
              </div>
              <div class="char-btn-col">
                ${isUnlocked ? `
                  <button class="btn-chip ${isSelected ? 'active' : ''}" data-select-char="${c.id}">
                    ${isSelected ? '✓ Выбран' : 'Выбрать'}
                  </button>
                  ${isTraining ? `
                    <span style="font-size:10px;color:var(--text-muted);">⏳ В зале...</span>
                  ` : (trainDone ? `
                    <button class="btn-premium-gold" data-collect-char="${c.id}" style="padding:5px 10px;font-size:10.5px;">🎁 Забрать</button>
                  ` : `
                    <button class="btn-premium-violet" data-train-char="${c.id}" style="padding:5px 10px;font-size:10.5px;">🏋️ В зал (${mins}м)</button>
                  `)}
                ` : `
                  <span style="font-size:11px;color:var(--text-dim);font-weight:800;">
                    ${c.unlockType === 'level' ? `Ур. ${c.unlockLevel} 🔒` : 'Кейс 🎁'}
                  </span>
                `}
              </div>
            </div>`;
        }).join('')}
      </div>
    </div>`;
}

/* ---------------- ДОРОГА УРОВНЕЙ ---------------- */
function renderRoad() {
  const lvl = calcLevel(state.player.xp).level;
  const claimable = roadClaimable();
  const next = nextRoadReward();
  const totalMilestones = ROAD_REWARDS.length;
  const claimedCount = state.roadClaimed.length;

  let rows = '';
  for (let l = 1; l <= ROAD_MAX_LEVEL; l++) {
    const r = ROAD_REWARDS.find(x => x.level === l);

    if (r) {
      const reached = lvl >= l;
      const claimed = state.roadClaimed.includes(l);
      const canClaim = reached && !claimed;
      const nodeCls = claimed ? 'claimed' : (canClaim ? 'claimable' : (reached ? 'reached' : ''));
      rows += `
        <div class="road-row" ${lvl === l ? 'id="road-current"' : ''}>
          <div class="road-node ${nodeCls}">${claimed ? '✓' : r.icon}<small>${l}</small></div>
          <div class="card road-card ${canClaim ? 'claimable' : ''}">
            <div class="rc-head">
              <span class="rc-lvl">Ур. ${l} · ${esc(r.title)}</span>
              <span class="rc-state">${claimed ? '✓ Получено' : (canClaim ? '🎁 Готово!' : '🔒 Закрыто')}</span>
            </div>
            <div class="rc-chips">
              ${describeGive(r.give).map(t => `<span class="rate-chip"><b>${esc(t)}</b></span>`).join('')}
            </div>
            ${canClaim ? `<button class="btn-premium-gold" data-road-claim="${l}" style="width:100%;padding:10px;font-size:12.5px;">Забрать награду</button>` : ''}
          </div>
        </div>`;
    } else {
      const reached = lvl >= l;
      const isCur = lvl === l;
      rows += `
        <div class="road-dot-row" ${isCur ? 'id="road-current"' : ''}>
          <div class="road-dot ${reached ? 'reached' : ''} ${isCur ? 'current' : ''}"><span></span></div>
          <span class="road-dot-lbl ${isCur ? 'current' : ''}">${isCur ? `Вы здесь · Ур. ${l}` : `Ур. ${l}`}</span>
        </div>`;
    }
  }

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <div>
          <h1 class="screen-title">🛣️ Дорога уровней</h1>
          <div class="screen-sub">Награда каждые ${ROAD_STEP} уровня — даже если было тяжело</div>
        </div>
        <div class="coins-badge js-coins">🪙 ${state.player.coins}</div>
      </div>

      <div class="card road-summary">
        <div class="rs-ic">${claimable.length ? '🎁' : '🏁'}</div>
        <div class="rs-main">
          <div class="rs-title">Ваш уровень: ${lvl}</div>
          <div class="rs-sub">
            ${claimable.length ? `Ждёт наград: ${claimable.length}` : (next ? `Следующая награда на ур. ${next.level} — ${next.icon} ${esc(next.title)}` : 'Все награды получены')}
            · Собрано ${claimedCount}/${totalMilestones}
          </div>
        </div>
        ${claimable.length ? `<button class="btn-premium-gold" id="road-claim-all" style="padding:9px 12px;font-size:12px;">Забрать все</button>` : ''}
      </div>

      <div class="road-wrap">
        <div class="road-line"></div>
        ${rows}
      </div>

      <div class="road-end">🏁 Дорога идёт до уровня ${ROAD_MAX_LEVEL}.<br>Копите опыт — впереди легендарные награды на ур. 30 и 60!</div>
    </div>`;
}

function scrollRoadToCurrent() {
  setTimeout(() => {
    const el = document.getElementById('road-current');
    const v = document.getElementById('view');
    if (!el || !v) return;
    const delta = el.getBoundingClientRect().top - v.getBoundingClientRect().top - v.clientHeight / 3;
    v.scrollTop += delta;
  }, 60);
}

function renderProfile() {
  const lvl = calcLevel(state.player.xp);
  const rank = getRank(lvl.level);
  const totalKcal = state.history.reduce((a, h) => a + (h.calories || 0), 0);
  const eqT = TROPHY_CATALOG[state.equippedTrophy];

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <h1 class="screen-title">⚙️ Профиль & Админка</h1>
      </div>

      <div class="card" style="display:flex;align-items:center;gap:14px;margin-bottom:16px;padding:16px;">
        <div style="width:58px;height:58px;border-radius:var(--radius-md);background:var(--primary-gradient);display:flex;align-items:center;justify-content:center;font-size:28px;">
          ${getActiveChar().icon}
        </div>
        <div style="flex:1;">
          <div style="font-size:16px;font-weight:900;">Атлет FitQuest</div>
          <div style="font-size:11.5px;color:var(--cyan);font-weight:700;">${esc(rank.name)} · Уровень ${lvl.level}</div>
          ${eqT && state.trophies.includes(eqT.id) ? `<div style="font-size:10.5px;color:var(--gold);font-weight:800;margin-top:2px;">${eqT.icon} Титул: ${esc(eqT.name)}</div>` : ''}
        </div>
        <div class="coins-badge js-coins">🪙 ${state.player.coins}</div>
      </div>

      <div style="display:grid;grid-template-columns:repeat(3, 1fr);gap:10px;margin-bottom:10px;">
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:var(--cyan);">${lvl.level}</div><div style="font-size:10px;color:var(--text-muted);">Уровень</div></div>
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:var(--violet);">${state.history.length}</div><div style="font-size:10px;color:var(--text-muted);">Тренировок</div></div>
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:var(--gold);">${state.unlockedCharacters.length} / ${state.characters.length}</div><div style="font-size:10px;color:var(--text-muted);">Бойцов</div></div>
      </div>
      <div style="display:grid;grid-template-columns:repeat(3, 1fr);gap:10px;margin-bottom:16px;">
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:#FF7043;">${totalKcal}</div><div style="font-size:10px;color:var(--text-muted);">Ккал сожжено</div></div>
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:var(--emerald);">${totalItems()}</div><div style="font-size:10px;color:var(--text-muted);">Предметов</div></div>
        <div class="card" style="text-align:center;padding:12px 8px;"><div style="font-size:18px;font-weight:900;color:#BFAEFF;">${state.roadClaimed.length}/${ROAD_REWARDS.length}</div><div style="font-size:10px;color:var(--text-muted);">Наград дороги</div></div>
      </div>

      <div class="card" style="margin-bottom:16px;padding:14px;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
          <div style="font-size:13px;font-weight:800;">🏆 Трофеи (${state.trophies.length})</div>
          <button class="btn-chip" id="profile-open-bag">🎒 Открыть сумку</button>
        </div>
        <div style="display:flex;gap:8px;flex-wrap:wrap;">
          ${state.trophies.map(tid => {
            const t = TROPHY_CATALOG[tid];
            if (!t) return '';
            return `<span class="btn-chip" style="color:var(--gold);border-color:rgba(255,184,0,${state.equippedTrophy === tid ? '0.9' : '0.3'});">${t.icon} ${esc(t.name)}${state.equippedTrophy === tid ? ' ✓' : ''}</span>`;
          }).join('')}
        </div>
      </div>

      <div class="card" style="margin-bottom:16px;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
          <div style="display:flex;align-items:center;gap:8px;">
            <span style="font-size:20px;">🛡️</span>
            <span style="font-weight:800;font-size:14px;">Секретная Панель Администратора</span>
          </div>
          ${state.isAdmin ? `<span class="admin-badge-tag">Доступ открыт</span>` : ''}
        </div>
        <div class="screen-sub" style="margin-bottom:14px;">
          Анимация меню, энергия и кликер, цены кейсов, шансы бойцов, награды тренировок. Все правки сохраняются кнопкой «Сохранить изменения».
        </div>
        ${state.isAdmin ? `
          <button class="btn-premium-violet" id="open-admin-dashboard-btn" style="width:100%;margin-bottom:8px;">
            Открыть настройки админки →
          </button>
          <button class="btn-premium-ghost" id="admin-logout-btn" style="width:100%;color:var(--danger);font-size:12px;padding:8px;">
            Выйти из режима админа
          </button>
        ` : `
          <button class="btn-premium-primary" id="open-admin-login-btn" style="width:100%;">
            Вход для администратора 🔑
          </button>
        `}
      </div>

      <div class="section-title-row">
        <h2 class="section-title">История тренировок</h2>
      </div>
      <div>
        ${state.history.length ? state.history.slice(0, 8).map(h => `
          <div class="card" style="display:flex;justify-content:space-between;align-items:center;padding:12px 14px;margin-bottom:8px;">
            <div style="min-width:0;">
              <div style="font-size:13px;font-weight:800;">${esc(h.name)}</div>
              <div style="font-size:10.5px;color:var(--text-muted);">${esc(h.date)} · ${h.duration || 1} мин</div>
            </div>
            <div style="text-align:right;flex-shrink:0;">
              <div style="font-size:11px;font-weight:800;color:#FF7043;">🔥 ${h.calories || 0} ккал</div>
              <div style="font-size:10.5px;font-weight:800;color:var(--gold);">+${h.coins} 🪙 · +${h.xp} XP</div>
            </div>
          </div>
        `).join('') : `
          <div style="text-align:center;padding:30px;color:var(--text-muted);font-size:12px;">Пока нет завершенных тренировок</div>
        `}
      </div>
    </div>`;
}

function renderClickerTab() {
  const tapXp = Math.max(1, Math.round(state.settings.tapXp * (1 + cb('tap'))));
  const tapCoins = Math.max(1, Math.round(state.settings.tapCoins * (1 + cb('tap'))));
  const critChance = Math.round(cb('crit') * 100);

  return `
    <div class="screen-pad">
      <div class="screen-header">
        <button class="btn-chip" id="clicker-back-btn">← Назад</button>
        <h1 class="screen-title" style="margin:0;font-size:18px;">👊 Боксёрская груша</h1>
        <div class="coins-badge js-coins" id="clicker-coins-val">🪙 ${state.player.coins}</div>
      </div>

      <div class="clicker-container">
        <div class="energy-pill-box">
          <div class="energy-hdr">
            <span>⚡ Запас ударов</span>
            <span data-ce-val>${Math.floor(state.clickerEnergy)} / ${maxCE()}</span>
          </div>
          <div class="energy-progress-bar">
            <div class="energy-progress-fill" data-ce-fill style="width:${(state.clickerEnergy / maxCE()) * 100}%;"></div>
          </div>
          <div class="energy-sub">
            <span>+1 удар каждые ${state.settings.clickerRegenSec} сек</span>
            <span data-ce-timer>${state.clickerEnergy >= maxCE() ? 'Полный ✓' : 'Следующий через ' + secToNextCE() + 'с'}</span>
          </div>
        </div>

        <button class="punch-circle-btn" id="main-punch-btn">
          🥊
        </button>

        <div style="font-size:13px;font-weight:800;color:var(--text-main);margin-bottom:4px;">Тапай по боксёрской груше!</div>
        <div class="screen-sub">Каждый удар: +${tapXp} XP и +${tapCoins} 🪙${critChance ? ` · шанс крита ${critChance}% (×${state.settings.critMultiplier})` : ''}</div>

        <div style="margin-top:16px;width:100%;">
          ${quickItemsRow(['click_charge', 'click_full', 'legend_elixir'])}
        </div>
      </div>
    </div>`;
}

/* =========================================================================
   20. АДМИН-ПАНЕЛЬ С СОХРАНЕНИЕМ ИЗМЕНЕНИЙ (ПАРОЛЬ Halid2007.)
   ========================================================================= */
let adminDraft = null;
let adminDirty = false;
let adminTab = 'general';
let adminCloseArmed = false;

function makeAdminDraft() {
  return {
    settings: clone(state.settings),
    cases: clone(state.cases),
    workouts: clone(state.workouts),
    characters: clone(state.characters)
  };
}

function pathGet(obj, path) {
  return path.split('.').reduce((o, k) => (o == null ? o : o[k]), obj);
}
function pathSet(obj, path, val) {
  const ks = path.split('.');
  let o = obj;
  for (let i = 0; i < ks.length - 1; i++) o = o[ks[i]];
  o[ks[ks.length - 1]] = val;
}

function admField(label, path, o = {}) {
  const v = pathGet(adminDraft, path);
  const scale = o.scale || 1;
  const type = o.type || 'num';
  const shown = type === 'pass' ? '' : (typeof v === 'number' ? Math.round(v * scale * 1000) / 1000 : (v == null ? '' : v));
  const attrs = type === 'pass'
    ? 'type="password" placeholder="Новый пароль (от 4 символов)" autocomplete="new-password"'
    : `type="number" inputmode="decimal" step="${o.step || 1}"`;
  return `
    <div class="adm-field">
      <label>${esc(label)}</label>
      <input class="input-field sm" ${attrs} value="${esc(shown)}" data-dp="${path}" data-dt="${type}" data-scale="${scale}">
    </div>`;
}

function admSwitch(path) {
  const v = pathGet(adminDraft, path) !== false;
  return `<label class="switch"><input type="checkbox" data-dp="${path}" data-dt="bool" ${v ? 'checked' : ''}><span class="slider"></span></label>`;
}

function adminTabGeneral() {
  const th = adminDraft.settings.ambientTheme;
  return `
    <div class="admin-section-box">
      <div class="admin-sec-title">🎨 Анимация меню (внутри приложения)</div>
      <div class="screen-sub">Выберите частицы. Предпросмотр сразу, но запоминается после «Сохранить»:</div>
      <div class="season-selector-grid">
        <button class="season-opt-btn ${th === 'autumn' ? 'active' : ''}" data-set-theme="autumn"><span>🍁</span><span>Осень (Листопад)</span></button>
        <button class="season-opt-btn ${th === 'winter' ? 'active' : ''}" data-set-theme="winter"><span>❄️</span><span>Зима (Снегопад)</span></button>
        <button class="season-opt-btn ${th === 'spring' ? 'active' : ''}" data-set-theme="spring"><span>🌸</span><span>Весна (Сакура)</span></button>
        <button class="season-opt-btn ${th === 'summer' ? 'active' : ''}" data-set-theme="summer"><span>✨</span><span>Лето (Светлячки)</span></button>
      </div>
      <button class="btn-premium-ghost ${th === 'off' ? 'active' : ''}" data-set-theme="off" style="width:100%;margin-top:8px;font-size:11px;padding:8px;">🚫 Отключить анимацию</button>
    </div>

    <div class="admin-section-box">
      <div class="admin-sec-title">⚡ Энергия тренировок</div>
      <div class="adm-grid">
        ${admField('Максимум энергии', 'settings.maxWorkoutEnergy')}
        ${admField('Секунд на +1 энергии', 'settings.workoutRegenSec')}
        ${admField('Коэфф. затрат (ккал → энергия)', 'settings.energyCostFactor', { step: 0.05 })}
      </div>
    </div>

    <div class="admin-section-box">
      <div class="admin-sec-title">👊 Кликер</div>
      <div class="adm-grid">
        ${admField('Лимит ударов', 'settings.clickerMax')}
        ${admField('Секунд на +1 удар', 'settings.clickerRegenSec')}
        ${admField('XP за удар', 'settings.tapXp')}
        ${admField('Монет за удар', 'settings.tapCoins')}
        ${admField('Множитель крита', 'settings.critMultiplier', { step: 0.5 })}
      </div>
    </div>

    <div class="admin-section-box">
      <div class="admin-sec-title">🔑 Сменить секретный пароль</div>
      ${admField('Новый пароль', 'settings.adminPassword', { type: 'pass' })}
    </div>`;
}

function adminTabCases() {
  return adminDraft.cases.map((c, i) => `
    <div class="admin-section-box">
      <div class="adm-list-head">
        <span class="adm-name"><span>${c.icon}</span><span>${esc(c.name)}</span></span>
        ${admSwitch(`cases.${i}.active`)}
      </div>
      <div class="adm-grid">
        ${c.isDaily ? '' : admField('Цена (монеты)', `cases.${i}.price`)}
        ${admField('Монеты от', `cases.${i}.minCoins`)}
        ${admField('Монеты до', `cases.${i}.maxCoins`)}
        ${admField('XP от', `cases.${i}.minXp`)}
        ${admField('XP до', `cases.${i}.maxXp`)}
        ${admField('Шанс бойца, %', `cases.${i}.rareDropChance`, { scale: 100 })}
        ${admField('Попыток предмета', `cases.${i}.itemRolls`)}
        ${admField('Шанс предмета, %', `cases.${i}.itemChance`, { scale: 100 })}
      </div>
    </div>
  `).join('');
}

function adminTabWorkouts() {
  return `
    <div class="screen-sub" style="margin-bottom:10px;">Награды и расход калорий за минуту. Включайте и выключайте программы переключателем.</div>
    <div class="admin-section-box">
      ${adminDraft.workouts.map((w, i) => `
        <div class="adm-list-row">
          <div class="adm-list-head">
            <span class="adm-name"><span>${w.icon}</span><span>${esc(w.name)}</span></span>
            ${admSwitch(`workouts.${i}.active`)}
          </div>
          <div class="adm-grid-3">
            ${admField('XP/мин', `workouts.${i}.xpPerMin`)}
            ${admField('Монет/мин', `workouts.${i}.coinsPerMin`)}
            ${admField('Ккал/мин', `workouts.${i}.kcalPerMin`, { step: 0.1 })}
          </div>
        </div>
      `).join('')}
    </div>`;
}

function adminTabChars() {
  return `
    <div class="screen-sub" style="margin-bottom:10px;">Вес выпадения (чем больше, тем чаще) и уровень разблокировки бойцов.</div>
    <div class="admin-section-box">
      ${adminDraft.characters.map((c, i) => `
        <div class="adm-list-row">
          <div class="adm-list-head">
            <span class="adm-name"><span>${c.icon}</span><span>${esc(c.name)}</span></span>
            ${c.unlockType === 'case' ? admSwitch(`characters.${i}.active`) : `<span class="unlock-source-tag">🏆 за уровни</span>`}
          </div>
          <div class="adm-grid">
            ${c.unlockType === 'case'
              ? admField('Вес выпадения', `characters.${i}.dropWeight`, { step: 0.1 })
              : admField('Уровень разблокировки', `characters.${i}.unlockLevel`)}
          </div>
        </div>
      `).join('')}
    </div>`;
}

function adminTabInstant() {
  return `
    <div class="admin-section-box">
      <div class="admin-sec-title">💰 Мгновенные действия</div>
      <div class="screen-sub" style="margin-bottom:10px;">Эти кнопки срабатывают сразу, без «Сохранить».</div>
      <div style="display:flex;gap:8px;margin-bottom:10px;flex-wrap:wrap;">
        <button class="btn-chip" id="admin-add-1000-coins">+1000 🪙</button>
        <button class="btn-chip" id="admin-add-5000-coins">+5000 🪙</button>
        <button class="btn-chip" id="admin-add-level">+1 Уровень 🏆</button>
        <button class="btn-chip" id="admin-add-voucher">+1 Купон 🎟️</button>
        <button class="btn-chip" id="admin-fill-energy">Заправить энергию ⚡</button>
        <button class="btn-chip" id="admin-reset-daily">Сбросить кулдаун кейса 🎁</button>
        <button class="btn-chip" id="admin-give-items">+1 каждого предмета 🎒</button>
        <button class="btn-chip" id="admin-reset-road">Сбросить дорогу 🛣️</button>
      </div>
      <button class="btn-premium-violet" id="admin-unlock-all-chars" style="width:100%;font-size:12px;padding:9px;margin-bottom:8px;">
        🔓 Открыть всех ${state.characters.length} бойцов сразу
      </button>
    </div>`;
}

function adminBodyHtml() {
  if (adminTab === 'cases') return adminTabCases();
  if (adminTab === 'workouts') return adminTabWorkouts();
  if (adminTab === 'chars') return adminTabChars();
  if (adminTab === 'instant') return adminTabInstant();
  return adminTabGeneral();
}

function openAdminLoginModal() {
  Sound.click();
  const root = document.getElementById('modal-root');
  root.innerHTML = `
    <div class="modal-overlay active" id="admin-login-ov">
      <div class="modal-sheet">
        <div class="modal-bar-handle"></div>
        <h2 style="font-size:18px;font-weight:900;text-align:center;margin-bottom:6px;">🔑 Вход в Админ-панель</h2>
        <div class="screen-sub" style="text-align:center;margin-bottom:18px;">
          Введите секретный мастер-пароль доступа
        </div>
        <input type="password" class="input-field" id="admin-pass-input" placeholder="Секретный пароль">
        <div style="display:flex;gap:10px;">
          <button class="btn-premium-ghost" id="admin-login-cancel" style="flex:1;">Отмена</button>
          <button class="btn-premium-primary" id="admin-login-submit" style="flex:1.5;">Войти</button>
        </div>
      </div>
    </div>`;

  document.getElementById('admin-login-cancel').onclick = closeModal;
  document.getElementById('admin-login-ov').onclick = (e) => { if (e.target.id === 'admin-login-ov') closeModal(); };

  const submitBtn = document.getElementById('admin-login-submit');
  const doSubmit = () => {
    const entered = document.getElementById('admin-pass-input').value;
    const cleanEntered = entered.trim();
    if (cleanEntered === SECRET_ADMIN_PASSWORD || cleanEntered === 'Halid2007' || cleanEntered === state.settings.adminPassword) {
      Sound.fanfare();
      state.isAdmin = true;
      adminDraft = null;
      closeModal();
      showToast('Секретный доступ в панель администратора открыт!');
      openAdminDashboardModal();
    } else {
      Sound.punch();
      showToast('Неверный секретный пароль!');
    }
  };

  submitBtn.onclick = doSubmit;
  document.getElementById('admin-pass-input').onkeydown = (e) => { if (e.key === 'Enter') doSubmit(); };
}

function openAdminDashboardModal(tab) {
  Sound.click();
  if (!adminDraft) {
    adminDraft = makeAdminDraft();
    adminDirty = false;
  }
  if (tab) adminTab = tab;
  adminCloseArmed = false;
  renderAdminDashboard();
}

function markAdminDirty() {
  adminDirty = true;
  adminCloseArmed = false;
  const badge = document.getElementById('admin-dirty-badge');
  if (badge) {
    badge.textContent = 'Есть изменения';
    badge.className = 'admin-badge-tag dirty';
  }
  const saveBtn = document.getElementById('admin-save-btn');
  if (saveBtn) saveBtn.disabled = false;
  const cancelBtn = document.getElementById('admin-cancel-btn');
  if (cancelBtn) cancelBtn.disabled = false;
}

function renderAdminDashboard() {
  const root = document.getElementById('modal-root');
  const oldSheet = root.querySelector('.modal-sheet');
  const prevScroll = oldSheet ? oldSheet.scrollTop : 0;

  const tabs = [
    { id: 'general', label: '⚙️ Общее' },
    { id: 'cases', label: '🎁 Кейсы' },
    { id: 'workouts', label: '🏋️ Тренировки' },
    { id: 'chars', label: '🎖️ Бойцы' },
    { id: 'instant', label: '💰 Ресурсы' },
  ];

  root.innerHTML = `
    <div class="modal-overlay active" id="admin-dash-ov">
      <div class="modal-sheet">
        <div class="modal-bar-handle"></div>
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <h2 style="font-size:18px;font-weight:900;">🛡️ Панель Администратора</h2>
          <span id="admin-dirty-badge" class="admin-badge-tag ${adminDirty ? 'dirty' : 'clean'}">${adminDirty ? 'Есть изменения' : 'Сохранено'}</span>
        </div>

        <div class="admin-tabs">
          ${tabs.map(t => `<button class="btn-chip ${adminTab === t.id ? 'active' : ''}" data-admin-tab="${t.id}">${t.label}</button>`).join('')}
        </div>

        <div id="admin-body">${adminBodyHtml()}</div>

        <div class="admin-section-box" style="margin-top:4px;">
          <button class="btn-premium-ghost" id="admin-defaults-btn" style="width:100%;font-size:11.5px;padding:9px;">
            ↺ Вернуть заводские значения (в черновик)
          </button>
        </div>

        <div class="admin-savebar">
          <button class="btn-premium-ghost" id="admin-cancel-btn" style="flex:1;" ${adminDirty ? '' : 'disabled'}>Отменить</button>
          <button class="btn-premium-emerald" id="admin-save-btn" style="flex:2;" ${adminDirty ? '' : 'disabled'}>💾 Сохранить изменения</button>
        </div>
        <button class="btn-premium-ghost" id="admin-close-dash-btn" style="width:100%;margin-top:10px;">Закрыть панель</button>
      </div>
    </div>`;

  const sheet = root.querySelector('.modal-sheet');
  if (sheet) sheet.scrollTop = prevScroll;

  bindAdminEvents(root);
}

function revertAdminPreview() {
  AmbientEngine.setMode(state.settings.ambientTheme || 'autumn');
}

function requestCloseAdmin() {
  if (adminDirty && !adminCloseArmed) {
    adminCloseArmed = true;
    Sound.denied();
    showToast('Есть несохранённые изменения! Нажмите «Закрыть» ещё раз, чтобы выйти без сохранения');
    return;
  }
  if (adminDirty) {
    revertAdminPreview();
    adminDraft = null;
    adminDirty = false;
  }
  adminCloseArmed = false;
  closeModal();
  render();
}

function commitAdminDraft() {
  const d = adminDraft;
  const s = d.settings;

  // Проверка и нормализация значений
  const pass = String(s.adminPassword || '').trim();
  if (pass.length < 4) {
    Sound.denied();
    showToast('Пароль должен быть не менее 4 символов!');
    return false;
  }
  s.adminPassword = pass;
  s.maxWorkoutEnergy = Math.max(10, Math.round(s.maxWorkoutEnergy) || 100);
  s.workoutRegenSec = Math.max(1, Math.round(s.workoutRegenSec) || 20);
  s.energyCostFactor = Math.max(0.05, Number(s.energyCostFactor) || 0.5);
  s.clickerMax = Math.max(1, Math.round(s.clickerMax) || 200);
  s.clickerRegenSec = Math.max(1, Math.round(s.clickerRegenSec) || 3);
  s.tapXp = Math.max(0, Math.round(s.tapXp));
  s.tapCoins = Math.max(0, Math.round(s.tapCoins));
  s.critMultiplier = Math.max(1, Number(s.critMultiplier) || 3);

  d.cases.forEach(c => {
    c.price = Math.max(0, Math.round(c.price) || 0);
    c.minCoins = Math.max(0, Math.round(c.minCoins) || 0);
    c.maxCoins = Math.max(c.minCoins, Math.round(c.maxCoins) || 0);
    c.minXp = Math.max(0, Math.round(c.minXp) || 0);
    c.maxXp = Math.max(c.minXp, Math.round(c.maxXp) || 0);
    c.rareDropChance = Math.min(1, Math.max(0, Number(c.rareDropChance) || 0));
    c.itemRolls = Math.max(0, Math.round(c.itemRolls) || 0);
    c.itemChance = Math.min(1, Math.max(0, Number(c.itemChance) || 0));
  });
  d.workouts.forEach(w => {
    w.xpPerMin = Math.max(0, Number(w.xpPerMin) || 0);
    w.coinsPerMin = Math.max(0, Number(w.coinsPerMin) || 0);
    w.kcalPerMin = Math.max(0.5, Number(w.kcalPerMin) || 1);
  });
  d.characters.forEach(c => {
    if (c.unlockType === 'case') c.dropWeight = Math.max(0.1, Number(c.dropWeight) || 1);
    else c.unlockLevel = Math.max(1, Math.round(c.unlockLevel) || 1);
  });

  // Применение
  state.settings = Object.assign({}, state.settings, s);
  state.cases = clone(d.cases);
  state.workouts = clone(d.workouts);
  state.characters = clone(d.characters);

  state.workoutEnergy = Math.min(state.workoutEnergy, maxWE());
  state.clickerEnergy = Math.min(state.clickerEnergy, maxCE());
  AmbientEngine.setMode(state.settings.ambientTheme || 'autumn');
  checkLevelUnlocks(calcLevel(state.player.xp).level);
  persist();

  adminDraft = makeAdminDraft();
  adminDirty = false;
  adminCloseArmed = false;
  return true;
}

function bindAdminEvents(root) {
  root.querySelectorAll('[data-admin-tab]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      adminTab = btn.dataset.adminTab;
      const sheet = root.querySelector('.modal-sheet');
      renderAdminDashboard();
      const s2 = document.querySelector('#modal-root .modal-sheet');
      if (s2) s2.scrollTop = 0;
    };
  });

  // Любое поле формы пишет в черновик
  root.querySelectorAll('[data-dp]').forEach(inp => {
    const handler = () => {
      const path = inp.dataset.dp;
      const dt = inp.dataset.dt;
      if (dt === 'bool') {
        pathSet(adminDraft, path, inp.checked);
      } else if (dt === 'pass') {
        const val = inp.value.trim();
        pathSet(adminDraft, path, val.length ? val : state.settings.adminPassword);
      } else {
        const n = parseFloat(inp.value);
        if (isNaN(n)) return;
        pathSet(adminDraft, path, n / (parseFloat(inp.dataset.scale) || 1));
      }
      markAdminDirty();
    };
    inp.addEventListener(inp.type === 'checkbox' ? 'change' : 'input', handler);
  });

  root.querySelectorAll('[data-set-theme]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      const theme = btn.dataset.setTheme;
      adminDraft.settings.ambientTheme = theme;
      AmbientEngine.setMode(theme);   // живой предпросмотр
      adminDirty = true;
      renderAdminDashboard();
    };
  });

  document.getElementById('admin-save-btn').onclick = () => {
    if (commitAdminDraft()) {
      Sound.fanfare();
      showToast('💾 Изменения сохранены и применены!');
      renderAdminDashboard();
      render();
    }
  };

  document.getElementById('admin-cancel-btn').onclick = () => {
    Sound.click();
    revertAdminPreview();
    adminDraft = makeAdminDraft();
    adminDirty = false;
    showToast('Изменения отменены');
    renderAdminDashboard();
  };

  document.getElementById('admin-defaults-btn').onclick = () => {
    Sound.click();
    adminDraft.settings = Object.assign({}, clone(DEFAULT_SETTINGS), { adminPassword: state.settings.adminPassword });
    adminDraft.cases = clone(initialCases);
    adminDraft.workouts = clone(initialWorkouts);
    adminDraft.characters = clone(initialCharacters);
    AmbientEngine.setMode(adminDraft.settings.ambientTheme);
    adminDirty = true;
    showToast('Заводские значения загружены в черновик — нажмите «Сохранить»');
    renderAdminDashboard();
  };

  document.getElementById('admin-close-dash-btn').onclick = requestCloseAdmin;
  document.getElementById('admin-dash-ov').onclick = (e) => { if (e.target.id === 'admin-dash-ov') requestCloseAdmin(); };

  // Мгновенные действия
  const bind = (id, fn) => { const el = document.getElementById(id); if (el) el.onclick = fn; };

  bind('admin-add-1000-coins', () => {
    Sound.fanfare(); state.player.coins += 1000; persist(); updateCoinsUI(); showToast('+1000 🪙 добавлено!');
  });
  bind('admin-add-5000-coins', () => {
    Sound.fanfare(); state.player.coins += 5000; persist(); updateCoinsUI(); showToast('+5000 🪙 добавлено!');
  });
  bind('admin-add-level', () => {
    Sound.fanfare();
    const cur = calcLevel(state.player.xp);
    addPlayerXp(cur.xpForNextLevel - cur.xpIntoLevel);
    render();
  });
  bind('admin-add-voucher', () => {
    Sound.fanfare(); addItem('voucher', 1); persist(); showToast('+1 🎟️ Купон на кейс добавлен в сумку!');
  });
  bind('admin-fill-energy', () => {
    Sound.fanfare();
    state.workoutEnergy = maxWE();
    state.clickerEnergy = maxCE();
    persist(); updateEnergyUI(); showToast('⚡ Энергия и кликер заполнены!');
  });
  bind('admin-reset-daily', () => {
    Sound.fanfare(); state.lastDailyCaseTimestamp = 0; persist(); showToast('Ежедневный кейс снова доступен!');
  });
  bind('admin-give-items', () => {
    Sound.fanfare();
    Object.keys(ITEM_CATALOG).forEach(id => addItem(id, 1));
    persist(); showToast('🎒 В сумку добавлено по 1 каждому предмету!');
  });
  bind('admin-reset-road', () => {
    Sound.fanfare(); state.roadClaimed = []; persist(); render(); showToast('Награды дороги можно забрать заново');
  });
  bind('admin-unlock-all-chars', () => {
    Sound.fanfare();
    state.unlockedCharacters = state.characters.map(c => c.id);
    persist(); render();
    showToast(`Все ${state.characters.length} персонажей разблокированы!`);
  });
}

/* =========================================================================
   21. ГЛАВНЫЙ ЦИКЛ РЕНДЕРА И ПРИВЯЗКА СОБЫТИЙ
   ========================================================================= */
function render(direction = null) {
  const viewEl = document.getElementById('view');
  let html = '';

  if (state.currentTab === 'home') html = renderHome();
  else if (state.currentTab === 'workouts') html = renderWorkouts();
  else if (state.currentTab === 'cases') html = renderCases();
  else if (state.currentTab === 'characters') html = renderCharacters();
  else if (state.currentTab === 'road') html = renderRoad();
  else if (state.currentTab === 'profile') html = renderProfile();
  else if (state.currentTab === 'clicker') html = renderClickerTab();

  viewEl.innerHTML = html;

  viewEl.className = '';
  if (direction === 'right') viewEl.classList.add('slide-in-right');
  else if (direction === 'left') viewEl.classList.add('slide-in-left');
  else viewEl.classList.add('fade-in');

  renderNav();
  bindViewEvents(viewEl);
}

function bindViewEvents(root) {
  const byId = (id, fn) => { const el = document.getElementById(id); if (el) el.onclick = fn; };

  byId('home-to-cases', () => switchTab('cases'));
  byId('home-to-clicker', () => { Sound.click(); state.currentTab = 'clicker'; render(); });
  byId('home-to-builder', openBuilderModal);
  byId('home-to-bag', () => openInventoryModal('items'));
  byId('home-to-road', () => switchTab('road'));
  byId('btn-open-constructor', openBuilderModal);
  byId('banner-constructor-btn', openBuilderModal);
  byId('home-all-workouts-link', () => switchTab('workouts'));
  byId('workouts-open-bag', () => openInventoryModal('items'));
  byId('cases-open-bag', () => openInventoryModal('items'));
  byId('cases-open-trophies', () => openInventoryModal('trophies'));
  byId('profile-open-bag', () => openInventoryModal('items'));
  byId('road-claim-all', claimAllRoadRewards);

  // Фильтр персонажей
  root.querySelectorAll('[data-char-filter]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      state.charSubTab = btn.dataset.charFilter;
      render();
    };
  });

  // Категории тренировок
  root.querySelectorAll('[data-w-cat]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      state.workoutCat = btn.dataset.wCat;
      render();
    };
  });

  // Быстрое применение предметов
  root.querySelectorAll('[data-quick-use]').forEach(btn => {
    btn.onclick = () => {
      const ok = useItem(btn.dataset.quickUse);
      if (ok && !document.getElementById('levelup-modal-ov')) render();
    };
  });

  // Награды дороги
  root.querySelectorAll('[data-road-claim]').forEach(btn => {
    btn.onclick = () => claimRoadReward(parseInt(btn.dataset.roadClaim, 10));
  });

  // Квесты
  root.querySelectorAll('[data-claim-quest]').forEach(btn => {
    btn.onclick = () => {
      Sound.fanfare();
      claimQuest(btn.dataset.claimQuest);
      if (!document.getElementById('levelup-modal-ov')) render();
    };
  });

  // Запуск тренировок
  root.querySelectorAll('[data-start-w]').forEach(btn => {
    btn.onclick = () => {
      const w = state.workouts.find(x => x.id === btn.dataset.startW);
      if (w) startStandardWorkout(w);
    };
  });

  // Открытие кейсов
  root.querySelectorAll('[data-open-case]').forEach(btn => {
    btn.onclick = () => {
      const c = state.cases.find(x => x.id === btn.dataset.openCase);
      if (c) openCaseWithRoulette(c, btn.dataset.pay);
    };
  });

  // Выбор бойца
  root.querySelectorAll('[data-select-char]').forEach(btn => {
    btn.onclick = () => {
      Sound.click();
      state.activeCharacterId = btn.dataset.selectChar;
      persist();
      render();
      const ch = state.characters.find(x => x.id === btn.dataset.selectChar);
      showToast(`${ch.name} теперь ваш наставник! ${ch.perk}`);
    };
  });

  // Авто-тренировки
  root.querySelectorAll('[data-train-char]').forEach(btn => {
    btn.onclick = () => sendCharToTraining(btn.dataset.trainChar);
  });
  root.querySelectorAll('[data-collect-char]').forEach(btn => {
    btn.onclick = () => collectCharTrainingReward(btn.dataset.collectChar);
  });

  // Кликер
  const punchBtn = document.getElementById('main-punch-btn');
  if (punchBtn) punchBtn.onclick = (e) => tapPunch(e);
  byId('clicker-back-btn', () => switchTab('home'));

  // Админ-панель
  byId('open-admin-login-btn', openAdminLoginModal);
  byId('open-admin-dashboard-btn', () => openAdminDashboardModal());
  byId('admin-logout-btn', () => {
    Sound.click();
    state.isAdmin = false;
    adminDraft = null;
    adminDirty = false;
    render();
    showToast('Вы вышли из режима администратора');
  });
}

/* =========================================================================
   22. ИНИЦИАЛИЗАЦИЯ ПРИЛОЖЕНИЯ
   ========================================================================= */
window.addEventListener('DOMContentLoaded', () => {
  AmbientEngine.init();
  AmbientEngine.setMode(state.settings.ambientTheme || 'autumn');
  setupSwipeGestures();
  resetQuestsIfNeeded();
  regenTick();
  checkLevelUnlocks(calcLevel(state.player.xp).level);
  persist();
  render();

  // При возвращении во вкладку — догоняем энергию и обновляем квесты
  document.addEventListener('visibilitychange', () => {
    if (!document.hidden) {
      regenTick();
      resetQuestsIfNeeded();
      updateEnergyUI();
    }
  });
});
</script>
</body>
</html>
