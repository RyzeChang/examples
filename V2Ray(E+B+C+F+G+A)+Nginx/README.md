介绍：

Xray 或 V2Ray 前置（监听 443 端口），利用 VLESS+TCP+TLS 回落及分流 WebSocket 特性、套娃 Trojan+TCP 与 Nginx 为 gRPC 提供反向代理，实现除 Xray 或 V2Ray 的 mKCP 应用外共用 443 端口，其应用如下：

1、E=VLESS+TCP+TLS（回落/分流配置，TLS由自己启用及处理。）

2、B=VMess+WebSocket+TLS（TLS由VLESS+TCP+TLS启用及处理，不需配置。）

3、C=Trojan+WebSocket+TLS（TLS由VLESS+TCP+TLS启用及处理，不需配置。）

4、F=Trojan+TCP+TLS（TLS由VLESS+TCP+TLS启用及处理，不需配置。）

5、G=Shadowsocks+gRPC+TLS（TLS由VLESS+TCP+TLS启用及处理，不需配置。）

6、A=VLESS+mKCP+seed

注意：

1、采用套娃方式实现共用 443 端口，仅需要一个域名及普通证书即可搞定，但套娃不支持 XTLS 应用。

2、Xray 或 V2Ray 的监听地址不支持 Shadowsocks（简称SS） 协议使用 UDS 监听。

3、Xray 版本不小于 v1.4.0 或 V2Ray 版本不小于 v4.36.2 才支持 gRPC 传输方式。

4、Nginx 支持 H2C server 及 gRPC proxy 需要 Nginx 包含 http_v2_module 模块。

5、Nginx 支持 HTTP 功能块接收 PROXY protocol 需要 Nginx 包含 http_realip_module 模块。

6、Nginx 支持 H2C server，但不支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程；故回落分成 http/1.1 回落与 h2 回落分别对应 Nginx 的 HTTP/1.1 server 与 H2C server。

7、套娃不支持 http/1.1 回落与 h2 回落分开（即 fallbacks 中 "alpn" 无效），而客户端连接一般自适应服务端配置，故套娃 Trojan+TCP 仅使用 h2 连接及回落。

8、不要使用 ACME 客户端在采用本示例的服务器上以 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书，因 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书需监听 80 或 443 端口，从而与当前应用端口冲突。

9、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接，且启用了 PROXY protocol。

10、本示例 F 兼容原版 Trojan 应用，F+C 组合等同于 Trojan-Go 应用；即可使用 Trojan 客户端 或 Trojan-Go 客户端对应连接。
