---


---

<h1 id="长安杯题解">2020长安杯题解</h1>
<h2 id="检材一">检材一</h2>
<ol>
<li><em>操作系统版本</em>：火眼中添加镜像即可查看</li>
<li><em>内核版本</em>：</li>
</ol>
<pre><code>uname -a
</code></pre>
<ol start="3">
<li><em>LVM（逻辑卷）的 LBA（逻辑区块地址）</em>：</li>
</ol>
<pre><code>fdisk -l
</code></pre>
<p><strong>分析服务器：</strong><br>
查看网络连接和端口：</p>
<pre><code>netstat -napt
</code></pre>
<p>（<u>-n 直接使用ip地址，而不通过域名服务器</u>）<br>
<img src="!%5B2022-07-04_105320.png%5D%28https://s2.loli.net/2022/07/04/vidHyjlBP1ZpMzo.png%29" alt=""><br>
获得信息：</p>
<ol>
<li>SSH(Secue Shell) 端口位于7001（远程连接服务器）</li>
<li>用到nginx反向代理</li>
</ol>
<p>查看nginx配置文件：</p>
<pre><code>cat  /etc/nginx/nginx.config
</code></pre>
<p><img alt="7204cd25c04fdfe12606783092f7d2eb.png"><br>
到所示目录下查看：</p>
<pre><code>cd  /etc/nginx/conf.d/
ls
</code></pre>
<p><img alt="27a7d5b6ba33d0855e85b17ed5976919.png"></p>
<blockquote>
<p>Written with <a href="https://stackedit.cn/">StackEdit</a>.</p>
</blockquote>
<blockquote>
<p>Written with <a href="https://stackedit.cn/">StackEdit</a>.</p>
</blockquote>

