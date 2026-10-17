
安装sui
VERSION=1.2.2 && bash <(curl -Ls https://raw.githubusercontent.com/bulianglin/demo/main/s-ui-install.sh) $VERSION


网页：
http://117.55.226.38:2095/app/



选择最优节点：
for d in aadcdn.msftauth.net tag-logger.demandbase.com github.gallerycdn.vsassets.io downloaddispatch.itunes.apple.com configuration.ls.apple.com cdn-dynmedia-1.microsoft.com www.xilinx.com apps.mzstatic.com d.oracleinfinity.io d0.m.awsstatic.com ; do t1=$(date +%s%3N); timeout 1 openssl s_client -connect $d:443 -servername $d </dev/null &>/dev/null && t2=$(date +%s%3N) && echo "$d: $((t2 - t1)) ms" || echo "$d: timeout"; done
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzg2MDIyNTI5XX0=
-->