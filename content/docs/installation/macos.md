---
title: "Macos下安装Docker"
linkTitle: "Macos"
weight: 220
date: 2021-05-09
description: >
  Docker在MacOS下的安装
---


以 MacOS Big Sur 为例，版本 11.3.1，参考官网文档：

https://docs.docker.com/docker-for-mac/install/

### 版本选择

新版本的MacOs 推荐使用 `Docker for Mac`，要是Apple Mac OS Yosemite 10.10.3 或者更高. 对于早期版本的macos建议使用 [Docker Toolbox](https://docs.docker.com/toolbox/overview/) 。

## 手工安装

我选择按照 Docker CE for Mac，官方介绍见：

https://store.docker.com/editions/community/docker-ce-desktop-mac

### 下载

下载要求有 docker id，登录之后才能下载。(新版本中似乎没有登录要求了)

https://download.docker.com/mac/stable/Docker.dmg

### 安装步骤

1. 双击 docker.dmg 文件，然后再弹出窗口中拖动到 application 中
2. 打开 Launchpad，找到 docker ，点击
3. 弹出窗口要求登录，用 docker id 登录，然后授权安装
4. 安装完毕之后，提示 docker desktop 已经安装完成并在运行，可以使用任何喜欢的终端开始

验证一下安装，看一下版本信息：

```bash
% docker version
Client: Docker Engine - Community
 Cloud integration: 1.0.12
 Version:           20.10.5
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        55c4c88
 Built:             Tue Mar  2 20:13:00 2021
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.5
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       363e9a8
  Built:            Tue Mar  2 20:15:47 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

验证一下：

```bash
  sudo docker run hello-world
```

输出如下：

```bash
% sudo docker run hello-world
Password:
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:f2266cbfc127c960fd30e76b7c792dc23b588c0db76233517e1891a4e357d519
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

备注：macos下运行 docker，似乎没有 linux 下 root 权限的问题。

## brew自动安装

目前brew已经支持docker安装，只要简单执行命令即可：

```bach
brew cask install docker
```

参考文章：

- [MacOS Docker 安装](http://www.runoob.com/docker/macos-docker-install.html)
- [macOS 安装 Docker](https://yeasy.gitbooks.io/docker_practice/install/mac.html)

## ~~特殊：AMD黑苹果~~

> 备注：为了黑苹果更完美，amd 3900x换成了intel 7960x，下面内容仅仅作为留档。

有台台式机，用的 amd 锐龙2 cpu，安装了黑苹果。

由于 amd cpu不支持intel的虚拟化技术，而 macos 目前又只支持 intel 虚拟技术来运行docker。因此 docker 安装完成之后就会报错：不兼容。

解决方案就是用 docker-machine + virtualbox，相当于在 virtualbox 中运行一个linux 虚拟机，然后配置 docker client 参数连接到这个虚拟机中去执行 docker 命令。

安装方式：

1. 安装  Docker Toolbox（包括 docker CLI，docker component，）：

   - 参考页面：https://docs.docker.com/toolbox/toolbox_install_mac/
   - 下载页面：https://github.com/docker/toolbox/releases （版本有点老，凑合用）

2. 安装 virtualbox

   - 下载页面：https://www.virtualbox.org/wiki/Downloads
   - 安装 VirtualBox  platform packages 和 VM VirtualBox Extension Pack

3. 用 docker-machine 安装

   - 参考页面：https://github.com/docker/machine

   - 执行命令:

     ```bash
     $ docker-machine create -d virtualbox  --virtualbox-no-vtx-check default
     $ eval "$(docker-machine env default)"
     $ docker run busybox echo hello world
     ```
   之后在 virtualbox中就可以看到这个虚拟机了，配置非常低，所以可以修改一下，增加cpu和内存。

4. 添加快捷命令

   为了方便设置 docker，修改 `/etc/profile`：

   ```
   alias docker-default='docker-machine start default; eval "$(docker-machine env default)"'
   ```

   这样以后每次只要执行 docker-default 命令就可以设置好 docker。

5. 安装 docker.app ，虽然不能用，但是有很多文件是需要的

6. 执行命令:

   ```
   ln -s "/Applications/Docker.app/Contents//Resources/bin/docker-credential-desktop" "/usr/local/bin/docker-credential-desktop"
   ln -s "/Applications/Docker.app/Contents//Resources/bin/docker-credential-osxkeychain" "/usr/local/bin/docker-credential-osxkeychain"
   ```

   