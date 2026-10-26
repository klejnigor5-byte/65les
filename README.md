# Express + Passport + MongoDB Atlas

Навчальний проєкт на Node.js та Express.js: авторизація Passport Local, збереження користувачів у MongoDB Atlas і відображення записів через EJS.

## Функціональність

- Реєстрація користувача; пароль зберігається лише у вигляді bcrypt-хешу.
- Вхід і вихід через Passport Local Strategy.
- Зберігання сесій у MongoDB за допомогою `connect-mongo`.
- `GET /users` — отримує користувачів із MongoDB і показує таблицю.
- `GET /users/:userId` — показує одного користувача за MongoDB ObjectId.
- Сторінки користувачів доступні лише після входу.
- Обробка помилок і некоректного ID.

## Вимоги

- Node.js LTS і npm.
- Обліковий запис MongoDB Atlas.

## Встановлення

1. Розпакуйте проєкт і відкрийте його папку в терміналі.
2. Встановіть залежності:

   ```bash
   npm install
   ```

3. Створіть файл `.env` на основі `.env.example`:

   **Windows PowerShell**
   ```powershell
   Copy-Item .env.example .env
   ```

4. Вкажіть у `.env` власні `MONGODB_URI` і `SESSION_SECRET`.

## Налаштування MongoDB Atlas

1. Увійдіть у MongoDB Atlas і створіть кластер (або виберіть наявний).
2. У розділі **Database Access** створіть database user із паролем.
3. У розділі **Network Access** додайте IP-адресу свого комп’ютера. Для навчального проєкту краще не відкривати доступ для всіх IP-адрес.
4. Відкрийте **Connect → Drivers → Node.js** і скопіюйте connection string.
5. Вставте URI у `MONGODB_URI` у `.env`. Замініть заповнювачі на свої значення; якщо пароль містить спеціальні символи, URL-кодуйте їх.
6. Назва бази в прикладі — `passport_mongodb_homework`. MongoDB створить базу та колекцію після першого запису. Колекція `users` наповнюється під час реєстрації.

**Не додавайте `.env` до Git і не публікуйте пароль від Atlas.** Файл `.env` уже внесений до `.gitignore`.

## Запуск

Режим розробки з автоматичним перезапуском:

```bash
npm run dev
```

Звичайний запуск:

```bash
npm start
```

Відкрийте `http://localhost:3000`, зареєструйте користувача, а потім перейдіть до списку користувачів.

## Маршрути

| Метод | Шлях | Опис |
|---|---|---|
| GET | `/` | Головна сторінка |
| GET | `/register` | Форма реєстрації |
| POST | `/register` | Створення користувача в MongoDB |
| GET | `/login` | Форма входу |
| POST | `/login` | Авторизація Passport Local |
| POST | `/logout` | Завершення сесії |
| GET | `/users` | Список користувачів із MongoDB; потрібен вхід |
| GET | `/users/:userId` | Деталі користувача; потрібен вхід |

## Структура

```text
express-mongodb-passport/
├── config/
│   ├── db.js
│   └── passport.js
├── controllers/
│   ├── authController.js
│   └── userController.js
├── middleware/
│   ├── authMiddleware.js
│   └── errorMiddleware.js
├── models/
│   └── User.js
├── public/
│   └── styles.css
├── routes/
│   ├── authRoutes.js
│   └── userRoutes.js
├── views/
│   ├── partials/
│   ├── users/
│   ├── error.ejs
│   ├── index.ejs
│   ├── login.ejs
│   └── register.ejs
├── .env.example
├── .gitignore
├── app.js
├── package.json
├── README.md
└── server.js
```

## Публікація на GitHub

1. Створіть новий порожній репозиторій на GitHub.
2. Переконайтеся, що `.env` не потрапив у репозиторій.
3. У терміналі з кореня проєкту виконайте:

   ```bash
   git init
   git add .
   git commit -m "Add Passport auth and MongoDB Atlas integration"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
   git push -u origin main
   ```

4. Замініть адресу `origin` на URL власного GitHub-репозиторію.
5. Надішліть викладачу посилання на репозиторій.

## Важливо для production

Перед розгортанням задайте на платформі хостингу `MONGODB_URI`, `SESSION_SECRET` і `NODE_ENV=production`. Для production потрібні HTTPS та коректна конфігурація довіреного проксі й cookie. Додайте додатковий захист від CSRF і обмеження частоти запитів перед використанням у реальному сервісі.
