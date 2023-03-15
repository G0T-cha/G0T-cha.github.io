
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


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MDgzNDcwNDAsLTY1MDg2MDQ2NywtNz
Y5MTM1NDExLDE5NzM0OTU0NTgsMTA4OTM3MTIzNywxOTc3MDEx
NDQ3LC0xNTY4MDkwNTc3LC02NjEwODQ2ODcsNjU1MzQwNDI2LD
k1OTkyNjQ5NiwtMzE2NzA0NDQ4LC0yMDQ0MzU1Njc1LC0xNDc1
MTc2NjQ5XX0=
-->