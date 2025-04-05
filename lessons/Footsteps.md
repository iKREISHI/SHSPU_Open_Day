## Шаг 1. Создание HTML-страницы
На этом этапе создаём простую HTML-страницу с базовой структурой, 
отображающую заголовок, два поля для отображения 
статистики и изображение, по которому будут совершаться клики.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кликер</title>
</head>
<body>
    <h1>Кликер</h1>
    <p>Деньги: <span id="money">0</span></p>
    <p>Всего кликов: <span id="total_clicks">0</span></p>
    <img id="clickerImage" src="media/game/almaz.png" alt="Кликер" style="cursor: pointer;">
</body>
</html>
```

<hr>

## Шаг 2. Обработка клика (изменение чисел на странице)
Добавляем обработчик клика по картинке, который будет 
увеличивать значение кликов и денег. Пока что 
изменения происходят только на странице.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кликер</title>
    <script>
        // Обновление данных на странице
        function updateUI(stats) {
            document.getElementById('money').innerText = stats.money;
            document.getElementById('total_clicks').innerText = stats.total_clicks;
        }

        // Обработка клика по картинке
        function handleClick() {
            // Берём текущие значения, создаём объект статистики
            let stats = {
                money: parseInt(document.getElementById('money').innerText),
                total_clicks: parseInt(document.getElementById('total_clicks').innerText),
                click_power: 1 // задаём силу клика
            };

            // Обновляем статистику
            stats.total_clicks += 1;
            stats.money += stats.click_power;

            // Обновляем UI
            updateUI(stats);
        }

        document.addEventListener('DOMContentLoaded', function() {
            document.getElementById('clickerImage').addEventListener('click', handleClick);
        });
    </script>
</head>
<body>
    <h1>Кликер</h1>
    <p>Деньги: <span id="money">0</span></p>
    <p>Всего кликов: <span id="total_clicks">0</span></p>
    <img id="clickerImage" src="media/game/almaz.png" alt="Кликер" style="cursor: pointer;">
</body>
</html>
```
<hr>

## Шаг 3. Сохранение кликов в localStorage
Теперь расширим функциональность: после клика обновлённые 
данные будут сохраняться в localStorage, чтобы при 
перезагрузке страницы данные не сбрасывались.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кликер</title>
    <script>
        // Функция для обновления UI
        function updateUI(stats) {
            document.getElementById('money').innerText = stats.money;
            document.getElementById('total_clicks').innerText = stats.total_clicks;
        }

        // Обработка клика по картинке с сохранением в localStorage
        function handleClick() {
            // Получаем данные из localStorage или создаём базовый объект
            let stats = JSON.parse(localStorage.getItem('clickerStats')) || {
                money: 0,
                total_clicks: 0,
                click_power: 1
            };

            // Обновляем статистику
            stats.total_clicks += 1;
            stats.money += stats.click_power;

            // Сохраняем обновлённые данные в localStorage
            localStorage.setItem('clickerStats', JSON.stringify(stats));
            updateUI(stats);
        }

        // При загрузке страницы, если есть сохранённые данные, их отображаем
        document.addEventListener('DOMContentLoaded', function() {
            let stats = JSON.parse(localStorage.getItem('clickerStats'));
            if (stats) {
                updateUI(stats);
            }
            document.getElementById('clickerImage').addEventListener('click', handleClick);
        });
    </script>
</head>
<body>
    <h1>Кликер</h1>
    <p>Деньги: <span id="money">0</span></p>
    <p>Всего кликов: <span id="total_clicks">0</span></p>
    <img id="clickerImage" src="media/game/almaz.png" alt="Кликер" style="cursor: pointer;">
