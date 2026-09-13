# Metaluck

Криптоказино с мини-играми внутри Telegram Mini App.

Проект развиваю своими силами. Если интересно поучаствовать — в коде, дизайне, идеях или просто посмотреть, как это устроено — пишите.

**Telegram:** [t.me/TheOnyxis](https://t.me/TheOnyxis)

Клиент: **React + Vite** (Vercel)  
API: **Fastify + TypeScript** (Railway)  
Данные: **Supabase (Postgres)**  
Крипто: **TON / USDT** + внутренние звёзды (★)

---

## Что внутри

- **Мини-игры** — Coinflip, Blackjack, Mine Rush, Arena, Aviator
- **Кейсы** — бесплатный кейс раз в 7 дней и платные кейсы
- **Награды** — ежедневный календарь, колесо фортуны, премиум-колесо
- **Кошелёк** — баланс, депозит и вывод, обмен валют, история
- **Кабинет** — рефералка, XP/уровень, лидерборд
- **Мультиязычность** — RU / UK / EN / ES / DE + светлая/тёмная тема
- **Демо-режим** — можно крутить игры без списания баланса

---

## Структура

```
minigames/
├── client/          # фронтенд (Vite React)
│   ├── src/
│   └── public/gifts # картинки подарков (WebP)
├── server/          # бэкенд (Fastify)
│   ├── src/
│   │   ├── index.ts       # bootstrap + getUserId
│   │   ├── routes/        # кейсы, daily, wheel, payments…
│   │   └── supabaseStore.ts
│   └── scripts/     # webhook, broadcast, импорт данных
├── scripts/         # локальные helper-скрипты
├── .env.example     # пример переменных окружения
└── package.json     # корневые npm-скрипты
```

### Экономика

- Баланс ★ и купоны меняются атомарными SQL-функциями (`add_balance`, `try_deduct_balance`, `add_coupons`, `try_deduct_coupons`).
- Мультивалютный ledger для STARS / TON / USDT.
- Rate-limit по Telegram user id; крипто-RNG для призов; кэш лидерборда ~45 с.

---

## Быстрый старт

### 1. Требования

- Node.js **22+**
- Аккаунт Telegram Bot + Mini App
- Проект Supabase

### 2. Установка

```bash
npm install
npm install --prefix client
npm install --prefix server
```

### 3. Окружение

Скопируйте `.env.example` → `.env` в корне (сервер подхватывает его) и заполните:

| Переменная | Назначение |
|---|---|
| `SUPABASE_URL` | URL проекта Supabase |
| `SUPABASE_SERVICE_ROLE_KEY` | service_role ключ (только сервер) |
| `TELEGRAM_BOT_TOKEN` | токен бота |
| `TELEGRAM_BOT_USERNAME` | username бота без `@` |
| `TELEGRAM_WEBHOOK_SECRET_TOKEN` | секрет webhook |
| `ADMIN_API_SECRET` | секрет для admin broadcast |
| `VITE_API_BASE_URL` | URL API для клиента (на Vercel) |

### 4. Локальный запуск

```bash
# клиент + сервер вместе
npm run dev

# по отдельности
npm run dev:client
npm run dev:server
```

По умолчанию:

- клиент: `http://localhost:5173`
- API: `http://127.0.0.1:3001`

---

## Сборка и деплой

```bash
npm run build
```

Типичная схема:

- **client** → Vercel (`client/`)
- **server** → Railway (`server/`)
- после деплоя API задайте `VITE_API_BASE_URL` на Vercel и пересоберите клиент
- webhook Telegram: `POST https://<api-host>/api/telegram/webhook`

Пример nginx для одного домена (SPA + API): `nginx.api-spa.example.conf`

---

## Полезные скрипты (server)

```bash
cd server
npm run webhook:set      # установить Telegram webhook
npm run webhook:info     # статус webhook
npm run broadcast -- --text "Текст"   # рассылка по /start
```

---

## Связь

Проект открыт к людям, которым интересно казино, мини-игры и крипта в Telegram.

Пишите в Telegram: [t.me/TheOnyxis](https://t.me/TheOnyxis)

---

## Лицензия

Приватный проект. Все права сохранены.
