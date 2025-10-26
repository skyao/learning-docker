---
title: "habor安装"
linkTitle: "安装"
weight: 20
date: 2025-03-11
description: >
  镜像仓库 habor 的安装
---

## 准备工作

### docker的安装

需要先安装好 docker 和 docker-compose。

## 安装

### 离线安装

https://github.com/goharbor/harbor/releases

下载 offline 安装文件如：

```bash
mkdir -p /home/sky/work/soft/
cd /home/sky/work/soft/

wget https://github.com/goharbor/harbor/releases/download/v2.14.0/harbor-offline-installer-v2.14.0.tgz
```

解压安装文件：

```bash
tar -xvf harbor-offline-installer-v2.14.0.tgz
```

复制并修改配置文件：

```bash
cd harbor
cp harbor.yml.tmpl harbor.yml
```

修改 hostname，port 和 harbor_admin_password 等参数：

```bash
hostname: 192.168.3.98
http:
  port: 5000
harbor_admin_password: xxxxxxxx
```

执行安装命令：

```bash
sudo ./install.sh
```

输出为：

```bash
[Step 0]: checking if docker is installed ...

Note: docker version: 28.5.1

[Step 1]: checking docker-compose is installed ...

Note: Docker Compose version v2.40.2

[Step 2]: loading Harbor images ...
Loaded image: goharbor/harbor-db:v2.14.0
Loaded image: goharbor/harbor-log:v2.14.0
Loaded image: goharbor/trivy-adapter-photon:v2.14.0
Loaded image: goharbor/redis-photon:v2.14.0
Loaded image: goharbor/nginx-photon:v2.14.0
Loaded image: goharbor/registry-photon:v2.14.0
Loaded image: goharbor/prepare:v2.14.0
Loaded image: goharbor/harbor-portal:v2.14.0
Loaded image: goharbor/harbor-core:v2.14.0
Loaded image: goharbor/harbor-jobservice:v2.14.0
Loaded image: goharbor/harbor-registryctl:v2.14.0
Loaded image: goharbor/harbor-exporter:v2.14.0

[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /home/sky/temp/harbor
Error happened in config validation...
ERROR:root:Error: The protocol is https but attribute ssl_cert is not set
```

报错没有设置 https 的证书，因为是开发环境，所以可以忽略https。因此修改 harbor.yml 文件，将 https 的配置注释掉：

```bash
# https related config
#https:
  # https port for harbor, default is 443
  #port: 443
  # The path of cert and key files for nginx
  #certificate: /your/certificate/path
  #private_key: /your/private/key/path
  # enable strong ssl ciphers (default: false)
  # strong_ssl_ciphers: false
```

重新安装：

```bash
sudo ./install.sh
``` 

输出为：

```bash 
[Step 0]: checking if docker is installed ...

Note: docker version: 28.5.1

[Step 1]: checking docker-compose is installed ...

Note: Docker Compose version v2.40.2

[Step 2]: loading Harbor images ...
Loaded image: goharbor/harbor-db:v2.14.0
Loaded image: goharbor/harbor-log:v2.14.0
Loaded image: goharbor/trivy-adapter-photon:v2.14.0
Loaded image: goharbor/redis-photon:v2.14.0
Loaded image: goharbor/nginx-photon:v2.14.0
Loaded image: goharbor/registry-photon:v2.14.0
Loaded image: goharbor/prepare:v2.14.0
Loaded image: goharbor/harbor-portal:v2.14.0
Loaded image: goharbor/harbor-core:v2.14.0
Loaded image: goharbor/harbor-jobservice:v2.14.0
Loaded image: goharbor/harbor-registryctl:v2.14.0
Loaded image: goharbor/harbor-exporter:v2.14.0

[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /home/sky/work/soft/harbor
WARNING:root:WARNING: HTTP protocol is insecure. Harbor will deprecate http protocol in the future. Please make sure to upgrade to https
Generated configuration file: /config/portal/nginx.conf
Generated configuration file: /config/log/logrotate.conf
Generated configuration file: /config/log/rsyslog_docker.conf
Generated configuration file: /config/nginx/nginx.conf
Generated configuration file: /config/core/env
Generated configuration file: /config/core/app.conf
Generated configuration file: /config/registry/config.yml
Generated configuration file: /config/registryctl/env
Generated configuration file: /config/registryctl/config.yml
Generated configuration file: /config/db/env
Generated configuration file: /config/jobservice/env
Generated configuration file: /config/jobservice/config.yml
copy /data/secret/tls/harbor_internal_ca.crt to shared trust ca dir as name harbor_internal_ca.crt ...
ca file /hostfs/data/secret/tls/harbor_internal_ca.crt is not exist
copy  to shared trust ca dir as name storage_ca_bundle.crt ...
copy None to shared trust ca dir as name redis_tls_ca.crt ...
Generated and saved secret to file: /data/secret/keys/secretkey
Successfully called func: create_root_cert
Generated configuration file: /compose_location/docker-compose.yml
Clean up the input dir


Note: stopping existing Harbor instance ...


[Step 5]: starting Harbor ...
[+] Running 10/10
 ✔ Network harbor_harbor        Created                                    0.0s
 ✔ Container harbor-log         Started                                    0.1s
 ✔ Container harbor-db          Started                                    0.2s
 ✔ Container registryctl        Started                                    0.2s
 ✔ Container redis              Started                                    0.2s
 ✔ Container harbor-portal      Started                                    0.2s
 ✔ Container registry           Started                                    0.2s
 ✔ Container harbor-core        Started                                    0.3s
 ✔ Container harbor-jobservice  Started                                    0.4s
 ✔ Container nginx              Started                                    0.4s
✔ ----Harbor has been installed and started successfully.----

```

安装完成后就可以用浏览器访问了：

http://192.168.3.130:5000/

用户名：admin，密码：xxxxxxx


## 开机自动启动

上面的方法只能临时启动 habor，方便起见还是应该设置为开机自动启动。

在 debian12/13 上，采用 systemd 的方式实现 habor 的开机自动启动：

```bash
sudo vi /usr/lib/systemd/system/harbor.service
```

内容为：


```properties
[Unit]
Description=Harbor
After=docker.service systemd-networkd.service systemd-resolved.service
Requires=docker.service
Documentation=http://github.com/vmware/harbor

[Service]
Type=simple
Restart=on-failure
RestartSec=5
ExecStart=/usr/local/bin/docker-compose -f /home/sky/work/soft/harbor/docker-compose.yml up
ExecStop=/usr/local/bin/docker-compose -f /home/sky/work/soft/harbor/docker-compose.yml down

[Install]
WantedBy=multi-user.target
```

保存后，执行命令：

```bash
sudo chmod +x /usr/lib/systemd/system/harbor.service
sudo systemctl enable harbor
sudo systemctl start harbor
```







