介绍：

Xray 前置（监听 443 端口），利用 VLESS+Vision+REALITY 回落（套娃） Shadowsocks+gRPC，实现 VLESS+Vision+REALITY 与 Shadowsocks+gRPC+REALITY 应用共用 443 端口，其应用如下：

1、M=VLESS+Vision+REALITY（回落与转发配置，REALITY由自己启用及处理。）

2、L=Shadowsocks+gRPC+REALITY（REALITY由VLESS+Vision+REALITY启用及处理，不需配置。）

注意：

1、Xray 版本不小于 v1.8.0 才支持 REALITY 及同步 uTLS（强制客户端必须使用指纹伪造）。

2、Xray 的监听地址不支持 Shadowsocks（SS） 协议使用 UDS 监听。

3、若目标网站使用外部的，其最低标准：国外网站，支持 TLSv1.3 与 HTTP/2，域名非跳转；是否符合标准可用 [SSL Server Test](https://www.ssllabs.com/ssltest/) 检查。

4、若目标网站使用自己的，网站及关联配置可参考 [Xray(M+K+B+G+A)+Nginx](https://github.com/lxhao61/integrated-examples/tree/main/Xray(M%2BK%2BB%2BG%2BA)%2BNginx) 或 [Xray(M+K+B+G+A)+Caddy(N)](https://github.com/lxhao61/integrated-examples/tree/main/Xray(M%2BK%2BB%2BG%2BA)%2BCaddy(N)) 配置。
