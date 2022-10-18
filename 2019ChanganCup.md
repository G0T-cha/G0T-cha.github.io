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

![image.png](https://s2.loli.net/2022/10/18/bIQ9ftljHoRLSKG.png)

### 8、找出该服务器的网站访问端口：

![](https://s2.loli.net/2022/10/18/aFuxP5TrMHNKGqh.png)

### 9、在本地 docker 镜像数：

    docker image ls

11

### 10、

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1Njk5MDUzNDQsODQ5NzI1MTg3LDkzOT
UyNDk4NiwxNjkwNzQwMDY2LDc4NzczNTczNiwxNzg0NjM0Mzk5
LDExMDAzMzk2NjAsMjEwOTIxNTU0MiwxMDU3OTAxNTIzLDE1NT
A2NDA0NzksLTE4NTA1NjM1NywtMjA4ODc0NjYxMl19
-->