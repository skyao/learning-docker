---
title: "Docker安装后的设置"
linkTitle: "安装后的设置"
weight: 280
date: 2021-02-01
description: >
  Docker安装后的设置
---


## 镜像加速

为了加速拉取 Docker ，需要配置镜像地址：

- 网易的镜像地址：http://hub-mirror.c.163.com
- 或者用阿里云的镜像: 打开地址 https://cr.console.aliyun.com/cn-hangzhou/mirrors 查看专属加速器地址，一般是 https://****.mirror.aliyuncs.com

#### mac下修改

任务栏点击 Docker for mac 应用图标 -> Perferences... -> Daemon -> Registry mirrors。

#### linux下修改

针对Docker客户端版本大于 1.10.0 的用户

通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://****.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

参考文章：

- [Docker 国内镜像库加速](https://www.jianshu.com/p/1a4025c5f186)