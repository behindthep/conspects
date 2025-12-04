Быстрый запуск популярных Docker контейнеров
Команды для запуска контейнеров из командной строки или через docker-compose. Mysql, Postgres, Mongo, LAMP

docker run -d --name=mysql-container -e MYSQL_ROOT_PASSWORD=passw0rd -p 3306:3306 mysql

Пользователь root, пароль passw0rd, порт для подключения 3306.

Возможные параметры
docker run \
--detach \
--name=container-name \
--env="MYSQL_ROOT_PASSWORD=root-password" \
--env="MYSQL_DATABASE=schema_name" \
--publish port:3306 \
--volume=path-to-Mysql-data:/var/lib/mysql \
mysql

Переменные:

container-name - имя контейнера, например library_mysql, todo_mysql.
root-password - пароль для пользователя root.
schema_name - имя базы данных, создаваемой при старте контейнера
port - порт через который можно будет подключится к базе данных в контейнере.
path-to-Mysql-data - путь до директории, в которой будут хранятся файлы базы данных. Если параметр volume не указывать, все данные будут потерены если контейнер удалить.
Пример команды с установленными параметрами:

Контейнер с именем sendel_mysql доступен на порту 3336, пароль root “12345678”, база данных сохраняется в директории /home/user/mysql-data:
docker run \
--detach \
--name=sendel_mysql \
--env="MYSQL_ROOT_PASSWORD=12345678" \
--env="MYSQL_DATABASE=db" \
--publish 3336:3306 \
--volume=/home/user/mysql-data:/var/lib/mysql \
mysql

Создайте файл ‘docker-compose.yml’ с содержимым, поменяйте значения параметров при необходимости.

version: '3.3'
services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_DATABASE: 'db' # имя бд в контейнере
      MYSQL_USER: 'user' # пользователь, права имеет только на MYSQL_DATABASE
      MYSQL_PASSWORD: 'password' # пароль пользователя
      MYSQL_ROOT_PASSWORD: 'password' # пароль пользователя root 
    ports:
      - '3336:3306' # 3336 будет виден снаружи контейнера
    # данные всех бд будут сохранены в /home/user/mysql-data
    volumes:
      - /home/user/mysql-data:/var/lib/mysql

Запуск. Перейдите в директорию с файлом docker-compose.yml и выполните:

docker-compose up -d

docker run -d --name=postgres-container -e POSTGRES_PASSWORD=passw0rd -p 5432:5432 postgres

Пользователь postgres, пароль passw0rd, порт для подключения 5432.

Возможные параметры
docker run \
--detach \
--name=container-name \
--env="POSTGRES_PASSWORD=postgres-password" \
--env="POSTGRES_DB=db-name" \
--publish port:5432 \
--volume=path-to-postgres-data:/var/lib/postgresql/data \
postgres

Переменные:

container-name - имя контейнера, например library_postgres, todo_postgres.
postgres-password - пароль для пользователя postgres.
db-name - имя бд, которая будет создана при старте контейнера, если не указывать будет postgres
port - порт через который можно будет подключится к базе данных в контейнере.
path-to-postgres-data - путь до директории, в которой будут хранятся файлы базы данных. Если параметр volume не указывать, все данные будут потеряны при удаления контейнера.
По-умолчанию, имя пользователя postgres.

Контейнер с именем sendel_postgres доступен на порту 5454, пароль “12345678” у пользователя postgres , база данных сохраняется в директории /home/user/postgres-data:
docker run \
--detach \
--name=sendel_postgres \
--env="POSTGRES_PASSWORD=12345678" \
--publish 5454:5432 \
--volume=/home/user/postgres-data:/var/lib/postgresql/data \
postgres

Создайте файл ‘docker-compose.yml’ с содержимым, поменяйте значения параметров при необходимости.

version: '3.3'
services:
  db:
    image: postgres:latest
    restart: always
    environment:
      POSTGRES_PASSWORD: 'password'
    ports:
      - '5432:5432'
    # данные всех бд контейнера будут сохранены в /home/user/postgres-data
    volumes:
      - /home/user/postgres-data:/var/lib/postgresql/data

Запуск. Перейдите в директорию с файлом docker-compose.yml и выполните:

docker-compose up -d

docker run -d --name=mongo-container -p 27017:27017 mongo

Подключение без пользователя по адресу mongodb://localhost:27017

Возможные параметры
docker run \
--detach \
--name=[container-name] \
--env="MONGO_INITDB_ROOT_USERNAME=[root-username]" \
--env="MONGO_INITDB_ROOT_PASSWORD=[root-password]" \
--publish [port]:27017 \
--volume=[path-to-mongo-data]:/data/db \
postgres

Переменные:

