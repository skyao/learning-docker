---
title: "Docker网络概述"
linkTitle: "概述"
weight: 901
date: 2021-02-01
description: >
  Docker网络概述
---


> 内容摘要自： https://docs.docker.com/network/

### 网络驱动

Docker的网络子系统是可插拔的，使用驱动程序。默认情况下存在多个驱动程序，并提供核心网络功能：

- `bridge`：默认网络驱动程序。如果未指定驱动程序，则这是您要创建的网络类型。**当您的应用程序在需要通信的独立容器中运行时，通常会使用桥接网络。**
- `host`：对于独立容器，移除容器和Docker主机之间的网络隔离，直接使用主机的网络。`host` 仅适用于Docker 17.06及更高版本的swarm service。
- `overlay`：覆盖网络将多个Docker守护程序连接在一起，并使swarm service能够相互通信。您还可以使用覆盖网络来促进swarm service和独立容器之间的通信，或者在不同Docker守护程序上的两个独立容器之间进行通信。此策略消除了在这些容器之间执行OS级别路由的需要。
- `macvlan`：Macvlan网络允许您为容器分配MAC地址，使其显示为网络上的物理设备。Docker守护程序通过其MAC地址将流量路由到容器。`macvlan` 在处理期望直接连接到物理网络的传统应用程序时是最佳选择，而不是通过Docker主机的网络堆栈进行路由。
- `none`：对于此容器，禁用所有网络。通常与自定义网络驱动程序一起使用。`none`不适用于swarm service。
- 网络插件：您可以使用Docker安装和使用第三方网络插件。这些插件可从 [Docker Hub](https://hub.docker.com/search?category=network&q=&type=plugin) 或第三方供应商处获得。有关安装和使用给定网络插件的信息，请参阅供应商的文档。

### 网络驱动摘要

- 当您需要多个容器在同一个Docker主机上进行通信时，**用户定义的桥接网络/User-defined bridge networks**是最佳选择。
- 当网络堆栈不应与Docker主机隔离时，但您希望隔离容器的其他方面，**主机网络/host network**是最好的。
- 当您需要在不同Docker主机上运行的容器进行通信时，**覆盖网络/Overlay networks**是最佳选择。
- 当您从VM设置迁移或需要容器看起来像网络上的物理主机时，**Macvlan网络**是最佳的，每个主机都具有唯一的MAC地址。
- **第三方网络插件**允许您将Docker与专用网络堆栈集成。