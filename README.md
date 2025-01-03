

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Підрахунок балів Skull King</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-image: url('https://orgoglionerd.it/wp-content/uploads/2023/06/Skull-king-carte-scatola-768x512.jpg.webp');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            padding: 20px;
            text-align: center;
            color: white;
        }
        table {
            margin: 20px auto;
            border-collapse: collapse;
            width: 80%;
            background: rgba(255, 255, 255, 0.8);
            border: 1px solid #ccc;
        }
        th, td {
            padding: 10px;
            border: 1px solid #ccc;
            text-align: center;
        }
        th {
            background-color: #4CAF50;
            color: white;
        }
        td {
            color: black; /* Чорний колір для написів у клітинках */
        }
        input {
            width: 60px;
            padding: 5px;
            text-align: center;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        input.error {
            border-color: red;
            background-color: #f8d7da; /* Світло-червоний фон для помилки */
        }
        .player-name {
            width: 100px;
            padding: 5px;
            text-align: center;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        tfoot td {
            font-weight: bold;
            background-color: #e0e0e0;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .error-message {
            color: red;
            font-size: 12px;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <h1>Підрахунок балів Skull King</h1>
    <table id="scoreTable">
        <thead>
            <tr>
                <th>Раунд</th>
                <!-- Поля для зміни імен гравців -->
                <th><input type="text" class="player-name" value="Гравець 1"></th>
                <th><input type="text" class="player-name" value="Гравець 2"></th>
                <th><input type="text" class="player-name" value="Гравець 3"></th>
                <th><input type="text" class="player-name" value="Гравець 4"></th>
                <th><input type="text" class="player-name" value="Гравець 5"></th>
                <th><input type="text" class="player-name" value="Гравець 6"></th>
                <th><input type="text" class="player-name" value="Гравець 7"></th>
                <th><input type="text" class="player-name" value="Гравець 8"></th>
            </tr>
        </thead>
        <tbody>
            <!-- Автоматично генеруються рядки для раундів -->
        </tbody>
        <tfoot>
            <tr>
                <td>Сума</td>
                <td id="sum1">0</td>
                <td id="sum2">0</td>
                <td id="sum3">0</td>
                <td id="sum4">0</td>
                <td id="sum5">0</td>
                <td id="sum6">0</td>
                <td id="sum7">0</td>
                <td id="sum8">0</td>
            </tr>
        </tfoot>
    </table>

    <script>
        // Кількість раундів та гравців
        const rounds = 8;
        const players = 8;

        // Генерація рядків для раундів
        const tbody = document.querySelector("#scoreTable tbody");
        for (let i = 1; i <= rounds; i++) {
            const row = document.createElement("tr");
            row.innerHTML = `<td style="color: black;">Раунд ${i}</td>`;
            for (let j = 1; j <= players; j++) {
                row.innerHTML += `<td><input type="number" class="score" data-player="${j}" min="0" step="1"></td>`;
            }
            tbody.appendChild(row);
        }

        // Функція для автоматичного підрахунку сум
        function calculateSums() {
            const sums = Array(players).fill(0); // Сума для кожного гравця
            const scores = document.querySelectorAll(".score");

            // Спочатку очищаємо помилки
            scores.forEach(input => {
                input.classList.remove('error');
                const errorMessage = input.nextElementSibling;
                if (errorMessage) {
                    errorMessage.remove();
                }
            });

            scores.forEach(input => {
                const player = parseInt(input.dataset.player) - 1;
                const value = input.value;
                let numericValue = parseInt(value);

                // Якщо клітинка порожня або введено некоректне значення
                if (value === "" || isNaN(numericValue)) {
                    numericValue = 0; // Якщо порожнє або некоректне - приймається 0
                    if (value !== "") { // Якщо введено нецифрове значення
                        input.classList.add('error');
                        if (!input.nextElementSibling || !input.nextElementSibling.classList.contains('error-message')) {
                            const errorMessage = document.createElement('div');
                            errorMessage.classList.add('error-message');
                            errorMessage.textContent = 'Введіть правильне число';
                            input.insertAdjacentElement('afterend', errorMessage);
                        }
                    }
                }

                sums[player] += numericValue;
            });

            // Відображення сум у нижньому рядку
            for (let i = 0; i < sums.length; i++) {
                document.getElementById(`sum${i + 1}`).innerText = sums[i];
            }
        }

        // Додавання обробника події для кожного поля вводу
        const scoreInputs = document.querySelectorAll(".score");
        scoreInputs.forEach(input => {
            input.addEventListener("input", calculateSums);
        });
    </script>
</body>
</html>
