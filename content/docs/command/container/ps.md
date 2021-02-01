---
title: "Docker的ps命令"
linkTitle: "ps命令"
weight: 545
date: 2021-02-01
description: >
  Docker的ps命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/ps/

列出容器

### 使用

```bash
docker ps [OPTIONS]
```

常用的命令行参数：

| 参数            | 描述                                                    |
| --------------- | ------------------------------------------------------- |
| --all , -a      | Show all containers (default shows just running)        |
| `--filter , -f` | Filter output based on conditions provided              |
| `--last , -n`   | Show n last created containers (includes all states)    |
| --latest , -l   | Show the latest created container (includes all states) |
| --size , -s     | Display total file sizes                                |

### 过滤

过滤标志（`-f或--filter`）格式是 key=value 对。 如果有多个过滤器，则传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"` ）

支持多种过滤方式，如 id，name，label，status等，具体看官方文档

```bash
docker ps --filter "label=color"
docker ps --filter "label=color=blue"
docker ps --filter "name=nostalgic_stallman"
docker ps --filter "name=nostalgic" # name可以使用substring
docker ps -a --filter 'exited=0'
docker ps -a --filter 'exited=137'
docker ps --filter status=running
docker ps --filter status=paused
docker ps --filter ancestor=ubuntu
docker ps --filter network=net1
docker ps --filter publish=80
docker ps --filter expose=8000-8080/tcp
docker ps --filter publish=80/udp
```



### 格式化输出

```bash
docker ps --format "{{.ID}}: {{.Command}}"
docker ps --format "table {{.ID}}\t{{.Labels}}"
```



看官方文档





