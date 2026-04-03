# Test Strategy

> **Версія:** v1.0 | **Дата:** 2025-04-01  
> **Вимоги:** [SSD — FR-01..FR-07](../architecture/SSD.md)

---

## 1. Мета тестування

Забезпечити відповідність системи функціональним і нефункціональним вимогам, визначеним у [SSD](../architecture/SSD.md).

---

## 2. Рівні тестування

| Рівень | Інструменти | Відповідальний |
|--------|-------------|----------------|
| **Unit тести** | Jest / JUnit | Розробник |
| **Integration тести** | Supertest / TestContainers | QA / Розробник |
| **API тести** | Postman / Newman | QA |
| **Performance тести** | k6 / Locust | QA / DevOps |
| **Security тести** | OWASP ZAP | DevOps / QA |

---

## 3. Стратегія автоматизації

- **Unit та Integration тести** — запускаються при кожному PR у CI/CD (GitHub Actions)
- **API тести** — запускаються після деплою на staging-середовище
- **Performance тести** — запускаються перед кожним production-релізом
- **Security тести** — запускаються щотижня автоматично

---

## 4. Критерії проходження

- Code coverage: ≥ 80% для unit тестів
- Всі API тести успішні перед деплоєм у production
- Час відповіді API: ≤ 300 мс при 500 req/min (згідно з SSD)
- Відсутність критичних вразливостей OWASP Top 10

---

## 5. Середовища тестування

| Середовище | Призначення |
|------------|-------------|
| Local | Розробка, unit тести |
| Staging | Integration, API, Performance тести |
| Production | Smoke тести після деплою |

---

## 6. Пов'язані документи

- [SSD — Функціональні вимоги](../architecture/SSD.md)
- [Traceability Matrix](traceability-matrix.md)
