<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Portal de Gráficas | SEVEN - OXXO - CIRCLEK</title>
  <style>
    :root {
      --bg-url: url('https://alertaurbana.com.ar/07-2021/resize_1627323907.png');
      --glass: rgba(255,255,255,0.08);
      --glass-strong: rgba(255,255,255,0.16);
      --text: #f5f7fb;
      --muted: #cfd6e3;
      --accent: #4f7cff;
      --danger: #ff5c5c;
      --shadow: 0 10px 30px rgba(0,0,0,.35);
      --radius: 20px;
    }
    * { box-sizing: border-box; }
    html, body {
      height: 100%;
      margin: 0;
      font-family: system-ui, sans-serif;
      color: var(--text);
    }
    .bg {
      position: fixed;
      inset: 0;
      background-image: var(--bg-url);
      background-size: cover;
      background-position: center;
    }
    .bg::after {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(180deg, rgba(0,0,0,.45), rgba(0,0,0,.55));
    }
    .wrap {
      position: relative;
      min-height: 100%;
      display: grid;
      place-items: center;
      padding: 24px;
      width: 100%;
      max-width: 1920px;
      margin: 0 auto;
    }
    .card {
      width: 100%;
      max-width: 480px;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.12);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      backdrop-filter: blur(8px);
      padding: 28px;
    }
    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 8px;
    }
    .brand__logo {
      width: 40px;
      height: 40px;
      border-radius: 10px;
      background: rgba(255,255,255,.15);
      display: grid;
      place-items: center;
      font-weight: 700;
    }
    .brand__title { font-weight: 700; }
    .muted { color: var(--muted); font-size: .95rem; }
    .fields {
      display: grid;
      gap: 12px;
      margin-top: 18px;
    }
    label { font-size: .9rem; color: var(--muted); }
    input[type="text"], input[type="password"] {
      width: 100%;
      padding: 14px;
      border-radius: 12px;
      background: rgba(0,0,0,.25);
      color: var(--text);
      border: 1px solid rgba(255,255,255,.18);
    }
    .btn {
      padding: 14px 16px;
      border-radius: 12px;
      border: 0;
      cursor: pointer;
      font-weight: 700;
      background: linear-gradient(180deg, #668bff, #4f7cff);
      color: white;
    }
    .error {
      margin-top: 10px;
      color: var(--danger);
      min-height: 1.2em;
      font-size: .92rem;
    }
    .powered {
      margin-top: 15px;
      text-align: center;
      font-size: 0.85rem;
      color: var(--muted);
    }

    /* 🔹 Menú ocupa toda la pantalla */
    .menu {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: transparent;
      margin: 0;
      padding: 0;
    }
    .menu__inner {
      height: 100%;
      display: flex;
      flex-direction: column;
    }
    .topbar {
      flex-shrink: 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(255,255,255,.08);
      padding: 12px 16px;
      border-radius: 0;
    }
    .topbar-left {
      display: flex;
      align-items: center;
      gap: 16px;
    }
    select#reportSelector {
      padding: 6px 10px;
      border-radius: 8px;
      background: rgba(0,0,0,.4);
      color: white;
      border: 1px solid rgba(255,255,255,.2);
    }

    /* 🔹 Contenedor que ajusta la gráfica */
    .report-container {
      flex: 1;
      overflow: hidden;
      position: relative;
    }
    iframe.report {
      width: 100%;
      height: 100%;
      border: none;
      display: block;
    }

    /* 🔹 Overlay inferior para ocultar barra Power BI */
    .overlay-bottom {
      position: fixed;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 113px; /* altura ajustada */
      background-image: var(--bg-url);
      background-size: cover;
      background-position: center;
      z-index: 999;
    }
    .overlay-bottom::after {
      content: "";
      position: absolute;
      inset: 0;
      background: rgba(0,0,0,.45);
    }

    /* 🔹 Splash de carga */
    .splash {
      position: fixed;
      inset: 0;
      background-image: var(--bg-url);
      background-size: cover;
      background-position: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 2000;
    }
    .splash::after {
      content: "";
      position: absolute;
      inset: 0;
      background: rgba(0,0,0,.65);
    }
    .splash-content {
      position: relative;
      z-index: 2001;
      text-align: center;
      color: white;
    }
    .loader {
      border: 6px solid rgba(255,255,255,0.2);
      border-top: 6px solid white;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      margin: 0 auto 15px;
      animation: spin 1s linear infinite;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    .countdown {
      margin-top: 10px;
      font-size: 1.1rem;
      font-weight: bold;
      color: #fff;
    }
    .hidden { display: none !important; }
  </style>
</head>
<body>
  <div class="bg"></div>
  <main class="wrap">
    <!-- LOGIN -->
    <section id="loginCard" class="card">
      <div class="brand">
        <div class="brand__logo">S</div>
        <div>
          <div class="brand__title">Portal de Gráficas — PHILIP MORRIS</div>
          <div class="muted">Acceso privado para visualizar reportes</div>
        </div>
      </div>
      <form id="loginForm" autocomplete="off">
        <div class="fields">
          <div>
            <label for="user">Usuario</label>
            <input id="user" name="user" type="text" required />
          </div>
          <div>
            <label for="pass">Contraseña</label>
            <input id="pass" name="pass" type="password" required />
          </div>
        </div>
        <label><input type="checkbox" id="remember" /> Mantener sesión</label>
        <button class="btn" type="submit">Entrar</button>
        <div id="error" class="error"></div>
        <div class="powered">Powered by Stratech/KeepGo</div>
      </form>
    </section>

    <!-- MENU CON GRAFICA -->
    <section id="menu" class="menu">
      <div class="menu__inner">
        <div class="topbar">
          <div class="topbar-left">
            <div class="brand">
              <div class="brand__logo">S</div>
              <div>
                <div class="brand__title">Panel de Gráficas</div>
                <div class="muted" id="welcomeMsg">Bienvenido</div>
              </div>
            </div>
            <!-- Selector solo visible para admin -->
            <select id="reportSelector" class="hidden">
              <option value="seven">Seven Eleven</option>
              <option value="oxxo">Oxxo</option>
              <option value="circlek">CircleK</option>
            </select>
          </div>
          <button id="logoutBtn" class="btn" style="background: linear-gradient(180deg,#ff7a7a,#ff5858);">Cerrar sesión</button>
        </div>

        <!-- 🚀 Contenedor con iframe -->
        <div class="report-container">
          <iframe id="reportFrame" class="report"></iframe>
          <!-- Overlay solo aparece cuando está el menú -->
          <div class="overlay-bottom"></div>
        </div>
      </div>
    </section>

    <!-- SPLASH -->
    <div id="splash" class="splash hidden">
      <div class="splash-content">
        <div class="loader"></div>
        <div>Cargando dashboard...</div>
        <div id="countdown" class="countdown"></div>
      </div>
    </div>
  </main>

  <script>
    const REPORT_URLS = {
      seven: 'https://app.powerbi.com/view?r=eyJrIjoiYjFiMmM5ZWMtZDI0YS00Njg5LTkzNGUtYWFlOGZhYTNhODc4IiwidCI6ImIxM2NlNGM5LTJiZTYtNDg0NC04Y2Q5LTYwOTcyMGFmYWY5YiJ9&pageName=05c1a881714a70e90340&chromeless=true',
      oxxo: 'https://powerbi.com/oxxo&chromeless=true',
      circlek: 'https://powerbi.com/circlek&chromeless=true'
    };

    const USERS = [
      { u: 'Seven eleven', p: 'Seven2025!', key: 'seven' },
      { u: 'Oxxo', p: 'Oxxo2025!', key: 'oxxo' },
      { u: 'CircleK', p: 'CircleK2025!', key: 'circlek' },
      { u: 'admin', p: 'admin', key: 'admin' }
    ];

    const loginForm = document.getElementById('loginForm');
    const loginCard = document.getElementById('loginCard');
    const menu = document.getElementById('menu');
    const errorBox = document.getElementById('error');
    const remember = document.getElementById('remember');
    const welcomeMsg = document.getElementById('welcomeMsg');
    const reportFrame = document.getElementById('reportFrame');
    const reportSelector = document.getElementById('reportSelector');
    const splash = document.getElementById('splash');
    const countdownEl = document.getElementById('countdown');
    const TOKEN_KEY = 'portal_token_v3';

    function login(userKey) {
      localStorage.setItem(TOKEN_KEY, JSON.stringify({ userKey, at: Date.now() }));
    }

    function logout() {
      localStorage.removeItem(TOKEN_KEY);
      location.reload();
    }

    function showMenu(userKey) {
      loginCard.classList.add('hidden');
      menu.style.display = 'block';
      if (userKey === 'admin') {
        welcomeMsg.textContent = "Bienvenido, Administrador";
        reportSelector.classList.remove('hidden');
        reportFrame.src = REPORT_URLS.seven;
      } else {
        welcomeMsg.textContent = `Bienvenido, ${userKey.charAt(0).toUpperCase() + userKey.slice(1)}`;
        reportSelector.classList.add('hidden');
        reportFrame.src = REPORT_URLS[userKey];
      }

      // 🚀 Mostrar splash con retroceso
      splash.classList.remove('hidden');
      let counter = 5;
      countdownEl.textContent = `Cargando en ${counter}...`;
      const interval = setInterval(() => {
        counter--;
        if (counter > 0) {
          countdownEl.textContent = `Cargando en ${counter}...`;
        } else {
          clearInterval(interval);
        }
      }, 1000);

      setTimeout(() => {
        splash.classList.add('hidden');
      }, 5000);
    }

    loginForm.addEventListener('submit', e => {
      e.preventDefault();
      errorBox.textContent = '';
      const user = document.getElementById('user').value.trim();
      const pass = document.getElementById('pass').value;
      const found = USERS.find(u => u.u === user && u.p === pass);
      if (!found) {
        errorBox.textContent = 'Usuario o contraseña incorrectos.';
        return;
      }
      if (remember.checked) login(found.key);
      showMenu(found.key);
    });

    document.getElementById('logoutBtn').addEventListener('click', logout);

    reportSelector.addEventListener('change', () => {
      const key = reportSelector.value;
      reportFrame.src = REPORT_URLS[key];
    });

    try {
      const saved = JSON.parse(localStorage.getItem(TOKEN_KEY));
      if (saved?.userKey) showMenu(saved.userKey);
    } catch {}
  </script>
</body>
</html>
