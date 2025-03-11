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
mkdir -p /home/sky/work/soft/harbor
cd /home/sky/work/soft/harbor

wget https://github.com/goharbor/harbor/releases/download/v2.12.2/harbor-offline-installer-v2.12.2.tgz
```

解压安装文件：

```bash
tar -xvf harbor-offline-installer-v2.12.2.tgz
```

复制并修改配置文件：

```bash
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

Note: docker version: 20.10.24

[Step 1]: checking docker-compose is installed ...

Note: Docker Compose version v2.33.1

[Step 2]: loading Harbor images ...
Loaded image: goharbor/redis-photon:v2.12.2
Loaded image: goharbor/nginx-photon:v2.12.2
Loaded image: goharbor/registry-photon:v2.12.2
Loaded image: goharbor/prepare:v2.12.2
Loaded image: goharbor/harbor-portal:v2.12.2
Loaded image: goharbor/harbor-core:v2.12.2
Loaded image: goharbor/harbor-jobservice:v2.12.2
Loaded image: goharbor/harbor-registryctl:v2.12.2
Loaded image: goharbor/harbor-log:v2.12.2
Loaded image: goharbor/harbor-db:v2.12.2
Loaded image: goharbor/harbor-exporter:v2.12.2
Loaded image: goharbor/trivy-adapter-photon:v2.12.2

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

Note: docker version: 20.10.24

[Step 1]: checking docker-compose is installed ...

Note: Docker Compose version v2.33.1

[Step 2]: loading Harbor images ...
Loaded image: goharbor/redis-photon:v2.12.2
Loaded image: goharbor/nginx-photon:v2.12.2
Loaded image: goharbor/registry-photon:v2.12.2
Loaded image: goharbor/prepare:v2.12.2
Loaded image: goharbor/harbor-portal:v2.12.2
Loaded image: goharbor/harbor-core:v2.12.2
Loaded image: goharbor/harbor-jobservice:v2.12.2
Loaded image: goharbor/harbor-registryctl:v2.12.2
Loaded image: goharbor/harbor-log:v2.12.2
Loaded image: goharbor/harbor-db:v2.12.2
Loaded image: goharbor/harbor-exporter:v2.12.2
Loaded image: goharbor/trivy-adapter-photon:v2.12.2


[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /home/sky/temp/harbor
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
Generated and saved secret to file: /data/secret/keys/secretkey
Successfully called func: create_root_cert
Generated configuration file: /compose_location/docker-compose.yml
Clean up the input dir


Note: stopping existing Harbor instance ...


[Step 5]: starting Harbor ...
[+] Running 10/10
 ✔ Network harbor_harbor        Created                                                                                                        0.0s
 ✔ Container harbor-log         Started                                                                                                        0.2s
 ✔ Container registry           Started                                                                                                        0.5s
 ✔ Container registryctl        Started                                                                                                        0.7s
 ✔ Container harbor-db          Started                                                                                                        0.6s
 ✔ Container harbor-portal      Started                                                                                                        0.8s
 ✔ Container redis              Started                                                                                                        0.8s
 ✔ Container harbor-core        Started                                                                                                        1.0s
 ✔ Container harbor-jobservice  Started                                                                                                        1.2s
 ✔ Container nginx              Started                                                                                                        1.1s
✔ ----Harbor has been installed and started successfully.----
```

安装完成后就可以用浏览器访问了：

http://192.168.3.221:5000/

用户名：admin，密码：xxxxxxx









