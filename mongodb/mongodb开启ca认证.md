1在 Docker 中使用 MongoDB CLI（命令行工具）来连接 MongoDB

进入正在运行的 MongoDB 容器

    docker exec -it <container_name_or_id> bash

2. 在 Docker 容器中安装 MongoDB CLI 工具

    apt-get update
    apt-get install -y mongodb-org-shell

3.修改 docker-compose.yml 启动 MongoDB（不启用 TLS）

    mongodb:
      image: mongo:7.0.11
      container_name: mongodb
      restart: always
      ports:
        - "27017:27017"
      volumes:
        - mongodbdata:/data/db
        - ./ssl/mongodb:/etc/ssl/mongodb  # 此处保留，后续会用于启用 TLS
      networks:
        - app-network
      command: >
        mongod --noauth --bind_ip 0.0.0.0

检查是否无认证模式： 

    docker exec -it mongodb mongosh --tls --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem --eval 'db.runCommand({ping: 1})'

4启动没有tsl的mongodb容器

    docker-compose up -d


连接到 MongoDB 并创建用户,使用mongosh

    docker exec -it mongodb  bash
    
    mongosh --host mongodb --port 27017 

    docker exec -it mongodb mongosh --tls  --tlsCAFile /etc/ssl/mongodb/ca.pem --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem --eval

    use admin

    db.createUser({ user: "adminMainUser",pwd: "YpwsYYDS!ThisY957337!",roles: [{ role: "readWrite", db: "mydatabase" }]})

超级管理员：

    db.createUser({ user: "adminMainUser",pwd: "YpwsYYDS!ThisY957337!",roles: [ { role: "root", db: "admin" } ]})

启动容器并开启ca验证

     command: mongod --auth --bind_ip 0.0.0.0 \
                      --tlsMode requireTLS \
                      --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem \
                      --tlsCAFile /etc/ssl/mongodb/ca.pem



可以用mongosh带 ca进入

    mongosh --host mongodb --port 27017 \
            --tls \
            --tlsCAFile /etc/ssl/mongodb/ca.pem \
            --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem \
            --username adminMainUser \
            --password 'YpwsYYDS!ThisY957337!' \
            --authenticationDatabase mydatabase


    mongodb://<credentials>@mongodb:27017/?directConnection=true&tls=true&tlsCAFile=%2Fetc%2Fssl%2Fmongodb%2Fca.pem&tlsCertificateKeyFile=%2Fetc%2Fssl%2Fmongodb%2Fmongodb.pem&authSource=mydatabase&appName=mongosh+2.2.6

注意：密码有特殊字符的时候要使用引号括起来，现在删除原来的用户，并创建新的用户

使用 dropUser 删除现有用户：

    db.dropUser("adminMainUser");





在 MongoDB 启用 TLS 和 CA 验证


MongoDB 配置了 TLS 和 CA 验证。
您有 CA 证书和客户端证书（如果使用了双向认证）。
在连接时指定 CA 证书，并在必要时提供客户端证书。

步骤 1：确保 MongoDB 配置了 TLS 和 CA 验证
您的 MongoDB 配置文件（通常是通过命令行传递给 mongod）应该如下所示：


 

    command: mongod --auth --bind_ip 0.0.0.0 \
                    --tlsMode requireTLS \
                    --tlsCertificateKeyFile /etc/ssl/mongodb/mongodb.pem \
                    --tlsCAFile /etc/ssl/mongodb/ca.pem
    		--tlsMode requireTLS：确保连接必须使用 TLS。
    		--tlsCertificateKeyFile /path/to/mongodb.pem：指定 MongoDB 服务器证书和密钥文件。
    		--tlsCAFile /path/to/ca.pem：指定用于验证客户端证书的 CA 证书。


步骤 2：使用 mongosh 连接时启用 TLS 和 CA 验证
假设您有以下文件：

ca.pem：MongoDB 服务器的 CA 根证书。
client.pem：客户端证书（如果使用双向认证，客户端也需要证书）。
连接命令格式：

 
mongosh --host <hostname> --port <port> \
        --tls \
        --tlsCAFile /path/to/ca.pem \
        --tlsCertificateKeyFile /path/to/client.pem
--tls：启用 TLS 加密。
--tlsCAFile /path/to/ca.pem：指定 CA 根证书，客户端用它来验证 MongoDB 服务器的证书。
--tlsCertificateKeyFile /path/to/client.pem：如果启用了双向 TLS，指定客户端证书。


示例：

 
mongosh --host mongodb --port 27017 \
        --tls \
        --tlsCAFile /path/to/ca.pem \
        --tlsCertificateKeyFile /path/to/client.pem
步骤 3：连接带认证（如果需要）
如果您的 MongoDB 配置了用户名和密码认证，您还需要提供认证信息：

 
 
mongosh "mongodb://<username>:<password>@<hostname>:<port>/<database>" \
        --tls \
        --tlsCAFile /path/to/ca.pem \
        --tlsCertificateKeyFile /path/to/client.pem
示例完整命令：
假设您的 CA 证书文件为 /etc/ssl/mongodb/ca.pem，客户端证书文件为 /etc/ssl/mongodb/client.pem，并且 MongoDB 的用户名为 admin，密码为 password，数据库名为 mydatabase，可以使用以下命令：


 
mongosh "mongodb://admin:password@mongodb:27017/mydatabase" \
        --tls \
        --tlsCAFile /etc/ssl/mongodb/ca.pem \
        --tlsCertificateKeyFile /etc/ssl/mongodb/client.pem
常见问题排查
证书格式问题：确保 .pem 格式的证书文件有效，并且没有损坏。
证书路径问题：确保在 --tlsCAFile 和 --tlsCertificateKeyFile 参数中提供的路径正确无误。
防火墙和端口：确保 Docker 容器和您的客户端之间可以正常访问 27017 端口。
MongoDB 认证问题：如果启用了认证，确保用户名和密码正确。
通过这种方式，您可以通过 mongosh 成功连接到启用了 TLS 和 CA 验证的 MongoDB 实例。







<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MjcxODk3MjddfQ==
-->