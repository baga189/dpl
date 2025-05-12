<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Daryn Premier Liga (DPL)</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background-color: #0a0f3c;
      color: white;
    }
    header {
      background-color: #1e3a8a;
      padding: 20px;
      text-align: center;
    }
    nav a {
      margin: 0 15px;
      text-decoration: none;
      color: #fff;
    }
    main {
      padding: 30px;
      text-align: center;
    }
    section {
      margin-top: 40px;
    }
    table {
      width: 90%;
      margin: 20px auto;
      background-color: #1e3a8a;
      border-collapse: collapse;
      color: white;
    }
    th, td {
      border: 1px solid #fff;
      padding: 10px;
      text-align: center;
    }
    input, select {
      padding: 8px;
      margin: 5px;
    }
    footer {
      text-align: center;
      padding: 10px;
      background-color: #1e3a8a;
      margin-top: 50px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Daryn Premier Liga (DPL)</h1>
    <nav>
      <a href="#home">Главная</a>
      <a href="#teams">Команды</a>
      <a href="#matches">Матчи</a>
      <a href="#table">Таблица</a>
    </nav>
  </header>

  <main>
    <section id="home">
      <h2>Добро пожаловать в DPL!</h2>
      <p>Только организатор может ввести результаты матчей. Следите за событиями и турнирной таблицей!</p>
    </section>

    <section id="teams">
      <h2>Команды</h2>
      <ul id="teamList"></ul>
    </section>

    <section id="matches">
      <h2>Матчи и события</h2>
      <div id="matchList"></div>

      <h3>Добавить матч (только для администратора)</h3>
      <form id="matchForm">
        <input type="password" id="adminPass" placeholder="Пароль">
        <br>
        <select id="homeTeam"></select>
        <input type="number" id="homeGoals" min="0" placeholder="Голы хозяев">
        <br>
        <select id="awayTeam"></select>
        <input type="number" id="awayGoals" min="0" placeholder="Голы гостей">
        <br>
        <button type="submit">Добавить матч</button>
        <p id="errorMsg" style="color:red;"></p>
      </form>
    </section>

    <section id="table">
      <h2>Турнирная таблица</h2>
      <table id="standings">
        <thead>
          <tr>
            <th>#</th>
            <th>Команда</th>
            <th>И</th>
            <th>В</th>
            <th>Н</th>
            <th>П</th>
            <th>ГЗ / ГП</th>
            <th>О</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 Daryn Premier Liga</p>
  </footer>

  <script>
    const adminPassword = "dpladmin";
    const teams = ["Команда 1", "Команда 2", "Команда 3", "Команда 4", "Команда 5", "Команда 6"];

    let matches = [];

    // Загрузка матчей из localStorage
    const savedMatches = localStorage.getItem("dpl_matches");
    if (savedMatches) {
      matches = JSON.parse(savedMatches);
    }

    const teamList = document.getElementById("teamList");
    const homeTeamSelect = document.getElementById("homeTeam");
    const awayTeamSelect = document.getElementById("awayTeam");

    // Загрузка команд
    teams.forEach((team, index) => {
      const li = document.createElement("li");
      li.textContent = team;
      teamList.appendChild(li);

      const opt1 = new Option(team, index);
      const opt2 = new Option(team, index);
      homeTeamSelect.add(opt1);
      awayTeamSelect.add(opt2);
    });

    function updateMatchList() {
      const container = document.getElementById("matchList");
      container.innerHTML = "";
      matches.forEach(match => {
        const div = document.createElement("div");
        div.innerHTML = `<h4>${teams[match.home]} vs ${teams[match.away]}</h4>
                         <p>Счёт: ${match.goalsHome} - ${match.goalsAway}</p>`;
        container.appendChild(div);
      });
    }

    function updateTable() {
      const tableData = teams.map(name => ({
        name,
        played: 0, win: 0, draw: 0, loss: 0,
