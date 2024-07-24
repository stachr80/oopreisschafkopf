#oopreisschafkopf
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spieler-Tabellen-Generator</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            max-width: 100%; 
            margin: 0 auto; 
            padding: 10px; 
            font-size: 16px;
        }
        input, button { 
            margin: 5px 0; 
            padding: 10px; 
            font-size: 16px;
            width: 100%;
            box-sizing: border-box;
        }
        table { 
            border-collapse: collapse; 
            width: 100%; 
            margin-top: 20px; 
        }
        th, td { 
            border: 1px solid black; 
            padding: 5px; 
            text-align: center; 
        }
        .total-row { 
            font-weight: bold; 
            background-color: #f0f0f0; 
        }
        .invalid-row { 
            background-color: #ffcccc; 
        }
        input[type="number"] { 
            width: 100%; 
            padding: 5px;
            -moz-appearance: textfield;
        }
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        @media (max-width: 600px) {
            table {
                font-size: 14px;
            }
            th, td {
                padding: 3px;
            }
            input[type="number"] {
                padding: 2px;
            }
        }
    </style>
</head>
<body>
    <h1>Spieler-Tabellen-Generator</h1>
    <div id="playerInput">
        <h2>Spielernamen eingeben:</h2>
    </div>
    <button onclick="addPlayers()">Spieler hinzufügen</button>
    <div id="gameInput" style="display: none;">
        <h2>Anzahl der Spiele:</h2>
        <input type="number" id="numGames" min="1">
        <button onclick="createTable()">Tabelle erstellen</button>
    </div>
    <div id="tableContainer"></div>

    <script>
        let players = [];

        function addPlayers() {
            players = [];
            for (let i = 1; i <= 4; i++) {
                const name = document.getElementById(`player${i}`).value.trim();
                if (name) players.push(name);
            }
            if (players.length === 4) {
                document.getElementById('playerInput').style.display = 'none';
                document.getElementById('gameInput').style.display = 'block';
            } else {
                alert('Bitte geben Sie genau 4 Spielernamen ein.');
            }
        }

        function createTable() {
            const numGames = parseInt(document.getElementById('numGames').value);
            if (isNaN(numGames) || numGames < 1) {
                alert('Bitte geben Sie eine gültige Anzahl von Spielen ein.');
                return;
            }

            let tableHTML = '<table><tr><th>Spiel</th>';
            players.forEach(player => {
                tableHTML += `<th>${player}</th>`;
            });
            tableHTML += '</tr>';

            for (let i = 1; i <= numGames; i++) {
                tableHTML += `<tr><td>Spiel ${i}</td>`;
                players.forEach((_, index) => {
                    tableHTML += `<td><input type="number" data-row="${i}" data-col="${index}" onchange="validateRow(${i})"></td>`;
                });
                tableHTML += '</tr>';
            }

            tableHTML += '<tr class="total-row"><td>Gesamt</td>';
            players.forEach((_, index) => {
                tableHTML += `<td id="total-${index}">0</td>`;
            });
            tableHTML += '</tr></table>';

            document.getElementById('tableContainer').innerHTML = tableHTML;
        }

        function validateRow(rowNum) {
            const inputs = document.querySelectorAll(`input[data-row="${rowNum}"]`);
            let sum = 0;
            inputs.forEach(input => {
                sum += parseInt(input.value) || 0;
            });

            const row = inputs[0].closest('tr');
            if (sum !== 0) {
                row.classList.add('invalid-row');
            } else {
                row.classList.remove('invalid-row');
            }

            updateTotal();
        }

        function updateTotal() {
            players.forEach((_, playerIndex) => {
                let total = 0;
                const inputs = document.querySelectorAll(`input[data-col="${playerIndex}"]`);
                inputs.forEach(input => {
                    total += parseInt(input.value) || 0;
                });
                document.getElementById(`total-${playerIndex}`).textContent = total;
            });
        }

        // Spieler-Eingabefelder erstellen
        for (let i = 1; i <= 4; i++) {
            const input = document.createElement('input');
            input.type = 'text';
            input.id = `player${i}`;
            input.placeholder = `Spieler ${i}`;
            document.getElementById('playerInput').appendChild(input);
        }
    </script>
</body>
</html>
