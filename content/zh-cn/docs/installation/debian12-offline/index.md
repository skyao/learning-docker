---
title: "debian 12 离线安装 Docker"
linkTitle: "debian12离线安装"
weight: 30
date: 2025-05-09
description: >
  Docker 在 debian12 离线安装
---

## 制作离线安装包

参考在线安装的方式， 同样需要先添加 docker 的 apt 仓库，然后找到需要安装的版本， 下载离线安装包。

```bash
# 创建临时目录
mkdir -p ~/temp/docker-offline && cd ~/temp/docker-offline

# 下载 Docker CE 的 .deb 包（替换版本号为最新版）
apt-get download docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 下载所有依赖（可能需要运行多次直到无新依赖）
apt-get download $(apt-cache depends docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin | grep -E 'Depends|Recommends' | cut -d ':' -f 2 | tr -d ' ' | grep -v "^docker" | sort -u)

# 下载 iptables 的几个依赖包
apt-get download libip6tc2 libnetfilter-conntrack3 libnfnetlink0
```

参考下载安装文档，下载最新的 docker-compose 离线安装包：

```bash
wget https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64
```

完成后的离线安装包如下：

```bash
ls -lh
total 192M
-rw-r--r-- 1 sky sky 602K Feb 14  2023 apparmor_3.0.8-3_amd64.deb
-rw-r--r-- 1 sky sky 150K Mar 11  2023 ca-certificates_20230311_all.deb
-rw-r--r-- 1 sky sky  30M Apr  2 18:54 containerd.io_1.7.27-1_amd64.deb
-rw-r--r-- 1 sky sky  34M Apr 25 21:06 docker-buildx-plugin_0.23.0-1~debian.12~bookworm_amd64.deb
-rw-r--r-- 1 sky sky  19M Apr 25 21:06 docker-ce_5%3a28.1.1-1~debian.12~bookworm_amd64.deb
-rw-r--r-- 1 sky sky  16M Apr 25 21:06 docker-ce-cli_5%3a28.1.1-1~debian.12~bookworm_amd64.deb
-rw-r--r-- 1 sky sky  71M May  7 19:24 docker-compose-linux-x86_64
-rw-r--r-- 1 sky sky  14M Apr 25 21:06 docker-compose-plugin_2.35.1-1~debian.12~bookworm_amd64.deb
-rw-r--r-- 1 sky sky 7.0M Jan 13 04:28 git_1%3a2.39.5-0+deb12u2_amd64.deb
-rw-r--r-- 1 sky sky 352K Jan 16  2023 iptables_1.8.9-2_amd64.deb
-rw-r--r-- 1 sky sky 2.7M Mar  8 05:26 libc6_2.36-9+deb12u10_amd64.deb
-rw-r--r-- 1 sky sky  19K Jan 16  2023 libip6tc2_1.8.9-2_amd64.deb
-rw-r--r-- 1 sky sky 384K Apr 23  2024 libltdl7_2.4.7-7~deb12u1_amd64.deb
-rw-r--r-- 1 sky sky  40K Jan  3  2023 libnetfilter-conntrack3_1.0.9-3_amd64.deb
-rw-r--r-- 1 sky sky  15K Apr  9  2022 libnfnetlink0_1.0.2-2_amd64.deb
-rw-r--r-- 1 sky sky  46K Jun 17  2024 libseccomp2_2.5.4-1+deb12u1_amd64.deb
-rw-r--r-- 1 sky sky 325K Mar  8 05:01 libsystemd0_252.36-1~deb12u1_amd64.deb
-rw-r--r-- 1 sky sky  63K Feb  6  2021 pigz_2.6-1_amd64.deb
-rw-r--r-- 1 sky sky 693K Dec 20  2022 procps_2%3a4.0.2-3_amd64.deb
-rw-r--r-- 1 sky sky 460K Apr  4 05:20 xz-utils_5.4.1-1_amd64.deb
```

