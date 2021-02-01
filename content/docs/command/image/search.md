---
title: "Docker的search命令"
linkTitle: "search命令"
weight: 523
date: 2021-02-01
description: >
  Docker的search命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/search/

在Docker Hub中搜索镜像。

> 搜索查询默认最多返回25个结果。

### 使用

通过镜像名字搜索：

```bash
$ docker search busybox
NAME                        DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
busybox                     Busybox base image.                             1494                [OK]                
progrium/busybox                                                            68                                      [OK]
hypriot/rpi-busybox-httpd   Raspberry Pi compatible Docker Image with a …   45                                      
radial/busyboxplus          Full-chain, Internet enabled, busybox made f…   21                                      [OK]
......
```

### 过滤

当前支持的过滤器有：

- stars (int - number of stars the image has)
- is-automated (boolean - true or false) - is the image automated or not
- is-official (boolean - true or false) - is the image official or not

