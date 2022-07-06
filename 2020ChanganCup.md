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

![](https://s2.loli.net/2022/07/06/qPz2UioDNgxj57A.png)

### 26.检材 2 中，嫌疑人给广告商发送广告图片邮件的发送时间是:
2020-09-20 12:53:00
### 27. 嫌疑人的邮箱密码是:
honglian7001
### 28. 嫌疑人使用了哪种远程管理工具，登录了检材 1 所在的服务器：
Xshell
### 29. 使用上述工具连接服务器时，使用的登录密码为

![](https://s2.loli.net/2022/07/06/qWROCmHPnFteLc5.png)
***
## 检材3

### 30. 检材 3 的原始磁盘 SHA256 值：
![](https://s2.loli.net/2022/07/06/5LOpsFUjc4qD1KM.png)

### 31. 操作系统版本：
Windows Server 2008 HPC Edition

### 32.检材 3 中，部署的网站名称是：
服务器管理器中查看：

![](https://s2.loli.net/2022/07/06/HBUu5yTv9YMXckP.png)

名称为：card
### 33. 部署的网站对应的网站根目录是：
C:\inetpub\wwwroot\v7w（见上图）
### 34. 部署的网站绑定的端口是：
80（见上图）
### 35. 具备登陆功能的代码页，对应的文件名为：
找到配置文件位置：/inetpub/wwwroot/v7w

先查看Web.config：

![](https://s2.loli.net/2022/07/06/e1G3FwJTl2yjbR6.png)

根据检材二中的Chrome浏览记录，发现嫌疑人是从 sdhj.com/dl 访问网站，故被重定向到dllogin.aspx文件

### 36. 对网站代码进行分析，网站登录过程中，代码中对输入的明文密码作了追加什么字符串处理：
查看dllogin.aspx：

![](https://s2.loli.net/2022/07/06/xq6KJwbQhktdFuD.png)

追加字符为0v0
### 37. 请对网站代码进行分析，网站登录过程中，代码中调用的动态扩展库文件的完整名称为：
App_Web_dllogin.aspx.7d7c2f33.dll（见上图）
### 38. 网站登录过程中，后台接收到明文密码后进行加密处理，首先使用的算法是 Encryption 中 的哪个函数：
使用 dnSpy 在 bin 文件夹中找到上述动态扩展库文件，找到主要方法：

![](https://s2.loli.net/2022/07/06/7PFaRqbVSI6pTuy.png)

首先使用的算法为：AESEncrypt
（审阅代码，先进行AES加密，AES加密函数中最后转化为base64，然后转化MD5）

### 39. 分析该网站连接的数据库地址为：

如上图，比较用户输入的密码和数据库密码由 DUserLogin 实现，查看该函数：

![](https://s2.loli.net/2022/07/06/dKu4wyPci51BW6s.png)

DBcon 应为数据库中正确密码和 IP 地址，查看：

![](https://s2.loli.net/2022/07/06/4Pxbrls3fpvVQ7g.png)

直接使用 Powershell 调用解密函数 AESDecrypt ：

    Add-Type -Path .\DBManager.dll
    [DBManager.Encryption]::AESDecrypt("Mcyj19i/VubvqSM21YPjWnnGzk8G/GG6x9+qwdcOJd9bTEyprEOxs8TD9Ma1Lz1Ct72xlK/g8DDRAQ+X0GtJ8w==", "HL", "forensix")
    
![](https://s2.loli.net/2022/07/06/oHcnIQFs4mM3r61.png)

192.168.1.174


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NDM3Nzc4NTEsMTk0MDIyNDQwNiwzNz
k3OTA2OTYsLTcwMTk0MDIyNCwxMjg5NTIwNjIzLC02NzExODc0
ODUsMjM1NjE0NTY5LDQ4NDg0OTExNSwtMzg4OTk4NzgwLDExMz
U0ODIyMTQsLTc2ODYwNzg3NywxMjU2NDQzODE0LC0xMTIzOTUy
OTYyLC0xNjMzOTU5NTE1LC0xNzc0MDY5MDAxLC0xMTEwMzkyNj
g2LC0yMDE4MDE4MDQ0LC0xNDYzODczOTY4LC0xMzY1MTA3NjQs
LTE5NTYwNTE1OTZdfQ==
-->