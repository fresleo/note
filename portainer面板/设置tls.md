步骤 1: 生成TLS 证书

    mkdir tls
    cd tls

 生成CA证书
 

    openssl genrsa -aes256 -out ca-key.pem 4096
    openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

为服务器生成密钥和证书签名请求 (CSR)

    openssl genrsa -out server-key.pem 4096
    openssl req -subj "/CN=docker-server" -new -key server-key.pem -out server.csr
    openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem


 为客户端生成密钥和证书
 

    openssl genrsa -out key.pem 4096
    openssl req -subj "/CN=docker-client" -new -key key.pem -out client.csr
    openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem


 配置 Docker 引擎支持 TLS
修改docker-compose.yml文件：

      portainer:
        image: portainer/portainer-ce:latest
        container_name: portainer
        restart: always
        ports:
          - "9000:9000"  # 映射 Portainer 的 Web 界面到主机的 9000 端口
          - "9443:9443"  # HTTPS
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock  # 挂载 Docker 套接字
          - portainer_data:/data  # 使用命名卷 portainer_data
          - /etc/timezone:/etc/timezone:ro
          - /etc/localtime:/etc/localtime:ro
          - ./ssl/portainer:/etc/ssl # 挂载您的证书
        environment:
          - TLS_CERT=/etc/ssl/server-cert.pem
          - TLS_KEY=/etc/ssl/server-key.pem

设置 Portainer TLS

在 Portainer Web 界面中：

传入server-crt.pem
key传入：server-key.pem，然后访问9443接口


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMyODc2Njc1M119
-->