---
title: "离线安装 Docker"
linkTitle: "离线安装"
weight: 30
date: 2025-05-09
description: >
  在 debian13 上离线安装 docker
---

## 制作离线安装包

参考在线安装的方式， 同样需要先添加 docker 的 apt 仓库，然后找到需要安装的版本， 下载离线安装包。

```bash
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

注意：要在一个没有安装 docker 的机器上下载。比如从 debian 的 basic 模板 clone,不要从 dev 模板 clone.

```bash
# 创建临时目录
mkdir -p ~/temp/docker-offline && cd ~/temp/docker-offline

VERSION_STRING=5:29.1.1-1~debian.13~trixie

# 下载 Docker CE 的 .deb 包（替换版本号为最新版）
apt-get download docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin

# 下载所有依赖（可能需要运行多次直到无新依赖）
apt-get download $(apt-cache depends docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin | grep -E 'Depends|Recommends' | cut -d ':' -f 2 | tr -d ' ' | grep -v "^docker" | sort -u)

# 下载 iptables 的几个依赖包
apt-get download libip4tc2 libip6tc2 libnetfilter-conntrack3 libnfnetlink0
```

参考下载安装文档，下载最新的 docker-compose 离线安装包：

```bash
wget https://github.com/docker/compose/releases/download/v2.40.3/docker-compose-linux-x86_64
```

完成后的离线安装包如下：

```bash
$ ls -lh

total 175M
-rw-r--r-- 1 sky sky 694K Apr 11  2025 apparmor_4.1.0-1_amd64.deb
-rw-r--r-- 1 sky sky 158K Apr 19  2025 ca-certificates_20250419_all.deb
-rw-r--r-- 1 sky sky  23M Nov 29 00:12 containerd.io_2.2.0-2~debian.13~trixie_amd64.deb
-rw-r--r-- 1 sky sky  16M Nov 29 00:08 docker-buildx-plugin_0.30.1-1~debian.13~trixie_amd64.deb
-rw-r--r-- 1 sky sky  21M Nov 29 00:08 docker-ce_5%3a29.1.1-1~debian.13~trixie_amd64.deb
-rw-r--r-- 1 sky sky  16M Nov 29 00:08 docker-ce-cli_5%3a29.1.1-1~debian.13~trixie_amd64.deb
-rw-rw-r-- 1 sky sky  74M Oct 30 17:20 docker-compose-linux-x86_64
-rw-r--r-- 1 sky sky  14M Nov  5 18:19 docker-compose-plugin_2.40.3-1~debian.13~trixie_amd64.deb
-rw-r--r-- 1 sky sky 8.5M Aug 23 05:05 git_1%3a2.47.3-0+deb13u1_amd64.deb
-rw-r--r-- 1 sky sky  39K Aug 23 05:05 init-system-helpers_1.69~deb13u1_all.deb
-rw-r--r-- 1 sky sky 353K Nov 20  2024 iptables_1.8.11-2_amd64.deb
-rw-r--r-- 1 sky sky 2.8M Aug  6 02:48 libc6_2.41-12_amd64.deb
-rw-r--r-- 1 sky sky  20K Nov 20  2024 libip4tc2_1.8.11-2_amd64.deb
-rw-r--r-- 1 sky sky  20K Nov 20  2024 libip6tc2_1.8.11-2_amd64.deb
-rw-r--r-- 1 sky sky  42K Sep 26  2024 libnetfilter-conntrack3_1.1.0-1_amd64.deb
-rw-r--r-- 1 sky sky  15K Mar 29  2024 libnfnetlink0_1.0.2-3_amd64.deb
-rw-r--r-- 1 sky sky 325K Jun 30 00:49 libnftables1_1.1.3-1_amd64.deb
-rw-r--r-- 1 sky sky  51K Mar 21  2025 libseccomp2_2.6.0-2_amd64.deb
-rw-r--r-- 1 sky sky 443K Oct 19 20:34 libsystemd0_257.9-1~deb13u1_amd64.deb
-rw-r--r-- 1 sky sky  75K Jun 30 00:49 nftables_1.1.3-1_amd64.deb
-rw-r--r-- 1 sky sky  62K Aug 23  2023 pigz_2.8-1_amd64.deb
-rw-r--r-- 1 sky sky 862K Jul 30 20:31 procps_2%3a4.0.4-9_amd64.deb
-rw-r--r-- 1 sky sky 645K Apr  4  2025 xz-utils_5.8.1-1_amd64.deb
```

将这个离线安装包压缩成一个 tar 包(注意要加入下面提到的安装脚本), 然后复制到 devserver 上以便后续使用：

```bash
cd ~/temp/
tar -czvf docker-offline-debian13-v29.1.1-1.tar.gz docker-offline

