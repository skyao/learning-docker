---
title: "Docker Compose概述"
linkTitle: "概述"
weight: 2001
date: 2021-02-01
description: >
  Docker Compose概述
---


Docker Compose 是一个用来定义和运行复杂应用的Docker工具。

使用Compose，你可以在一个文件中定义一个多容器应用，然后使用一条命令来启动你的应用，完成一切准备工作。

Docker Compose 的文档：

https://docs.docker.com/compose/overview/

### 安装

安装方式参考官方文档：

https://docs.docker.com/compose/install/

建议还是直接从github发布地址手工下载最新版本的 `docker-compose-Linux-x86_64`：

https://github.com/docker/compose/releases

然后手工复制到目标路径，增加运行权限：

```bash
sudo mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

验证安装：

```bash
docker-compose version
docker-compose version 1.23.1, build b02f1306
docker-py version: 3.5.0
CPython version: 3.6.7
OpenSSL version: OpenSSL 1.1.0f  25 May 2017
```

### 错误处理

如果遇到执行`docker-compose up`命令时报错：

```bash
$ docker-compose up --build -d
ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?

If it's at a non-standard location, specify the URL with the DOCKER_HOST environment variable.
```

请检查是否有将当前用户加入docker组，如果已经有加入，则可能是加入后还没有重新登录。建议退出当前用户再重新登录，或者重启，如果不想退出当前用户，可以用一个取巧的方案：

```bash
sudo su  # 先su到root用户
su sky   # 再su当当前用户
docker-compose up -d    # 再执行命令
```



### 参考资料

- https://henryz.gitbooks.io/learing-docker-compose/