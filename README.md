# meruru

轻量、直观的 sing-box 一键管理脚本。自动安装依赖和内核，使用数字菜单完成节点创建、修改、分享与运行管理。

## 功能

- 多协议：VLESS / AnyTLS REALITY、Hysteria2、TUIC v5、Shadowsocks-2022、Trojan TLS、VLESS WS + TLS。
- 端口跳跃：Hysteria2 自动配置跳跃范围及转发规则。
- 一键落地：粘贴节点链接，自动创建转发入口并输出新链接。
- SNI 优选：热门品牌、当地可信机构、自定义域名，支持延迟检测、历史评分与复测。
- REALITY 回落过滤：可对单个 VLESS 或 AnyTLS 节点启用 SNI 检查，并在验证失败时尝试恢复配置。
- 自有域名：Caddy 自动证书、HTTPS 反代与内置祝愿墙。
- 节点管理：编号选择、修改端口与凭据、分享链接、二维码及客户端配置。
- 运行维护：服务启停、开机自启、配置检查、日志、BBR 与端口跳跃诊断。
- 在线更新：分别更新管理脚本、sing-box 和 Caddy，支持管理脚本回退。

## 安装

适用 Debian / Ubuntu 及使用 systemd 的常见 RPM 发行版，支持 amd64 / arm64。以 root 运行，任选一种：

```bash
curl -fL --retry 3 https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/meruru -o /root/meruru && bash /root/meruru
```

```bash
wget -O /root/meruru https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/meruru && bash /root/meruru
```

安装后输入 `meruru`、`M` 或 `m`。快捷命令可在运行管理中修改。

## 管理与更新

- 主菜单 **16 → 检查更新**：更新管理脚本、sing-box 或 Caddy；支持回退上一版管理脚本。
- 更新内核前检查节点配置兼容性；启动失败会尝试恢复旧程序。停止的服务不会因更新而自动启动。
- 主菜单 **14 → 运行管理**：管理节点服务和 Caddy，查看状态与日志。
- 对缺少 ALPN 的旧 TUIC 节点，启动面板时会自动备份并补齐 `h3`；如果服务正在运行，会重启并验证防偷回落，失败时尝试恢复原配置。
- 配置目录：`/etc/meruru`；服务名：`meruru.service`、`meruru-caddy.service`。
- 配置损坏时进入恢复模式，可查看日志、隔离节点配置；`meruru-rollback` 可独立回退上一版管理脚本。
- 管理操作使用单会话锁，避免多个 SSH 面板同时修改节点；RTT 阈值会自动保存。
- 节点保存时同时核对配置和分享信息；删除前备份配置与自签证书，规则清理失败会保留数据供重试。

## 使用提示

- 云厂商安全组需要自行放行端口；Hysteria2 跳跃范围中的所有 UDP 端口都要放行。
- Trojan TLS 和 VLESS WS + TLS 需要自己的域名；Caddy 负责自动证书和 HTTPS 站点。
- 冷门和热门 SNI 使用中国大陆网站屏蔽名单辅助筛查；已知存在 SNI 屏蔽的 google.com 及其子域名排除。热门品牌域名命中网站名单时提示风险并继续检测，仍须通过网络归属等检查；名单命中不等于 SNI 必然被拦截。
- VPS 上的探测不能证明国内 SNI 可达。请从实际使用的国内网络测试建好的节点；浏览器能否打开网站也不能代替节点测试。列表中的旧候选不会在更新时自动删除。
- SNI 的 RTT 为三次 TCP 建连中位数；TLS 建连耗时为单次测量（包含 TCP），并显示服务端发送的证书数量。这些是独立指标，不代表国内端到端连接时延。
- CDN 筛查包括 CNAME、响应头和 Cloudflare 公布的 IPv4 网段；网段数据不可用时保留旧候选并暂停判定。HTTP 跳转只显示目标主机，不跟随跳转替换 SNI。
- 热门候选结合官方页面与网络归属资料更新：Apple、Google、Microsoft 使用网络范围，Tesla、Intel、NVIDIA、HP、Cisco 使用 RIPE 路由 ASN 证据。路由归属不能证明 CDN 专属或禁止域前置，其他品牌暂不作归属保证。
- 回落过滤默认关闭，仅拒绝不匹配或缺失的明文 SNI；无法检查加密后的 HTTP Host，也不能完全防止利用相同 SNI 的回落流量。已有节点不会自动开启。
- 节点配置包含密码和密钥，请妥善备份，不要公开上传。
- 防火墙自动清理仅针对新版记录的自有规则；旧版无归属记录的规则会保守保留，需自行检查整理。
- firewalld 分别更新运行和持久规则，避免全量重载影响端口跳跃；UFW 已有的等价规则不会被认领删除。
- sing-box 与 Caddy 从各自官方 GitHub Releases 下载；本项目不是二者的官方项目。
