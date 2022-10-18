---


---

<h1 id="长安杯题解">2021长安杯题解</h1>
<h2 id="镜像挂载">镜像挂载</h2>
<p>使用VeraCrypt挂载并输入密码</p>
<hr>
<h2 id="part1">PART1</h2>
<ul>
<li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 拖进雷电APP智能分析</li>
</ul>
<h3 id="检材一apk哈希值：">1. 检材一APK哈希值：</h3>
<p><img src="https://s2.loli.net/2022/07/07/VdRc3nTwWgsEHbi.png" alt=""></p>
<p>3fece1e93be4f422c8446b77b6863eb6a39f19d8fa71ff0250aac10f8bdde73a</p>
<h3 id="apk应用包名">2. APK应用包名</h3>
<p>plus.H5B8E45D3（见上图）</p>
<h3 id="该apk程序在封装服务商的应用唯一标识（appid）为：">3. 该APK程序在封装服务商的应用唯一标识（APPID）为：</h3>
<p><img src="https://s2.loli.net/2022/07/07/TkhLBGzqilSWO5e.png" alt=""></p>
<p>H5B8E45D3</p>
<h3 id="该apk具备下列哪些危险权限（多选）：-a.读取短信-b.读取通讯录-c.读取精确位置-d.修改通讯录-e.修改短信">4. 该APK具备下列哪些危险权限（多选）： A.读取短信 B.读取通讯录 C.读取精确位置 D.修改通讯录 E.修改短信</h3>
<p><img src="https://s2.loli.net/2022/07/07/dHaYOKvIx8Up69E.png" alt=""></p>
<p>ABCDE</p>
<h3 id="该apk发送回后台服务器的数据包含一下哪些内容（多选题）：a.手机通讯录-b.手机应用列表-c.手机号码-d.验证码-e.gps定位信息：">5. 该APK发送回后台服务器的数据包含一下哪些内容（多选题）：A.手机通讯录 B.手机应用列表 C.手机号码 D.验证码 E.GPS定位信息：</h3>
<p>ACDE（见第八题）</p>
<h3 id="该apk程序回传通讯录时使用的http请求方式为：">6. 该APK程序回传通讯录时,使用的http请求方式为：</h3>
<p>下载安卓模拟器：使用雷电进行抓包（记得选为代理抓包，HTTP抓包抓不到）：</p>
<p><img src="https://s2.loli.net/2022/07/08/dZsJg4F9cqDlMmC.png" alt=""></p>
<p>红框为虚拟机设置的通讯录</p>
<p><img src="https://s2.loli.net/2022/07/08/GQcPdzl7fwxXWFU.png" alt=""></p>
<p>POST</p>
<h3 id="该apk程序的回传地址域名为：">7. 该APK程序的回传地址域名为：</h3>
<p><a href="http://www.honglian7001.com">www.honglian7001.com</a> （见上题）</p>
<h3 id="该apk程序代码中配置的变量-apiserver-的值为：">8. 该APK程序代码中配置的变量 apiserver 的值为：</h3>
<p>审阅代码：使用雷电自带的jadx反编译</p>
<p><img src="https://s2.loli.net/2022/07/08/wde7NKaE1lFfmXb.png" alt=""></p>
<p>找到资源文件下的 index.html , 发现最后一段 Sojson 加密代码：</p>
<p><img src="https://s2.loli.net/2022/07/08/S8mprIQG9vVbxan.png" alt=""></p>
<p>复制下来在 <a href="https://jsdec.js.org/">https://jsdec.js.org/</a> 中进行解密，在VS中打开：</p>
<p><img src="https://s2.loli.net/2022/07/08/IdzN3JKgl1FRA25.png" alt=""></p>
<p>apiserver 变量值为http://www.honglian7001.com/api/uploads/</p>
<h3 id="分析该apk，发现该程序还具备获取短信回传到后台的功能，短信上传服务器接口地址为：">9. 分析该APK，发现该程序还具备获取短信回传到后台的功能，短信上传服务器接口地址为：</h3>
<p>审阅代码：</p>
<p><img src="https://s2.loli.net/2022/07/08/Ax5bqGrSoY9WiNL.png" alt=""></p>
<p><a href="http://www.honglian7001.com/api/uploads/apisms">http://www.honglian7001.com/api/uploads/apisms</a> 即为接口地址</p>
<p>（mui.ajax( url [,settings] )：<strong>-url</strong>：请求发送的目标地址， <strong>settings</strong>：key/value格式的json对象，用来配置ajax请求参数）</p>
<p><img src="https://s2.loli.net/2022/07/08/1dEbP3sGACqm6yT.png" alt=""></p>
<p>获取了经纬度，即GPS信息</p>
<h3 id="经分析，发现该apk在运行过程中会在手机中产生一个数据库文件，该文件的文件名为：">10. 经分析，发现该APK在运行过程中会在手机中产生一个数据库文件，该文件的文件名为：</h3>
<p>使用 Frida 对 apk 进行 hook（雷电自带）</p>
<p><img src="https://s2.loli.net/2022/07/08/92KJVv7EZTUhdBj.png" alt=""></p>
<p>test.db</p>
<h3 id="该数据库的初始密码为：">11. 该数据库的初始密码为：</h3>
<p>c74d97b01eae257e44aa9d5bade97baf（见上题）</p>
<hr>
<h2 id="part2">PART2</h2>
<h3 id="sha256值：">12. SHA256值：</h3>
<p>E6873068B83AF9988D297C6916329CEC9D8BCB672C6A894D393E68764391C589</p>
<h3 id="查询涉案于案发时间段内登陆服务器的ip地址为：">13. 查询涉案于案发时间段内登陆服务器的IP地址为：</h3>
<p>仿真检材二：</p>
<pre><code>last
</code></pre>
<p><img src="https://s2.loli.net/2022/07/08/MyGULNIVkHZrKc7.png" alt=""></p>
<p>192.168.110.203</p>
<h3 id="请对检材二进行分析，并回答该服务器在集群中承担的主要作用是：">14. 请对检材二进行分析，并回答该服务器在集群中承担的主要作用是：</h3>
<p>负载均衡服务器（见以下分析）</p>
<h3 id="上一题中，提到的主要功能对应的服务监听的端口为：">15. 上一题中，提到的主要功能对应的服务监听的端口为：</h3>
<p>查看 <code>history</code>：</p>
<p><img src="https://s2.loli.net/2022/07/09/5T6YsuASF7EgCnc.png" alt=""></p>
<p>发现服务器的目录文件就是<code>/opt/honglianjingsai</code>，看一下 README.txt：</p>
<p><img src="https://s2.loli.net/2022/07/09/QiHwsk6SCn1xfBT.png" alt=""></p>
<p>发现端口配置是在文件 const.js 修改，查看：</p>
<p><img src="https://s2.loli.net/2022/07/09/cFrMXYwGJBqv8oy.png" alt=""></p>
<p>配置端口为 80</p>
<h3 id="上一题中，提到的服务所使用的启动命令为：">16. 上一题中，提到的服务所使用的启动命令为：</h3>
<p>查看history：</p>
<p><img src="https://s2.loli.net/2022/07/09/49zdgT7DjbfXPuC.png" alt=""></p>
<p>reboot 重启后，只有此条命令可能是启动服务命令：</p>
<pre><code>node app.js
</code></pre>
<h3 id="经分析，该服务对于请求来源ip的处理依据是：根据请求源ip地址的第几位进行判断：">17. 经分析，该服务对于请求来源IP的处理依据是：根据请求源IP地址的第几位进行判断：</h3>
<p><img src="https://s2.loli.net/2022/07/09/nDvYOyuJ6KqzTro.png" alt=""></p>
<p>看到 Proxy 文件，看一下：</p>
<p><img src="https://s2.loli.net/2022/07/09/D3RA6Uz87yvT5dk.png" alt=""></p>
<p>可知是 IP 地址的第3位</p>
<p>（对ip进行判断，根据不同的条件转发到不同的服务器上，所以功能为<strong>负载均衡服务器</strong>）</p>
<h3 id="经分析，当判断条件小于50时，服务器会将该请求转发到ip为多少的服务器上：">18. 经分析，当判断条件小于50时，服务器会将该请求转发到IP为多少的服务器上：</h3>
<p>192.168.110.111（见上题）</p>
<h3 id="请分析，该服务器转发的目标服务器一共有几台：">19. 请分析，该服务器转发的目标服务器一共有几台：</h3>
<p>3（见上题）</p>
<h3 id="请分析，受害者通讯录被获取时，其设备的ip地址为：">20. 请分析，受害者通讯录被获取时，其设备的IP地址为：</h3>
<p>查看目录下<code>logs</code>：</p>
<p><img src="https://s2.loli.net/2022/07/09/nUCbLVcvqxiIrH3.png" alt=""></p>
<p>192.168.110.252</p>
<h3 id="请分析，受害者的通讯录被窃取之后，经由该服务器转发到了ip为多少的服务器上：">21.请分析，受害者的通讯录被窃取之后，经由该服务器转发到了IP为多少的服务器上：</h3>
<p>192.168.110.113（见上题）</p>
<hr>
<h2 id="part3">PART3</h2>
<h3 id="检材三的原始硬盘的sha256值为：">22. 检材三的原始硬盘的SHA256值为：</h3>
<p>205C1120874CE0E24ABFB3BB1525ACF330E05111E4AD1D323F3DEE59265306BF</p>
<h3 id="请分析第21题中，所指的服务器的开机密码为：">23. 请分析第21题中，所指的服务器的开机密码为：</h3>
<h3 id="嫌疑人架设网站使用了宝塔面板，请问面板的登陆用户名为：">24. 嫌疑人架设网站使用了宝塔面板，请问面板的登陆用户名为：</h3>
<pre><code>bt default
</code></pre>
<p><img src="https://s2.loli.net/2022/07/10/xmXSTjVyfNq3DvO.png" alt=""></p>
<p>可知用户名为：hl123</p>
<h3 id="请分析用于重置宝塔面板密码的函数名为">25. 请分析用于重置宝塔面板密码的函数名为</h3>
<p>使用账号密码在 <a href="http://192.168.110.113:8888/12345678/">http://192.168.110.113:8888/12345678/</a> 登录，提示密码错误，于是在虚拟机中修改密码：<code>bt</code></p>
<p><img src="https://s2.loli.net/2022/07/10/s3HhRL4TX9C6yMK.png" alt=""></p>
<p>重新登录，成功</p>
<p>题中要求分析函数名，则需审阅代码，<code>bt</code> 调用的为 /www/server/panel 路径下  <a href="http://tool.py">tool.py</a> 文件：</p>
<p><img src="https://s2.loli.net/2022/07/10/jaW9NHoiVXZA7g2.png" alt=""></p>
<p>调用的函数名为：set_panel_pwd</p>
<h3 id="请分析宝塔面板登陆密码的加密方式所使用的哈希算法为：">26. 请分析宝塔面板登陆密码的加密方式所使用的哈希算法为：</h3>
<p>找到函数：MD5</p>
<p><img src="https://s2.loli.net/2022/07/10/3tSze9g4Q7VAkul.png" alt=""></p>
<h3 id="请分析宝塔面板对于其默认用户的密码一共执行了几次上题中的哈希算法：">27. 请分析宝塔面板对于其默认用户的密码一共执行了几次上题中的哈希算法：</h3>
<p>password_salt 为 public 中的方法，找到public路径：</p>
<p><img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223608828-523260372.png" alt=""></p>
<p>定位到函数：</p>
<p><img src="https://s2.loli.net/2022/07/10/docIDXBAaVe1zpU.png" alt=""></p>
<p>用了两次 md5 ，加上前面一次，共三次</p>
<h3 id="请分析当前宝塔面板密码加密过程中所使用的salt值为：">28. 请分析当前宝塔面板密码加密过程中所使用的salt值为：</h3>
<p>由上题可知，salt 的获取调用了 M 方法，查找 <code>def M</code>:</p>
<p><img src="https://s2.loli.net/2022/07/11/DZtLTxglH1aEQ3w.png" alt=""></p>
<p>发现默认数据库为<code>/wwww/server/panel/data/default.db</code>，下载该文件，使用 <strong>DB Broswer for SQLite</strong> 打开数据库：</p>
<p><img src="https://s2.loli.net/2022/07/11/CTbsg6NI3Uk7YHi.png" alt=""></p>
<p>salt值为：v87ilhAVumZL</p>
<h3 id="请分析该服务器，网站源代码所在的绝对路径为">29. 请分析该服务器，网站源代码所在的绝对路径为</h3>
<p><img src="https://s2.loli.net/2022/07/11/znHhxvJ2q9McjFU.png" alt=""></p>
<p>绝对路径为：/www/wwwroot/www.honglian7001</p>
<h3 id="请分析，网站所使用的数据库位于ip为多少的服务器上：">30. 请分析，网站所使用的数据库位于IP为多少的服务器上：</h3>
<p>网站目录下找到database文件：</p>
<p><img src="https://s2.loli.net/2022/07/11/qSaEshQZxgXAbc2.png" alt=""></p>
<p>192.168.110.115</p>
<h3 id="数据库的登陆密码为">31. 数据库的登陆密码为:</h3>
<p>wxrM5GtNXk5k5EPX（见上题）</p>
<h3 id="请尝试重构该网站，并指出，该网站的后台管理界面的入口为：">32. 请尝试重构该网站，并指出，该网站的后台管理界面的入口为：</h3>
<p>检材四PC机Chrome浏览记录：<br>
<img src="https://s2.loli.net/2022/07/12/Hd8rWcMXYOht7ge.png" alt=""></p>
<h3 id="已该涉案网站代码中对登录用户的密码做了加密处理。请找出加密算法中的salt值：">33. 已该涉案网站代码中对登录用户的密码做了加密处理。请找出加密算法中的salt值：</h3>
<p><img src="https://s2.loli.net/2022/07/12/Mu2JPaX5Q6B9DVw.png" alt=""></p>
<h3 id="请分析该网站的管理员用户的密码为：">34. 请分析该网站的管理员用户的密码为：</h3>
<p>日志中查找 password:</p>
<p><img src="https://s2.loli.net/2022/07/12/noWXkDyzf1UTV7P.png" alt=""></p>
<h3 id="在对后台账号的密码加密处理过程中，后台一共计算几次哈希值">35. 在对后台账号的密码加密处理过程中，后台一共计算几次哈希值:</h3>
<p>3 （见 33 题）</p>
<h3 id="请统计，后台中，一共有多少条设备记录">36. 请统计，后台中，一共有多少条设备记录</h3>
<p>挂载检材五，数据库镜像被raid打碎：</p>
<p><img src="https://s2.loli.net/2022/07/12/zR9CrlnvwNq4KXQ.png" alt=""></p>
<p>使用 UFS Professional Recovery 重组, 导入镜像</p>
<p><img src="https://s2.loli.net/2022/07/12/PEqb84zK1olW6BG.png" alt=""></p>
<p>New RAID----Add to RAID</p>
<p><img src="https://s2.loli.net/2022/07/12/xaFVBC1lWZcbArn.png" alt=""></p>
<p>Build:</p>
<p><img src="https://s2.loli.net/2022/07/12/IKwJNzZXkyuSHlL.png" alt=""></p>
<p>Save disk image</p>
<p><img src="https://s2.loli.net/2022/07/12/xAeWLyzVf3gYTEc.png" alt=""></p>
<p>把保存的数据库镜像仿真，此时访问 <a href="http://192.168.110.113/admin">http://192.168.110.113/admin</a> 不再报错（缺失数据库），出现登录页面，使用 admin security 登录</p>
<p>用 Navicat 连接：</p>
<p><img src="https://s2.loli.net/2022/07/12/hMxVy1oW5CZLfIn.png" alt=""></p>
<p>设备记录：6002</p>
<p><img src="https://s2.loli.net/2022/07/12/IgaHfpQGLdh136V.png" alt=""></p>
<h3 id="请通过后台确认，本案中受害者的手机号码为：">37. 请通过后台确认，本案中受害者的手机号码为：</h3>
<p><img src="https://s2.loli.net/2022/07/12/fvmtQdbDEkCJ5aW.png" alt=""></p>
<p>和20题日志对比（20题为 UTC 时间，需要 -8），受害人手机号为18644099137</p>
<h3 id="请分析，本案中受害者的通讯录一共有多少条记录：">38. 请分析，本案中受害者的通讯录一共有多少条记录：</h3>
<p><img src="https://s2.loli.net/2022/07/12/4IoqCxL9Mfytkev.png" alt=""></p>
<p>34条</p>
<hr>
<h2 id="part4：">PART4：</h2>
<h3 id="请计算检材四-pc的原始硬盘的sha256值：">39. 请计算检材四-PC的原始硬盘的SHA256值：</h3>
<p>取证要求输入 bitlocker 密码，忽略，计算哈希值：</p>
<p>E9ABE6C8A51A633F809A3B9FE5CE80574AED133BC165B5E1B93109901BB94C2B</p>
<h3 id="检材四-pc的bitlocker加密分区的解密密钥为：">40. 检材四-PC的Bitlocker加密分区的解密密钥为：</h3>
<p>查看 txt 格式文件，发现密钥文件：</p>
<p><img src="https://s2.loli.net/2022/07/12/LBymb9W3ITMxklO.png" alt=""></p>
<p>511126-518936-161612-135234-698357-082929-144705-622578</p>
<h3 id="检材四-pc的开机密码为：">41. 检材四-PC的开机密码为：</h3>
<p>仿真中挂载，输入 bitlocker 密码，可查看开机密码：</p>
<p>12306</p>
<h3 id="检材四-pc是嫌疑人用于管理服务器的设备，其主要通过哪个浏览器控制网站后：">42. 检材四-PC是嫌疑人用于管理服务器的设备，其主要通过哪个浏览器控制网站后：</h3>
<p>查看浏览记录可知：</p>
<p>Chrome</p>
<h3 id="算pc检材中用户目录下的zip文件的sha256值：">43. 算PC检材中用户目录下的zip文件的sha256值：</h3>
<p>取证中直接右键计算即可</p>
<p><img src="https://s2.loli.net/2022/07/12/1aMsg2woCpGQZeR.png" alt=""></p>
<h3 id="检材四-phone，该手机的imei号为：">44. 检材四-phone，该手机的IMEI号为：</h3>
<p><img src="https://s2.loli.net/2022/07/12/9zD4paubqve3fSg.png" alt=""></p>
<h3 id="检材四-phone，嫌疑人和本案受害者是通过什么软件开始接触的：">45. 检材四-phone，嫌疑人和本案受害者是通过什么软件开始接触的：</h3>
<p>查看聊天记录：</p>
<p><img src="https://s2.loli.net/2022/07/12/XvCKZAVsuQ12BSG.png" alt=""></p>
<p>伊对</p>
<h3 id="检材四-phone，受害者下载恶意apk安装包的地址为">46. 检材四-phone，受害者下载恶意APK安装包的地址为</h3>
<p>查看伊对中聊天记录可知：</p>
<p><a href="https://cowtransfer.com/s/a6b28b4818904c">https://cowtransfer.com/s/a6b28b4818904c</a></p>
<h3 id="检材四-phone，受害者的微信内部id号为">47. 检材四-phone，受害者的微信内部ID号为</h3>
<p>查看微信聊天记录即得：wxid_op8i06j0aano22</p>
<h3 id="检材四-phone，嫌疑人用于敲诈本案受害者的qq账号为：">48. 检材四-phone，嫌疑人用于敲诈本案受害者的QQ账号为：</h3>
<p>查看QQ聊天记录：1649840939</p>
<h3 id="嫌疑人用于管理敲诈对象的容器文件的sha256值为：">49. 嫌疑人用于管理敲诈对象的容器文件的SHA256值为：</h3>
<p>解压“我的赚钱工具”，发现需要密码，没有任何提示：使用检材4开机密码12306（<s>脑洞？</s>）解压出虚拟机文件，	直接打开 vmx 文件开启虚拟机，需要开机密码，使用火眼仿真一下，得出密码：money，进入桌面，发现什么都没有：</p>
<p><img src="https://s2.loli.net/2022/07/12/Ze4ihUjuqScNdLV.png" alt=""></p>
<p>回头看解压出的文件夹，发现有两个 vmdk ，查看一下快照：</p>
<p><img src="https://s2.loli.net/2022/07/12/GMSdT7j1QIWmwFX.png" alt=""></p>
<p>恢复快照，桌面有东西了。</p>
<p>结合取证中的访问痕迹，发现：</p>
<p><img src="https://s2.loli.net/2022/07/12/GTl27mKjD94tex8.png" alt=""></p>
<p>计算哈希：</p>
<p><img src="https://s2.loli.net/2022/07/12/VO8LJP51chfMvH7.png" alt=""></p>
<h3 id="请综合分析嫌疑人检材，另外一受害者“郭先生”的手机号码为：">50.请综合分析嫌疑人检材，另外一受害者“郭先生”的手机号码为：</h3>
<p>在虚拟机中挂载<em>小白鼠.txt</em>：</p>
<p><img src="https://s2.loli.net/2022/07/12/rnz5ZQjLU2cg49F.png" alt=""></p>
<p>需要密码，查看取证软件：</p>
<p><img src="https://s2.loli.net/2022/07/12/gsIeKdHBOltXwUG.png" alt=""></p>
<p>用户痕迹中的 key 应是密钥文件，添加，挂载成功，打开该卷：</p>
<p><img src="https://s2.loli.net/2022/07/12/ZMGDNh8UTIn67ty.png" alt=""></p>
<p>可得手机号码</p>
<h3 id="通过嫌疑人检材，其中记录了几位受害者的信息：">51. 通过嫌疑人检材，其中记录了几位受害者的信息：</h3>
<p><img src="https://s2.loli.net/2022/07/12/rOgm8ZpcnY6kG9e.png" alt=""></p>
<p>5</p>
<h3 id="请使用第11题的密码解压“金先生转账.zip”文件，并对压缩包中的文件计算sha256值：">52. 请使用第11题的密码解压“金先生转账.zip”文件，并对压缩包中的文件计算SHA256值：</h3>
<p>解压，计算哈希值，可得：cd62a83690a53e5b441838bc55ab83be92ff5ed26ec646d43911f119c15df510</p>
<h3 id="请综合分析，受害者一共被嫌疑人敲诈了多少钱：">53. 请综合分析，受害者一共被嫌疑人敲诈了多少钱：</h3>
<p>微信两千：</p>
<p><img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223609070-2045151680.png" alt=""></p>
<p>伊对一千：</p>
<p><img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223608834-556812107.png" alt=""></p>
<p>QQ六百：</p>
<p><img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223608819-312786404.png" alt=""></p>
<p>压缩包两千：<br>
<img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223608857-1366895648.png" alt=""></p>
<p>数据库中一千：</p>
<p><img src="https://s2.loli.net/2022/07/12/ygAbOZVcpHLPehl.png" alt=""></p>
<p>（导出查看为一千）</p>
<p>总共6600元</p>



> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk0ODQ1MzAwOV19
-->