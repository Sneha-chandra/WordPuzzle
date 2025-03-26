<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pirate Wordle Adventure</title>
    
    <style>
        body {
            text-align: center;
            background: url('https://www.publicdomainpictures.net/pictures/300000/velka/treasure-island-background.jpg') no-repeat center center fixed;
            background-size: cover;
            color: white;
            font-family: Arial, sans-serif;
            user-select: none; /* Disable text selection */
        }
        
        #game-board {
            display: grid;
            grid-template-columns: repeat(5, 60px);
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .letter-box {
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px solid white;
            font-size: 24px;
            font-weight: bold;
            text-transform: uppercase;
        }

        .green { background-color: green; }
        .yellow { background-color: yellow; }
        .gray { background-color: darkgray; }
    </style>

    <script>
        const words = ["ocean", "skull", "chest", "sails", "cannon", "whale", "storm", "rumor", "anchor", "shark"];
        let targetWord = words[Math.floor(Math.random() * words.length)];
        let attempts = [];
        let maxAttempts = 6;

        document.addEventListener("keydown", function(event) {
            if (event.key === "Enter" && attempts.length < maxAttempts) {
                let guess = prompt("Enter a 5-letter word:").toLowerCase();
                if (guess.length !== 5) return;

                let row = document.createElement("div");
                row.classList.add("word-row");
                
                let clue = "";
                for (let i = 0; i < 5; i++) {
                    let box = document.createElement("div");
                    box.classList.add("letter-box");
                    box.textContent = guess[i];
                    
                    if (guess[i] === targetWord[i]) box.classList.add("green");
                    else if (targetWord.includes(guess[i])) box.classList.add("yellow");
                    else box.classList.add("gray");
                    
                    row.appendChild(box);
                    if (attempts.length >= 4) clue = targetWord.replace(/[^aeiou]/g, "_");
                }

                document.getElementById("game-board").appendChild(row);
                attempts.push(guess);

                if (attempts.length === 5) document.getElementById("clue").textContent = `Clue: ${clue}`;
                
                if (guess === targetWord) {
                    setTimeout(() => alert("🏴‍☠️ Arrr! Ye found the treasure!"), 100);
                    location.reload();
                } else if (attempts.length >= maxAttempts) {
                    setTimeout(() => alert(`☠️ Ye be doomed! The word was: ${targetWord}`), 100);
                    location.reload();
                }
            }
        });

        // Disable right-click and inspect element
        document.addEventListener("contextmenu", (event) => event.preventDefault());
        document.addEventListener("keydown", (event) => {
            if (event.key === "F12" || (event.ctrlKey && event.shiftKey && event.key === "I")) {
                event.preventDefault();
            }
        });
    </script>
</head>
<body>
    <h1>🏴‍☠️ Pirate Wordle Adventure</h1>
    <p>Guess the 5-letter pirate word!</p>
    <div id="game-board"></div>
    <p id="clue"></p>
</body>
</html>
