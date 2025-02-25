介绍：

本示例配置为 trojan-go 或 trojan 应用。trojan-go 或 trojan 服务端前置（监听 443 端口）处理来自墙内的 HTTPS 请求，如果是合法的 trojan-go 或 trojan 客户端请求，那么为该请求提供服务（科学上网）；否则将已解除 TLS 的流量请求回落（转发）给 WEB 服务器 caddy，由 caddy 为其提供服务。

原理：

默认流程：trojan-go/trojan client <------ HTTPS ------> trojan-go/trojan server  
回落流程：WEB client <------------- HTTPS ------------> trojan-go/trojan server <-- 回落 --> caddy（WEB server）

注意：

1、caddy 版本不小于 v2.3.0 才支持 Caddyfile 配置开启 H2C server。

2、caddy 支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程（Unix Domain Socket 应用）。

3、本示例 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可（效果一样）。

4、因 trojan-go 或 trojan 不支持 Unix Domain Socket，故回落仅端口回落。

5、因 trojan-go 或 trojan 不支持 PROXY protocol，故回落不能启用此项应用。

6、trojan-go 完全兼容 trojan，服务端还有自己的特色：支持 trojan 应用与自己的 Websocket 应用共存；支持 CDN 流量中转(基于 WebSocket over TLS)；支持使用 AEAD 对 trojan 协议流量进行二次加密(基于 Shadowsocks AEAD)。

7、不要使用非 caddy（自带 ACME 客户端）的 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需监听 80 端口（或 443 端口），从而与当前应用端口冲突。
