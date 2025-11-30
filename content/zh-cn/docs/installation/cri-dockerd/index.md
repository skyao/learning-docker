---
title: "安装 cri-dockerd"
linkTitle: "安装 cri-dockerd"
weight: 40
date: 2025-11-30
description: >
  cri-dockerd 安装
---


## 安装 cri-dockerd

cri-dockerd 的安装参考：

https://mirantis.github.io/cri-dockerd/usage/install/

从 release 页面下载：

https://github.com/Mirantis/cri-dockerd/releases

debian 13 选择下载文件并安装：

```bash
mkdir -p ~/temp/
cd ~/temp/
# 从官方下载
# wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.4.0/cri-dockerd_0.4.0.3-0.debian-bookworm_amd64.deb
# 可以使用 devserver 上准备好的文件
wget http://192.168.3.193/downloads/cri-dockerd/cri-dockerd_0.4.0.3-0.debian-bookworm_amd64.deb
sudo dpkg -i ./cri-dockerd_0.4.0.3-0.debian-bookworm_amd64.deb
```

安装后会提示：

```bash
Selecting previously unselected package cri-dockerd.
(Reading database ... 68005 files and directories currently installed.)
Preparing to unpack .../cri-dockerd_0.4.0.3-0.debian-bookworm_amd64.deb ...
Unpacking cri-dockerd (0.4.0~3-0~debian-bookworm) ...
Setting up cri-dockerd (0.4.0~3-0~debian-bookworm) ...
Created symlink /etc/systemd/system/multi-user.target.wants/cri-docker.service → /lib/systemd/system/cri-docker.service.
Created symlink /etc/systemd/system/sockets.target.wants/cri-docker.socket → /lib/systemd/system/cri-docker.socket.
```

安装后查看状态：

```bash
sudo systemctl status cri-docker.service
```

如果成功则状态为：

```bash
● cri-docker.service - CRI Interface for Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/cri-docker.service; enabled; preset: enabled)
     Active: active (running) since Sat 2025-05-10 20:39:41 CST; 19s ago
TriggeredBy: ● cri-docker.socket
       Docs: https://docs.mirantis.com
   Main PID: 8294 (cri-dockerd)
      Tasks: 9
     Memory: 13.1M
        CPU: 18ms
     CGroup: /system.slice/cri-docker.service
             └─8294 /usr/bin/cri-dockerd --container-runtime-endpoint fd://

May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Hairpin mode is set to none"
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="The binary conntrack is not installed, this can cau>
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="The binary conntrack is not installed, this can cau>
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Loaded network plugin cni"
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Docker cri networking managed by network plugin cni"
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Setting cgroupDriver systemd"
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Docker cri received runtime config &RuntimeConfig{N>
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Starting the GRPC backend for the Docker CRI interf>
May 10 20:39:41 debian12 cri-dockerd[8294]: time="2025-05-10T20:39:41+08:00" level=info msg="Start cri-dockerd grpc backend"
May 10 20:39:41 debian12 systemd[1]: Started cri-docker.service - CRI Interface for Docker Application Container Engine.
```


## pause 镜像的版本问题

后面执行 init 命令时遇到警告: 

```bash
W1127 11:14:12.643662   17241 checks.go:827] detected that the sandbox image "registry.k8s.io/pause:3.10" of the container runtime is inconsistent with that used by kubeadm. It is recommended to use "192.168.3.193:5000/k8s-proxy/pause:3.10.1" as the CRI sandbox image.
```

这个警告的意思是：容器运行时（CRI）默认使用的 sandbox 镜像版本和 kubeadm 期望的不一致。

Kubernetes 在运行 Pod 时，会用一个特殊的“pause”容器作为 Pod 的基础（sandbox）. 目前我安装的 CRI（cri-dockerd 或 containerd）默认拉的是：

```bash
registry.k8s.io/pause:3.10
```

而 kubeadm 在 v1.34.2 对应的版本期望的是： 

```bash
registry.k8s.io/pause:3.10.1
```

因为版本号不一致，就出现了这个警告。

解决方法, 修改 CRI 默认 sandbox 镜像, 对于 cri-dockerd, 需要在 cri-dockerd 的 systemd 启动参数里指定

```bash
sudo vi /lib/systemd/system/cri-docker.service
```

找到这行:

```bash
ExecStart=/usr/bin/cri-dockerd --container-runtime-endpoint fd://
```

在后面加入内容:

```bash
 --pod-infra-container-image=pause:3.10.1
```

或者如果有代码仓库：

```bash
 --pod-infra-container-image=192.168.3.193:5000/k8s-proxy/pause:3.10.1
```

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart cri-docker
```

验证:

```bash
ps -ef | grep cri-dockerd

root       19302       1  0 11:40 ?        00:00:00 /usr/bin/cri-dockerd --container-runtime-endpoint fd:// --pod-infra-container-image=192.168.3.193:5000/k8s-proxy/pause:3.10.1
```
