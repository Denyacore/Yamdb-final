# Api yamdb by [Denyacore](https://github.com/Denyacore)

![Api Yamdb](https://github.com/Denyacore/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Альфа версия проекта [Api Yamdb](https://github.com/Denyacore/api_yamdb) с настроенными *Continuous Integration* и *Continuous Deployment*



## Шаблон наполнения env-файла

Для функционирования - создать файл .env и прописать переменные окружения в нём.

```bash
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

## Запуск

Выполнить команду из директории, где находится файл docker-compose.yaml
```bash
docker-compose up
```
Выполнить миграции, создать суперпользователя и собрать статику. 
```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```
## Описание команды для заполнения базы данными

Отредактировать settings.py, чтобы значения загружались из переменных окружения:
```bash
DATABASES = {
    'default': {
        'ENGINE': os.getenv('DB_ENGINE'),
        'NAME': os.getenv('DB_NAME'),
        'USER': os.getenv('POSTGRES_USER'),
        'PASSWORD': os.getenv('POSTGRES_PASSWORD'),
        'HOST': os.getenv('DB_HOST'),
        'PORT': os.getenv('DB_PORT')
    }
}
```

## Примеры запросов:

Добавление нового поста.

*POST .../api/v1/posts/*

```
{
    "text": "Вечером собрались в редакции «Русской мысли», чтобы поговорить о народном театре. Проект Шехтеля всем нравится.",
    "group": 1
}
```
Пример ответа:

```
{
    "id": 14,
    "text": "Вечером собрались в редакции «Русской мысли», чтобы поговорить о народном театре. Проект Шехтеля всем нравится.",
    "author": "anton",
    "image": null,
    "group": 1,
    "pub_date": "2021-06-01T08:47:11.084589Z"
}
```
Добавление комментария к посту:

*POST .../api/v1/posts/14/comments/*

```
{
    "text": "тест тест"
}
```
Пример ответа:

```
{
    "id": 4,
    "author": "anton",
    "post": 14,
    "text": "тест тест",
    "created": "2021-06-01T10:14:51.388932Z"
}
```
Получение информацию о группе:

*GET .../api/v1/groups/2/*

Пример ответа:

```
{
    "id": 2,
    "title": "Математика",
    "slug": "math",
    "description": "Посты на тему математики"
}
```

Все подписки пользователя:

*GET .../api/v1/follow/*

Пример ответа:

```
{
    "user": "Антон Чехов",
    "following": "Максим горький"
}
```
