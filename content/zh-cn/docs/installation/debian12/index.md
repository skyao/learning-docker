---
title: "debian 12 下安装 Docker"
linkTitle: "debian12"
weight: 20
date: 2025-03-04
description: >
  Docker 在 debian12 下的安装
---

## 安装docker

参考 docker 官网的安装文档：

https://docs.docker.com/engine/install/debian/

### 卸载旧版本

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

### 用 apt 安装

添加 docker 的 apt 仓库：

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

如果要安装最新版本，可以直接：

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

也可以选择要安装版本。查看可选的 docker 版本：

```bash
apt-cache madison docker-ce | awk '{ print $3 }'
```

列出的版本有：

```bash
$ apt-cache madison docker-ce | awk '{ print $3 }'
5:28.0.1-1~debian.12~bookworm
5:28.0.0-1~debian.12~bookworm
5:27.5.1-1~debian.12~bookworm
5:27.5.0-1~debian.12~bookworm
5:27.4.1-1~debian.12~bookworm
......
 ```

这里可以选择使用一个版本，我直接选了最新的 28.0.1 版本， 执行安装命令：

```bash
VERSION_STRING=5:28.0.1-1~debian.12~bookworm

sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

安装完成后检验一下：

```bash
sudo docker run hello-world
```

查看安装的版本：

```bash
$ docker --version
Docker version 28.0.1, build 068a01e
```

## 安装后设置

参考 docker 官网的设置文档：

https://docs.docker.com/engine/install/linux-postinstall/

### 设置权限避免每次sudo

为了以非 root 用户使用 docker, 可以将用户加入 "docker" 组.

```bash
# sudo groupadd docker
sudo usermod -aG docker $USER
```

重新登录之后生效。或者用下面的命令直接生效：

```bash
newgrp docker
```

验证一下：

```bash
docker run hello-world
```

### 设置 docker 为系统服务

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

### 取消 apt source 更新

docker 安装之后，没有必要时刻保持更新，因此可以取消 apt source的设置，加速 apt update 的执行：

```bash
sudo vi /etc/apt/sources.list.d/docker.list
```


## 参考资料

- https://linuxiac.com/how-to-install-docker-on-debian-12-bookworm/
- https://www.linuxcloudvps.com/blog/how-to-install-docker-on-debian-12/