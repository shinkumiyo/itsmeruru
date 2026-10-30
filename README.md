# meruru

轻量、直观的 sing-box 一键管理脚本。自动安装依赖和内核，使用数字菜单完成节点创建、修改、分享与运行管理。

## 功能

- 多协议：VLESS / AnyTLS REALITY、Hysteria2、TUIC v5、Shadowsocks-2022、Trojan TLS、VLESS WS + TLS。
- 端口跳跃：Hysteria2 自动配置跳跃范围及转发规则。
- 一键落地：粘贴节点链接，自动创建转发入口并输出新链接。
- SNI 优选：热门品牌、当地可信机构、自定义域名，支持延迟检测、历史评分与复测；高级选项可探测已获授权的 IPv4 网段，最多保存 3 个合格候选。
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
- 配置目录：`/etc/meruru`；服务名：`meruru.service`、`meruru-caddy.service`。
- 配置损坏时进入恢复模式，可查看日志、隔离节点配置；`meruru-rollback` 可独立回退上一版管理脚本。
- 管理操作使用单会话锁，避免多个 SSH 面板同时修改节点；RTT 阈值会自动保存。
- 节点保存时同时核对配置和分享信息；删除前备份配置与自签证书，规则清理失败会保留数据供重试。

## 使用提示

- 云厂商安全组需要自行放行端口；Hysteria2 跳跃范围中的所有 UDP 端口都要放行。
- Trojan TLS 和 VLESS WS + TLS 需要自己的域名；Caddy 负责自动证书和 HTTPS 站点。
- SNI 筛查不保证所有国内网络可达，也不保证完全识别第三方 CDN。
- 网段探测自动推算本机 IPv4 的 /24、/23 或 /22，仅限已获授权的网段；使用默认 TLS 证书和 PTR 提取域名，站点用途及国内可访问性仍需自行核实。
- 节点配置包含密码和密钥，请妥善备份，不要公开上传。
- 防火墙自动清理仅针对新版记录的自有规则；旧版无归属记录的规则会保守保留，需自行检查整理。
- firewalld 分别更新运行和持久规则，避免全量重载影响端口跳跃；UFW 已有的等价规则不会被认领删除。
- sing-box 与 Caddy 从各自官方 GitHub Releases 下载；本项目不是二者的官方项目。