将这个离线安装包压缩成一个 tar 包：

```bash
cd ~/temp/
tar -czvf docker-offline-v28.1.1-1.tar.gz docker-offline
```

## 离线安装

### 获取离线安装文件

下载离线安装包到本地：

```bash
mkdir -p ~/temp/ && cd ~/temp/

scp sky@192.168.3.179:/home/sky/temp/docker-offline-v28.1.1-1.tar.gz .
```

解压离线安装包：

```bash
tar -xvf docker-offline-v28.1.1-1.tar.gz
cd docker-offline
```

### 手工安装 docker

```bash
# 安装各种依赖
sudo dpkg -i apparmor_3.0.8-3_amd64.deb
sudo dpkg -i ca-certificates_20230311_all.deb
sudo dpkg -i libc6_2.36-9+deb12u10_amd64.deb
sudo dpkg -i libltdl7_2.4.7-7~deb12u1_amd64.deb
sudo dpkg -i libseccomp2_2.5.4-1+deb12u1_amd64.deb
sudo dpkg -i libsystemd0_252.36-1~deb12u1_amd64.deb
sudo dpkg -i pigz_2.6-1_amd64.deb
sudo dpkg -i git_1%3a2.39.5-0+deb12u2_amd64.deb   
sudo dpkg -i procps_2%3a4.0.2-3_amd64.deb
sudo dpkg -i xz-utils_5.4.1-1_amd64.deb

# 安装 iptables 和它的依赖
sudo dpkg -i libip6tc2_1.8.9-2_amd64.deb 
sudo dpkg -i libnetfilter-conntrack3_1.0.9-3_amd64.deb
sudo dpkg -i libnfnetlink0_1.0.2-2_amd64.deb
sudo dpkg -i iptables_1.8.9-2_amd64.deb

# 安装 docker
sudo dpkg -i containerd.io_1.7.27-1_amd64.deb                             
sudo dpkg -i docker-buildx-plugin_0.23.0-1~debian.12~bookworm_amd64.deb   
sudo dpkg -i docker-ce_5%3a28.1.1-1~debian.12~bookworm_amd64.deb          
sudo dpkg -i docker-ce-cli_5%3a28.1.1-1~debian.12~bookworm_amd64.deb      
sudo dpkg -i docker-compose-plugin_2.35.1-1~debian.12~bookworm_amd64.deb
```

添加用户到 docker 组：

```bash
sudo usermod -aG docker $USER
newgrp docker
```

启动 docker 服务：

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

设置 docker 的镜像源：

```bash
sudo mkdir -p /etc/docker && sudo tee /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["http://192.168.3.91:5000/"],
  "insecure-registries": ["192.168.3.91:5000"]
}
EOF
```

验证一下：

```bash
docker run hello-world
```

### 手工安装 docker-compose

```bash
sudo mv ./docker-compose-linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 制作离线安装脚本

离线安装避免在线安装的网络问题，非常方便，考虑写一个离线安装脚本，方便以后使用。

```bash
install_docker_offline.zsh
```

内容为：

```bash
#!/usr/bin/env zsh

# ------------------------------------------------------------
# Docker & Docker Compose 离线安装脚本 (Debian 12)
# 前提条件：
# 1. 所有 .deb 文件和 docker-compose 二进制文件已放在 ~/docker-offline
# ------------------------------------------------------------

set -e  # 遇到错误立即退出

DOCKER_OFFLINE_DIR="./docker-offline"

# 检查是否在 Debian 12 上运行
if ! grep -q "Debian GNU/Linux 12" /etc/os-release; then
    echo "❌ 错误：此脚本仅适用于 Debian 12！"
    exit 1
fi

# 检查是否已安装 Docker
if command -v docker &>/dev/null; then
    echo "⚠️ Docker 已安装，跳过安装步骤。"
    exit 0
fi

