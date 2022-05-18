# YaMDb (Yet another Movie Database)
Стек технологий: Python 3, Django, REST API, Docker

![YaMDB_workflow](https://github.com/m00nrock/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

![Python](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue) ![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) ![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray) ![Postgres](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
  
### Алгоритм регистрации пользователей:  
  
1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт /api/v1/auth/signup/.  
2. YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.  
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).  
4. При желании пользователь отправляет PATCH-запрос на эндпоинт /api/v1/users/me/ и заполняет поля в своём профайле (описание полей — в документации).  
  
### Пользовательские роли:  
  
- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.  
- **Аутентифицированный пользователь (user)** — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.  
- **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.  
- **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.  
- **Суперюзер Django** — обладет правами администратора (admin)  
  
### Аутентификация:  
  
используется аутентификация с использованием JWT-токенов  
  
### Как запустить проект:  

#### 1. для разработки:  
Клонировать репозиторий и перейти в него в командной строке:  
  
```shell
git clone https://github.com/m00nrock/infra_sp2.git
```

Заполнените .env файл:
В директории infra_sp2/infra/ создайте .env файл и укажите значения для переменных окружения:
Пример заполнения доступен в файле .env.example

- SECRET_KEY
- HOSTS
- DB_ENGINE
- DB_NAME
- POSTGRES_USER
- POSTGRES_PASSWORD
- DB_HOST
- DB_PORT
  
Создать и активировать виртуальное окружение:  
  
```shell
python -m venv venv  
```
  
```shell
source venv/bin/activate  
```
  
```shell
python -m pip install --upgrade pip  
```
  
Установить зависимости из файла requirements.txt:  
  
```shell
pip install -r requirements.txt  
```
  
Выполнить миграции:  
  
```shell
python manage.py migrate  
```

Запустить проект:  
  
```shell
python manage.py runserver  
```

#### 2. запуск проекта в контейнерах:
Для запуска потребуется Docker. [Get Docker](https://docs.docker.com/get-docker/)

Все действия выполняются из директории /infra/

```
docker-compose up -d --build
```

### Выполнить миграции:
из директории infra/

```
docker-compose exec web python manage.py migrate
```

### Создать суперпользователя:

```
docker-compose exec web python manage.py createsuperuser
```

### Собрать статику:

```
docker-compose exec web python manage.py collectstatic --no-input
```

## Заполнить базу данных:

```
docker-compose exec web python manage.py loaddata fixtures.json
```

Проект доступен по следующим адресам:
    http://84.252.137.84/admin/ - админ-панель
    http://84.252.137.84/redoc/ - документация к API
    http://84.252.137.84/api/v1/ - API