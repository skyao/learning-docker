---
title: "Docker命令概述"
linkTitle: "概述"
weight: 501
date: 2021-02-01
description: >
  Docker命令概述
---


Docker 分为客户端和服务端两部分, docker 为客户端调用的命令, dockerd 为服务端调用的命令, 这里着重介绍客户端的用法。

Docker主要用法：`docker [docker命令选项] [子命令] [子命令选项]`

> 备注：使用 `docker [子命令] --help` 可查看每个子命令的详细用法。

## 命令选项列表

| 选项                     | 说明                                                         | 其他                           |
| ------------------------ | ------------------------------------------------------------ | ------------------------------ |
| --config [string]        | 客户端本地配置文件路径                                       | 默认为 `~/.docker`             |
| -D, --debug              | 启用调试模式                                                 |                                |
| --help                   | 打印用法                                                     |                                |
| -H, --host list          | 通过socket访问指定的docker守护进程(服务端)                   | `unix://` , `fd://` , `tcp://` |
| -l, --log-level [string] | 设置日志级别 (`debug` 、`info` 、`warn` 、`error` 、`fatal`) | 默认为 `info`                  |
| --tls                    | 启用TLS加密                                                  |                                |
| --tlscacert [string]     | 指定信任的CA根证书路径                                       | 默认为 `~/.docker/ca.pem`      |
| --tlscert [string]       | 客户端证书路径                                               | 默认为 `~/.docker/cert.pem`    |
| --tlskey [string]        | 客户端证书私钥路径                                           | 默认为 `~/.docker/key.pem`     |
| --tlsverify              | 启用TLS加密并验证客户端证书                                  |                                |
| -v, --version            | 打印docker客户端版本信息                                     |                                |

## 命令列表

### 本地镜像相关命令

- docker images： 

### 镜像仓库相关命令

- docker search: 查找镜像
- docker pull: 获取镜像
- docker push: 推送镜像到仓库
- docker login: 登录第三方仓库
- docker logout: 退出第三方仓库

### 参考资料

- [docker命令详解](https://segmentfault.com/a/1190000008876540)：很全
- [Docker 常用指令详解](https://www.jianshu.com/p/7c9e2247cfbd): 2017-12，很全，推荐