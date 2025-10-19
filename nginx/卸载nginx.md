1. 停止 Nginx 服务
首先停止 Nginx 服务，确保它不再运行。

    sudo systemctl stop nginx

2. 禁用 Nginx 自启动
为了防止 Nginx 在系统重启时自动启动，您可以禁用它。

    sudo systemctl disable nginx

3. 卸载 Nginx 软件包
接下来，您可以卸载 Nginx。

如果您只想卸载 Nginx 而保留配置文件：

    sudo apt-get remove nginx

4. 删除不再需要的依赖包

    sudo apt-get autoremove

5. 清理 Nginx 配置文件（如果您不需要保留配置文件）
即使您已使用 purge 命令卸载 Nginx，某些配置文件可能仍然存在。如果您想彻底删除所有相关文件，可以手动删除 Nginx 配置目录：

    sudo rm -rf /etc/nginx

6. 检查是否完全卸载
确认 Nginx 已被卸载：

    which nginx

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDczOTAyMzAzXX0=
-->