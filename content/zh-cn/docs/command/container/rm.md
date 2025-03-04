---
title: "Docker的rm命令"
linkTitle: "rm命令"
weight: 550
date: 2021-02-01
description: >
  Docker的rm命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/rm/

删除一个或者多个容器

### 使用

```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

命令行参数：

| 参数             | 描述                                                    |
| ---------------- | ------------------------------------------------------- |
| `--force , -f`   | Force the removal of a running container (uses SIGKILL) |
| `--link , -l`    | Remove the specified link                               |
| `--volumes , -v` | Remove the volumes associated with the container        |

### 示例

```bash
$ docker rm test
$ docker rm --force redis
$ docker rm $(docker ps -a -q) # 删除所有停止运行的容器
$ docker rm -v redis
```

