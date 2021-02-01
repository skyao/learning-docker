---
title: "Docker的tag命令"
linkTitle: "tag命令"
weight: 524
date: 2021-02-01
description: >
  Docker的tag命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/tag/

创建一个引用到 SOURCE_IMAGE 的 tag TARGET_IMAGE

镜像名称由斜杠分隔的名称组件组成，可选地以仓库主机名为前缀。主机名必须符合标准DNS规则，但可能不包含下划线。如果存在主机名，则可以选择后跟端口号如以下格式的 `:8080`。如果不存在，该命令默认使用位于 `registry-1.docker.io` 的Docker公共注册表。名称组件可能包含小写字母，数字和分隔符。分隔符定义为句点，一个或两个下划线，或一个或多个破折号。 名称组件不能以分隔符开头或结尾。

Tag名称必须是有效的ASCII，并且可以包含小写和大写字母，数字，下划线，句点和短划线。 Tag名称不能以句点或短划线开头，最多可包含128个字符。

### 使用

```bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

