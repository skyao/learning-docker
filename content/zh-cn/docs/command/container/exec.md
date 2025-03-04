---
title: "Docker的exec命令"
linkTitle: "exec命令"
weight: 546
date: 2021-02-01
description: >
  Docker的exec命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/exec/

在运行的容器内运行命令

docker exec 命令在正在运行的容器中运行新命令。

使用docker exec启动的命令仅在容器的主进程（PID 1）运行时运行，如果重新启动容器则不会重新启动。

COMMAND将在容器的默认目录中运行。 如果底层镜像在其Dockerfile中使用WORKDIR指令指定有自定义目录，则将使用此目录。

COMMAND应该是可执行文件，链接或带引号的命令不起作用。 例：

`docker exec -ti my_container "echo a && echo b"` 不会工作, 但是 `docker exec -ti my_container sh -c "echo a && echo b"` 可以.

### 使用

```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

常用的命令行参数：

| 参数               | 描述                                                 |
| ------------------ | ---------------------------------------------------- |
| --detach , -d      | Detached mode: run command in the background         |
| --env , -e         | Set environment variables                            |
| --interactive , -i | Keep STDIN open even if not attached                 |
| --privileged       | Give extended privileges to the command              |
| --tty , -t         | Allocate a pseudo-TTY                                |
| --user , -u        | Username or UID (format: <name\|uid>[:<group\|gid>]) |
| --workdir , -w     | Working directory inside the container               |

### 启动命令

先启动一个容器：

```bash
$ docker run -it --rm --name exectest  ubuntu bash
root@f13ee9356da6:/# 
```

用docker ps命令确认一下：

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f13ee9356da6        ubuntu              "bash"              24 seconds ago      Up 22 seconds                           exectest
```

运行简单命令：

```bash
$ docker exec exectest pwd
/
$ docker exec exectest ls /
bin
boot
dev
etc
......
```

如果想后台执行：

```bash
$ docker exec -d exectest pwd
```

也可以用进到容器里面，打开bash，再慢慢执行命令：

```bash
$ docker exec -it exectest bash
root@f13ee9356da6:/# pwd
/
```

这会在容器 exectest 里面创建一个新的bash session。

可以在进入容器时定义一些环境变量，通过 -e 参数传递进去：

```bash
$ docker exec -it -e var1=v1 exectest bash
root@f13ee9356da6:/# echo $var1
v1
```

注意这个环境变量只有在这个新的bash session里面才能看到，在原来打开这个容器的bash里面是无法看到的。

默认新的 bash session 的工作目录是在当前容器被创建时设置的工作目录，可以通过 -w 参数修改：

```bash
$ docker exec -it -w /root exectest bash
root@f13ee9356da6:~# pwd
/root
```



