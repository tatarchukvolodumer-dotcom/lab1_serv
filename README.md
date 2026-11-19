# Лабораторна робота №1: Healthcheck API, Docker Compose та CI

Базовий серверний вебзастосунок на Python (Flask + Gunicorn) із системними ендпоінтами для моніторингу стану (Liveness та Readiness probes), контейнеризацією через Docker Compose та автоматизованим CI-пайплайном на базі GitHub Actions.

## Структура проєкту

* `main.py` — вебзастосунок із реалізацією ендпоінтів `/healthz/live` та `/healthz/ready`.
* `requirements.txt` — зафіксовані залежності проєкту.
* `Dockerfile` — конфігурація збірки мінімального Docker-образу на базі `python:3.11-slim`.
* `docker-compose.yaml` — оркестрація локального запуску сервісу з прокиданням портів та перевіркою стану.
* `.github/workflows/ci.yml` — пайплайн безперервної інтеграції (лінтер `ruff`, збірка Docker-образу та валідація Compose-конфігурації).

## Запуск проєкту через Docker Compose

### Попередні вимоги
* Встановлений **Docker** та **Docker Compose** (або **Docker Desktop**).

### Інструкція із запуску

1. Клонуйте репозиторій та перейдіть у директорію проєкту:
   ```bash
   git clone <URL_ВАШОГО_РЕПОЗИТОРІЮ>
   cd lab1_serv
   ```

2. Зберіть образ та запустіть контейнер у фоновому режимі:
   ```bash
   docker compose up --build -d
   ```

3. Перевірте роботу системних ендпоінтів у браузері або через `curl`:
   * **Liveness probe:**
     ```bash
     curl http://localhost:8080/healthz/live
     ```
     Очікувана відповідь (`200 OK`):
     ```json
     {
       "status": "alive",
       "timestamp": "2026-11-19T12:00:00.000000+00:00"
     }
     ```

   * **Readiness probe:**
     ```bash
     curl http://localhost:8080/healthz/ready
     ```
     Очікувана відповідь (`200 OK`):
     ```json
     {
       "database": "not_connected_yet",
       "status": "ready",
       "timestamp": "2026-11-19T12:00:00.000000+00:00"
     }
     ```

4. Для зупинки сервісу та видалення контейнерів виконайте:
   ```bash
   docker compose down
   ```
_ІО-42 Татарчук Володимир_