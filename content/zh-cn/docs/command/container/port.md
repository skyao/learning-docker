---
title: "Docker的port命令"
linkTitle: "port命令"
weight: 549
date: 2021-02-01
description: >
  Docker的port命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/port/

列出容器的端口映射或特定映射

### 使用

```bash
docker port CONTAINER [PRIVATE_PORT[/PROTO]]
```

### 示例

```bash
$ docker port test
$ docker port test 7890/tcp
$ docker port test 7890/udp
$ docker port test 7890
```

