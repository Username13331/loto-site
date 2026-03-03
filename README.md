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
    .slot .u{font-size:11px;color:rgba(255,255,255,.88);font-weight:800}
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
  </style>
</head>

<body>
  <div class="wrap">

    <!-- SCREEN: LOBBY -->
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

      <div class="balanceCard glass">
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
        <div class="roomCard">
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

        <div class="roomCard">
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

        <div class="roomCard">
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

    <!-- SCREEN: ROOM -->
    <div id="room" class="screen">
      <div class="backRow">
        <div class="backBtn" onclick="goLobby()">←</div>
        <div style="font-weight:900">Комната <span id="roomTitle">№1</span> (Взнос: <span id="roomEntry">10</span> TON)</div>
      </div>

      <div class="roomHeader">
        <div class="t">Участники</div>
        <div class="s">Игра начнётся при заполнении комнаты (5/5)</div>
      </div>

      <div class="slots">
        <div class="slot"><div class="avatar"></div><div class="u">@player1</div></div>
        <div class="slot"><div class="avatar"></div><div class="u">@player2</div></div>
        <div class="slot"><div class="avatar"></div><div class="u">@player3</div></div>
        <div class="slot empty"><div class="avatar">+</div><div class="u">Ожидание…</div></div>
        <div class="slot empty"><div class="avatar">+</div><div class="u">Ожидание…</div></div>
      </div>

      <div class="stats">
        <div class="h">Статистика игры</div>
        <div class="row"><span>Всего участников</span><b>3/5</b></div>
        <div class="row"><span>Текущий призовой фонд</span><b>30 TON</b></div>
        <div class="row"><span>Комиссия 5%</span><b>1.5 TON</b></div>
        <div class="row"><span>Ожидаемый выигрыш</span><b>28.5 TON</b></div>

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

    let currentRoom = {id:1, entry:10};

    function show(id){
      document.querySelectorAll('.screen').forEach(x=>x.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function openRoom(id, entry){
      currentRoom = {id, entry};
      document.getElementById('roomTitle').textContent = `№${id}`;
      document.getElementById('roomEntry').textContent = entry;
      document.getElementById('roomEntry2').textContent = entry;
      show('room');
    }

    function goLobby(){ show('lobby'); }

    function joinCurrent(){
      const payload = `join_room_${currentRoom.id}`;
      if (window.Telegram?.WebApp?.sendData) {
        Telegram.WebApp.sendData(payload);
        Telegram.WebApp.showToast?.(`Отправлено: ${payload}`);
      } else {
        alert(`WebApp data: ${payload}`);
      }
    }

    function fake(type){
      if (type === 'deposit') alert('Далее подключим пополнение (TON/CryptoBot).');
      if (type === 'withdraw') alert('Далее подключим вывод (через заявку/админку).');
    }
  </script>
</body>
</html>
