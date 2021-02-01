---
title: "Docker的images命令"
linkTitle: "images命令"
weight: 522
date: 2021-02-01
description: >
  Docker的images命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/images/

默认的docker images 命令将显示所有顶级镜像，其存储库和标签，以及它们的大小。

Docker image具有中间层，通过允许缓存每个步骤来提高可重用性，减少磁盘使用并加速docker build。 默认情况下不显示这些中间层。

SIZE是镜像及其所有父镜像占用的累积空间。 这也是Docker save命令制作镜像时创建的Tar文件内容使用的磁盘空间。

如果镜像具有多个存储库名称或标记，则会多次列出该镜像。 此单个镜像（可通过其匹配的IMAGE ID识别）仅使用一次列出的SIZE。

### 使用

```bash
$ docker images
REPOSITORY                                 TAG                                        IMAGE ID            CREATED             SIZE
k8s.gcr.io/kube-proxy                      v1.12.2                                    15e9da1ca195        3 months ago        96.5MB
k8s.gcr.io/kube-apiserver                  v1.12.2                                    51a9c329b7c5        3 months ago        194MB
k8s.gcr.io/kube-controller-manager         v1.12.2                                    15548c720a70        3 months ago        164MB
<none>                                     <none>                                     b8988e488a42        6 months ago        733MB
envoyproxy/envoy-build-ubuntu              7f7f5666c72e00ac7c1909b4fc9a2121d772c859   936ba3592a5c        8 months ago        2.31GB

```

### 通过name和tag来列出镜像

docker images 命令有可选的 `[REPOSITORY[:TAG]]` 参数，该参数将列表限制为与参数匹配的镜像。

如果指定 REPOSITORY 但没有 TAG，则 docker images 命令会列出给定存储库中的所有图像。

```bash
$ docker images istio/pilot
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
istio/pilot         0.7.1               6223c9364df7        10 months ago       51.9MB

$ docker images istio/*
REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
istio/istio-ca                           0.7.1               5c82fea42c78        10 months ago       42.7MB
istio/mixer                              0.7.1               251b0d3d2b93        10 months ago       54MB
6223c9364df7        10 months ago       51.9MB
istio/examples-bookinfo-reviews-v3       1.5.0               7301fee7c9cb        12 months ago       431MB
istio/examples-bookinfo-reviews-v2       1.5.0               

$ docker images istio/*:0.7.1
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
istio/istio-ca           0.7.1               5c82fea42c78        10 months ago       42.7MB
istio/mixer              0.7.1               251b0d3d2b93        10 months ago       54MB
istio/sidecar_injector   0.7.1               1db980725edb        10 months ago       33.4MB
```

### 过滤

过滤标志（`-f`或`--filter`）格式为"key=value"。 如果有多个过滤器，则传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）

目前支持的过滤器是：

- dangling悬空 (boolean - true or false)
- label (`label=<key>` or `label=<key>=<value>`)
- before (`<image-name>[:<tag>]`, `<image id>` or `<image@digest>`) - filter images created before given id or references
- since (`<image-name>[:<tag>]`, `<image id>` or `<image@digest>`) - filter images created since given id or references
- reference (pattern of an image reference) - filter images whose reference matches the specified pattern

具体看官方文档。

### dangling镜像的处理

dangling镜像就是images命令中出现的，REPOSITORY 和 TAG 都显示为 `<none>` 的镜像，如：

```bash
$ docker images
REPOSITORY                                 TAG                                        IMAGE ID            CREATED             SIZE
......
<none>                                     <none>                                     b8988e488a42        6 months ago        733MB
```

可以通过过滤器将dangling镜像都找出来：

```bash
$ docker images --filter "dangling=true"
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              b8988e488a42        6 months ago        733MB
```

这将显示未标记的镜像，这些镜像是镜像树的叶子（不是中间层）。 当镜像的新构建从这个镜像ID取走 `repo:tag` ，而留下 `<none>:<none>` 或 untagged 时，会出现这些的镜像。如果在容器当前正在使用镜像时尝试移除镜像，则会发出警告。通过使用此标志，允许批量清理。

可以联合`docker rmi `命令一起使用来批量删除dangling镜像：

```bash
docker rmi $(docker images -f "dangling=true" -q)
Deleted: sha256:b8988e488a42c652492c6b488f44469245d2088f793c1e6448777afcedb34ca1
```

