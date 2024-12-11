<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Просмотр задач</title>
    <style>
        /* Добавляем стиль для увеличения изображения */
        .modal {
            display: none; /* Скрываем по умолчанию */
            position: fixed; /* Открепляем от нормального потока */
            z-index: 1000; /* На переднем плане */
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto; /* Добавляем прокрутку при необходимости */
            background-color: rgba(0, 0, 0, 0.8); /* Полупрозрачный фон */
        }
        .modal-content {
            margin: auto;
            display: block;
            width: 80%;
            max-width: 700px;
        }
        .close {
            position: absolute;
            top: 15px;
            right: 35px;
            color: white;
            font-size: 40px;
            font-weight: bold;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="index.html">Главная</a></li>
                <li><a href="cybernetics.html">Кибернетика</a></li>
                <li><a href="mathematics.html">Математика</a></li>
                <li><a href="physics.html">Физика</a></li>
                <li><a href="humanities.html">Гуманитарные предметы</a></li>
                <li><a href="view_tasks.html">Просмотр задач</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <h1>Просмотр задач</h1>

        <div class="form-group">
            <label for="subject">Выберите предмет:</label>
            <select id="subject">
                <option value="humanities">Гуманитарные</option>
                <option value="cybernetics">Кибернетика</option>
                <option value="mathematics">Математика</option>
                <option value="physics">Физика</option>
            </select>
        </div>

        <div class="form-group">
            <h2>Поиск задач:</h2>
            <input type="text" id="search" placeholder="Поиск задачи...">
        </div>

        <div class="form-group">
            <h2>Список задач:</h2>
            <ul id="taskList"></ul>
        </div>
    </main>
    <footer>
        <p>Сайт помощник. 2024</p>
    </footer>

    <!-- Модальное окно для увеличенного изображения -->
    <div id="myModal" class="modal">
        <span class="close" onclick="closeModal()">&times;</span>
        <img class="modal-content" id="img01">
    </div>

    <script>
        const taskList = document.getElementById('taskList');
        const subjectSelect = document.getElementById('subject');
        const searchInput = document.getElementById('search');
        const modal = document.getElementById("myModal");
        const modalImg = document.getElementById("img01");

        // Load tasks from Local Storage for selected subject
        function loadTasks() {
            const subject = subjectSelect.value;
            const tasks = JSON.parse(localStorage.getItem(subject)) || [];
            displayTasks(tasks);
        }

        // Display tasks
        function displayTasks(tasks) {
            taskList.innerHTML = '';
            const searchTerm = searchInput.value.toLowerCase();
            tasks.forEach((task) => {
                if (task.text.toLowerCase().includes(searchTerm)) {
                    const li = document.createElement('li');
                    li.textContent = task.text; // Отображаем текст задачи

                    // Отображаем изображение, если оно есть
                    if (task.image) {
                        const img = document.createElement('img');
                        img.src = task.image;
                        img.alt = 'Изображение задачи';
                        img.style.maxWidth = '100px'; // Устанавливаем максимальную ширину
                        img.style.maxHeight = '100px'; // Устанавливаем максимальную высоту
                        img.style.cursor = 'pointer'; // Указываем, что изображение кликабельное
                        img.onclick = function() {
                            openModal(this.src); // Открываем модальное окно с изображением
                        };
                        li.appendChild(img);
                        li.appendChild(document.createElement('br')); // Добавляем перенос строки
                    }

                    // Создаем кнопку "Как решается"
                    const button = document.createElement('button');
                    button.textContent = 'Как решается';
                    button.onclick = () => {
                        window.location.href = 'telega.html'; // Переход на страницу telega.html
                    };

                    li.appendChild(button); // Добавляем кнопку в элемент списка
                    taskList.appendChild(li); // Добавляем элемент списка в список задач
                }
            });
        }

        // Open modal with image
        function openModal(src) {
            modal.style.display = "block";
            modalImg.src = src;
        }

        // Close modal
        function closeModal() {
            modal.style.display = "none";
        }

        // Load tasks on page load
        window.onload = loadTasks;

        // Update tasks when subject changes
        subjectSelect.addEventListener('change', loadTasks);

        // Search tasks
        searchInput.addEventListener('input', () => {
            const subject = subjectSelect.value;
            const tasks = JSON.parse(localStorage.getItem(subject)) || [];
            displayTasks(tasks);
        });
    </script>
</body>
</html>
