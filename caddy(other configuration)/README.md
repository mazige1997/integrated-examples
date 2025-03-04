一、回落后由 caddy 分流到不同网站的配置方法

此方法解决 Xray 或 v2ray 前置监听443后，不影响原来 caddy 前置时不同域名访问不同网站问题。见 shunt_caddy.json 或 shunt_Caddyfile 配置。

注意：

1、若不同域名没有使用通配符证书，那么还需要在 Xray 或 v2ray 中并列配置多个域名对应的证书及密钥。

2、此回落到不同网站是 Xray 或 v2ray 解除 TLS 后 caddy 进行的 host（域名）分流。

3、也可以用 caddy SNI 或 haproxy SNI 等分流来解决问题（不同方法，达到相同效果。）。caddy SNI 配置示例见如下介绍。haproxy SNI 配置示例参考用 haproxy SNI 分流的 haproxy 配置示例。

二、caddy SNI 分流的配置方法

此方法也可以解决 Xray 或 v2ray 应用与网站应用（原网站不想做回落网站，或 nginx、caddy 等有多个网站应用。）共用443端口问题。

注意：

1、caddy 加 caddy-l4 插件定制编译的才可以实现 SNI 分流，目前仅支持使用 json 配置。

2、1_SNI_caddy.json 采用 Local Loopback 连接，实现转发端口（域名）的分流，简称 caddy SNI 的端口分流。端口分流配置虽然效率稍低，但可适用任意系统服务器。

3、2_SNI_caddy.json 采用 Unix Domain Socket 连接，实现转发进程（域名）的分流，简称 caddy SNI 的进程分流。进程分流配置效率高，但在 Windows 10 Build 17036 之前版本不可用。

4、相关示例已配置 caddy SNI 分流共用端口，此配置仅备份及参考等。

三、caddy 以 DNS challenge 方式申请证书及密钥

1、以 DNS challenge 方式申请证书及密钥，普通证书与通配符证书都可以申请，不受限制。

2、Xray 服务端可以直接使用 caddy 申请的证书及密钥，配合Xray（版本必须不低于v1.3.0）自动重载证书及密钥（OCSP Stapling），可实现示例所需证书及密钥申请与更新全自动化。

3、其它服务端也可以直接使用 caddy 申请的证书及密钥，只是若自己不支持重载证书及密钥，caddy 证书及密钥到期更新后需重启自己来重新加载更新后的证书及密钥。

注意：

1、示例为通配符证书及密钥申请，普通证书及密钥或混合申请类似。

2、caddy 以 DNS challenge 方式申请证书及密钥，必须带对应 DNS API 插件。dnspod 解析分 dnspod.com（国际版）与 dnspod.cn（中国版），故两者插件不通用，必须对应各自 dnspod 解析使用。

3、cloudflare 已不支持 freenom 的免费域名以 DNS challenge 方式申请证书及密钥了。

4、acmeh 或 zerossl 申请成功后证书及密钥所在路径及目录。  
1）、acme 申请的普通证书及密钥在 “/home/tls/certificates/acme-v02.api.letsencrypt.org-directory/xx.yy” 目录中。/home/tls 为设置存放证书及密钥的基本路径。xx.yy 为域名，根据自己域名变化。  
2）、acme 申请的通配符证书及密钥在 “/home/tls/certificates/acme-v02.api.letsencrypt.org-directory/wildcard_.xx.yy” 目录中。/home/tls 为设置存放证书及密钥的基本路径。xx.yy 为域名，根据自己域名变化。  
3）、zerossl 申请的普通证书及密钥在 “/home/tls/certificates/acme.zerossl.com-v2-dv90/xx.yy” 目录中。/home/tls 为设置存放证书及密钥的基本路径。xx.yy 为域名，根据自己域名变化。  
4）、zerossl 申请的通配符证书及密钥在 “/home/tls/certificates/acme.zerossl.com-v2-dv90/wildcard_.xx.yy” 目录中。/home/tls 为设置存放证书及密钥的基本路径。xx.yy 为域名，根据自己域名变化。

5、caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可。Caddyfile 配置仅“二、回落为主应用”须启用额外无用端口来联动实现自动申请与更新证书及密钥。

6、提供了 dnspod（国际版dnspod.com插件）、cloudflare、dnspodcn（中国版dnspod.cn插件）三种最常见插件配置示例，其它插件配置类似，参考 dnspod 或 cloudflare 配置示例。

四、caddy webdav 应用配置

以 webdav 应用及文件服务打造不同的网盘应用。详见 webdav_caddy.json 或 webdav_Caddyfile 配置。
