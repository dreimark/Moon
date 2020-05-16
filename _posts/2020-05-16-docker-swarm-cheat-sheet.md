---
layout: post
title: "Docker Swarm Cheat Sheet"
date: 2020-05-16 12:06:31
tags: [tech, cli, cheatsheets, docker]
description: "Basic Docker Swarm Commands"
excerpt: "Docker Swarm Cheat Sheet"
#image: ""
categories:
    - cheatsheets
---

# Create vbox manager

docker-machine create --driver virtualbox manager1

# Create hyperv manager

docker-machine create --driver hyperv manager1

# Docker Client Info

docker-machine env manager1

# Declare Manager via ssh

docker swarm init --advertise-addr 192.168.99.100

# Get Join Token on manager via ssh

docker swarm join-token worker

# Create Service Instance with Replicas

docker service create --replicas 8 -p 80:80 --name web nginx
docker stack deploy -c docker-stack.yml elk

# List Swarm Services

docker service ls

# Inspect specific Service

docker service ps elk

### scale swarm nodes

```bash
docker-compose scale node=5
```
