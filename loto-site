<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>TON Lottery</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>

  <style>
    :root{
      --bg0:#070A12;
      --bg1:#0B1020;
      --card:rgba(255,255,255,.06);
      --card2:rgba(255,255,255,.04);
      --stroke:rgba(255,255,255,.10);
      --text:#EAF0FF;
      --muted:rgba(234,240,255,.70);
      --muted2:rgba(234,240,255,.55);
      --gold:#FFD24A;
      --gold2:#FFB800;
      --teal:#43F5D5;
      --purple:#B06CFF;
      --pink:#FF63C6;
      --shadow:0 18px 55px rgba(0,0,0,.55);
      --r16:16px;
      --r20:20px;
      --r24:24px;
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Arial;
      color:var(--text);
      background:
        radial-gradient(1200px 700px at 15% -10%, rgba(176,108,255,.20), transparent 55%),
        radial-gradient(900px 600px at 85% 10%, rgba(67,245,213,.16), transparent 55%),
        radial-gradient(900px 700px at 55% 110%, rgba(255,99,198,.12), transparent 55%),
        linear-gradient(180deg, var(--bg1), var(--bg0));
      overflow-x:hidden;
    }

    .wrap{max-width:430px;margin:0 auto;padding:18px 16px 28px}
    .topbar{display:flex;align-items:center;justify-content:space-between;margin:8px 0 14px}
    .brand{
      display:flex;align-items:center;gap:10px;
      font-weight:800;letter-spacing:.2px;
      font-size:18px;
    }
    .brand .logo{
      width:34px;height:34px;border-radius:12px;
      background:
        radial-gradient(circle at 30% 30%, rgba(255,210,74,.35), transparent 60%),
        linear-gradient(180deg, rgba(255,210,74,.25), rgba(255,184,0,.08));
      border:1px solid rgba(255,210,74,.35);
      box-shadow: 0 10px 30px rgba(255,184,0,.08);
      display:grid;place-items:center;
    }
    .brand .logo span{font-size:18px}
    .sub{font-size:12px;color:var(--muted2);margin-top:2px}

    .glass{
      background:linear-gradient(180deg, rgba(255,255,255,.08), rgba(255,255,255,.04));
      border:1px solid var(--stroke);
      border-radius:var(--r24);
      box-shadow:var(--shadow);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
    }

    .balanceCard{
      padding:16px 16px 14px;
      display:flex;align-items:center;justify-content:space-between;
      margin:10px 0 12px;
    }
    .balanceLeft .lbl{font-size:12px;color:var(--muted2)}
    .balanceLeft .val{
      display:flex;align-items:baseline;gap:8px;margin-top:6px;
      font-size:28px;font-weight:900;
    }
    .coin{
      width:22px;height:22px;border-radius:999px;
      background: radial-gradient(circle at 35% 35%, rgba(255,255,255,.35), transparent 55%),
                  linear-gradient(180deg, var(--gold), var(--gold2));
      box-shadow:0 10px 30px rgba(255,184,0,.18);
      border:1px solid rgba(255,255,255,.22);
      display:inline-block;
    }
    .balanceRight{
      display:flex;flex-direction:column;gap:8px;min-width:140px;
    }

    .btn{
      width:100%;
      border:0;
      cursor:pointer;
      padding:12px 12px;
      border-radius:16px;
      color:#061018;
      font-weight:900;
      letter-spacing:.2px;
      display:flex;align-items:center;justify-content:center;
      gap:8px;
      transition: transform .08s ease, filter .2s ease;
      user-select:none;
    }
    .btn:active{transform: scale(.98)}
    .btn.green{
      background: linear-gradient(180deg, rgba(67,245,213,1), rgba(67,245,213,.70));
      box-shadow: 0 16px 45px rgba(67,245,213,.18);
    }
    .btn.purple{
      background: linear-gradient(180deg, rgba(176,108,255,1), rgba(176,108,255,.70));
      box-shadow: 0 16px 45px rgba(176,108,255,.16);
    }
    .btn.gold{
      color:#231800;
      background: linear-gradient(180deg, rgba(255,210,74,1), rgba(255,184,0,.86));
      box-shadow: 0 18px 55px rgba(255,184,0,.12);
    }

    .sectionTitle{
      margin:16px 2px 10px;
      font-size:13px;
      color:var(--muted);
      font-weight:800;
      letter-spacing:.3px;
      text-transform: uppercase;
    }

    .rooms{display:flex;flex-direction:column;gap:12px}
    .roomCard{
      padding:14px;
      border-radius:var(--r24);
      position:relative;
      background: linear-gradient(180deg, rgba(255,255,255,.07), rgba(255,255,255,.03));
      border:1px solid rgba(255,255,255,.10);
      box-shadow: 0 18px 55px rgba(0,0,0,.50);
      overflow:hidden;
    }
    .roomCard::before{
      content:"";
      position:absolute;inset:-2px;
      background:
        radial-gradient(600px 160px at 20% 0%, rgba(67,245,213,.18), transparent 55%),
        radial-gradient(600px 160px at 80% 0%, rgba(255,99,198,.14), transparent 55%),
        radial-gradient(600px 200px at 50% 110%, rgba(255,184,0,.10), transparent 55%);
      pointer-events:none;
    }
    .roomInner{position:relative;z-index:1}
    .roomTop{display:flex;align-items:center;justify-content:space-between}
    .roomName{font-weight:900;font-size:16px}
    .badge{
      display:flex;align-items:center;gap:6px;
      font-size:12px;font-weight:800;color:rgba(255,255,255,.85);
      padding:6px 10px;border-radius:999px;
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
    }
    .meta{margin-top:6px;font-size:12px;color:var(--muted)}
    .progress{
      height:10px;border-radius:999px;
      background: rgba(255,255,255,.08);
      border:1px solid rgba(255,255,255,.08);
      overflow:hidden;
      margin:12px 0 12px;
    }
    .bar{
      height:100%;
      width:0%;
      background: linear-gradient(90deg, rgba(67,245,213,.95), rgba(255,210,74,.95));
      box-shadow: 0 10px 30px rgba(67,245,213,.12);
      border-radius:999px;
      transition: width .25s ease;
    }

    .roomBtnRow{display:flex;gap:10px}
    .roomBtnRow .btn{padding:12px 12px;border-radius:16px}

    /* Room screen */
    .screen{display:none}
    .screen.active{display:block}
    .backRow{display:flex;align-items:center;gap:10px;margin:6px 0 12px}
    .backBtn{
      width:38px;height:38px;border-radius:14px;
      background: rgba(255,255,255,.06);
      border:1px solid rgba(255,255,255,.10);
      display:grid;place-items:center;
      cursor:pointer;
    }
    .roomHeader{
      padding:14px 14px 12px;
      border-radius:var(--r24);
      border:1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.05);
      box-shadow: var(--shadow);
    }
    .roomHeader .t{font-weight:900;font-size:16px}
    .roomHeader .s{margin-top:6px;color:var(--muted);font-size:12px}

    .slots{display:flex;gap:10px;margin:14px 0 14px}
    .slot{
      flex:1;
      border-radius:18px;
      border:1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.05);
      padding:10px 8px;
      text-align:center;
    }
    .avatar{
      width:34px;height:34px;border-radius:14px;margin:0 auto 8px;
      background: radial-gradient(circle at 30% 30%, rgba(255,255,255,.30), transparent 55%),
                  linear-gradient(180deg, rgba(67,245,213,.55), rgba(176,108,255,.35));
      border:1px solid rgba(255,255,255,.16);
    }
    .slot .u{font-size:11px;color:rgba(255,255,255,.88);font-weight:800; overflow:hidden; text-overflow:ellipsis; white-space:nowrap;}
    .slot.empty .avatar{
      background: rgba(255,255,255,.06);
      display:grid;place-items:center;
      color:rgba(255,255,255,.55);
      font-weight:900;
    }
    .slot.empty .u{color:rgba(255,255,255,.45);font-weight:700}

    .stats{
      padding:14px;
      border-radius:var(--r24);
      border:1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.05);
      box-shadow: var(--shadow);
    }
    .stats .h{font-weight:900;margin-bottom:8px}
    .row{display:flex;justify-content:space-between;gap:10px;font-size:12px;color:var(--muted);padding:6px 0;border-bottom:1px solid rgba(255,255,255,.06)}
    .row:last-child{border-bottom:0}
    .row b{color:rgba(255,255,255,.92)}
    .bigJoin{margin-top:14px}
    .foot{margin-top:10px;color:rgba(255,255,255,.45);font-size:11px;text-align:center}

    /* ====== Glow + Animated background ====== */
    .bg-orbs{
      position:fixed; inset:-40px; z-index:-1;
      filter: blur(22px);
      opacity:.95;
      pointer-events:none;
    }
    .orb{
      position:absolute;
      width:520px; height:520px; border-radius:999px;
      mix-blend-mode: screen;
      animation: floaty 10s ease-in-out infinite;
    }
    .orb.a{ left:-120px; top:-140px; background: radial-gradient(circle at 30% 30%, rgba(176,108,255,.85), transparent 60%); animation-duration: 12s;}
    .orb.b{ right:-180px; top:-120px; background: radial-gradient(circle at 30% 30%, rgba(67,245,213,.75), transparent 60%); animation-duration: 14s;}
    .orb.c{ left:20%; bottom:-220px; background: radial-gradient(circle at 35% 35%, rgba(255,99,198,.55), transparent 62%); animation-duration: 16s;}
    @keyframes floaty{
      0%,100%{ transform: translate3d(0,0,0) scale(1); }
      50%{ transform: translate3d(18px,-22px,0) scale(1.03); }
    }

    .glow-border{
      position:relative;
    }
    .glow-border:after{
      content:"";
      position:absolute; inset:-1px;
      border-radius:inherit;
      background: linear-gradient(90deg, rgba(67,245,213,.55), rgba(255,210,74,.55), rgba(176,108,255,.55));
      opacity:.35;
      filter: blur(10px);
      z-index:-1;
    }

    .btn.gold{
      position:relative;
      overflow:hidden;
    }
    .btn.gold:before{
      content:"";
      position:absolute; top:-80px; left:-120px;
      width:120px; height:220px;
      background: rgba(255,255,255,.35);
      transform: rotate(25deg);
      opacity:0;
    }
    .btn.gold:hover{ filter: brightness(1.05); }
    .btn.gold:hover:before{
      animation: shine .9s ease-in-out;
    }
    @keyframes shine{
      0%{ transform: translateX(-120px) rotate(25deg); opacity:0; }
      20%{ opacity:.55; }
      100%{ transform: translateX(520px) rotate(25deg); opacity:0; }
    }

    /* cards pop */
    .roomCard, .balanceCard, .roomHeader, .stats{
      transition: transform .12s ease, border-color .2s ease, background .2s ease;
    }
    .roomCard:hover{
      transform: translateY(-2px);
      border-color: rgba(255,255,255,.16);
      background: linear-gradient(180deg, rgba(255,255,255,.09), rgba(255,255,255,.035));
    }
  </style>
