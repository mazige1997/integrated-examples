介绍：

Xray 或 v2ray 前置（监听 443 端口），利用 vless+tcp+xtls 或 vless+tcp+tls 强大的回落/分流 WebSocket（WS）特性与 nginx 为 gRPC 提供反向代理，实现除 Xray 或 v2ray 的 KCP 应用外共用 443 端口。其应用如下：

1、E=vless+tcp+xtls/tls（回落/分流配置，TLS由自己提供及处理。）

2、B=vless+ws+tls（TLS由vless+tcp+xtls/tls提供及处理，不需配置。另可改、可增其它WS类应用，参考对应的服务端单一应用配置示例。）

3、C=SS+v2ray-plugin+tls（TLS由vless+tcp+xtls/tls提供及处理，不需配置。）

4、G=vless+grpc+tls（TLS由vless+tcp+xtls/tls提供及处理，不需配置。另可改、可增其它gRPC类应用，参考对应的服务端单一应用配置示例。）

5、A=vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、Xray 版本不小于 v1.4.0 或 v2ray 版本不小于v4.36.2，才支持 gRPC 传输方式。

2、nginx 支持 H2C server 及 gRPC proxy，需要 nginx 包含 http_v2_module 模块。

3、nginx 支持 HTTP 功能块接收 PROXY protocol，需要 nginx 包含 http_realip_module 模块。

4、nginx 支持 H2C server，但不支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程（Unix Domain Socket 应用）；故回落配置就必须分成 http/1.1 回落与 h2 回落两部分，以便分别对应 nginx 的 HTTP/1.1 server 与 H2C server。

5、不要使用 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需监听 80 端口（或 443 端口），从而与当前应用端口冲突。

6、配置1：采用端口回落/分流、端口转发。配置2：采用进程回落/分流、进程转发。配置3：采用进程回落/分流、进程转发，且启用了 PROXY protocol。
