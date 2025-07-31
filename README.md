<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Инвест Мини — Детский банк</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Comic Sans MS', cursive, sans-serif;
    }
    body {
      background: url('https://avatars.mds.yandex.net/i?id=370c8f7eb67048a19227bc90c19cab10_sr-3724606-images-thumbs&n=13') no-repeat center / cover fixed;
      color: #333;
      height: 100vh;
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }

    /* Приветственный экран */
    .welcome-screen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(255, 255, 255, 0.95);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 2000;
      animation: fadeIn 0.5s ease-in-out;
    }
    .welcome-screen.fade-out {
      animation: fadeOut 0.5s ease-in-out forwards;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    @keyframes fadeOut {
      from { opacity: 1; }
      to { opacity: 0; }
    }
    .welcome-screen h1 {
      color: #00838f;
      font-size: 2em;
      margin-bottom: 10px;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }
    .welcome-screen p {
      font-size: 1.2em;
      color: #333;
      margin-bottom: 20px;
    }
    .welcome-image {
      width: 100px;
      height: 100px;
      background: #ddd;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    /* Стили для экранов */
    .screen {
      display: none;
      flex: 1;
      padding: 20px;
      overflow-y: auto;
      padding-bottom: 80px;
    }
    .screen.active-screen {
      display: flex;
      flex-direction: column;
    }
    h1 {
      text-align: center;
      color: #333;
      font-size: 1.6em;
      margin: 20px 0;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    /* Значок уведомления и баланс */
    .notification-btn {
      position: fixed;
      top: 20px;
      right: 20px;
      font-size: 1.8em;
      color: #333;
      cursor: pointer;
      z-index: 100;
    }
    .notification-btn:hover {
      color: #d81b60;
    }
    .balance-display {
      position: fixed;
      top: 50px;
      right: 20px;
      background: rgba(0, 131, 143, 0.9);
      color: white;
      padding: 8px 15px;
      border-radius: 10px;
      font-size: 1.1em;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      z-index: 100;
      text-align: right;
    }
    .balance-display span {
      font-weight: bold;
    }

    /* Карта */
    .card {
      background: linear-gradient(135deg, #00838f, #00bfa5);
      color: white;
      border-radius: 20px;
      padding: 20px;
      margin: 20px auto;
      width: 90%;
      max-width: 350px;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
      text-align: left;
    }
    .card-header {
      font-size: 0.9em;
      opacity: 0.9;
    }
    .card-number {
      font-family: monospace;
      font-size: 1.4em;
      letter-spacing: 3px;
      margin: 10px 0;
      opacity: 0.8;
    }
    .card-name, .card-valid {
      margin-top: 15px;
      font-size: 1.1em;
    }
    .card-valid {
      opacity: 0.9;
    }

    /* Выпадающий список операций */
    .transactions {
      width: 90%;
      max-width: 350px;
      margin: 20px auto;
    }
    .transactions h3 {
      background: rgba(255, 255, 255, 0.95);
      padding: 15px;
      border-radius: 15px;
      font-size: 1.2em;
      color: #333;
      cursor: pointer;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .transactions h3::after {
      content: "▼";
    }
    .transactions.expanded h3::after {
      content: "▲";
    }
    #transactions-list {
      background: rgba(255, 255, 255, 0.95);
      border-radius: 0 0 15px 15px;
      padding: 15px;
      margin-top: -5px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      display: none;
    }
    .transaction-item {
      padding: 10px 0;
      border-bottom: 1px dashed #eee;
      font-size: 1.1em;
      display: flex;
      justify-content: space-between;
    }
    .transaction-item:last-child {
      border-bottom: none;
    }
    .amount {
      font-weight: bold;
    }
    .income { color: #00c853; }
    .expense { color: #d50000; }

    /* Вкладка "Копилка" */
    .piggy-container {
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .piggy-scene {
      position: relative;
      width: 100%;
      height: 250px;
      background: url('https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/dd1c3d16532917.562ad5374e593.png') no-repeat center / cover;
      border-radius: 15px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      display: flex;
      align-items: flex-end;
      justify-content: center;
      padding: 20px;
      overflow: hidden;
    }
    .piggy {
      position: absolute;
      bottom: 20px;
      width: 100px;
      transition: left 0.5s ease-in-out, transform 0.5s ease-in-out;
    }
    .piggy.bounce {
      animation: bounce-and-sway 0.5s ease-in-out infinite alternate;
    }
    .piggy.gentle {
      animation: gentle-sway 1s ease-in-out infinite alternate;
    }
    .piggy.cheer {
      animation: piggy-cheer 0.8s ease-in-out;
    }
    @keyframes bounce-and-sway {
      from { transform: translateY(0) rotate(0deg); }
      to { transform: translateY(-10px) rotate(5deg); }
    }
    @keyframes gentle-sway {
      from { transform: translateY(0) rotate(-3deg); }
      to { transform: translateY(-5px) rotate(3deg); }
    }
    @keyframes piggy-cheer {
      0% { transform: translateY(0) rotate(0deg); }
      25% { transform: translateY(-20px) rotate(-10deg); }
      50% { transform: translateY(-20px) rotate(10deg); }
      75% { transform: translateY(-20px) rotate(-10deg); }
      100% { transform: translateY(0) rotate(0deg); }
    }
    .coin {
      position: absolute;
      width: 20px;
      height: 20px;
      background: url('https://img.icons8.com/color/48/000000/coins.png') no-repeat center / contain;
      animation: coin-fall 1s ease-in-out forwards;
      opacity: 0;
    }
    @keyframes coin-fall {
      0% { transform: translateY(-100px); opacity: 1; }
      100% { transform: translateY(100px); opacity: 0; }
    }
    .goal-image {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 100px;
      opacity: 0;
      transition: opacity 0.5s ease-in-out;
    }
    .goal-image.visible {
      opacity: 0.7;
      animation: fade-in 0.5s ease-in-out;
    }
    @keyframes fade-in {
      from { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
      to { opacity: 0.7; transform: translate(-50%, -50%) scale(1); }
    }
    .progress-bar {
      width: 90%;
      height: 20px;
      background: #e0e0e0;
      border-radius: 10px;
      overflow: hidden;
      margin: 15px 0;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    .progress-fill {
      height: 100%;
      background: linear-gradient(90deg, #00bfa5, #4caf50);
      transition: width 0.5s ease-in-out;
    }
    .buttons {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin: 15px 0;
    }
    .buttons button {
      padding: 15px 20px;
      font-size: 1.2em;
      color: white;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: transform 0.2s, box-shadow 0.2s;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    .buttons button:hover {
      transform: scale(1.05);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    }
    .buttons button:disabled {
      background-color: #aaa;
      cursor: not-allowed;
      transform: none;
      box-shadow: none;
    }
    .buttons button.goal-btn {
      background-color: #0288d1;
    }
    .buttons button.add-btn {
      background-color: #4caf50;
    }
    .buttons button.remove-btn {
      background-color: #d81b60;
    }
    .buttons button.cheer-btn {
      background-color: #ff9800;
    }
    .info {
      text-align: center;
      margin-top: 10px;
      font-size: 1.1em;
      background: rgba(255, 255, 255, 0.95);
      padding: 15px;
      border-radius: 15px;
      width: 90%;
      max-width: 350px;
      color: #333;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .upload-goal-image {
      margin: 10px 0;
      font-size: 0.9em;
      color: #555;
    }
    .upload-goal-btn {
      background: #00838f;
      color: white;
      border: none;
      padding: 8px 15px;
      border-radius: 10px;
      font-size: 0.9em;
      cursor: pointer;
    }

    /* Модальные окна */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    .modal-content {
      background: white;
      padding: 25px;
      border-radius: 20px;
      width: 90%;
      max-width: 400px;
      text-align: center;
      position: relative;
      max-height: 80vh;
      overflow-y: auto;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
    }
    .modal input {
      width: 80%;
      padding: 12px;
      margin: 10px 0;
      border: 2px solid #00bfa5;
      border-radius: 10px;
      font-size: 1em;
    }
    .close-btn {
      position: absolute;
      top: 10px;
      right: 15px;
      font-size: 1.5em;
      color: #aaa;
      cursor: pointer;
    }
    .save-btn {
      background: #00838f;
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 10px;
      font-size: 1.1em;
      cursor: pointer;
      margin-top: 10px;
      transition: background-color 0.3s;
    }
    .save-btn:hover {
      background: #006064;
    }
    .event-item {
      padding: 10px;
      border-bottom: 1px dashed #eee;
      font-size: 1.1em;
      text-align: left;
      color: #333;
    }
    .event-item:last-child {
      border-bottom: none;
    }

    /* Стили для остальных вкладок */
    .profile {
      text-align: center;
      padding: 15px;
    }
    .avatar {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      margin: 0 auto 15px;
      object-fit: cover;
      border: 4px solid #00bfa5;
      background: #ddd;
      font-size: 4em;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    .name {
      font-size: 1.4em;
      font-weight: bold;
      margin: 10px 0;
    }
    .profile-info p {
      font-size: 1.1em;
      margin: 8px 0;
    }
    .edit-profile-btn {
      background: #4caf50;
      color: white;
      border: none;
      padding: 12px 20px;
      border-radius: 15px;
      font-size: 1.1em;
      cursor: pointer;
      margin-top: 20px;
    }
    .achievements {
      margin: 20px auto;
      width: 90%;
      max-width: 350px;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 15px;
      padding: 15px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .achievements h3 {
      color: #00838f;
      margin-bottom: 10px;
      text-align: center;
    }
    .achievement-item {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 10px;
      border-bottom: 1px dashed #eee;
      font-size: 1.1em;
    }
    .achievement-item:last-child {
      border-bottom: none;
    }
    .achievement-icon {
      font-size: 1.5em;
    }
    .achievement-locked {
      opacity: 0.5;
    }
    .upload-avatar {
      margin: 10px 0;
      font-size: 0.9em;
      color: #555;
    }
    .upload-btn {
      background: #00838f;
      color: white;
      border: none;
      padding: 8px 15px;
      border-radius: 10px;
      font-size: 0.9em;
      cursor: pointer;
    }
    input[type="file"] {
      display: none;
    }

    /* Вкладка ИИ-помощник */
    .ai-container {
      width: 100%;
      height: calc(100% - 80px);
      background: rgba(255, 255, 255, 0.95);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      overflow-y: auto;
      display: flex;
      flex-direction: column;
    }
    .ai-message {
      max-width: 80%;
      background: #e0f7fa;
      padding: 15px;
      border-radius: 12px;
      margin: 15px 10px;
      font-size: 1.2em;
      color: #006064;
    }
    .ai-message.user-message {
      background: #e8f5e9;
      align-self: flex-end;
      text-align: right;
    }
    .ai-input {
      display: flex;
      gap: 15px;
      margin-top: auto;
      padding: 10px;
      background: rgba(255, 255, 255, 0.95);
      position: sticky;
      bottom: 0;
    }
    .ai-input input {
      flex: 1;
      padding: 15px;
      border: 2px solid #00bfa5;
      border-radius: 12px;
      font-size: 1.2em;
    }
    .ai-input button {
      background: #00838f;
      color: white;
      border: none;
      padding: 15px 20px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 1.2em;
    }
    .ai-input button:hover {
      background: #006064;
    }
    .game-btn {
      display: block;
      margin: 15px auto;
      padding: 12px 20px;
      background: #ff9800;
      color: white;
      border-radius: 15px;
      text-decoration: none;
      font-size: 1.2em;
      width: 80%;
      max-width: 300px;
      text-align: center;
      transition: transform 0.2s;
    }
    .game-btn:hover {
      transform: scale(1.05);
    }
    .tabs {
      display: flex;
      background: rgba(255, 255, 255, 0.95);
      height: 60px;
      box-shadow: 0 -4px 10px rgba(0, 0, 0, 0.1);
      position: fixed;
      bottom: 0;
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
      left: 0;
      right: 0;
      z-index: 100;
    }
    .tab {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      font-size: 0.9em;
      color: #333;
      cursor: pointer;
    }
    .tab.active {
      color: #00838f;
      font-weight: bold;
    }
    .tab i {
      font-size: 1.5em;
      margin-bottom: 4px;
    }

    /* Стили для игры "Финансовый квест" */
    .game-container {
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      overflow-y: auto;
    }
    .map-container {
      position: relative;
      width: 100%;
      max-width: 350px;
      height: 600px;
      margin: 20px auto;
    }
    .level {
      position: absolute;
      width: 80px;
      height: 80px;
      background: #fff;
      border: 4px solid #ffca28;
      border-radius: 50%;
      text-align: center;
      line-height: 1.2;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      transition: transform 0.3s;
      color: #3e2723;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1em;
    }
    .level:hover {
      transform: scale(1.1);
    }
    .level.locked {
      opacity: 0.5;
      pointer-events: none;
    }
    .level p {
      margin: 0;
      padding: 5px;
    }
    .path {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 0;
    }
    svg {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
    .helper {
      max-width: 80px;
      position: fixed;
      bottom: 80px;
      right: 20px;
      z-index: 100;
    }
    canvas#confettiCanvas {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 2000;
    }
    .answer-btn {
      display: block;
      margin: 10px auto;
      padding: 10px;
      background: #e0f7fa;
      border: 2px solid #00bfa5;
      border-radius: 10px;
      cursor: pointer;
      width: 80%;
      font-size: 1em;
      color: #006064;
      transition: background-color 0.3s;
    }
    .answer-btn:hover {
      background: #b2ebf2;
    }
    .back-btn {
      display: block;
      margin: 15px auto;
      padding: 12px 20px;
      background: #ff9800;
      color: white;
      border-radius: 15px;
      text-decoration: none;
      font-size: 1.2em;
      width: 80%;
      max-width: 300px;
      text-align: center;
      transition: transform 0.2s;
    }
    .back-btn:hover {
      transform: scale(1.05);
    }
  </style>
</head>
<body>
  <!-- Приветственный экран -->
  <div id="welcomeScreen" class="welcome-screen">
    <h1>Добро пожаловать!</h1>
    <p>Начни копить в Инвест Мини! 🐷</p>
    <div class="welcome-image"></div>
  </div>

  <!-- Главная страница -->
  <div id="home" class="screen active-screen container">
    <h1>Мой счёт</h1>
    <i class="fas fa-bell notification-btn"></i>
    <div class="balance-display" id="balanceDisplay">
      <div>Монетки: <span id="coinBalance">0</span> ₿</div>
      <div>Рубли: <span id="rubleBalance">0</span> ₽</div>
    </div>
    <div class="card">
      <div class="card-header">Инвест Мини</div>
      <div class="card-number">●●●● ●●●● ●●●● 3456</div>
      <div class="card-name" id="cardName">Алексей Петров</div>
      <div class="card-valid">До 12/28</div>
    </div>
    <div class="transactions" id="transactionsBlock">
      <h3>Последние операции</h3>
      <div id="transactions-list" style="display: none;">
        <div class="transaction-item"><span>Карманные</span> <span class="amount income">+50</span></div>
        <div class="transaction-item"><span>Мороженое</span> <span class="amount expense">-20</span></div>
        <div class="transaction-item"><span>Уборка комнаты</span> <span class="amount income">+30</span></div>
        <div class="transaction-item"><span>Игрушка</span> <span class="amount expense">-45</span></div>
      </div>
    </div>
  </div>

  <!-- Вкладка "Копилка" -->
  <div id="piggy" class="screen container">
    <h1>Копилка 🐷</h1>
    <div class="piggy-container">
      <div class="piggy-scene">
        <img src="https://e7.pngegg.com/pngimages/739/532/png-clipart-pink-coin-bank-illustration-saving-piggy-bank-money-credit-card-piggy-bank-service-payment.png"
             alt="Свинка-копилка"
             class="piggy"
             id="piggy"
             style="left: 0;">
        <img id="goalImage" class="goal-image" alt="Цель">
        <div id="coinContainer"></div>
      </div>
      <div class="progress-bar">
        <div class="progress-fill" id="progressFill" style="width: 0%;"></div>
      </div>
      <div class="buttons">
        <button class="goal-btn"><i class="fas fa-bullseye"></i> Выбрать цель</button>
        <button class="add-btn"><i class="fas fa-coins"></i> Пополнить</button>
        <button class="remove-btn"><i class="fas fa-money-bill-wave"></i> Вывести</button>
        <button class="cheer-btn" id="cheerBtn"><i class="fas fa-heart"></i> Похвалить свинку</button>
      </div>
      <div class="info">
        <div>Цель: <span id="goalName">Велосипед</span></div>
        <div>Баланс копилки: <span id="currentBalance">0</span> ₿</div>
        <div id="piggyMessage"></div>
      </div>
    </div>
  </div>

  <!-- Вкладка "Игры" -->
  <div id="games" class="screen container">
    <h1>Обучающие игры 🎮</h1>
    <a href="#" class="game-btn">Угадай цену</a>
    <a href="#" class="game-btn">Заработай монетки</a>
    <a href="#" class="game-btn">Планируй покупку</a>
    <a href="#" class="game-btn">Финансовый квест</a>
    <div id="financialQuest" class="game-container" style="display: none;">
      <div class="map-container">
        <svg width="100%" height="100%">
          <path d="M175,550 Q150,450 175,400 Q200,350 175,300 Q150,250 175,200 Q200,150 175,100" stroke="#00bfa5" stroke-width="6" fill="none" />
        </svg>
        <div class="level" style="top: 500px; left: 150px;">Уровень 1</div>
        <div class="level" style="top: 400px; left: 150px;">Уровень 2</div>
        <div class="level" style="top: 300px; left: 150px;">Уровень 3</div>
        <div class="level" style="top: 200px; left: 150px;">Уровень 4</div>
        <div class="level" style="top: 100px; left: 150px;">Итог</div>
      </div>
      <img id="helper" class="helper" src="" alt="Помощник Дениска">
      <input type="file" id="helperUpload" accept="image/*">
      <button class="back-btn">Назад</button>
      <canvas id="confettiCanvas"></canvas>
    </div>
  </div>

  <!-- Вкладка "Профиль" -->
  <div id="profile" class="screen container">
    <h1>Мой профиль 👤</h1>
    <div class="profile">
      <img class="avatar" id="profileAvatar" src="" alt="Аватар">
      <p class="name" id="profileName">Алексей, 10 лет</p>
      <div class="profile-info">
        <p>Город: <span id="profileCity">Москва</span></p>
        <p>Уровень: Молодой экономист 🌟</p>
      </div>
      <div class="achievements">
        <h3>Твои достижения</h3>
        <div class="achievement-item">
          <span class="achievement-icon">🌟</span>
          <span>Первые шаги: Открыл счёт</span>
        </div>
        <div class="achievement-item" id="save50">
          <span class="achievement-icon achievement-locked">💰</span>
          <span>Копи 50 монет</span>
        </div>
        <div class="achievement-item" id="goalReached">
          <span class="achievement-icon achievement-locked">🎯</span>
          <span>Достиг цель</span>
        </div>
        <div class="achievement-item" id="playAllGames">
          <span class="achievement-icon achievement-locked">🎓</span>
          <span>Пройди все игры</span>
        </div>
      </div>
      <button class="edit-profile-btn">Редактировать</button>
    </div>
  </div>

  <!-- Вкладка "ИИ-помощник" -->
  <div id="ai" class="screen container">
    <h1>ИИ-помощник 🤖</h1>
    <div class="ai-container">
      <div class="ai-message">
        Привет! Я — твой финансовый помощник, работающий на DeepSeek. Спрашивай, как копить, тратить или зарабатывать! 😊
      </div>
      <div class="ai-input">
        <input type="text" id="aiInput" placeholder="Задай вопрос..." />
        <button>Отправить</button>
      </div>
    </div>
  </div>

  <!-- Навигация -->
  <div class="tabs">
    <div class="tab active">
      <i class="fas fa-home"></i>
      <span>Главная</span>
    </div>
    <div class="tab">
      <i class="fas fa-piggy-bank"></i>
      <span>Копилка</span>
    </div>
    <div class="tab">
      <i class="fas fa-gamepad"></i>
      <span>Игры</span>
    </div>
    <div class="tab">
      <i class="fas fa-robot"></i>
      <span>Помощник</span>
    </div>
    <div class="tab">
      <i class="fas fa-user"></i>
      <span>Я</span>
    </div>
  </div>

  <!-- Модальные окна -->
  <div id="profileModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2>Редактировать профиль</h2>
      <input type="text" id="modalName" placeholder="Имя">
      <input type="text" id="modalSurname" placeholder="Фамилия">
      <input type="number" id="modalAge" placeholder="Возраст">
      <input type="text" id="modalCity" placeholder="Город">
      <div class="upload-avatar">
        <label for="avatarUpload" class="upload-btn">Выбрать фото</label>
        <input type="file" id="avatarUpload" accept="image/*">
        <p id="fileName" style="margin-top: 5px; font-size: 0.8em; color: #555;"></p>
      </div>
      <button class="save-btn">Сохранить</button>
    </div>
  </div>

  <div id="goalModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2>Выбрать цель</h2>
      <input type="text" id="goalInputName" placeholder="На что копим?">
      <input type="number" id="goalInputAmount" placeholder="Сколько нужно?">
      <div class="upload-goal-image">
        <label for="goalImageUpload" class="upload-goal-btn">Выбрать картинку цели</label>
        <input type="file" id="goalImageUpload" accept="image/*">
        <p id="goalFileName" style="margin-top: 5px; font-size: 0.8em; color: #555;"></p>
      </div>
      <button class="save-btn">Сохранить</button>
    </div>
  </div>

  <div id="addModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2>Пополнить копилку</h2>
      <input type="number" id="addAmount" placeholder="Сумма пополнения">
      <button class="save-btn">Пополнить</button>
    </div>
  </div>

  <div id="removeModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2>Вывести из копилки</h2>
      <input type="number" id="removeAmount" placeholder="Сумма снятия">
      <button class="save-btn">Вывести</button>
    </div>
  </div>

  <div id="gameModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2 id="gameTitle">Игра</h2>
      <p id="gameText">Содержимое игры</p>
      <button class="save-btn" id="gameNextBtn">Далее</button>
    </div>
  </div>

  <div id="notificationsModal" class="modal">
    <div class="modal-content">
      <span class="close-btn">&times;</span>
      <h2>Уведомления 🔔</h2>
      <div id="notifications-list"></div>
    </div>
  </div>

  <script>
    // Данные пользователя
    const userData = {
      name: 'Алексей',
      surname: 'Петров',
      age: 10,
      city: 'Москва',
      goal: 'Велосипед',
      goalCurrent: 0,
      goalTotal: 200,
      balance: 0,
      progressPercent: 0,
      avatar: '',
      goalImage: '',
      events: [],
      achievements: {
        save50: false,
        goalReached: false,
        playAllGames: false
      },
      financialQuestProgress: {
        completedLevels: [1, 2, 3, 4],
        completedFinal: false
      }
    };

    // Данные для игры "Финансовый квест"
    const levels = {
      1: [
        {text: "Дениска: Деньги бывают бумажные и на карте.", question: "Где могут храниться деньги?", options: ["На карте", "На дереве"], correct: 0},
        {text: "Дениска: Копить — это значит не тратить сразу.", question: "Зачем люди копят?", options: ["Чтобы купить позже", "Чтобы выбросить"], correct: 0},
        {text: "Дениска: Банк — это место, где хранят деньги.", question: "Что делает банк?", options: ["Хранит деньги", "Прячет игрушки"], correct: 0}
      ],
      2: [
        {text: "Дениска: У каждой покупки есть цена.", question: "Что показывает цена?", options: ["Сколько стоит вещь", "Сколько она весит"], correct: 0},
        {text: "Дениска: Бюджет помогает планировать расходы.", question: "Зачем нужен бюджет?", options: ["Чтобы планировать деньги", "Чтобы рисовать"], correct: 0},
        {text: "Дениска: Монеты — тоже деньги.", question: "Монеты — это деньги?", options: ["Да", "Нет"], correct: 0}
      ],
      3: [
        {text: "Дениска: Деньги не берутся из воздуха.", question: "Откуда берутся деньги?", options: ["Из работы", "Из сна"], correct: 0},
        {text: "Дениска: Покупки нужно сравнивать.", question: "Почему сравнивают цены?", options: ["Чтобы выбрать лучшее", "Чтобы просто посмотреть"], correct: 0},
        {text: "Дениска: Нельзя тратить больше, чем есть.", question: "Что значит тратить разумно?", options: ["Не больше, чем есть", "Сколько хочешь"], correct: 0}
      ],
      4: [
        {text: "Дениска: Есть полезные и бесполезные траты.", question: "Что такое полезные траты?", options: ["На еду", "На 100 игрушек"], correct: 0},
        {text: "Дениска: Деньги можно заработать трудом.", question: "Как заработать деньги?", options: ["Помогая", "Лежа"], correct: 0},
        {text: "Дениска: Нужно уметь отложить.", question: "Что значит отложить деньги?", options: ["Не потратить сразу", "Подарить их"], correct: 0}
      ]
    };

    // Кэширование элементов
    let elements = {};
    let piggyAnimationInterval = null;
    let lastCheerTime = 0;
    let currentLevel = 0;
    let step = 0;

    // Загрузка данных
    function load() {
      try {
        const saved = localStorage.getItem('kidBankUserData');
        if (saved) {
          const data = JSON.parse(saved);
          Object.assign(userData, data);
          if (!userData.achievements) {
            userData.achievements = { save50: false, goalReached: false, playAllGames: false };
          }
          if (!userData.events) {
            userData.events = [];
          }
          if (!userData.goalImage) {
            userData.goalImage = '';
          }
          if (!userData.financialQuestProgress) {
            userData.financialQuestProgress = { completedLevels: [1, 2, 3, 4], completedFinal: false };
          }
        }
      } catch (e) {
        console.warn('Ошибка загрузки данных:', e);
      }
      updateUI();
      checkAchievements();
      switchTab('home');
    }

    // Сохранение данных
    function save() {
      try {
        localStorage.setItem('kidBankUserData', JSON.stringify(userData));
      } catch (e) {
        console.error('Ошибка сохранения данных:', e);
      }
    }

    // Добавление события
    function addEvent(message) {
      const timestamp = new Date().toLocaleString('ru-RU');
      userData.events.unshift({ message, timestamp });
      if (userData.events.length > 10) {
        userData.events.pop();
      }
      save();
      updateNotifications();
      if (document.getElementById('piggy').classList.contains('active-screen')) {
        showPiggyMessage(message);
      }
    }

    // Отображение сообщения в копилке
    function showPiggyMessage(message) {
      const piggyMessage = document.getElementById('piggyMessage');
      if (piggyMessage) {
        piggyMessage.textContent = message;
        setTimeout(() => {
          piggyMessage.textContent = '';
        }, 3000);
      }
    }

    // Обновление списка уведомлений
    function updateNotifications() {
      const list = document.getElementById('notifications-list');
      if (list) {
        list.innerHTML = userData.events.length
          ? userData.events.map(event => `<div class="event-item">${event.timestamp}: ${event.message}</div>`).join('')
          : '<div class="event-item">Нет новых уведомлений</div>';
      }
    }

    // Обновление интерфейса
    function updateUI() {
      if (elements.cardName) elements.cardName.textContent = `${userData.name} ${userData.surname}`;
      if (elements.profileName) elements.profileName.textContent = `${userData.name}, ${userData.age} лет`;
      if (elements.profileCity) elements.profileCity.textContent = userData.city;
      if (elements.goalName) elements.goalName.textContent = userData.goal;
      if (elements.currentBalance) elements.currentBalance.textContent = userData.goalCurrent.toFixed(2);
      if (elements.progressFill) {
        const maxPosition = 85;
        const width = Math.min(userData.progressPercent, maxPosition);
        elements.progressFill.style.width = `${width}%`;
      }
      if (elements.coinBalance) elements.coinBalance.textContent = userData.balance.toFixed(2);
      if (elements.rubleBalance) elements.rubleBalance.textContent = (userData.balance * 100).toFixed(2);

      // Обновление аватара
      const avatar = elements.profileAvatar;
      if (avatar) {
        if (userData.avatar) {
          avatar.src = userData.avatar;
          avatar.style.fontSize = '0';
        } else {
          avatar.src = '';
          avatar.textContent = '👦';
          avatar.style.fontSize = '4em';
        }
      }

      // Обновление позиции свинки
      const piggy = document.getElementById('piggy');
      if (piggy) {
        const maxPosition = 85;
        const position = Math.min(userData.progressPercent, maxPosition);
        piggy.style.left = `${position}%`;
      }

      // Обновление изображения цели
      const goalImage = document.getElementById('goalImage');
      if (goalImage) {
        goalImage.src = userData.goalImage || 'https://img.icons8.com/color/96/000000/star.png';
        goalImage.classList.toggle('visible', userData.progressPercent >= 100);
      }

      // Обновление прогресса игры "Финансовый квест"
      const levelElements = document.querySelectorAll('#financialQuest .level');
      levelElements.forEach((el, index) => {
        el.classList.remove('locked');
      });

      updateNotifications();
    }

    // Проверка достижений
    function checkAchievements() {
      if (userData.goalCurrent >= 50 && !userData.achievements.save50) {
        userData.achievements.save50 = true;
        addEvent('🎉 Достижение: "Копи 50 монет в копилке"!');
        showAchievementToast('🎉 Достижение: "Копи 50 монет"!');
      }
      if (userData.goalCurrent >= userData.goalTotal && !userData.achievements.goalReached) {
        userData.achievements.goalReached = true;
        addEvent('🎯 Достижение: "Достиг цель"!');
        showAchievementToast('🎯 Достижение: "Достиг цель"!');
      }
      if (userData.financialQuestProgress.completedFinal && !userData.achievements.playAllGames) {
        userData.achievements.playAllGames = true;
        addEvent('🎓 Достижение: "Пройди все игры"!');
        showAchievementToast('🎓 Достижение: "Пройди все игры"!');
      }
      save();
      updateUI();
    }

    // Уведомление о достижении
    function showAchievementToast(message) {
      const toast = document.createElement('div');
      toast.style.cssText = `
        position: fixed;
        top: 20px;
        left: 50%;
        transform: translateX(-50%);
        background: #ff9800;
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        font-size: 1.1em;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        z-index: 2000;
        animation: slideIn 0.5s, fadeOut 0.5s 2.5s forwards;
      `;
      toast.textContent = message;
      document.body.appendChild(toast);
      setTimeout(() => {
        if (toast.parentNode) {
          document.body.removeChild(toast);
        }
      }, 3000);
    }

    // Анимация свинки
    function triggerPiggyAnimation(type = 'bounce') {
      const piggy = document.getElementById('piggy');
      if (piggy) {
        piggy.classList.remove('bounce', 'gentle', 'cheer');
        piggy.classList.add(type);
        setTimeout(() => {
          piggy.classList.remove(type);
        }, type === 'bounce' ? 500 : type === 'gentle' ? 1000 : 800);
      }
    }

    // Периодическая анимация свинки
    function startPeriodicAnimation() {
      if (piggyAnimationInterval) {
        clearInterval(piggyAnimationInterval);
      }
      const piggyScreen = document.getElementById('piggy');
      if (piggyScreen.classList.contains('active-screen')) {
        piggyAnimationInterval = setInterval(() => {
          triggerPiggyAnimation('gentle');
        }, 5000);
      }
    }

    // Анимация падающих монет
    function triggerCoinAnimation() {
      const coinContainer = document.getElementById('coinContainer');
      if (coinContainer) {
        for (let i = 0; i < 5; i++) {
          const coin = document.createElement('div');
          coin.className = 'coin';
          coin.style.left = `${Math.random() * 100}%`;
          coin.style.animationDelay = `${Math.random() * 0.5}s`;
          coinContainer.appendChild(coin);
          setTimeout(() => {
            if (coin.parentNode) {
              coin.parentNode.removeChild(coin);
            }
          }, 1000);
        }
      }
    }

    // Похвалить свинку
    function cheerPiggy() {
      const now = Date.now();
      const cheerBtn = document.getElementById('cheerBtn');
      if (now - lastCheerTime < 60000) {
        showPiggyMessage('Свинка устала! Похвали её через минуту! 😅');
        return;
      }
      lastCheerTime = now;
      userData.balance += 1;
      addEvent('Похвалил свинку и получил 1 ₿! 🐷');
      triggerPiggyAnimation('cheer');
      triggerCoinAnimation();
      cheerBtn.disabled = true;
      setTimeout(() => {
        cheerBtn.disabled = false;
      }, 60000);
      save();
      updateUI();
    }

    // Переключение вкладок
    function switchTab(tabId) {
      document.querySelectorAll('.screen').forEach(el => el.classList.remove('active-screen'));
      const screen = document.getElementById(tabId);
      if (screen) {
        screen.classList.add('active-screen');
      }
      document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
      const tabs = ['home', 'piggy', 'games', 'ai', 'profile'];
      const index = tabs.indexOf(tabId);
      if (index !== -1) {
        const tab = document.querySelectorAll('.tab')[index];
        if (tab) tab.classList.add('active');
      }
      if (tabId === 'piggy') {
        triggerPiggyAnimation('gentle');
        startPeriodicAnimation();
      } else {
        if (piggyAnimationInterval) {
          clearInterval(piggyAnimationInterval);
          piggyAnimationInterval = null;
        }
      }
      if (tabId !== 'games') {
        document.getElementById('financialQuest').style.display = 'none';
        document.querySelectorAll('.game-btn').forEach(btn => btn.style.display = 'block');
      }
    }

    // Выход из игры
    function exitGame() {
      document.getElementById('financialQuest').style.display = 'none';
      document.querySelectorAll('.game-btn').forEach(btn => btn.style.display = 'block');
      closeModal('gameModal');
      const canvas = document.getElementById('confettiCanvas');
      if (canvas) canvas.style.display = 'none';
    }

    // Выпадающий список операций
    function setupTransactions() {
      const header = elements.transactionsBlock?.querySelector('h3');
      const list = document.getElementById('transactions-list');
      if (header && list) {
        header.addEventListener('click', () => {
          elements.transactionsBlock.classList.toggle('expanded');
          list.style.display = elements.transactionsBlock.classList.contains('expanded') ? 'block' : 'none';
        });
      }
    }

    // Открытие модальных окон
    function openModal(id) {
      console.log('Открывается модалка:', id);
      console.log('elements:', elements);
      if (id === 'profileModal') {
        if (elements.modalName) elements.modalName.value = userData.name || '';
        if (elements.modalSurname) elements.modalSurname.value = userData.surname || '';
        if (elements.modalAge) elements.modalAge.value = userData.age || '';
        if (elements.modalCity) elements.modalCity.value = userData.city || '';
        if (elements.avatarUpload) elements.avatarUpload.value = '';
        const fileName = document.getElementById('fileName');
        if (fileName) fileName.textContent = '';
      } else if (id === 'goalModal') {
        if (elements.goalInputName) elements.goalInputName.value = userData.goal || '';
        if (elements.goalInputAmount) elements.goalInputAmount.value = userData.goalTotal || '';
        if (elements.goalImageUpload) elements.goalImageUpload.value = '';
        const goalFileName = document.getElementById('goalFileName');
        if (goalFileName) goalFileName.textContent = '';
      } else if (id === 'notificationsModal') {
        updateNotifications();
      }
      const modal = document.getElementById(id);
      if (modal) modal.style.display = 'flex';
    }

    // Закрытие модальных окон
    function closeModal(id) {
      const modal = document.getElementById(id);
      if (modal) modal.style.display = 'none';
      const canvas = document.getElementById('confettiCanvas');
      if (canvas) canvas.style.display = 'none';
    }

    // Загрузка аватара
    document.getElementById('avatarUpload')?.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        if (file.size > 5 * 1024 * 1024) {
          alert('Файл слишком большой! Максимум 5 МБ.');
          return;
        }
        const reader = new FileReader();
        reader.onload = function(ev) {
          userData.avatar = ev.target.result;
          updateUI();
        };
        reader.readAsDataURL(file);
        document.getElementById('fileName').textContent = file.name;
      }
    });

    // Загрузка изображения цели
    document.getElementById('goalImageUpload')?.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        if (file.size > 5 * 1024 * 1024) {
          alert('Файл слишком большой! Максимум 5 МБ.');
          return;
        }
        const reader = new FileReader();
        reader.onload = function(ev) {
          userData.goalImage = ev.target.result;
          updateUI();
        };
        reader.readAsDataURL(file);
        document.getElementById('goalFileName').textContent = file.name;
      }
    });

    // Загрузка помощника для игры
    function loadHelper(e) {
      const file = e.target.files[0];
      if (file) {
        if (file.size > 5 * 1024 * 1024) {
          alert('Файл слишком большой! Максимум 5 МБ.');
          return;
        }
        const reader = new FileReader();
        reader.onload = function(event) {
          document.getElementById('helper').src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    }

    // Редактирование профиля
    function saveProfile() {
      const name = elements.modalName?.value.trim() || '';
      const surname = elements.modalSurname?.value.trim() || '';
      const age = parseInt(elements.modalAge?.value);
      const city = elements.modalCity?.value.trim() || '';

      if (!name || !surname || !city || isNaN(age) || age < 3 || age > 18) {
        alert('Введите корректные данные: имя, фамилию, город и возраст от 3 до 18.');
        return;
      }

      userData.name = name;
      userData.surname = surname;
      userData.age = age;
      userData.city = city;

      save();
      updateUI();
      closeModal('profileModal');
    }

    // Установка цели
    function saveGoal() {
      const title = elements.goalInputName?.value.trim() || '';
      const amount = parseFloat(elements.goalInputAmount?.value);
      if (!title || isNaN(amount) || amount <= 0) {
        alert('Введите корректную цель и сумму больше 0.');
        return;
      }
      userData.goal = title;
      userData.goalTotal = amount;
      userData.progressPercent = userData.goalTotal > 0 ? (userData.goalCurrent / userData.goalTotal) * 100 : 0;
      addEvent(`Новая цель: "${title}" на ${amount.toFixed(2)} ₿`);
      save();
      updateUI();
      closeModal('goalModal');
      checkAchievements();
    }

    // Пополнение копилки
    function addMoney() {
      const amount = parseFloat(elements.addAmount?.value);
      if (isNaN(amount) || amount <= 0) {
        alert('Введите корректную сумму больше 0.');
        return;
      }
      userData.goalCurrent += amount;
      addEvent(`Пополнение копилки на ${amount.toFixed(2)} ₿`);
      elements.addAmount.value = '';
      closeModal('addModal');
      triggerPiggyAnimation('bounce');
      triggerCoinAnimation();
      updateProgress();
    }

    // Снятие из копилки
    function removeMoney() {
      const amount = parseFloat(elements.removeAmount?.value);
      if (isNaN(amount) || amount <= 0 || amount > userData.goalCurrent) {
        alert('Введите корректную сумму, не превышающую баланс копилки.');
        return;
      }
      userData.goalCurrent -= amount;
      if (userData.goalCurrent < 0) userData.goalCurrent = 0;
      addEvent(`Снятие из копилки ${amount.toFixed(2)} ₿`);
      elements.removeAmount.value = '';
      closeModal('removeModal');
      triggerPiggyAnimation('bounce');
      triggerCoinAnimation();
      updateProgress();
    }

    // Обновление прогресса
    function updateProgress() {
      userData.progressPercent = userData.goalTotal > 0 ? Math.min((userData.goalCurrent / userData.goalTotal) * 100, 100) : 0;
      if (userData.progressPercent >= 100 && userData.goalTotal > 0) {
        addEvent(`Цель "${userData.goal}" достигнута! 🐷🎉`);
        showAchievementToast(`Поздравляем! Цель "${userData.goal}" достигнута! 🐷🎉`);
        triggerPiggyAnimation('cheer');
      }
      save();
      updateUI();
    }

    // Игры
    let gameStep = 0;
    let currentGame = '';
    let gamesCompleted = new Set();
    function openGame(game) {
      currentGame = game;
      gameStep = 0;
      if (game === 'financialQuest') {
        document.getElementById('financialQuest').style.display = 'block';
        document.querySelectorAll('.game-btn').forEach(btn => btn.style.display = 'none');
        updateUI();
      } else {
        document.getElementById('financialQuest').style.display = 'none';
        openModal('gameModal');
        playGame();
      }
    }
    function playGame() {
      const content = {
        guessPrice: [
          { title: 'Угадай цену! 💸', text: 'Попробуй угадать, сколько стоят вещи. Яблоко стоит около 5 монет.' },
          { text: '💡 Сравнивай цены, чтобы не переплачивать!' }
        ],
        earnCoins: [
          { title: 'Заработай монетки! 💰', text: 'Помоги с уборкой — получи 10 монет!' },
          { text: '💡 Работа, как помощь дома, приносит деньги!' }
        ],
        planPurchase: [
          { title: 'Планируй покупку! 🛒', text: 'У тебя 50 монет. Купи еду (30) или игрушку (40)?' },
          { text: '💡 Покупай нужное сначала, а на игрушки копи!' }
        ]
      };
      const steps = content[currentGame];
      if (!steps) return;
      const step = steps[gameStep];
      elements.gameTitle.textContent = step.title || 'Игра';
      elements.gameText.innerHTML = step.text;
      elements.gameNextBtn.textContent = gameStep < steps.length - 1 ? 'Далее' : 'Закрыть';
      elements.gameNextBtn.onclick = () => {
        if (gameStep < steps.length - 1) {
          gameStep++;
          playGame();
        } else {
          closeModal('gameModal');
          gamesCompleted.add(currentGame);
          if (gamesCompleted.size === 3 && userData.financialQuestProgress.completedFinal && !userData.achievements.playAllGames) {
            userData.achievements.playAllGames = true;
            addEvent('🎓 Достижение: "Пройди все игры"!');
            showAchievementToast('🎓 Достижение: "Пройди все игры"!');
            save();
          }
          updateUI();
        }
      };
    }

    // Финансовый квест
    function openLevel(n) {
      currentLevel = n;
      step = 0;
      showStep();
    }

    function showStep() {
      const q = levels[currentLevel][step];
      elements.gameTitle.textContent = `Уровень ${currentLevel}`;
      elements.gameText.innerHTML = `<div class="question">${q.text}</div><div class="question">${q.question}</div>` +
        q.options.map((opt, i) =>
          `<button class="answer-btn" onclick="checkAnswer(${i})">${opt}</button>`).join('');
      elements.gameNextBtn.style.display = 'none';
      openModal('gameModal');
    }

    function checkAnswer(i) {
      const correct = levels[currentLevel][step].correct;
      const feedback = i === correct ? '✅ Верно!' : '❌ Неверно.';
      elements.gameText.innerHTML += `<div style="margin-top:10px;"><strong>${feedback}</strong></div>`;
      elements.gameNextBtn.style.display = 'inline-block';
      elements.gameNextBtn.textContent = step < levels[currentLevel].length - 1 ? 'Далее' : 'Завершить';
      elements.gameNextBtn.onclick = nextStep;
    }

    function nextStep() {
      step++;
      if (step < levels[currentLevel].length) {
        showStep();
      } else {
        closeModal('gameModal');
        if (!userData.financialQuestProgress.completedLevels.includes(currentLevel)) {
          userData.financialQuestProgress.completedLevels.push(currentLevel);
          addEvent(`Пройден уровень ${currentLevel} в "Финансовом квесте"!`);
        }
        save();
        updateUI();
      }
    }

    function openFinalTest() {
      elements.gameTitle.textContent = 'Итог';
      elements.gameText.innerHTML = 'Поздравляем! Вы прошли все уровни 🎉<br>Вот вам салют!';
      elements.gameNextBtn.style.display = 'none';
      openModal('gameModal');
      startConfetti();
      if (!userData.financialQuestProgress.completedFinal) {
        userData.financialQuestProgress.completedFinal = true;
        addEvent('🎉 Пройден Финансовый квест!');
        gamesCompleted.add('financialQuest');
        checkAchievements();
      }
      save();
      updateUI();
    }

    function startConfetti() {
      const canvas = document.getElementById('confettiCanvas');
      canvas.style.display = 'block';
      const ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      let particles = [];

      for (let i = 0; i < 100; i++) {
        particles.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height - canvas.height,
          r: Math.random() * 6 + 4,
          d: Math.random() * 10,
          color: `hsl(${Math.random() * 360}, 100%, 50%)`,
          tilt: Math.random() * 10 - 10
        });
      }

      function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (let i = 0; i < particles.length; i++) {
          let p = particles[i];
          ctx.beginPath();
          ctx.fillStyle = p.color;
          ctx.fillRect(p.x, p.y, p.r, p.r);
          p.y += Math.cos(p.d) + 2;
          p.x += Math.sin(p.d);
          if (p.y > canvas.height) {
            p.y = -10;
            p.x = Math.random() * canvas.width;
          }
        }
        requestAnimationFrame(draw);
      }

      draw();
    }

    // ИИ-помощник с заглушкой
    async function askAI() {
      const input = document.getElementById('aiInput');
      const container = document.querySelector('#ai .ai-container');
      const text = input.value.trim();

      if (!text) return;

      const userMsg = document.createElement('div');
      userMsg.className = 'ai-message user-message';
      userMsg.textContent = text;
      container.insertBefore(userMsg, container.lastElementChild);

      const fallbackResponses = {
        'как заработать на велосипед': 'Чтобы заработать на велосипед 🚲, откладывай 20 ₿ каждую неделю! Также можешь помогать с уборкой или выполнять задания. 😊',
        'как копить деньги': 'Копи 10-20% от карманных денег! 💰 Поставь цель в копилке и следи за прогрессом. 😊',
        'что такое бюджет': 'Бюджет — это план трат. Раздели деньги: 50% на нужное, 30% копи, 20% на радость! 📊'
      };

      const questionLower = text.toLowerCase();
      let responseText = null;
      for (const key in fallbackResponses) {
        if (questionLower.includes(key)) {
          responseText = fallbackResponses[key];
          break;
        }
      }

      const botMsg = document.createElement('div');
      botMsg.className = 'ai-message';
      botMsg.textContent = responseText || 'Извини, я пока не могу ответить на этот вопрос. Попробуй спросить про деньги или копилку! 😊';
      container.insertBefore(botMsg, container.lastElementChild);
      input.value = '';
      container.scrollTop = container.scrollHeight;
    }

    // Закрытие модалок по клику вне
    window.onclick = function(e) {
      if (e.target.classList.contains('modal')) {
        e.target.style.display = 'none';
        const canvas = document.getElementById('confettiCanvas');
        if (canvas) canvas.style.display = 'none';
      }
    };

    // Инициализация
    document.addEventListener('DOMContentLoaded', () => {
      // Анимация приветственного экрана
      const welcomeScreen = document.getElementById('welcomeScreen');
      if (welcomeScreen) {
        setTimeout(() => {
          welcomeScreen.classList.add('fade-out');
          setTimeout(() => {
            welcomeScreen.style.display = 'none';
            document.body.style.overflow = 'auto';
          }, 500);
        }, 3000);
      }

      // Кэширование элементов
      elements = {
        cardName: document.getElementById('cardName'),
        profileName: document.getElementById('profileName'),
        profileCity: document.getElementById('profileCity'),
        goalName: document.getElementById('goalName'),
        currentBalance: document.getElementById('currentBalance'),
        progressFill: document.getElementById('progressFill'),
        transactionsBlock: document.getElementById('transactionsBlock'),
        profileAvatar: document.getElementById('profileAvatar'),
        modalName: document.getElementById('modalName'),
        modalSurname: document.getElementById('modalSurname'),
        modalAge: document.getElementById('modalAge'),
        modalCity: document.getElementById('modalCity'),
        goalInputName: document.getElementById('goalInputName'),
        goalInputAmount: document.getElementById('goalInputAmount'),
        addAmount: document.getElementById('addAmount'),
        removeAmount: document.getElementById('removeAmount'),
        gameTitle: document.getElementById('gameTitle'),
        gameText: document.getElementById('gameText'),
        gameNextBtn: document.getElementById('gameNextBtn'),
        avatarUpload: document.getElementById('avatarUpload'),
        achSave50: document.getElementById('save50'),
        achGoalReached: document.getElementById('goalReached'),
        achPlayAllGames: document.getElementById('playAllGames'),
        coinBalance: document.getElementById('coinBalance'),
        rubleBalance: document.getElementById('rubleBalance'),
        piggyMessage: document.getElementById('piggyMessage'),
        goalImage: document.getElementById('goalImage')
      };

      // Установка обработчиков событий
      document.querySelector('.notification-btn').addEventListener('click', () => openModal('notificationsModal'));
      document.querySelector('#piggy .goal-btn').addEventListener('click', () => openModal('goalModal'));
      document.querySelector('#piggy .add-btn').addEventListener('click', () => openModal('addModal'));
      document.querySelector('#piggy .remove-btn').addEventListener('click', () => openModal('removeModal'));
      document.querySelector('#piggy .cheer-btn').addEventListener('click', cheerPiggy);
      document.querySelector('#profile .edit-profile-btn').addEventListener('click', () => openModal('profileModal'));
      document.querySelector('#ai button').addEventListener('click', askAI);
      document.querySelectorAll('#games .game-btn')[0].addEventListener('click', () => openGame('guessPrice'));
      document.querySelectorAll('#games .game-btn')[1].addEventListener('click', () => openGame('earnCoins'));
      document.querySelectorAll('#games .game-btn')[2].addEventListener('click', () => openGame('planPurchase'));
      document.querySelectorAll('#games .game-btn')[3].addEventListener('click', () => openGame('financialQuest'));
      document.querySelectorAll('#financialQuest .level').forEach((el, i) => {
        if (i < 4) {
          el.addEventListener('click', () => openLevel(i + 1));
        } else {
          el.addEventListener('click', openFinalTest);
        }
      });
      document.querySelector('#financialQuest .back-btn').addEventListener('click', exitGame);
      document.querySelectorAll('.close-btn').forEach(btn => btn.addEventListener('click', () => closeModal(btn.parentElement.parentElement.id)));
      document.querySelector('#profileModal .save-btn').addEventListener('click', saveProfile);
      document.querySelector('#goalModal .save-btn').addEventListener('click', saveGoal);
      document.querySelector('#addModal .save-btn').addEventListener('click', addMoney);
      document.querySelector('#removeModal .save-btn').addEventListener('click', removeMoney);
      document.querySelector('#helperUpload').addEventListener('change', loadHelper);
      document.querySelectorAll('.tab').forEach((tab, i) => {
        tab.addEventListener('click', () => switchTab(['home', 'piggy', 'games', 'ai', 'profile'][i]));
      });

      setupTransactions();
      load();
    });
  </script>
</body>
</html>
