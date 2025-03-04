---
title: "Docker的logs命令"
linkTitle: "logs命令"
weight: 549
date: 2021-02-01
description: >
  Docker的logs命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/logs/

docker logs命令批量检索执行时存在的日志。

> 注意：此命令仅适用于使用 json-file 或 journald 日志记录驱动程序启动的容器。

`docker logs --follow` 命令将持续从容器的 STDOUT 和 STDERR 流式传输新输出。

将负数或非整数传递给 `--tail` 无效，并且在该情况下将值设置为all。

`docker logs --timestamps` 命令将为每个日志条目添加 RFC3339 Nano时间戳，例如 2014-09-16T06:17:46.000000000Z 。为了确保时间戳对齐，时间戳的纳秒部分将在必要时填充为零。

`docker logs --details` 命令将添加在创建容器时提供给 `--log-opt` 的额外属性，例如环境变量和标签。

`--since` 选项仅显示在给定日期之后生成的容器日志。您可以将日期指定为RFC 3339日期，UNIX时间戳或Go duration string（例如1m30s，3h）。除RFC3339日期格式外，您还可以使用RFC3339Nano， 2006-01-02T15:04:05, 2006-01-02T15:04:05.999999999, 2006-01-02Z07:00, and 2006-01-02。如果在时间戳结束时未提供Z或 +-00:00 时区偏移，则将使用客户端上的本地时区。提供Unix时间戳时输入秒[.nanoseconds]，其中秒是自1970年1月1日（午夜UTC / GMT）以来经过的秒数，不计算闰秒（也称为Unix纪元或Unix时间）和可选项。纳秒字段是一秒的一小部分，不超过九位数。您可以将--since选项与--follow或--tail选项中的任何一个或两者结合使用。

### 使用

```bash
docker logs [OPTIONS] CONTAINER
```

常用的命令行参数：

| 参数              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| `--details`       | Show extra details provided to logs                          |
| `--follow , -f`   | Follow log output                                            |
| `--since`         | Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes) |
| `--tail`          | Number of lines to show from the end of the logs             |
| --timestamps , -t | Show timestamps                                              |
| `--until`         | Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes) |


### 示例

先启动一个容器，这里会每秒钟打印一条日志：

```bash
docker run -it --rm --name logstest  ubuntu sh -c "while true; do $(echo date); sleep 1; done"
```

然后再在另外一个终端中运行：

```bash
docker logs -f logstest
```

就可以在第二个终端中看到输出。