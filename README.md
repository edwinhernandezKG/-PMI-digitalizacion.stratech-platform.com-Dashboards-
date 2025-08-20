<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
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
    html, body { height: 100%; margin: 0; font-family: system-ui, sans-serif; color: var(--text); }

    .bg {
      position: fixed; inset: 0;
      background-image: var(--bg-url);
      background-size: cover;
      background-position: center;
    }
    .bg::after {
      content: "";
      position: absolute; inset: 0;
      background: linear-gradient(180deg, rgba(0,0,0,.45), rgba(0,0,0,.55));
    }

    .wrap { position: relative; min-height: 100%; display: grid; place-items: center; padding: 24px; }

    .card {
      width: 100%; max-width: 480px; background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.12); border-radius: var(--radius);
      box-shadow: var(--shadow); backdrop-filter: blur(8px);
      padding: 28px;
    }

    .brand { display: flex; align-items: center; gap: 12px; margin-bottom: 8px; }
    .brand__logo { width: 40px; height: 40px; border-radius: 10px; background: rgba(255,255,255,.15); display: grid; place-items: center; font-weight: 700; }
    .brand__title { font-weight: 700; }
    .muted { color: var(--muted); font-size: .95rem; }

    .fields { display: grid; gap: 12px; margin-top: 18px; }
    label { font-size: .9rem; color: var(--muted); }
    input[type="text"], input[type="password"] {
      width: 100%; padding: 14px; border-radius: 12px;
      background: rgba(0,0,0,.25); color: var(--text); border: 1px solid rgba(255,255,255,.18);
    }

    .btn {
      padding: 14px 16px; border-radius: 12px; border: 0; cursor: pointer; font-weight: 700;
      background: linear-gradient(180deg, #668bff, #4f7cff); color: white;
    }

    .error { margin-top: 10px; color: var(--danger); min-height: 1.2em; font-size: .92rem; }

    .powered { margin-top: 15px; text-align: center; font-size: 0.85rem; color: var(--muted); }

    .menu {
      display: none; padding: 24px; width: 100%; height: 100vh;
    }

    .menu__inner {
      height: 100%; display: grid; grid-template-rows: auto 1fr; gap: 18px;
    }

    .topbar {
      display: flex; justify-content: space-between; align-items: center;
      background: rgba(255,255,255,.08); padding: 12px 16px; border-radius: 16px;
    }

    /* 🚀 Contenedor para aplicar zoom y esconder barra */
    .report-container {
      width: 100%;
      height: 100vh;   /* ✅ Siempre ocupa la pantalla completa */
      overflow: hidden;
      position: relative;
    }

    iframe.report {
      width: 100%;
      height: 100%;
      border: none;
      transform: scale(1.15);        /* 🔍 Más zoom para tapar la franja */
      transform-origin: top center;  /* desde arriba */
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
        <div class="powered">Powered by KeepGo</div>
      </form>
    </section>

    <!-- MENU CON GRAFICA -->
    <section id="menu" class="menu">
      <div class="menu__inner">
        <div class="topbar">
          <div class="brand">
            <div class="brand__logo">S</div>
            <div>
              <div class="brand__title">Panel de Gráficas</div>
              <div class="muted" id="welcomeMsg">Bienvenido</div>
            </div>
          </div>
          <button id="logoutBtn" class="btn" style="background: linear-gradient(180deg,#ff7a7a,#ff5858);">Cerrar sesión</button>
        </div>
        <!-- 🚀 Contenedor con zoom -->
        <div class="report-container">
          <iframe id="reportFrame" class="report"></iframe>
        </div>
      </div>
    </section>
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
      { u: 'CircleK', p: 'CircleK2025!', key: 'circlek' }
    ];

    const loginForm = document.getElementById('loginForm');
    const loginCard = document.getElementById('loginCard');
    const menu = document.getElementById('menu');
    const errorBox = document.getElementById('error');
    const remember = document.getElementById('remember');
    const welcomeMsg = document.getElementById('welcomeMsg');
    const reportFrame = document.getElementById('reportFrame');
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
      welcomeMsg.textContent = `Bienvenido, ${userKey.charAt(0).toUpperCase() + userKey.slice(1)}`;
      reportFrame.src = REPORT_URLS[userKey];
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

    try {
      const saved = JSON.parse(localStorage.getItem(TOKEN_KEY));
      if (saved?.userKey) showMenu(saved.userKey);
    } catch {}
  </script>
</body>
</html>
