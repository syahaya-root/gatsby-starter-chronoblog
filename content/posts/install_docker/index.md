---
title: Installation Docker
cover: ./docker.png
date: 2022-06-13
description: Docker
tags: ['Docker']
---

### Installation

## Docker CE

- Update first your list package
```
apt update
```
- Install some requirements package
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
- Add GPG Key for docker repository to your system
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- Add Docker repository to your apt sources in server
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
- Check and make sure you install docker from docker repo
```
apt-cache policy docker-ce
```
- And install the Docker
```
sudo apt install docker-ce
```
- Check status Docker
```
sudo systemctl status docker
```

## Docker Compose

Note : installation docker compose use repository

- Update list package
```
apt update
```
- Install docker compose latest
```
sudo apt-get install docker-compose-plugin
```
- Check version
```
docker compose version
```
