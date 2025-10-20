执行Mongo_ssl_generate.bat，生成密钥文件

上传密钥文件到服务器并且设置权限：

    cd /root/home/ssl/mongodb
    
    chown 999:999 mongodb.pem keyfile ca.pem
    
    chmod 400 mongodb.pem keyfile
    
    chmod 444 ca.pem

开启无认证模式的docker容器：
dockercompose配置：

    version: "3.8"
    
    services:
      mongodb:
        image: mongo:7.0.11
        container_name: mongodb
        restart: always
        ports:
          - "27017:27017"
        volumes:
          - mongodbdata:/data/db
          - ./ssl/mongodb:/etc/ssl/mongodb:ro
        command: >
          mongod --bind_ip_all
            --replSet rs0
            --tlsMode requireTLS
            --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem
            --tlsCAFile /etc/ssl/mongodb/ca.pem
        environment:
          - TZ=Asia/Shanghai


不指定keyfile，不使用authorize


查看是否启用了keyfile，
再容器内部执行：

    ps aux | grep mongod


检查是否无认证模式：

     docker exec -it mongodb mongosh --tls --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem --eval 'db.runCommand({ping: 1})'

无认证进行连接：

    docker exec -it mongodb mongosh --tls  --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem --eval


启用创建的用户进入容器：

    docker exec -it mongodb mongosh --tls --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem -u "replicaSetUser" -p 'replicaSetPassword!2024' --authenticationDatabase "admin"

通过 副本集名称 + 所有节点 URI 来连接：

    mongosh "mongodb://mongodb:27017,107.174.250.107:27017/mydatabase?replicaSet=rs0&readPreference=primaryPreferred" --tls --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem -u "replicaSetUser" -p 'replicaSetPassword!2024' --authenticationDatabase "admin"


主mongodb创建用户

replica用户：

    db.createUser({ user: "replicaSetUser",pwd: "replicaSetPassword!2024",roles: [ { role: "root", db: "admin" } ]})
backend用户：

db.createUser({ user: "adminMainUser",pwd: "YpwsYYDS!ThisY957337!",roles: [{ role: "readWrite", db: "mydatabase" }]})


执行初始化过程
在backend中进行初始化

确定实际读取节点
可以在 mongosh 中执行：
db.videos.find().limit(1).explain("executionStats")

在输出里找到：
"server": "mongodb:27017"


主节点为hostname：b476bf2625d7，为docker中的hostname信息





<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDk0MTMyNF19
-->