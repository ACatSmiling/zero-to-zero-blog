>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-02`*

## CentOS 安装

### ISO 镜像下载

> 官方网站：https://www.centos.org/

目前，最新版本为 CentOS Stream 9：

![image-20240809232431749](linux-series-01-install/image-20240809232431749.png)

本文以 CentOS 7 为例，下载页拉到下面，选择旧版本安装。

>## Older Versions
>
>Legacy versions of CentOS are no longer supported. For historical purposes, CentOS keeps an archive of older versions. If you’re absolutely sure you need an older version [then click here](http://wiki.centos.org/Download).

进入[旧版本下载页面](http://wiki.centos.org/Download)：

![image-20240810192403243](linux-series-01-install/image-20240810192403243.png)

进入[镜像下载地址](https://www.centos.org/download/mirrors)：

![image-20240810192934661](linux-series-01-install/image-20240810192934661.png)

进入[镜像列表](https://mirrormanager.fedoraproject.org/mirrors/CentOS)，选择中国区的镜像：

![image-20240810193044573](linux-series-01-install/image-20240810193044573.png)

此处，选择 [Tencent 的镜像](mirrors.tencent.com)，然后选择 [CentOS 7.9 版本](https://mirrors.cloud.tencent.com/centos/7.9.2009/)：

![image-20240810231332578](linux-series-01-install/image-20240810231332578.png)

![image-20240810231404187](linux-series-01-install/image-20240810231404187.png)

### Hyper-V 虚拟机安装

打开控制面板，找到程序`启用或关闭 Windows 功能`，开启 Hyper-V：

![image-20240902213506399](linux-series-01-install/image-20240902213506399.png)

win + R 快捷键，输入`virtmgmt.msc`，打开 Hyper-V 管理器，新建虚拟机：

![image-20240810232300618](linux-series-01-install/image-20240810232300618.png)

![image-20240810232628638](linux-series-01-install/image-20240810232628638.png)

![image-20240810232655989](linux-series-01-install/image-20240810232655989.png)

![image-20240810232712363](linux-series-01-install/image-20240810232712363.png)

![image-20240810232808392](linux-series-01-install/image-20240810232808392.png)

![image-20240810232854591](linux-series-01-install/image-20240810232854591.png)

在启动新建的虚拟机之前，进行额外设置：

![image-20240810233111225](linux-series-01-install/image-20240810233111225.png)

![image-20240810233140916](linux-series-01-install/image-20240810233140916.png)

![image-20240810233236380](linux-series-01-install/image-20240810233236380.png)

![image-20240810233334324](linux-series-01-install/image-20240810233334324.png)

> 虚拟交换机 LAN1：
>
> ![image-20240810233544839](linux-series-01-install/image-20240810233544839.png)
>
> ![image-20240810233601103](linux-series-01-install/image-20240810233601103.png)

现在，启动虚拟机，并连接虚拟机，然后进行 CentOS 7.9 的安装：

![image-20240810233702440](linux-series-01-install/image-20240810233702440.png)

![image-20240810233805639](linux-series-01-install/image-20240810233805639.png)

安装选项首页面：

![image-20240811093656974](linux-series-01-install/image-20240811093656974.png)

日期和时间设置：

![image-20240811002148179](linux-series-01-install/image-20240811002148179.png)

软件选择设置：

![image-20240811002246815](linux-series-01-install/image-20240811002246815.png)

安装位置设置：

![image-20240811151656610](linux-series-01-install/image-20240811151656610.png)

![image-20240811152413706](linux-series-01-install/image-20240811152413706.png)

![image-20240811152836151](linux-series-01-install/image-20240811152836151.png)

![image-20240811153221227](linux-series-01-install/image-20240811153221227.png)

>如果按上述配置完成后，提示如下 Configuration Error，则将引导分区的挂载点修改为：/boot/efi。
>
>![image-20240811154512798](linux-series-01-install/image-20240811154512798.png)

KDUMP 设置：（Kdump 是一个内核崩溃转储机制。在系统崩溃的时候，kdump 将捕获系统信息，这对于诊断崩溃的原因非常有用。注意，kdump 需要预留一部分系统内存，且这部分内存对于其他用户是不可用的）

![image-20240811154935352](linux-series-01-install/image-20240811154935352.png)

网络和主机名设置：（网络和主机名，在虚拟机安装完成后，也可以通过命令行设置）

![image-20240811155349951](linux-series-01-install/image-20240811155349951.png)

配置完成后，点击开始安装：

![image-20240811155720123](linux-series-01-install/image-20240811155720123.png)

等待安装完成，然后重启虚拟机。

## CentOS 初始化

### 设置静态 IP

```shell
# 1. 设置静态 IP，修改网卡 eth0 的配置
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
# 将动态 IP 修改为静态 IP
#BOOTPROTO="dhcp"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="eth0"
UUID="a784cd9a-e2fc-4593-839b-9bffa52d6898"
DEVICE="eth0"
ONBOOT="yes"
# 添加静态 IP 地址
IPADDR=192.168.1.30
# 添加网关
GATEWAY=192.168.1.1
# 添加域名解析器
DNS1=192.168.1.1

# 2. 重启网络服务
$ service network restart

# 3. 重新查看虚拟机地址
$ ip a
```

### 设置主机名

```shell
# 查看主机名
$ hostname

# 修改主机名，需要重启虚拟机
$ vi /etc/hostname

# 修改主机名，立即生效
$ hostnamectl set-hostname test
$ hostname
```

### 添加本地主机域名解析

```shell
$ vi /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

# 新增本地主机域名解析
192.168.1.20 zeloud
```

### yum 源配置

到镜像地址下载源文件，再手动上传到服务器：

![image-20240817152143079](linux-series-01-install/image-20240817152143079.png)

- 阿里云镜像：http://mirrors.aliyun.com/repo/CentOS-7.repo
- 网易 163 镜像：http://mirrors.163.com/.help/CentOS7-Base-163.repo

使用 root 用户执行以下操作：

```shell
# 1. 进入 /etc/yum.repos.d/ 目录
$ cd /etc/yum.repos.d/

# 2. 备份默认的 CentOS-Base.repo
$ cp CentOS-Base.repo CentOS-Base.repo.bak

# 3. 使用镜像的 repo 文件替换默认的 repo 文件
$ mv /home/zeloud/Centos-7.repo .
$ mv Centos-7.repo CentOS-Base.repo

# 4. 清理旧的缓存数据
$ yum clean all

# 5. 缓存新数据
$ yum makecache

# 6. 测试
$ yum list wget
```

- 注意修改镜像源文件的用户和用户组信息：`chown root:root Centos-7.repo`。

### 安装常用工具

```shell
$ yum install -y vim-enhanced

$ yum install -y zip unzip

$ yum install -y wget

$ yum install -y netstat

$ yum install -y lsof
```

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/OperatingSystem/linux.md