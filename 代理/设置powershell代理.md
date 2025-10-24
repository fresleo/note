
下面给出在 Windows PowerShell 中设置代理的常用方式。按需选择其一或组合使用。

一、仅当前会话生效（关闭窗口失效）
````powershell
# 基本代理（如需账号密码，按需加入 user:pass@）
$env:HTTP_PROXY  = "http://proxy.host:8080"
$env:HTTPS_PROXY = "http://proxy.host:8080"
# 直连白名单（分号分隔，域可用前导点）
$env:NO_PROXY    = "localhost;127.0.0.1;.company.local"
````

二、持久化到用户环境变量（新开终端后生效）
````powershell
# 推荐方式（可清空删除）
[Environment]::SetEnvironmentVariable("HTTP_PROXY",  "http://proxy.host:8080", "User")
[Environment]::SetEnvironmentVariable("HTTPS_PROXY", "http://proxy.host:8080", "User")
[Environment]::SetEnvironmentVariable("NO_PROXY",    "localhost;127.0.0.1;.company.local", "User")

# 取消持久化
[Environment]::SetEnvironmentVariable("HTTP_PROXY",  $null, "User")
[Environment]::SetEnvironmentVariable("HTTPS_PROXY", $null, "User")
[Environment]::SetEnvironmentVariable("NO_PROXY",    $null, "User")
````

三、设置系统 WinHTTP 代理（部分系统服务/工具使用）
需要管理员权限的 PowerShell。
````powershell
# 设置
netsh winhttp set proxy proxy-server="http=proxy.host:8080;https=proxy.host:8080" bypass-list="localhost;127.0.0.1"
# 从 IE/系统代理导入
netsh winhttp import proxy source=ie
# 取消
netsh winhttp reset proxy
````

四、PowerShell 自带网络命令使用代理
- 针对单次命令（Install-Module/Save-Module/Invoke-WebRequest/Invoke-RestMethod 等）：
````powershell
# 交互输入凭据更安全
$cred = Get-Credential
Install-Module PSGallery -Proxy "http://proxy.host:8080" -ProxyCredential $cred

# Invoke-WebRequest 示例
Invoke-WebRequest https://www.example.com -Proxy "http://proxy.host:8080" -ProxyCredential $cred
````

五、取消当前会话代理
````powershell
Remove-Item Env:HTTP_PROXY  -ErrorAction SilentlyContinue
Remove-Item Env:HTTPS_PROXY -ErrorAction SilentlyContinue
Remove-Item Env:NO_PROXY    -ErrorAction SilentlyContinue
````

六、验证是否生效
````powershell
$env:HTTP_PROXY; $env:HTTPS_PROXY; $env:NO_PROXY
# 简单联网测试
Invoke-RestMethod https://api.ipify.org?format=json
````

提示
- 带鉴权代理可用 http://user:pass@proxy.host:port，但不建议明文保存；优先用 -ProxyCredential。
- npm/yarn/git 等工具通常读取这些环境变量，但它们也有各自的独立代理配置（例如 npm config set proxy/https-proxy）。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTA1MTk5NDddfQ==
-->