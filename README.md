<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Dice Duel</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: 20px;
      font-family: system-ui, sans-serif;
      background: #eef2ff;
      color: #202344;
    }

    main {
      width: 100%;
      max-width: 440px;
      padding: 32px;
      text-align: center;
      background: white;
      border-radius: 24px;
      box-shadow: 0 12px 40px #20234415;
    }

    h1 { margin-top: 0; }

    .players {
      display: flex;
      justify-content: space-around;
      gap: 20px;
      margin: 28px 0;
    }

    .die {
      display: block;
      font-size: 80px;
      line-height: 1.2;
    }

    .score { font-weight: bold; }

    button {
      padding: 12px 22px;
      border: 0;
      border-radius: 10px;
      font: inherit;
      font-weight: bold;
      cursor: pointer;
      background: #5143c8;
      color: white;
    }

    button:hover { background: #3d319d; }
    button:focus-visible { outline: 3px solid #c57700; outline-offset: 4px; }
    button:disabled { opacity: 0.5; cursor: default; }

    .reset {
      margin-left: 8px;
      background: #e8e8f0;
      color: #202344;
    }

    .reset:hover { background: #d5d5e3; }
    #message { min-height: 48px; }
  </style>
</head>
<body>
  <main>
    <h1>🎲 Dice Duel</h1>
    <p>Higher roll earns a point. First to 5 wins!</p>

    <div class="players">
      <section aria-label="Your dice and score">
        <h2>You</h2>
        <span class="die" id="youDie" role="img" aria-label="Die showing 1">⚀</span>
        <p class="score">Points: <span id="youScore">0</span></p>
      </section>

      <section aria-label="Computer dice and score">
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
    const faces = ["⚀", "⚁", "⚂", "⚃", "⚄", "⚅"];
    const rollButton = document.getElementById("roll");
    const message = document.getElementById("message");
    let yourPoints = 0;
    let computerPoints = 0;

    function showDie(id, value) {
      const die = document.getElementById(id);
      die.textContent = faces[value - 1];
      die.setAttribute("aria-label", `Die showing ${value}`);
    }

    function updateScores() {
      document.getElementById("youScore").textContent = yourPoints;
      document.getElementById("cpuScore").textContent = computerPoints;
    }

    rollButton.addEventListener("click", () => {
      const yours = Math.floor(Math.random() * 6) + 1;
      const computers = Math.floor(Math.random() * 6) + 1;

      showDie("youDie", yours);
      showDie("cpuDie", computers);

      let result;
      if (yours > computers) {
        yourPoints++;
        result = "You earn a point!";
      } else if (computers > yours) {
        computerPoints++;
        result = "Computer earns a point!";
      } else {
        result = "It's a tie! No points.";
      }

      updateScores();

      if (yourPoints === 5 || computerPoints === 5) {
        result = yourPoints === 5
          ? "🎉 You won the game!"
          : "Computer won! Try another game.";
        rollButton.disabled = true;
      }

      message.textContent =
        `You rolled ${yours}. Computer rolled ${computers}. ${result}`;
    });

    document.getElementById("reset").addEventListener("click", () => {
      yourPoints = 0;
      computerPoints = 0;
      updateScores();
      showDie("youDie", 1);
      showDie("cpuDie", 1);
      message.textContent = "Ready? Roll the dice!";
      rollButton.disabled = false;
      rollButton.focus();
    });
  </script>
</body>
</html>
