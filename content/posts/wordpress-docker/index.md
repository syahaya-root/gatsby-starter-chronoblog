---
title: Wordpress With Docker Compose
cover: ./dockerwp.png
date: 2022-06-14
description: Wordpress
tags: ['Docker']
---

### Build Wordpress With Compose

Create Directory empty, and create a file ***docker-compose.yml*** (you can use extension .yml or yaml)

File ***docker-compose.yml*** start for your Wordpress and separate MySQL instance with volume mounts for data persistence.

You can use this template for the ***docker-compose.yml***:

```
# ./docker-compose.yml
 version: '3'

 services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
#phpmyadmin      
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8080:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password 
    networks:
      - wpsite
#wordpress      
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:
```

After that, Run the command:
```
docker-compose up -d
```
If you get error, i recommend for shutdown and cleanup. You can use the command:
```
docker-compose down 
docker-compose down --volumes
```
#### Explanation
* docker-compose down for removes the containers and default network, but preserves your WordPress database.
* docker-compose down --volumes for removes the containers, default network, and the WordPress database.
