
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>AITT Plus - Ludo</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
<style>
/* ============ RESET & BASE ============ */
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden;font-family:'Cairo',sans-serif;background:#050816;color:#fff}

/* ============ BACKGROUND ============ */
#bg{position:fixed;inset:0;z-index:0;background:radial-gradient(circle at 20% 20%,#0a1a4a 0%,#050816 50%),radial-gradient(circle at 80% 80%,#2a0a4a 0%,transparent 50%)}
#bg::before{content:'';position:absolute;inset:0;background-image:linear-gradient(rgba(0,212,255,.05) 1px,transparent 1px),linear-gradient(90deg,rgba(0,212,255,.05) 1px,transparent 1px);background-size:40px 40px;animation:grid 20s linear infinite}
@keyframes grid{to{background-position:40px 40px}}

/* ============ SCREENS ============ */
.screen{position:fixed;inset:0;z-index:10;display:none;flex-direction:column;padding:16px;overflow-y:auto}
.screen.active{display:flex;animation:fadeIn .4s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

/* ============ SPLASH ============ */
#splash{justify-content:center;align-items:center;text-align:center}
.logo{width:150px;height:150px;border-radius:50%;background:radial-gradient(circle,#2196f3,#0a4a8a);display:flex;align-items:center;justify-content:center;font-family:'Orbitron',sans-serif;font-size:48px;font-weight:900;color:#fff;box-shadow:0 0 40px #00d4ff,0 0 80px #00d4ff88;margin-bottom:20px;position:relative;animation:pulse 2s infinite}
.logo::after{content:'+';position:absolute;top:-5px;right:-5px;color:#00d4ff;font-size:30px;text-shadow:0 0 10px #00d4ff}
@keyframes pulse{0%,100%{transform:scale(1);box-shadow:0 0 40px #00d4ff,0 0 80px #00d4ff88}50%{transform:scale(1.05);box-shadow:0 0 60px #00d4ff,0 0 120px #00d4ff}}
h1{font-family:'Orbitron',sans-serif;font-size:32px;letter-spacing:4px;background:linear-gradient(90deg,#00d4ff,#a855f7);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin-bottom:8px}
.subtitle{color:#8899bb;font-size:14px;margin-bottom:40px;letter-spacing:2px}

/* ============ BUTTONS ============ */
.btn{background:linear-gradient(135deg,#00d4ff,#0066ff);border:none;color:#fff;padding:14px 40px;border-radius:50px;font-family:'Cairo',sans-serif;font-size:18px;font-weight:700;cursor:pointer;box-shadow:0 0 20px #00d4ff88,0 4px 20px rgba(0,0,0,.4);transition:all .2s;display:inline-flex;align-items:center;gap:10px}
.btn:active{transform:scale(.95)}
.btn:disabled{opacity:.4;cursor:not-allowed;box-shadow:none}

/* ============ HEADER ============ */
.header{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:16px}
.title{font-family:'Orbitron',sans-serif;font-size:18px;color:#00d4ff}
.coins{background:rgba(0,212,255,.15);border:1px solid #00d4ff66;border-radius:20px;padding:6px 14px;font-size:14px;font-weight:700}

/* ============ MENU ============ */
.menu-card{background:linear-gradient(135deg,rgba(0,212,255,.1),rgba(168,85,247,.1));border:1px solid #00d4ff44;border-radius:16px;padding:18px;margin-bottom:12px;display:flex;align-items:center;gap:14px;cursor:pointer;transition:all .2s;backdrop-filter:blur(10px)}
.menu-card:active{transform:scale(.98);border-color:#00d4ff}
.menu-card .icon{font-size:32px}
.menu-card h3{font-size:16px;color:#fff;margin-bottom:2px}
.menu-card p{font-size:12px;color:#8899bb}
.menu-card .arrow{margin-right:auto;font-size:24px;color:#00d4ff}

/* ============ AVATAR GRID ============ */
.avatar-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;padding-bottom:80px}
.avatar-item{aspect-ratio:1;border-radius:12px;border:2px solid #1a2a4a;background:#0a1428;display:flex;align-items:center;justify-content:center;font-size:40px;cursor:pointer;transition:all .2s;position:relative;overflow:hidden}
.avatar-item.selected{border-color:#00d4ff;box-shadow:0 0 20px #00d4ff88;transform:scale(1.05)}
.avatar-item.vip{border-color:#ffd700;background:linear-gradient(135deg,#1a1000,#0a0a0a)}
.avatar-item.vip.selected{box-shadow:0 0 25px #ffd70088}
.avatar-item .crown{position:absolute;top:2px;right:4px;font-size:14px}

/* ============ GAME BOARD ============ */
#game{padding:8px}
.game-top{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;margin-bottom:8px}
.player-box{background:rgba(10,20,40,.8);border-radius:10px;padding:6px;text-align:center;border:2px solid transparent;transition:all .3s}
.player-box.active{border-color:#00d4ff;box-shadow:0 0 15px #00d4ff88}
.player-box.red.active{border-color:#ff3355;box-shadow:0 0 15px #ff335588}
.player-box.blue.active{border-color:#3399ff;box-shadow:0 0 15px #3399ff88}
.player-box.green.active{border-color:#33cc66;box-shadow:0 0 15px #33cc6688}
.player-box.yellow.active{border-color:#ffcc00;box-shadow:0 0 15px #ffcc0088}
.player-box .avatar{font-size:24px}
.player-box .name{font-size:10px;color:#8899bb;margin-top:2px}
.player-box .score{font-size:12px;font-weight:700}

/* ============ BOARD CANVAS ============ */
.board-wrap{position:relative;width:100%;aspect-ratio:1;background:radial-gradient(circle,#0a1428,#050816);border-radius:16px;border:2px solid #00d4ff33;overflow:hidden;box-shadow:0 0 40px #00d4ff22 inset}
canvas{width:100%;height:100%;display:block}

/* ============ DICE ============ */
.game-bottom{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-top:8px}
.turn-info{background:rgba(0,212,255,.15);border:1px solid #00d4ff66;border-radius:20px;padding:8px 14px;font-size:13px;font-weight:700;flex:1}
.dice-platform{width:70px;height:70px;border-radius:50%;background:radial-gradient(circle,#00d4ff,#0066ff);display:flex;align-items:center;justify-content:center;box-shadow:0 0 30px #00d4ff,0 0 60px #00d4ff66;cursor:pointer;transition:all .2s;position:relative}
.dice-platform:active{transform:scale(.9)}
.dice-platform.rolling{animation:roll 1.5s ease}
@keyframes roll{0%{transform:rotate(0) scale(1)}50%{transform:rotate(720deg) scale(1.3)}100%{transform:rotate(1440deg) scale(1)}}
.dice-face{font-size:40px;color:#fff;text-shadow:0 0 10px #fff}

/* ============ VICTORY ============ */
#victory{justify-content:center;align-items:center;text-align:center}
.trophy{font-size:100px;animation:bounce 1s infinite;filter:drop-shadow(0 0 20px gold)}
@keyframes bounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-20px)}}
.victory-title{font-family:'Orbitron',sans-serif;font-size:36px;background:linear-gradient(90deg,#ffd700,#ff8800);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin:20px 0}
.leaderboard{width:100%;max-width:400px;margin:20px 0}
.lb-row{display:flex;align-items:center;gap:10px;background:rgba(10,20,40,.8);border-radius:12px;padding:12px;margin-bottom:8px;border:1px solid #1a2a4a}
.lb-row.first{border-color:#ffd700;box-shadow:0 0 20px #ffd70066}
.lb-rank{font-size:20px;font-weight:900;color:#00d4ff;width:30px}
.lb-name{flex:1;text-align:right}
.lb-score{font-weight:700;color:#ffd700}

/* ============ MODAL ============ */
.modal{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:100;display:none;align-items:center;justify-content:center;padding:20px}
.modal.active{display:flex}
.modal-box{background:#0a1428;border:2px solid #00d4ff;border-radius:16px;padding:24px;max-width:320px;width:100%;text-align:center;box-shadow:0 0 40px #00d4ff88}
.modal-box h3{font-family:'Orbitron',sans-serif;color:#00d4ff;margin-bottom:12px}

/* ============ SCROLLBAR ============ */
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-thumb{background:#00d4ff66;border-radius:2px}
</style>
</head>
<body>

<div id="bg"></div>

<!-- ============ SPLASH ============ -->
<div class="screen active" id="splash">
  <div class="logo">AITT</div>
  <h1>LUDO AITTP</h1>
  <p class="subtitle">PLAY • CONNECT • ENJOY</p>
  <button class="btn" onclick="Game.go('menu')">▶ PLAY</button>
</div>

<!-- ============ MENU ============ -->
<div class="screen" id="menu">
  <div class="header">
    <div class="title">AITT PLUS</div>
    <div class="coins">🪙 <span id="coinDisplay">0</span></div>
  </div>
  <h2 style="font-size:14px;color:#8899bb;margin-bottom:12px">Select Mode</h2>

  <div class="menu-card" onclick="Game.quickMatch()">
    <div class="icon">🌍</div>
    <div><h3>Quick Match</h3><p>Play with random players</p></div>
    <div class="arrow">›</div>
  </div>
  <div class="menu-card" onclick="Game.showModal('قريباً','اللعب مع الأصدقاء سيكون متاحاً قريباً')">
    <div class="icon">👥</div>
    <div><h3>Play with Friends</h3><p>Create or join a room</p></div>
    <div class="arrow">›</div>
  </div>
  <div class="menu-card" onclick="Game.go('avatar')">
    <div class="icon">🤖</div>
    <div><h3>VS AI</h3><p>Choose difficulty</p></div>
    <div class="arrow">›</div>
  </div>
  <div class="menu-card" onclick="Game.showModal('قريباً','لوحة الترتيب ستكون متاحة قريباً')">
    <div class="icon">🏆</div>
    <div><h3>Leaderboard</h3><p>Top players</p></div>
    <div class="arrow">›</div>
  </div>
  <div class="menu-card" onclick="Game.showModal('قريباً','الإعدادات ستكون متاحة قريباً')">
    <div class="icon">⚙️</div>
    <div><h3>Settings</h3><p>Sound, language...</p></div>
    <div class="arrow">›</div>
  </div>
</div>

<!-- ============ AVATAR ============ -->
<div class="screen" id="avatar">
  <div class="header">
    <div class="title">👤 CHARACTER</div>
    <button class="coins" onclick="Game.go('menu')" style="cursor:pointer">← Back</button>
  </div>
  <h2 style="font-size:14px;color:#8899bb;margin-bottom:12px">Choose Your Character</h2>
  <div class="avatar-grid" id="avatarGrid"></div>
  <div style="position:fixed;bottom:0;left:0;right:0;padding:16px;background:linear-gradient(transparent,#050816)">
    <button class="btn" style="width:100%;justify-content:center" onclick="Game.confirmAvatar()">✓ CONFIRM</button>
  </div>
</div>

<!-- ============ GAME ============ -->
<div class="screen" id="game">
  <div class="game-top">
    <div class="player-box red" id="pbox-0"><div class="avatar" id="pav-0">🦊</div><div class="name">You</div><div class="score" id="psc-0">0</div></div>
    <div class="player-box blue" id="pbox-1"><div class="avatar" id="pav-1">🐧</div><div class="name">AI 1</div><div class="score" id="psc-1">0</div></div>
    <div class="player-box green" id="pbox-2"><div class="avatar" id="pav-2">🐢</div><div class="name">AI 2</div><div class="score" id="psc-2">0</div></div>
    <div class="player-box yellow" id="pbox-3"><div class="avatar" id="pav-3">🦁</div><div class="name">AI 3</div><div class="score" id="psc-3">0</div></div>
  </div>

  <div class="board-wrap">
    <canvas id="board"></canvas>
  </div>

  <div class="game-bottom">
    <div class="turn-info" id="turnInfo">🎲 دورك - ارمِ النرد</div>
    <div class="dice-platform" id="diceBtn" onclick="Game.rollDice()">
      <div class="dice-face" id="diceFace">⚀</div>
    </div>
  </div>
</div>

<!-- ============ VICTORY ============ -->
<div class="screen" id="victory">
  <div class="trophy">🏆</div>
  <h1 class="victory-title" id="victoryTitle">VICTORY!</h1>
  <div class="leaderboard" id="leaderboard"></div>
  <button class="btn" onclick="Game.playAgain()">🔄 Play Again</button>
  <button class="btn" style="background:linear-gradient(135deg,#666,#333);margin-top:10px" onclick="Game.go('menu')">🏠 Main Menu</button>
</div>

<!-- ============ MODAL ============ -->
<div class="modal" id="modal">
  <div class="modal-box">
    <h3 id="modalTitle">Message</h3>
    <p id="modalText" style="margin-bottom:16px;color:#8899bb">...</p>
    <button class="btn" onclick="Game.closeModal()">OK</button>
  </div>
</div>

<script>
/* ============================================================
   AITT PLUS - LUDO GAME
   ============================================================ */

const Game = {

  /* ===== STATE ===== */
  state: {
    coins: 0,
    selectedAvatar: 0,
    currentPlayer: 0,
    diceValue: 0,
    diceRolled: false,
    gameOver: false,
    players: [
      {color:'#ff3355', pos:[0,0,0,0], home:0, name:'You',  finished:0},
      {color:'#3399ff', pos:[0,0,0,0], home:0, name:'AI 1', finished:0},
      {color:'#33cc66', pos:[0,0,0,0], home:0, name:'AI 2', finished:0},
      {color:'#ffcc00', pos:[0,0,0,0], home:0, name:'AI 3', finished:0}
    ]
  },

  avatars: [
    {e:'🦊',n:'Fox',vip:false},{e:'🐧',n:'Penguin',vip:false},{e:'🐢',n:'Turtle',vip:false},
    {e:'🦁',n:'Lion',vip:false},{e:'🐯',n:'Tiger',vip:true},{e:'🐺',n:'Wolf',vip:false},
    {e:'🐸',n:'Frog',vip:false},{e:'🦄',n:'Unicorn',vip:true},{e:'🐲',n:'Dragon',vip:true}
  ],

  /* ===== SCREENS ===== */
  go(id){
    document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
    const el = document.getElementById(id);
    if(el) el.classList.add('active');
    if(id==='game') setTimeout(()=>this.initBoard(),100);
  },

  /* ===== MENU ===== */
  quickMatch(){
    this.state.coins += 10;
    this.updateCoins();
    this.go('avatar');
  },

  updateCoins(){
    document.getElementById('coinDisplay').textContent = this.state.coins;
  },

  /* ===== AVATAR SELECT ===== */
  initAvatarGrid(){
    const grid = document.getElementById('avatarGrid');
    grid.innerHTML = '';
    this.avatars.forEach((a,i)=>{
      const div = document.createElement('div');
      div.className = 'avatar-item' + (a.vip?' vip':'') + (i===this.state.selectedAvatar?' selected':'');
      div.innerHTML = a.e + (a.vip?'<span class="crown">👑</span>':'');
      div.onclick = ()=>{
        this.state.selectedAvatar = i;
        this.initAvatarGrid();
      };
      grid.appendChild(div);
    });
  },

  confirmAvatar(){
    this.go('game');
  },

  /* ===== GAME BOARD ===== */
  canvas:null,
  ctx:null,
  cell:0,
  boardPath:[], // 52 cells
  homePaths:[], // 4 x 6

  initBoard(){
    this.canvas = document.getElementById('board');
    const wrap = this.canvas.parentElement;
    this.canvas.width = wrap.clientWidth;
    this.canvas.height = wrap.clientHeight;
    this.buildPaths();
    this.draw();
    this.updateTurn();
  },

  buildPaths(){
    const c = this.canvas.width;
    this.cell = c / 15; // 15x15 grid
    const u = this.cell;

    // Standard Ludo 52-cell path (simplified)
    // Red starts at bottom-left area, goes clockwise
    this.boardPath = [];
    const p = this.boardPath;

    // Bottom row going right (columns 6-8, row 13)
    for(let x=6;x<=8;x++) p.push({x, y:13});
    // Right side going up (col 13, rows 12-9)
    for(let y=12;y>=9;y--) p.push({x:13,y});
    // Right col going right (cols 14, row 8)
    for(let x=14;x>=9;x--) p.push({x, y:8});
    // Top right (row 6, cols 8-6)
    for(let x=8;x>=6;x--) p.push({x, y:6});
    // Right middle (col 6, rows 5-1)
    for(let y=5;y>=1;y--) p.push({x:6,y});
    // Top row going left (row 0, cols 5-0) - skip
    // Actually let's use a simpler approach: define 52 points manually

    // Reset - use simpler grid path
    p.length = 0;
    // Path definition (going clockwise from red start)
    const path = [
      // Red start (bottom, going right)
      [6,13],[7,13],[8,13],
      [9,13],[10,13],[11,13],
      [12,13],[13,12],[13,11],
      [13,10],[13,9],[12,9],
      [11,9],[10,9],[9,9],
      [8,9],[7,9],[6,9],
      [5,9],[4,9],[3,9],
      [2,9],[1,9],[0,9],
      [0,8],[0,7],[1,7],
      [2,7],[3,7],[4,7],
      [5,7],[6,7],[7,7],
      [7,6],[7,5],[7,4],
      [7,3],[7,2],[7,1],
      [7,0],[8,0],[9,0],
      [9,1],[9,2],[9,3],
      [9,4],[9,5],[9,6],
      [10,7],[11,7],[12,7],
      [13,7],[13,8]
    ];
    path.forEach(pt=>p.push({x:pt[0], y:pt[1]}));

    // Home paths (6 cells each leading to center)
    this.homePaths = [
      [{x:7,y:13},{x:7,y:12},{x:7,y:11},{x:7,y:10},{x:7,y:9},{x:7,y:8}], // Red
      [{x:13,y:7},{x:12,y:7},{x:11,y:7},{x:10,y:7},{x:9,y:7},{x:8,y:7}], // Blue
      [{x:7,y:1},{x:7,y:2},{x:7,y:3},{x:7,y:4},{x:7,y:5},{x:7,y:6}],   // Green
      [{x:1,y:7},{x:2,y:7},{x:3,y:7},{x:4,y:7},{x:5,y:7},{x:6,y:7}]    // Yellow
    ];

    // Start positions for each player on the path
    this.startPos = [0, 13, 26, 39];
  },

  draw(){
    const ctx = this.ctx = this.canvas.getContext('2d');
    const c = this.canvas.width;
    const u = this.cell;

    ctx.clearRect(0,0,c,c);

    // Draw board background
    ctx.fillStyle = 'rgba(10,20,40,0.6)';
    ctx.fillRect(0,0,c,c);

    // Draw center home
    ctx.fillStyle = '#0a1428';
    ctx.strokeStyle = '#00d4ff';
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.arc(c/2, c/2, u*1.5, 0, Math.PI*2);
    ctx.fill();
    ctx.stroke();

    // Center logo
    ctx.fillStyle = '#00d4ff';
    ctx.font = `bold ${u*0.6}px Orbitron, sans-serif`;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillText('AITT', c/2, c/2);

    // Draw home bases (4 corners)
    const bases = [
      {x:0, y:9, color:'#ff3355'},     // Red
      {x:9, y:9, color:'#3399ff'},     // Blue
      {x:9, y:0, color:'#33cc66'},     // Green
      {x:0, y:0, color:'#ffcc00'}      // Yellow
    ];
    bases.forEach(b=>{
      ctx.fillStyle = b.color + '44';
      ctx.strokeStyle = b.color;
      ctx.lineWidth = 2;
      ctx.fillRect(b.x*u, b.y*u, u*6, u*6);
      ctx.strokeRect(b.x*u, b.y*u, u*6, u*6);

      // 4 circles inside
      for(let i=0;i<4;i++){
        const cx = b.x*u + u*(1.5 + (i%2)*3);
        const cy = b.y*u + u*(1.5 + Math.floor(i/2)*3);
        ctx.beginPath();
        ctx.arc(cx, cy, u*0.7, 0, Math.PI*2);
        ctx.fillStyle = b.color + '66';
        ctx.fill();
        ctx.strokeStyle = b.color;
        ctx.lineWidth = 1.5;
        ctx.stroke();
      }
    });

    // Draw path cells
    this.boardPath.forEach((pt,i)=>{
      const isStart = this.startPos.includes(i);
      const color = isStart ? ['#ff3355','#3399ff','#33cc66','#ffcc00'][this.startPos.indexOf(i)] : null;
      ctx.fillStyle = color || '#1a2a4a';
      ctx.strokeStyle = color || '#00d4ff44';
      ctx.lineWidth = 1;
      ctx.fillRect(pt.x*u, pt.y*u, u, u);
      ctx.strokeRect(pt.x*u, pt.y*u, u, u);
    });

    // Draw home paths
    this.homePaths.forEach((hp,pi)=>{
      const color = ['#ff3355','#3399ff','#33cc66','#ffcc00'][pi];
      hp.forEach(pt=>{
        ctx.fillStyle = color + '99';
        ctx.strokeStyle = color;
        ctx.lineWidth = 1;
        ctx.fillRect(pt.x*u, pt.y*u, u, u);
        ctx.strokeRect(pt.x*u, pt.y*u, u, u);
      });
    });

    // Draw pieces
    this.drawPieces();
  },

  drawPieces(){
    const ctx = this.ctx;
    const u = this.cell;

    this.state.players.forEach((p,pi)=>{
      p.pos.forEach((pos,pieceIdx)=>{
        let x, y;
        if(pos === 0){
          // In home base
          const bases = [{x:0,y:9},{x:9,y:9},{x:9,y:0},{x:0,y:0}];
          const b = bases[pi];
          x = b.x*u + u*(1.5 + (pieceIdx%2)*3);
          y = b.y*u + u*(1.5 + Math.floor(pieceIdx/2)*3);
        } else if(pos >= 100){
          // Finished
          x = 7.5*u;
          y = 7.5*u;
        } else if(pos > 52){
          // In home path
          const hp = this.homePaths[pi];
          const idx = Math.min(pos - 53, 5);
          x = hp[idx].x*u + u/2;
          y = hp[idx].y*u + u/2;
        } else {
          // On path
          const pt = this.boardPath[(this.startPos[pi] + pos - 1) % 52];
          x = pt.x*u + u/2;
          y = pt.y*u + u/2;
        }

        // Glow
        ctx.shadowColor = p.color;
        ctx.shadowBlur = 15;

        // Circle
        ctx.beginPath();
        ctx.arc(x, y, u*0.35, 0, Math.PI*2);
        ctx.fillStyle = p.color;
        ctx.fill();
        ctx.strokeStyle = '#fff';
        ctx.lineWidth = 2;
        ctx.stroke();

        // Inner dot
        ctx.beginPath();
        ctx.arc(x, y, u*0.15, 0, Math.PI*2);
        ctx.fillStyle = '#fff';
        ctx.fill();

        ctx.shadowBlur = 0;
      });
    });
  },

  /* ===== GAME LOGIC ===== */
  rollDice(){
    if(this.state.diceRolled || this.state.gameOver) return;

    const btn = document.getElementById('diceBtn');
    btn.classList.add('rolling');

    setTimeout(()=>{
      const value = Math.floor(Math.random()*6)+1;
      this.state.diceValue = value;
      const faces = ['⚀','⚁','⚂','⚃','⚄','⚅'];
      document.getElementById('diceFace').textContent = faces[value-1];
      btn.classList.remove('rolling');
      this.state.diceRolled = true;
      this.handleRoll(value);
    }, 1000);
  },

  handleRoll(value){
    if(this.state.currentPlayer === 0){
      // Human player
      const moved = this.tryMove(0, value);
      if(!moved){
        this.showModal('لا حركة', 'لا توجد قطع يمكن تحريكها');
        setTimeout(()=>this.nextTurn(), 1500);
      } else {
        this.state.diceRolled = false;
        setTimeout(()=>this.nextTurn(), 1500);
      }
    } else {
      this.aiMove(this.state.currentPlayer, value);
    }
    this.draw();
  },

  tryMove(player, value){
    const p = this.state.players[player];
    // Find movable piece
    for(let i=0;i<4;i++){
      if(p.pos[i] === 0 && value === 6) return true;
      if(p.pos[i] > 0 && p.pos[i] < 100 && p.pos[i] + value <= 58) return true;
    }
    return false;
  },

  aiMove(player, value){
    const p = this.state.players[player];
    // Simple AI
    let bestPiece = -1;
    let bestScore = -1;

    for(let i=0;i<4;i++){
      const pos = p.pos[i];
      let score = 0;

      if(pos === 0 && value === 6) score = 50;
      else if(pos > 0 && pos + value <= 58) score = 30 + (pos + value);
      else continue;

      if(score > bestScore){
        bestScore = score;
        bestPiece = i;
      }
    }

    if(bestPiece === -1){
      // No move
      setTimeout(()=>this.nextTurn(), 800);
      return;
    }

    setTimeout(()=>{
      this.movePiece(player, bestPiece, value);
      this.draw();
      setTimeout(()=>this.nextTurn(), 800);
    }, 1000);
  },

  movePiece(player, pieceIdx, value){
    const p = this.state.players[player];
    const pos = p.pos[pieceIdx];

    if(pos === 0 && value === 6){
      p.pos[pieceIdx] = 1;
    } else if(pos > 0 && pos + value <= 58){
      p.pos[pieceIdx] = pos + value;
      if(p.pos[pieceIdx] === 58){
        p.finished++;
        p.pos[pieceIdx] = 100; // finished
      }
    }

    this.updateScores();

    // Check win
    if(p.finished === 4){
      this.endGame(player);
    }
  },

  updateScores(){
    this.state.players.forEach((p,i)=>{
      document.getElementById('psc-'+i).textContent = p.finished;
    });
  },

  nextTurn(){
    if(this.state.gameOver) return;
    this.state.diceRolled = false;
    this.state.currentPlayer = (this.state.currentPlayer + 1) % 4;
    this.updateTurn();

    // If AI, auto roll
    if(this.state.currentPlayer !== 0){
      setTimeout(()=>this.rollDice(), 1000);
   