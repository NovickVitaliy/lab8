# UI Documentation — Storybook

> **Інструмент:** Storybook  
> **Версія:** v1.0 | **Дата:** 2025-04-01  
> **Призначення:** Візуальна документація UI-компонентів клієнтського застосунку

---

## 1. Що таке Storybook і навіщо він потрібен

Storybook — це інструмент для ізольованої розробки та документування UI-компонентів.
Кожен компонент демонструється у всіх своїх станах (stories) незалежно від бізнес-логіки застосунку.

**Переваги:**
- розробники бачать усі стани компонента без запуску всього застосунку
- дизайнери можуть перевіряти відповідність макетам
- QA-інженери тестують компоненти ізольовано

---

## 2. Запуск Storybook

```bash
# Встановлення
npx storybook@latest init

# Запуск локально
npm run storybook        # http://localhost:6006

# Збірка статичного сайту
npm run build-storybook  # папка storybook-static/
```

---

## 3. Компонент: Button

### Призначення
Основна кнопка дії в застосунку. Використовується для підтвердження бронювання, оплати, входу.

### Props

| Prop | Тип | За замовчуванням | Опис |
|------|-----|-----------------|------|
| `label` | `string` | — | Текст кнопки |
| `variant` | `primary \| secondary \| danger` | `primary` | Візуальний варіант |
| `disabled` | `boolean` | `false` | Вимкнений стан |
| `loading` | `boolean` | `false` | Стан завантаження |
| `onClick` | `() => void` | — | Обробник кліку |

### Stories

#### Default (Primary)
```js
// Button.stories.js
export const Default = {
  args: {
    label: 'Забронювати',
    variant: 'primary',
    disabled: false,
    loading: false,
  },
};
```
**Вигляд:** синя кнопка, активна, клікабельна.

---

#### Disabled
```js
export const Disabled = {
  args: {
    label: 'Забронювати',
    variant: 'primary',
    disabled: true,
  },
};
```
**Вигляд:** сіра кнопка, курсор `not-allowed`, не реагує на клік.  
**Використання:** квиток недоступний або форма не заповнена.

---

#### Loading
```js
export const Loading = {
  args: {
    label: 'Обробка...',
    variant: 'primary',
    loading: true,
  },
};
```
**Вигляд:** спіннер замість тексту, кнопка не клікабельна.  
**Використання:** під час відправлення запиту на бронювання/оплату.

---

#### Danger
```js
export const Danger = {
  args: {
    label: 'Скасувати бронювання',
    variant: 'danger',
  },
};
```
**Вигляд:** червона кнопка.  
**Використання:** деструктивні дії (скасування замовлення).

---

## 4. Компонент: BookingStatusBadge

### Призначення
Бейдж статусу бронювання. Відображається у списку замовлень та на сторінці деталей.

### Props

| Prop | Тип | Допустимі значення | Опис |
|------|-----|--------------------|------|
| `status` | `string` | `pending \| confirmed \| cancelled` | Статус бронювання |

### Stories

#### Pending
```js
export const Pending = {
  args: { status: 'pending' },
};
```
**Вигляд:** жовтий бейдж з текстом "Очікує оплати".

---

#### Confirmed
```js
export const Confirmed = {
  args: { status: 'confirmed' },
};
```
**Вигляд:** зелений бейдж з текстом "Підтверджено".

---

#### Cancelled
```js
export const Cancelled = {
  args: { status: 'cancelled' },
};
```
**Вигляд:** сірий бейдж з текстом "Скасовано".

---

## 5. Структура файлів Storybook

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.css
│   │   └── Button.stories.js     ← Story-файл
│   └── BookingStatusBadge/
│       ├── BookingStatusBadge.jsx
│       └── BookingStatusBadge.stories.js
.storybook/
├── main.js                        ← конфігурація
└── preview.js                     ← глобальні декоратори
```

---

## 6. Публікація

Статичний сайт Storybook публікується автоматично через CI/CD на GitHub Pages:

```
https://<org>.github.io/<repo>/storybook/
```

Детально — у [ci-cd-docs.md](ci-cd-docs.md).
