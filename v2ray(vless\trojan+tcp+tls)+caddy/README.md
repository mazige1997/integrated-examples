介绍：

本示例配置包含 vless+tcp+xtls 或 vless+tcp+tls 与 trojan+tcp+xtls 或 trojan+tcp+tls 应用。Xray 或 v2ray 服务端前置（监听 443 端口）处理来自墙内的 HTTPS 请求，如果是合法的 Xray 或 v2ray 客户端请求，那么为该请求提供服务（科学上网）；否则将已解除 TLS 的流量请求回落（转发）给 WEB 服务器 caddy，由 caddy 为其提供服务。

原理：

默认流程：Xray/v2ray client <------ TCP+TLS（HTTPS） ------> Xray/v2ray server  
回落流程：WEB client <----------------- HTTPS ----------------> Xray/v2ray server <-- 回落 --> caddy（WEB server）

其中 trojan+tcp+xtls 或 trojan+tcp+tls 应用还实现了兼容原版 trojan，即可使用原版 trojan 客户端连接。

注意：

1、v2ray 版本不小于 v4.31.0 才支持 trojan 协议。

2、caddy 版本不小于 v2.3.0 才支持 Caddyfile 配置开启 H2C server。

3、caddy 支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程（Unix Domain Socket 应用）。

4、caddy 发行版不支持接收 PROXY protocol。如要支持接收 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译，或下载本人 Releases 中编译好的 caddy 来使用即可。

5、本示例 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可。若使用 caddy 申请证书及密钥，推荐使用 json 格式配置，优化更好。

6、不要使用非 caddy（自带 ACME 客户端）的 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需监听 80 端口（或 443 端口），从而与当前应用端口冲突。

7、Xray 所需证书及密钥推荐使用 caddy 申请，配合 Xray（版本必须不低于v1.3.0）自动重载证书及密钥（OCSP Stapling），可实现证书及密钥申请与更新全自动化。

8、配置1：采用端口回落。配置2：采用进程回落。配置3：采用进程回落，且启用了 PROXY protocol。