scp ./docker-offline-debian13-v29.1.1-1.tar.gz sky@192.168.3.193:/home/sky/temp

# 用 sky 帐号登录 192.168.3.193 机器后再执行：
# 安全期间不容许 root 帐号直接 ssh 登录
ssh sky@192.168.3.193
sudo mv ~/temp/docker-offline-debian13-v29.1.1-1.tar.gz /mnt/data/downloads/docker
```

用浏览器打开如下地址就可以看到离线安装包：

[http://192.168.3.193/downloads/docker/](http://192.168.3.193/downloads/docker/)

## 离线安装

### 获取离线安装文件

在某台没有安装 docker 的机器上,比如重新从 debian basic 模板 clone 一个新的虚拟机. 

下载离线安装包到本地：

```bash
mkdir -p ~/temp/ && cd ~/temp/

wget http://192.168.3.193/downloads/docker/docker-offline-debian13-v29.1.1-1.tar.gz
```

解压离线安装包：

```bash
tar -xvf docker-offline-debian13-v29.1.1-1.tar.gz
```

### 手工安装 docker

备注： 这里的操作仅仅是用来为后面的离线安装包做验证。

备注2： 后面使用脚本安装，这里的版本信息就没有及时更新了。

```bash
cd ~/temp/docker-offline

# 安装各种依赖
sudo dpkg -i apparmor_4.1.0-1_amd64.deb
sudo dpkg -i ca-certificates_20250419_all.deb
sudo dpkg -i libc6_2.41-12_amd64.deb
sudo dpkg -i libseccomp2_2.6.0-2_amd64.deb
sudo dpkg -i libsystemd0_257.8-1\~deb13u2_amd64.deb
sudo dpkg -i pigz_2.8-1_amd64.deb
sudo dpkg -i git_1%3a2.47.3-0+deb13u1_amd64.deb 
sudo dpkg -i procps_2%3a4.0.4-9_amd64.deb
sudo dpkg -i xz-utils_5.8.1-1_amd64.deb

# 安装 iptables 和它的依赖
sudo dpkg -i libnfnetlink0_1.0.2-3_amd64.deb
sudo dpkg -i libnetfilter-conntrack3_1.1.0-1_amd64.deb
sudo dpkg -i libip4tc2_1.8.11-2_amd64.deb
sudo dpkg -i libip6tc2_1.8.11-2_amd64.deb
sudo dpkg -i iptables_1.8.11-2_amd64.deb

# 安装 docker
sudo dpkg -i containerd.io_2.1.5-1\~debian.13\~trixie_amd64.deb                   
sudo dpkg -i docker-buildx-plugin_0.29.1-1\~debian.13\~trixie_amd64.deb
sudo dpkg -i docker-ce-cli_5%3a29.0.0-1\~debian.13\~trixie_amd64.deb
sudo dpkg -i docker-ce_5%3a29.0.0-1\~debian.13\~trixie_amd64.deb
sudo dpkg -i docker-compose-plugin_2.40.3-1\~debian.13\~trixie_amd64.deb
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
  "registry-mirrors": ["http://192.168.3.193:5000/"],
  "insecure-registries": ["192.168.3.193:5000"]
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