</body>
</html>
```

<hr>

## Шаг 4. Загрузка данных из API
На последнем шаге мы добавляем функцию, которая загружает 
данные с API. Предполагается, что API возвращает массив, 
поэтому мы берём первый объект. Полученные данные 
сохраняются в localStorage и отображаются на странице.


```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кликер</title>
    <script>
        // Функция для обновления UI
        function updateUI(stats) {
            document.getElementById('money').innerText = stats.money;
            document.getElementById('total_clicks').innerText = stats.total_clicks;
        }

        // Функция загрузки статистики с сервера
        async function loadStats() {
            try {
                const response = await fetch('/api/v0/stats/');
                if (response.ok) {
                    const data = await response.json();
                    // Если API возвращает массив, берём первый объект
                    const stats = Array.isArray(data) ? data[0] : data;
                    // Сохраняем данные в localStorage
                    localStorage.setItem('clickerStats', JSON.stringify(stats));
                    updateUI(stats);
                } else {
                    console.error('Ошибка при загрузке статистики с сервера');
                }
            } catch (error) {
                console.error('Ошибка запроса: ', error);
            }
        }

        // Обработка клика по картинке с сохранением в localStorage
        function handleClick() {
            // Получаем данные из localStorage или создаём базовый объект
            let stats = JSON.parse(localStorage.getItem('clickerStats')) || {
                money: 0,
                total_clicks: 0,
                click_power: 1
            };

            // Обновляем статистику
            stats.total_clicks += 1;
            stats.money += stats.click_power;

            // Сохраняем обновлённые данные в localStorage
            localStorage.setItem('clickerStats', JSON.stringify(stats));
            updateUI(stats);

            // Здесь можно добавить AJAX-запрос для синхронизации данных с сервером
            // Например: fetch('/api/v0/stats/update/', { method: 'POST', body: JSON.stringify(stats), headers: { 'Content-Type': 'application/json' } });
        }

        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            // Загружаем данные из API
            loadStats();
            // Назначаем обработчик клика
            document.getElementById('clickerImage').addEventListener('click', handleClick);
        });
    </script>
</head>
<body>
    <h1>Кликер</h1>
    <p>Деньги: <span id="money">0</span></p>
    <p>Всего кликов: <span id="total_clicks">0</span></p>
    <img id="clickerImage" src="media/game/almaz.png" alt="Кликер" style="cursor: pointer;">
</body>
</html>

```


## Шаг 5. Отправка данных на сервер
Добавим функцию `sendStats`, которая будет считывать 
статистику из localStorage и отправлять её на сервер с 
помощью AJAX-запроса (PATCH). Затем с помощью `setInterval` 
вызываем эту функцию каждые 5 секунд.


```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Кликер</title>
    <script>
        // Функция для обновления UI
        function updateUI(stats) {
            document.getElementById('money').innerText = stats.money;
            document.getElementById('total_clicks').innerText = stats.total_clicks;
        }

        // Функция загрузки статистики с сервера
        async function loadStats() {
            try {
                const response = await fetch('/api/v0/stats/');
                if (response.ok) {
                    const data = await response.json();
                    // Если API возвращает массив, берём первый объект
                    const stats = Array.isArray(data) ? data[0] : data;
                    // Сохраняем данные в localStorage
                    localStorage.setItem('clickerStats', JSON.stringify(stats));
                    updateUI(stats);
                } else {
                    console.error('Ошибка при загрузке статистики с сервера');
                }
            } catch (error) {
                console.error('Ошибка запроса: ', error);
            }
        }

        // Обработка клика по картинке с сохранением в localStorage
        function handleClick() {
            // Получаем данные из localStorage или создаём базовый объект
            let stats = JSON.parse(localStorage.getItem('clickerStats')) || {
                player: null,
                money: 0,
                total_clicks: 0,
                click_power: 1,
                passive_power: 0
            };

            // Обновляем статистику
            stats.total_clicks += 1;
            stats.money += stats.click_power;

            // Сохраняем обновлённые данные в localStorage
            localStorage.setItem('clickerStats', JSON.stringify(stats));
            updateUI(stats);
        }

        // Функция отправки данных на сервер
        async function sendStats() {
            // Берем статистику из localStorage
            let stats = JSON.parse(localStorage.getItem('clickerStats'));
            if (!stats) return;

            const payload = {
                player: stats.player,
                money: stats.money,
                total_clicks: stats.total_clicks,
                click_power: stats.click_power,
                passive_power: stats.passive_power
            };

            try {
                const response = await fetch('/api/v0/stats/', {
                    method: 'PATCH',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-CSRFToken': '{{ csrf_token }}'
                    },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    console.error('Ошибка при отправке данных на сервер');
                } else {
                    console.log('Данные успешно отправлены на сервер');
                }
            } catch (error) {
                console.error('Ошибка запроса при отправке данных: ', error);
            }
        }

        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            // Загружаем данные из API
            loadStats();
            // Назначаем обработчик клика для картинки
            document.getElementById('clickerImage').addEventListener('click', handleClick);
            // Устанавливаем отправку данных на сервер каждые 30 секунд
            setInterval(sendStats, 5000); // 30000 миллисекунд = 30 секунд
        });
    </script>
</head>
<body>
    <h1>Кликер</h1>
    <p>Деньги: <span id="money">0</span></p>
    <p>Всего кликов: <span id="total_clicks">0</span></p>
    <img id="clickerImage" src="media/game/almaz.png" alt="Кликер" style="cursor: pointer;">
</body>
</html>

```

