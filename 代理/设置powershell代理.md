下面给出在 Windows PowerShell 中设置代理的常用方式。按需选择其一或组合使用。

一、仅当前会话生效（关闭窗口失效）

 基本代理（如需账号密码，按需加入 user:pass@）
 

    $env:HTTP_PROXY  = "http://proxy.host:8080"
    $env:HTTPS_PROXY = "http://proxy.host:8080"

 直连白名单（分号分隔，域可用前导点）
 

    $env:NO_PROXY    = "localhost;127.0.0.1;.company.local"

二、持久化到用户环境变量（新开终端后生效）

三、设置系统 WinHTTP 代理（部分系统服务/工具使用）  
需要管理员权限的 PowerShell。

四、PowerShell 自带网络命令使用代理

-   针对单次命令（Install-Module/Save-Module/Invoke-WebRequest/Invoke-RestMethod 等）：

五、取消当前会话代理

六、验证是否生效

提示

-   带鉴权代理可用 http://user:pass@proxy.host:port，但不建议明文保存；优先用 -ProxyCredential。
-   npm/yarn/git 等工具通常读取这些环境变量，但它们也有各自的独立代理配置（例如 npm config set proxy/https-proxy）。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MzgzMjM0MTVdfQ==
-->