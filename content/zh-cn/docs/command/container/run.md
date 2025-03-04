---
title: "Docker的run命令"
linkTitle: "run命令"
weight: 544
date: 2021-02-01
description: >
  Docker的run命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/run/

创建、启动容器并执行相应的命令。

在新的容器中运行命令。

docker run命令首先在指定的镜像上创建一个可写容器层，然后使用指定的命令启动它。 也就是说，docker run相当于API `/containers/create`  再 `/containers/(id)/start`。 可以使用 `docker start` 重新启动已停止的容器，并使其先前的所有更改保持不变。 请参阅 `docker ps -a` 以查看所有容器的列表。

`docker run` 命令可与 `docker commit` 结合使用，以更改容器运行的命令。

更多资料：

- [Docker run reference](https://docs.docker.com/engine/reference/run/)
-  [Docker network overview](https://docs.docker.com/engine/userguide/networking/)

### 使用

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

和 docker create 命令相同。

常用的命令行参数：

| 参数               | 描述                                                 |
| ------------------ | ---------------------------------------------------- |
| --attach , -a      | Attach to STDIN, STDOUT or STDERR                    |
| --env , -e         | Set environment variables                            |
| --env-file         | Read in a file of environment variables              |
| --expose           | Expose a port or a range of ports                    |
| `--hostname , -h`  | Container host name                                  |
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

### 分配名字和伪TTY (--name, -it)

```bash
$ docker run --name test -it debian
```

### 连接到STDIN/STDOUT/STDERR (-a)

`-a`标志告诉 `docker run` 绑定到容器的STDIN，STDOUT或STDERR。 这使得可以根据需要操纵输出和输入。

```bash
$ docker run -a stdout ubuntu echo test
test
```

### 完整容器能力 (--privileged)

特权容器？

默认情况下，大多数潜在危险的内核功能都会被删除; 包括 cap_sys_admin（挂载文件系统所需）。 但是，--privileged标志将允许它运行。

```bash
$ docker run -t -i --privileged ubuntu bash
```

`--privileged` 标志为容器提供了所有功能，它还解除了设备cgroup控制器强制执行的所有限制。 换句话说，容器几乎可以完成主机可以执行的所有操作。 此标志存在以允许特殊用例，例如在Docker中运行Docker。

### 设置工作目录 (-w)

```bash
$ docker  run -w /path/to/dir/ -i -t  ubuntu pwd
/path/to/dir
```

`-w` 允许命令在给定的目录中执行，这里是 `/path/to/dir/`。 如果路径不存在，则在容器内创建。

### 挂载卷 (-v, --read-only)

```bash
docker  run  -v `pwd`:`pwd` -w `pwd` -i -t  ubuntu pwd
```

`-v`标志将当前工作目录挂载到容器中。 `-w` 让命令在当前工作目录中执行，方法是将工作目录更改为pwd返回的值。所以这个组合使用容器执行命令，但在当前工作目录中。

### 发布或者暴露端口 (-p, --expose)

```bash
docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
```

这将容器的端口8080绑定到主机的127.0.0.1上的TCP端口80。还可以指定udp和sctp端口。 

```bash
docker run --expose 80 ubuntu bash
```

这会暴露容器的端口80，而不会将端口发布到主机系统接口。

### 设置环境变量(-e, --env, --env-file)

可以通过`-e, --env, --env-file` 设置容器的环境变量：

```bash
docker run -e MYVAR1 --env MYVAR2=foo --env-file ./env.list ubuntu bash
```

本地已经export的环境变量，可以不用=号和值：

```bash
export VAR1=value1
export VAR2=value2

$ docker run --env VAR1 --env VAR2 ubuntu env | grep VAR
VAR1=value1
VAR2=value2
```

### 在容器上设置元数据(-l, --label, --label-file)

可以通过`-l, --label, --label-file` 设置容器的label：

```bash
$ docker run -l my-label --label com.example.foo=bar ubuntu bash
```

### 将容器连接到网络(--network)

```bash
docker run -itd --network=my-net busybox
docker run -itd --network=my-net --ip=10.10.9.75 busybox
```

也可以使用 `docker connect` 命令

### 从容器挂载卷(--volumes-from)

```bash
docker run --volumes-from 777f7dc92da7 --volumes-from ba8c0c54f0f2:ro -i -t ubuntu pwd
```

