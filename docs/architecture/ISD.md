# ISD — Infrastructure Specification Document

> **Версія:** v1.0 | **Дата:** 2025-04-01  
> **Статус:** Затверджено  
> **Архітектура:** [SDD](SDD.md)

---

## 1. Середовище розгортання

| Параметр | Значення |
|----------|----------|
| Хмарний провайдер | AWS / Azure |
| Контейнеризація | Docker |
| Оркестрація | Kubernetes (K8s) |
| CI/CD | GitHub Actions |

---

## 2. Інфраструктурні компоненти

### 2.1 Мережевий рівень
- **Load Balancer** — розподіл вхідного трафіку між репліками
- **API Gateway** — маршрутизація, rate limiting, авторизація

### 2.2 Обчислювальний рівень
- Кожен мікросервіс розгортається як окремий **Docker-контейнер**
- Управляється через **Kubernetes Deployment**
- Мінімум 2 репліки на кожен сервіс у production

### 2.3 Рівень даних
- **PostgreSQL** — основна реляційна БД
- **Redis** — кешування сесій та часто запитуваних даних

### 2.4 CI/CD Pipeline

```
[Git Push / PR]
      │
      ▼
[GitHub Actions]
  ├── Lint & Tests
  ├── Build Docker Image
  ├── Push to Container Registry
  └── Deploy to Kubernetes
```

---

## 3. Масштабування

- **Горизонтальне масштабування** сервісів через K8s Deployment replicas
- **Horizontal Pod Autoscaler (HPA)** — автоматичне масштабування за навантаженням CPU/Memory
- **Load Balancer** — рівномірний розподіл трафіку

---

## 4. Моніторинг та логування

- Логи агрегуються через **ELK Stack** (Elasticsearch, Logstash, Kibana) або CloudWatch
- Метрики: **Prometheus + Grafana**
- Алерти при перевищенні порогів доступності або часу відповіді

---

## 5. Безпека інфраструктури

- TLS-термінація на рівні Load Balancer
- Secrets зберігаються в **Kubernetes Secrets** або **AWS Secrets Manager**
- Мережеві політики між сервісами (NetworkPolicy у K8s)

---

## 6. Пов'язані документи

- [SDD — Архітектура системи](SDD.md)
- [Developer Onboarding](../developer/onboarding.md)