</head>

<body>
  <div class="bg-orbs">
    <div class="orb a"></div>
    <div class="orb b"></div>
    <div class="orb c"></div>
  </div>

  <div class="wrap">

    <div id="lobby" class="screen active">
      <div class="topbar">
        <div>
          <div class="brand">
            <div class="logo"><span>🍀</span></div>
            <div>
              TON Lottery
              <div class="sub">Быстро • Честно</div>
            </div>
          </div>
        </div>
        <div style="opacity:.55;font-size:12px;font-weight:700">Mini App</div>
      </div>

      <div class="balanceCard glass glow-border">
        <div class="balanceLeft">
          <div class="lbl">Ваш баланс</div>
          <div class="val"><span class="coin"></span> <span id="bal">12.5</span> <span style="font-size:14px;color:var(--muted2);font-weight:800">TON</span></div>
        </div>
        <div class="balanceRight">
          <button class="btn green" onclick="fake('deposit')">Пополнить Balance</button>
          <button class="btn purple" onclick="fake('withdraw')">Вывести TON</button>
        </div>
      </div>

      <div class="sectionTitle">Доступные комнаты</div>

      <div class="rooms">
        <div class="roomCard glow-border">
          <div class="roomInner">
            <div class="roomTop">
              <div class="roomName">Комната №1</div>
              <div class="badge"><span class="coin" style="width:14px;height:14px"></span> 10 TON</div>
            </div>
            <div class="meta">Участников: <b>3/5</b></div>
            <div class="progress"><div class="bar" style="width:60%"></div></div>
            <div class="roomBtnRow">
              <button class="btn gold" onclick="openRoom(1, 10)">Присоединиться</button>
            </div>
          </div>
        </div>

        <div class="roomCard glow-border">
          <div class="roomInner">
            <div class="roomTop">
              <div class="roomName">Комната №2</div>
              <div class="badge"><span class="coin" style="width:14px;height:14px"></span> 5 TON</div>
            </div>
            <div class="meta">Участников: <b>2/5</b></div>
            <div class="progress"><div class="bar" style="width:40%"></div></div>
            <div class="roomBtnRow">
              <button class="btn gold" onclick="openRoom(2, 5)">Присоединиться</button>
            </div>
          </div>
        </div>

        <div class="roomCard glow-border">
          <div class="roomInner">
            <div class="roomTop">
              <div class="roomName">Комната №3</div>
              <div class="badge"><span class="coin" style="width:14px;height:14px"></span> 1 TON</div>
            </div>
            <div class="meta">Участников: <b>4/5</b></div>
            <div class="progress"><div class="bar" style="width:80%"></div></div>
            <div class="roomBtnRow">
              <button class="btn gold" onclick="openRoom(3, 1)">Присоединиться</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div id="room" class="screen">
      <div class="backRow">
        <div class="backBtn" onclick="goLobby()">←</div>
        <div style="font-weight:900">Комната <span id="roomTitle">№1</span> (Взнос: <span id="roomEntry">10</span> TON)</div>
      </div>

      <div class="roomHeader">
        <div class="t">Участники</div>
        <div class="s">Игра начнётся при заполнении комнаты (5/5)</div>
      </div>

      <div id="slots" class="slots"></div>

      <div class="stats">
        <div class="h">Статистика игры</div>
        <div class="row"><span>Всего участников</span><b>0/5</b></div>
        <div class="row"><span>Текущий призовой фонд</span><b>0 TON</b></div>
        <div class="row"><span>Комиссия 5%</span><b>0 TON</b></div>
        <div class="row"><span>Ожидаемый выигрыш</span><b>0 TON</b></div>

        <div class="bigJoin">
          <button class="btn gold" onclick="joinCurrent()">Присоединиться к игре (<span id="roomEntry2">10</span> TON)</button>
          <div class="foot">Нажимая, вы подтверждаете списание с внутреннего баланса</div>
        </div>
      </div>
    </div>

  </div>

  <script>
    // Telegram polish
    const tg = window.Telegram?.WebApp;
    try { tg?.ready(); tg?.expand(); } catch(e){}

    // ИЗМЕНЕНИЕ 1: Добавили IP твоего сервера в API_BASE
    const API_BASE = "http://144.31.103.60"; 

    let currentRoom = {id:1, entry:10};

    function show(id){
      document.querySelectorAll('.screen').forEach(x=>x.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    // ИЗМЕНЕНИЕ 4: Добавлен startRoomPolling()
    function openRoom(id, entry){
      currentRoom = {id, entry};
      document.getElementById('roomTitle').textContent = `№${id}`;
      document.getElementById('roomEntry').textContent = entry;
      document.getElementById('roomEntry2').textContent = entry;
      show('room');
      startRoomPolling(); 
    }

    // ИЗМЕНЕНИЕ 4: Добавлен stopRoomPolling()
    function goLobby(){
      stopRoomPolling();
      show('lobby');
    }

    // ИЗМЕНЕНИЕ 3: Функции рендера комнаты и работы с API
    function tgUser() {
      const u = tg?.initDataUnsafe?.user;
      return {
        id: u?.id ?? null,
        username: u?.username ? "@"+u.username : (u?.first_name || "Игрок"),
        photo: u?.photo_url ?? null
      };
    }

    function renderSlots(members) {
      const el = document.getElementById("slots");
      if (!el) return;

      const max = 5;
      const filled = members.slice(0, max);
      const emptyCount = Math.max(0, max - filled.length);

      const slotHtml = (m) => `
        <div class="slot">
          <div class="avatar" style="${m.photo_url ? `background-image:url('${m.photo_url}');background-size:cover;background-position:center;` : ""}"></div>
          <div class="u">${m.username || "Игрок"}</div>
        </div>
      `;

      const emptyHtml = () => `
        <div class="slot empty">
          <div class="avatar">+</div>
          <div class="u">Ожидание…</div>
        </div>
      `;

      el.innerHTML = filled.map(slotHtml).join("") + Array.from({length: emptyCount}).map(emptyHtml).join("");
    }

    async function fetchRoomState(roomId){
      // сервер отдаст JSON: {room_id, entry, members:[{username, photo_url}], count, pot, fee, expected}
      const r = await fetch(`${API_BASE}/api/rooms/${roomId}`, { cache: "no-store" });
      if (!r.ok) throw new Error("API error");
      return await r.json();
    }

    async function refreshRoom(){
      try{
        const st = await fetchRoomState(currentRoom.id);

        // обновим заголовки/взнос
        document.getElementById('roomTitle').textContent = `№${st.room_id}`;
        document.getElementById('roomEntry').textContent = st.entry;
        document.getElementById('roomEntry2').textContent = st.entry;

        // обновим слоты
        renderSlots(st.members || []);

        // обновим статы
        const count = st.count ?? (st.members?.length ?? 0);
        document.querySelectorAll(".row b")[0].textContent = `${count}/5`;
        document.querySelectorAll(".row b")[1].textContent = `${st.pot ?? (st.entry * count)} TON`;
        document.querySelectorAll(".row b")[2].textContent = `${st.fee ?? 0} TON`;
        document.querySelectorAll(".row b")[3].textContent = `${st.expected ?? 0} TON`;
      }catch(e){
        // пока API не готово, выводим пустые слоты (чтобы дизайн не ломался)
        if(document.getElementById("slots").innerHTML === "") {
             renderSlots([]);
        }
      }
    }

    let roomTimer = null;
    function startRoomPolling(){
      stopRoomPolling();
      refreshRoom();
      roomTimer = setInterval(refreshRoom, 2000);
    }
    function stopRoomPolling(){
      if (roomTimer) clearInterval(roomTimer);
      roomTimer = null;
    }

    // ИЗМЕНЕНИЕ 5: Теперь отправляем красивый JSON с данными игрока
    async function joinCurrent(){
      const u = tgUser();
      const payload = JSON.stringify({
        action: "join",
        room_id: currentRoom.id,
        user: u
      });

      if (window.Telegram?.WebApp?.sendData) {
        Telegram.WebApp.sendData(payload);
        Telegram.WebApp.showToast?.(`Отправка данных...`);
      } else {
        alert("Данные для бота: " + payload);
      }
    }

    function fake(type){
      if (type === 'deposit') alert('Далее подключим пополнение (TON/CryptoBot).');
      if (type === 'withdraw') alert('Далее подключим вывод (через заявку/админку).');
    }
  </script>
</body>
</html>
