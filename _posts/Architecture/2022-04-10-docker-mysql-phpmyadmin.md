---
layout: post
title: '[MySQL] MySQL PHPMyAdmin Docker로 올리기'
category: Architecture
tags: [mysql, phpmyadmin, docker]
comments: true
---

## 파일 구성

```
-- docker-compose.yml
|
-- .env
|
-- mysql -- data
         |
         -- config -- my.cnf
```

## 실행
```sh
# 시작
$ docker-compose up -d

# 중단
$ docker-compose down
```


## .env

```sh
MYSQL_ROOT_PASSWORD=<MYSQL_ROOT_PASSWORD>
MYSQL_DATABASE=<MYSQL_DATABASE>
MYSQL_USER=<MYSQL_USER>
MYSQL_PASSWORD=<MYSQL_PASSWORD>
MYSQL_PORT=<MYSQL_PORT>
PMA_PORT=<PMA_PORT>
```

## my.cnf

```sh
[mysqld]
# port=3306

default-time-zone='+9:00'

character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
skip-character-set-client-handshake
max_connections = 2000
max_allowed_packet = 1G
innodb_log_file_size = 1G
innodb_buffer_pool_size = 8G
innodb_log_file_size = 256M
innodb_thread_concurrency = 16

[client]
# port=3306
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8mb4
```


## docker-compose.yml

```sh
version: '3.8'

services:
  db:
    image: mysql:5.7
    container_name: mysql
    volumes:
      - db_dat:/var/lib/mysql
    restart: always
    ports:
      - "${MYSQL_PORT}:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      TZ: Asia/Seoul
    configs:
      - source: my_conf
        target: /etc/my.cnf
        mode: 444
    networks:
      - mysql_net
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    ports:
      - '${PMA_PORT}:80'
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      PMA_CONTROLHOST: db
      PMA_CONTROLPORT: 3306
      PMA_PMADB: phpmyadmin
      PMA_CONTROLUSER: root
      PMA_CONTROLPASS: ${MYSQL_ROOT_PASSWORD}
      PMA_QUERYHISTORYDB: true
      PMA_QUERYHISTORYMAX: 5000
      MEMORY_LIMIT: 1G
    networks:
      - mysql_net
    depends_on:
      - db

configs:
  my_conf:
    file: ./mysql/config/my.cnf

networks:
  mysql_net:

volumes:
  db_dat:
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/lab/devtools/mysql-phpmyadmin/mysql/data'
```