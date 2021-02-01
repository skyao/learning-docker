---
title: "Docker的启停命令"
linkTitle: "启停命令"
weight: 543
date: 2021-02-01
description: >
  Docker的启停命令，包括start / stop / kill / restart / pause / unparse
---

## start命令

https://docs.docker.com/engine/reference/commandline/start/

启动一个或者多个停止的容器

```bash
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

命令行参数：

| 参数               | 描述                                                 |
| ------------------ | ---------------------------------------------------- |
| --attach , -a      | Attach to STDIN, STDOUT or STDERR                    |
| --interactive , -i | Keep STDIN open even if not attached                 |

```
date: 2019-01-27T07:30:00+08:00
title: start命令
weight: 342
menu:
  main:
    parent: "command-container"
description : "Docker的start命令"
```

## stop命令

https://docs.docker.com/engine/reference/commandline/stop

停止一个或者多个运行的容器

容器内的主进程将先接收到 SIGTERM 信号，并在优雅关闭限期后，接收到 SIGKILL 信号。

```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

命令行参数：

| 参数        | 描述                                       |
| ----------- | ------------------------------------------ |
| --time , -t | Seconds to wait for stop before killing it |

## kill命令

https://docs.docker.com/engine/reference/commandline/kill

杀死一个或者多个运行的容器

容器内的主进程将先接收到 SIGTERM 信号，并在优雅关闭限期后，接收到 SIGKILL 信号。

```bash
docker kill [OPTIONS] CONTAINER [CONTAINER...]
```

命令行参数：

| 参数          | 描述                            |
| ------------- | ------------------------------- |
| --signal , -s | Signal to send to the container |

## restart命令

https://docs.docker.com/engine/reference/commandline/restart

重新启动一个或者多个运行的容器

```bash
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

命令行参数：

| 参数        | 描述                                                  |
| ----------- | ----------------------------------------------------- |
| --time , -t | Seconds to wait for stop before killing the container |

## pause命令

https://docs.docker.com/engine/reference/commandline/pause

暂停一个或多个容器中的所有进程

docker pause命令挂起(suspend)指定容器中的所有进程。 在Linux上，它使用cgroups freezer。 传统上，当暂停进程时，使用SIGSTOP信号，该进程被暂停可观察到。 使用cgroups freezer，该过程无感知，无法捕获它被暂停和随后的恢复。 在Windows上，只有Hyper-V容器可以被暂停。

```bash
docker pause CONTAINER [CONTAINER...]
```

### unpause命令

恢复一个或多个容器中的所有暂停进程

```bash
docker unpause CONTAINER [CONTAINER...]
```

