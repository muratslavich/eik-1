# Docker overview

```
$ docker ps  (список запущенных контейнеров)
$ docker ps -a (список запущенных и потушенных контейнеров)
$ docker images -a
$ docker volume ls

$ docker start name
$ docker stop name   (SIGTERM)
$ docker kill name   (SIGKILL)

# permanently remove container
$ docker rm -f <conteiner id> (удалить контейнер совсем)
$ docker rm $(docker ps -a -q)

# permanently remove images
$ docker rmi name
$ docker rmi $(docker images -a -q)

# clean up all resources - images, containers, volumes, and networks that are not used
$ docker system prune
$ docker images prune

$ docker run -d -name nginx -p 8080:80 nginx
    -d - запускает detached
    -p host:container - паблишит порт в контейнер (форвардит порт из системы)
    -v host:container:ro - add volume, ro - read only mode
    -rm - automatically delete container after exit
    -e VAR_NAME=VALUE - add environment variable
    --link=container_name - connect with other container, like docker-compose
    -t присваивает псевдо-tty или терминал внутри нового контейнера.
    -i позволяет создавать интерактивное соединение, захватывая стандартный вход (STDIN) контейнера.

$ docker logs name

# подцепидься к работающему контейнеру 
# -ti псевду tty консоль 
# изменения доступны на время жизни контейнера
$ docker exec -ti nginx bash

Пуш
$ docker push <hub-user>/<repo-name>:<tag>
```

[Clean up docker](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)

HOST\_PORT:CONTAINER\_PORT

Линковать контейнеры docker-compose

```
-- docker-compose.yml
версия 1 - простое описание команд запуска докер контейнеров
версия 2 - сервисы, контейнеры можно склеить, вольюмы сети и драйвера
версия 3 - docker-engine 1.13+, деплой в swarm

version: '2'
services:
wp:
  image: wordpress:4.7
  volumes:
  - ./themes:/var/www/html/wp-content/themes
  ports:
  - 80:80
  environment:
  - WORDPRESS_DB_PASSWORD=password
  - WORDPRESS_DB_HOST=db
db:
  image: mysql:5.7
  environment:
  - MYSQL_ROOT_PASSWORD=password

$ docker-compose build
$ docker-compose up -d
$ docker-compose ps
$ docker-compose down

$ docker-compose -f docker-compose-staging.yml up -d
```

dockerfile

```
FROM python:2.7-alpine
MAINTAINER Andrew S. <halfb00t@gmail.com>
    RUN apk add --no-cache libxslt libxml2
    RUN apk add --no-cache --virtual .build-deps build-base git libxslt-dev libxml2-dev \
        && cd /tmp && git clone https://github.com/halfb00t/bamboo-build-tools.git . \
        && python setup.py build && python setup.py install \
        && cd / && rm -rf /tmp/* \
        && apk del .build-deps

ENV APP_HOME /app
WORKDIR $APP_HOME

FROM - базовый имейдж
RUN - инструкции которые выполняются внутри имейджа. Каждое завершение команды создает новый слой
COPY - копирует файлы из текущего контекста системы в имейдж
ADD - аналогично копи, понимает урлы и архивы
CMD - команды которые следует выполнить пи старте проекта (like $docker run)

$ docker build -t bamboo-build-tools .
docker build -t repo/image:v2 .
```

![](https://github.com/tpucci/blog-bam-review/raw/master/assets/docker-production-stack.png) ![](https://github.com/tpucci/blog-bam-review/raw/master/assets/docker-dev-stack.png)
