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

在容器中查看 history

### 28、网站目录中网站的主配置文件是哪一个：

查看 server/db.js ，数据库名：mongodb

![](https://s2.loli.net/2022/10/19/enI47BqCHvRgYEp.png)

### 29-33：

由 db.js 可知

### 34、在案发时，黑客对该服务器某个文件/目录进行了加密，请问是哪个文件/目录：

在 root 下查看 history：.

![](https://s2.loli.net/2022/10/20/hY7IiOLSoGB1Kzr.png)

## 检材二：

### 35、该数据库服务器使用数据库的安装路径：

文件全显，查找 mongo ，查看路径

### 36、数据库的配置文件的路径：

mongo.conf

### 37、数据库的日志文件路径：

mongod.log

### 38、该数据库的网站用户表名是什么：

查看上述日志，搜索 password：

![输入图片说明](https://s2.loli.net/2022/10/26/ryFd8Qqu5OBCRYt.png)

users

### 39、该数据库中网站用户表里的密码字段加密方式：

未加密

前端中没有进行加解密：

![](https://forensics.xidian.edu.cn/wiki/images/image-20201112114926679.png)

### 40、该用户表被做过什么样的修改：

日志中：修改密码

### 41、嫌疑人对该数据库的哪个库进行了风险操作：

![输入图片说明](https://s2.loli.net/2022/10/26/n6PRSpiEYObsurj.png)

![输入图片说明](https://s2.loli.net/2022/10/26/Lw8543RFpAuSxXt.png)

### 42、44：

同上

### 43、嫌疑人在哪个时间段内登陆数据库：

检材2中：`last`

![输入图片说明](https://s2.loli.net/2022/10/26/tRPCpj4z6IKoHOw.png)


## 检材三

### 44、该服务器所使用的VPN软件，采用了什么协议：

![输入图片说明](https://s2.loli.net/2022/10/26/AkiFvKobVxmJP92.png)

PPTP

### 45、该服务器的时区为：

```
timedatectl
``` 

### 46、该服务器中对VPN软件配置的option的文件位置在哪里：

history 中可以看出

### 47、

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDk4MjY3MDUzLC0yNDkzMTQwNDAsLTg4ND
QyNDYxMiw2OTA3MjAyMTgsLTkxOTExMDksLTIxMDI5Mzk0Mjgs
LTEyOTQ0NDE5OTksLTMyMTM1ODU1MywxNTE5NDY5MjgzLDc4OD
Y0MjUxNSwyMTI3NDg1MzUzLC0xMTgzMTExOTEzLDc5MTMyODE3
MywtMTk4MDA2MTc5NiwtMTE5NjI0NjI0LDIwMDI5Njg3ODcsMT
YwODcwOTA1MywxODYzNTA5ODQyLC0yMDgxNzkxOTEyLC0xNjU0
NTY5OTcxXX0=
-->