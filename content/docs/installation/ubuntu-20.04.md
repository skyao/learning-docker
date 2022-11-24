---
title: "Ubuntu20.04下安装Docker"
linkTitle: "Ubuntu20.04"
weight: 211
date: 2021-12-25
description: >
  Docker在Ubuntu20.04下的安装
---

以 ubuntu 20.04（也包括作为桌面使用的 Linux Mint 21） 为例，参考官网文档：

https://docs.docker.com/install/linux/docker-ce/ubuntu/

## 安装准备

看一下自己的Linux机器版本和内核信息：

- `uname --all`

	```bash
	Linux skywork 5.15.0-53-generic #59-Ubuntu SMP Mon Oct 17 18:53:30 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
	```

- `cat /proc/version`

  ```bash
  Linux version 5.15.0-53-generic (buildd@lcy02-amd64-047) (gcc (Ubuntu 11.2.0-19ubuntu1) 11.2.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #59-Ubuntu SMP Mon Oct 17 18:53:30 UTC 2022
  ```

- `lsb_release -a`

	```bash
  No LSB modules are available.
  Distributor ID:	Linuxmint
  Description:	Linux Mint 21
  Release:	21
  Codename:	vanessa
	```

这台机器是Linux Mint 21 升级到非常新的Linux内核5.15.0-53了，Linux Mint 21 是基于 Ubuntu 20.04，但是有些系统参数不一样，比如这里 Codename 是 `vanessa`，而不是 Ubuntu 20.04 的 `Focal Fossa`。后面安装时要小心，需要修改为 Ubuntu 20.04 的参数。

> ubuntu各个版本的codename 请见：https://download.docker.com/linux/ubuntu/dists/

## 安装步骤

### 卸载旧版本

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### 安装docker

简单起见，使用 docker 仓库安装。

```bash
$ sudo apt-get install ca-certificates curl gnupg lsb-release

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 官方给的是这样，
# echo \
#  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
# https://download.docker.com/linux/ubuntu \
#  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 但是`lsb_release -cs`在Linux Mint下取值会不对，因此手工修改
# for ubuntu 20.04 / linuxmint 20.02，$(lsb_release -cs)替代为 focal
$ echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu focal stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

如果不想安装最新版本，则可以先查看当前可选版本：

```bash
$ apt-cache madison docker-ce
docker-ce | 5:20.10.21~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.20~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.19~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.18~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.17~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.16~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.8~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.7~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.2~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.1~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.0~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```

然后执行 :

```bash
sudo apt-get install docker-ce=20.10.21~3-0~ubuntu
```

### 验证安装

安装完成之后，看一下版本信息：

```bash
$ docker --version
Docker version 20.10.21, build baeda1f
```

验证一下：

```bash
sudo docker run hello-world
```

输出如下：

```bash
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally

latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:faa03e786c97f07ef34423fccceeec2398ec8a5759259f94d99078f264e9d7af
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### 设置权限避免每次sudo

为了以非root用户使用docker, 可以将用户加入"docker"组.

```bash
sudo usermod -aG docker sky
```

重新登录之后生效。

### 设置docker的cgroup driver

> 备注： 发现 docker 20.10.21~3-0~ubuntu 版本已经修改为默认 systemd ，因此新版本可以不用这么设置。

docker 默认的 cgroup driver 是 cgroupfs，可以通过 docker info 命令查看：

```bash
$ docker info | grep "Cgroup Driver"
Cgroup Driver: cgroupfs
```

而 kubernetes 在v1.22版本之后，如果用户没有在 KubeletConfiguration 下设置 cgroupDriver 字段，则 kubeadm 将默认为 `systemd`。

为了保持一致，需要修改 docker 的 cgroup driver 为 `systemd`, 方式为打开 docker 的配置文件（如果不存在则创建）

```bash
sudo vi /etc/docker/daemon.json
```

增加内容：

```json
{
	"exec-opts": ["native.cgroupdriver=systemd"]
}
```

修改完成后重启 docker：

```bash
sudo systemctl restart docker

# 重启后检查一下
docker info | grep "Cgroup Driver"
```

### 设置swap limit

如果遇到如下的 "No swap limit support" 警告：

```bash
docker info | grep "Cgroup Driver"
 Cgroup Driver: systemd
WARNING: No swap limit support
```

则说明 swap limit 功能没能开启。解决方法是 grub 

```bash
$ sudo vi /etc/default/grub
......
GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
......
```

{{% alert title="注意" color="warning" %}}
如果 GRUB_CMDLINE_LINUX= 内有内容，切记不可删除，只需在后面追加，并用空格和前面的内容分隔开。
{{% /alert %}}

执行下面命令生效并重启。

```bash
$ sudo update-grub2
$ sudo reboot
```

参考资料：

- https://www.k2zone.cn/?p=2356
- https://unix.stackexchange.com/questions/342735/docker-warning-no-swap-limit-support

