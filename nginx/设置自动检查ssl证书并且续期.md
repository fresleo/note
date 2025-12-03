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


#第三步：在宿主机创建 API Token 文件（cf.ini）
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkxMjU0ODY1Ml19
-->