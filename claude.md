<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Airship Stakes</title>
  <style>
    :root {
      --bg1: #08111f;
      --bg2: #10243f;
      --panel: rgba(6, 15, 28, 0.86);
      --panel-border: rgba(255, 255, 255, 0.12);
      --text: #eef6ff;
      --muted: #9bb2c8;
      --accent: #f4b860;
      --accent-2: #7cd4ff;
      --danger: #ff7070;
      --success: #79e39d;
      --shadow: 0 18px 50px rgba(0, 0, 0, 0.4);
    }

    * {
      box-sizing: border-box;
    }

    html,
    body {
      margin: 0;
      min-height: 100%;
      font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at 20% 20%, rgba(124, 212, 255, 0.12), transparent 30%),
        radial-gradient(circle at 80% 30%, rgba(244, 184, 96, 0.14), transparent 26%),
        linear-gradient(180deg, var(--bg2), var(--bg1));
    }

    body {
      display: flex;
      justify-content: center;
      padding: 20px;
    }

    .app {
      width: min(1200px, 100%);
      display: grid;
      grid-template-columns: 320px 1fr;
      gap: 18px;
      align-items: start;
    }

    .panel,
    .stage-wrap {
      background: var(--panel);
      border: 1px solid var(--panel-border);
      border-radius: 20px;
      box-shadow: var(--shadow);
      backdrop-filter: blur(12px);
    }

    .panel {
      padding: 18px;
      position: sticky;
      top: 20px;
    }

    h1 {
      margin: 0 0 6px;
      font-size: 28px;
      letter-spacing: 0.02em;
    }

    .subtitle {
      margin: 0 0 20px;
      color: var(--muted);
      font-size: 14px;
      line-height: 1.45;
    }

    .hud {
      display: grid;
      gap: 10px;
      margin-bottom: 18px;
    }

    .stat {
      padding: 12px 14px;
      border-radius: 14px;
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .stat-label {
      font-size: 12px;
      color: var(--muted);
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    .stat-value {
      margin-top: 4px;
      font-size: 24px;
      font-weight: 800;
    }

    .controls {
      display: grid;
      gap: 14px;
    }

    .group {
      display: grid;
      gap: 10px;
    }

    .group label,
    .group-title {
      font-size: 13px;
      font-weight: 700;
      color: var(--muted);
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    .bet-row {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 10px;
    }

    input[type='number'] {
      width: 100%;
      background: rgba(0, 0, 0, 0.24);
      color: var(--text);
      border: 1px solid rgba(255, 255, 255, 0.12);
      border-radius: 12px;
      padding: 12px 14px;
      font-size: 16px;
      outline: none;
    }

    input[type='number']:focus {
      border-color: var(--accent-2);
      box-shadow: 0 0 0 3px rgba(124, 212, 255, 0.15);
    }

    .quick-bets,
    .ship-list,
    .actions {
      display: grid;
      gap: 8px;
    }

    .quick-bets {
      grid-template-columns: repeat(4, 1fr);
    }

    .ship-list {
      grid-template-columns: 1fr;
    }

    button {
      border: 0;
      border-radius: 12px;
      padding: 12px 14px;
      font-size: 15px;
      font-weight: 700;
      color: var(--text);
      background: rgba(255, 255, 255, 0.07);
      border: 1px solid rgba(255, 255, 255, 0.08);
      cursor: pointer;
      transition: transform 0.12s ease, background 0.12s ease, border-color 0.12s ease;
    }

    button:hover:enabled {
      transform: translateY(-1px);
      background: rgba(255, 255, 255, 0.11);
    }

    button:disabled {
      cursor: not-allowed;
      opacity: 0.45;
    }

    .ship-btn {
      display: grid;
      grid-template-columns: 1fr auto;
      align-items: center;
      text-align: left;
      gap: 8px;
      min-height: 56px;
    }

    .ship-btn.selected {
      border-color: var(--accent);
      background: rgba(244, 184, 96, 0.12);
      box-shadow: inset 0 0 0 1px rgba(244, 184, 96, 0.22);
    }

    .ship-meta {
      display: flex;
      align-items: center;
      gap: 10px;
      min-width: 0;
    }

    .swatch {
      width: 16px;
      height: 16px;
      border-radius: 999px;
      box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.18);
      flex: 0 0 auto;
    }

    .ship-name {
      font-weight: 800;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .odds {
      color: var(--accent);
      font-weight: 800;
      font-size: 14px;
    }

    .actions {
      grid-template-columns: 1fr 1fr;
      margin-top: 6px;
    }

    .primary {
      background: linear-gradient(180deg, #efb866, #dc9e41);
      color: #241506;
      border-color: rgba(255, 255, 255, 0.15);
    }

    .secondary {
      background: linear-gradient(180deg, #88bcd5, #5b9fbf);
      color: #07141d;
      border-color: rgba(255, 255, 255, 0.15);
    }

    .message {
      margin-top: 14px;
      min-height: 52px;
      padding: 12px 14px;
      border-radius: 14px;
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(255, 255, 255, 0.08);
      color: var(--muted);
      line-height: 1.4;
      font-size: 14px;
    }

    .message.win {
      color: var(--success);
      border-color: rgba(121, 227, 157, 0.25);
      background: rgba(121, 227, 157, 0.08);
    }

    .message.lose {
      color: #ffb2b2;
      border-color: rgba(255, 112, 112, 0.25);
      background: rgba(255, 112, 112, 0.08);
    }

    .message.info {
      color: var(--accent-2);
      border-color: rgba(124, 212, 255, 0.25);
      background: rgba(124, 212, 255, 0.08);
    }

    .stage-wrap {
      padding: 14px;
      position: relative;
      overflow: hidden;
    }

    .stage-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      margin-bottom: 10px;
      padding: 6px 6px 0;
    }

    .race-title {
      font-size: 18px;
      font-weight: 800;
    }

    .phase {
      color: var(--muted);
      font-size: 13px;
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    canvas {
      width: 100%;
      height: auto;
      display: block;
      border-radius: 16px;
      background: linear-gradient(180deg, #1f4168, #0b1220 72%);
    }

    .legend {
      display: flex;
      flex-wrap: wrap;
      gap: 10px 14px;
      margin-top: 12px;
      padding: 0 6px 4px;
      color: var(--muted);
      font-size: 13px;
    }

    .legend-item {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .overlay-note {
      position: absolute;
      right: 24px;
      top: 62px;
      background: rgba(6, 15, 28, 0.8);
      border: 1px solid rgba(255, 255, 255, 0.1);
      padding: 10px 12px;
      border-radius: 12px;
      color: var(--muted);
      font-size: 13px;
      pointer-events: none;
    }

    @media (max-width: 900px) {
      .app {
        grid-template-columns: 1fr;
      }

      .panel {
        position: static;
      }
    }

    @media (max-width: 520px) {
      body {
        padding: 12px;
      }

      .quick-bets,
      .actions {
        grid-template-columns: 1fr 1fr;
      }

      .bet-row {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <aside class="panel">
      <h1>Airship Stakes</h1>
      <p class="subtitle">Bet fictional credits on a sky race. Pick a ship, set your wager, and try not to go broke.</p>

      <div class="hud">
        <div class="stat">
          <div class="stat-label">Bankroll</div>
          <div id="bankroll" class="stat-value">100 cr</div>
        </div>
        <div class="stat">
          <div class="stat-label">Current Bet</div>
          <div id="currentBet" class="stat-value">0 cr</div>
        </div>
      </div>

      <div class="controls">
        <div class="group">
          <label for="betInput">Bet amount</label>
          <div class="bet-row">
            <input id="betInput" type="number" min="1" step="1" value="10" />
            <button id="maxBetBtn" type="button">Max</button>
          </div>
          <div class="quick-bets">
            <button type="button" data-bet="5">+5</button>
            <button type="button" data-bet="10">+10</button>
            <button type="button" data-bet="25">+25</button>
            <button type="button" data-bet="50">+50</button>
          </div>
        </div>

        <div class="group">
          <div class="group-title">Choose your airship</div>
          <div id="shipList" class="ship-list"></div>
        </div>

        <div class="actions">
          <button id="startBtn" class="primary" type="button">Start race</button>
          <button id="restartBtn" class="secondary" type="button">Restart</button>
        </div>
      </div>

      <div id="message" class="message info">Choose a ship and place a bet to begin.</div>
    </aside>

    <section class="stage-wrap">
      <div class="stage-header">
        <div class="race-title">Grand Circuit of the Sky Docks</div>
        <div id="phaseLabel" class="phase">Waiting for bets</div>
      </div>
      <canvas id="raceCanvas" width="860" height="540"></canvas>
      <div class="overlay-note">Fictional credits only. Losing all credits ends the run.</div>
      <div id="legend" class="legend"></div>
    </section>
  </div>

  <script>
    (() => {
      const canvas = document.getElementById('raceCanvas');
      const ctx = canvas.getContext('2d');

      const bankrollEl = document.getElementById('bankroll');
      const currentBetEl = document.getElementById('currentBet');
      const messageEl = document.getElementById('message');
      const phaseLabelEl = document.getElementById('phaseLabel');
      const shipListEl = document.getElementById('shipList');
      const legendEl = document.getElementById('legend');
      const betInputEl = document.getElementById('betInput');
      const startBtn = document.getElementById('startBtn');
      const restartBtn = document.getElementById('restartBtn');
      const maxBetBtn = document.getElementById('maxBetBtn');

      const W = canvas.width;
      const H = canvas.height;
      const LANES = 5;
      const SHIP_W = 82;
      const START_X = 44;
      const FINISH_X = W - 134;
      const COUNTDOWN_TIME = 2.2;
      const RESULT_TIME = 3.2;
      const CLOUD_COUNT = 7;
      const MAX_PARTICLES = 240;
      const MAX_SHIPS = LANES;

      const shipPalette = ['#ff8a7a', '#f3c867', '#7ce7c7', '#88b4ff', '#de8cff'];
      const shipNames = ['Crimson Gale', 'Sunflare', 'Mistral Bloom', 'Blue Ember', 'Violet Wake'];

      const state = {
        bankroll: 100,
        currentBet: 10,
        selectedShip: 0,
        phase: 'betting',
        winnerIndex: -1,
        countdown: 0,
        resultTimer: 0,
        message: 'Choose a ship and place a bet to begin.',
        messageClass: 'info',
        raceId: 0,
        flashes: 0,
        shipCount: MAX_SHIPS,
        clouds: new Array(CLOUD_COUNT),
        ships: new Array(MAX_SHIPS),
        particles: new Array(MAX_PARTICLES),
        raceProgress: 0
      };

      for (let i = 0; i < CLOUD_COUNT; i++) {
        state.clouds[i] = {
          x: (W / CLOUD_COUNT) * i + Math.random() * 50,
          y: 50 + i * 42 + Math.random() * 10,
          w: 90 + Math.random() * 80,
          h: 24 + Math.random() * 18,
          speed: 10 + Math.random() * 12
        };
      }

      for (let i = 0; i < MAX_SHIPS; i++) {
        state.ships[i] = createShip(i);
      }

      for (let i = 0; i < MAX_PARTICLES; i++) {
        state.particles[i] = {
          active: 0,
          x: 0,
          y: 0,
          vx: 0,
          vy: 0,
          life: 0,
          maxLife: 1,
          size: 0,
          hue: 0
        };
      }

      function createShip(i) {
        return {
          index: i,
          name: shipNames[i],
          color: shipPalette[i],
          odds: 2,
          laneY: laneCenterY(i),
          x: START_X,
          speed: 0,
          bobPhase: Math.random() * Math.PI * 2,
          bobOffset: 0,
          engineBias: 0,
          burst: 0,
          finished: 0,
          finishRank: 0,
          finishTime: 0,
          strength: 1,
          propellerSpin: 0,
          trailTick: 0
        };
      }

      function laneCenterY(lane) {
        const top = 92;
        const bottom = H - 90;
        const spacing = (bottom - top) / (LANES - 1);
        return top + spacing * lane;
      }

      function clamp(v, min, max) {
        return v < min ? min : v > max ? max : v;
      }

      function setMessage(text, cls) {
        state.message = text;
        state.messageClass = cls || 'info';
        messageEl.textContent = text;
        messageEl.className = 'message ' + state.messageClass;
      }

      function credits(v) {
        return Math.max(0, Math.floor(v)) + ' cr';
      }

      function syncHud() {
        bankrollEl.textContent = credits(state.bankroll);
        currentBetEl.textContent = credits(state.currentBet);
        phaseLabelEl.textContent = state.phase === 'betting' ? 'Waiting for bets' :
          state.phase === 'countdown' ? 'Race starts in ' + Math.ceil(state.countdown) :
          state.phase === 'racing' ? 'Race in progress' :
          state.phase === 'result' ? 'Results posted' : 'Game over';
        messageEl.textContent = state.message;
        messageEl.className = 'message ' + state.messageClass;
        betInputEl.value = String(state.currentBet);

        const locked = state.phase !== 'betting';
        betInputEl.disabled = locked || state.phase === 'gameover';
        maxBetBtn.disabled = locked || state.phase === 'gameover';
        startBtn.disabled = locked || state.phase === 'gameover';
        shipListEl.querySelectorAll('button').forEach((btn, i) => {
          btn.disabled = locked || state.phase === 'gameover';
          btn.classList.toggle('selected', i === state.selectedShip);
        });
      }

      function renderShipButtons() {
        shipListEl.innerHTML = '';
        legendEl.innerHTML = '';

        for (let i = 0; i < state.shipCount; i++) {
          const ship = state.ships[i];
          const btn = document.createElement('button');
          btn.type = 'button';
          btn.className = 'ship-btn' + (i === state.selectedShip ? ' selected' : '');
          btn.innerHTML = '<span class="ship-meta"><span class="swatch" style="background:' + ship.color + '"></span><span class="ship-name">' + ship.name + '</span></span><span class="odds">' + ship.odds.toFixed(1) + 'x</span>';
          btn.addEventListener('click', () => {
            if (state.phase !== 'betting') return;
            state.selectedShip = i;
            syncHud();
          });
          shipListEl.appendChild(btn);

          const legend = document.createElement('div');
          legend.className = 'legend-item';
          legend.innerHTML = '<span class="swatch" style="background:' + ship.color + '"></span><span>' + ship.name + ' — ' + ship.odds.toFixed(1) + 'x payout</span>';
          legendEl.appendChild(legend);
        }
      }

      function generateRace() {
        const base = [0.92, 1.02, 1.08, 1.16, 1.28];
        for (let i = base.length - 1; i > 0; i--) {
          const j = (Math.random() * (i + 1)) | 0;
          const t = base[i];
          base[i] = base[j];
          base[j] = t;
        }

        for (let i = 0; i < state.shipCount; i++) {
          const ship = state.ships[i];
          ship.laneY = laneCenterY(i);
          ship.x = START_X;
          ship.speed = 0;
          ship.finished = 0;
          ship.finishRank = 0;
          ship.finishTime = 0;
          ship.engineBias = base[i];
          ship.strength = base[i];
          ship.burst = 0;
          ship.bobPhase = Math.random() * Math.PI * 2;
          ship.propellerSpin = 0;
          ship.trailTick = 0;
          ship.odds = clamp(Math.round((3.85 - ship.strength * 1.45) * 10) / 10, 1.8, 3.8);
        }
        state.winnerIndex = -1;
        state.raceProgress = 0;
        state.raceId++;
        renderShipButtons();
      }

      function startRace() {
        if (state.phase !== 'betting') return;
        const bet = clamp((betInputEl.valueAsNumber || 0) | 0, 0, state.bankroll);
        if (bet < 1) {
          setMessage('Enter a bet of at least 1 credit.', 'lose');
          syncHud();
          return;
        }
        if (state.selectedShip < 0 || state.selectedShip >= state.shipCount) {
          setMessage('Choose an airship first.', 'lose');
          syncHud();
          return;
        }

        state.currentBet = bet;
        state.bankroll -= bet;
        state.phase = 'countdown';
        state.countdown = COUNTDOWN_TIME;
        state.resultTimer = 0;
        state.winnerIndex = -1;
        state.flashes = 0.8;
        setMessage('Engines primed. Race begins in a moment.', 'info');
        syncHud();
      }

      function settleRace() {
        const winner = state.ships[state.winnerIndex];
        if (state.selectedShip === state.winnerIndex) {
          const payout = Math.round(state.currentBet * winner.odds);
          state.bankroll += payout;
          setMessage('You backed ' + winner.name + ' and won ' + credits(payout) + '.', 'win');
        } else {
          setMessage('You lost ' + credits(state.currentBet) + '. ' + winner.name + ' took the race.', 'lose');
        }

        if (state.bankroll <= 0) {
          state.bankroll = 0;
          state.phase = 'gameover';
          setMessage('You are out of credits. Game over.', 'lose');
        } else {
          state.phase = 'result';
          state.resultTimer = RESULT_TIME;
        }
        syncHud();
      }

      function resetGame() {
        state.bankroll = 100;
        state.currentBet = 10;
        state.selectedShip = 0;
        state.phase = 'betting';
        state.countdown = 0;
        state.resultTimer = 0;
        state.flashes = 0;
        generateRace();
        setMessage('Fresh bankroll loaded. Place your next bet.', 'info');
        syncHud();
      }

      function spawnParticle(x, y, vx, vy, size, life, hue) {
        for (let i = 0; i < MAX_PARTICLES; i++) {
          const p = state.particles[i];
          if (!p.active) {
            p.active = 1;
            p.x = x;
            p.y = y;
            p.vx = vx;
            p.vy = vy;
            p.size = size;
            p.life = life;
            p.maxLife = life;
            p.hue = hue;
            return;
          }
        }
      }

      function updateParticles(dt) {
        for (let i = 0; i < MAX_PARTICLES; i++) {
          const p = state.particles[i];
          if (!p.active) continue;
          p.life -= dt;
          if (p.life <= 0) {
            p.active = 0;
            continue;
          }
          p.x += p.vx * dt;
          p.y += p.vy * dt;
          p.vx *= 0.992;
          p.vy -= 1.2 * dt;
          p.size += dt * 3.4;
        }
      }

      function updateClouds(dt) {
        for (let i = 0; i < CLOUD_COUNT; i++) {
          const c = state.clouds[i];
          c.x += c.speed * dt;
          if (c.x - c.w > W + 40) {
            c.x = -c.w - Math.random() * 80;
          }
        }
      }

      function updateBetting(dt) {
        state.flashes = Math.max(0, state.flashes - dt);
      }

      function updateCountdown(dt) {
        state.countdown -= dt;
        state.flashes = Math.max(0, state.flashes - dt * 1.2);
        if (state.countdown <= 0) {
          state.phase = 'racing';
          state.countdown = 0;
          setMessage('The race is on.', 'info');
          syncHud();
        }
      }

      function updateRacing(dt) {
        let finishers = 0;
        let bestX = START_X;
        for (let i = 0; i < state.shipCount; i++) {
          const ship = state.ships[i];
          ship.bobPhase += dt * (2.6 + ship.strength * 0.2);
          ship.bobOffset = Math.sin(ship.bobPhase) * 4;
          ship.propellerSpin += dt * (18 + ship.speed * 0.06);

          if (ship.finished) {
            finishers++;
            if (ship.x > bestX) bestX = ship.x;
            continue;
          }

          ship.burst -= dt;
          if (ship.burst <= 0) {
            ship.burst = 0.35 + Math.random() * 0.9;
            ship.speed += 10 + Math.random() * 34 * ship.strength;
          }

          ship.speed += (32 * ship.strength + Math.random() * 16 - 6) * dt;
          ship.speed *= 0.988;
          ship.speed = clamp(ship.speed, 58, 230 + ship.strength * 18);
          ship.x += ship.speed * dt;
          ship.trailTick += ship.speed * dt;

          while (ship.trailTick > 18) {
            ship.trailTick -= 18;
            spawnParticle(
              ship.x - 16,
              ship.laneY + 8 + ship.bobOffset,
              -18 - Math.random() * 24,
              -4 + Math.random() * 8,
              4 + Math.random() * 4,
              0.45 + Math.random() * 0.35,
              i
            );
          }

          if (ship.x >= FINISH_X) {
            ship.x = FINISH_X;
            ship.finished = 1;
            ship.finishTime = performance.now();
            finishers++;
            if (state.winnerIndex === -1) {
              state.winnerIndex = i;
              state.flashes = 0.9;
              settleRace();
              return;
            }
          }
          if (ship.x > bestX) bestX = ship.x;
        }
        state.raceProgress = (bestX - START_X) / (FINISH_X - START_X);
      }

      function updateResult(dt) {
        state.resultTimer -= dt;
        state.flashes = Math.max(0, state.flashes - dt);
        if (state.resultTimer <= 0) {
          state.phase = 'betting';
          generateRace();
          setMessage('New race posted. Place your bet.', 'info');
          syncHud();
        }
      }

      function updateGameOver(dt) {
        state.flashes = Math.max(0, state.flashes - dt * 0.6);
      }

      function update(dt) {
        updateClouds(dt);
        updateParticles(dt);

        if (state.phase === 'betting') updateBetting(dt);
        else if (state.phase === 'countdown') updateCountdown(dt);
        else if (state.phase === 'racing') updateRacing(dt);
        else if (state.phase === 'result') updateResult(dt);
        else updateGameOver(dt);
      }

      function drawBackground() {
        const g = ctx.createLinearGradient(0, 0, 0, H);
        g.addColorStop(0, '#2d5d91');
        g.addColorStop(0.55, '#193355');
        g.addColorStop(1, '#09111d');
        ctx.fillStyle = g;
        ctx.fillRect(0, 0, W, H);

        ctx.fillStyle = 'rgba(255,255,255,0.08)';
        for (let i = 0; i < CLOUD_COUNT; i++) {
          const c = state.clouds[i];
          drawCloud(c.x, c.y, c.w, c.h);
        }

        for (let i = 0; i < LANES; i++) {
          const y = laneCenterY(i);
          ctx.strokeStyle = 'rgba(255,255,255,0.08)';
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(24, y + 26);
          ctx.lineTo(W - 24, y + 26);
          ctx.stroke();
        }

        ctx.fillStyle = 'rgba(255,255,255,0.07)';
        ctx.fillRect(FINISH_X + 46, 54, 10, H - 108);
        for (let i = 0; i < 12; i++) {
          ctx.fillStyle = i % 2 === 0 ? 'rgba(255,255,255,0.85)' : 'rgba(20,20,25,0.85)';
          ctx.fillRect(FINISH_X + 56, 70 + i * 32, 26, 16);
        }

        ctx.fillStyle = 'rgba(255,255,255,0.2)';
        ctx.fillRect(18, H - 64, W - 36, 18);
      }

      function drawCloud(x, y, w, h) {
        ctx.beginPath();
        ctx.ellipse(x, y, w * 0.24, h * 0.55, 0, 0, Math.PI * 2);
        ctx.ellipse(x + w * 0.18, y - h * 0.24, w * 0.22, h * 0.62, 0, 0, Math.PI * 2);
        ctx.ellipse(x + w * 0.38, y, w * 0.28, h * 0.7, 0, 0, Math.PI * 2);
        ctx.ellipse(x + w * 0.62, y + h * 0.04, w * 0.24, h * 0.52, 0, 0, Math.PI * 2);
        ctx.fill();
      }

      function drawParticles() {
        for (let i = 0; i < MAX_PARTICLES; i++) {
          const p = state.particles[i];
          if (!p.active) continue;
          const alpha = p.life / p.maxLife;
          ctx.fillStyle = 'rgba(220, 230, 245, ' + (alpha * 0.35) + ')';
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
          ctx.fill();
        }
      }

      function drawShip(ship, isWinner) {
        const x = ship.x;
        const y = ship.laneY + ship.bobOffset;

        ctx.save();
        ctx.translate(x, y);

        if (isWinner && (state.phase === 'result' || state.phase === 'gameover')) {
          ctx.shadowColor = ship.color;
          ctx.shadowBlur = 18;
        }

        ctx.fillStyle = ship.color;
        ctx.beginPath();
        ctx.ellipse(0, -16, 32, 22, 0, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = 'rgba(255,255,255,0.28)';
        ctx.beginPath();
        ctx.ellipse(-8, -21, 14, 7, -0.2, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = '#3b2b20';
        ctx.fillRect(-18, 5, 26, 12);
        ctx.fillRect(10, 0, 12, 5);

        ctx.fillStyle = '#d5b58a';
        ctx.fillRect(-14, 7, 18, 8);

        ctx.fillStyle = '#1d2734';
        ctx.beginPath();
        ctx.moveTo(24, -12);
        ctx.lineTo(40, -4);
        ctx.lineTo(24, 2);
        ctx.closePath();
        ctx.fill();

        ctx.fillStyle = '#adbfd9';
        ctx.beginPath();
        ctx.moveTo(-10, -6);
        ctx.lineTo(-24, 10);
        ctx.lineTo(-6, 6);
        ctx.closePath();
        ctx.fill();

        ctx.strokeStyle = '#e4edf7';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.moveTo(-8, -2);
        ctx.lineTo(-8, 6);
        ctx.moveTo(0, -1);
        ctx.lineTo(0, 6);
        ctx.stroke();

        ctx.save();
        ctx.translate(26, 2);
        ctx.rotate(ship.propellerSpin);
        ctx.strokeStyle = '#f0f6fb';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.moveTo(-8, 0);
        ctx.lineTo(8, 0);
        ctx.moveTo(0, -8);
        ctx.lineTo(0, 8);
        ctx.stroke();
        ctx.restore();

        ctx.restore();

        ctx.fillStyle = 'rgba(255,255,255,0.9)';
        ctx.font = '700 12px Inter, sans-serif';
        ctx.fillText(ship.name, x - 30, y - 42);
      }

      function drawRaceFx() {
        if (state.phase === 'countdown') {
          const t = state.countdown;
          const txt = t > 1 ? String(Math.ceil(t)) : 'GO';
          ctx.save();
          ctx.fillStyle = 'rgba(0,0,0,0.24)';
          ctx.fillRect(0, 0, W, H);
          ctx.fillStyle = '#ffffff';
          ctx.font = '900 82px Inter, sans-serif';
          ctx.textAlign = 'center';
          ctx.textBaseline = 'middle';
          ctx.fillText(txt, W * 0.5, H * 0.48);
          ctx.restore();
        }

        if (state.phase === 'gameover') {
          ctx.save();
          ctx.fillStyle = 'rgba(0,0,0,0.5)';
          ctx.fillRect(0, 0, W, H);
          ctx.textAlign = 'center';
          ctx.fillStyle = '#ffffff';
          ctx.font = '900 44px Inter, sans-serif';
          ctx.fillText('GAME OVER', W * 0.5, H * 0.42);
          ctx.font = '600 20px Inter, sans-serif';
          ctx.fillStyle = 'rgba(255,255,255,0.84)';
          ctx.fillText('You lost all fictional credits. Press Restart.', W * 0.5, H * 0.51);
          ctx.restore();
        }

        if (state.flashes > 0) {
          ctx.save();
          ctx.fillStyle = 'rgba(255, 235, 180, ' + (state.flashes * 0.12) + ')';
          ctx.fillRect(0, 0, W, H);
          ctx.restore();
        }
      }

      function drawProgress() {
        const x = 24;
        const y = 18;
        const w = W - 48;
        ctx.fillStyle = 'rgba(0,0,0,0.25)';
        ctx.fillRect(x, y, w, 12);
        ctx.fillStyle = 'rgba(124,212,255,0.85)';
        ctx.fillRect(x, y, w * clamp(state.raceProgress, 0, 1), 12);
        ctx.strokeStyle = 'rgba(255,255,255,0.18)';
        ctx.strokeRect(x, y, w, 12);
      }

      function render() {
        ctx.clearRect(0, 0, W, H);
        drawBackground();
        drawProgress();
        drawParticles();

        for (let i = 0; i < state.shipCount; i++) {
          drawShip(state.ships[i], state.winnerIndex === i);
        }

        drawRaceFx();
      }

      betInputEl.addEventListener('input', () => {
        if (state.phase !== 'betting') return;
        const val = clamp((betInputEl.valueAsNumber || 0) | 0, 0, state.bankroll);
        state.currentBet = val;
        currentBetEl.textContent = credits(state.currentBet);
      });

      document.querySelectorAll('[data-bet]').forEach((btn) => {
        btn.addEventListener('click', () => {
          if (state.phase !== 'betting') return;
          const add = Number(btn.dataset.bet) || 0;
          state.currentBet = clamp(state.currentBet + add, 1, state.bankroll || 1);
          betInputEl.value = String(state.currentBet);
          currentBetEl.textContent = credits(state.currentBet);
        });
      });

      maxBetBtn.addEventListener('click', () => {
        if (state.phase !== 'betting') return;
        state.currentBet = Math.max(1, state.bankroll);
        betInputEl.value = String(state.currentBet);
        currentBetEl.textContent = credits(state.currentBet);
      });

      startBtn.addEventListener('click', startRace);
      restartBtn.addEventListener('click', resetGame);

      let last = performance.now();
      function frame(now) {
        const dt = Math.min(0.033, (now - last) / 1000);
        last = now;
        update(dt);
        render();
        requestAnimationFrame(frame);
      }

      generateRace();
      syncHud();
      requestAnimationFrame(frame);
    })();
  </script>
</body>
</html>
