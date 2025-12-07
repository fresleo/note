# 2. 最简单的 `docker-compose.yml`

    version: "3.9"
    
    services:
      ros2:
        image: ros:humble
        container_name: ros2_container
        tty: true
        stdin_open: true
        network_mode: host  # Windows Mac 上可以去掉 host，使用端口映射
        volumes:
          - ./ros2_ws:/root/ros2_ws  # 持久化工作空间
        command: /bin/bash




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4NzgxNjkxMywxMzcyNzI4OTY1XX0=
-->