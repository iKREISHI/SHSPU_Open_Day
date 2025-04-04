# Синтаксис JavaScript: Основы для начинающих

Привет, ребята! Сегодня мы познакомимся с синтаксисом JavaScript — набором правил, по которым пишется код, чтобы компьютер понимал ваши инструкции. Разберём основные элементы языка с примерами и интерактивными заданиями.

---

## 1. Переменные

Переменные используются для хранения данных. В JavaScript их можно объявлять с помощью трёх ключевых слов:

- **`let`** — создаёт переменную, значение которой можно изменять.
- **`const`** — создаёт константу, значение которой фиксировано.
- **`var`** — старый способ объявления переменных (реже используется сегодня).

**Пример:**

```javascript
let имя = "Оля";
const возраст = 16;
var город = "Санкт-Петербург";
```

## 2. Функции

Функции позволяют группировать набор команд и выполнять их по вызову. Это очень удобно для повторного использования кода.

Пример функции:

```javascript
function sayHello(имя) {
  console.log("Привет, " + имя + "!");
}

sayHello("Оля");  // Выведет: Привет, Оля!

```

## 3. Условия
Условия позволяют выполнять разные действия в зависимости от истинности выражений. Основные конструкции — `if`, `else if `и `else`.
```javascript
let оценка = 8;

if (оценка >= 9) {
  console.log("Отлично!");
} else if (оценка >= 7) {
  console.log("Хорошо!");
} else {
  console.log("Можно лучше!");
}

```

## 4. Циклы
Циклы используются для повторения одних и тех же действий несколько раз. Наиболее часто используется цикл `for`.

Пример цикла `for`:
```javascript
for (let i = 1; i <= 5; i++) {
  console.log("Номер итерации: " + i);
}

```

## 5. Объекты
Объекты — это структуры, которые позволяют хранить данные в виде пар «ключ: значение». Они очень полезны для организации информации.
```javascript
let студент = {
  имя: "Оля",
  возраст: 16,
  предметы: ["математика", "физика", "информатика"]
};

console.log(студент.имя); // Выведет "Оля"

```

## Заключение
Синтаксис JavaScript — это основа для создания программ и веб-приложений. Сегодня мы изучили:
Переменные для хранения данных.

 - Функции для группировки и повторного использования кода.

 - Условия для принятия решений.

 - Циклы для повторения действий.

 - Объекты для структурирования информации.

Эти базовые знания помогут вам начать писать свои собственные программы и создавать интерактивные веб-страницы.