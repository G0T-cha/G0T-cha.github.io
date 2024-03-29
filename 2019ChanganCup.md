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

### 45、该服务器所使用的VPN软件，采用了什么协议：

![输入图片说明](https://s2.loli.net/2022/10/26/AkiFvKobVxmJP92.png)

PPTP

### 46、该服务器的时区为：

```
timedatectl
``` 

### 47、该服务器中对VPN软件配置的option的文件位置在哪里：

history 中可以看出

### 48、VPN软件开启了写入客户端的连接与断开，请问写入的文件：

![输入图片说明](https://s2.loli.net/2022/10/26/gV7o1WQGfd93EXy.png)

wtmp

### 49、VPN软件客户端被分配的IP范围：

![输入图片说明](https://s2.loli.net/2022/10/26/BMYhF49Pp3w1gRL.png)

### 50、VPN软件的日志路径：

查看47中的文件options：

![输入图片说明](https://s2.loli.net/2022/10/26/Go5ibhRx7lgIpuJ.png)

### 51、VPN软件记录了客户端使用的名称和密码，记录的文件是：

穷举


### 52、在服务器时间2019-07-02_02:08:27登陆过VPN客户端的用户名是哪个：

查看50的日志：root

![输入图片说明](https://s2.loli.net/2022/10/26/XmH2coOWJYQtSzr.png)![](https://img2020.cnblogs.com/blog/2597809/202110/2597809-20211023110247663-356516573.png)

### 53、在服务器时间2019-07-02_02:08:27登陆过VPN客户端的用户名是哪个：

clientip

### 54、通过IP172.16.80.188登陆VPN服务器的用户名是哪个：

```
cat pptpd.log | grep C5 172.16.80.188
```

![输入图片说明](https://s2.loli.net/2022/10/27/qbBCyp9lLOdaTuK.png)

### 55、上题用户登陆VPN服务器的北京时间是：

根据逝去，上述事件+2

### 56、该服务器曾被进行过抓包，请问network.cap是对哪个网卡进行抓包获得的抓包文件：

![输入图片说明](https://s2.loli.net/2022/10/27/tZiq7TsIvCEdjwo.png)

### 57、对ens37网卡进行抓包产生的抓包文件并保存下来的是哪个：

![输入图片说明](https://s2.loli.net/2022/10/27/KCRBUYjXZDmWhi3.png)

rm：删除

### 58、从保存的数据包中分析可知，出口的IP为：

![输入图片说明](https://s2.loli.net/2022/10/27/OSYckHEDGqJvF4n.png)

172.16.80.92

## 检材四：

### 59、哈希值

### 60、操作系统版本

### 61、用户最后一次登陆时间

### 62、该系统最后一次正常关机时间

### 63、计算检材桌面上文本文件的sha256值

### 64、该系统于2019年7月13日安装的软件为

### 65、找出该嫌疑人于2019-07-13 17:52:19时，使用WinRAR工具访问的文件：

bitlocker.rar（时间线）

### 66、系统于2019-07-13 17:53:45时运行的程序

### 67 68、文件test2-master.zip是什么时间下载到本机的：

Chrome-下载记录

### 69、嫌疑人成功连接至192.168.184.128服务器的时间为：

Xshell — 跳转到 192.168.184.128 记录

![输入图片说明](https://s2.loli.net/2022/10/28/itcUOJB9HvIwpCY.png)

![输入图片说明](https://s2.loli.net/2022/10/27/szIPVvXt53iSchu.png)


### 70 71 72、嫌疑人通过远程连接到128服务器，下载了什么文件到本机：

...

### 73、嫌疑人所用的手机的IMEI号码：

ios备份记录

---
添加手机检材
___
### 74、嫌疑人是通过何种方式联系到售卖恶意程序的卖家的：

qq聊天记录

### 75、嫌疑人和卖家的资金来往是通过何种方式：

微信转账

### 76、嫌疑人在犯罪过程中所使用的QQ账号为：

74

### 77、卖家所使用的微信账号ID为：

![输入图片说明](https://s2.loli.net/2022/10/28/ewXrF3psVItJDNq.png)

带头大哥

### 78、嫌疑人下载了几个恶意程序到本机：

2
和带头大哥聊天记录

### 79、恶意程序被嫌疑人保存在什么位置：

D盘

![输入图片说明](https://s2.loli.net/2022/10/28/7IPh4nQGlERv6MC.png)

### 80、恶意程序是使用什么工具下载到本地的：

![输入图片说明](https://s2.loli.net/2022/10/28/xh2NOlEdzjIPHLW.png)

edge 下载记录

### 81、嫌疑人是什么时间开始对受害者实施诈骗的：

![输入图片说明](https://s2.loli.net/2022/10/28/8VxzuQBJchk9RIG.png)

### 82、请提取受害者的银行卡信息，银行卡账号为：

### 83、 请综合分析，嫌疑人第一次入侵目标服务器的行为发生在

找到访问 http://192.168.184.128:8091/ 时间

### 84、请综合分析，嫌疑人入侵服务所使用的登录方式为：

SSH密钥





<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU4ODAwMDg0NywxNDY1MDAwNzM4LDEwNT
AyNTMxOTksLTIwNzY2Mzk1NTgsMTE2MzI0OTM4NSwyNDI5ODUy
MjcsMTMyOTAxODMyNCwzMDM5NzY3MzEsMTQ4OTE2MTEwNCwyMD
Q1OTE3NzA2LC0xNDk2OTA0NTA2LC0xNjQ0NTc2OTE2LDEzNjA4
MzcxMDcsLTI3NjI5NjY2NywtOTYyNTMzMjI2LC0zNjcyMDM4Mj
QsLTEyOTU3NDcxMDEsLTE4NTcyNDkwMCwyMTI0MzI2ODgwLDY3
NzAwNjc4OF19
-->