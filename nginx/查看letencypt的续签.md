## 方法 1：使用 certbot 官方命令（最推荐）

`certbot certificates`


## 方法 2：使用 openssl 直接查看证书文件（最底层）

如果你知道证书路径（例如你的是 Cloudflare DNS + Certbot）：

`openssl x509 -in /etc/letsencrypt/live/你的域名/fullchain.pem -noout -dates`
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg1MzYzMDM0XX0=
-->