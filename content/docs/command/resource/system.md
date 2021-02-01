---
title: "Docker的system命令"
linkTitle: "system命令"
weight: 563
date: 2021-02-01
description: >
  Docker的system命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/system/

管理docker系统

### 使用

```bash
docker system COMMAND
```

### df 子命令

显示docker的磁盘使用

https://docs.docker.com/engine/reference/commandline/system_df/

```bash
docker system df
docker system df -v
```

### events 子命令

和docker events命令有何差异？看输出是一样。

### info 子命令

和docker info命令有何差异？看输出是一样。

### prune 子命令

删除不再使用的资源。

https://docs.docker.com/engine/reference/commandline/system_prune/

