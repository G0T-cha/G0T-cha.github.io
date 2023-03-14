
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

shiyong

#### 1、HTTP Basics




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTEwMzQxNTYsOTU5OTI2NDk2LC0zMT
Y3MDQ0NDgsLTIwNDQzNTU2NzUsLTE0NzUxNzY2NDldfQ==
-->