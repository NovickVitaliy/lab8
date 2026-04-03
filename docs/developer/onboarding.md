# Developer Onboarding

> Інструкція для нових розробників проекту REST API для бронювання квитків.

---

## 1. Огляд системи

Перед початком роботи ознайомся з документацією:

- [SSD — Що робить система](../architecture/SSD.md)
- [SDD — Як вона влаштована](../architecture/SDD.md)
- [ISD — Де вона розгорнута](../architecture/ISD.md)

---

## 2. Необхідні інструменти

| Інструмент | Версія | Призначення |
|------------|--------|-------------|
| Docker | ≥ 24.x | Запуск контейнерів |
| Node.js | ≥ 20.x | Середовище виконання |
| Git | ≥ 2.40 | Контроль версій |
| Postman | остання | Тестування API |

---

## 3. Локальний запуск

```bash
# 1. Клонувати репозиторій
git clone https://github.com/your-org/ticket-booking-api.git
cd ticket-booking-api

# 2. Скопіювати змінні середовища
cp .env.example .env

# 3. Запустити через Docker Compose
docker compose up -d

# 4. Перевірити статус
curl http://localhost:3000/health
```

---

## 4. Структура проекту

```
src/
├── services/
│   ├── user/
│   ├── booking/
│   ├── payment/
│   └── search/
├── gateway/
└── shared/
docs/
├── architecture/
├── quality/
└── developer/
```

---

## 5. Стандарти коду

- Мова: TypeScript (strict mode)
- Форматування: Prettier (автоматично через pre-commit hook)
- Лінтер: ESLint
- Гілки: `feature/`, `fix/`, `chore/`
- Commit-повідомлення: Conventional Commits (`feat:`, `fix:`, `docs:`)

---

## 6. Процес розробки

1. Створи гілку від `main`: `git checkout -b feature/my-feature`
2. Розроби функціонал, покрий unit-тестами
3. Відкрий Pull Request → пройде CI (lint, tests)
4. Після code review — merge у `main`
5. GitHub Actions автоматично задеплоїть на staging

---

## 7. Корисні посилання

- [Test Strategy](../quality/test-strategy.md)
- [Traceability Matrix](../quality/traceability-matrix.md)
- [API документація](#) *(Swagger UI доступний на `/api/docs`)*
