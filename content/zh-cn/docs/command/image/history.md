---
title: "Docker的history命令"
linkTitle: "history命令"
weight: 525
date: 2021-02-01
description: >
  Docker的history命令
---

### 介绍

https://docs.docker.com/engine/reference/commandline/history/

显示镜像的历史，显示镜像每层的变更内容

### 使用

```bash
docker history [OPTIONS] IMAGE
```

如：

```bash
$ docker history istio/mixer:0.7.1 --no-trunc
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
251b0d3d2b93        10 months ago       /bin/sh -c #(nop)  CMD ["--configStoreURL=fs…   0B                  
<missing>           10 months ago       /bin/sh -c #(nop)  ENTRYPOINT ["/usr/local/b…   0B                  
<missing>           10 months ago       /bin/sh -c #(nop) ADD file:04fbcb926d6c80af5…   53.7MB              
<missing>           10 months ago       /bin/sh -c #(nop) ADD file:f74cafb0f2a94a0b9…   252kB  
```

