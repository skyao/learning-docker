---
title: "Docker的stats命令"
linkTitle: "stats命令"
weight: 562
date: 2021-02-01
description: >
  Docker的stats命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/stats/

显示容器资源使用情况统计信息的实时流。

docker stats命令返回用于运行容器的实时数据流。要将数据限制为一个或多个特定容器，请指定由空格分隔的容器名称或ID列表。您可以指定已停止的容器，但已停止的容器不会返回任何数据。

如果您想了解有关容器资源使用情况的更多详细信息，请使用 /containers/(id)/stats API端点。

> 注意：在Linux上，Docker CLI通过从总内存使用量中减去页面缓存使用情况来报告内存使用情况。 API不执行此类计算，而是提供总内存使用量和页面缓存中的金额，以便客户端可以根据需要使用数据。

> 注意：PIDS列包含该容器创建的进程数和内核线程数。线程是Linux内核使用的术语。其他等效术语是“轻量级进程”或“内核任务”等.PIDS列中的大量数据与少量进程（由ps或top报告）可能表明容器中的某些内容正在创建许多线程。

### 使用

```bash
docker stats [OPTIONS] [CONTAINER...]
```

常用命令行参数

| 参数       | 描述                                             |
| ---------- | ------------------------------------------------ |
| --all , -a | Show all containers (default shows just running) |

### 示例

如果没有运行的容器，用下面命令启动一个：

```bash
docker run -it --rm --name statstest  ubuntu sh -c "while true; do $(echo date); sleep 1; done"
```

然后在另外一个终端中执行 `docker stats`：

```bash
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
0412e567f426        statstest           0.32%               1.801MiB / 15.12GiB   0.01%               2.85kB / 0B         0B / 0B             2
```

更多用法和格式化输出见官方文档。