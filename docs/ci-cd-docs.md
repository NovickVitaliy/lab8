# Documentation CI/CD Pipeline

> **Версія:** v1.0 | **Дата:** 2025-04-01  
> **Пов'язані документи:** [OpenAPI](api/openapi.yaml) · [Storybook](storybook.md) · [Definition of Done](definition-of-done.md)

---

## 1. Артефакти документації

| Артефакт | Інструмент | Результат |
|----------|-----------|-----------|
| API-документація | Swagger UI | Статичний HTML-сайт |
| UI-документація | Storybook | Статичний HTML-сайт |
| Загальний сайт | MkDocs | Статичний HTML-сайт |

---

## 2. Тригери генерації

| Тригер | Дія |
|--------|-----|
| Push у будь-яку `feature/*` гілку | Валідація OpenAPI spec (lint only) |
| Відкриття Pull Request у `main` | Валідація + тестова збірка всіх артефактів |
| Merge у `main` | Повна збірка та публікація на GitHub Pages |
| Git tag `v*.*.*` | Публікація версійованої документації |

---

## 3. Pipeline — покрокова схема

```
──────────────────────────────────────────────────────────────
  TRIGGER: merge into main  /  git tag v*.*.*
──────────────────────────────────────────────────────────────

  Step 1: Validate
  ┌─────────────────────────────────────────────┐
  │  • npx @redocly/cli lint api/openapi.yaml   │
  │  • Перевірка на broken links у Markdown     │
  └─────────────────────────────────────────────┘
            │ fail → pipeline зупиняється
            ▼ pass

  Step 2: Build Swagger UI
  ┌─────────────────────────────────────────────┐
  │  • Копіювати openapi.yaml                   │
  │  • Генерувати swagger-ui.html               │
  │  • Артефакт: dist/api/                      │
  └─────────────────────────────────────────────┘

  Step 3: Build Storybook
  ┌─────────────────────────────────────────────┐
  │  • npm ci                                   │
  │  • npm run build-storybook                  │
  │  • Артефакт: storybook-static/              │
  └─────────────────────────────────────────────┘

  Step 4: Build MkDocs site
  ┌─────────────────────────────────────────────┐
  │  • pip install mkdocs mkdocs-material       │
  │  • mkdocs build                             │
  │  • Артефакт: site/                          │
  └─────────────────────────────────────────────┘

  Step 5: Merge artifacts
  ┌─────────────────────────────────────────────┐
  │  site/                                      │
  │  ├── (mkdocs pages)                         │
  │  ├── api/        ← Swagger UI               │
  │  └── storybook/  ← Storybook                │
  └─────────────────────────────────────────────┘

  Step 6: Publish to GitHub Pages
  ┌─────────────────────────────────────────────┐
  │  • actions/deploy-pages@v4                  │
  │  • Гілка: gh-pages                          │
  └─────────────────────────────────────────────┘
```

---

## 4. GitHub Actions — workflow файл

```yaml
# .github/workflows/docs.yml
name: Documentation CI/CD

on:
  push:
    branches: [main]
  push:
    tags: ['v*.*.*']
  pull_request:
    branches: [main]

jobs:
  validate:
    name: Validate Documentation
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Validate OpenAPI spec
        run: npx @redocly/cli lint api/openapi.yaml

      - name: Check Markdown links
        run: npx markdown-link-check docs/**/*.md

  build-and-deploy:
    name: Build & Deploy Docs
    needs: validate
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || startsWith(github.ref, 'refs/tags/v')
    permissions:
      pages: write
      id-token: write

    steps:
      - uses: actions/checkout@v4

      - name: Build Storybook
        run: |
          npm ci
          npm run build-storybook -- -o site/storybook

      - name: Build MkDocs
        run: |
          pip install mkdocs mkdocs-material
          mkdocs build

      - name: Copy Swagger UI
        run: |
          mkdir -p site/api
          cp api/openapi.yaml site/api/
          cp api/swagger-ui.html site/api/index.html

      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
        with:
          artifact_name: github-pages
```

---

## 5. URL-структура після публікації

```
https://<org>.github.io/<repo>/
├──                          ← MkDocs (загальна документація)
├── api/                     ← Swagger UI
└── storybook/               ← Storybook UI-документація
```

---

## 6. Версійність документації

| Правило | Опис |
|---------|------|
| Версія документації = версія продукту | `v1.0.0` → документація `v1.0.0` |
| Patch (`v1.0.x`) | Оновлення документації без нової версії сайту |
| Minor (`v1.x.0`) | Нова версія, стара залишається доступною |
| Major (`v2.0.0`) | Breaking change → обов'язково нова версія + changelog |
| Git tag | `git tag v1.1.0` → автоматична публікація версійованої документації |

**Приклад:** версія `v1.0.0` документації відповідає стабільному релізу API `v1.0.0`.  
При зміні сигнатури будь-якого ендпоінта — мінімум minor-версія (`v1.1.0`).  
При видаленні або перейменуванні ендпоінта — major-версія (`v2.0.0`).
