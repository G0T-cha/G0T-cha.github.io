
# SQL注入（进阶）

## 环境搭建

老师的视频中给出的是 Fedola 中的搭建，但在此尝试了 Kali 下的搭建，由于文件目录等的不同，在搭建时出现了诸多问题:
尝试了
https://blog.csdn.net/weixin_54252904/article/details/117855931
https://huaweicloud.csdn.net/638db1bbdacf622b8df8c6a0.html#5mysqlphpnginxmysqlphpapache_36
https://blog.csdn.net/weixin_43268590/article/details/125650002
但都不成功，恢复了无数次快照之后，按照
https://xp-rience.blogspot.com/2020/05/nginx-php-fpm-setup-under-kali-linux.html
成功搭建了 php+nginx 的环境：
![输入图片说明](/imgs/2023-05-01/ybY35dzlQrrS24fa.png)

kali 自带 mysql，但后面还是打不开 1.php，试了好多办法还是不行，推倒重来 fedora 了
大概思路是先安装 nginx（启动，检查是否启动成功），安装php、php-fpm（启动，检查是否启动成功），修改 nginx 配置文件：监听php，这时就可以通过浏览器打开 nginx 目录下的 php 文件了，然后安装 mysql（mariadb 及服务器），使用命令`mysql_secure_installation` 配置mysql，导入需要的sql_injection表，安装 php-mysqlnd，重启 nginx，php-fpm，这是按老师的演示视频就可以打开 1.php 查看内容，*不过我打开还是空白*？但是 1.html 可以打开，输入 user id 后也可正常回显，就这样用着吧先

## 实际使用

为了便于观测，修改数据库配置文件 my.cnf，新建一个日志目录用于记录数据库查询记录，然后使用`sudo tail -f /var/log/mariadb/queries.log` 持续观察：
![输入图片说明](/imgs/2023-05-07/z1PI87MXORtb1ekc.png)

在建立一个 用于mysql查询的命令行窗口：`mysql -u root -p`，输入密码登录进入数据库，再进到表 sql_injection中：`use sql_injection`：
![输入图片说明](/imgs/2023-05-08/R2xSGCa1ApdJmNF9.png)


> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMzUwMTQ4NiwtNjc0NjU0OTU4LDE1OD
kwNDg1ODksLTE4NTg4NjI5NTcsLTE1NjYzNTQxNzAsMjcxOTkw
NjM0LDIzOTc0NzIyNiwtMjA2ODc4ODUxMiwxNjQ4NTU2NjEwLC
0xODAyMzc4MDYwLC0xOTE1NDI2OTUsLTU5ODkwMjE1LC0zNTkx
OTU3OTcsMjMyMDgxNzMsMTczMjY3NjE4OF19
-->