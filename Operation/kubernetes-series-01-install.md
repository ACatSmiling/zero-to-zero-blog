> *`Author: ACatSmiling`*
>
> *`Since: 2024-10-17`*

## 配置要求

服务器要求：

- k8s-master：192.168.1.120。
- k8s-node1：192.168.1.121。
- k8s-node1：192.168.1.122。

每台服务器最低配置：2 核、2G 内存、20G 硬盘。

> 使用 Hyper-V 时，注意配置动态内存的最小值：
>
> <img src="kubernetes-series-01-install/image-20240902213655950.png" alt="image-20240902213655950" style="zoom:80%;" />

## 操作系统

```shell
[root@k8s-master k8s]# cat /etc/centos-release
CentOS Linux release 7.9.2009 (Core)
[root@k8s-master k8s]# uname -r
3.10.0-1160.el7.x86_64
[root@k8s-master k8s]# uname -a
Linux k8s-master 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

## 设置 hostname

```shell
[root@k8s-master ~]# hostnamectl set-hostname k8s-master
```

- 修改 hostname 后，需要重启虚拟机。

## 设置 hosts

```shell
[root@k8s-master ~]# vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.1.120 k8s-master
192.168.1.121 k8s-node1
192.168.1.122 k8s-node2
```

## 关闭防火墙

```shell
# 查看防火墙状态
[root@k8s-master ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2024-08-27 23:20:58 CST; 21min ago
     Docs: man:firewalld(1)
 Main PID: 549 (firewalld)
   CGroup: /system.slice/firewalld.service
           └─549 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

Aug 27 23:20:58 master systemd[1]: Starting firewalld - dynamic firewall daemon...
Aug 27 23:20:58 master systemd[1]: Started firewalld - dynamic firewall daemon.
Aug 27 23:20:58 master firewalld[549]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure configuration option. It will... it now.
Hint: Some lines were ellipsized, use -l to show in full.
# 关闭防火墙
[root@k8s-master ~]# systemctl stop firewalld
# 禁止防火墙开机自启
[root@k8s-master ~]# systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
[root@k8s-master ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)

Aug 27 23:20:58 master systemd[1]: Starting firewalld - dynamic firewall daemon...
Aug 27 23:20:58 master systemd[1]: Started firewalld - dynamic firewall daemon.
Aug 27 23:20:58 master firewalld[549]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure configuration option. It will... it now.
Aug 27 23:42:27 master systemd[1]: Stopping firewalld - dynamic firewall daemon...
Aug 27 23:42:28 master systemd[1]: Stopped firewalld - dynamic firewall daemon.
Hint: Some lines were ellipsized, use -l to show in full.
```

> ```shell
> $ systemctl stop firewalld
> $ systemctl disable firewalld
> ```

## 关闭 swap

```shell
[root@k8s-master ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:           3950        2286         918           8         745        1437
Swap:          4095           0        4095

[root@k8s-master ~]# cat /etc/fstab 

#
# /etc/fstab
# Created by anaconda on Thu Aug 22 23:53:48 2024
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=f724baab-c349-497d-ba1f-da8f619fdf89 /                       xfs     defaults        0 0
UUID=F0B8-EAC5          /boot/efi               vfat    umask=0077,shortname=winnt 0 0
UUID=94ca6141-23b0-4eff-ab4f-ae74b1cfe406 swap                    swap    defaults        0 0
# 永久关闭 swap
[root@k8s-master ~]# sed -ri 's/.*swap.*/#&/' /etc/fstab
[root@k8s-master ~]# cat /etc/fstab 

#
# /etc/fstab
# Created by anaconda on Thu Aug 22 23:53:48 2024
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=f724baab-c349-497d-ba1f-da8f619fdf89 /                       xfs     defaults        0 0
UUID=F0B8-EAC5          /boot/efi               vfat    umask=0077,shortname=winnt 0 0
#UUID=94ca6141-23b0-4eff-ab4f-ae74b1cfe406 swap                    swap    defaults        0 0

# 关闭 swap 后，一定要重启虚拟机
[root@k8s-master ~]# reboot
[root@k8s-master ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:           3950         198        3671           8          79        3593
Swap:             0           0           0
```

- 临时关闭：`swapoff -a`。

>```shell
>$ sed -ri 's/.*swap.*/#&/' /etc/fstab
>$ reboot
>```

## 关闭 selinux

```shell
[root@k8s-master ~]# cat /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 


# 永久关闭
[root@k8s-master ~]# sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

[root@k8s-master ~]# cat /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 


```

- 临时关闭：`setenforce 0`。

>SELinux 是一个为 Linux 提供访问控制安全策略的安全模块，它通过定义和执行安全策略来限制进程对系统资源的访问，从而增强系统的安全性。然而，K8s 集群的某些组件或配置可能与 SELinux 的默认策略不兼容，导致安装或运行过程中出现权限问题。
>
>- 临时关闭
>
>```shell
># 此命令将 SELinux 设置为宽容模式（permissive mode），在这种模式下，SELinux 会记录违反策略的行为但不会阻止它们
>$ setenforce 0
>```
>
>- 永久关闭
>
>```shell
>$ sed -i 's/SELINUX=permissive/SELINUX=disabled/' /etc/sysconfig/selinux
>
>$ sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
>
>$ reboot
>```
>
>需要注意的是，关闭 SELinux 会降低系统的安全性。因此，在关闭 SELinux 之前，应该仔细评估潜在的安全风险，并确保已经采取了其他适当的安全措施来保护系统。
>
>**在 K8s 集群中，关闭 Swap 和 SELinux 是出于性能和稳定性方面的考虑。**
>
>为什么要关闭 Swap？
>
>- 性能问题：
> - Swap 内存通常比物理内存慢很多。当系统内存不足时，Linux 会将部分内存内容交换到磁盘上的 Swap 分区中，这会导致应用程序的性能显著下降。在 K8s 集群中，容器应用对性能有较高要求，频繁的 Swap 操作会严重影响这些应用的运行效率。
> - 关闭 Swap 可以避免这种性能损失，确保容器应用能够直接从物理内存中获取所需资源，从而提高整体性能。
>
>- 稳定性问题：
> - K8s 对容器运行环境的要求比较高，关闭 Swap 可以减少因为内存兑换（即内存与 Swap 之间的数据交换）引起的异常现象。这有助于确保集群的稳定性，避免因为内存问题导致的容器崩溃或系统不稳定。
>
>- 资源管理：
> - K8s 本身对内存的管理非常严格，它通过资源配额（Resource Quotas）和限制（Limits）来确保容器不会超出其分配的资源范围。如果启用 Swap，这些管理机制可能无法有效发挥作用，因为 Swap 允许容器在物理内存不足时继续运行，但性能会大幅下降。
>
>
>为什么要关闭 SELinux？
>
>- 兼容性问题：
> - SELinux 是一种安全增强型的 Linux 内核安全模块，它可以提供强大的访问控制机制。然而，在某些情况下，SELinux 可能会与 K8s 的某些组件或特性不兼容，导致集群运行不稳定或出现权限问题。
>
>- 简化部署和管理：
> - 关闭 SELinux 可以简化 K8s 的部署和管理过程。在没有 SELinux 的情况下，管理员可以更容易地配置和调试集群，而无需担心 SELinux 的安全策略可能会干扰集群的正常运行。
>
>- 安全风险：
> - 虽然 SELinux 可以增强系统的安全性，但在某些情况下，它也可能成为安全漏洞的源头。例如，如果 SELinux 的策略配置不当，可能会允许未授权的访问或操作。因此，在某些情况下，关闭 SELinux 可能是一种更安全的选择，特别是当管理员能够确保通过其他方式（如网络隔离、身份验证等）来保护系统时。
>
>
>**综上所述，关闭 Swap 和 SELinux 是 K8s 集群部署中的常见做法，旨在提高集群的性能和稳定性，并简化部署和管理过程。然而，这些决策也需要在安全性和性能之间做出权衡，并根据具体的业务需求和安全策略来确定是否适用。**

## 将桥接的 IPv4 流量传递到 iptables 的链

```shell
[root@k8s-master ~]# cat > /etc/sysctl.d/k8s.conf << EOF
> net.ipv4.ip_forward = 1
> net.bridge.bridge-nf-call-ip6tables = 1
> net.bridge.bridge-nf-call-iptables = 1
> vm.swappiness = 0
> EOF

# 生效配置
[root@k8s-master ~]# sysctl --system
* Applying /usr/lib/sysctl.d/00-system.conf ...
* Applying /usr/lib/sysctl.d/10-default-yama-scope.conf ...
kernel.yama.ptrace_scope = 0
* Applying /usr/lib/sysctl.d/50-default.conf ...
kernel.sysrq = 16
kernel.core_uses_pid = 1
kernel.kptr_restrict = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.promote_secondaries = 1
net.ipv4.conf.all.promote_secondaries = 1
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
* Applying /etc/sysctl.d/99-sysctl.conf ...
* Applying /etc/sysctl.d/k8s.conf ...
net.ipv4.ip_forward = 1
vm.swappiness = 0
* Applying /etc/sysctl.conf ...
```

>```shell
>$ cat > /etc/sysctl.d/k8s.conf << EOF
>net.ipv4.ip_forward = 1
>net.bridge.bridge-nf-call-ip6tables = 1
>net.bridge.bridge-nf-call-iptables = 1
>vm.swappiness = 0
>EOF
>
>$ sysctl --system
>```

## 时间同步

```shell
[root@k8s-master ~]# yum -y install ntpdate
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
Resolving Dependencies
--> Running transaction check
---> Package ntpdate.x86_64 0:4.2.6p5-29.el7.centos.2 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================================================
 Package                         Arch                           Version                                           Repository                    Size
=====================================================================================================================================================
Installing:
 ntpdate                         x86_64                         4.2.6p5-29.el7.centos.2                           base                          87 k

Transaction Summary
=====================================================================================================================================================
Install  1 Package

Total download size: 87 k
Installed size: 121 k
Downloading packages:
ntpdate-4.2.6p5-29.el7.centos.2.x86_64.rpm                                                                                    |  87 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : ntpdate-4.2.6p5-29.el7.centos.2.x86_64                                                                                            1/1 
  Verifying  : ntpdate-4.2.6p5-29.el7.centos.2.x86_64                                                                                            1/1 

Installed:
  ntpdate.x86_64 0:4.2.6p5-29.el7.centos.2                                                                                                           

Complete!
[root@k8s-master ~]# ntpdate time.windows.com
28 Aug 00:08:27 ntpdate[1077]: adjust time server 52.231.114.183 offset 0.248979 sec
```

> ```shell
> $ yum -y install ntpdate
> 
> $ ntpdate time.windows.com
> ```

## Docker 安装

> 安装过程略。

注意，Docker 在默认情况下使用 Vgroup Driver 为 cgroupfs，而 kubernetes 推荐使用 systemd 来替代 cgroupfs。

```shell
# 切换镜像源
[root@k8s-master ~]# wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
--2024-08-28 00:18:09--  https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
Resolving mirrors.aliyun.com (mirrors.aliyun.com)... 124.238.242.241, 117.68.48.82, 117.68.48.80, ...
Connecting to mirrors.aliyun.com (mirrors.aliyun.com)|124.238.242.241|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2081 (2.0K) [application/octet-stream]
Saving to: ‘/etc/yum.repos.d/docker-ce.repo’

100%[===========================================================================================================>] 2,081       --.-K/s   in 0s      

2024-08-28 00:18:09 (684 MB/s) - ‘/etc/yum.repos.d/docker-ce.repo’ saved [2081/2081]

# 查看当前镜像源中支持的 Docker 版本
[root@k8s-master ~]# yum list docker-ce --showduplicates
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
docker-ce-stable                                                                                                              | 3.5 kB  00:00:00     
(1/2): docker-ce-stable/7/x86_64/updateinfo                                                                                   |   55 B  00:00:00     
(2/2): docker-ce-stable/7/x86_64/primary_db                                                                                   | 152 kB  00:00:00     
Available Packages
docker-ce.x86_64                                               17.03.0.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.03.1.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.03.2.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.03.3.ce-1.el7                                                      docker-ce-stable
docker-ce.x86_64                                               17.06.0.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.06.1.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.06.2.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.09.0.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.09.1.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.12.0.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               17.12.1.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               18.03.0.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               18.03.1.ce-1.el7.centos                                               docker-ce-stable
docker-ce.x86_64                                               18.06.0.ce-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               18.06.1.ce-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               18.06.2.ce-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               18.06.3.ce-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:18.09.0-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.1-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.2-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.3-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.4-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.5-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.6-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.7-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.8-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:18.09.9-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.0-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.1-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.2-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.3-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.4-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.5-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.6-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.7-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.8-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.9-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:19.03.10-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:19.03.11-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:19.03.12-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:19.03.13-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:19.03.14-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:19.03.15-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.0-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.1-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.2-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.3-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.4-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.5-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.6-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.7-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.8-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.9-3.el7                                                       docker-ce-stable
docker-ce.x86_64                                               3:20.10.10-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.11-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.12-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.13-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.14-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.15-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.16-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.17-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.18-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.19-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.20-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.21-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.22-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.23-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:20.10.24-3.el7                                                      docker-ce-stable
docker-ce.x86_64                                               3:23.0.0-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.1-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.2-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.3-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.4-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.5-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:23.0.6-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.0-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.1-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.2-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.3-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.4-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.5-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.6-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.7-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.8-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:24.0.9-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.0-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.1-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.2-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.3-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.4-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:25.0.5-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.0.0-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.0.1-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.0.2-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.1.0-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.1.1-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.1.2-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.1.3-1.el7                                                        docker-ce-stable
docker-ce.x86_64                                               3:26.1.4-1.el7                                                        docker-ce-stable

# 安装特定版本的 docker-ce
[root@k8s-master ~]# yum -y install --setopt=obsoletes=0 docker-ce-18.06.3.ce-3.el7
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
Resolving Dependencies
--> Running transaction check
---> Package docker-ce.x86_64 0:18.06.3.ce-3.el7 will be installed
--> Processing Dependency: container-selinux >= 2.9 for package: docker-ce-18.06.3.ce-3.el7.x86_64
--> Processing Dependency: libseccomp >= 2.3 for package: docker-ce-18.06.3.ce-3.el7.x86_64
--> Processing Dependency: libcgroup for package: docker-ce-18.06.3.ce-3.el7.x86_64
--> Processing Dependency: libltdl.so.7()(64bit) for package: docker-ce-18.06.3.ce-3.el7.x86_64
--> Processing Dependency: libseccomp.so.2()(64bit) for package: docker-ce-18.06.3.ce-3.el7.x86_64
--> Running transaction check
---> Package container-selinux.noarch 2:2.119.2-1.911c772.el7_8 will be installed
--> Processing Dependency: policycoreutils-python for package: 2:container-selinux-2.119.2-1.911c772.el7_8.noarch
---> Package libcgroup.x86_64 0:0.41-21.el7 will be installed
---> Package libseccomp.x86_64 0:2.3.1-4.el7 will be installed
---> Package libtool-ltdl.x86_64 0:2.4.2-22.el7_3 will be installed
--> Running transaction check
---> Package policycoreutils-python.x86_64 0:2.5-34.el7 will be installed
--> Processing Dependency: setools-libs >= 3.3.8-4 for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libsemanage-python >= 2.5-14 for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: audit-libs-python >= 2.1.3-4 for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: python-IPy for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libqpol.so.1(VERS_1.4)(64bit) for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libqpol.so.1(VERS_1.2)(64bit) for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libapol.so.4(VERS_4.0)(64bit) for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: checkpolicy for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libqpol.so.1()(64bit) for package: policycoreutils-python-2.5-34.el7.x86_64
--> Processing Dependency: libapol.so.4()(64bit) for package: policycoreutils-python-2.5-34.el7.x86_64
--> Running transaction check
---> Package audit-libs-python.x86_64 0:2.8.5-4.el7 will be installed
---> Package checkpolicy.x86_64 0:2.5-8.el7 will be installed
---> Package libsemanage-python.x86_64 0:2.5-14.el7 will be installed
---> Package python-IPy.noarch 0:0.75-6.el7 will be installed
---> Package setools-libs.x86_64 0:3.3.8-4.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================================================
 Package                                 Arch                    Version                                     Repository                         Size
=====================================================================================================================================================
Installing:
 docker-ce                               x86_64                  18.06.3.ce-3.el7                            docker-ce-stable                   41 M
Installing for dependencies:
 audit-libs-python                       x86_64                  2.8.5-4.el7                                 base                               76 k
 checkpolicy                             x86_64                  2.5-8.el7                                   base                              295 k
 container-selinux                       noarch                  2:2.119.2-1.911c772.el7_8                   extras                             40 k
 libcgroup                               x86_64                  0.41-21.el7                                 base                               66 k
 libseccomp                              x86_64                  2.3.1-4.el7                                 base                               56 k
 libsemanage-python                      x86_64                  2.5-14.el7                                  base                              113 k
 libtool-ltdl                            x86_64                  2.4.2-22.el7_3                              base                               49 k
 policycoreutils-python                  x86_64                  2.5-34.el7                                  base                              457 k
 python-IPy                              noarch                  0.75-6.el7                                  base                               32 k
 setools-libs                            x86_64                  3.3.8-4.el7                                 base                              620 k

Transaction Summary
=====================================================================================================================================================
Install  1 Package (+10 Dependent packages)

Total download size: 42 M
Installed size: 174 M
Downloading packages:
(1/11): container-selinux-2.119.2-1.911c772.el7_8.noarch.rpm                                                                  |  40 kB  00:00:00     
(2/11): audit-libs-python-2.8.5-4.el7.x86_64.rpm                                                                              |  76 kB  00:00:00     
(3/11): libcgroup-0.41-21.el7.x86_64.rpm                                                                                      |  66 kB  00:00:00     
(4/11): libseccomp-2.3.1-4.el7.x86_64.rpm                                                                                     |  56 kB  00:00:00     
(5/11): checkpolicy-2.5-8.el7.x86_64.rpm                                                                                      | 295 kB  00:00:00     
(6/11): libtool-ltdl-2.4.2-22.el7_3.x86_64.rpm                                                                                |  49 kB  00:00:00     
(7/11): libsemanage-python-2.5-14.el7.x86_64.rpm                                                                              | 113 kB  00:00:00     
(8/11): python-IPy-0.75-6.el7.noarch.rpm                                                                                      |  32 kB  00:00:00     
(9/11): policycoreutils-python-2.5-34.el7.x86_64.rpm                                                                          | 457 kB  00:00:00     
(10/11): setools-libs-3.3.8-4.el7.x86_64.rpm                                                                                  | 620 kB  00:00:00     
warning: /var/cache/yum/x86_64/7/docker-ce-stable/packages/docker-ce-18.06.3.ce-3.el7.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 621e9f35: NOKEY
Public key for docker-ce-18.06.3.ce-3.el7.x86_64.rpm is not installed
(11/11): docker-ce-18.06.3.ce-3.el7.x86_64.rpm                                                                                |  41 MB  00:00:20     
-----------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                2.0 MB/s |  42 MB  00:00:20     
Retrieving key from https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
Importing GPG key 0x621E9F35:
 Userid     : "Docker Release (CE rpm) <docker@docker.com>"
 Fingerprint: 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
 From       : https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : libcgroup-0.41-21.el7.x86_64                                                                                                     1/11 
  Installing : setools-libs-3.3.8-4.el7.x86_64                                                                                                  2/11 
  Installing : audit-libs-python-2.8.5-4.el7.x86_64                                                                                             3/11 
  Installing : libseccomp-2.3.1-4.el7.x86_64                                                                                                    4/11 
  Installing : python-IPy-0.75-6.el7.noarch                                                                                                     5/11 
  Installing : libtool-ltdl-2.4.2-22.el7_3.x86_64                                                                                               6/11 
  Installing : libsemanage-python-2.5-14.el7.x86_64                                                                                             7/11 
  Installing : checkpolicy-2.5-8.el7.x86_64                                                                                                     8/11 
  Installing : policycoreutils-python-2.5-34.el7.x86_64                                                                                         9/11 
  Installing : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch                                                                              10/11 
setsebool:  SELinux is disabled.
  Installing : docker-ce-18.06.3.ce-3.el7.x86_64                                                                                               11/11 
  Verifying  : checkpolicy-2.5-8.el7.x86_64                                                                                                     1/11 
  Verifying  : libsemanage-python-2.5-14.el7.x86_64                                                                                             2/11 
  Verifying  : libtool-ltdl-2.4.2-22.el7_3.x86_64                                                                                               3/11 
  Verifying  : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch                                                                               4/11 
  Verifying  : python-IPy-0.75-6.el7.noarch                                                                                                     5/11 
  Verifying  : libseccomp-2.3.1-4.el7.x86_64                                                                                                    6/11 
  Verifying  : policycoreutils-python-2.5-34.el7.x86_64                                                                                         7/11 
  Verifying  : audit-libs-python-2.8.5-4.el7.x86_64                                                                                             8/11 
  Verifying  : docker-ce-18.06.3.ce-3.el7.x86_64                                                                                                9/11 
  Verifying  : setools-libs-3.3.8-4.el7.x86_64                                                                                                 10/11 
  Verifying  : libcgroup-0.41-21.el7.x86_64                                                                                                    11/11 

Installed:
  docker-ce.x86_64 0:18.06.3.ce-3.el7                                                                                                                

Dependency Installed:
  audit-libs-python.x86_64 0:2.8.5-4.el7      checkpolicy.x86_64 0:2.5-8.el7                  container-selinux.noarch 2:2.119.2-1.911c772.el7_8     
  libcgroup.x86_64 0:0.41-21.el7              libseccomp.x86_64 0:2.3.1-4.el7                 libsemanage-python.x86_64 0:2.5-14.el7                 
  libtool-ltdl.x86_64 0:2.4.2-22.el7_3        policycoreutils-python.x86_64 0:2.5-34.el7      python-IPy.noarch 0:0.75-6.el7                         
  setools-libs.x86_64 0:3.3.8-4.el7          

Complete!

# 添加一个配置文件
[root@k8s-master ~]# mkdir /etc/docker/
[root@k8s-master ~]# cat <<EOF> /etc/docker/daemon.json
> {
>     "exec-opts": ["native.cgroupdriver=systemd"],
>     "registry-mirrors": ["https://kn0t2bca.mirror.aliyuncs.com"]
> }
> EOF

# 生效配置
[root@k8s-master ~]# systemctl daemon-reload
[root@k8s-master ~]# systemctl restart docker
[root@k8s-master ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@k8s-master ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2024-08-28 23:50:07 CST; 59s ago
     Docs: https://docs.docker.com
 Main PID: 1272 (dockerd)
   CGroup: /system.slice/docker.service
           ├─1272 /usr/bin/dockerd
           └─1279 docker-containerd --config /var/run/docker/containerd/containerd.toml

Aug 28 23:50:06 k8s-master dockerd[1272]: time="2024-08-28T23:50:06.860979100+08:00" level=info msg="pickfirstBalancer: HandleSubConnState...ule=grpc
Aug 28 23:50:06 k8s-master dockerd[1272]: time="2024-08-28T23:50:06.861083300+08:00" level=info msg="pickfirstBalancer: HandleSubConnState...ule=grpc
Aug 28 23:50:06 k8s-master dockerd[1272]: time="2024-08-28T23:50:06.861095500+08:00" level=info msg="Loading containers: start."
Aug 28 23:50:06 k8s-master dockerd[1272]: time="2024-08-28T23:50:06.971279100+08:00" level=info msg="Default bridge (docker0) is assigned ...address"
Aug 28 23:50:07 k8s-master dockerd[1272]: time="2024-08-28T23:50:07.007541200+08:00" level=info msg="Loading containers: done."
Aug 28 23:50:07 k8s-master dockerd[1272]: time="2024-08-28T23:50:07.027280100+08:00" level=info msg="Docker daemon" commit=d7080c1 graphdr....06.3-ce
Aug 28 23:50:07 k8s-master dockerd[1272]: time="2024-08-28T23:50:07.027418100+08:00" level=info msg="Daemon has completed initialization"
Aug 28 23:50:07 k8s-master dockerd[1272]: time="2024-08-28T23:50:07.044033700+08:00" level=warning msg="Could not register builder git sou...n $PATH"
Aug 28 23:50:07 k8s-master dockerd[1272]: time="2024-08-28T23:50:07.050779700+08:00" level=info msg="API listen on /var/run/docker.sock"
Aug 28 23:50:07 k8s-master systemd[1]: Started Docker Application Container Engine.
Hint: Some lines were ellipsized, use -l to show in full.
```

> Docker 在默认情况下使用 Vgroup Driver 为 cgroupfs，而 kubernetes 推荐使用 systemd 来替代 cgroupfs。

>```shell
>$ wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
>
>$ yum list docker-ce --showduplicates
>
>$ yum -y install --setopt=obsoletes=0 docker-ce-18.06.3.ce-3.el7
>
>$ mkdir /etc/docker/
>$ cat <<EOF> /etc/docker/daemon.json
>{
>	"exec-opts": ["native.cgroupdriver=systemd"],
>	"registry-mirrors": ["https://kn0t2bca.mirror.aliyuncs.com"]
>}
>EOF
>
>$ systemctl daemon-reload
>$ systemctl restart docker
>$ systemctl enable docker
>$ systemctl status docker
>```

## 配置 kubernetes 镜像源

```shell
[root@k8s-master yum.repos.d]# cat > /etc/yum.repos.d/kubernetes.repo << EOF
> [kubernetes]
> name=Kubernetes
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
> enabled=1
> gpgcheck=0
> repo_gpgcheck=0
> gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
> EOF
```

> ```shell
> $ cat > /etc/yum.repos.d/kubernetes.repo << EOF
> [kubernetes]
> name=Kubernetes
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
> enabled=1
> gpgcheck=0
> repo_gpgcheck=0
> gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
> EOF
> ```

## 安装 kubeadm、kubelet 和 kubectl

```shell
[root@k8s-master yum.repos.d]# yum install -y --setopt=obsoletes=0 kubeadm-1.23.6 kubelet-1.23.6 kubectl-1.23.6
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
base                                                                                                                          | 3.6 kB  00:00:00     
docker-ce-stable                                                                                                              | 3.5 kB  00:00:00     
extras                                                                                                                        | 2.9 kB  00:00:00     
kubernetes                                                                                                                    | 1.4 kB  00:00:00     
updates                                                                                                                       | 2.9 kB  00:00:00     
kubernetes/primary                                                                                                            | 137 kB  00:00:00     
kubernetes                                                                                                                                 1022/1022
Resolving Dependencies
--> Running transaction check
---> Package kubeadm.x86_64 0:1.23.6-0 will be installed
--> Processing Dependency: kubernetes-cni >= 0.8.6 for package: kubeadm-1.23.6-0.x86_64
--> Processing Dependency: cri-tools >= 1.19.0 for package: kubeadm-1.23.6-0.x86_64
---> Package kubectl.x86_64 0:1.23.6-0 will be installed
---> Package kubelet.x86_64 0:1.23.6-0 will be installed
--> Processing Dependency: socat for package: kubelet-1.23.6-0.x86_64
--> Processing Dependency: conntrack for package: kubelet-1.23.6-0.x86_64
--> Running transaction check
---> Package conntrack-tools.x86_64 0:1.4.4-7.el7 will be installed
--> Processing Dependency: libnetfilter_cttimeout.so.1(LIBNETFILTER_CTTIMEOUT_1.1)(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
--> Processing Dependency: libnetfilter_cttimeout.so.1(LIBNETFILTER_CTTIMEOUT_1.0)(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
--> Processing Dependency: libnetfilter_cthelper.so.0(LIBNETFILTER_CTHELPER_1.0)(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
--> Processing Dependency: libnetfilter_queue.so.1()(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
--> Processing Dependency: libnetfilter_cttimeout.so.1()(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
--> Processing Dependency: libnetfilter_cthelper.so.0()(64bit) for package: conntrack-tools-1.4.4-7.el7.x86_64
---> Package cri-tools.x86_64 0:1.26.0-0 will be installed
---> Package kubernetes-cni.x86_64 0:1.2.0-0 will be installed
---> Package socat.x86_64 0:1.7.3.2-2.el7 will be installed
--> Running transaction check
---> Package libnetfilter_cthelper.x86_64 0:1.0.0-11.el7 will be installed
---> Package libnetfilter_cttimeout.x86_64 0:1.0.0-7.el7 will be installed
---> Package libnetfilter_queue.x86_64 0:1.0.2-2.el7_2 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================================================
 Package                                     Arch                        Version                               Repository                       Size
=====================================================================================================================================================
Installing:
 kubeadm                                     x86_64                      1.23.6-0                              kubernetes                      9.0 M
 kubectl                                     x86_64                      1.23.6-0                              kubernetes                      9.5 M
 kubelet                                     x86_64                      1.23.6-0                              kubernetes                       21 M
Installing for dependencies:
 conntrack-tools                             x86_64                      1.4.4-7.el7                           base                            187 k
 cri-tools                                   x86_64                      1.26.0-0                              kubernetes                      8.6 M
 kubernetes-cni                              x86_64                      1.2.0-0                               kubernetes                       17 M
 libnetfilter_cthelper                       x86_64                      1.0.0-11.el7                          base                             18 k
 libnetfilter_cttimeout                      x86_64                      1.0.0-7.el7                           base                             18 k
 libnetfilter_queue                          x86_64                      1.0.2-2.el7_2                         base                             23 k
 socat                                       x86_64                      1.7.3.2-2.el7                         base                            290 k

Transaction Summary
=====================================================================================================================================================
Install  3 Packages (+7 Dependent packages)

Total download size: 65 M
Installed size: 296 M
Downloading packages:
(1/10): conntrack-tools-1.4.4-7.el7.x86_64.rpm                                                                                | 187 kB  00:00:00     
(2/10): 89104c7beafab5f04d6789e5425963fc8f91ba9711c9603f1ad89003cdea4fe4-kubeadm-1.23.6-0.x86_64.rpm                          | 9.0 MB  00:00:02     
(3/10): 3f5ba2b53701ac9102ea7c7ab2ca6616a8cd5966591a77577585fde1c434ef74-cri-tools-1.26.0-0.x86_64.rpm                        | 8.6 MB  00:00:02     
(4/10): 868c4a6ee448d1e8488938812a19a991b5132c81de511cd737d93493b98451cc-kubectl-1.23.6-0.x86_64.rpm                          | 9.5 MB  00:00:02     
(5/10): libnetfilter_cthelper-1.0.0-11.el7.x86_64.rpm                                                                         |  18 kB  00:00:00     
(6/10): libnetfilter_cttimeout-1.0.0-7.el7.x86_64.rpm                                                                         |  18 kB  00:00:00     
(7/10): libnetfilter_queue-1.0.2-2.el7_2.x86_64.rpm                                                                           |  23 kB  00:00:00     
(8/10): socat-1.7.3.2-2.el7.x86_64.rpm                                                                                        | 290 kB  00:00:00     
(9/10): 68a98b2ae673eef4a5ddbf1f3c830db0df8fbb888e035aea6054677d88f8a8bc-kubelet-1.23.6-0.x86_64.rpm                          |  21 MB  00:00:05     
(10/10): 0f2a2afd740d476ad77c508847bad1f559afc2425816c1f2ce4432a62dfe0b9d-kubernetes-cni-1.2.0-0.x86_64.rpm                   |  17 MB  00:00:04     
-----------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                7.3 MB/s |  65 MB  00:00:08     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : kubectl-1.23.6-0.x86_64                                                                                                          1/10 
  Installing : libnetfilter_cthelper-1.0.0-11.el7.x86_64                                                                                        2/10 
  Installing : socat-1.7.3.2-2.el7.x86_64                                                                                                       3/10 
  Installing : libnetfilter_cttimeout-1.0.0-7.el7.x86_64                                                                                        4/10 
  Installing : cri-tools-1.26.0-0.x86_64                                                                                                        5/10 
  Installing : libnetfilter_queue-1.0.2-2.el7_2.x86_64                                                                                          6/10 
  Installing : conntrack-tools-1.4.4-7.el7.x86_64                                                                                               7/10 
  Installing : kubernetes-cni-1.2.0-0.x86_64                                                                                                    8/10 
  Installing : kubelet-1.23.6-0.x86_64                                                                                                          9/10 
  Installing : kubeadm-1.23.6-0.x86_64                                                                                                         10/10 
  Verifying  : kubeadm-1.23.6-0.x86_64                                                                                                          1/10 
  Verifying  : conntrack-tools-1.4.4-7.el7.x86_64                                                                                               2/10 
  Verifying  : libnetfilter_queue-1.0.2-2.el7_2.x86_64                                                                                          3/10 
  Verifying  : cri-tools-1.26.0-0.x86_64                                                                                                        4/10 
  Verifying  : kubernetes-cni-1.2.0-0.x86_64                                                                                                    5/10 
  Verifying  : kubelet-1.23.6-0.x86_64                                                                                                          6/10 
  Verifying  : libnetfilter_cttimeout-1.0.0-7.el7.x86_64                                                                                        7/10 
  Verifying  : socat-1.7.3.2-2.el7.x86_64                                                                                                       8/10 
  Verifying  : libnetfilter_cthelper-1.0.0-11.el7.x86_64                                                                                        9/10 
  Verifying  : kubectl-1.23.6-0.x86_64                                                                                                         10/10 

Installed:
  kubeadm.x86_64 0:1.23.6-0                        kubectl.x86_64 0:1.23.6-0                        kubelet.x86_64 0:1.23.6-0                       

Dependency Installed:
  conntrack-tools.x86_64 0:1.4.4-7.el7              cri-tools.x86_64 0:1.26.0-0                       kubernetes-cni.x86_64 0:1.2.0-0                
  libnetfilter_cthelper.x86_64 0:1.0.0-11.el7       libnetfilter_cttimeout.x86_64 0:1.0.0-7.el7       libnetfilter_queue.x86_64 0:1.0.2-2.el7_2      
  socat.x86_64 0:1.7.3.2-2.el7                     

Complete!
```

> ```shell
> $ yum install -y --setopt=obsoletes=0 kubeadm-1.23.6 kubelet-1.23.6 kubectl-1.23.6
> ```

## 设置 kubelet 开机自启

```shell
[root@k8s-master yum.repos.d]# systemctl enable kubelet
Created symlink from /etc/systemd/system/multi-user.target.wants/kubelet.service to /usr/lib/systemd/system/kubelet.service.
[root@k8s-master yum.repos.d]# systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
   Loaded: loaded (/usr/lib/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: inactive (dead)
     Docs: https://kubernetes.io/docs/
```

> ```shell
> $ systemctl enable kubelet
> $ systemctl status kubelet
> ```

## 集群 Master 节点初始化

```shell
# 此操作只在 master 节点执行
[root@k8s-master yum.repos.d]# kubeadm init \
> --apiserver-advertise-address=192.168.1.120 \
> --image-repository registry.aliyuncs.com/google_containers \
> --kubernetes-version=v1.23.6 \
> --service-cidr=10.96.0.0/12 \
> --pod-network-cidr=10.244.0.0/16
[init] Using Kubernetes version: v1.23.6
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.1.120]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master localhost] and IPs [192.168.1.120 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master localhost] and IPs [192.168.1.120 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 6.504656 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.23" in namespace kube-system with the configuration for the kubelets in the cluster
NOTE: The "kubelet-config-1.23" naming of the kubelet ConfigMap is deprecated. Once the UnversionedKubeletConfigMap feature gate graduates to Beta the default name will become just "kubelet-config". Kubeadm upgrade will handle this transition transparently.
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node k8s-master as control-plane by adding the labels: [node-role.kubernetes.io/master(deprecated) node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node k8s-master as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: rczo3l.hgi643ox3vzw4ttr
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.1.120:6443 --token rczo3l.hgi643ox3vzw4ttr \
        --discovery-token-ca-cert-hash sha256:714d757f758bbf794c88d48078fcceaca6993a71c32a2e0f131f17ded0099f75
```

>注意结合自己的实际情况，修改对应的参数配置：
>
>```shell
>$ kubeadm init \
>--apiserver-advertise-address=192.168.1.120 \
>--image-repository registry.aliyuncs.com/google_containers \
>--kubernetes-version=v1.23.6 \
>--service-cidr=10.96.0.0/12 \
>--pod-network-cidr=10.244.0.0/16
>```
>
>- `apiserver-advertise-address`：指定 Api Server 地址。
>- `image-repository`：镜像仓库地址。
>- `kubernetes-version`：kubernetes 版本。
>- `service-cidr`：Service 的网络 IP 地址段。
>- `pod-network-cidr`：Pod 的网络 IP 地址段。

当看到`Your Kubernetes control-plane has initialized successfully!`提示，说明集群 Master 节点初始化成功，按照提示，依次执行以下命令：

```shell
[root@k8s-master ~]# mkdir -p $HOME/.kube
[root@k8s-master ~]# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
[root@k8s-master ~]# sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Master 节点初始化成功后，可以查看启动的 Docker 容器：

```shell
[root@k8s-master ~]# docker ps
CONTAINER ID        IMAGE                                               COMMAND                  CREATED             STATUS              PORTS               NAMES
516411b3e4f9        df7b72818ad2                                        "kube-controller-man…"   About an hour ago   Up About an hour                        k8s_kube-controller-manager_kube-controller-manager-k8s-master_kube-system_cf39bc9bfe4da000c1780f37274acf68_3
2ed3b58d0c11        595f327f224a                                        "kube-scheduler --au…"   About an hour ago   Up About an hour                        k8s_kube-scheduler_kube-scheduler-k8s-master_kube-system_ad30b41672979e80f74f72181c1c9762_3
85de5232c506        4c0375452406                                        "/usr/local/bin/kube…"   2 hours ago         Up 2 hours                              k8s_kube-proxy_kube-proxy-m2nbx_kube-system_199d9d71-dd85-4363-8c46-addd357aac3b_1
9394d7715a75        registry.aliyuncs.com/google_containers/pause:3.6   "/pause"                 2 hours ago         Up 2 hours                              k8s_POD_kube-proxy-m2nbx_kube-system_199d9d71-dd85-4363-8c46-addd357aac3b_1
a4c2a7a80eb7        8fa62c12256d                                        "kube-apiserver --ad…"   2 hours ago         Up 2 hours                              k8s_kube-apiserver_kube-apiserver-k8s-master_kube-system_f844c52da54beeb845dab75e338dce7a_1
82c2e3320e5f        25f8c7f3da61                                        "etcd --advertise-cl…"   2 hours ago         Up 2 hours                              k8s_etcd_etcd-k8s-master_kube-system_2f6b828b40e9edd50c7cb676b0b4bacf_1
84f3a53a6139        registry.aliyuncs.com/google_containers/pause:3.6   "/pause"                 2 hours ago         Up 2 hours                              k8s_POD_kube-scheduler-k8s-master_kube-system_ad30b41672979e80f74f72181c1c9762_1
ca132ea49edc        registry.aliyuncs.com/google_containers/pause:3.6   "/pause"                 2 hours ago         Up 2 hours                              k8s_POD_kube-apiserver-k8s-master_kube-system_f844c52da54beeb845dab75e338dce7a_1
848d64a5403d        registry.aliyuncs.com/google_containers/pause:3.6   "/pause"                 2 hours ago         Up 2 hours                              k8s_POD_kube-controller-manager-k8s-master_kube-system_cf39bc9bfe4da000c1780f37274acf68_1
435f1caea216        registry.aliyuncs.com/google_containers/pause:3.6   "/pause"                 2 hours ago         Up 2 hours                              k8s_POD_etcd-k8s-master_kube-system_2f6b828b40e9edd50c7cb676b0b4bacf_1
```

## 集群 Node 节点 join

在两个 Node 节点，执行以下命令：

```shell
[root@k8s-node1 ~]# kubeadm join 192.168.1.120:6443 --token y4a0gj.5iaxcci7uqkdjcf0 --discovery-token-ca-cert-hash sha256:714d757f758bbf794c88d48078fcceaca6993a71c32a2e0f131f17ded0099f75
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```

- `--toke`：Master 节点初次 init 时，在 init 成功后，会在最后输出 Node 节点 join 的命令。在后续操作时，如果没有 token，可以通过以下方式获取：

  ```shell
  # 查看没有过期的 token 列表
  [root@k8s-master ~]# kubeadm token list
  
  # 重新申请 token
  [root@k8s-master ~]# kubeadm token create
  y4a0gj.5iaxcci7uqkdjcf0
  [root@k8s-master ~]# kubeadm token list
  TOKEN                     TTL         EXPIRES                USAGES                   DESCRIPTION                                                EXTRA GROUPS
  y4a0gj.5iaxcci7uqkdjcf0   23h         2024-09-05T15:40:56Z   authentication,signing   <none>                                                     system:bootstrappers:kubeadm:default-node-token
  ```

- `--discovery-token-ca-cert-hash`：SSL 证书对应的 Hash 值。同样的，Master 节点初次 init 时，也会输出，如果后续没有了，通过以下命令获取，再将获得的值，拼接上 "sha256:"，即可获得 Hash 值为 "sha256:714d757f758bbf794c88d48078fcceaca6993a71c32a2e0f131f17ded0099f75"。

  ```shell
  $ [root@k8s-master ~]# openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl sha256 -hex | sed 's/^.* //'
  714d757f758bbf794c88d48078fcceaca6993a71c32a2e0f131f17ded0099f75
  ```

  - `openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt`：从 kubernetes 的 CA 证书（位于`/etc/kubernetes/pki/ca.crt`）中提取公钥。
  - `| openssl rsa -pubin -outform der 2>/dev/null`：将公钥转换为 DER 格式。2>/dev/null 用于重定向错误输出，以保持命令行整洁。
  - `| openssl sha256 -hex`：对 DER 格式的公钥进行 SHA-256 哈希计算，并以十六进制形式输出。
  - `| sed 's/^.* //'`：使用 sed 工具删除输出中的任何前导文本，只留下哈希值。

> 如果是 Ubuntu 系统，使用以下命令：
>
> ```shell
> $ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
> ```

此时，可以查看 Node 节点上启动的容器：

```shell
[root@k8s-node1 yum.repos.d]# docker ps
CONTAINER ID        IMAGE                                                COMMAND                  CREATED             STATUS              PORTS               NAMES
6caa9ae8a696        registry.aliyuncs.com/google_containers/kube-proxy   "/usr/local/bin/kube…"   11 minutes ago      Up 11 minutes                           k8s_kube-proxy_kube-proxy-6xntx_kube-system_a0a43318-a317-46a8-9a01-a510218b93f7_0
58126cacdd00        registry.aliyuncs.com/google_containers/pause:3.6    "/pause"                 11 minutes ago      Up 11 minutes                           k8s_POD_kube-proxy-6xntx_kube-system_a0a43318-a317-46a8-9a01-a510218b93f7_0
```

同时，在 Master 节点上，查看集群的状态，仍处于 NotReady 的状态：

```shell
[root@k8s-master ~]# kubectl get nodes
NAME         STATUS     ROLES                  AGE     VERSION
k8s-master   NotReady   control-plane,master   6d23h   v1.23.6
k8s-node1    NotReady   <none>                 56s     v1.23.6
k8s-node2    NotReady   <none>                 53s     v1.23.6
```

## 集群 Master 节点安装网络插件

在 Node 节点 join 之后，执行以下命令查看 Component 和 Pod 的状态：

```shell
[root@k8s-master ~]# kubectl get componentstatus
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE                         ERROR
controller-manager   Healthy   ok                              
scheduler            Healthy   ok                              
etcd-0               Healthy   {"health":"true","reason":""}

[root@k8s-master ~]# kubectl get pods -n kube-system
NAME                                 READY   STATUS    RESTARTS       AGE
coredns-6d8c4cb4d-7m22g              0/1     Pending   0              7d23h
coredns-6d8c4cb4d-q22dm              0/1     Pending   0              7d23h
etcd-k8s-master                      1/1     Running   4 (23h ago)    7d23h
kube-apiserver-k8s-master            1/1     Running   4 (23h ago)    7d23h
kube-controller-manager-k8s-master   1/1     Running   7 (105m ago)   7d23h
kube-proxy-6xntx                     1/1     Running   1 (23h ago)    23h
kube-proxy-m2nbx                     1/1     Running   5 (105m ago)   7d23h
kube-proxy-vzsp7                     1/1     Running   1 (23h ago)    23h
kube-scheduler-k8s-master            1/1     Running   6 (23h ago)    7d23h
```

可以看到，Component 的状态都是 Healthy，说明集群是正常的，但是 "coredns-6d8c4cb4d-7m22g" 和 "coredns-6d8c4cb4d-q22dm" 都是 Pending 状态，造成这种现象的原因，就是因为网络。因此，下面在 Master 节点上安装网络插件。

```shell
$ wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

如果无法通过 wget 下载，可以直接访问 https://github.com/flannel-io/flannel/tree/master/Documentation/kube-flannel.yml，复制文件内容，然后在 /opt/k8s 路径下，创建 kube-flannel.yml 文件，再将复制的内容粘贴。

```shell
[root@k8s-master k8s]# pwd
/opt/k8s
[root@k8s-master k8s]# ls
kube-flannel.yml
# 需要下载的镜像
[root@k8s-master k8s]# grep image kube-flannel.yml 
        image: docker.io/flannel/flannel-cni-plugin:v1.5.1-flannel2
        image: docker.io/flannel/flannel:v0.25.6
        image: docker.io/flannel/flannel:v0.25.6
        
# 将 docker.io 替换为空，否则到官方下载镜像，可能很慢
[root@k8s-master k8s]# sed -i 's#docker.io/##g' kube-flannel.yml 
[root@k8s-master k8s]# grep image kube-flannel.yml 
        image: flannel/flannel-cni-plugin:v1.5.1-flannel2
        image: flannel/flannel:v0.25.6
        image: flannel/flannel:v0.25.6
        
# 应用网络插件
[root@k8s-master k8s]# kubectl apply -f kube-flannel.yml
namespace/kube-flannel created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds created
```

等待一段时间，再次查看 Pod 的状态，可以看到，"coredns-6d8c4cb4d-7m22g" 和 "coredns-6d8c4cb4d-q22dm" 都是 Running 的状态：

```shell
[root@k8s-master ~]# kubectl get pod -n kube-system
NAME                                 READY   STATUS    RESTARTS       AGE
coredns-6d8c4cb4d-7m22g              1/1     Running   0              7d23h
coredns-6d8c4cb4d-q22dm              1/1     Running   0              7d23h
etcd-k8s-master                      1/1     Running   4 (23h ago)    7d23h
kube-apiserver-k8s-master            1/1     Running   4 (23h ago)    7d23h
kube-controller-manager-k8s-master   1/1     Running   7 (129m ago)   7d23h
kube-proxy-6xntx                     1/1     Running   1 (23h ago)    23h
kube-proxy-m2nbx                     1/1     Running   5 (129m ago)   7d23h
kube-proxy-vzsp7                     1/1     Running   1 (23h ago)    23h
kube-scheduler-k8s-master            1/1     Running   6 (23h ago)    7d23h
```

> 如果需要查看某个 Pod 的详细信息，可以使用以下命令：
>
> ```shell
> $ kubectl describe pod coredns-6d8c4cb4d-7m22g -n kube-system
> ```

> 此处使用的是 Flannel 网络插件，也可以使用 CNI 网路插件，访问 https://calico-v3-25.netlify.app/archive/v3.25/manifests/calico.yaml，复制文件内容，然后在 /opt/k8s 路径下，创建 calico.yaml，再将复制的内容粘贴。
>
> 1. 修改 CALICO_IPV4POOL_CIDR 配置，该配置默认是注释掉的，如果不是注释掉的，将其修改为与 Master 节点 init 时的 "--pod-network-cidr=10.244.0.0/16" 保持一致。
>
>    <img src="kubernetes-series-01-install/image-20240906230815028.png" alt="image-20240906230815028" style="zoom:80%;" />
>
> 2. 将文件中需要下载的镜像前面的 docker.io 替换为空。
>
> 3. 应用 CNI 插件，使用命令`kubectl apply -f calico.yaml`。

## 查看集群状态

使用以下命令，查看集群状态，当 STATUS 全部为 Ready 时，才可继续后面的操作（此过程可能需耗费较长时间）：

```shell
[root@k8s-master ~]# kubectl get nodes
NAME         STATUS   ROLES                  AGE     VERSION
k8s-master   Ready    control-plane,master   7d23h   v1.23.6
k8s-node1    Ready    <none>                 24h     v1.23.6
k8s-node2    Ready    <none>                 24h     v1.23.6
```

>如果 Flannel 需检查网络情况，重新进行如下操作：
>
>`kubectl delete -f kube-flannel.yml` ---> 再次下载文件 ---> `kubectl apply -f kube-flannel.yml`。

## 测试 kubenetes 集群

```shell
# 创建部署一个 Nginx 服务
[root@k8s-master ~]# kubectl create deployment nginx --image=nginx
deployment.apps/nginx created
# 暴露容器内的端口
[root@k8s-master ~]# kubectl expose deployment nginx --port=80 --type=NodePort
service/nginx exposed
# 查看 Pod 以及服务信息，容器内端口 80，对应宿主机的端口 31173
[root@k8s-master ~]# kubectl get pod,svc
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-85b98978db-j4sjq   1/1     Running   0          10m

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        8d
service/nginx        NodePort    10.98.189.130   <none>        80:31173/TCP   10m
```

查看 Nginx 服务：

```shell
[root@k8s-master ~]# curl 192.168.1.120:31173
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

- "curl 192.168.1.121:31173" 和 "curl 192.168.1.122:31173" 具有相同的效果。

页面访问 192.168.1.120:31173：

<img src="kubernetes-series-01-install/image-20240907192611766.png" alt="image-20240907192611766" style="zoom:80%;" />

- 访问 192.168.1.121:31173 和 192.168.1.122:31173 具有相同的效果。

## 命令行工具 kubectl

kubectl 是使用 kubernetes API 与 kubernetes 集群进行通信的命令行工具。

kubectl 工具，默认只在 Master 节点上有效，在 Node 节点上执行无效：

```shell
[root@k8s-node1 ~]# kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

其原因是，在 Master 节点上，有用户认证相关的设置，并且 kubectl 工具调用时，知道调用的 Api 的服务器地址（https://192.168.1.120:6443）：

```shell
[root@k8s-master .kube]# pwd
/root/.kube
[root@k8s-master .kube]# cat config 
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJME1EZ3lPREUyTVRReE5Gb1hEVE0wTURneU5qRTJNVFF4TkZvd0ZURVRNQkVHQTFVRQpBeE1LY99886776SnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBS01rCjlmMWRYY2k2TUc5ZkpBUVN2Y1JqM2QwaWk1b0NockcyNGxUSmJha3c3dzV4TVhmUU9qM21FdUFROVkvYkpacnYKaytSUGVvSVlsdUYzbytPbHdWSktRLzBvbnZVbXM2YTFCMi9MRmVLNTRmUGllcXVmSVZDMElGalhvQVBiRTZOZQphc1lRbjV0SDQ2MDV3TjA3OFNHVzk4L1JuVjlkYTlDenEvb1BTYmNpUTRnbU1UYm0rSzU2eTJ0MWdOM0krUW91ClVHaFlucWZJa0VZTzNsTm1VbWtpOFUzaWNHaXhHWkVRYjA0UVowQ0JEck14MGV6anRWS1hrbVFMWW9zVjlhOFcKM1Bkb3AySXlqdkVCVTZxR001aE9BVlA3QUcrQ1hDSTVCVDBxVm1aeXVzYXVuekVnaTVOcWhKZ1hQZGdMWXNpUQprY2Rzc1lyWkFsRnhoenFTdGhzQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZQNlQ4aE5aTnMrcjUzWm1Cd3Q0UURvOVJIa0ZNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBQWg0QWZKTnU1YWxLbVJrSmJCOAppZ2wzZm53YVNkTXZCVUFFaE5JYWt0NytTWGZKdGNnLy9zanpPYlhIQVVDcHgrbnNualhVUW40Uzl1OW9IQlBOCnFUbGxSelBDZ0U4MXRRMnBVekU0T3BGSDQwblRucjYzRWtsbGNoZFU0QzlyMXcwbDJLS296UmJaV3ZCajBZa20KNmh3SlpzVzZyMW9iUjI2VzlFcXpLUXFweWRVcHZFR3lUcDk4YXBsTXFKazNlUS9hYkVOS1k3TW0vNll2K3FicwpCc1dSTVJuMEhzbTZ0dWNlZ21GOFNOZUxvSU5SdExSc1Y4VWZTU0F6WmJTSTVjY1VSTDFycGp2c0NBZUFkWGdECkh4a2l1Ny9IRW5wNmpoM05mRkZKZ0FIL2hvdW52RS9hODk3VmduTHBpcE8xZjBFKzR4YUFMNC9tVWozVm9DRTQKUVRBPQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://192.168.1.120:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJZWpxb29NcVhGS3d3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBNE1qZ3hOakUwTVRSYUZ3MHlOVEE0TWpneE5qRTBNVFZhTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXVVbys0dkhnZmNLeGxmdWMKeHkhj56hfh0ZHd1JCU2F6MVRaMmdKV1dUeEtrTE5XemtET3BjWk5oUkdlVGozaVlvdjAvdWxkZGJzdml2LwpSQ2tjUTZrWGdVaXdGSytNUkdKR3dKbmFCY0QwSFdzc0tqMUh6RjVYMm5kbUFvNXhsTThQcms0TWYyREtnbjBHClRiaG5uc1ZERk9tWG5JSTRucFJFMXA4SFIrQUQvTkdDOXhIbVRkV0szSVZOWERvYXhIZXY3VmoxVHk1TkNUWEwKWmQraEJEa0h1NHRwTzM0bGpuWlU4elBua0ljcHJMeU5wdjNCb2UvUjR1WXZIdkFrUG1qYXRUWHMvaGc5QXVSegpKcDcrODFiR3Voc3A2cVZBbzlwOUJsNHovck1XUk9qR3JxdDNEWnFKU2tVTVpqVWdaZHFyY0kzNHJiTWJySlMwCmtqdUpHd0lEQVFBQm8xWXdWREFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RBWURWUjBUQVFIL0JBSXdBREFmQmdOVkhTTUVHREFXZ0JUK2svSVRXVGJQcStkMlpnY0xlRUE2UFVSNQpCVEFOQmdrcWhraUc5dzBCQVFzRkFBT0NBUUVBbnNMVzNpaU9XMTBFbEtVVXlaM3NIRkpzeW1oSUxpOVpJdmE2ClRDVmh5S3huZVEyY1daKzRFTWZJT3RJTi92WitMak96WmVYbzMzRDlybUdBOUtSMi9udTJ6b2pmbzZEdnJLL2YKMVVPOGFWNnlHMFBoeFlCT1dXeUhNczFzd1dzNU8yY01PRGR5TU9JcUIzZHJ3eVEzWkZrblgxa3UwZ1NqeVdheApWR2VQN1grSFowZ1RpSUtGWFdaVjdHWCtlSEdQZHBIZVBkR0doMXdGbUJCcTRVOUdoZERoTVBtenNZMkFpMjJrClpZOTFxYmtucGF4blEvNlRBN0lwdkcya0wwT2N6ZFpoQXh0NWZFRWVmTDZ2M1VVM1c4c3lYTnd0M3ROamJlQk4KZTdRSitaQjcwYjdQQi9lSFZQTmlxRThRdVYySzlIQjRUS3ZkdlNJZTZ1VHN6VXJDZ0E9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcFFJQkFBS0NBUUVBdVVvKzR2SGdmY0t4bGZ1Y3hySUhIOEM0SEtGR3dSQlNhejFUWjJnSldXVHhLa0xOCld6a0RPcGNaTmhSR2VUajNpWW92MC91bGRkYnN2aXYvUkNrY1E2a1hnVWl3RksrTVJHSkd3Sm5hQmNEMEhXc3MKS2oxSHpGNVgybmRtQW81eGxNOFByazRNZjJES2duMEdUYmhubnNWREZPbVhuSUk0bnBSRTFwOEhSK0FEL05HQwo5eEhtVGRXSzNJVk5YRG9heEhldjdWajFUeTVOQ1RYTFpkK2hCRGtIdTR0cE8zNGxqblpVOHpQbmtJY3ByTHlOCnB2M0JvZS9SNHVZdkh2QWtQbWphdFRYcy9oZzlBdVJ6SnA3KzgxYkd1aHNwNnFWQW85cDlCbDR6L3JNV1JPakcKcnF0M0RacUpTa1VNWmpVZ1pkcXJjSTM0cmJNYnJKUzBranVKR3dJREFRQUJBb0lCQVFDTkdONitqeFkyYmpZeApVa05XZzJjdFpPSk8ydms0TjZlcmhpMm5CdkJucEppSmFBbGRPQk1mWU1TUUMreUdqenpnL2R2aC96VkdnUDRTCjZ3b2Q2M2hjaGIwaWRDbXg5dVJIaHRiOS82cW95d0NhRG15NVZhVUJHYTZvN0ZkQUJ4eXpCdUtZQjFNNUJJbngKeUNjdXRBZ2tQVzhSMDdmaU5ML004bmRoUUFTWlU4NlpNTUJyUjB6cS9DNy9PS2lITDJZdjI5eng3ek5TSGUxUQozNkwrWmhzVXlsclYvdnMwWFY4cHBQaCsvVEFVREZkeXVXNlJsQ1lNQXF2U2RxVEp4M2dOK3Rva3R3VWF5NVk1ClVFV0srODMwQjYyNE5lRVJqSHBmOFhVYmJ1UFovcXBnbmxLSjBiRWM4RENvb2szQVBBUEtSMkU0eTBlRHBMWFQKV0xhR05wY2hBb0dCQU96RldFWG5OYWVMY2JtZzFuaXFkQms4anBBOWowUVVkR0lBZkpMMnA5cE1RVnFrVHlMdApCaE9MVEY4RUVHS1dsN0ticDhZZXVyM2gycnlmNldVa2svTTRIMWhFbE9FdGp3aFpWa1hGMENPUzFoSjJRSUlmCm10SFFSUENuZFg0TTdmekIvUmptSE9KWVdWM3BOVjZ3ZE5KOS9PSjghfsdfs3sFJ1VE1xMmpYWTRKQW9HQkFNaFcKa3FSOFZVM0J4Mm9oWHFBQTUxb1ZKbVg3L2p2akhhUXJTRHhQOFNJcVVnVjJaQ3EyWDBRTlRLV0lSVDhhcmsrYQpvaGd2ZytnVWlIcjZDQUdYWmRESTluWEFYbG40aGVyaUYzNVJSMkRVbzhndHpIaDFyNUVad2pEOG1DTlFhMkZaCndidWgreGxPVzRSeU1IY2Jkb1QzUDlFTGdrWHRSU1pRa0pyYlk2Y0RBb0dCQU5VUVZYbzZNTWMvcmF4TXR4TkkKMkVicGZxVUFNSThrRlFNbnl2SjVNZDA0cDhzSWR3cEgzeUx4UkYxd2k4b2NHQkNyRDlReVRQdVlaYjA5N2NxTgptdkhRdkN3ek13SmJmQTRZVHBGbERBTW5IS3JxYk94cndtY3lrd2M0dW5zZTZYNTlsdU8wRjZQN3V4Zk9SNitZCi9OZDZkbm1CdXdWeFIzUk1CdHZJV2VUNUFvR0FMMWhHVDVrU2o4MjcwdGtRQThBeTdKY1MvQWNSamhXZWE2M08KNUhJQUNwTDF6MVNyVjJ6Q0Z0TU55aERxVEgrQnNrNVpBRjQ2VGg2TUlvUDBZR3ZuSS9CYVRubW4wcHRwQ3Bsago4L1pCYUNEWWsvWSszRGp6eE5iUmpjSWtNalJQTERLS0ZrMnhpY2w2MTFJbElnRGJnWkR0QS9vMFQxSkRoVXFFCjRoUDIrUUVDZ1lFQWd2U1owOFRrS2VKM1Z1dkdIeU4wWUk3Umx1Q284ZVVVY3IrOEFvR0MrNjRHbmQ1WFc1eGcKZFViQ1lxN3h2OERjN2MzTkt1RVNKSmg4czYyRllXVjRmQkxGckwrYzRreVpxYnRxdDExN3ZnZklvZU1MNkxTdwpINmc2R3Ria01Fand1eldPQUFlSllsNEptMUUzSDB3eGVBZUlWeldoZ2I5NWF4aFZkWlZlT2VZPQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
```

如果需要在 Node 节点使用 kubectl 工具，需要进行如下配置：

1. 拷贝 Master 节点中 "/etc/kubernetes/admin.conf" 文件到对应的 Node 节点服务器的 "/etc/kubernetes" 目录中：

   ```shell
   [root@k8s-master .kube]# scp /etc/kubernetes/admin.conf root@k8s-node1:/etc/kubernetes/
   The authenticity of host 'k8s-node1 (192.168.1.121)' can't be established.
   ECDSA key fingerprint is SHA256:1wcmZ7fQ/AJ9ak1Hu/qURBJMFWuqvu66TbMV8OUD9us.
   ECDSA key fingerprint is MD5:ce:5e:b8:01:2d:f6:ee:77:87:33:b4:7d:4b:64:a5:d9.
   Are you sure you want to continue connecting (yes/no)? yes
   Warning: Permanently added 'k8s-node1,192.168.1.121' (ECDSA) to the list of known hosts.
   root@k8s-node1's password: 
   admin.conf                                                                                                         100% 5641     4.7MB/s   00:00
   ```

2. 在对应的 Node 服务器上配置环境变量：

   ```shell
   [root@k8s-node1 ~]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> ~/.bash_profile 
   [root@k8s-node1 ~]# source ~/.bash_profile 
   [root@k8s-node1 ~]# kubectl get nodes
   NAME         STATUS   ROLES                  AGE     VERSION
   k8s-master   Ready    control-plane,master   11d     v1.23.6
   k8s-node1    Ready    <none>                 4d23h   v1.23.6
   k8s-node2    Ready    <none>                 4d23h   v1.23.6
   ```

kubectl 工具的常用的命令和功能如下，更多查看官网：https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

![1725896605275](kubernetes-series-01-install/1725896605275.jpg)

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/Operation/kubernetes.md

