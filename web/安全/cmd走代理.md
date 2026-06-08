

**打开powershell**

    netsh winhttp set proxy 127.0.0.1:10808
        
    set http_proxy=http://127.0.0.1:10808 
    set https_proxy=http://127.0.0.1:10808


### 🌟 终极省心大招（永久生效）

因为用 `set` 定义的环境变量只要关闭 CMD 窗口就会失效。如果你不想每次打开终端都复制一遍，可以直接把它变成**系统永久环境变量**。

以**管理员身份**打开 CMD，运行以下两行：


    setx http_proxy "http://127.0.0.1:10808" /M
    setx https_proxy "http://127.0.0.1:10808" /M

**查看**

    netsh winhttp show proxy

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMDk1MjE3MDIsODgzMzA5N119
-->