# 2020长安杯题解

## 检材一
### 1. *操作系统版本*：
火眼中添加镜像即可查看

### 2. *内核版本*：
```
uname -a
```
3. *LVM（逻辑卷）的 LBA（逻辑区块地址）*：
```
fdisk -l
```
### ***分析服务器：***
查看网络连接和端口：
```
netstat -napt
```
*（-n 直接使用ip地址，而不通过域名服务器）*

![2022-07-04_105320.png](https://s2.loli.net/2022/07/04/vidHyjlBP1ZpMzo.png)

获得信息：
1. SSH(Secue Shell) 端口位于7001（远程连接服务器）
2. 用到nginx反向代理

查看nginx配置文件：
```
cat  /etc/nginx/nginx.config
```
![Image.png](https://s2.loli.net/2022/07/04/Z3p8xuJtVARjULe.png)

到所示目录下查看：
```
cd  /etc/nginx/conf.d/
ls
```
![2022-07-04_124519.png](https://s2.loli.net/2022/07/04/mOAve2QharbtUV9.png)

服务器共绑定3个对外开放域名（**题5答案**），www.kkzhc.com即为题目中嫌疑人犯罪网站

    vim www.kkzjc.com

![image.png](https://s2.loli.net/2022/07/04/uGHdL6z42aZbQXI.png)

可以得知：

1. *“www.kkzjc.com”对应的 Web 服务对外开放的端口*：32000（**题4答案**）
2. proxy-pass反向代理转发给8091端口

但8091端口在netstat命令后并未显示
查看历史：

    history
    
![image.png](https://s2.loli.net/2022/07/04/H431UBgIu6s2GbN.png)

发现使用很多docker命令
首先打开未启动的docker：

    systemctl start docker
    docker ps -a
*（-a包括未启动的容器）*

![image.png](https://s2.loli.net/2022/07/04/d3q9J1tLXn7DoyY.png)

此时再使用netstat查看网络连接，发现8091端口：

![image.png](https://s2.loli.net/2022/07/04/zy9qUdcOljY4fxo.png)

其中的30000、31000、32000都实际转发到8091端口，实现一级跳板：

![](https://img-blog.csdnimg.cn/20201121205650340.png#pic_center)

进入容器中查看一下：

    docker exec -it 08 /bin/bash
*（ exec 在运行状态下的容器中执行命令 /bin/bash解释执行模式 ）*
容器中同样进行上述分析：

 - 查看history（分析服务器可以先看一下）
 - 发现用到多次nginx：`more /etc/nginx/nginx.conf`
 - 发现 include：/etc/nginx/conf.d/*.conf
 - 到该路径下：`cd /etc/nginx/conf.d/`  `ls`
 - 目录下有：hl.conf
 - 查看配置文件：`more hl.conf`

![image.png](https://s2.loli.net/2022/07/05/tbErldXaGFIgxwo.png)

可以得到：
 1. 监听docker内部80端口
 2. proxy_pass到192.168.1.176实现跳板（**题8答案**）

```
exit
```
### *第7题：查看远程登录使用的IP地址*

    last
*（显示用户最近登录信息）*

![image.png](https://s2.loli.net/2022/07/05/4LWia69keVtrhqK.png)

192.168.99.222即为**题7答案**

### *题6题9*：
使用`docker exec -it 08 /bin/bash`进入容器，`cat /etc/nginx/nginx.conf`中得知nginx日志access.log位置：

![image.png](https://s2.loli.net/2022/07/05/8lh5atUuRiVLNfz.png)

进入该目录`cd /var/log/nginx`,`cat access.log`无反应，`ls -l`（ -l 得到一个详细的文件和目录名列表）发现 access.log 重定向到其他位置，为链接无法查看：

![image.png](https://s2.loli.net/2022/07/05/kpcW6xNzECOMrXf.png)

尝试其他方法

    docker logs 08
   （查看docker日志）
   
![image.png](https://s2.loli.net/2022/07/05/18gDAcOhKCFrvpI.png)

（99.3为IP访问 下面几行为域名访问）
192.168.99.3即为**题6**答案：服务器所在原始IP

题9统计数量即可：

    docker logs 08 | grep 192.168.99.222 | wc -l

*（| 管道  grep 筛选前面管道传来的信息  wc -l 统计行数）*
得出18，即为**题9**答案
***

## 检材二

**题6**的简单解：
Chrome浏览记录：

![image.png](https://s2.loli.net/2022/07/05/bLPQJ6qvnAcxGuK.png)

192.168.99.3即为**题6**服务器原始ip地址

或通过查看/Windows/System32/drivers/etc/hosts

![image.png](https://s2.loli.net/2022/07/05/6t9Bjpge3hyK1wN.png)

*（hosts：将一些常用的网址域名与其对应的 IP 地址建立关联）*

### 10. *检材2的SHA值*：
使用火眼直接计算即可
2D926D5E1CFE27553AE59B6152E038560D64E7837ECCAD30F2FBAD5052FABF37

### 11. *OS内部版本号*：
系统-设置-关于-Windows规格-操作系统版本：18363.1082

![image.png](https://s2.loli.net/2022/07/05/liSXB2ApVQ1TEHb.png)

### 12. 最后一次正常关机时间：

火眼证据分析：  
分析——基本信息——操作系统信息——最后一次正常关机时间

### 13. Vmware安装时间：
……
### 14. Vmware启动次数：
……
### 15. 嫌疑人通过Web从检材2访问检材1连接的目标端口：
8091（见第六题）
### 16. 该端口上运行的进程的程序名称（Program name）为：
docker-proxy（见服务器分析打开docker后的netstat）
### 17. 嫌疑人从检材 2 上访问该网站时，所使用的域名为：
取证分析中查看Chrome浏览记录：www.sdhj.com
### 18. 嫌疑人所使用的微信 ID 是：
取证分析中查看用户痕迹，发现my iphone.tar.Ink，找到所在位置，导出文件，在取证分析中添加该镜像，即可对手机进行分析：![](https://s2.loli.net/2022/07/06/YwukBhPWzJ3N564.png)

微信ID为：sstt119999
### 19. 嫌疑人为与广告位供应商沟通时使用的通联工具的名称为：
telegram![](https://s2.loli.net/2022/07/06/mfvyqZOCSszaBF2.png)

### 20. 嫌疑人使用何种虚拟货币与供应商进行交易：
微信聊天记录：dogcoin

![](https://s2.loli.net/2022/07/06/jifABrgnLcqxy51.png)
### 21. 对方的收款地址是：
 DPBEgbwap7VW5HbNdGi9TyKJbqTLWYYkvf（扫描二维码）
### 22. 交易时间：
![](https://s2.loli.net/2022/07/06/nYkgFdEGXiA586Z.png)

### 23. 支付货币数量：
4000（见上题）
### 24.，嫌疑人使用的虚拟机的虚拟磁盘被加密，密码为：
打开仿真虚拟机中的VMware，找到镜像所在位置

![](https://s2.loli.net/2022/07/06/gyJcpMRaEoI8Ywb.png)

导出路径下的vmx文件到pyvmx-cracker-master目录下，将其改名为“ 1.vmx ”（方便操作），打开命令行，执行：`python .\pyvmx-cracker.py -v 1.vmx -d wordlist.txt`爆破密码：

![](https://s2.loli.net/2022/07/06/lc6YFjry3maezER.png)

![](https://s2.loli.net/2022/07/06/FaIKEkDeXJOP57c.png)

得出密码为：zzzxxx
### 25. 嫌疑人发送给广告商的邮件中的图片附件的 SHA256 值为：

在虚拟机设置——选项——访问控制中移除密码：

![](https://s2.loli.net/2022/07/06/v4dLu7BQqHfFl39.png)

即可把镜像挂载到取证软件下进行快速分析。
找到“广告”图片，导出。使用`certutil -hashfile "文件名" SHA256`计算哈希值：
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzkxNTcwMzAsLTExMTAzOTI2ODYsLT
IwMTgwMTgwNDQsLTE0NjM4NzM5NjgsLTEzNjUxMDc2NCwtMTk1
NjA1MTU5NiwxOTU0MDU1MjA2LC03MDg3MjczMzcsLTYyODEzMz
gyMywtMTU3Njg1MDAxLC0xNTkwODUwNjk1LC0xNTE0Mzg0Mzcz
LDExMTg3MTQ2MSwtMTI4NDA3MjQ2Miw3NDEyOTkyOTksLTIwMT
cwMDE0MjksNjAxMjgwMTQ4LDE2NjAxODE3MzYsLTU1ODIyMjgw
NSw4MzczMzk0MDBdfQ==
-->