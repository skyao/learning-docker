---
title: "在线安装 Docker"
linkTitle: "在线安装"
weight: 20
date: 2025-03-04
description: >
  在 debian13 上在线安装 docker
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
```

如果想使用 neuxs 的 apt 仓库来进行加速，可以修改 apt 源为本地 nexus 的代理源。

参考： http://skyao.net/learning-debian/docs/develop/tools/nexus/#apt-%E4%BB%93%E5%BA%93

然后更新 apt 源：

```bash
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
5:29.1.1-1~debian.13~trixie
5:29.1.0-1~debian.13~trixie
5:29.0.4-1~debian.13~trixie
5:29.0.3-1~debian.13~trixie
5:29.0.2-1~debian.13~trixie
5:29.0.1-1~debian.13~trixie
5:29.0.0-1~debian.13~trixie
5:28.5.2-1~debian.13~trixie
5:28.5.1-1~debian.13~trixie
5:28.5.0-1~debian.13~trixie
......
 ```

这里可以选择使用一个版本，我直接选了最新的 5:29.1.1-1 版本， 执行安装命令：

```bash
VERSION_STRING=5:29.1.1-1~debian.13~trixie

sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

安装完成后检验一下：

```bash
sudo docker run hello-world
```

查看安装的版本：

```bash
$ docker --version
Docker version 29.1.1, build 0aedba5
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

## 安装 docker compose

可以选择用 apt 安装：

```bash
sudo apt-get install docker-compose
```

但安装出来的不是最新版本：

```bash
$ docker-compose --version
docker-compose version 1.29.2, build 5becea4c
```

要安装最新的版本，可以手工下载：

https://github.com/docker/compose/releases

可以远程下载最新的版本： 

```bash
wget https://github.com/docker/compose/releases/download/v2.40.3/docker-compose-linux-x86_64
```

也可以从开发服务器上下载：

```bash
wget http://192.168.3.193/downloads/docker-compose/docker-compose-linux-x86_64
```

安装：

```bash
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

验证：

```bash
$  docker-compose --version
Docker Compose version v2.40.3
```

## 参考资料

- https://linuxiac.com/how-to-install-docker-on-debian-12-bookworm/
- https://www.linuxcloudvps.com/blog/how-to-install-docker-on-debian-12/