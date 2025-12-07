# 2. 最简单的 `docker-compose.yml`

`version:  "3.9"  services:  ros2:  image:  ros:humble  container_name:  ros2_container  tty:  true  stdin_open:  true  network_mode:  host  # Windows Mac 上可以去掉 host，使用端口映射  volumes:  -  ./ros2_ws:/root/ros2_ws  # 持久化工作空间  command:  /bin/bash` 

----------

# ✅ 3. 说明每一行

配置

作用

version

Docker Compose 版本

services

定义服务（容器）

image

使用官方 ROS2 Humble 镜像

container_name

容器名字

tty / stdin_open

保持交互终端可用

network_mode: host

让容器网络和宿主机一致（Mac/Win 可去掉，用端口映射）

volumes

映射本地工作空间到容器内

command


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ0MTg4MDEwNywxMzcyNzI4OTY1XX0=
-->