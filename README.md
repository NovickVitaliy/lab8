# Ticket Booking API

REST API сервіс для бронювання квитків.

## 📚 Документація

Вся документація знаходиться в директорії [`/docs`](docs/README.md).

### Швидкий доступ
- [Огляд документації](docs/README.md)
- [System Specification](docs/architecture/SSD.md)
- [System Design](docs/architecture/SDD.md)
- [Infrastructure](docs/architecture/ISD.md)
- [Test Strategy](docs/quality/test-strategy.md)

## 🚀 Швидкий старт

```bash
docker compose up -d
```

## 📖 Генерація сайту документації

```bash
pip install mkdocs mkdocs-material
cd docs
mkdocs serve       # локальний перегляд на http://127.0.0.1:8000
mkdocs build       # генерація статичного сайту в папку site/
```

## 🔄 Версійність документації

| Версія | Реліз | Опис |
|--------|-------|------|
| v1.0 | 2025-04-01 | Початкова документація |

> При зміні API або вимог — створюється нова версія документації через Git tag (`git tag v1.1`).
