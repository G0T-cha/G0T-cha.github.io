# 2019长安杯题解
## 检材一：
### 1、SHA256值
### 2、操作系统版本
### 3、内核版本：

    uname -a
    
### 4、硬盘分区数：

![](https://s2.loli.net/2022/10/18/yT7Agw5J8ONWLpI.png)

2个分区

### 5、含有LVM逻辑卷的分区开始的逻辑区块地址：

![](https://s2.loli.net/2022/10/18/U5JF6Obwd1DXBqN.png)

2099200

### 6、该分区逻辑卷分区内root逻辑卷的文件系统是什么：

    df -T

![](https://s2.loli.net/2022/10/18/tOAEqLbCuP1WUFY.png)

*Linux df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。-T    --print-type 显示文件系统的形式*

### 7、该分区逻辑卷分区内root逻辑卷的物理大小：



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc4NDYzNDM5OSwxMTAwMzM5NjYwLDIxMD
kyMTU1NDIsMTA1NzkwMTUyMywxNTUwNjQwNDc5LC0xODUwNTYz
NTcsLTIwODg3NDY2MTJdfQ==
-->