# 检查离线目录是否存在
if [[ ! -d "$DOCKER_OFFLINE_DIR" ]]; then
    echo "❌ 错误：离线目录 $DOCKER_OFFLINE_DIR 不存在！"
    exit 1
fi

echo "🔧 开始离线安装 Docker 和 Docker Compose..."

# ------------------------------------------------------------
# 1. 安装 Docker CE 及其依赖
# ------------------------------------------------------------
echo "📦 安装 Docker CE 的依赖包..."
cd "$DOCKER_OFFLINE_DIR"

# 按顺序安装依赖（防止 dpkg 报错）
for pkg in apparmor ca-certificates libc6 libltdl7 libseccomp2 libsystemd0 pigz git procps xz-utils libip6tc2 libnetfilter-conntrack3 libnfnetlink0 iptables ; do
    if ls "${pkg}"*.deb &>/dev/null; then
        echo "➡️ 正在安装: ${pkg}"
        sudo dpkg -i "${pkg}"*.deb || true  # 忽略部分错误，后续用 apt-get -f 修复
    fi
done

# 修复依赖关系
echo "🛠️ 修复依赖关系..."
sudo apt-get -f install -y

# 按顺序安装 docker 组件（防止 dpkg 报错）
echo "📦 安装 Docker CE 组件..."
for pkg in containerd.io docker-buildx-plugin docker-ce docker-ce-cli docker-compose-plugin; do
    if ls "${pkg}"*.deb &>/dev/null; then
        echo "➡️ 正在安装: ${pkg}"
        sudo dpkg -i "${pkg}"*.deb || true  # 忽略部分错误，后续用 apt-get -f 修复
    fi
done

# 修复依赖关系
echo "🛠️ 修复依赖关系..."
sudo apt-get -f install -y

# ------------------------------------------------------------
# 2. 安装 Docker Compose
# ------------------------------------------------------------
echo "📦 安装 Docker Compose..."
if [[ -f "./docker-compose-linux-x86_64" ]]; then
    sudo cp "./docker-compose-linux-x86_64" /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
else
    echo "⚠️ 未找到 docker-compose 二进制文件，跳过安装。"
fi

# ------------------------------------------------------------
# 3. 配置 Docker 并启动服务
# ------------------------------------------------------------
echo "⚙️ 配置 Docker 使用本地镜像源..."

# 创建 daemon.json（优化配置）
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["http://192.168.3.91:5000/"],
  "insecure-registries": ["192.168.3.91:5000"]
}
EOF

# 启动并启用 Docker 服务
echo "🚀 启动 Docker 服务..."
sudo systemctl enable --now docker.service
sudo systemctl enable --now containerd.service

# ------------------------------------------------------------
# 4. 验证安装
# ------------------------------------------------------------
echo "✅ 安装完成！验证版本："
docker --version
docker-compose --version

echo "✨ docker 和 docker-compose 安装完成！"

# 添加当前用户到 docker 组（避免每次用 sudo）
echo "👥 为 避免每次用 sudo ，请将当前用户加入 docker 组..."
echo "👥 请执行命令： sudo usermod -aG docker \$USER"
echo "👥 然后重新登录，或者执行命令立即生效： newgrp docker"
echo "🟢 之后请运行测试容器： docker run hello-world"

```

这个离线安装脚本可以和 docker 离线安装文件一起打包，方便以后使用。

```bash
tar -czvf docker-offline-v28.1.1-1.tar.gz docker-offline install_docker_offline.zsh
```

然后将这个离线安装包拷贝备份到 devserver 机器下，以后就可以方便的重用了。

### 用脚本安装

从 devserver 机器获取离线安装包，解压离线安装包，执行离线安装脚本：

```bash
mkdir -p ~/temp/
scp sky@192.168.3.91:/mnt/data/backup/soft/docker/docker-offline/docker-offline-v28.1.1-1.tar.gz ~/temp/
cd ~/temp/
tar -xvf docker-offline-v28.1.1-1.tar.gz
./install_docker_offline.zsh
```

现在就非常方便了！


