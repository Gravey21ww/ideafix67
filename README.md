# Навигатор ЕГЭ — MAX Bot

Бот помогает изучать темы ЕГЭ по математике через MAX и опубликованный Chatflow в Dify.

- Репозиторий: https://github.com/Gravey21ww/ideafix67
- Проект в MAX: https://max.ru/ideya_fiks_154_bot

## Файлы проекта

- `Dockerfile` — инструкция для сборки Docker-образа.
- `requirements.txt` — Python-зависимости и их версии.
- `exam_bot/` — код бота, в том числе рендерер графиков.
- `assets/` — ресурсы для отображения формул.
- `dify/prompts/` — системные инструкции для Chatflow в Dify.

## Требования

- Docker Desktop с работающим Linux Engine.
- Интернет для загрузки базового образа, Python-пакетов и Chromium.
- Токен бота MAX.
- API-ключ приложения Dify и настроенный опубликованный Chatflow.

Зависимости из `requirements.txt`:

| Пакет | Версия | Назначение |
|---|---:|---|
| `requests` | `2.34.2` | Совместимость со списком зависимостей MAX Bot v1 |
| `certifi` | `2026.10.23` | Пакет сертификатов из списка MAX Bot v1 |
| `playwright` | `1.64.1` | Рендеринг формул и графиков в PNG через Chromium |

## Сборка Docker-образа в Windows PowerShell

Откройте PowerShell в папке `MAX_Bot`. Создайте локальный файл настроек и заполните в нём `MAX_BOT_TOKEN` и `DIFY_API_KEY` (он вставлен уже тестовый):
Соберите образ:
docker build -t max-ege-bot:latest .
Создайте постоянное хранилище для состояния бота:
docker volume create max-ege-data
Запустите бота:
docker run -d --name max-ege-bot --restart unless-stopped --env-file .env -e DATA_DIR=/app/data -v max-ege-data:/app/data max-ege-bot:latest run
Посмотреть журнал работы:
docker logs -f max-ege-bot
Проверить настройки подключения к MAX и Dify:
docker run --rm --env-file .env max-ege-bot:latest check
max-ege-data хранит состояние бота между перезапусками. Не удаляйте этот Docker volume при обновлении контейнера.
