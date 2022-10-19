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

### 20、网站目录所在的容器内部路径为：

进入容器内查看

### 21、网站目录所在的主机路径为下列选项中的哪个：

火眼中选择文件全显，高级过滤中选择目录名称

### 22、网站日志的路径：

文件中查找，进入容器中查找

### 23、案发当时，该服务器的原始IP地址是多少：

进入上述网站日志，发现：192.168.184.128

### 24、在docker中，各容器节点和主机之间的网络连接模式是什么：

![](https://s2.loli.net/2022/10/19/WFznDKgdNZ2TRSG.png)

`docker inspect`     获取容器/镜像的元数据

`docker network` 显示所有docker局域网络

bridge

### 25、当我们想将网站重构好时，访问网站时，web应用在其中承担什么样的工作：

转发（？）

### 26、从网站日志中，我们可以看到嫌疑人入侵服务器所使用的IP是：

![](https://s2.loli.net/2022/10/19/wd3LH8Et9ale5TN.png)

嫌疑人尝试爆破密码，IP为 192.168.184.133

### 27、网站目录中网站的主配置文件是哪一个：

查看 history

### 28、网站目录中网站的主配置文件是哪一个：


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5ODAwNjE3OTYsLTExOTYyNDYyNCwyMD
AyOTY4Nzg3LDE2MDg3MDkwNTMsMTg2MzUwOTg0MiwtMjA4MTc5
MTkxMiwtMTY1NDU2OTk3MSwtOTA3NTIwNzksLTQ0Nzc1OTI0OC
wtMTc2OTIzMDkxMSwtMjA2MTY0ODEwNSwtMTI3MzIxMDY5MCwy
MDE1MDc5NDE1LC0xNTY5OTA1MzQ0LDg0OTcyNTE4Nyw5Mzk1Mj
Q5ODYsMTY5MDc0MDA2Niw3ODc3MzU3MzYsMTc4NDYzNDM5OSwx
MTAwMzM5NjYwXX0=
-->