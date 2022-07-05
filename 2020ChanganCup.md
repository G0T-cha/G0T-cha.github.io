# 2020长安杯题解

## 检材一
1. *操作系统版本*：火眼中添加镜像即可查看
2. *内核版本*：
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
第7题：查看远程登录使用的IP地址

    last
*（显示用户最近登录信息）*
![image.png](https://s2.loli.net/2022/07/05/4LWia69keVtrhqK.png)

192.168.99.222即为**题7答案**

**题6题9**：
使用`docker exec -it 08 /bin/bash`进入容器，`cat /etc/nginx/nginx.conf`中得知nginx日志access.log位置：
![image.png](https://s2.loli.net/2022/07/05/8lh5atUuRiVLNfz.png)

进入该目录`cd /var/log/nginx`,


***

## 检材二

**题6**：
Chrome浏览记录：
![image.png](https://s2.loli.net/2022/07/05/bLPQJ6qvnAcxGuK.png)

192.168.99.3即为**题6**服务器原始ip地址

或通过查看/Windows/System32/drivers/etc/hosts
![image.png](https://s2.loli.net/2022/07/05/6t9Bjpge3hyK1wN.png)

*（hosts：将一些常用的网址域名与其对应的 IP 地址建立关联）*



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMwMDg2MzUwNCwtMzk5MDgyNDYxLDMwOD
E5MTE4MiwxNzU4Mjc4OTM2LDEyMDc2MzkxODMsNjU1OTQ3OTAz
LDEzODQ4NTgxNCwxNjM3MDIyNjg5LDY3ODE4NDkzNywxNDg1OD
U0MjEzXX0=
-->