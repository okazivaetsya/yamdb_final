# Проект YAMDB
![workflow status badge](https://github.com/okazivaetsya/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg?event=push)

Проект собирает отзывы и оченки к различным произведениям. 


### Описание
Проект YAMDB собирает отзывы и оченки к различным произведениям.

В проекте реализован CI (Continuous Integration) и CD (Continuous Deployment) а именно:
* автоматический запуск тестов(проверка на PEP8),
* обновление образов на Docker Hub,
* автоматический деплой на боевой сервер при пуше в главную ветку main
* отправка уведомления в телеграм чат


### Инструкция по запуску проекта
1. Склонируйте репозиторий на локальный компьютер:
```bash 
git clone git@github.com:okazivaetsya/yamdb_final.git
```
2. Зайдите на свой удаленный сервер:
```bash 
ssh <username>@<host>
```
3. Установите Docker и docker-compose на сарвер:
```bash 
# Установливаем утилиту для скачивания файлов
sudo apt install curl

# Скачиваем скрипт для установки докера
curl -fsSL https://get.docker.com -o get-docker.sh

# Запускаем скрипт
sh get-docker.sh

# Устаналиваем docker-compose
sudo apt install docker-compose -y 
```
4. Сопируйте файлы docker-compose.yml и nginx.conf из директории infra с локального компьютера на сервер:
```bash
scp docker-compose.yml <username>@<host>:
scp nginx.conf <username>@<host>:/nginx/
```
5. Создайте файл с переменными окружения .env 
```bash
DB_ENGINE=django.db.backends.postgresql
DB_NAME=<имя базы данных> 
POSTGRES_USER=<логин для подключения к базе данных>
POSTGRES_PASSWORD=<пароль для подключения к БД>
DB_HOST=<название сервиса (контейнера)>
DB_PORT=<порт для подключения к БД>
SECRET_KEY=<секретный ключ проекта django>
```
6. Добавьте в GitHub следующие Actions secrets:
```bash
DB_ENGINE=<django.db.backends.postgresql>
DB_HOST=<db>
DB_PORT=<порт для подключения к БД>
DB_NAME=<имя базы данных> 
POSTGRES_USER=<пользователь бд>
POSTGRES_PASSWORD=<пароль>
DOCKER_USERNAME=<имя пользователя на DockerHub>
DOCKER_PASSWORD=<пароль от DockerHub>
SECRET_KEY=<секретный ключ проекта django>
USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш SSH ключ>
TELEGRAM_TO=<ID чата, в который придет сообщение. Получить можно тут [@userinfobot](https://t.me/userinfobot)
TELEGRAM_TOKEN=<токен вашего бота. Получить можно тут[@BotFather](https://t.me/BotFather)
```
7. Запустите docker-compose:
```bash
sudo docker-compose up -d --build 
```
8. Выполните миграции, создайте суперпользователя и соберите статику:
```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```


### Ссылка на развернутый проект
http://okazivaetsya.ddns.net



### Примеры запросов к API
Доступные энд-поинты:
GET-запросы
```
/api/v1/users/
```
```
/api/v1/titles/
```
```
/api/v1/categories/
```
```
/api/v1/genres/
```
```
/api/v1/titles/{title_id}/reviews/
```
```
/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
