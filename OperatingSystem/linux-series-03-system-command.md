> *`Author: ACatSmiling`*
>
> *`Since: 2024-10-22`*

**操作系统信息：**

1. 方式一：`uname -a`。
2. 方式二：`cat /etc/os-release`。

**内核版本信息：**

1. 方式一：`uname -r`。
2. 方式二：`cat /proc/version`。

**服务器架构信息：**

1. 方式一：`uname -m`。
2. 方式二：`lscpu | grep -i 'Architecture'`。

**CPU 信息：**

1. 方式一：`lscpu`。

   ```shell
   [root@skynet13 ~]# lscpu
   Architecture:          x86_64
   CPU op-mode(s):        32-bit, 64-bit
   Byte Order:            Little Endian
   CPU(s):                96
   On-line CPU(s) list:   0-95
   Thread(s) per core:    2
   Core(s) per socket:    24
   Socket(s):             2
   NUMA node(s):          8
   Vendor ID:             HygonGenuine
   CPU family:            24
   Model:                 2
   Model name:            Hygon C86 7365 24-core Processor
   Stepping:              2
   CPU MHz:               2200.098
   BogoMIPS:              4400.19
   Virtualization:        AMD-V
   L1d cache:             32K
   L1i cache:             64K
   L2 cache:              512K
   L3 cache:              8192K
   NUMA node0 CPU(s):     0-5,48-53
   NUMA node1 CPU(s):     6-11,54-59
   NUMA node2 CPU(s):     12-17,60-65
   NUMA node3 CPU(s):     18-23,66-71
   NUMA node4 CPU(s):     24-29,72-77
   NUMA node5 CPU(s):     30-35,78-83
   NUMA node6 CPU(s):     36-41,84-89
   NUMA node7 CPU(s):     42-47,90-95
   Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc art rep_good nopl nonstop_tsc extd_apicid amd_dcm aperfmperf eagerfpu pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw skinit wdt tce topoext perfctr_core perfctr_nb bpext perfctr_l2 cpb hw_pstate avic fsgsbase bmi1 avx2 smep bmi2 rdseed adx smap clflushopt sha_ni xsaveopt xsavec xgetbv1 arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold overflow_recov succor smca
   ```

2. 方式二：`cat /proc/cpuinfo | grep 'processor'`。

   ```shell
   [root@skynet13 ~]# cat /proc/cpuinfo | grep 'processor'
   processor       : 0
   processor       : 1
   processor       : 2
   processor       : 3
   processor       : 4
   processor       : 5
   processor       : 6
   processor       : 7
   processor       : 8
   processor       : 9
   processor       : 10
   processor       : 11
   processor       : 12
   processor       : 13
   processor       : 14
   processor       : 15
   processor       : 16
   processor       : 17
   processor       : 18
   processor       : 19
   processor       : 20
   processor       : 21
   processor       : 22
   processor       : 23
   processor       : 24
   processor       : 25
   processor       : 26
   processor       : 27
   processor       : 28
   processor       : 29
   processor       : 30
   processor       : 31
   processor       : 32
   processor       : 33
   processor       : 34
   processor       : 35
   processor       : 36
   processor       : 37
   processor       : 38
   processor       : 39
   processor       : 40
   processor       : 41
   processor       : 42
   processor       : 43
   processor       : 44
   processor       : 45
   processor       : 46
   processor       : 47
   processor       : 48
   processor       : 49
   processor       : 50
   processor       : 51
   processor       : 52
   processor       : 53
   processor       : 54
   processor       : 55
   processor       : 56
   processor       : 57
   processor       : 58
   processor       : 59
   processor       : 60
   processor       : 61
   processor       : 62
   processor       : 63
   processor       : 64
   processor       : 65
   processor       : 66
   processor       : 67
   processor       : 68
   processor       : 69
   processor       : 70
   processor       : 71
   processor       : 72
   processor       : 73
   processor       : 74
   processor       : 75
   processor       : 76
   processor       : 77
   processor       : 78
   processor       : 79
   processor       : 80
   processor       : 81
   processor       : 82
   processor       : 83
   processor       : 84
   processor       : 85
   processor       : 86
   processor       : 87
   processor       : 88
   processor       : 89
   processor       : 90
   processor       : 91
   processor       : 92
   processor       : 93
   processor       : 94
   processor       : 95
   ```

**GPU 信息：**

```shell
[root@skynet13 ~]# lspci | grep -i 'vga\|3d\|display'
22:00.0 VGA compatible controller: ASPEED Technology, Inc. ASPEED Graphics Family (rev 41)
69:00.0 3D controller: NVIDIA Corporation Device 20f1 (rev a1)
6d:00.0 3D controller: NVIDIA Corporation Device 20f1 (rev a1)
[root@skynet13 ~]# nvidia-smi
Tue Oct 22 11:36:43 2024       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 510.39.01    Driver Version: 510.39.01    CUDA Version: 11.6     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA A100-PCI...  Off  | 00000000:69:00.0 Off |                    0 |
| N/A   36C    P0    40W / 250W |  29999MiB / 40960MiB |      0%      Default |
|                               |                      |             Disabled |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA A100-PCI...  Off  | 00000000:6D:00.0 Off |                    0 |
| N/A   35C    P0    39W / 250W |   4305MiB / 40960MiB |      0%      Default |
|                               |                      |             Disabled |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     15491      C   java                             4101MiB |
|    0   N/A  N/A     29074      C   java                             4405MiB |
|    0   N/A  N/A     31086      C   java                             1571MiB |
|    0   N/A  N/A     34440      C   java                             3641MiB |
|    0   N/A  N/A     66550      C   java                             7231MiB |
|    0   N/A  N/A     71424      C   java                             1711MiB |
|    0   N/A  N/A     71545      C   java                             1469MiB |
|    0   N/A  N/A     71962      C   java                             1535MiB |
|    0   N/A  N/A     77319      C   java                             4329MiB |
|    1   N/A  N/A     29074      C   java                             4303MiB |
+-----------------------------------------------------------------------------+
```

**内存信息：**

1. 方式一：`free -h`。
2. 方式二：`cat /proc/meminfo`。

磁盘信息：

1. 方式一：`df -h`。
2. 方式二：`fdisk -l`。