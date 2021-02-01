---
title: "Docker的attach命令"
linkTitle: "attach命令"
weight: 548
date: 2021-02-01
description: >
  Docker的attach命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/attach/

将本地标准 input，output 和 error 流附加到正在运行的容器。

使用 docker attach 将终端的标准 input，output 和 error （或三者的任意组合）通过容器的ID或名称附加到正在运行的容器中。这允许您查看其正在进行的输出或以交互方式控制它，就像命令直接在您的终端中运行一样。

> 注意：attach命令将显示 ENTRYPOINT/CMD 进程的输出。这可能看起来好像挂起命令被挂起，而实际上该进程实际上可能根本就没有与终端进行交互。

您可以从Docker主机上的不同会话同时多次附加到同一个容器进程。

要停止容器，请使用CTRL-c。 此键序列将SIGKILL发送到容器。 如果--sig-proxy为true（默认值），则CTRL-c将SIGINT发送到容器。 您可以使用CTRL-p CTRL-q键序列从容器中分离并使其保持运行。

> 注意：在容器内作为PID 1运行的进程由Linux专门处理：它忽略具有默认操作的任何信号。 因此，除非进行编码，否则进程不会在SIGINT或SIGTERM上终止。

在附加到启用tty的容器（即：使用-t启动）时，禁止重定向docker attach命令的标准输入。

当客户端使用docker attach连接到容器的stdio时，Docker使用~1MB内存缓冲区来最大化应用程序的吞吐量。 如果填充此缓冲区，API连接的速度将开始影响过程输出写入速度。 这类似于SSH等其他应用程序。 因此，建议不要运行性能关键型应用程序，这些应用程序通过慢速客户端连接在前台生成大量输出。 相反，用户应使用docker logs命令来访问日志。

### 使用

```bash
docker attach [OPTIONS] CONTAINER
```

常用的命令行参数：

| 参数        | 描述                                      |
| ----------- | ----------------------------------------- |
| --no-stdin  | Do not attach STDIN                       |
| --sig-proxy | Proxy all received signals to the process |


### 启动命令

先启动一个容器：

```bash
$ docker run -it --rm --name attachtest  ubuntu bash
root@f13ee9356da6:/# 
```

然后再在另外一个终端中运行：

```bash
docker attach attachtest
```

之后在两个终端中输入命令，都可以看到同样的输出。

备注：当第二个终端用 exit 命令退出时，第一个终端（也就是启动容器的终端）也会同样因为stdin里面输入了exit 命令而退出，容器关闭。

如果不想影响原有容器的工作，可以用 --no-stdin 参数：

```bash
docker attach  --no-stdin attachtest
```

这样不附加 stdin ，也就不会有输入影响容器的运行，这个终端需要推出时，ctrl + c 即可。