[container-name] - имя контейнера, например library_mongodb, mongodb.
[root-username] - имя пользователя root.
[root-password] - пароль для пользователя root.
[port] - порт через который можно будет подключится к базе данных в контейнере.
[path-to-mongo-data] - путь до директории, в которой будут хранятся файлы базы данных. Если параметр volume не указывать, все данные будут потеряны если контейнер удалить.
Пример команды с установленными параметрами:

Контейнер с именем mongodb доступен на порту 27017, пароль “12345678” у пользователя postgres , база данных сохраняется в директории /home/user/mongo-data:
docker run \
--detach \
--name=mongodb \
--env="MONGO_INITDB_ROOT_USERNAME=root" \
--env="MONGO_INITDB_ROOT_PASSWORD=12345678" \
--publish 27017:27017 \
--volume=/home/user/mongo-data:/data/db \
mongo

Создайте файл ‘docker-compose.yml’ с содержимым, поменяйте значения параметров при необходимости.

version: '3.3'
services:
  db:
    image: mongo:latest
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: 'root'
      MONGO_INITDB_ROOT_PASSWORD: '12345678'
    ports:
      - '27017:27017'
    # данные всех бд контейнера будут сохранены в /home/user/mongo-data
    volumes:
      - /home/user/mongo-data:/data/db

Запуск. Перейдите в директорию с файлом docker-compose.yml и выполните:

docker-compose up -d

LAMP Docker контейнер
Запустим docker контейнер с PHP 7.x/8.x, MySQL 8.x, Apache 2.4, phpMyAdmin.

Быстрый и простой вариант использовать образ mattrayner/lamp:

docker run --name lamp -p "80:80" -p "3306:3306" -v [path-to-app]:/app mattrayner/lamp:latest-1804

Переменные:

[path-to-app] - директория в которой находится ваше php приложение.
Пример команды с установленными параметрами:

Контейнер с именем lamp доступен на порту 80, директория приложения разворачиваемого в LAMP /home/user/app:
docker run -d --name lamp -p "80:80" -p "3306:3306" -v ~/app:/app mattrayner/lamp:latest-1804

Для доступа к приложению перейдите по адресу http://127.0.0.1/

MySql доступен на порту 3306, пользователь по умолчанию admin. Пароль для пользователя находится в логе запуска контейнера. Прочитать командой

docker logs [container-id] | grep uadmin

где [container-id] - id контейнера с LAMP.

<img width="1904" height="209" alt="image" src="https://github.com/user-attachments/assets/3078c59f-07d9-429f-afd5-c641ef4c71ba" />

Как получить пароль MySql

посмотреть id контейнера. И выделен пароль при чтении лога, пароль на скриншоте строка без -p, а именно 6ySY6mgmM2EF, логин пользователя admin.

Данные бд сохранены только в контейнере. Для полноценной настройки LAMP контейнера вариант запуска Docker Compose или добавьте параметр -v [path-to-mysql-data]:/var/lib/mysql, где замените [path-to-mysql-data] на путь до директории, в которой уже хранятся данные или пустая директория.

Docker Compose
Создайте файл ‘docker-compose.yml’ с содержимым, поменяйте значения параметров при необходимости.

version: '3.3'
services:
  lamp:
    image: mattrayner/lamp:latest-1804
    container_name: lamp
    restart: always
    environment:
      MYSQL_ADMIN_PASS: '12345678' # пароль пользователя admin
      MYSQL_USER_NAME: 'user' # создать нового пользавтеля user
      MYSQL_USER_PASS: 'supersecretpass' # пароль пользователя user
      MYSQL_USER_DB: 'mydb' # создать бд для пользователя user
      PHP_UPLOAD_MAX_FILESIZE: '32M'
      PHP_POST_MAX_SIZE: '32M'
    ports:
      - '80:80'
      - '3306:3306'
    # данные всех бд контейнера будут сохранены в /home/user/mysql-data
    # приложение PHP будет использовать из директории /home/user/app
    volumes:
      - /home/user/mysql-data:/var/lib/mysql
      - /home/user/app:/app

Запуск. Перейдите в директорию с файлом docker-compose.yml и выполните:

docker-compose up -d

Автостарт контейнеров
Чтобы контейнер всегда стартовал автоматически при старте системы или произошла ошибка и контейнер аварийно завершился, установите параметр restart:always для docker-compose или --restart always для docker команды.

Если хотите чтобы был автостарт до того как вы вручную не остановите командой docker stop, используйте параметр restart: unless-stopped или --restart unless-stopped для docker команды.

restart: unless-stopped

docker run --name lamp -p "80:80" -p "3306:3306" --restart no -v /home/sendel/app:/app mattrayner/lamp:latest-1804

Для пользователей RHEL, CentOS или Fedora
Если у вас ошибка чтения или монитирования volume. Используйте в окончании пути :Z, пример:

docker run --name lamp -p "80:80" -p "3306:3306" --restart no -v /home/sendel/app:/app:Z mattrayner/lamp:latest-1804
