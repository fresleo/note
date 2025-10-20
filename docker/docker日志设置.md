1.docker-compose中设置轮转（是 针对单个容器（service）生效 的。）

    services:
      app:
        image: your_image
        logging:
          driver: "json-file"
          options:
            max-size: "10m"
            max-file: "3"

2.让所有容器都默认受限制
在 **/etc/docker/daemon.json** 里加上同样配置：

    {
      "log-driver": "json-file",
      "log-opts": {
        "max-size": "10m",
        "max-file": "3"
      }
    }

然后：

    systemctl restart docker

查看是否生效：

✅ 一、查看 Docker 全局配置是否启用

运行以下命令：

    docker info | grep -A3 "Logging Driver"

输出示例：

    Logging Driver: json-file
    Cgroup Driver: systemd
    Cgroup Version: 2

👉 如果显示 Logging Driver: json-file
说明全局驱动确实是 json-file（和你在 /etc/docker/daemon.json 中设置的一致）


查询容器的日志是否按照json中一致：
docker inspect backend | grep -A5 LogConfig

如果输出和json文件中一致就行，如果不对那么重启docker容器就可以

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzY5MzkwOTg3XX0=
-->