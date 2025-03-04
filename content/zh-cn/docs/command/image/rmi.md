---
title: "Docker的rmi命令"
linkTitle: "rmi命令"
weight: 526
date: 2021-02-01
description: >
  Docker的rmi命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/rmi/

删除本地镜像

### 使用

```bash
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

可以使用镜像的短ID或长ID，Tag或digest删除镜像。 

如果镜像有一个或多个引用它的Tag，则必须在删除镜像之前删除所有Tag。通过Tag删除镜像时，将自动删除Digest引用。

或者使用 "-f" 标记来强制删除（包括所有tag的镜像）。

### 清理镜像

通过 `docker system df` 命令可以看到当前本地镜像的磁盘使用情况：

```bash
$ docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              38                  1                   7.314GB             7.314GB (99%)
Containers          4                   0                   0B                  0B
Local Volumes       0                   0                   0B                  0B
Build Cache         0                   0                   0B                  0B
```

可以通过 `docker images` 命令查看当前本地镜像情况，然后手工删除不需要的镜像。

`docker system prune` 命令可以帮助删除不需要使用的各种资源。

可以配合 `docker images` 命令来进行批量删除，如：

```bash
$ docker rmi $(docker images istio/* -q)
Untagged: istio/istio-ca:0.7.1
Untagged: istio/istio-ca@sha256:744e7a4426474d10f7984c601590ee6dab304f5cf6677a80b37c3025993dbd4e
Deleted: sha256:5c82fea42c7850f7f42e3b6326bc35b2a8941e77532210d925770eb501c6de1b
Deleted: sha256:8ccea48ceaaffc2fab84cc0d579948a221249d384156f6cfbdbcc9341ec59be7
Deleted: sha256:6ecf4e847360cffdcfe3a5d1b5de4e27b227aa5688c0ed63170cff2ecf7e43f6
Untagged: istio/mixer:0.7.1
......
```

如果要清空本地镜像，可以执行命令 `docker rmi -f $(docker images -q)`。

