---
title: "Ubuntu下安装Docker"
linkTitle: "Ubuntu"
weight: 20
date: 2021-12-25
description: >
  Docker在Ubuntu20.04/22.04/23.04下的安装
---

参考官网文档：

https://docs.docker.com/install/linux/docker-ce/ubuntu/

适用于以下版本：

- Ubuntu Lunar 23.04
- Ubuntu Kinetic 22.10
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)

## 准备工作

清除老版本：

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

安装需要的工具：

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```

添加 GPG key：

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

准备用于 apt 的仓库：

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

此时打开 `/etc/apt/sources.list.d/docker.list` 文件：

```bash
sudo vi /etc/apt/sources.list.d/docker.list
```

就能看到如下内容：

```bash
deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu   jammy stable
```

其中 `jammy` 是ubuntu版本代码名称 VERSION_CODENAME ，其内容来自文件 `/etc/os-release`:

```bash
PRETTY_NAME="Ubuntu 22.04.2 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.2 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

{{% alert title="注意" color="warning" %}}
对于 Linux Mint 这样的基于 ubuntu 的发行版本，VERSION_CODENAME 会不一样。

因此需要在安装 docker 时，需要修订这个值为对应 ubuntu 的值，或者叫做 UBUNTU_CODENAME

ubuntu各个版本的codename 请见：https://download.docker.com/linux/ubuntu/dists/

{{% /alert %}}

执行 apt update 之后：

```bash
sudo apt-get update
```

就可以安装了。

## 安装

一般 **不推荐** 安装最新版本，尤其是和 k8s 一起使用时：

~~sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin~~

可以先执行

```bash
apt-cache madison docker-ce
```

查看当前可选版本：

```
 docker-ce | 5:24.0.2-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:24.0.1-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:24.0.0-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.6-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.5-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.4-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.3-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.2-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.1-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:23.0.0-1~ubuntu.22.04~jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.24~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.23~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.22~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.21~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.20~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.19~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.18~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.17~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
```

这里先固定使用 20.10.21 版本， 执行安装命令：

```bash
VERSION_STRING=5:20.10.21~3-0~ubuntu-jammy
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

安装完成后检验一下：

```bash
sudo docker run hello-world
```

查看安装的版本：

```bash
$ docker --version
Docker version 20.10.21, build baeda1f
```

## 设置

### 设置权限避免每次sudo

为了以非 root 用户使用 docker, 可以将用户加入 "docker" 组.

```bash
sudo usermod -aG docker sky
```

重新登录之后生效。验证一下：

```bash
docker run hello-world
```

### 取消 apt source 更新

docker 安装之后，没有必要时刻保持更新，因此可以取消 apt source的设置，加速 apt update 的执行：

```bash
sudo vi /etc/apt/sources.list.d/docker.list
```

将内容注释即可。

