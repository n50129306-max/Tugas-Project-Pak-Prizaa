<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AlgoViz Pro • Visualisasi Struktur Data & Algoritma Interaktif</title>
  <style>
    :root {
      --bg: #0a0a14;
      --surface: #12122a;
      --surface2: #1a1a3e;
      --primary: #7c5cfc;
      --primary2: #a78bfa;
      --accent: #00d4ff;
      --accent2: #ff6bcb;
      --text: #e0e0f0;
      --text2: #a0a0c0;
      --danger: #ff4757;
      --success: #2ed573;
      --warning: #ffa502;
      --glass: rgba(20, 20, 50, 0.65);
      --glass-border: rgba(255, 255, 255, 0.08);
      --radius: 18px;
      --radius-sm: 10px;
      --shadow: 0 8px 40px rgba(0,0,0,0.5);
      --transition: 0.35s cubic-bezier(0.4,0,0.2,1);
      --font: 'Segoe UI', 'Inter', system-ui, -apple-system, sans-serif;
    }
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      font-family: var(--font);
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
      -webkit-font-smoothing: antialiased;
    }
    #particles {
      position: fixed; top:0; left:0; width:100%; height:100%;
      pointer-events: none; z-index:0;
    }
    .particle {
      position: absolute; border-radius:50%;
      animation: floatUp linear infinite; opacity:0.5; pointer-events:none;
    }
    @keyframes floatUp {
      0% { transform:translateY(105vh) scale(0) rotate(0deg); opacity:0; }
      5% { opacity:0.7; }
      80% { opacity:0.3; }
      100% { transform:translateY(-10vh) scale(1.4) rotate(360deg); opacity:0; }
    }
    .glow-orb {
      position: fixed; border-radius:50%; filter:blur(120px);
      pointer-events:none; z-index:0; opacity:0.18;
      animation: orbPulse 8s ease-in-out infinite;
    }
    .glow-orb.orb1 { width:500px; height:500px; background:var(--primary); top:-15%; left:-10%; animation-delay:0s; }
    .glow-orb.orb2 { width:400px; height:400px; background:var(--accent2); bottom:-12%; right:-8%; animation-delay:-4s; }
    .glow-orb.orb3 { width:350px; height:350px; background:var(--accent); top:50%; left:50%; transform:translate(-50%,-50%); animation-delay:-6s; opacity:0.1; }
    @keyframes orbPulse {
      0%,100% { transform:translate(0,0) scale(1); }
      25% { transform:translate(40px,-30px) scale(1.15); }
      50% { transform:translate(-20px,20px) scale(0.9); }
      75% { transform:translate(-35px,-15px) scale(1.1); }
    }
    #loginOverlay {
      position:fixed; inset:0; z-index:1000; display:flex; align-items:center; justify-content:center;
      background:rgba(6,6,20,0.92); backdrop-filter:blur(20px); transition: all 0.6s ease;
    }
    #loginOverlay.hidden { opacity:0; pointer-events:none; transform:scale(1.08); }
    .login-card {
      background:var(--glass); border:1px solid var(--glass-border); border-radius:var(--radius);
      padding:45px 40px; width:420px; max-width:92vw; box-shadow:var(--shadow), 0 0 80px rgba(124,92,252,0.25);
      backdrop-filter:blur(30px); text-align:center; animation: cardEntry 0.7s ease-out;
    }
    @keyframes cardEntry { from{opacity:0; transform:translateY(40px) scale(0.9);} to{opacity:1; transform:translateY(0) scale(1);} }
    .login-card .logo { font-size:3rem; margin-bottom:8px; animation: logoBounce 2s ease-in-out infinite; }
    @keyframes logoBounce { 0%,100%{transform:translateY(0);} 30%{transform:translateY(-14px);} 50%{transform:translateY(0);} 70%{transform:translateY(-7px);} }
    .login-card h1 { font-size:1.8rem; font-weight:700; background:linear-gradient(135deg,var(--accent),var(--primary2),var(--accent2)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; margin-bottom:6px; }
    .login-card .subtitle { color:var(--text2); font-size:0.9rem; margin-bottom:28px; }
    .login-card input { width:100%; padding:14px 18px; margin:8px 0; border-radius:var(--radius-sm); border:1px solid rgba(255,255,255,0.15); background:rgba(255,255,255,0.04); color:var(--text); font-size:1rem; transition:var(--transition); font-family:var(--font); }
    .login-card input:focus { outline:none; border-color:var(--primary2); box-shadow:0 0 25px rgba(124,92,252,0.3); background:rgba(255,255,255,0.07); }
    .login-card .btn-login { width:100%; padding:14px; margin-top:16px; border:none; border-radius:var(--radius-sm); background:linear-gradient(135deg,var(--primary),#6c3ce0); color:#fff; font-size:1.05rem; font-weight:600; cursor:pointer; transition:var(--transition); letter-spacing:0.5px; position:relative; overflow:hidden; }
    .login-card .btn-login:hover { transform:translateY(-2px); box-shadow:0 12px 35px rgba(124,92,252,0.45); }
    .login-card .btn-login:active { transform:scale(0.96); }
    .login-error { color:var(--danger); font-size:0.85rem; min-height:20px; margin-top:4px; }
    #mainApp { display:flex; min-height:100vh; position:relative; z-index:1; opacity:0; transition:opacity 0.5s; }
    #mainApp.visible { opacity:1; }
    .sidebar { width:260px; background:var(--glass); border-right:1px solid var(--glass-border); backdrop-filter:blur(25px); padding:24px 16px; display:flex; flex-direction:column; gap:4px; position:sticky; top:0; height:100vh; z-index:10; flex-shrink:0; transition:all 0.4s ease; }
    .sidebar .side-logo { text-align:center; margin-bottom:16px; font-weight:700; font-size:1.3rem; background:linear-gradient(135deg,var(--accent),var(--primary2)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; }
    .sidebar .nav-item { display:flex; align-items:center; gap:12px; padding:13px 18px; border-radius:var(--radius-sm); cursor:pointer; transition:var(--transition); color:var(--text2); font-weight:500; font-size:0.95rem; text-decoration:none; user-select:none; }
    .sidebar .nav-item:hover { background:rgba(255,255,255,0.05); color:var(--text); transform:translateX(4px); }
    .sidebar .nav-item.active { background:rgba(124,92,252,0.2); color:#fff; box-shadow:0 0 20px rgba(124,92,252,0.2); font-weight:600; }
    .sidebar .nav-item.active::before { content:''; position:absolute; left:0; top:12px; bottom:12px; width:3px; border-radius:0 4px 4px 0; background:var(--accent); }
    .sidebar .nav-icon { font-size:1.3rem; width:28px; text-align:center; }
    .sidebar .logout-btn { margin-top:auto; padding:13px 18px; border-radius:var(--radius-sm); cursor:pointer; transition:var(--transition); color:var(--danger); font-weight:500; display:flex; align-items:center; gap:12px; font-size:0.95rem; }
    .sidebar .logout-btn:hover { background:rgba(255,71,87,0.12); }
    .content { flex:1; padding:30px 36px; overflow-y:auto; max-height:100vh; position:relative; z-index:1; }
    .page { display:none; animation: fadeSlideIn 0.5s ease-out; }
    .page.active { display:block; }
    @keyframes fadeSlideIn { from{opacity:0; transform:translateY(20px);} to{opacity:1; transform:translateY(0);} }
    .card { background:var(--glass); border:1px solid var(--glass-border); border-radius:var(--radius); padding:28px 30px; backdrop-filter:blur(20px); box-shadow:var(--shadow); margin-bottom:20px; transition:var(--transition); }
    .card:hover { box-shadow:0 12px 50px rgba(0,0,0,0.55); border-color:rgba(255,255,255,0.14); }
    .card h2 { font-size:1.4rem; font-weight:700; margin-bottom:6px; }
    .card h3 { font-size:1.1rem; font-weight:600; margin-bottom:14px; color:var(--text2); }
    .card p { color:var(--text2); line-height:1.7; font-size:0.95rem; }
    .badge { display:inline-block; padding:5px 12px; border-radius:20px; font-size:0.78rem; font-weight:600; background:rgba(124,92,252,0.2); color:var(--primary2); margin:3px 4px; }
    .badge.accent { background:rgba(0,212,255,0.15); color:var(--accent); }
    .btn { padding:11px 22px; border-radius:25px; border:none; cursor:pointer; font-weight:600; font-size:0.9rem; transition:var(--transition); font-family:var(--font); letter-spacing:0.3px; display:inline-flex; align-items:center; gap:7px; }
    .btn-primary { background:linear-gradient(135deg,var(--primary),#6c3ce0); color:#fff; box-shadow:0 4px 20px rgba(124,92,252,0.35); }
    .btn-primary:hover { transform:translateY(-3px); box-shadow:0 8px 30px rgba(124,92,252,0.5); }
    .btn-accent { background:linear-gradient(135deg,var(--accent),#009ec5); color:#000; box-shadow:0 4px 20px rgba(0,212,255,0.3); }
    .btn-accent:hover { transform:translateY(-3px); box-shadow:0 8px 30px rgba(0,212,255,0.45); }
    .btn-outline { background:transparent; border:1.5px solid rgba(255,255,255,0.3); color:var(--text); }
    .btn-outline:hover { border-color:var(--primary2); background:rgba(124,92,252,0.1); }
    .btn-sm { padding:7px 15px; font-size:0.8rem; }
    .btn:active { transform:scale(0.94); }
    .btn:disabled { opacity:0.5; pointer-events:none; }
    .bar-container { display:flex; align-items:flex-end; gap:3px; height:300px; padding:10px 0; overflow-x:auto; }
    .bar { flex:1; min-width:8px; border-radius:5px 5px 0 0; transition:all 0.15s ease; background:linear-gradient(180deg,var(--primary2),var(--primary)); position:relative; }
    .bar.comparing { background:linear-gradient(180deg,#ff6bcb,#ff4088)!important; box-shadow:0 0 18px rgba(255,107,203,0.7); animation:pulseBar 0.5s infinite; }
    .bar.swapping { background:linear-gradient(180deg,#ffa502,#ff6b00)!important; box-shadow:0 0 18px rgba(255,165,2,0.7); animation:shakeBar 0.4s ease; }
    .bar.sorted { background:linear-gradient(180deg,#2ed573,#1db954)!important; box-shadow:0 0 12px rgba(46,213,115,0.5); }
    .bar.pivot { background:linear-gradient(180deg,#ff4757,#e03040)!important; box-shadow:0 0 20px rgba(255,71,87,0.8); animation:glowPivot 0.7s infinite; }
    @keyframes pulseBar { 0%,100%{transform:scaleY(1);} 50%{transform:scaleY(1.04);} }
    @keyframes shakeBar { 0%,100%{transform:translateX(0);} 25%{transform:translateX(-4px);} 75%{transform:translateX(4px);} }
    @keyframes glowPivot { 0%,100%{box-shadow:0 0 20px rgba(255,71,87,0.8);} 50%{box-shadow:0 0 40px rgba(255,71,87,1.3);} }
    .bar-value { text-align:center; font-size:0.7rem; font-weight:600; color:#fff; margin-top:-22px; }
    .stack-visual, .queue-visual { display:flex; gap:6px; align-items:center; min-height:120px; flex-wrap:wrap; padding:10px; }
    .stack-visual { flex-direction:column-reverse; align-items:stretch; }
    .queue-visual { flex-direction:row; }
    .stack-item, .queue-item { background:linear-gradient(135deg,var(--primary),#5a3fd4); color:#fff; padding:14px 22px; border-radius:var(--radius-sm); font-weight:700; text-align:center; transition:all 0.35s ease; animation:popIn 0.4s ease-out; box-shadow:0 4px 15px rgba(124,92,252,0.3); min-width:50px; }
    @keyframes popIn { from{transform:scale(0);opacity:0;} to{transform:scale(1);opacity:1;} }
    .stack-item.top-item, .queue-item.front-item { border:2px solid var(--accent); box-shadow:0 0 25px rgba(0,212,255,0.6); }
    .input-group { display:flex; gap:10px; flex-wrap:wrap; align-items:center; }
    .input-group input, .input-group select { padding:11px 16px; border-radius:25px; border:1px solid rgba(255,255,255,0.2); background:rgba(255,255,255,0.05); color:var(--text); font-size:0.9rem; font-family:var(--font); transition:var(--transition); }
    .input-group input:focus, .input-group select:focus { outline:none; border-color:var(--primary2); box-shadow:0 0 20px rgba(124,92,252,0.25); }
    .stats-row { display:flex; gap:16px; flex-wrap:wrap; margin:16px 0; }
    .stat-box { flex:1; min-width:100px; background:var(--glass); border:1px solid var(--glass-border); border-radius:var(--radius-sm); padding:16px 20px; text-align:center; backdrop-filter:blur(10px); }
    .stat-box .stat-val { font-size:1.8rem; font-weight:700; background:linear-gradient(135deg,var(--accent),var(--primary2)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; }
    .stat-box .stat-label { font-size:0.78rem; color:var(--text2); margin-top:2px; text-transform:uppercase; letter-spacing:1px; }
    .profile-avatar { width:100px; height:100px; border-radius:50%; background:linear-gradient(135deg,var(--primary),var(--accent2)); display:flex; align-items:center; justify-content:center; font-size:2.8rem; margin:0 auto 16px; box-shadow:0 8px 35px rgba(124,92,252,0.4); animation:avatarGlow 3s infinite; }
    @keyframes avatarGlow { 0%,100%{box-shadow:0 8px 35px rgba(124,92,252,0.4);} 50%{box-shadow:0 8px 55px rgba(255,107,203,0.55);} }
    .profile-form input, .profile-form textarea { width:100%; padding:12px 16px; margin:7px 0; border-radius:var(--radius-sm); border:1px solid rgba(255,255,255,0.18); background:rgba(255,255,255,0.04); color:var(--text); font-family:var(--font); font-size:0.9rem; transition:var(--transition); }
    .profile-form input:focus, .profile-form textarea:focus { outline:none; border-color:var(--primary2); }
    .toast-container { position:fixed; top:20px; right:20px; z-index:2000; display:flex; flex-direction:column; gap:10px; }
    .toast { padding:14px 22px; border-radius:25px; color:#fff; font-weight:600; font-size:0.9rem; animation:slideInRight 0.4s ease-out, fadeOutRight 0.4s 2.5s forwards; backdrop-filter:blur(15px); box-shadow:0 8px 30px rgba(0,0,0,0.4); }
    .toast.success{background:rgba(46,213,115,0.8);} .toast.info{background:rgba(0,212,255,0.8);} .toast.warning{background:rgba(255,165,2,0.8);}
    @keyframes slideInRight { from{transform:translateX(120%);opacity:0;} to{transform:translateX(0);opacity:1;} }
    @keyframes fadeOutRight { from{transform:translateX(0);opacity:1;} to{transform:translateX(120%);opacity:0;} }
    .speed-slider { -webkit-appearance:none; width:160px; height:8px; border-radius:4px; background:rgba(255,255,255,0.15); outline:none; cursor:pointer; }
    .speed-slider::-webkit-slider-thumb { -webkit-appearance:none; width:24px; height:24px; border-radius:50%; background:var(--accent); cursor:pointer; box-shadow:0 0 15px rgba(0,212,255,0.5); }
    @media (max-width:768px){
      .sidebar{width:70px;padding:16px 8px;} .sidebar .nav-item span,.side-logo,.logout-btn span{display:none;} .sidebar .nav-item{justify-content:center;padding:14px;}
      .content{padding:20px 16px;} .bar-container{height:200px;} .stats-row{flex-direction:column;}
    }
    ::-webkit-scrollbar{width:6px;height:6px;} ::-webkit-scrollbar-track{background:transparent;} ::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.12);border-radius:3px;}
    .sound-toggle { cursor:pointer; }
  </style>
</head>
<body>
  <div class="glow-orb orb1"></div><div class="glow-orb orb2"></div><div class="glow-orb orb3"></div>
  <div id="particles"></div>
  <div class="toast-container" id="toastContainer"></div>

  <!-- LOGIN -->
  <div id="loginOverlay">
    <div class="login-card">
      <div class="logo">🧠</div>
      <h1>AlgoViz Pro</h1>
      <p class="subtitle">Visualisasi Struktur Data & Algoritma Interaktif</p>
      <input type="text" id="loginUser" placeholder="👤 Username" autocomplete="off">
      <input type="password" id="loginPass" placeholder="🔒 Password">
      <div class="login-error" id="loginError"></div>
      <button class="btn-login" onclick="handleLogin()">🚀 Masuk ke Dashboard</button>
      <p style="font-size:0.7rem;color:var(--text2);margin-top:12px;">Demo: admin / admin123</p>
    </div>
  </div>

  <!-- MAIN APP -->
  <div id="mainApp">
    <nav class="sidebar">
      <div class="side-logo">🧠 AlgoViz</div>
      <div class="nav-item active" data-page="dashboard" onclick="navigateTo('dashboard',this)"><span class="nav-icon">🏠</span> <span>Dashboard</span></div>
      <div class="nav-item" data-page="sorting" onclick="navigateTo('sorting',this)"><span class="nav-icon">📊</span> <span>Sorting Visualizer</span></div>
      <div class="nav-item" data-page="search" onclick="navigateTo('search',this)"><span class="nav-icon">🔍</span> <span>Search Algorithms</span></div>
      <div class="nav-item" data-page="stack" onclick="navigateTo('stack',this)"><span class="nav-icon">📚</span> <span>Stack Simulator</span></div>
      <div class="nav-item" data-page="queue" onclick="navigateTo('queue',this)"><span class="nav-icon">🚶</span> <span>Queue Simulator</span></div>
      <div class="nav-item" data-page="profile" onclick="navigateTo('profile',this)"><span class="nav-icon">👤</span> <span>Profil Saya</span></div>
      <div class="nav-item sound-toggle" id="soundToggleBtn" onclick="toggleSound()"><span class="nav-icon">🔊</span> <span>Suara</span></div>
      <div class="logout-btn" onclick="handleLogout()"><span class="nav-icon">🚪</span> <span>Logout</span></div>
    </nav>

    <div class="content">
      <!-- DASHBOARD -->
      <div class="page active" id="page-dashboard">
        <div class="card" style="text-align:center;">
          <h2>✨ Selamat Datang di AlgoViz Pro</h2>
          <p style="max-width:700px;margin:0 auto;">Platform <strong>visualisasi interaktif</strong> untuk mempelajari struktur data dan algoritma. Saksikan setiap langkah proses sorting, searching, stack, dan queue secara <em>real-time</em> dengan animasi hidup dan penjelasan detail. Desain modern, elegan, penuh efek kreatif agar belajar lebih menyenangkan.</p>
          <div style="margin-top:18px;">
            <span class="badge">🔢 6 Algoritma Sorting</span>
            <span class="badge accent">🔍 2 Metode Search</span>
            <span class="badge">📚 Stack</span>
            <span class="badge accent">🚶 Queue</span>
            <span class="badge">⚡ Custom Array</span>
            <span class="badge accent">🔊 Efek Suara</span>
          </div>
        </div>
        <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:18px;">
          <div class="card" style="text-align:center;"><span style="font-size:3rem;">📊</span><h3>Sorting Visualizer</h3><p>Bubble, Selection, Insertion, <strong>Shell</strong>, <strong>Merge</strong>, & <strong>Quick Sort</strong> dengan animasi dan penjelasan setiap pertukaran.</p></div>
          <div class="card" style="text-align:center;"><span style="font-size:3rem;">🔍</span><h3>Search Algorithms</h3><p>Linear & Binary Search interaktif, bisa pakai array kustom.</p></div>
          <div class="card" style="text-align:center;"><span style="font-size:3rem;">📚</span><h3>Stack Simulator</h3><p>Push & Pop dengan animasi LIFO yang responsif.</p></div>
          <div class="card" style="text-align:center;"><span style="font-size:3rem;">🚶</span><h3>Queue Simulator</h3><p>Enqueue & Dequeue FIFO dengan visual antrian.</p></div>
        </div>
      </div>

      <!-- SORTING PAGE -->
      <div class="page" id="page-sorting">
        <div class="card">
          <h2>📊 Sorting Visualizer</h2>
          <p style="color:var(--text2);">Pilih algoritma, atur array (acak atau custom), lalu saksikan proses sorting langkah demi langkah beserta penjelasan pertukaran angka.</p>
          <div class="input-group" style="margin-top:14px;">
            <select id="sortAlgo">
              <option value="bubble">🫧 Bubble Sort</option>
              <option value="selection">🎯 Selection Sort</option>
              <option value="insertion">📌 Insertion Sort</option>
              <option value="shell">🐚 Shell Sort</option>
              <option value="merge">🔗 Merge Sort</option>
              <option value="quick">⚡ Quick Sort</option>
            </select>
            <input type="number" id="sortSize" value="30" min="5" max="80" placeholder="Ukuran" style="width:90px;">
            <button class="btn btn-primary" onclick="generateSortArray()">🎲 Generate Acak</button>
            <input type="text" id="sortCustomInput" placeholder="Misal: 12,7,23,45" style="width:180px;">
            <button class="btn btn-outline btn-sm" onclick="setSortCustomArray()">✏️ Pakai Array</button>
            <span style="color:var(--text2);">Kecepatan:</span>
            <input type="range" class="speed-slider" id="sortSpeed" min="10" max="300" value="80">
            <span id="speedLabel" style="color:var(--accent);">80ms</span>
            <button class="btn btn-accent" id="btnStartSort" onclick="startSorting()">▶️ Mulai Sorting</button>
            <button class="btn btn-outline btn-sm" onclick="stopSorting()">⏹ Stop</button>
          </div>
          <div class="stats-row">
            <div class="stat-box"><div class="stat-val" id="sortComparisons">0</div><div class="stat-label">Perbandingan</div></div>
            <div class="stat-box"><div class="stat-val" id="sortSwaps">0</div><div class="stat-label">Pertukaran</div></div>
            <div class="stat-box"><div class="stat-val" id="sortSteps">0</div><div class="stat-label">Langkah</div></div>
            <div class="stat-box"><div class="stat-val" id="sortStatus">⚪ Idle</div><div class="stat-label">Status</div></div>
          </div>
          <div class="bar-container" id="barContainer"></div>
          <p style="font-size:0.9rem;color:var(--text2);text-align:center;min-height:24px;" id="sortDescription"></p>
        </div>
      </div>

      <!-- SEARCH PAGE -->
      <div class="page" id="page-search">
        <div class="card">
          <h2>🔍 Search Algorithms</h2>
          <p style="color:var(--text2);">Cari nilai dalam array (kustom atau acak) dan lihat proses pencariannya.</p>
          <div class="input-group" style="margin-top:14px;">
            <select id="searchAlgo"><option value="linear">🔎 Linear Search</option><option value="binary">📖 Binary Search</option></select>
            <input type="number" id="searchTarget" value="42" placeholder="Nilai dicari" style="width:110px;">
            <button class="btn btn-primary" onclick="generateSearchArray()">🎲 Generate Acak</button>
            <input type="text" id="searchCustomInput" placeholder="Misal: 5,10,15,20" style="width:180px;">
            <button class="btn btn-outline btn-sm" onclick="setSearchCustomArray()">✏️ Pakai Array</button>
            <button class="btn btn-accent" id="btnStartSearch" onclick="startSearching()">▶️ Mulai Cari</button>
          </div>
          <div class="bar-container" id="searchBarContainer" style="height:200px;"></div>
          <p style="font-size:0.9rem;color:var(--text2);text-align:center;" id="searchResult"></p>
        </div>
      </div>

      <!-- STACK PAGE -->
      <div class="page" id="page-stack">
        <div class="card">
          <h2>📚 Stack Simulator (LIFO)</h2>
          <div class="input-group"><input type="number" id="stackInput" placeholder="Nilai"><button class="btn btn-primary" onclick="stackPush()">➕ Push</button><button class="btn btn-accent" onclick="stackPop()">➖ Pop</button><button class="btn btn-outline btn-sm" onclick="stackPeek()">👀 Peek</button><button class="btn btn-outline btn-sm" onclick="stackClear()">🗑 Clear</button></div>
          <p><strong>Top:</strong> <span id="stackTopInfo">-</span> | <strong>Size:</strong> <span id="stackSizeInfo">0</span></p>
          <div class="stack-visual" id="stackVisual"></div>
        </div>
      </div>

      <!-- QUEUE PAGE -->
      <div class="page" id="page-queue">
        <div class="card">
          <h2>🚶 Queue Simulator (FIFO)</h2>
          <div class="input-group"><input type="number" id="queueInput" placeholder="Nilai"><button class="btn btn-primary" onclick="queueEnqueue()">➕ Enqueue</button><button class="btn btn-accent" onclick="queueDequeue()">➖ Dequeue</button><button class="btn btn-outline btn-sm" onclick="queueFront()">👀 Front</button><button class="btn btn-outline btn-sm" onclick="queueClear()">🗑 Clear</button></div>
          <p><strong>Front:</strong> <span id="queueFrontInfo">-</span> | <strong>Rear:</strong> <span id="queueRearInfo">-</span> | <strong>Size:</strong> <span id="queueSizeInfo">0</span></p>
          <div class="queue-visual" id="queueVisual"></div>
        </div>
      </div>

      <!-- PROFILE PAGE -->
      <div class="page" id="page-profile">
        <div class="card" style="max-width:550px;margin:0 auto;text-align:center;">
          <div class="profile-avatar" id="profileAvatar">👤</div>
          <h2>Profil Saya</h2>
          <div class="profile-form"><input type="text" id="profileName" placeholder="Nama"><input type="email" id="profileEmail" placeholder="Email"><textarea id="profileBio" rows="3" placeholder="Bio"></textarea><button class="btn btn-primary" onclick="saveProfile()">💾 Simpan</button></div>
        </div>
      </div>
    </div>
  </div>

  <script>
    (function() {
      // ========== AUDIO SYSTEM ==========
      let soundEnabled = true;
      const AudioContext = window.AudioContext || window.webkitAudioContext;
      let audioCtx = null;
      function getAudioCtx() {
        if (!audioCtx) audioCtx = new AudioContext();
        if (audioCtx.state === 'suspended') audioCtx.resume();
        return audioCtx;
      }
      function playBeep(freq, dur, type='sine', vol=0.1) {
        if (!soundEnabled) return;
        try {
          const ctx = getAudioCtx();
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = type; osc.frequency.value = freq;
          gain.gain.value = vol;
          osc.connect(gain); gain.connect(ctx.destination);
          osc.start(); osc.stop(ctx.currentTime + dur);
        } catch(e) {}
      }
      function playClick() { playBeep(800, 0.07, 'square', 0.12); }
      function playTick() { playBeep(600, 0.05, 'sine', 0.08); }
      function playSuccess() { playBeep(1000, 0.1, 'sine', 0.13); setTimeout(()=>playBeep(1200,0.1,'sine',0.13), 100); }
      window.toggleSound = function() {
        soundEnabled = !soundEnabled;
        const icon = document.querySelector('#soundToggleBtn .nav-icon');
        if (icon) icon.textContent = soundEnabled ? '🔊' : '🔇';
      };

      // Global click sound for all buttons
      document.addEventListener('click', function(e) {
        if (e.target.closest('button')) playClick();
      });

      // ========== PARTICLES ==========
      function createParticles() {
        const c = document.getElementById('particles');
        const colors = ['#7c5cfc','#a78bfa','#00d4ff','#ff6bcb','#2ed573','#ffa502','#fff'];
        for(let i=0;i<50;i++){
          const p=document.createElement('div'); p.className='particle';
          p.style.left=Math.random()*100+'%'; p.style.width=p.style.height=(Math.random()*4+2)+'px';
          p.style.background=colors[Math.floor(Math.random()*colors.length)];
          p.style.animationDuration=(Math.random()*12+8)+'s'; p.style.animationDelay=(Math.random()*10)+'s';
          c.appendChild(p);
        }
      }

      // ========== TOAST ==========
      function showToast(msg,type='info'){
        const container=document.getElementById('toastContainer');
        const t=document.createElement('div'); t.className=`toast ${type}`; t.textContent=msg;
        container.appendChild(t); setTimeout(()=>t.remove(),3000);
      }

      // ========== LOGIN ==========
      let currentUser=null;
      window.handleLogin = function(){
        const u=document.getElementById('loginUser').value.trim();
        const p=document.getElementById('loginPass').value.trim();
        const err=document.getElementById('loginError');
        if(!u||!p){ err.textContent='⚠️ Isi username & password.'; return; }
        if(u==='admin' && p==='admin123'){
          currentUser={username:u,name:'Administrator',email:'admin@algoviz.pro',bio:'Pengguna demo.'};
          err.textContent='';
          document.getElementById('loginOverlay').classList.add('hidden');
          document.getElementById('mainApp').classList.add('visible');
          loadProfile(); generateSortArray(); generateSearchArray(); updateStackVisual(); updateQueueVisual();
          showToast('✅ Login berhasil!','success');
        }else{
          err.textContent='❌ Username/password salah.';
          document.querySelector('.login-card').style.animation='none';
          document.querySelector('.login-card').offsetHeight;
          document.querySelector('.login-card').style.animation='cardEntry 0.5s ease-out';
        }
      };
      document.getElementById('loginPass').addEventListener('keydown',e=>{if(e.key==='Enter')handleLogin();});
      document.getElementById('loginUser').addEventListener('keydown',e=>{if(e.key==='Enter')document.getElementById('loginPass').focus();});

      window.handleLogout = function(){
        currentUser=null;
        document.getElementById('loginOverlay').classList.remove('hidden');
        document.getElementById('mainApp').classList.remove('visible');
        stopSorting(); stopSearching();
        showToast('👋 Logout berhasil.','info');
      };

      // ========== NAVIGATION ==========
      window.navigateTo = function(page,el){
        document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
        document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
        const pg=document.getElementById('page-'+page);
        if(pg) pg.classList.add('active');
        if(el) el.classList.add('active');
        stopSorting(); stopSearching();
      };

      // ========== PROFILE ==========
      function loadProfile(){
        const saved=localStorage.getItem('algoviz_profile');
        if(saved){
          const d=JSON.parse(saved);
          document.getElementById('profileName').value=d.name||'';
          document.getElementById('profileEmail').value=d.email||'';
          document.getElementById('profileBio').value=d.bio||'';
          document.getElementById('profileAvatar').textContent=d.name?d.name.charAt(0).toUpperCase():'👤';
        }
      }
      window.saveProfile = function(){
        const d={
          name:document.getElementById('profileName').value.trim(),
          email:document.getElementById('profileEmail').value.trim(),
          bio:document.getElementById('profileBio').value.trim()
        };
        localStorage.setItem('algoviz_profile',JSON.stringify(d));
        document.getElementById('profileAvatar').textContent=d.name?d.name.charAt(0).toUpperCase():'👤';
        showToast('💾 Profil disimpan!','success');
      };

      // ========== SORTING STATE ==========
      let sortArray=[], sortSteps=[], sortStepIndex=0, sortTimer=null, isSorting=false, sortComparisons=0, sortSwaps=0;

      function renderSortBars(arr, comparing=[], swapping=[], pivot=-1){
        const container=document.getElementById('barContainer');
        const max=Math.max(...arr,1);
        container.innerHTML = arr.map((v,i)=>{
          let cls='bar';
          if(comparing.includes(i)) cls+=' comparing';
          if(swapping.includes(i)) cls+=' swapping';
          if(i===pivot) cls+=' pivot';
          return `<div style="flex:1;display:flex;flex-direction:column;align-items:center;min-width:8px;">
            <span class="bar-value">${v}</span><div class="${cls}" style="height:${(v/max)*100}%;width:100%;"></div></div>`;
        }).join('');
      }

      function recordStep(arr, comp, swap, pivot=-1, desc=''){
        sortSteps.push({array:[...arr], comparing:[...comp], swapping:[...swap], pivot, description:desc});
      }

      function buildSortSteps(algo){
        sortSteps=[]; sortComparisons=0; sortSwaps=0;
        const arr=[...sortArray]; const n=arr.length;
        if(algo==='bubble'){
          for(let i=0;i<n-1;i++){
            let sw=false;
            for(let j=0;j<n-i-1;j++){
              sortComparisons++;
              recordStep(arr,[j,j+1],[],-1,`🔍 Bandingkan angka ${arr[j]} (idx ${j}) dan ${arr[j+1]} (idx ${j+1})`);
              if(arr[j]>arr[j+1]){
                sortSwaps++;
                [arr[j],arr[j+1]]=[arr[j+1],arr[j]];
                sw=true;
                recordStep(arr,[],[j,j+1],-1,`🔄 Tukar: ${arr[j+1]} ↔ ${arr[j]} (posisi ${j} & ${j+1})`);
              }
            }
            if(!sw) break;
          }
        }else if(algo==='selection'){
          for(let i=0;i<n-1;i++){
            let min=i;
            for(let j=i+1;j<n;j++){
              sortComparisons++;
              recordStep(arr,[min,j],[],-1,`🔍 Cari minimum: bandingkan ${arr[min]} (idx ${min}) dengan ${arr[j]} (idx ${j})`);
              if(arr[j]<arr[min]) min=j;
            }
            if(min!==i){
              sortSwaps++;
              [arr[i],arr[min]]=[arr[min],arr[i]];
              recordStep(arr,[],[i,min],-1,`🔄 Tukar minimum: ${arr[i]} ↔ ${arr[min]} (indeks ${i} & ${min})`);
            }
          }
        }else if(algo==='insertion'){
          for(let i=1;i<n;i++){
            const key=arr[i]; let j=i-1;
            recordStep(arr,[i],[],-1,`📌 Ambil kunci: angka ${key} di indeks ${i}`);
            while(j>=0 && arr[j]>key){
              sortComparisons++;
              arr[j+1]=arr[j];
              recordStep(arr,[j,j+1],[j+1],-1,`➡️ Geser ${arr[j]} ke kanan (indeks ${j}→${j+1})`);
              j--;
            }
            arr[j+1]=key;
            if(j+1!==i){ sortSwaps++; recordStep(arr,[],[j+1],-1,`📌 Tempatkan ${key} di indeks ${j+1}`); }
          }
        }else if(algo==='shell'){
          let gap=Math.floor(n/2);
          while(gap>0){
            recordStep(arr,[],[],-1,`🐚 Gap = ${gap}`);
            for(let i=gap;i<n;i++){
              const temp=arr[i]; let j=i;
              while(j>=gap && arr[j-gap]>temp){
                sortComparisons++;
                arr[j]=arr[j-gap];
                recordStep(arr,[j-gap,j],[j],-1,`🐚 Shell geser: ${arr[j]} ke indeks ${j} (gap ${gap})`);
                j-=gap; sortSwaps++;
              }
              arr[j]=temp;
              if(j!==i) recordStep(arr,[],[j],-1,`🐚 Shell sisipkan ${temp} di indeks ${j}`);
            }
            gap=Math.floor(gap/2);
          }
        }else if(algo==='merge'){
          function mergeSort(l,r){
            if(l>=r) return;
            const mid=Math.floor((l+r)/2);
            recordStep(arr,[l,r],[],-1,`🔗 Divide: rentang [${l}..${r}], mid=${mid}`);
            mergeSort(l,mid); mergeSort(mid+1,r);
            const left=arr.slice(l,mid+1), right=arr.slice(mid+1,r+1);
            let i=0,j=0,k=l;
            while(i<left.length && j<right.length){
              sortComparisons++;
              recordStep(arr,[l+i,mid+1+j],[],-1,`🔗 Merge bandingkan ${left[i]} vs ${right[j]}`);
              if(left[i]<=right[j]){ arr[k]=left[i]; i++; } else { arr[k]=right[j]; j++; }
              sortSwaps++; recordStep(arr,[],[k],-1,`🔗 Tempatkan ${arr[k]} di indeks ${k}`);
              k++;
            }
            while(i<left.length){ arr[k]=left[i]; recordStep(arr,[],[k],-1,`🔗 Sisa kiri: ${arr[k]}`); i++; k++; sortSwaps++; }
            while(j<right.length){ arr[k]=right[j]; recordStep(arr,[],[k],-1,`🔗 Sisa kanan: ${arr[k]}`); j++; k++; sortSwaps++; }
          }
          mergeSort(0,n-1);
        }else if(algo==='quick'){
          function qs(low,high){
            if(low>=high) return;
            const pivot=arr[high];
            recordStep(arr,[],[],high,`⚡ Pivot = ${pivot} di indeks ${high}`);
            let i=low-1;
            for(let j=low;j<high;j++){
              sortComparisons++;
              recordStep(arr,[j,high],[],high,`⚡ Bandingkan ${arr[j]} dengan pivot ${pivot}`);
              if(arr[j]<pivot){
                i++;
                [arr[i],arr[j]]=[arr[j],arr[i]];
                sortSwaps++;
                if(i!==j) recordStep(arr,[],[i,j],high,`⚡ Tukar: ${arr[i]} ↔ ${arr[j]}`);
              }
            }
            [arr[i+1],arr[high]]=[arr[high],arr[i+1]];
            sortSwaps++;
            recordStep(arr,[],[i+1,high],-1,`⚡ Tempatkan pivot di indeks ${i+1}`);
            qs(low,i); qs(i+2,high);
          }
          qs(0,n-1);
        }
        recordStep(arr,[],[],-1,'✅ Sorting selesai!');
        sortArray=[...arr];
      }

      window.generateSortArray = function(){
        stopSorting();
        const size=Math.min(80,Math.max(5,parseInt(document.getElementById('sortSize').value)||30));
        document.getElementById('sortSize').value=size;
        sortArray=Array.from({length:size},()=>Math.floor(Math.random()*200)+5);
        resetSortStats();
        document.getElementById('sortDescription').textContent='Array acak dibuat. Klik ▶️ Mulai Sorting.';
        renderSortBars(sortArray);
      };

      window.setSortCustomArray = function(){
        stopSorting();
        const raw=document.getElementById('sortCustomInput').value.trim();
        if(!raw){ showToast('⚠️ Masukkan deret angka (pisah koma).','warning'); return; }
        const nums=raw.split(',').map(v=>parseInt(v.trim())).filter(v=>!isNaN(v));
        if(nums.length<2){ showToast('⚠️ Minimal 2 angka valid.','warning'); return; }
        sortArray=nums;
        document.getElementById('sortSize').value=nums.length;
        resetSortStats();
        document.getElementById('sortDescription').textContent='Array kustom siap. Klik ▶️ Mulai Sorting.';
        renderSortBars(sortArray);
        showToast('✏️ Array kustom berhasil diatur.','success');
      };

      function resetSortStats(){
        sortComparisons=0; sortSwaps=0; sortSteps=[]; sortStepIndex=0;
        document.getElementById('sortComparisons').textContent='0';
        document.getElementById('sortSwaps').textContent='0';
        document.getElementById('sortSteps').textContent='0';
        document.getElementById('sortStatus').textContent='⚪ Idle';
      }

      window.startSorting = function(){
        if(isSorting) return;
        if(sortArray.length===0) generateSortArray();
        stopSorting();
        const algo=document.getElementById('sortAlgo').value;
        resetSortStats();
        document.getElementById('sortStatus').textContent='🟡 Memproses...';
        document.getElementById('btnStartSort').disabled=true;
        const original=[...sortArray];
        buildSortSteps(algo);
        sortArray=[...original];
        isSorting=true;
        playSortStep();
      };

      function playSortStep(){
        if(!isSorting || sortStepIndex>=sortSteps.length){
          if(sortStepIndex>=sortSteps.length){
            document.getElementById('sortStatus').textContent='✅ Selesai!';
            document.getElementById('sortDescription').textContent='✨ Sorting selesai.';
            renderSortBars(sortArray); document.querySelectorAll('#barContainer .bar').forEach(b=>b.classList.add('sorted'));
            playSuccess();
          }
          isSorting=false; document.getElementById('btnStartSort').disabled=false; return;
        }
        const step=sortSteps[sortStepIndex];
        sortArray=[...step.array];
        document.getElementById('sortComparisons').textContent=sortComparisons;
        document.getElementById('sortSwaps').textContent=sortSwaps;
        document.getElementById('sortSteps').textContent=sortStepIndex+1;
        document.getElementById('sortStatus').textContent=`🔵 Langkah ${sortStepIndex+1}/${sortSteps.length}`;
        document.getElementById('sortDescription').textContent=step.description||'';
        renderSortBars(sortArray, step.comparing, step.swapping, step.pivot);
        playTick();
        sortStepIndex++;
        const delay=parseInt(document.getElementById('sortSpeed').value)||80;
        sortTimer=setTimeout(playSortStep, delay);
      }

      window.stopSorting = function(){
        isSorting=false; if(sortTimer) clearTimeout(sortTimer);
        document.getElementById('btnStartSort').disabled=false;
        if(sortStepIndex>0) document.getElementById('sortStatus').textContent='⏹ Dihentikan';
      };

      document.getElementById('sortSpeed').addEventListener('input',function(){
        document.getElementById('speedLabel').textContent=this.value+'ms';
      });

      // ========== SEARCH ==========
      let searchArray=[], searchTimer=null, isSearching=false;
      window.generateSearchArray = function(){
        stopSearching();
        searchArray=Array.from({length:20},()=>Math.floor(Math.random()*150)+1).sort((a,b)=>a-b);
        document.getElementById('searchResult').textContent='Array terurut dibuat. Klik ▶️ Mulai Cari.';
        renderSearchBars(searchArray);
      };
      window.setSearchCustomArray = function(){
        stopSearching();
        const raw=document.getElementById('searchCustomInput').value.trim();
        if(!raw){ showToast('⚠️ Masukkan deret angka.','warning'); return; }
        const nums=raw.split(',').map(v=>parseInt(v.trim())).filter(v=>!isNaN(v));
        if(nums.length<2){ showToast('⚠️ Minimal 2 angka.','warning'); return; }
        searchArray=nums.sort((a,b)=>a-b);
        document.getElementById('searchResult').textContent='Array kustom diurutkan. Klik ▶️ Mulai Cari.';
        renderSearchBars(searchArray);
        showToast('✏️ Array kustom siap.','success');
      };
      function renderSearchBars(arr, highlight=[], found=-1){
        const container=document.getElementById('searchBarContainer');
        const max=Math.max(...arr,1);
        container.innerHTML=arr.map((v,i)=>{
          let cls='bar';
          if(highlight.includes(i)) cls+=' comparing';
          if(i===found) cls+=' sorted';
          return `<div style="flex:1;display:flex;flex-direction:column;align-items:center;min-width:10px;">
            <span class="bar-value">${v}</span><div class="${cls}" style="height:${(v/max)*100}%;width:100%;"></div></div>`;
        }).join('');
      }
      window.startSearching = function(){
        if(isSearching) return;
        stopSearching();
        const algo=document.getElementById('searchAlgo').value;
        const target=parseInt(document.getElementById('searchTarget').value);
        if(isNaN(target)){ showToast('⚠️ Masukkan nilai yang dicari!','warning'); return; }
        isSearching=true; document.getElementById('btnStartSearch').disabled=true;
        if(algo==='linear') linearSearchAnim(target); else binarySearchAnim(target);
      };
      function linearSearchAnim(target){
        let i=0;
        function step(){
          if(!isSearching || i>=searchArray.length){
            if(i>=searchArray.length) document.getElementById('searchResult').textContent=`❌ ${target} tidak ditemukan.`;
            isSearching=false; document.getElementById('btnStartSearch').disabled=false; return;
          }
          renderSearchBars(searchArray,[i]); document.getElementById('searchResult').textContent=`🔎 Periksa indeks ${i}: ${searchArray[i]}`;
          playTick();
          if(searchArray[i]===target){
            renderSearchBars(searchArray,[],i); document.getElementById('searchResult').textContent=`✅ Ditemukan di indeks ${i}.`;
            playSuccess(); isSearching=false; document.getElementById('btnStartSearch').disabled=false; return;
          }
          i++; searchTimer=setTimeout(step,350);
        }
        step();
      }
      function binarySearchAnim(target){
        let low=0, high=searchArray.length-1;
        function step(){
          if(!isSearching || low>high){
            if(low>high) document.getElementById('searchResult').textContent=`❌ ${target} tidak ditemukan.`;
            isSearching=false; document.getElementById('btnStartSearch').disabled=false; return;
          }
          const mid=Math.floor((low+high)/2);
          renderSearchBars(searchArray,[low,mid,high]);
          document.getElementById('searchResult').textContent=`📖 low=${low} mid=${mid} high=${high} → nilai mid=${searchArray[mid]}`;
          playTick();
          if(searchArray[mid]===target){
            renderSearchBars(searchArray,[],mid); document.getElementById('searchResult').textContent=`✅ Ditemukan di indeks ${mid}.`;
            playSuccess(); isSearching=false; document.getElementById('btnStartSearch').disabled=false; return;
          }else if(searchArray[mid]<target) low=mid+1; else high=mid-1;
          searchTimer=setTimeout(step,500);
        }
        step();
      }
      window.stopSearching = function(){
        isSearching=false; if(searchTimer) clearTimeout(searchTimer);
        document.getElementById('btnStartSearch').disabled=false;
      };

      // ========== STACK ==========
      let stackData=[];
      window.stackPush=function(){
        const v=document.getElementById('stackInput').value.trim();
        if(!v||isNaN(v)){showToast('⚠️ Masukkan angka!','warning');return;}
        stackData.push(v); document.getElementById('stackInput').value=''; updateStackVisual();
        showToast(`📚 Push: ${v}`,'success');
      };
      window.stackPop=function(){
        if(!stackData.length){showToast('⚠️ Stack kosong!','warning');return;}
        const v=stackData.pop(); updateStackVisual(); showToast(`📚 Pop: ${v}`,'info');
      };
      window.stackPeek=function(){
        if(!stackData.length){showToast('⚠️ Stack kosong!','warning');return;}
        showToast(`👀 Top: ${stackData[stackData.length-1]}`,'info');
      };
      window.stackClear=function(){stackData=[];updateStackVisual();showToast('🗑 Stack dikosongkan.','info');};
      function updateStackVisual(){
        document.getElementById('stackTopInfo').textContent=stackData.length?stackData[stackData.length-1]:'-';
        document.getElementById('stackSizeInfo').textContent=stackData.length;
        document.getElementById('stackVisual').innerHTML=stackData.map((v,i)=>`<div class="stack-item${i===stackData.length-1?' top-item':''}">${v}${i===stackData.length-1?' ← TOP':''}</div>`).join('');
      }

      // ========== QUEUE ==========
      let queueData=[];
      window.queueEnqueue=function(){
        const v=document.getElementById('queueInput').value.trim();
        if(!v||isNaN(v)){showToast('⚠️ Masukkan angka!','warning');return;}
        queueData.push(v); document.getElementById('queueInput').value=''; updateQueueVisual();
        showToast(`🚶 Enqueue: ${v}`,'success');
      };
      window.queueDequeue=function(){
        if(!queueData.length){showToast('⚠️ Queue kosong!','warning');return;}
        const v=queueData.shift(); updateQueueVisual(); showToast(`🚶 Dequeue: ${v}`,'info');
      };
      window.queueFront=function(){
        if(!queueData.length){showToast('⚠️ Queue kosong!','warning');return;}
        showToast(`👀 Front: ${queueData[0]} | Rear: ${queueData[queueData.length-1]}`,'info');
      };
      window.queueClear=function(){queueData=[];updateQueueVisual();showToast('🗑 Queue dikosongkan.','info');};
      function updateQueueVisual(){
        document.getElementById('queueFrontInfo').textContent=queueData.length?queueData[0]:'-';
        document.getElementById('queueRearInfo').textContent=queueData.length?queueData[queueData.length-1]:'-';
        document.getElementById('queueSizeInfo').textContent=queueData.length;
        document.getElementById('queueVisual').innerHTML=queueData.map((v,i)=>`<div class="queue-item${i===0?' front-item':''}">${v}${i===0?' FRONT':''}</div>`).join('');
      }

      // ========== INIT ==========
      createParticles();
      generateSortArray(); generateSearchArray();
      updateStackVisual(); updateQueueVisual();
      loadProfile();
    })();
  </script>
</body>
</html>
