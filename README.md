# Work
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Sort It! Waste Game</title>

  <style>
    body {
      margin: 0;
      padding: 30px;
      background: #e8f5e9;
      color: #20352a;
      font-family: Arial, sans-serif;
      text-align: center;
    }

    main {
      max-width: 850px;
      margin: auto;
      padding: 30px;
      border-radius: 20px;
      background: white;
      box-shadow: 0 8px 25px #0002;
    }

    h1 {
      margin-bottom: 5px;
      font-size: 3rem;
    }

    .score {
      margin: 20px 0;
      font-size: 1.2rem;
      font-weight: bold;
    }

    #item-card {
      width: 230px;
      min-height: 170px;
      margin: 25px auto;
      padding: 20px;
      border: 3px dashed #6c9275;
      border-radius: 18px;
      background: #fffdf5;
      cursor: grab;
      display: grid;
      place-content: center;
      gap: 10px;
      font-size: 1.2rem;
    }

    #item-card span {
      font-size: 4rem;
    }

    #message {
      min-height: 25px;
      font-weight: bold;
    }

    .bins {
      display: flex;
      gap: 15px;
      justify-content: center;
      margin-top: 25px;
    }

    .bin {
      width: 180px;
      min-height: 160px;
      border: 0;
      border-radius: 18px 18px 8px 8px;
      color: white;
      font-size: 1.1rem;
      font-weight: bold;
      cursor: pointer;
    }

    .bin span {
      display: block;
      margin-bottom: 10px;
      font-size: 3rem;
    }

    .organic { background: #3f8157; }
    .recycling { background: #2879ad; }
    .landfill { background: #5c5c5c; }

    .bin:hover {
      transform: translateY(-5px);
      filter: brightness(1.1);
    }

    #reset-button {
      margin-top: 30px;
      padding: 10px 18px;
      border: 0;
      border-radius: 8px;
      background: #f7bf4f;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
    }

    @media (max-width: 650px) {
      .bins {
        flex-direction: column;
        align-items: center;
      }

      .bin {
        width: 100%;
        min-height: 100px;
      }
    }
  </style>
</head>

<body>
  <main>
    <h1>Sort It!</h1>
    <p>Drag the rubbish into a bin, or click a bin to choose it.</p>

    <p class="score">Score: <span id="score">0</span> / 6</p>

    <div id="item-card" draggable="true">
      Loading...
    </div>

    <p id="message"></p>

    <div class="bins">
      <button class="bin organic" data-bin="organic">
        <span>🍂</span>
        Organics
      </button>

      <button class="bin recycling" data-bin="recycling">
        <span>♻️</span>
        Recycling
      </button>

      <button class="bin landfill" data-bin="landfill">
        <span>🗑️</span>
        Landfill
      </button>
    </div>

    <button id="reset-button">Restart game</button>
  </main>

  <script>
    // Each object is one rubbish item in the game.
    // Check local council rules before using these classifications in your assessment.
    const rubbish = [
      { name: "Banana peel", icon: "🍌", bin: "organic" },
      { name: "Apple core", icon: "🍎", bin: "organic" },
      { name: "Aluminium can", icon: "🥫", bin: "recycling" },
      { name: "Plastic drink bottle", icon: "🧴", bin: "recycling" },
      { name: "Broken ceramic mug", icon: "☕", bin: "landfill" },
      { name: "Chip packet", icon: "🥔", bin: "landfill" }
    ];

    // These variables track the current game.
    let currentItem = 0;
    let score = 0;
    let locked = false;

    // Find the HTML elements that JavaScript will update.
    const card = document.querySelector("#item-card");
    const scoreText = document.querySelector("#score");
    const message = document.querySelector("#message");
    const bins = document.querySelectorAll(".bin");
    const resetButton = document.querySelector("#reset-button");

    // Show the current rubbish item on the screen.
    function showItem() {
      locked = false;

      // If every item has been sorted, end the game.
      if (currentItem === rubbish.length) {
        card.innerHTML = "<span>🏆</span><strong>Game complete!</strong>";
        card.draggable = false;
        message.textContent = "Well done! Press Restart game to play again.";
        return;
      }

      const item = rubbish[currentItem];

      card.innerHTML = `
        <span>${item.icon}</span>
        <strong>${item.name}</strong>
      `;

      card.draggable = true;
      message.textContent = "Choose the correct bin.";
    }

    // Check whether the player's chosen bin is correct.
    function chooseBin(chosenBin) {
      // Stop double-clicking while the game changes to the next item.
      if (locked || currentItem === rubbish.length) {
        return;
      }

      locked = true;

      const item = rubbish[currentItem];

      if (chosenBin === item.bin) {
        score += 1;
        scoreText.textContent = score;
        message.textContent = "Correct! Well done.";

        // Wait briefly, then show the next item.
        setTimeout(() => {
          currentItem += 1;
          showItem();
        }, 700);
      } else {
        message.textContent = "Not quite. Try another bin.";
        locked = false;
      }
    }

    // Let the player drag the rubbish card.
    card.addEventListener("dragstart", (event) => {
      event.dataTransfer.setData("text/plain", "rubbish");
    });

    // Add click and drop events to every bin.
    bins.forEach((bin) => {
      bin.addEventListener("click", () => {
        chooseBin(bin.dataset.bin);
      });

      bin.addEventListener("dragover", (event) => {
        event.preventDefault();
      });

      bin.addEventListener("drop", (event) => {
        event.preventDefault();
        chooseBin(bin.dataset.bin);
      });
    });

    // Restart the game.
    resetButton.addEventListener("click", () => {
      currentItem = 0;
      score = 0;
      scoreText.textContent = score;
      showItem();
    });

    // Start the first game.
    showItem();
  </script>
</body>
</html>
