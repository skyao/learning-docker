---
title: "inspect命令"
linkTitle: "inspect命令"
weight: 514
date: 2021-02-01
description: >
  Docker的inspect命令
---


### 介绍

https://docs.docker.com/engine/reference/commandline/inspect/

返回有关Docker对象的低层信息。

Docker inspect 命令提供被其控制的各种结构的详细信息。

### 使用

```bash
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```

需要给出目标对象的name或者id。

如果要查看镜像，可以先用 `docker images` 命令看到本地所有镜像

```bash
docker images
REPOSITORY                                 TAG                                        IMAGE ID            CREATED             SIZE
k8s.gcr.io/kube-proxy                      v1.12.2                                    15e9da1ca195        3 months ago        96.5MB
k8s.gcr.io/kube-apiserver                  v1.12.2                                    51a9c329b7c5        3 months ago        194MB

```

然后通过ID 来使用 inspect 命令：

```bash
docker inspect 15e9da1ca195
```

默认是返回完整信息，json格式：

```json
[
    {
        "Id": "sha256:15e9da1ca195086627363bb2e8f5bfca7048345325287d307afd317bab2045b0",
        "RepoTags": [
            "k8s.gcr.io/kube-proxy:v1.12.2"
        ],
        "RepoDigests": [
            "k8s.gcr.io/kube-proxy@sha256:b77f615c0a914efbf4c66b37011b0f87e28498af6cc71590ecbd67b6be005a60"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2018-10-24T07:43:58.286868924Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ADD file:5e8feab95fcebf67c56cc114f6ec97cfb09bcfedacb175bb1951cb5d6e385574 in /usr/local/bin/kube-proxy "
            ],
            "ArgsEscaped": true,
            "Image": "sha256:e1f02e54b200d3a4f3b46b6f0ec4995d92c33707a3c2edf6fb5162c7c406fab5",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:e1f02e54b200d3a4f3b46b6f0ec4995d92c33707a3c2edf6fb5162c7c406fab5",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 96469663,
        "VirtualSize": 96469663,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/e35f184dc38f9107e8bdaafd718e61a2e4251879d81a33ed59a884ced01b7b8e/diff:/var/lib/docker/overlay2/b1237288693892b66dcaee6b47625303b7814844a93c71bd2fd5d7bb49fbefaa/diff",
                "MergedDir": "/var/lib/docker/overlay2/229d3a5fb18ee157fc6b3b3a8d11d3adfb5b6cefa9d81a5e9332b345816de10c/merged",
                "UpperDir": "/var/lib/docker/overlay2/229d3a5fb18ee157fc6b3b3a8d11d3adfb5b6cefa9d81a5e9332b345816de10c/diff",
                "WorkDir": "/var/lib/docker/overlay2/229d3a5fb18ee157fc6b3b3a8d11d3adfb5b6cefa9d81a5e9332b345816de10c/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:0c1604b64aedf4f5060a25da1b96b21aa803a878ce475633d02a4aba854252a7",
                "sha256:dc6f419d40a27032570a3bd2813f10abd09620e68974f3d74e09ecf6a3a4113c",
                "sha256:2d9b7a4a23dd71483150c1b9bfb136e2a1196897efb8191e4aafdc78eb7bcb4e"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

### 格式化输出

同样可以通过格式化输出获取特定的属性值。

```bash
$ docker inspect --format='{{.Id}}'  15e9da1ca195
sha256:15e9da1ca195086627363bb2e8f5bfca7048345325287d307afd317bab2045b0
$ docker inspect --format='{{.RootFS.Type}}'  15e9da1ca195
layers
```

更多案例参看官方文档:  https://docs.docker.com/engine/reference/commandline/inspect/