---
title: "Docker的top命令"
linkTitle: "top命令"
weight: 546
date: 2021-02-01
description: >
  Docker的top命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/top/

显示容器的运行进程

### 使用

```bash
docker top CONTAINER [ps OPTIONS]
```

命令行参数可以参考ps命名。

```bash
# 在一个终端中启动容器
$ docker run -it --rm --name toptest  ubuntu bash
root@7493e17d1238:/# 
# 在另一个终端中查看
$ docker top toptest
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                21872               21848               0                   11:46               pts/0               00:00:00            bash
```





