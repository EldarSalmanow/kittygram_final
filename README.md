# Kittygram

Kittygram - веб-приложение для публикации карточек котиков. Пользователи могут регистрироваться, добавлять питомцев с фотографиями и цветами, редактировать карточки и просматривать общий каталог.

## Цели проекта

- Развернуть fullstack-приложение в контейнерах.
- Настроить стабильную отдачу статики и медиа через nginx.
- Автоматизировать проверки качества кода и сборку образов через GitHub Actions.
- Обеспечить простой процесс запуска на сервере через Docker Compose.

## Функциональность

- Регистрация и авторизация пользователей.
- CRUD-операции с карточками котиков.
- Загрузка и отображение изображений котиков.
- Пагинация списка карточек.
- Раздельная работа backend, frontend, PostgreSQL и nginx через контейнеры.

## Стек технологий

- Backend: Python, Django, Django REST Framework, Djoser, Gunicorn
- Frontend: React
- База данных: PostgreSQL
- Веб-сервер: Nginx
- Контейнеризация: Docker, Docker Compose
- CI/CD: GitHub Actions
- Линтинг/форматирование: Ruff, pycodestyle, pre-commit

## Структура сервисов

- db - контейнер PostgreSQL
- backend - Django API + админка
- frontend - сборка React-статики
- gateway - nginx, проксирование API/админки и выдача статики/медиа

## Переменные окружения (.env)

Создайте файл `.env` в корне проекта. Можно взять за основу `.env.example`.

Минимально необходимые переменные:

```env
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_NAME=kittygram
DB_HOST=db
DB_PORT=5432

SECRET_KEY=django-insecure-change-me
DEBUG=False
ALLOWED_HOSTS=127.0.0.1,localhost
TIME_ZONE=Europe/Moscow
USE_SQLITE=False
```

Пояснения:

- `SECRET_KEY`, `ALLOWED_HOSTS`, `DEBUG`, `TIME_ZONE` загружаются Django из `.env`.
- Если `USE_SQLITE=True`, backend использует SQLite для быстрого локального тестирования.
- Если `USE_SQLITE=False`, используется PostgreSQL.

## Локальный запуск через Docker Compose

1. Соберите и запустите контейнеры:

```bash
docker compose up -d --build
```

2. Проверьте доступность:

- Главная страница: `http://localhost:9000/`
- Админка: `http://localhost:9000/admin/`
- API: `http://localhost:9000/api/`

3. Остановите проект:

```bash
docker compose down
```

## Деплой и CI/CD

Workflow находится в `.github/workflows/main.yml` и дублируется в `kittygram_workflow.yml`.

- Тесты запускаются при push в любую ветку.
- Сборка и push Docker-образов выполняются только для веток `main` и `master`.
- Docker Hub логин и токен берутся из GitHub Secrets.
- В Telegram отправляется расширенное сообщение: автор push, сообщение коммита и ссылка на коммит.

Пример необходимых секретов:

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `DOCKER_HUB_LOGIN`
- `TELEGRAM_TO`
- `TELEGRAM_TOKEN`

## Проверка качества кода

В workflow выполняются:

- `ruff format --check backend/`
- `ruff check backend/`

Также добавлен pre-commit-конфиг для запуска этих проверок перед коммитом.

Установка pre-commit локально:

```bash
pip install pre-commit
pre-commit install
pre-commit run --all-files
```

## Запуск тестов проекта

```bash
pytest
```
