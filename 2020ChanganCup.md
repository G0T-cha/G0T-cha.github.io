---


---

<h1 id="长安杯题解">2020长安杯题解</h1>
<h2 id="检材一">检材一</h2>
<h3 id="操作系统版本：">1. <em>操作系统版本</em>：</h3>
<p>火眼中添加镜像即可查看</p>
<h3 id="内核版本：">2. <em>内核版本</em>：</h3>
<pre><code>uname -a
</code></pre>
<ol start="3">
<li><em>LVM（逻辑卷）的 LBA（逻辑区块地址）</em>：</li>
</ol>
<pre><code>fdisk -l
</code></pre>
<h3 id="分析服务器："><em><strong>分析服务器：</strong></em></h3>
<p>查看网络连接和端口：</p>
<pre><code>netstat -napt
</code></pre>
<p><em>（-n 直接使用ip地址，而不通过域名服务器）</em></p>
<p><img src="https://s2.loli.net/2022/07/04/vidHyjlBP1ZpMzo.png" alt="2022-07-04_105320.png"></p>
<p>获得信息：</p>
<ol>
<li>SSH(Secue Shell) 端口位于7001（远程连接服务器）</li>
<li>用到nginx反向代理</li>
</ol>
<p>查看nginx配置文件：</p>
<pre><code>cat  /etc/nginx/nginx.config
</code></pre>
<p><img src="https://s2.loli.net/2022/07/04/Z3p8xuJtVARjULe.png" alt="Image.png"></p>
<p>到所示目录下查看：</p>
<pre><code>cd  /etc/nginx/conf.d/
ls
</code></pre>
<p><img src="https://s2.loli.net/2022/07/04/mOAve2QharbtUV9.png" alt="2022-07-04_124519.png"></p>
<p>服务器共绑定3个对外开放域名（<strong>题5答案</strong>），www.kkzhc.com即为题目中嫌疑人犯罪网站</p>
<pre><code>vim www.kkzjc.com
</code></pre>
<p><img src="https://s2.loli.net/2022/07/04/uGHdL6z42aZbQXI.png" alt="image.png"></p>
<p>可以得知：</p>
<ol>
<li><em>“<a href="http://www.kkzjc.com">www.kkzjc.com</a>”对应的 Web 服务对外开放的端口</em>：32000（<strong>题4答案</strong>）</li>
<li>proxy-pass反向代理转发给8091端口</li>
</ol>
<p>但8091端口在netstat命令后并未显示<br>
查看历史：</p>
<pre><code>history
</code></pre>
<p><img src="https://s2.loli.net/2022/07/04/H431UBgIu6s2GbN.png" alt="image.png"></p>
<p>发现使用很多docker命令<br>
首先打开未启动的docker：</p>
<pre><code>systemctl start docker
docker ps -a
</code></pre>
<p><em>（-a包括未启动的容器）</em></p>
<p><img src="https://s2.loli.net/2022/07/04/d3q9J1tLXn7DoyY.png" alt="image.png"></p>
<p>此时再使用netstat查看网络连接，发现8091端口：</p>
<p><img src="https://s2.loli.net/2022/07/04/zy9qUdcOljY4fxo.png" alt="image.png"></p>
<p>其中的30000、31000、32000都实际转发到8091端口，实现一级跳板：</p>
<p><img src="https://img-blog.csdnimg.cn/20201121205650340.png#pic_center" alt=""></p>
<p>进入容器中查看一下：</p>
<pre><code>docker exec -it 08 /bin/bash
</code></pre>
<p><em>（ exec 在运行状态下的容器中执行命令 /bin/bash解释执行模式 ）</em><br>
容器中同样进行上述分析：</p>
<ul>
<li>查看history（分析服务器可以先看一下）</li>
<li>发现用到多次nginx：<code>more /etc/nginx/nginx.conf</code></li>
<li>发现 include：/etc/nginx/conf.d/*.conf</li>
<li>到该路径下：<code>cd /etc/nginx/conf.d/</code>  <code>ls</code></li>
<li>目录下有：hl.conf</li>
<li>查看配置文件：<code>more hl.conf</code></li>
</ul>
<p><img src="https://s2.loli.net/2022/07/05/tbErldXaGFIgxwo.png" alt="image.png"></p>
<p>可以得到：</p>
<ol>
<li>监听docker内部80端口</li>
<li>proxy_pass到192.168.1.176实现跳板（<strong>题8答案</strong>）</li>
</ol>
<pre><code>exit
</code></pre>
<h3 id="第7题：查看远程登录使用的ip地址"><em>第7题：查看远程登录使用的IP地址</em></h3>
<pre><code>last
</code></pre>
<p><em>（显示用户最近登录信息）</em></p>
<p><img src="https://s2.loli.net/2022/07/05/4LWia69keVtrhqK.png" alt="image.png"></p>
<p>192.168.99.222即为<strong>题7答案</strong></p>
<h3 id="题6题9："><em>题6题9</em>：</h3>
<p>使用<code>docker exec -it 08 /bin/bash</code>进入容器，<code>cat /etc/nginx/nginx.conf</code>中得知nginx日志access.log位置：</p>
<p><img src="https://s2.loli.net/2022/07/05/8lh5atUuRiVLNfz.png" alt="image.png"></p>
<p>进入该目录<code>cd /var/log/nginx</code>,<code>cat access.log</code>无反应，<code>ls -l</code>（ -l 得到一个详细的文件和目录名列表）发现 access.log 重定向到其他位置，为链接无法查看：</p>
<p><img src="https://s2.loli.net/2022/07/05/kpcW6xNzECOMrXf.png" alt="image.png"></p>
<p>尝试其他方法</p>
<pre><code>docker logs 08
</code></pre>
<p>（查看docker日志）</p>
<p><img src="https://s2.loli.net/2022/07/05/18gDAcOhKCFrvpI.png" alt="image.png"></p>
<p>（99.3为IP访问 下面几行为域名访问）<br>
192.168.99.3即为<strong>题6</strong>答案：服务器所在原始IP</p>
<p>题9统计数量即可：</p>
<pre><code>docker logs 08 | grep 192.168.99.222 | wc -l
</code></pre>
<p><em>（| 管道  grep 筛选前面管道传来的信息  wc -l 统计行数）</em><br>
得出18，即为<strong>题9</strong>答案</p>
<hr>
<h2 id="检材二">检材二</h2>
<p><strong>题6</strong>的简单解：<br>
Chrome浏览记录：</p>
<p><img src="https://s2.loli.net/2022/07/05/bLPQJ6qvnAcxGuK.png" alt="image.png"></p>
<p>192.168.99.3即为<strong>题6</strong>服务器原始ip地址</p>
<p>或通过查看/Windows/System32/drivers/etc/hosts</p>
<p><img src="https://s2.loli.net/2022/07/05/6t9Bjpge3hyK1wN.png" alt="image.png"></p>
<p><em>（hosts：将一些常用的网址域名与其对应的 IP 地址建立关联）</em></p>
<h3 id="检材2的sha值：">10. <em>检材2的SHA值</em>：</h3>
<p>使用火眼直接计算即可<br>
2D926D5E1CFE27553AE59B6152E038560D64E7837ECCAD30F2FBAD5052FABF37</p>
<h3 id="os内部版本号：">11. <em>OS内部版本号</em>：</h3>
<p>系统-设置-关于-Windows规格-操作系统版本：18363.1082</p>
<p><img src="https://s2.loli.net/2022/07/05/liSXB2ApVQ1TEHb.png" alt="image.png"></p>
<h3 id="最后一次正常关机时间：">12. 最后一次正常关机时间：</h3>
<p>火眼证据分析：<br>
分析——基本信息——操作系统信息——最后一次正常关机时间</p>
<h3 id="vmware安装时间：">13. Vmware安装时间：</h3>
<p>……</p>
<h3 id="vmware启动次数：">14. Vmware启动次数：</h3>
<p>……</p>
<h3 id="嫌疑人通过web从检材2访问检材1连接的目标端口：">15. 嫌疑人通过Web从检材2访问检材1连接的目标端口：</h3>
<p>8091（见第六题）</p>
<h3 id="该端口上运行的进程的程序名称（program-name）为：">16. 该端口上运行的进程的程序名称（Program name）为：</h3>
<p>docker-proxy（见服务器分析打开docker后的netstat）</p>
<h3 id="嫌疑人从检材-2-上访问该网站时，所使用的域名为：">17. 嫌疑人从检材 2 上访问该网站时，所使用的域名为：</h3>
<p>取证分析中查看Chrome浏览记录：<a href="http://www.sdhj.com">www.sdhj.com</a></p>
<h3 id="嫌疑人所使用的微信-id-是：">18. 嫌疑人所使用的微信 ID 是：</h3>
<p>取证分析中查看用户痕迹，发现my iphone.tar.Ink，找到所在位置，导出文件，在取证分析中添加该镜像，即可对手机进行分析：<img src="https://s2.loli.net/2022/07/06/YwukBhPWzJ3N564.png" alt=""></p>
<p>微信ID为：sstt119999</p>
<h3 id="嫌疑人为与广告位供应商沟通时使用的通联工具的名称为：">19. 嫌疑人为与广告位供应商沟通时使用的通联工具的名称为：</h3>
<p>telegram<img src="https://s2.loli.net/2022/07/06/mfvyqZOCSszaBF2.png" alt=""></p>
<h3 id="嫌疑人使用何种虚拟货币与供应商进行交易：">20. 嫌疑人使用何种虚拟货币与供应商进行交易：</h3>
<p>微信聊天记录：dogcoin</p>
<p><img src="https://s2.loli.net/2022/07/06/jifABrgnLcqxy51.png" alt=""></p>
<h3 id="对方的收款地址是：">21. 对方的收款地址是：</h3>
<p>DPBEgbwap7VW5HbNdGi9TyKJbqTLWYYkvf（扫描二维码）</p>
<h3 id="交易时间：">22. 交易时间：</h3>
<p><img src="https://s2.loli.net/2022/07/06/nYkgFdEGXiA586Z.png" alt=""></p>
<h3 id="支付货币数量：">23. 支付货币数量：</h3>
<p>4000（见上题）</p>
<h3 id="，嫌疑人使用的虚拟机的虚拟磁盘被加密，密码为：">24.，嫌疑人使用的虚拟机的虚拟磁盘被加密，密码为：</h3>
<p>打开仿真虚拟机中的VMware，找到镜像所在位置</p>
<p><img src="https://s2.loli.net/2022/07/06/gyJcpMRaEoI8Ywb.png" alt=""></p>
<p>导出路径下的vmx文件到pyvmx-cracker-master目录下，将其改名为“ 1.vmx ”（方便操作），打开命令行，执行：<code>python .\pyvmx-cracker.py -v 1.vmx -d wordlist.txt</code>爆破密码：</p>
<p><img src="https://s2.loli.net/2022/07/06/lc6YFjry3maezER.png" alt=""></p>
<p><img src="https://s2.loli.net/2022/07/06/FaIKEkDeXJOP57c.png" alt=""></p>
<p>得出密码为：zzzxxx</p>
<h3 id="嫌疑人发送给广告商的邮件中的图片附件的-sha256-值为：">25. 嫌疑人发送给广告商的邮件中的图片附件的 SHA256 值为：</h3>
<p>在虚拟机设置——选项——访问控制中移除密码：</p>
<p><img src="https://s2.loli.net/2022/07/06/v4dLu7BQqHfFl39.png" alt=""></p>
<p>即可把镜像挂载到取证软件下进行快速分析。</p>
<p>找到“广告”图片，导出。使用<code>certutil -hashfile "文件名" SHA256</code>计算哈希值：</p>
<p><img src="https://s2.loli.net/2022/07/06/qPz2UioDNgxj57A.png" alt=""></p>
<h3 id="检材-2-中，嫌疑人给广告商发送广告图片邮件的发送时间是">26.检材 2 中，嫌疑人给广告商发送广告图片邮件的发送时间是:</h3>
<p>2020-09-20 12:53:00</p>
<h3 id="嫌疑人的邮箱密码是">27. 嫌疑人的邮箱密码是:</h3>
<p>honglian7001</p>
<h3 id="嫌疑人使用了哪种远程管理工具，登录了检材-1-所在的服务器：">28. 嫌疑人使用了哪种远程管理工具，登录了检材 1 所在的服务器：</h3>
<p>Xshell</p>
<h3 id="使用上述工具连接服务器时，使用的登录密码为">29. 使用上述工具连接服务器时，使用的登录密码为</h3>
<p><img src="https://s2.loli.net/2022/07/06/qWROCmHPnFteLc5.png" alt=""></p>
<hr>
<h2 id="检材3">检材3</h2>
<h3 id="检材-3-的原始磁盘-sha256-值：">30. 检材 3 的原始磁盘 SHA256 值：</h3>
<p><img src="https://s2.loli.net/2022/07/06/5LOpsFUjc4qD1KM.png" alt=""></p>
<h3 id="操作系统版本：-1">31. 操作系统版本：</h3>
<p>Windows Server 2008 HPC Edition</p>
<h3 id="检材-3-中，部署的网站名称是：">32.检材 3 中，部署的网站名称是：</h3>
<p>服务器管理器中查看：</p>
<p><img src="https://s2.loli.net/2022/07/06/HBUu5yTv9YMXckP.png" alt=""></p>
<p>名称为：card</p>
<h3 id="部署的网站对应的网站根目录是：">33. 部署的网站对应的网站根目录是：</h3>
<p>C:\inetpub\wwwroot\v7w（见上图）</p>
<h3 id="部署的网站绑定的端口是：">34. 部署的网站绑定的端口是：</h3>
<p>80（见上图）</p>
<h3 id="具备登陆功能的代码页，对应的文件名为：">35. 具备登陆功能的代码页，对应的文件名为：</h3>
<p>找到配置文件位置：/inetpub/wwwroot/v7w</p>
<p>先查看Web.config：</p>
<p><img src="https://s2.loli.net/2022/07/06/e1G3FwJTl2yjbR6.png" alt=""></p>
<p>根据检材二中的Chrome浏览记录，发现嫌疑人是从 <a href="http://sdhj.com/dl">sdhj.com/dl</a> 访问网站，故被重定向到dllogin.aspx文件</p>
<h3 id="对网站代码进行分析，网站登录过程中，代码中对输入的明文密码作了追加什么字符串处理：">36. 对网站代码进行分析，网站登录过程中，代码中对输入的明文密码作了追加什么字符串处理：</h3>
<p>查看dllogin.aspx：</p>
<p><img src="https://s2.loli.net/2022/07/06/xq6KJwbQhktdFuD.png" alt=""></p>
<p>追加字符为0v0</p>
<h3 id="请对网站代码进行分析，网站登录过程中，代码中调用的动态扩展库文件的完整名称为：">37. 请对网站代码进行分析，网站登录过程中，代码中调用的动态扩展库文件的完整名称为：</h3>
<p>App_Web_dllogin.aspx.7d7c2f33.dll（见上图）</p>
<h3 id="网站登录过程中，后台接收到明文密码后进行加密处理，首先使用的算法是-encryption-中-的哪个函数：">38. 网站登录过程中，后台接收到明文密码后进行加密处理，首先使用的算法是 Encryption 中 的哪个函数：</h3>
<p>使用 dnSpy 在 bin 文件夹中找到上述动态扩展库文件，找到主要方法：</p>
<p><img src="https://s2.loli.net/2022/07/06/7PFaRqbVSI6pTuy.png" alt=""></p>
<p>首先使用的算法为：AESEncrypt<br>
（审阅代码，先进行AES加密，AES加密函数中最后转化为base64，然后转化MD5）</p>
<h3 id="分析该网站连接的数据库地址为：">39. 分析该网站连接的数据库地址为：</h3>
<p>如上图，比较用户输入的密码和数据库密码由 DUserLogin 实现，查看该函数：</p>
<p><img src="https://s2.loli.net/2022/07/06/dKu4wyPci51BW6s.png" alt=""></p>
<p>DBcon 应为数据库中正确密码和 IP 地址，查看：</p>
<p><img src="https://s2.loli.net/2022/07/06/4Pxbrls3fpvVQ7g.png" alt=""></p>
<p>直接使用 Powershell 调用解密函数 AESDecrypt ：</p>
<pre><code>Add-Type -Path .\DBManager.dll
[DBManager.Encryption]::AESDecrypt("Mcyj19i/VubvqSM21YPjWnnGzk8G/GG6x9+qwdcOJd9bTEyprEOxs8TD9Ma1Lz1Ct72xlK/g8DDRAQ+X0GtJ8w==", "HL", "forensix")
</code></pre>
<p><img src="https://s2.loli.net/2022/07/06/oHcnIQFs4mM3r61.png" alt=""></p>
<p>192.168.1.174即为连接的数据库地址</p>
<h3 id="网站连接数据库使用的密码为：">40. 网站连接数据库使用的密码为：</h3>
<p>c4f2737e88（见 39 题）</p>
<h3 id="网站连接数据库服务器的端口是：">41. 网站连接数据库服务器的端口是：</h3>
<p>1433（见39题）</p>
<hr>
<h2 id="检材4">检材4</h2>
<h3 id="sha值：">42. SHA值：</h3>
<p>E5E548CCAECD02608A0E053A5C7DCDBFBDD4BE5B0E743EB9DC8E318C369BEBC8</p>
<h3 id="：至今navicat连不上，疯了。">43-50：至今Navicat连不上，疯了。</h3>

