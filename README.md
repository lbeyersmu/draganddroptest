<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Dice Duel</title>
  <style>
    * { box-sizing: border-box; }

    :root {
      --bg: #eef2ff;
      --panel: #ffffff;
      --text: #202344;
      --primary: #5143c8;
      --primary-dark: #3d319d;
      --soft: #e8e8f0;
      --soft-hover: #d5d5e3;
      --focus: #c57700;
      --shadow: rgba(32, 35, 68, 0.09);
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: 20px;
      font-family: system-ui, sans-serif;
      background: var(--bg);
      color: var(--text);
    }

    main {
      width: 100%;
      max-width: 440px;
      padding: 32px;
      text-align: center;
      background: var(--panel);
      border-radius: 24px;
      box-shadow: 0 12px 40px var(--shadow);
    }

    h1 {
      margin-top: 0;
      margin-bottom: 0.25rem;
    }

    .players {
      display: flex;
      justify-content: space-around;
      gap: 20px;
      margin: 28px 0;
    }

    .player {
      flex: 1;
    }

    .die {
      display: block;
      font-size: 80px;
      line-height: 1.2;
      min-height: 96px;
      transition: transform 0.15s ease;
    }

    .score {
      font-weight: 700;
      margin-bottom: 0;
    }

    button {
      padding: 12px 22px;
      border: 0;
      border-radius: 10px;
      font: inherit;
      font-weight: 700;
      cursor: pointer;
      background: var(--primary);
      color: white;
      transition: background 0.2s ease, transform 0.15s ease;
    }

    button:hover {
      background: var(--primary-dark);
    }

    button:active {
      transform: translateY(1px);
    }

    button:focus-visible {
      outline: 3px solid var(--focus);
      outline-offset: 4px;
    }

    button:disabled {
      opacity: 0.5;
      cursor: default;
    }

    .reset {
      margin-left: 8px;
      background: var(--soft);
      color: var(--text);
    }

    .reset:hover {
      background: var(--soft-hover);
    }

    #message {
      min-height: 48px;
      margin: 0 0 20px;
      font-weight: 600;
    }
  </style>
</head>
<body>
  <main>
    <h1>🎲 Dice Duel</h1>
    <p>Higher roll earns a point. First to 5 wins!</p>

    <div class="players">
      <section class="player" aria-label="Your dice and score">
        <h2>You</h2>
        <span class="die" id="youDie" role="img" aria-label="Die showing 1">⚀</span>
        <p class="score">Points: <span id="youScore">0</span></p>
      </section>

      <section class="player" aria-label="Computer dice and score">
        <h2>Computer</h2>
        <span class="die" id="cpuDie" role="img" aria-label="Die showing 1">⚀</span>
        <p class="score">Points: <span id="cpuScore">0</span></p>
      </section>
    </div>

    <p id="message" role="status" aria-live="polite">
      Ready? Roll the dice!
    </p>

    <button id="roll" type="button">Roll Dice</button>
    <button id="reset" class="reset" type="button">New Game</button>
  </main>

  <script>
    const WINNING_SCORE = 5;
    const faces = ["⚀", "⚁", "⚂", "⚃", "⚄", "⚅"];

    const rollButton = document.getElementById("roll");
    const resetButton = document.getElementById("reset");
    const message = document.getElementById("message");

    let yourPoints = 0;
    let computerPoints = 0;

    function randomDieValue() {
      return Math.floor(Math.random() * faces.length) + 1;
    }

    function showDie(id, value) {
      const die = document.getElementById(id);
      die.textContent = faces[value - 1];
      die.setAttribute("aria-label", `Die showing ${value}`);
    }

    function updateScores() {
      document.getElementById("youScore").textContent = String(yourPoints);
      document.getElementById("cpuScore").textContent = String(computerPoints);
    }

    function isGameOver() {
      return yourPoints >= WINNING_SCORE || computerPoints >= WINNING_SCORE;
    }

    function setMessage(text) {
      message.textContent = text;
    }

    function finishGame() {
      rollButton.disabled = true;
      const winnerText = yourPoints > computerPoints
        ? "🎉 You won the game!"
        : "Computer won! Try another game.";
      setMessage(winnerText);
    }

    function resetGame() {
      yourPoints = 0;
      computerPoints = 0;
      updateScores();
      showDie("youDie", 1);
      showDie("cpuDie", 1);
      setMessage("Ready? Roll the dice!");
      rollButton.disabled = false;
      rollButton.focus();
    }

    rollButton.addEventListener("click", () => {
      if (isGameOver()) {
        return;
      }

      const yours = randomDieValue();
      const computers = randomDieValue();

      showDie("youDie", yours);
      showDie("cpuDie", computers);

      let result;

      if (yours > computers) {
        yourPoints += 1;
        result = "You earn a point!";
      } else if (computers > yours) {
        computerPoints += 1;
        result = "Computer earns a point!";
      } else {
        result = "It's a tie! No points.";
      }

      updateScores();

      if (isGameOver()) {
        finishGame();
        return;
      }

      setMessage(`You rolled ${yours}. Computer rolled ${computers}. ${result}`);
    });

    resetButton.addEventListener("click", resetGame);
  </script>
</body>
</html>
