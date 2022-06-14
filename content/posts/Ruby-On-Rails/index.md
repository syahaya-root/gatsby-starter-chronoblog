---
title: Ruby On Rails
cover: ./dockerruby.png
date: 2022-06-13
description: Ruby
tags: ['Docker']
---


## Create Website based on Ruby

Prerequisites:
- Docker-compose installed on your server

### Dockerfile

* App for Web run inside in Docker container, you can create a Dockerfile with template:

```
# syntax=docker/dockerfile:1
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Configure the main process to run when running the image
CMD ["rails", "server", "-b", "0.0.0.0"]
```

* Create file bootstrap "Gemfile", and this will be overwritten by "rails new".

```
source 'https://rubygems.org'
gem 'rails', '~>5'
```

* Create file "Gemfile.lock".

```
touch Gemfile.lock
```

* After that, also create file "entrypoint.sh", Use this script for that file.

```
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /myapp/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
```

* You need "docker-compose,yml" for configuration services, image and other. This script can you use for that file:

```
version: "3.9"
services:
  db:
    image: postgres
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

### Build Project

* You can generete Rails skeleton use "docker-compose run".

```
docker-compose run --no-deps web rails new . --force --database=postgresql
```

* If process is done, you should have fresh app. Can check with command:

```
ls -lah
```

* Make sure ownership is non-root.

```
chown -R <user>:<user>
```

* And build again the image.

```
docker-compose build
```

### Connect Database

* For file "database.yml" in directory "/config/database.yml", adjust to:

```
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: password
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test
```

* Boot the app with "docker-compose up -d", you can see output:

```
db_1   | 2022-05-30 04:35:08.938 UTC [1] LOG:  database system is ready to accept connections
```

* And create database with command:

```
docker-compose run web rake db:create
```

* Test with the URL:
```
http://localhost:3000
```

Note: "docker ps" for check the container is up or down.
