---


---

**<p>步骤 1: 在 Cloudflare 注册账户并添加域名</p>**
<p>注册并登录到 Cloudflare：</p>
<p>访问 Cloudflare 官网 并创建一个账户，或者登录你现有的 Cloudflare 账户。</p>
<p>添加域名到 Cloudflare：</p>
<p>在 Cloudflare 仪表板中，点击 “Add a site”。<br>
输入你在 NameSilo 或其他注册商购买的域名（例如 <a href="http://example.com">example.com</a>），然后点击 “Add Site”。</p>
<p>选择计划：</p>
<p>Cloudflare 提供多个计划，包括免费版、Pro 版等。选择 Free Plan 计划即可继续。</p>
<p>步骤 2: 更新 DNS 记录<br>
在添加域名并扫描完 DNS 记录后，Cloudflare 会要求你验证并更新域名的 DNS 记录。这一步骤是将域名解析到你的服务器 IP 地址。</p>
<p>编辑 DNS 记录：</p>
<p>进入 DNS 配置页面，你会看到默认的 DNS 记录。</p>
<p>需要确保将域名的 A 记录指向你自己的服务器 IP 地址。如果你是使用主机的公网上的 IP 地址，可以通过以下方式进行配置。</p>
<p>A 记录：</p>
<p>Name: @ 或你的域名（例如 <a href="http://example.com">example.com</a>）<br>
Type: A<br>
Content: 你的服务器的公网 IP 地址（例如 203.0.113.10）<br>
TTL: 设置为 Auto<br>
如果你有子域名（例如 <a href="http://www.example.com">www.example.com</a>），你可以添加一个 CNAME 记录指向主域名 <a href="http://example.com">example.com</a>，或者直接指向你服务器的 IP 地址。</p>
<p>CNAME 记录（如果需要）：<br>
Name: www<br>
Type: CNAME<br>
Content: <a href="http://example.com">example.com</a>（或者你的域名）<br>
保存更改：</p>
<p>保存 DNS 记录，Cloudflare 会提示你这些更改需要一段时间（通常几分钟到 24 小时）才能生效。</p>
<p>步骤 3: 更新 NameSilo 中的 Nameservers<br>
接下来，你需要在 NameSilo 中将域名的 Nameserver 修改为 Cloudflare 提供的服务器地址。这将使得 DNS 解析由 Cloudflare 处理。</p>
<p>获取 Cloudflare 的 Nameservers：<br>
在 Cloudflare 仪表板中，选择你的域名，进入 DNS 设置页面，Cloudflare 会为你提供两个 Nameserver 地址，通常如下所示：<br>
<a href="http://xxxx.ns.cloudflare.com">xxxx.ns.cloudflare.com</a><br>
<a href="http://yyyy.ns.cloudflare.com">yyyy.ns.cloudflare.com</a><br>
在 NameSilo 修改 Nameservers：<br>
登录到 NameSilo。<br>
进入你的域名管理页面。<br>
找到 Nameserver 设置，选择 Custom Nameservers，并将其设置为 Cloudflare 提供的两个 Nameserver 地址。<br>
保存更改：<br>
保存并提交修改，NameSilo 将会更新你域名的 Nameservers。</p>
<p>在配置文件中添加以下内容：<br><code>
server {<br>
listen 80;<br>
server_name <a href="http://example.com">example.com</a> <a href="http://www.example.com">www.example.com</a>;  # 使用你自己的域名</p>
<pre>location / {
    proxy_pass http://localhost:3000;  # 假设你希望将流量代理到本地的前端应用（如 Node.js 应用）
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
</pre>
}</code><br>
启用这个配置并重启 NGINX：
<p>sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/<br>
sudo systemctl restart nginx</p>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NTIzODY3OThdfQ==
-->