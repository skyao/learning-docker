---
title: "安装后的设置"
linkTitle: "设置"
weight: 30
date: 2025-03-04
description: >
  Docker 安装后的通用设置
---


## 镜像加速

如果遇到镜像无法下载下来，比如报错：

```bash
$ docker run hello-world

Unable to find image 'hello-world:latest' locally
docker: Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```

就需要考虑配置镜像地址。

### 可用的镜像地址

为了加速拉取 Docker ，需要配置镜像地址：

- docker-cn：https://registry.docker-cn.com

- 网易镜像：http://hub-mirror.c.163.com

- 阿里云镜像: 

  打开地址 https://cr.console.aliyun.com/cn-hangzhou/mirrors 查看专属加速器地址

### 修改镜像地址

#### mac下修改

任务栏点击 Docker for mac 应用图标 -> Perferences... -> Daemon -> Registry mirrors。

#### linux下修改

通过修改 daemon 配置文件 `/etc/docker/daemon.json` 来使用加速器: 

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
