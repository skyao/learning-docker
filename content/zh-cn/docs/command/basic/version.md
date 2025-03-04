---
title: "version命令"
linkTitle: "version命令"
weight: 513
date: 2021-02-01
description: >
  Docker的version命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/version/

默认情况下，这将以易于阅读的布局呈现所有版本信息。 如果指定了格式，则将执行给定的模板。

### 使用

```bash
$ docker version
Client:
 Version:           18.09.1
 API version:       1.39
 Go version:        go1.10.6
 Git commit:        4c52b90
 Built:             Wed Jan  9 19:35:23 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.1
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.6
  Git commit:       4c52b90
  Built:            Wed Jan  9 19:02:44 2019
  OS/Arch:          linux/amd64
  Experimental:     false
```

### 格式化输出

```bash
$ docker version --format '{{json .}}'
```

则输出格式(https://www.json.cn/ 转格式)为：

```json
{
    "Client":{
        "Platform":{
            "Name":""
        },
        "Version":"18.09.1",
        "ApiVersion":"1.39",
        "DefaultAPIVersion":"1.39",
        "GitCommit":"4c52b90",
        "GoVersion":"go1.10.6",
        "Os":"linux",
        "Arch":"amd64",
        "BuildTime":"Wed Jan 9 19:35:23 2019",
        "Experimental":false
    },
    "Server":{
        "Platform":{
            "Name":"Docker Engine - Community"
        },
        "Components":[
            {
                "Name":"Engine",
                "Version":"18.09.1",
                "Details":{
                    "ApiVersion":"1.39",
                    "Arch":"amd64",
                    "BuildTime":"Wed Jan 9 19:02:44 2019",
                    "Experimental":"false",
                    "GitCommit":"4c52b90",
                    "GoVersion":"go1.10.6",
                    "KernelVersion":"4.15.0-38-generic",
                    "MinAPIVersion":"1.12",
                    "Os":"linux"
                }
            }
        ],
        "Version":"18.09.1",
        "ApiVersion":"1.39",
        "MinAPIVersion":"1.12",
        "GitCommit":"4c52b90",
        "GoVersion":"go1.10.6",
        "Os":"linux",
        "Arch":"amd64",
        "KernelVersion":"4.15.0-38-generic",
        "BuildTime":"2019-01-09T19:02:44.000000000+00:00"
    }
}
```

可以单独获取其中某个数据，如:

```bash
docker version --format '{{.Server.Version}}'

18.09.1
```

