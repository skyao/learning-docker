---
title: "Docker容器网络"
linkTitle: "Docker容器网络"
weight: 910
date: 2021-02-01
description: >
  Docker容器网络
---


> 摘要自：  https://docs.docker-cn.com/engine/userguide/networking/

### 默认网络

安装docker时自动创建的网络，可以通过 `docker network ls` 命令查看：

```bash
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
19b2dd70e44d        bridge              bridge              local
68b80f077f8b        host                host                local
0f94ce7160cf        none                null                local
```

运行容器时，可以通过 `--network` 标记指定容器要连接到的网络。

桥接网络代表所有Docker安装中存在的docker0网络。 除非您使用docker run --network = <NETWORK>选项另行指定，否则Docker守护程序默认情况下将容器连接到此网络。 您可以通过在主机上使用ifconfig命令将此桥接器视为主机网络堆栈的一部分。

