# Тестовое задание Junior.
## Bewise

# Стек
- Python 3.10
- Docker
- docker-compose
- Django 3.2.15
- Django REST framework
- CI/CD
- PostgreSQL

## Описание задания bewise:
- В сервисе реализовано REST API, принимающее на вход POST запросы с содержимым вида {"questions_num": integer}.
- После получения запроса сервис, в свою очередь, запрашивает с публичного API (англоязычные вопросы для викторин) https://jservice.io/api/random?count=1 указанное в полученном запросе количество вопросов.
- Далее, полученные ответы сохраняются в базе данных из п. 1, причем сохраняется как минимум следующая информация: ID вопроса, Текст вопроса, Текст ответа, Дата создания вопроса.
- Если в БД имеется такой же вопрос, к публичному API с викторинами выполняются дополнительные запросы до тех пор, пока не будет получен уникальный вопрос для викторины.
- Ответом на запрос из п.2.a предыдущей сохранённый вопрос для викторины. В случае его отсутствия - пустой объект.

## Запуск API с использованием CI/CD:

### Установить docker, docker-compose на сервере виртуальной машины:

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install curl -y
```

```bash
sudo curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh 
```

```bash
get-docker.sh && sudo rm get-docker.sh
```

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

### Переносим файлы docker-compose.yml и .env на сервер.

```bash
scp .env username@server_ip:/home/username/ваша-директория/
```

```bash
scp docker-compose.yml username@server_ip:/home/username/ваша-директория/
```

```bash
scp default.conf username@server_ip:/home/username/ваша-директория/
```

- Так же, можно создать пустой файл .env в директории:

```bash
touch .env
```
- Заполнить в настройках репозитория секреты в .env:

```bash
DB_ENGINE='django.db.backends.postgresql'
POSTGRES_DB='bewise'
POSTGRES_USER='user-bewise'
POSTGRES_PASSWORD='password-bewise'
DB_HOST='db'
DB_PORT='5432'
SECRET_KEY='поместите_свой_ключ-Django'
ALLOWED_HOSTS='127.0.0.1, localhost, backend, ваш_IP-сервер'
DEBUG=False
```

### Запускаем контейнеры находясь в директории:
```bash
sudo docker-compose up -d --build
```

- Затем применяем миграции, собираем статику:

```bash
sudo docker-compose exec backend python manage.py makemigrations
```
```bash
sudo docker-compose exec backend python manage.py migrate --noinput 
```
```bash
sudo docker-compose exec backend python manage.py createsuperuser
```
```bash
sudo docker-compose exec backend python manage.py collectstatic --no-input
```

## API будет доступно по адресу: http://ваш_IP-сервер/api/post/

## Остановка контейнеров:
```bash
sudo docker-compose stop/down
```

Автор: [TonyxRazzor](https://github.com/TonyxRazzor)