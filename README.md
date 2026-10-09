<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SMASH ARENA</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Rajdhani:wght@500;700&display=swap');
    * { margin:0; padding:0; box-sizing:border-box; }
    body { background:#0a0a0f; color:#fff; font-family:'Rajdhani',sans-serif; overflow:hidden; user-select:none; }
    .screen { display:none; width:100vw; height:100vh; }
    .screen.active { display:flex; }

    #titleScreen {
      flex-direction:column; align-items:center; justify-content:center;
      background: radial-gradient(ellipse at 50% 40%, #2a0a4e 0%, #0d0020 50%, #000 100%);
      position:relative; overflow:hidden;
    }
    #titleScreen::before {
      content:''; position:absolute; inset:0;
      background: repeating-linear-gradient(0deg,transparent,transparent 59px,rgba(255,107,53,0.04) 60px),
                  repeating-linear-gradient(90deg,transparent,transparent 59px,rgba(255,107,53,0.04) 60px);
      pointer-events:none;
    }
    #titleScreen h1 {
      font-family:'Orbitron',sans-serif; font-size:5.5rem; font-weight:900;
      letter-spacing:0.12em; position:relative; z-index:1;
      background: linear-gradient(135deg,#ff6b35 0%,#f7c59f 40%,#fff 55%,#f7c59f 70%,#ff6b35 100%);
      -webkit-background-clip:text; -webkit-text-fill-color:transparent;
      filter: drop-shadow(0 0 30px rgba(255,107,53,0.6)) drop-shadow(0 0 60px rgba(232,67,147,0.4));
      animation: titlePulse 3s ease-in-out infinite;
      margin-bottom:0.15em;
    }
    #titleSubtitle {
      font-family:'Rajdhani',sans-serif; font-size:1rem; letter-spacing:0.4em;
      color:#ff6b35; text-transform:uppercase; margin-bottom:3rem; position:relative; z-index:1;
      opacity:0.8;
    }
    @keyframes titlePulse {
      0%,100%{ filter:drop-shadow(0 0 20px rgba(255,107,53,0.5)) drop-shadow(0 0 40px rgba(232,67,147,0.3)); }
      50%{ filter:drop-shadow(0 0 40px rgba(255,107,53,0.9)) drop-shadow(0 0 80px rgba(232,67,147,0.6)); }
    }
    .title-btns { display:flex; gap:1.2rem; position:relative; z-index:1; }
    .btn {
      padding:0.8rem 2.8rem; border:none; border-radius:4px; font-size:1rem;
      font-family:'Orbitron',sans-serif; font-weight:700; cursor:pointer;
      transition:all 0.2s; letter-spacing:0.08em; position:relative; overflow:hidden;
    }
    .btn-primary {
      background:linear-gradient(135deg,#ff6b35,#e84393);
      color:#fff; box-shadow:0 4px 20px rgba(255,107,53,0.4), 0 0 0 1px rgba(255,107,53,0.3);
    }
    .btn-primary:hover { transform:translateY(-2px) scale(1.03); box-shadow:0 8px 30px rgba(255,107,53,0.6), 0 0 0 1px rgba(255,107,53,0.5); }
    .btn-secondary {
      background:rgba(255,255,255,0.05); color:#ccc;
      border:1px solid rgba(255,255,255,0.15); backdrop-filter:blur(10px);
    }
    .btn-secondary:hover { background:rgba(255,255,255,0.1); color:#fff; border-color:rgba(255,107,53,0.5); }

    #charSelect {
      flex-direction:column; align-items:center; justify-content:flex-start;
      background: radial-gradient(ellipse at 50% 0%, #0d1a3e 0%, #050510 60%);
      padding:1.2rem; overflow:hidden;
    }
    #charSelect h2 {
      font-family:'Orbitron',sans-serif; font-size:1.6rem; font-weight:900;
      margin-bottom:0.2rem; color:#f7c59f;
      text-shadow:0 0 20px rgba(247,197,159,0.5);
    }
    #charSelect .subtitle { color:#556; margin-bottom:1rem; font-size:0.85rem; letter-spacing:0.1em; }
    .char-grid { display:grid; grid-template-columns:repeat(5,1fr); gap:0.5rem; width:100%; max-width:880px; margin-bottom:1rem; }
    .char-card {
      background:linear-gradient(135deg,#0d0d1e,#111128);
      border:1px solid rgba(255,255,255,0.08); border-radius:10px;
      padding:0.6rem 0.3rem; text-align:center; cursor:pointer;
      transition:all 0.2s; position:relative; overflow:hidden;
    }
    .char-card.selected {
      border-color:#ff6b35; background:linear-gradient(135deg,#1a0a1e,#200a28);
      box-shadow:0 0 20px rgba(255,107,53,0.4), inset 0 0 20px rgba(255,107,53,0.05);
    }
    .char-card .char-emoji { font-size:2rem; display:block; margin-bottom:0.25rem; }
    .char-card .char-name { font-size:0.7rem; font-weight:700; color:#ddd; font-family:'Orbitron',sans-serif; }
    .char-card .char-type { font-size:0.6rem; color:#556; margin-top:0.1rem; }

    .room-row {
      display:flex; gap:0.8rem; align-items:center; justify-content:center; margin:0.5rem 0 1rem; flex-wrap:wrap;
    }
    .room-row input {
      width:180px; padding:0.8rem 1rem; border-radius:8px; border:1px solid rgba(255,255,255,0.1);
      background:rgba(10,10,20,0.8); color:#fff; font-family:'Orbitron',sans-serif; font-size:0.85rem;
      letter-spacing:0.08em; text-transform:uppercase; outline:none;
    }
    .room-row input:focus { border-color:rgba(255,107,53,0.8); }
    .status-line {
      min-height:18px; color:#ffb28c; margin-bottom:12px; text-align:center; font-size:0.8rem; letter-spacing:0.08em;
    }

    .diff-row { display:flex; gap:0.5rem; margin-bottom:1rem; align-items:center; flex-wrap:wrap; justify-content:center; }
    .diff-row label { color:#556; font-size:0.8rem; margin-right:0.3rem; letter-spacing:0.1em; text-transform:uppercase; }
    .diff-btn {
      padding:0.35rem 0.8rem; border:1px solid rgba(255,255,255,0.1); border-radius:4px;
      background:rgba(0,0,0,0.3); color:#778; cursor:pointer; font-size:0.75rem;
      font-family:'Orbitron',sans-serif; font-weight:700; transition:all 0.15s;
    }
    .diff-btn.active { box-shadow:0 0 12px rgba(255,107,53,0.3), inset 0 0 8px rgba(255,107,53,0.05); }
    .char-preview {
      display:flex; gap:2rem; align-items:center; margin-bottom:1rem;
      background:linear-gradient(135deg,rgba(13,13,30,0.9),rgba(17,17,40,0.9));
      border:1px solid rgba(255,107,53,0.2); border-radius:12px; padding:0.9rem 2rem;
      backdrop-filter:blur(10px); box-shadow:0 4px 20px rgba(0,0,0,0.4);
    }
    .preview-big { font-size:3.5rem; }
    .preview-info h3 { font-size:1.1rem; font-weight:800; color:#f7c59f; font-family:'Orbitron',sans-serif; }
    .preview-info p { color:#556; font-size:0.8rem; margin-top:0.2rem; }

    #gameScreen { flex-direction:column; background:#000; }
    #hud {
      display:flex; justify-content:space-between; align-items:center;
      background:linear-gradient(180deg,rgba(5,5,20,0.98),rgba(8,8,25,0.95));
      border-bottom:1px solid rgba(255,107,53,0.2);
      padding:0 1.5rem; height:64px; flex-shrink:0;
      box-shadow:0 2px 20px rgba(0,0,0,0.8);
    }
    .hud-player { display:flex; align-items:center; gap:1rem; }
    .hud-portrait {
      width:44px; height:44px; border-radius:8px; display:flex; align-items:center; justify-content:center;
      font-size:1.6rem; background:rgba(255,255,255,0.05);
      border:1px solid rgba(255,255,255,0.1);
      box-shadow:0 0 10px rgba(0,0,0,0.5);
    }
    .hud-info { display:flex; flex-direction:column; gap:3px; }
    .hud-name { font-size:0.65rem; color:#556; text-transform:uppercase; letter-spacing:0.1em; font-family:'Orbitron',sans-serif; }
    .hud-damage {
      font-size:2rem; font-weight:900; font-family:'Orbitron',sans-serif;
      transition:color 0.2s, text-shadow 0.2s; line-height:1;
    }
    .hud-damage.low { color:#44ff88; text-shadow:0 0 10px rgba(68,255,136,0.5); }
    #canvas-wrap { flex:1; position:relative; overflow:hidden; }
    #gameCanvas { display:block; width:100%; height:100%; }
    .hud-center { display:flex; flex-direction:column; align-items:center; gap:3px; }
    .hud-timer { font-size:2rem; font-weight:900; font-family:'Orbitron',sans-serif; color:#f7c59f; min-width:70px; text-align:center; line-height:1; }
    #resultScreen {
      flex-direction:column; align-items:center; justify-content:center;
      background:radial-gradient(ellipse at center,#1a0a2e,#0a0a0f);
      position:relative; overflow:hidden;
    }
    #resultScreen h2 {
      font-family:'Orbitron',sans-serif; font-size:3.5rem; font-weight:900;
      color:#f7c59f; margin-bottom:0.4rem;
      text-shadow:0 0 30px rgba(247,197,159,0.5);
    }
    #resultScreen .result-sub { color:#556; margin-bottom:2rem; font-size:1rem; letter-spacing:0.05em; }
    .result-stats { display:flex; gap:3rem; margin-bottom:2.5rem; }
    .result-stat {
      text-align:center; background:rgba(255,255,255,0.03);
      border:1px solid rgba(255,107,53,0.2); border-radius:12px; padding:1rem 1.5rem;
    }
    .result-stat .val { font-size:2.2rem; font-weight:900; color:#ff6b35; font-family:'Orbitron',sans-serif; }
    .result-stat .lbl { font-size:0.7rem; color:#556; margin-top:0.3rem; text-transform:uppercase; letter-spacing:0.1em; }

    #adminOverlay, #adminLogin, #pauseOverlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,0.9); z-index:1000; align-items:center; justify-content:center; }
    #adminOverlay.open, #adminLogin.open, #pauseOverlay.open { display:flex; }
    #adminPanel {
      background:linear-gradient(135deg,#0d0d1e,#111128); border:1px solid rgba(255,107,53,0.4); border-radius:16px;
      padding:2rem; width:520px; max-height:85vh; overflow-y:auto;
    }
    .toggle-row { display:flex; justify-content:space-between; align-items:center; padding:0.5rem 0; border-bottom:1px solid rgba(255,255,255,0.04); }
    .toggle { width:40px; height:22px; border-radius:11px; background:#1a1a2e; position:relative; cursor:pointer; }
    .toggle.on { background:linear-gradient(90deg,#ff6b35,#e84393); }
    .toggle::after { content:''; position:absolute; top:3px; left:3px; width:14px; height:14px; border-radius:50%; background:#fff; transition:left 0.25s; }
    .toggle.on::after { left:21px; }
    .login-box {
      background:linear-gradient(135deg,#0d0d1e,#111128); border:1px solid rgba(255,255,255,0.1); border-radius:16px;
      padding:2rem; width:320px; text-align:center;
    }
    .login-box h3 { color:#f7c59f; margin-bottom:1rem; font-family:'Orbitron',sans-serif; font-size:1rem; }
    .login-box input {
      width:100%; padding:0.7rem 1rem; background:rgba(0,0,0,0.4); border:1px solid rgba(255,255,255,0.1); border-radius:6px;
      color:#fff; font-size:0.9rem; margin-bottom:0.8rem; outline:none;
    }
    .login-box button { flex:1; padding:0.65rem; border:none; border-radius:6px; cursor:pointer; font-weight:700; font-family:'Orbitron',sans-serif; font-size:0.8rem; }
    #loginConfirm { background:linear-gradient(135deg,#ff6b35,#e84393); color:#fff; }
    #loginCancel { background:rgba(255,255,255,0.05); color:#aaa; border:1px solid rgba(255,255,255,0.1); }
    .login-btns { display:flex; gap:0.5rem; }
    .pause-box {
      background:linear-gradient(135deg,#0d0d1e,#111128); border:1px solid rgba(255,255,255,0.1); border-radius:20px;
      padding:2.5rem 3rem; text-align:center;
    }
    .pause-box h2 { color:#f7c59f; font-size:2rem; margin-bottom:1.5rem; font-family:'Orbitron',sans-serif; }
  </style>
</head>
<body>
  <div class="screen active" id="titleScreen">
    <canvas id="titleCanvas" style="position:absolute;inset:0;width:100%;height:100%;pointer-events:none;"></canvas>
    <h1>⚔️ SMASH ARENA</h1>
    <p id="titleSubtitle">SELECT YOUR FIGHTER &nbsp;·&nbsp; BATTLE FOR GLORY</p>
    <div class="title-btns">
      <button class="btn btn-primary" onclick="showScreen('charSelect')">▶ PLAY</button>
      <button class="btn btn-secondary" onclick="openAdminLogin()">⚙ ADMIN</button>
    </div>
  </div>

  <div class="screen" id="charSelect">
    <h2>⚔️ CHARACTER SELECT</h2>
    <p class="subtitle">Choose your fighter — then create or join a room</p>
    <div class="char-grid" id="charGrid"></div>

    <div class="room-row">
      <input id="roomCodeInput" maxlength="5" placeholder="ROOM CODE" />
      <button class="btn btn-primary" id="createRoomBtn">CREATE</button>
      <button class="btn btn-secondary" id="joinRoomBtn">JOIN</button>
    </div>
    <div class="status-line" id="roomStatus">Waiting for a match</div>

    <div class="diff-row">
      <label>DIFFICULTY:</label>
      <div id="diffBtns" style="display:flex;gap:0.5rem;flex-wrap:wrap;"></div>
    </div>

    <div class="char-preview" id="charPreview">
      <div class="preview-big" id="previewEmoji">❓</div>
      <div class="preview-info">
        <h3 id="previewName">Select a character</h3>
        <p id="previewDesc">—</p>
      </div>
    </div>

    <div style="display:flex;gap:1rem;">
      <button class="btn btn-primary" id="startFightBtn" onclick="startOnlineMatch()" disabled>⚔️ FIGHT!</button>
      <button class="btn btn-secondary" onclick="showScreen('titleScreen')">← Back</button>
    </div>
  </div>

  <div class="screen" id="gameScreen">
    <div id="hud">
      <div class="hud-player">
        <div class="hud-portrait" id="p1Portrait">🥷</div>
        <div class="hud-info">
          <div class="hud-name" id="p1Name">PLAYER</div>
          <div class="hud-damage low" id="p1Damage">0%</div>
        </div>
      </div>
      <div class="hud-center">
        <div class="hud-timer" id="hudTimer">99</div>
      </div>
      <div class="hud-player" style="flex-direction:row-reverse;">
        <div class="hud-portrait" id="p2Portrait">🤖</div>
        <div class="hud-info" style="align-items:flex-end;">
          <div class="hud-name" id="p2Name">BOT</div>
          <div class="hud-damage low" id="p2Damage">0%</div>
        </div>
      </div>
    </div>
    <div id="canvas-wrap">
      <canvas id="gameCanvas"></canvas>
    </div>
  </div>

  <div class="screen" id="resultScreen">
    <h2 id="resultTitle">🏆 WINNER!</h2>
    <p class="result-sub" id="resultSub">—</p>
    <div class="result-stats">
      <div class="result-stat"><div class="val" id="resDamage">0%</div><div class="lbl">Damage</div></div>
      <div class="result-stat"><div class="val" id="resTime">0s</div><div class="lbl">Fight Time</div></div>
    </div>
    <div style="display:flex;gap:1rem;flex-wrap:wrap;justify-content:center;">
      <button class="btn btn-primary" onclick="showScreen('charSelect')">🔄 Back To Select</button>
      <button class="btn btn-secondary" onclick="showScreen('titleScreen')">🏠 Title</button>
    </div>
  </div>

  <div id="adminLogin">
    <div class="login-box">
      <h3>🔐 Admin Access</h3>
      <input type="password" id="adminPwInput" placeholder="Password" onkeydown="if(event.key==='Enter') confirmLogin()" />
      <div class="login-btns">
        <button id="loginConfirm" onclick="confirmLogin()">Enter</button>
        <button id="loginCancel" onclick="closeAdminLogin()">Cancel</button>
      </div>
    </div>
  </div>

  <div id="adminOverlay">
    <div id="adminPanel">
      <h2 style="color:#ff6b35; font-size:1.3rem; font-weight:900; margin-bottom:1.2rem; font-family:'Orbitron',sans-serif;">⚙️ Admin Panel</h2>
      <div class="toggle-row"><label>Infinite Stocks</label><div class="toggle" id="tgl_infiniteStocks" onclick="toggleAdmin('infiniteStocks')"></div></div>
      <div class="toggle-row"><label>Infinite Time</label><div class="toggle" id="tgl_infiniteTime" onclick="toggleAdmin('infiniteTime')"></div></div>
      <div class="toggle-row"><label>One-Hit KO</label><div class="toggle" id="tgl_ohko" onclick="toggleAdmin('ohko')"></div></div>
      <button class="btn btn-primary" style="margin-top:1rem; width:100%;" onclick="closeAdmin()">✕ Close</button>
    </div>
  </div>

  <div id="pauseOverlay">
    <div class="pause-box">
      <h2>⏸ PAUSED</h2>
      <button class="btn btn-primary" onclick="resumeGame()" style="display:block;width:100%;margin-bottom:0.7rem;">▶ Resume</button>
      <button class="btn btn-secondary" onclick="openAdminLogin()" style="display:block;width:100%;margin-bottom:0.7rem;">⚙ Admin</button>
      <button class="btn btn-secondary" onclick="quitToTitle()" style="display:block;width:100%;">🏠 Quit</button>
    </div>
  </div>

  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();
    const CHARACTERS = [
      {id:'ninja', name:'Ninja', emoji:'🥷', type:'Speedster', desc:'Lightning fast, low power'},
      {id:'mage', name:'Mage', emoji:'🧙', type:'Zoner', desc:'Long-range magic blasts'},
      {id:'knight', name:'Knight', emoji:'🛡️', type:'Tank', desc:'Heavy armor, slow but tough'},
      {id:'fox', name:'Fox', emoji:'🦊', type:'Trickster', desc:'Agile with reflector'},
      {id:'dragon', name:'Dragon', emoji:'🐉', type:'Powerhouse', desc:'Massive knockback, slow'},
      {id:'robot', name:'Robot', emoji:'🤖', type:'Technical', desc:'Precise combos, no mercy'},
      {id:'witch', name:'Witch', emoji:'🧝', type:'Zoner', desc:'Curses and ranged spells'},
      {id:'samurai', name:'Samurai', emoji:'⚔️', type:'Swordsman', desc:'Precise sword strikes'},
      {id:'ghost', name:'Ghost', emoji:'👻', type:'Trickster', desc:'Phases through platforms'},
      {id:'bear', name:'Bear', emoji:'🐻', type:'Tank', desc:'Enormous weight, huge hits'},
      {id:'angel', name:'Angel', emoji:'😇', type:'Floater', desc:'Multiple jumps, very light'},
      {id:'demon', name:'Demon', emoji:'😈', type:'Rushdown', desc:'Aggressive, high damage'},
      {id:'pirate', name:'Pirate', emoji:'🏴‍☠️', type:'Brawler', desc:'Unpredictable, tricky'},
      {id:'alien', name:'Alien', emoji:'👽', type:'Zoner', desc:'Weird physics, beam attacks'},
      {id:'viking', name:'Viking', emoji:'🪓', type:'Powerhouse', desc:'Axe combos, high knockback'},
      {id:'cat', name:'Cat', emoji:'🐱', type:'Speedster', desc:'Fastest on the ground'},
      {id:'wizard', name:'Wizard', emoji:'🧝‍♂️', type:'Mage', desc:'Teleport and arcane blasts'},
      {id:'zombie', name:'Zombie', emoji:'🧟', type:'Tank', desc:'Slow but relentless'},
      {id:'phoenix', name:'Phoenix', emoji:'🦅', type:'Floater', desc:'Rises from KO with fire'},
      {id:'golem', name:'Golem', emoji:'🗿', type:'Tank', desc:'Immovable, devastating smash'}
    ];

    const DIFFICULTIES = [
      {id:'easy', label:'Easy'},
      {id:'medium', label:'Medium'},
      {id:'hard', label:'Hard'},
      {id:'expert', label:'Expert'}
    ];

    let selectedChar = null;
    let selectedDiff = DIFFICULTIES[1];
    let roomCode = '';
    let onlineState = null;
    let gameRunning = false;
    let myId = null;
    let rafId = null;

    function showScreen(id) {
      document.querySelectorAll('.screen').forEach((screen) => screen.classList.remove('active'));
      document.getElementById(id).classList.add('active');
      if (id === 'gameScreen') resizeCanvas();
    }

    function buildCharSelect() {
      const grid = document.getElementById('charGrid');
      grid.innerHTML = '';
      CHARACTERS.forEach((char) => {
        const card = document.createElement('div');
        card.className = 'char-card';
        card.innerHTML = `<span class="char-emoji">${char.emoji}</span><div class="char-name">${char.name}</div><div class="char-type">${char.type}</div>`;
        card.onclick = () => selectChar(char);
        grid.appendChild(card);
      });

      const diffBtns = document.getElementById('diffBtns');
      diffBtns.innerHTML = '';
      DIFFICULTIES.forEach((diff) => {
        const btn = document.createElement('button');
        btn.className = 'diff-btn' + (diff.id === selectedDiff.id ? ' active' : '');
        btn.textContent = diff.label;
        btn.onclick = () => {
          selectedDiff = diff;
          document.querySelectorAll('.diff-btn').forEach((el) => el.classList.remove('active'));
          btn.classList.add('active');
        };
        diffBtns.appendChild(btn);
      });
    }

    function selectChar(char) {
      selectedChar = char;
      document.querySelectorAll('.char-card').forEach((card) => card.classList.remove('selected'));
      const target = [...document.querySelectorAll('.char-card')].find((card) => card.textContent.includes(char.name));
      if (target) target.classList.add('selected');
      document.getElementById('previewEmoji').textContent = char.emoji;
      document.getElementById('previewName').textContent = `${char.name} — ${char.type}`;
      document.getElementById('previewDesc').textContent = char.desc;
      document.getElementById('startFightBtn').disabled = false;
    }

    document.getElementById('createRoomBtn').onclick = () => {
      if (!selectedChar) return;
      const name = 'Host';
      socket.emit('createRoom', { name, charId: selectedChar.id });
      document.getElementById('roomStatus').textContent = 'Creating room...';
    };

    document.getElementById('joinRoomBtn').onclick = () => {
      if (!selectedChar) return;
      const code = document.getElementById('roomCodeInput').value.trim();
      if (!code) {
        document.getElementById('roomStatus').textContent = 'Enter a room code to join.';
        return;
      }
      socket.emit('joinRoom', { code, name: 'Guest', charId: selectedChar.id });
      document.getElementById('roomStatus').textContent = 'Joining room...';
    };

    socket.on('roomCreated', ({ code }) => {
      roomCode = code;
      document.getElementById('roomCodeInput').value = code;
      document.getElementById('roomStatus').textContent = `Room created: ${code}`;
    });

    socket.on('roomError', (message) => {
      document.getElementById('roomStatus').textContent = message;
    });

    socket.on('matchStart', () => {
      startOnlineMatch(true);
    });

    function startOnlineMatch(autoStart = false) {
      if (!selectedChar) return;
      if (!roomCode && !autoStart) {
        roomCode = document.getElementById('roomCodeInput').value.trim();
      }
      if (!roomCode && !autoStart) {
        document.getElementById('roomStatus').textContent = 'Create or join a room first.';
        return;
      }
      showScreen('gameScreen');
      resizeCanvas();
      gameRunning = true;
      socket.emit('input', { left: false, right: false, jump: false, attack: false, special: false });
      document.getElementById('p1Name').textContent = 'YOU';
      document.getElementById('p2Name').textContent = 'OPPONENT';
      document.getElementById('p1Portrait').textContent = selectedChar.emoji;
      document.getElementById('p2Portrait').textContent = '🤖';
      startLoop();
    }

    function startLoop() {
      cancelAnimationFrame(rafId);
      const renderFrame = () => {
        if (!gameRunning) return;
        drawGame();
        rafId = requestAnimationFrame(renderFrame);
      };
      renderFrame();
    }

    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    function resizeCanvas() {
      const w = document.getElementById('canvas-wrap');
      canvas.width = w.clientWidth;
      canvas.height = w.clientHeight;
    }
    window.addEventListener('resize', resizeCanvas);

    socket.on('roomState', (state) => {
      onlineState = state;
      if (state.players && state.players.length >= 2) {
        const myPlayer = state.players.find((player) => player.id === socket.id) || state.players[0];
        myId = myPlayer ? myPlayer.id : socket.id;
        document.getElementById('p1Name').textContent = state.players[0].name || 'YOU';
        document.getElementById('p2Name').textContent = state.players[1].name || 'OPPONENT';
        document.getElementById('p1Portrait').textContent = (state.players[0].charId ? CHARACTERS.find((c) => c.id === state.players[0].charId)?.emoji : '🥷') || '🥷';
        document.getElementById('p2Portrait').textContent = (state.players[1].charId ? CHARACTERS.find((c) => c.id === state.players[1].charId)?.emoji : '🤖') || '🤖';
        document.getElementById('hudTimer').textContent = Math.ceil(state.timer || 99);
      }
    });

    socket.on('matchEnd', ({ winner }) => {
      gameRunning = false;
      document.getElementById('resultTitle').textContent = winner === 'Draw' ? '🤝 DRAW!' : winner === 'Host' || winner === 'YOU' ? '🏆 YOU WIN!' : '💀 YOU LOSE!';
      document.getElementById('resultSub').textContent = `Winner: ${winner}`;
      document.getElementById('resDamage').textContent = '0%';
      document.getElementById('resTime').textContent = '99s';
      showScreen('resultScreen');
    });

    function emitInput() {
      const input = {
        left: !!keys['KeyA'],
        right: !!keys['KeyD'],
        jump: !!keys['KeyW'] || !!keys['Space'],
        attack: !!keys['KeyF'],
        special: !!keys['KeyG'],
      };
      socket.emit('input', input);
    }

    const keys = {};
    document.addEventListener('keydown', (e) => {
      keys[e.code] = true;
      if (e.code === 'Escape' && gameRunning) {
        document.getElementById('pauseOverlay').classList.add('open');
      }
      if (['Space','ArrowUp','ArrowDown'].includes(e.code)) e.preventDefault();
      emitInput();
    });

    document.addEventListener('keyup', (e) => {
      keys[e.code] = false;
      emitInput();
    });

    function drawBackground() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      const grad = ctx.createLinearGradient(0, 0, 0, canvas.height);
      grad.addColorStop(0, '#050015');
      grad.addColorStop(0.6, '#0e0820');
      grad.addColorStop(1, '#0a0a0f');
      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = 'rgba(255,255,255,0.8)';
      for (let i = 0; i < 80; i++) {
        const x = (i * 97) % canvas.width;
        const y = (i * 59) % (canvas.height * 0.7);
        ctx.fillRect(x, y, 2, 2);
      }

      const floorY = canvas.height - 70;
      ctx.fillStyle = '#1e1e3a';
      ctx.fillRect(0, floorY, canvas.width, 70);
      ctx.fillStyle = '#ff6b35';
      ctx.fillRect(0, floorY, canvas.width, 4);
    }

    function drawFighter(player) {
      if (!player) return;
      const isMine = player.id === socket.id;
      const char = CHARACTERS.find((c) => c.id === player.charId) || CHARACTERS[0];
      const x = player.x;
      const y = player.y;
      const w = player.w || 48;
      const h = player.h || 48;

      ctx.save();
      ctx.translate(x, y);
      ctx.scale(player.facing || 1, 1);

      ctx.fillStyle = 'rgba(0,0,0,0.35)';
      ctx.beginPath();
      ctx.ellipse(0, h + 8, w * 0.8, 12, 0, 0, Math.PI * 2);
      ctx.fill();

      ctx.fillStyle = char.name === 'Mage' ? '#9b59b6' : '#1f2a44';
      ctx.fillRect(-w / 2, 0, w, h);
      ctx.fillStyle = '#fff';
      ctx.fillRect(-w / 3, -12, w / 1.5, 18);
      ctx.fillStyle = '#ff6b35';
      ctx.fillRect(-w / 2 + 6, 8, w - 12, 7);
      ctx.fillStyle = '#111';
      ctx.fillRect(-w / 2 + 4, h - 4, w - 8, 6);

      ctx.restore();

      ctx.font = 'bold 14px Orbitron';
      ctx.fillStyle = isMine ? '#ff6b35' : '#b8d9ff';
      ctx.textAlign = 'center';
      ctx.fillText(char.emoji, x, y - 18);
      ctx.fillText(`${player.damage.toFixed(0)}%`, x, y - 34);
    }

    function drawGame() {
      drawBackground();
      if (!onlineState || !onlineState.players) return;

      onlineState.players.forEach((player) => {
        player.y = player.y || 0;
        drawFighter(player);
      });

      const p1 = onlineState.players[0];
      const p2 = onlineState.players[1];
      if (p1) {
        document.getElementById('p1Damage').textContent = `${Math.floor(p1.damage || 0)}%`;
      }
      if (p2) {
        document.getElementById('p2Damage').textContent = `${Math.floor(p2.damage || 0)}%`;
      }
      document.getElementById('hudTimer').textContent = Math.ceil(onlineState.timer || 99);
    }

    function resumeGame() {
      document.getElementById('pauseOverlay').classList.remove('open');
    }

    function quitToTitle() {
      document.getElementById('pauseOverlay').classList.remove('open');
      gameRunning = false;
      showScreen('titleScreen');
    }

    function openAdminLogin() {
      document.getElementById('adminPwInput').value = '';
      document.getElementById('adminLogin').classList.add('open');
    }

    function closeAdminLogin() {
      document.getElementById('adminLogin').classList.remove('open');
    }

    function confirmLogin() {
      if (document.getElementById('adminPwInput').value === 'ericking123') {
        closeAdminLogin();
        openAdmin();
      }
    }

    function openAdmin() {
      document.getElementById('adminOverlay').classList.add('open');
    }

    function closeAdmin() {
      document.getElementById('adminOverlay').classList.remove('open');
    }

    function toggleAdmin(key) {
      const el = document.getElementById('tgl_' + key);
      if (el) el.classList.toggle('on');
    }

    const titleCanvas = document.getElementById('titleCanvas');
    const tctx = titleCanvas.getContext('2d');
    const stars = [];
    function resizeTitle() {
      titleCanvas.width = window.innerWidth;
      titleCanvas.height = window.innerHeight;
    }
    resizeTitle();
    window.addEventListener('resize', resizeTitle);
    for (let i = 0; i < 150; i++) {
      stars.push({ x: Math.random() * titleCanvas.width, y: Math.random() * titleCanvas.height, r: Math.random() * 2 + 1 });
    }
    function animTitle() {
      if (!document.getElementById('titleScreen').classList.contains('active')) return;
      tctx.clearRect(0, 0, titleCanvas.width, titleCanvas.height);
      stars.forEach((star) => {
        star.y += 0.2;
        if (star.y > titleCanvas.height) star.y = 0;
        tctx.fillStyle = 'rgba(255,255,255,0.8)';
        tctx.beginPath();
        tctx.arc(star.x, star.y, star.r, 0, Math.PI * 2);
        tctx.fill();
      });
      requestAnimationFrame(animTitle);
    }
    animTitle();

    buildCharSelect();
    resizeCanvas();
  </script>
</body>
</html>
