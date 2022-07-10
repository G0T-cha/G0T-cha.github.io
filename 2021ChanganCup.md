---


---

<h1 id="长安杯题解">2021长安杯题解</h1>
<h2 id="镜像挂载">镜像挂载</h2>
<p>使用VeraCrypt挂载并输入密码</p>
<hr>
<h2 id="检材一">检材一</h2>
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
<h2 id="检材二">检材二</h2>
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
<h2 id="检材三">检材三</h2>
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
<p>password_salt 为 public 中的方法：</p>
<p><img src="https://img2022.cnblogs.com/blog/2817142/202206/2817142-20220605223608828-523260372.png" alt=""></p>

