---
title: "Docker In Docker"
date: 2019-08-25T19:05:07+08:00
Keywords: ["Docker in Docker","Ansible","Docker"]
draft: false
---
>是以圣人之治,虚其心, 实其腹,弱其志,强其骨.

Docker In Docker=在Docker Contanier里运行Docker? 有这样的需求？当然有，官方还专门有这样一个[Docker Image](!https://hub.docker.com/_/docker). 可以用于编译测试Docker本身。

当然这个用法有点复杂。我们还可以当WSL用。有时候Windows中运行Docker,运行shell还不是很方便，所有一种简单的Docker In Docker环境就很有用了。

<!--more-->

## How

只需将Docker二进制文件，和docker.sock映射到任何容器中。这个容器就可以用Docker命令了。

`docker run -it --rm -v /usr/local/bin/docker:/usr/local/bin/docker -v /var/run/docker.sock:/var/run/docker.sock wxdlong/docker bash`

## Example
```bash
[root@783e8d479aa8 /]# docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS               NAMES
783e8d479aa8        wxdlong/docker           "bash"                   12 seconds ago      Up 11 seconds                           modest_poitras
```

## Ansible Invoke Docker

