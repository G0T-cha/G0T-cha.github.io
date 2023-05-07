
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
大概思路是先下载nginx，

> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg2MjM0ODA2MiwtMTgwMjM3ODA2MCwtMT
kxNTQyNjk1LC01OTg5MDIxNSwtMzU5MTk1Nzk3LDIzMjA4MTcz
LDE3MzI2NzYxODhdfQ==
-->