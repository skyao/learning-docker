---
title: "[归档]ubuntu 16.04下安装Docker"
linkTitle: "[归档]ubuntu 16.04"
weight: 219
date: 2021-02-01
description: >
  Docker在Linux下的安装
---

{{% pageinfo color="primary" %}}
归档原因：不再使用ubuntu 16.04.
{{% /pageinfo %}}

以 ubuntu 为例，参考官网文档：

https://docs.docker.com/install/linux/docker-ce/ubuntu/

### 安装准备

看一下自己的Linux机器版本和内核信息：

- `uname --all`

	```bash
	Linux skywork 4.15.0-38-generic #41~16.04.1-Ubuntu SMP Wed Oct 10 20:16:04 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
	```

- `cat /proc/version`

	```bash
	Linux version 4.15.0-38-generic (buildd@lcy01-amd64-023) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.10)) #41~16.04.1-Ubuntu SMP Wed Oct 10 20:16:04 UTC 2018
	```

- `lsb_release -a`

	```bash
  No LSB modules are available.
  Distributor ID:	LinuxMint
  Description:	Linux Mint 18.3 Sylvia
  Release:	18.3
  Codename:	sylvia
	```

这台机器是Linux Mint 18.3 升级到非常新的Linux内核4.15.0-38了，Linux Mint 18.3 是基于 Ubuntu 16.04，但是有些系统参数不一样，比如这里 Codename 是 `sylvia`，而不是 Ubuntu 16.04 的 `xenial`。后面安装时要小心，需要修改为 Ubuntu 16.04 的参数。

### 安装步骤

1. 卸载旧版本

    ```bash
    sudo apt-get remove docker docker-engine docker.io
    ```

2. 安装docker

    简单起见，使用 docker 仓库安装。

    ```bash
    sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    # 官方给的是这样，但是`lsb_release -cs`在Linux Mint下取值会不对，因此手工修改
    # Linux Mint 18是基于ubuntu 16.04，为 xenial
    # Linux Mint 19是基于ubuntu 18.04，为 bionic
    # sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    # for ubuntu 16.04 / linuxmint 18.*
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
    # for ubuntu 18.04 / linuxmint 19.*
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
    sudo apt-get update
    sudo apt-get install docker-ce
    ```

    如果不想安装最新版本，则可以先查看当前可选版本：

    ```bash
    $ apt-cache madison docker-ce
     docker-ce | 5:18.09.2~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 5:18.09.1~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 5:18.09.0~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 18.06.2~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 18.06.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 18.06.0~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
     docker-ce | 18.03.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
    ```
	
	然后执行 :
	
	```bash
	    sudo apt-get install docker-ce=18.06.2~ce~3-0~ubuntu
	```

3. 验证安装

    安装完成之后，看一下版本信息：

    ```bash
    $ docker version
    Client:
    Version:           18.06.1-ce
    API version:       1.38
    Go version:        go1.10.3
    Git commit:        e68fc7a
    Built:             Tue Aug 21 17:24:56 2018
    OS/Arch:           linux/amd64
    Experimental:      false

    Server:
    Engine:
    Version:          18.06.1-ce
    API version:      1.38 (minimum version 1.12)
    Go version:       go1.10.3
    Git commit:       e68fc7a
    Built:            Tue Aug 21 17:23:21 2018
    OS/Arch:          linux/amd64
    Experimental:     false

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
    78445dd45222: Pull complete
    Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ...
    ```

4. 设置权限避免每次sudo

    为了以非root用户使用docker, 可以将用户加入"docker"组.

    ```bash
    sudo usermod -aG docker sky
    ```

	重新登录之后生效。

5. 安装 docker client

	从下列地址下载到对应的 docker client 安装包：

	https://mirrors.ustc.edu.cn/docker-ce/linux/static/stable/x86_64/

	```bash
	tar -zxvf docker-18.09.2.tgz
	cd docker
	sudo cp * /usr/local/bin/
	```

	