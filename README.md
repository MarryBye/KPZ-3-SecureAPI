# Лабораторно-практична робота №3

## Розробка та тестування захищеного REST API на Node.js та Express.

Мета: Закріплення теоретичних знань та набуття практичних навичок з розробки, захисту та тестування серверних додатків на базі REST архітектури.

Короткий опис
Проєкт демонструє простий захищений REST API на Node.js + Express з in-memory даними (користувачі, документи, співробітники). Навчальна мета — реалізація аутентифікації, авторизації за ролями та логування запитів.

Файли проєкту
- [data.js](data.js) — in-memory дані (масиви `users`, `documents`, `employees`).
- [server.js](server.js) — основний сервер, маршрути та middleware (див. також [`authMiddleware`](server.js), [`adminOnlyMiddleware`](server.js), [`loggingMiddleware`](server.js)).
- [test-client.js](test-client.js) — Node.js скрипт для автоматичного тестування API.
- [package.json](package.json) — маніфест проєкту.
- [.gitignore](.gitignore) — ігнорування node_modules.
- [README.md](README.md) — ця документація.
- Скриншоти для демонстрації роботи знаходяться в папці images/.

Швидкий старт
1. Встановити залежності:
   npm install

2. Запустити сервер:
   node server.js
   або (якщо додано скрипт start у package.json):
   npm start

3. Запустити тестовий клієнт у окремому терміналі (сервер має бути запущений):
   node test-client.js
   або (якщо додано скрипт test у package.json):
   npm test

Аутентифікація та авторизація
- Аутентифікація виконується простими заголовками HTTP: X-Login і X-Password.
- Авторизація за ролями: лише користувач з role === 'admin' має доступ до конфіденційних ресурсів.
- Реалізація — в [server.js](server.js) як [`authMiddleware`](server.js) і [`adminOnlyMiddleware`](server.js).
- Логування всіх запитів реалізовано в [`loggingMiddleware`](server.js).

API ендпоінти

| Метод | URL | Опис | Заголовки | Тіло запиту (JSON) | Очікувані коди |
|-------|-----|------|-----------|---------------------|----------------|
| GET | /documents | Отримати список документів | X-Login, X-Password | — | 200 OK (успіх), 401 Unauthorized (без заголовків/неправильні) |
| POST | /documents | Створити новий документ | X-Login, X-Password | { "title": "...", "content": "..." } | 201 Created (успіх), 400 Bad Request (відсутні поля), 401 Unauthorized |
| DELETE | /documents/:id | Видалити документ за id | X-Login, X-Password | — | 204 No Content (успіх), 404 Not Found, 401 Unauthorized |
| GET | /employees | Отримати список співробітників (конфіденційно) | X-Login, X-Password (admin) | — | 200 OK (admin), 403 Forbidden (user), 401 Unauthorized |

Приклади заголовків для тестування
- Для звичайного користувача:
  - X-Login: user1
  - X-Password: password123
- Для адміністратора:
  - X-Login: admin1
  - X-Password: password123

Очікувані відповіді / статуси
- 200 OK — успішне отримання ресурсу.
- 201 Created — успішне створення ресурсу.
- 204 No Content — успішне видалення без тіла відповіді.
- 400 Bad Request — валідація (наприклад, відсутнє поле title).
- 401 Unauthorized — невірні або відсутні облікові дані.
- 403 Forbidden — доступ заборонено (не admin).
- 404 Not Found — маршрут або ресурс не знайдено.

Тестування
- Використовуйте Postman або подібний інструмент для ручного тестування за таблицею вище.
- Запуск автоматичного сценарію: [test-client.js](test-client.js).
- Приклад локальної адреси: http://localhost:3000

Посилання
- Репозиторій (як вказано в package.json): https://github.com/MarryBye/secure-api-lab.git
- Основні файли:
  - [server.js](server.js)
  - [data.js](data.js)
  - [test-client.js](test-client.js)
  - [package.json](package.json)
  - [.gitignore](.gitignore)

Примітки для викладача / перевірки
- У репозиторії мають бути мінімум 5 змістовних комітів (за етапами лабораторної).
- Переконайтесь, що node_modules не комітиться (див. [.gitignore](.gitignore)).
- У README має бути скріншоти роботи через Postman (папка images/).

Контакти та додаткові матеріали
- Документація Express: https://expressjs.com/
- Код middleware і маршрути дивіться в [server.js](server.js): [`authMiddleware`](server.js), [`adminOnlyMiddleware`](server.js),