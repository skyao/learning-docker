---
title: "Docker的create命令"
linkTitle: "create命令"
weight: 542
date: 2021-02-01
description: >
  Docker的create命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/create/

根据镜像生成一个新的容器

`docker create`命令在指定的映像上创建可写容器层，并准备运行指定的命令。 然后将容器ID打印到STDOUT。 这类似于`docker run -d`，但容器从未启动过。 然后，您可以使用 `docker start <container_id>` 命令在任何位置启动容器。

当您想要提前设置容器配置，以便在需要时启动容器已经准备好，这非常有用。 新容器的初始状态已经创建。

### 使用

```bash
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
```

常用的命令行参数：

| 参数               | 描述                                                 |
| ------------------ | ---------------------------------------------------- |
| --attach , -a      | Attach to STDIN, STDOUT or STDERR                    |
| --env , -e         | Set environment variables                            |
| --env-file         | Read in a file of environment variables              |
| --expose           | Expose a port or a range of ports                    |
| --interactive , -i | Keep STDIN open even if not attached                 |
| --label , -l       | Set meta data on a container                         |
| --label-file       | Read in a line delimited file of labels              |
| --mount            | Attach a filesystem mount to the container           |
| --name             | Assign a name to the container                       |
| --publish , -p     | Publish a container’s port(s) to the host            |
| --read-only        | Mount the container’s root filesystem as read only   |
| --rm               | Automatically remove the container when it exits     |
| --tty , -t         | Allocate a pseudo-TTY                                |
| --user , -u        | Username or UID (format: <name\|uid>[:<group\|gid>]) |
| --volume , -v      | Bind mount a volume                                  |
| --volumes-from     | Mount volumes from the specified container(s)        |
| --workdir , -w     | Working directory inside the container               |
|                    |                                                      |
|                    |                                                      |
|                    |                                                      |
|                    |                                                      |

