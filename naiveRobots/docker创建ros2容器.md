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


进入容器：

`docker exec -it ros2_container bash` 

你就进入了 ROS2 的终端环境，可以执行：

`ros2 run demo_nodes_cpp talker
ros2 run demo_nodes_cpp listener`


# ✅ 1. `ros2 run demo_nodes_cpp talker`

-   **作用**：启动一个 **Publisher 节点**
    
-   **来源**：属于 ROS2 自带的示例包 `demo_nodes_cpp`
    
-   **功能**：向名为 `/chatter` 的 Topic 不停地发布消息（字符串 “Hello World: N”）
    
-   **原理**：
    
    1.  节点名：`talker`
        
    2.  Topic 名：`/chatter`
        
    3.  消息类型：`std_msgs/msg/String`
        
    4.  周期性发送消息
        

> 简单比喻：`talker` 就像机器人的“嘴巴”，不停说话，把信息广播出去。

----------

# ✅ 2. `ros2 run demo_nodes_cpp listener`

-   **作用**：启动一个 **Subscriber 节点**
    
-   **来源**：同样属于 `demo_nodes_cpp` 包
    
-   **功能**：订阅 `/chatter` Topic，接收 `talker` 发布的消息，并打印在终端
    
-   **原理**：
    
    1.  节点名：`listener`
        
    2.  订阅 Topic：`/chatter`
        
    3.  收到消息就显示
        

> 简单比喻：`listener` 就像机器人的“耳朵”，听到 talker 说的话就显示出来。


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ1Mzc1NjI2OSwxMzcyNzI4OTY1XX0=
-->