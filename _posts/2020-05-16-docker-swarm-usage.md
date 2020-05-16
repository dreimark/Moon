---
layout: post
title: "Docker Swarm Cheat Sheet"
date: 2020-05-16 16:12:31
tags: [tech, cli, cheatsheets, docker]
description: "Basic Docker Swarm Commands"
excerpt: "Using Docker Swarm"
#image: ""
categories:
    - cheatsheets
---

# create vbox manager

```bash
docker-machine create --driver virtualbox manager1
```

# create hyperv manager

```bash
docker-machine create --driver hyperv manager1
```

# docker client info

```bash
docker-machine env manager1
```

# declare Manager via ssh

```bash
docker swarm init --advertise-addr 192.168.99.100
```

# get join token on manager via ssh

```bash
docker swarm join-token worker
```

#create service instance with replicas

```bash
docker service create --replicas 8 -p 80:80 --name web nginx
docker stack deploy -c docker-stack.yml elk
```

# list swarm services

```bash
docker service ls
```

# inspect specific service

```bash
docker service ps elk
```

# scale swarm nodes

```bash
docker-compose scale node=5
```