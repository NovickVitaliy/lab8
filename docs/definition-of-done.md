# Definition of Done — Documentation

> **Версія:** v1.0 | **Дата:** 2025-04-01  
> DoD застосовується до кожного Pull Request, який вносить зміни в код або вимоги.

---

## 1. Загальні правила

- Жоден PR не може бути змерджений у `main`, якщо не виконані всі пункти DoD для відповідного типу змін.
- Перевірка DoD відбувається на етапі code review та автоматично через CI/CD pipeline.
- Документація оновлюється **у тому ж PR**, що й код — не в окремому.

---

## 2. Матриця DoD за типом змін

| Тип зміни | Обов'язкові дії | Автоматична перевірка |
|-----------|-----------------|----------------------|
| **Додано новий API-ендпоінт** | Додати path у `openapi.yaml` з усіма методами, параметрами, responses та схемами | `redocly lint` у CI |
| **Змінено request / response** | Оновити відповідну схему в `components/schemas` | `redocly lint` у CI |
| **Видалено або перейменовано ендпоінт** | Оновити `openapi.yaml`, підвищити major-версію API, оновити `SDD.md` | `redocly lint` + code review |
| **Додано нову функціональну вимогу** | Додати запис у `SSD.md` (FR-xx), рядок у `traceability-matrix.md` | Code review |
| **Змінено нефункціональну вимогу** | Оновити розділ у `SSD.md` | Code review |
| **Додано UI-компонент** | Додати Story-файл з мінімум 2 станами (default + альтернативний) | `build-storybook` у CI |
| **Змінено UI-компонент** | Оновити існуючі Stories, додати новий стан за потреби | `build-storybook` у CI |
| **Зміна інфраструктури** | Оновити `ISD.md` | Code review |
| **Breaking change (будь-який)** | Підвищити версію документації, додати запис у CHANGELOG | Version tag у CI |

---

## 3. Чек-лист для розробника (перед відкриттям PR)

### Будь-яка зміна API
- [ ] `api/openapi.yaml` оновлено
- [ ] `npx @redocly/cli lint api/openapi.yaml` — без помилок локально
- [ ] Якщо breaking change — версія API підвищена в `openapi.yaml` (поле `info.version`)

### Нова функціональна вимога
- [ ] Додано `FR-xx` у `docs/architecture/SSD.md`
- [ ] Додано рядок у `docs/quality/traceability-matrix.md` зі статусом

### Зміна або додавання UI-компонента
- [ ] Є файл `ComponentName.stories.js`
- [ ] Мінімум 2 stories: default-стан та один альтернативний (disabled/loading/error)
- [ ] `npm run build-storybook` — успішно локально

### Будь-який PR
- [ ] CI pipeline проходить (validate → build → no errors)
- [ ] Документація оновлена **у тому ж PR**, що й код

---

## 4. Критерії готовності (перевіряється на code review)

| Артефакт | Що перевіряється | Де перевіряється |
|----------|-----------------|-----------------|
| `openapi.yaml` | Синтаксична коректність, всі нові ендпоінти описані | CI: `redocly lint` |
| `SSD.md` | Нові вимоги додані з унікальним ID | Code review |
| `traceability-matrix.md` | Кожна вимога має відповідний тест-кейс | Code review |
| `*.stories.js` | Існує для кожного нового/зміненого компонента | CI: `build-storybook` |
| `ISD.md` | Відповідає реальній інфраструктурі | Code review |
| CI pipeline | Всі кроки зелені | GitHub Actions |

---

## 5. Що відбувається при порушенні DoD

1. CI pipeline падає → PR не може бути змерджений (захист гілки `main`)
2. Code reviewer залишає коментар із посиланням на цей документ
3. Розробник виправляє у тому ж PR — не створює новий

---

## 6. Пов'язані документи

- [OpenAPI специфікація](api/openapi.yaml)
- [CI/CD Pipeline](ci-cd-docs.md)
- [Test Strategy](quality/test-strategy.md) *(з ЛР-8)*
- [Traceability Matrix](quality/traceability-matrix.md) *(з ЛР-8)*