验证一下:

```bash
docker-compose version
```

### 制作离线安装脚本

离线安装避免在线安装的网络问题，非常方便，考虑写一个离线安装脚本，方便以后使用。

```bash
cd ~/temp
vi install_docker_offline_debian13.zsh
```

内容为：

```bash
#!/usr/bin/env zsh

# ------------------------------------------------------------
# Docker & Docker Compose 离线安装脚本 (Debian 13)
# 前提条件：
# 1. 所有 .deb 文件和 docker-compose 二进制文件已放在 ~/docker-offline
# ------------------------------------------------------------

set -e  # 遇到错误立即退出

DOCKER_OFFLINE_DIR="./docker-offline"

# 检查是否在 Debian 13 上运行
if ! grep -q "Debian GNU/Linux 13" /etc/os-release; then
    echo "❌ 错误：此脚本仅适用于 Debian 13！"
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
for pkg in apparmor ca-certificates libc6 libseccomp2 libsystemd0 pigz git procps xz-utils libip6tc2 libnfnetlink0 libnetfilter-conntrack3 libip4tc2 iptables ; do
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
for pkg in containerd.io docker-buildx-plugin docker-ce-cli docker-ce docker-compose-plugin; do
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
  "registry-mirrors": ["http://192.168.3.193:5000/"],
  "insecure-registries": ["192.168.3.193:5000"]
}
EOF

echo "⚙️ 配置 Docker 使用代理..."

# 创建 http-proxy.conf
sudo mkdir -p /etc/systemd/system/docker.service.d/
sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf <<EOF
[Service]
Environment="HTTP_PROXY=http://192.168.3.1:7890"
Environment="HTTPS_PROXY=http://192.168.3.1:7890"
Environment="NO_PROXY=127.0.0.1,localhost,local,.local,.lan,192.168.0.0/16,10.0.0.0/16"
EOF

# 启动并启用 Docker 服务
echo "🚀 启动 Docker 服务..."
sudo systemctl enable --now docker.service
sudo systemctl enable --now containerd.service
sudo systemctl daemon-reload
sudo systemctl restart docker

# ------------------------------------------------------------
# 4. 验证安装
# ------------------------------------------------------------
echo "✅ 安装完成！验证版本："
docker --version
docker-compose --version

echo "✨ docker 和 docker-compose 安装完成！"

# 添加当前用户到 docker 组（避免每次用 sudo）
echo "👥 为避免每次用 sudo ，将当前用户加入 docker 组..."
echo "👥 执行命令： sudo usermod -aG docker \$USER"
sudo usermod -aG docker $USER
    
echo "👥 为避免重新登录，即将执行命令 newgrp docker 以便立即生效"
echo "请在脚本执行结束后, 手工执行 docker run hello-world 以检验 docker 安装是否成功."
echo ""
echo "docker run hello-world"

cd ..
newgrp docker
```

这个离线安装脚本可以和 docker 离线安装文件一起打包，方便以后使用。

```bash
cd ~/temp
chmod +x docker-offline install_docker_offline_debian13.zsh
tar -czvf docker-offline-debian13-v29.1.1-1.tar.gz docker-offline install_docker_offline_debian13.zsh
```

然后将这个离线安装包拷贝备份到 devserver 机器下，以后就可以方便的重用了。

### 用脚本安装

从 devserver 机器获取离线安装包，解压离线安装包，执行离线安装脚本：

```bash
mkdir -p ~/temp/
cd ~/temp/
wget http://192.168.3.193/downloads/docker/docker-offline-debian13-v29.1.1-1.tar.gz
tar -xvf docker-offline-debian13-v29.1.1-1.tar.gz
./install_docker_offline_debian13.zsh
```

现在就非常方便了！


