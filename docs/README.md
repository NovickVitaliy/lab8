# 📚 Documentation Overview

Документація проекту **REST API для бронювання квитків**.

> **Single Source of Truth:** Усі функціональні вимоги описуються виключно в [SSD](architecture/SSD.md).
> Test Strategy та Test Plan посилаються на ідентифікатори вимог з SSD.

---

## 🗂 Навігація

### 🏗 Архітектура
| Документ | Опис |
|----------|------|
| [System Specification (SSD)](architecture/SSD.md) | Що система повинна робити (вимоги) |
| [System Design (SDD)](architecture/SDD.md) | Як система реалізована (архітектура) |
| [Infrastructure Specification (ISD)](architecture/ISD.md) | Розгортання та інфраструктура |

### ✅ Якість
| Документ | Опис |
|----------|------|
| [Test Strategy](quality/test-strategy.md) | Загальна стратегія тестування |
| [Traceability Matrix](quality/traceability-matrix.md) | Матриця простежуваності вимог |

### 👨‍💻 Розробка
| Документ | Опис |
|----------|------|
| [Developer Onboarding](developer/onboarding.md) | Інструкція для нових розробників |

### 🔌 API
| Документ | Опис |
|----------|------|
| [OpenAPI специфікація](api/openapi.yaml) | Повна специфікація у форматі OpenAPI 3.0 |
| [Swagger UI](api/swagger-ui.html) | Інтерактивна документація та тестування API |

### ⚙️ Процеси
| Документ | Опис |
|----------|------|
| [CI/CD Pipeline](ci-cd-docs.md) | Автоматизація збірки та публікації документації |
| [Definition of Done](definition-of-done.md) | Критерії готовності змін з точки зору документації |
| [UI Components (Storybook)](storybook.md) | Візуальна документація UI-компонентів |

---

## 🔄 Версійність

| Версія | Дата | Опис змін |
|--------|------|-----------|
| v1.0 | 2025-04-01 | Початковий реліз документації (ЛР-7, ЛР-8) |
| v1.1 | 2025-04-03 | Додано API документацію, CI/CD, DoD, Storybook (ЛР-9) |

---

## 📌 Правила оновлення документації

1. Будь-яка зміна вимог → оновлення `SSD.md` першочергово
2. Зміна архітектури → оновлення `SDD.md`
3. Зміна інфраструктури → оновлення `ISD.md`
4. Нові вимоги → додати рядок до `traceability-matrix.md`
5. Кожна зміна фіксується через Git commit із описом
