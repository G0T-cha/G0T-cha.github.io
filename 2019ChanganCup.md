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

### 10、docker 应用 server 版本：

    docker version

### 11、docker 容器节点数：

    docker ps -a

### 12、运行中的容器节点数：

    docker ps

### 13、名称为 romantic_varahamihira 的容器节点，它的 hostname

### 14、上题容器节点中，占用了主机的哪个端口：

未占用？

### 15、容器ID为15debb1824e6的容器节点，它运行了什么服务：

ssh

### 16、上题容器节点中，占用了主机的哪个端口：

39999

### 17、该服务器中网站运行在 docker 容器中，其中web服务使用的是什么应用：

nginx

### 18、上题所述运行web服务的容器节点，使用的镜像名称是什么：

![](https://s2.loli.net/2022/10/19/UYg1OoB5TjZSEXV.png)

### 19、上题所述容器节点占用的容器端口是什么：

**容器端口**：80（后面的）

### 20、


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjE2NDgxMDUsLTEyNzMyMTA2OTAsMj
AxNTA3OTQxNSwtMTU2OTkwNTM0NCw4NDk3MjUxODcsOTM5NTI0
OTg2LDE2OTA3NDAwNjYsNzg3NzM1NzM2LDE3ODQ2MzQzOTksMT
EwMDMzOTY2MCwyMTA5MjE1NTQyLDEwNTc5MDE1MjMsMTU1MDY0
MDQ3OSwtMTg1MDU2MzU3LC0yMDg4NzQ2NjEyXX0=
-->