# YamDB_final

![example workflow](https://github.com/iricshkin/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Документация доступна по адресу: http://51.250.105.138:8080/redoc/

## REST API для сервиса YaMDb — базы отзывов о фильмах, книгах и музыке.

версия с Docker, Continuous Integration на GitHub Actions

Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Произведению может быть присвоен жанр. Новые жанры может создавать только администратор.
Читатели оставляют к произведениям текстовые отзывы и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти).
Из множества оценок автоматически высчитывается средняя оценка произведения.

Аутентификация по JWT-токену

Поддерживает методы GET, POST, PUT, PATCH, DELETE

Предоставляет данные в формате JSON

Cоздан в команде из трёх человек с использованим Git в рамках учебного курса Яндекс.Практикум.

## Стек технологий

- проект написан на Python с использованием Django REST Framework
- библиотека Simple JWT - работа с JWT-токеном
- библиотека django-filter - фильтрация запросов
- базы данных - SQLite3
- система управления версиями - git

## Ресурсы API YaMDb

**AUTH**: аутентификация.

**USERS**: пользователи.

**TITLES**: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

**CATEGORIES**: категории (типы) произведений ("Фильмы", "Книги", "Музыка").

**GENRES**: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

**REVIEWS**: отзывы на произведения. Отзыв привязан к определённому произведению.

**COMMENTS**: комментарии к отзывам. Комментарий привязан к определённому отзыву.

## Алгоритм регистрации пользователей

Пользователь отправляет POST-запрос с параметром email на `/api/v1/auth/email/`.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
Пользователь отправляет POST-запрос с параметрами email и confirmation_code на `/api/v1/auth/token/`, в ответе на запрос ему приходит token (JWT-токен).
Эти операции выполняются один раз, при регистрации пользователя. В результате пользователь получает токен и может работать с API, отправляя этот токен с каждым запросом.

## Пользовательские роли

**Аноним** — может просматривать описания произведений, читать отзывы и комментарии.

**Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.

**Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.

**Администратор (admin)** — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

**Администратор Django** — те же права, что и у роли Администратор.

## Установка

Склонируйте репозиторий. Находясь в папке с кодом создайте виртуальное окружение `python -m venv venv`, активируйте его (Windows: `source venv\scripts\activate`; Linux/Mac: `source venv/bin/activate`), установите зависимости `python -m pip install -r requirements.txt`.

Для запуска сервера разработки, находясь в директории проекта выполните команды:

```
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

## Запуск проекта

Перейдите в папку проекта и выполните команду:

```
docker-compose up -d --build
```

При первом запуске для функционирования проекта выполните команды:

```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```

Заполните базу начальными данными

```
docker-compose exec web python manage.py loaddata fixtures.json
```

## Деплой на удаленный сервер

Необходимо создать переменные окружения в вашем репозитории github в разделе `secrets`

```
DOCKER_PASSWORD # Пароль от Docker Hub
DOCKER_USERNAME # Логин от Docker Hub
HOST # Публичный ip адрес сервера
USER # Пользователь сервера
PASSPHRASE # Если ssh-ключ защищен фразой-паролем
SSH_KEY # Приватный ssh-ключ
TELEGRAM_TO # ID телеграм-аккаунта (для оправки уведомления об успешном деплое)
TELEGRAM_TOKEN # Токен бота (для оправки уведомления об успешном деплое)
```

При каждом обновлении репозитория (git push) будет происходить:

- проверка кода соответствие страндарту PEP8 (с помощью пакета flake8)
- выполнение тестов pytest
- сборка и обновление образа на сервисе Docker Hub
- автоматический деплой на сервер, указанный в secrets
- отправка уведомления в Telegram

### Участники:

**Фок Ирина**

- Отзывы (Review) и комментарии (Comments): модели и view, эндпойнты, права доступа для запросов. Рейтинги произведений.
- Докеризация, разработка процесса CI (непрерывной интеграции) с использованием GitHub Actions.
- Подготовка к production и deploy на YandexCloud.

**Баранов Михаил** Управление пользователями (Auth и Users): система регистрации и аутентификации, права доступа, работа с токеном, система подтверждения e-mail, поля.

**Тимершин Руслан** Категории (Categories), жанры (Genres) и произведения (Titles): модели, view и эндпойнты для них и рейтинги.
