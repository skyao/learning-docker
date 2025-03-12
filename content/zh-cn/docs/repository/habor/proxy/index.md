---
title: "Harbor 代理仓库"
linkTitle: "代理仓库"
weight: 50
date: 2025-03-11
description: >
  使用 habor 作为代理仓库
---


## 新建仓库

"系统管理" -》 "仓库管理" 下，点"新建目标", 

![](images/new-repository.png)


## 新建项目

新建项目，指向刚才新建的仓库：

![](images/new-project.png)


## 使用代理仓库

使用新建的代理仓库 pull 镜像：

```bash
$ docker pull 192.168.3.221:5000/dockerhub/library/hello-world:latest
```

从日志可以看到，镜像是从代理仓库拉取的：

```bash
latest: Pulling from dockerhub/library/hello-world
Digest: sha256:bfbb0cc14f13f9ed1ae86abc2b9f11181dc50d779807ed3a3c5e55a6936dbdd5
Status: Downloaded newer image for 192.168.3.221:5000/dockerhub/library/hello-world:latest
192.168.3.221:5000/dockerhub/library/hello-world:latest
```

或者直接用 docker run 命令执行：

```bash
docker run 192.168.3.221:5000/dockerhub/library/hello-world:latest
```





