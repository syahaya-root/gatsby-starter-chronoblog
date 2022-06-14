---
title: Basic Command Docker
cover: ./docker.png
date: 2022-06-14
description: Command
tags: ['Docker']
---


## Basic Command Docker

Note: Don't forget in your server is mode "sudo".

* For download/take image from docker hub.
```
docker pull <name_image>
```
* For Push image to registry/docker hub
```
docker push <name_image>
```
* For list images
```
docker images
```
* Login to docker registry
```
docker login <url_registry>
```
* For create a new container from image. After create run container use parameter "it" and "bash".
```
docker run --name mycontainer -it ubuntu bash
```
* For star the container.
```
docker start <name_container>
```
* Display all container list.
```
docker ps -a
```
* Stop docker container.
```
docker stop Mycontainer
```
* Erase the container/image.
```
docker rm <name_image/name_container>
```
* Kill the container.
```
docker kill Mycontainer
```
* For build image from Dockerfile
```
docker build
```
