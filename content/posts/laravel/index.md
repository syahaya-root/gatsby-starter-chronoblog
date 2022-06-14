---
title: Laravel With Docker Compose
cover: ./dockerlaravel.png
date: 2022-06-13
description: Laravel
tags: ['Docker']
---

### Obtaining Demo Application

* Enter the directory and take the repository from community github laravel.
```
cd ~
curl -L https://github.com/do-community/travellist-laravel-demo/archive/tutorial-1.0.1.zip -o travellist.zip
```
* Install application "unzip" for extract the code.
```
apt install unzip
```
* Unzip the contents and rename for easier access.
```
unzip travellist.zip
mv travellist-laravel-demo-tutorial-1.0.1 travellist-demo
```
* Enter the directory demo.
```
cd travellist-demo
```
* You can copy file .env.example for new file .env (file .env is used to laravel configuration for set up environment-dependent configuration)
```
cp .env.example .env
```
* Open file .env.
```
nano .env
```
* Change the variables in DB_HOST with "db".
```
DB_HOST=db
DB_DATABASE=travellist
DB_USERNAME=travellist_user
DB_PASSWORD=password
```

### Dockerfile

* Create a Dockerfile.
```
nano Dockerfile
```
* You can use this template.
```
FROM php:7.4-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Set working directory
WORKDIR /var/www

USER $user
```

### Nginx

* Create directory and file for configuration nginx.
```
mkdir -p docker-compose/nginx
nano docker-compose/nginx/travellist.conf
```
* Use this template for the travellist.conf.
```
server {
    listen 80;
    index index.php index.html;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /var/www/public;
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
    location / {
        try_files $uri $uri/ /index.php?$query_string;
        gzip_static on;
    }
}
```
* Create directory "mysql" in the directory "docker-compose".
```
mkdir docker-compose/mysql
```
* And create the file "init_db.sql", use this scripts.
```
DROP TABLE IF EXISTS `places`;

CREATE TABLE `places` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `visited` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

INSERT INTO `places` (name, visited) VALUES ('Berlin',0),('Budapest',0),('Cincinnati',1),('Denver',0),('Helsinki',0),('Lisbon',0),('Moscow',1),('Nairobi',0),('Oslo',1),('Rio',0),('Tokyo',0);
```

### Docker-compose

* Create a file docker-compose.yml. And can this scripts.
```
version: "3.7"
services:
  app:
    build:
      args:
        user: sammy
        uid: 1000
      context: ./
      dockerfile: Dockerfile
    image: travellist
    container_name: travellist-app
    restart: unless-stopped
    working_dir: /var/www/
    volumes:
      - ./:/var/www
    networks:
      - travellist

  db:
    image: mysql:5.7
    container_name: travellist-db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: ${DB_DATABASE}
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_USER: ${DB_USERNAME}
      SERVICE_TAGS: dev
      SERVICE_NAME: mysql
    volumes:
      - ./docker-compose/mysql:/docker-entrypoint-initdb.d
    networks:
      - travellist

  nginx:
    image: nginx:alpine
    container_name: travellist-nginx
    restart: unless-stopped
    ports:
      - 8000:80
    volumes:
      - ./:/var/www
      - ./docker-compose/nginx:/etc/nginx/conf.d/
    networks:
      - travellist

networks:
  travellist:
    driver: bridge
```

* Build the application with docker-compose.
```
docker-compose build app
```
* After build the application, you can run with command.
```
docker-compose up -d
```
* Check the container is running.
```
docker-compose ps
```
* Make sure owner and groups file is non-root, you can check with command.
```
docker-compose exec app ls -l
```
* After that, run composer install to install application dependencies.
```
docker-compose exec app composer install
```

### Troubleshoot: 
* if you got error, you can follow the URL:
(https://stackoverflow.com/questions/61177995/laravel-packagemanifest-php-undefined-index-name)

* Generate a unique application key with the artisan.
```
docker-compose exec app php artisan key:generate
```
### Test your application
```
http://server_domain_or_IP:8000 or http://localhost:8000
```
