生成证书，添加 SAN（推荐）
新建一个文件 openssl-san.cnf，内容如下：

    [ req ]
    default_bits       = 4096
    default_md         = sha256
    prompt             = no
    distinguished_name = req_distinguished_name
    req_extensions     = req_ext
    
    [ req_distinguished_name ]
    CN = RedisServer
    
    [ req_ext ]
    subjectAltName = @alt_names
    
    [ alt_names ]
    IP.1 = 192.168.31.48
    DNS.1 = redis-server.local


上面的ip代表：如果客户端通过 IP 地址连接服务器，证书的 SAN 中必须包含该 IP，否则会报错
DNS代表：如果客户端通过域名（例如 redis-server.local 或 example.com）连接服务器，证书的 SAN 中必须包含该域名。
1.windows下使用OpenSSL
安装后，将 OpenSSL 添加到系统环境变量 PATH 中，以便可以从命令行直接使用。

生成 TLS 证书和密钥文件

步骤 1: 生成 CA 证书和私钥

    openssl genrsa -out ca.key 4096
    openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt -subj "/CN=MyRedisCA"

步骤 2: 生成 Redis 服务器证书和密钥

    openssl genrsa -out redis.key 4096
    openssl req -new -key redis.key -out redis.csr -config openssl-san.cnf

使用 CA 签署服务器的证书请求，生成服务器证书：

    openssl x509 -req -in redis.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out redis.crt -days 3650 -extensions req_ext -extfile openssl-san.cnf

步骤 3: 验证生成的文件
查看 CA 证书：

    openssl x509 -in ca.crt -text -noout

查看服务器证书：

    openssl x509 -in redis.crt -text -noout

3. 配置 Redis 启用 TLS

    port 0
    tls-port 6379

证书和密钥文件路径

    tls-cert-file /etc/ssl/redis/redis.crt
    tls-key-file /etc/ssl/redis/redis.key
    tls-ca-cert-file /etc/ssl/redis/ca.crt

强制客户端验证（可选）
tls-auth-clients yes


使用redisinsight连接：
Use TLS打勾

VerifyTLSCertificate打勾并且放入ca.cert

RequiresTLSClientAuthentication打勾，并且放入redis.cert,

PrivateKey放入redis.key










<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgyMTU0ODA4OV19
-->