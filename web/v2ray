v2ray:bash <(curl -s -L https://git.io/v2ray-setup.sh)

如何检查 BBR 是否已开启？
请在 Debian 12 终端中运行以下两条命令：

1. 检查当前的拥塞控制算法

sysctl net.ipv4.tcp_congestion_control
如果返回： net.ipv4.tcp_congestion_control = bbr，说明已经开启了。

如果返回： net.ipv4.tcp_congestion_control = cubic，说明尚未开启。


如果没开，如何在 Debian 12 上开启 BBR？
开启 BBR 非常简单，不需要安装任何额外软件，只需两步：

echo "net.core.default_qdisc=fq" >> /etc/sysctl.d/99-bbr.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.d/99-bbr.conf
然后使配置生效：

sysctl --system
验证一下：

sysctl net.ipv4.tcp_congestion_control
这时候应该就会完美输出 net.ipv4.tcp_congestion_control = bbr 了！
