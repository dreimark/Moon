---
layout: post
title: "Docker Cheat Sheet"
#subtitle: ""
#image: ""
date: 2020-05-15 20:38:41
tags:
    - tech
    - cli
    - cheatsheets
    - docker
description: "Bread and butter docker commands and handy scripts"
categories:
    - cheatsheets
---

## managing containers

Managing containers can be a hassle and quite time consuming without some easy tweaks. I also recommend assigning these commands to an alias in your ~/.bashrc making it available and convenient in your home environment and remote shells.

### stop containers

```bash
docker stop $(docker ps -a -q)
```

### delete downloaded Images

```bash
docker rmi -f $(docker images -q)
```

### delete stopped containers

```bash
docker rm $(docker ps -a -q)
```

### delete dangling images

```bash
docker rmi $(docker images -q)
```

### live log containers

```bash
docker-compose logs --follow
```

### live log specific containers

```bash
docker logs -ft dc --follow
```

### bash into container

```bash
docker exec -ti pihole /bin/bash
```

### inspect specific container

```bash
docker inspect [container-id]
```

### scale swarm nodes

```bash
docker-compose scale node=5
```

### mount volumes

```bash
docker run -it -v pihole-etc:/mnt/ ubuntu bash
```

### save running container as image

```bash
docker commit -m “commit message” -a “author” container_name username/image_name:tag
```

### debian

```bash
docker run -d --name debian -it debian
```

### alpine

```bash
docker run -it alpine /bin/sh
```

## Automating docker with scripts

### kill & delete stopped containers, images & volumes

Simple task for removing running things.

```bash
#!/usr/bin/env bash
docker-compose down
docker kill $(docker ps -q)
docker rm $(docker ps -a -q)
docker system prune -f
docker image prune -f
docker volume prune -f
docker ps
```

### compose kill, delete containers, images and volumes

This snippet also includes compose down and up rather than restart, since i was running in some issues using restart option with stateless containers.

```bash
#!/usr/bin/env bash
docker-compose down
docker kill $(docker ps -q)
docker rm $(docker ps -a -q)
docker system prune -f
docker image prune -f
docker volume prune -f
docker rmi -f $(docker images -q)
docker-compose pull
docker-compose down
docker-compose up
docker ps
docker-compose ps
docker-compose logs --follow
```

### compose down up as a service

no brainer for restarting projects and keep them as a service.

```bash
#!/usr/bin/env bash
docker-compose pull
docker-compose down
docker-compose pull
docker-compose up -d --build
docker ps
docker-compose ps
```
