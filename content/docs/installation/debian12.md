---
title: "debian12下安装Docker"
linkTitle: "debian12"
weight: 30
date: 2024-02-05
description: >
  Docker在debian12下的安装
---

## 安装docker

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] https://download.docker.com/linux/debian bookworm stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
```

如果要安装最新版本，可以直接：

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin
```

安全一点，可以选择安装不那么新的版本。查看可选的 docker 版本：

```bash
apt-cache madison docker-ce
```

列出的选择有：

```bash
$ apt-cache madison docker-ce
 docker-ce | 5:25.0.2-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:25.0.1-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:25.0.0-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.9-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.8-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.7-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.6-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.5-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.4-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.3-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.2-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.1-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:24.0.0-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.6-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.5-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.4-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.3-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.2-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.1-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 docker-ce | 5:23.0.0-1~debian.12~bookworm | https://download.docker.com/linux/debian bookworm/stable amd64 Packages
 ```

 这里先选择使用 24.08 版本， 执行安装命令：

```bash
export VERSION_STRING=5:24.0.8-1~debian.12~bookworm

sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin -y
```

安装完成后检验一下：

```bash
sudo docker run hello-world
```

查看安装的版本：

```bash
$ docker --version
Docker version 24.0.8, build e0dfb46
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


## 参考资料

- https://linuxiac.com/how-to-install-docker-on-debian-12-bookworm/
- https://www.linuxcloudvps.com/blog/how-to-install-docker-on-debian-12/