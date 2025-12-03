# 第一步：去 Cloudflare 获取 API Token
### 1. 登录 Cloudflare

进入：[https://dash.cloudflare.com/](https://dash.cloudflare.com/)

### 2. 进入 API Tokens 页面

右上角头像 → **My Profile**（我的资料）  
左侧菜单 → **API Tokens**

### 3. 点击 **Create Token**（创建 Token）

### 4. 选择模板：

找到并点击 **"Edit zone DNS"** 模板（这是官方推荐给 Certbot 的最小权限模板）。

> 如果你实在找不到模板，可以按下面的最小权限手动配置。
# 第二步：设置最小权限（最推荐）

如果使用自定义权限，请这样设置（最安全）：

### **Permissions**

`Zone → DNS → Edit` 

### **Zone Resources**

`Include → Specific zone → 选择你的 avmonkey.tv` 

### 完成 → Create Token

你会看到一个 Token 字符串，例如：

`qazwsx1234567890poiuytre...` 

记住：这个 Token 之后不能再次查看，如果忘记就要重新生成。


# 第三步：在宿主机创建 API Token 文件（cf.ini）

在你的服务器上运行：

`mkdir -p /root/.secrets
nano /root/.secrets/cf.ini` 

文件内容写：

`dns_cloudflare_api_token = YOUR_TOKEN_HERE` 

示例：

`dns_cloudflare_api_token = qazwsx1234567890poiuytre` 

保存并退出 (`Ctrl + O`, 回车, `Ctrl + X`)


# 第四步：设置文件权限（非常关键）

Certbot **要求权限必须是 600，否则会拒绝读取**：

`chmod 600 /root/.secrets/cf.ini` 

查看是否生效：

`ls -l /root/.secrets/cf.ini` 

应该显示类似：

`-rw------- 1 root root  70 Dec  3 20:10 cf.ini`

# 第五步：运行你的 Certbot Docker 命令

现在你的命令就能正常工作了：

```
docker run -it --rm -v /etc/letsencrypt:/etc/letsencrypt -v /root/.secrets/cf.ini:/etc/letsencrypt/cloudflare.ini:ro certbot/dns-cloudflare certonly --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini -d "*.avmonkey.tv" -d "avmonkey.tv"
```


# 自动续期（结合 **Certbot 的 renew 功能 + 定时任务（cron 或 systemd timer）**）


续签命令已经写好了（去掉-it模式）：
docker run --rm -v /etc/letsencrypt:/etc/letsencrypt -v /root/.secrets/cf.ini:/etc/letsencrypt/cloudflare.ini:ro certbot/dns-cloudflare renew --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini

# 在 cron 中添加任务
1.  编辑 root 的 crontab：
    

`sudo crontab -e` 

2.  添加一条每天凌晨执行的任务（假设每天 2 点）：
    

`0 2 * * * docker run --rm -v /etc/letsencrypt:/etc/letsencrypt -v /root/.secrets/cf.ini:/etc/letsencrypt/cloudflare.ini:ro certbot/dns-cloudflare renew --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini && systemctl reload nginx`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEyMDA5ODE5OCwtMTAxNzI5ODI2MCwtMj
EzMjcyMTMyMywxODk1NjM2NzAsODExOTgzNTAyXX0=
-->