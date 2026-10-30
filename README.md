# itsmeruru

单文件 sing-box 交互管理脚本，面向 Debian / Ubuntu，兼容使用 systemd 的常见 RPM 发行版，支持 amd64 / arm64。

支持 VLESS / AnyTLS REALITY、Hysteria2 端口跳跃、TUIC、Shadowsocks-2022、Trojan TLS、VLESS WS + TLS，以及一键落地转发、SNI 优选和 Caddy 自有域名站点。

首次以 root 运行脚本，自动检查并安装必要依赖、独立 sing-box 内核和管理命令。之后输入 `M` 或 `m` 打开面板；可在运行管理中更改快捷命令。

## 安装

以 root 运行，任选一种：

```bash
curl -fL --retry 3 https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/sbx -o /root/sbx && bash /root/sbx
```

```bash
wget -O /root/sbx https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/sbx && bash /root/sbx
```

安装后输入 `M` 或 `m`。更新脚本时重新运行同一条安装命令即可，现有节点保留。

## 使用提示

- 云厂商安全组需要自行放行端口；Hysteria2 跳跃范围中的所有 UDP 端口都要放行。
- Trojan TLS 和 VLESS WS + TLS 需要自己的域名；Caddy 负责自动证书和 HTTPS 站点。
- SNI 筛查不保证所有国内网络可达，也不保证完全识别第三方 CDN。
- 节点配置保存在 `/etc/sbx`，包含密码和密钥，请勿公开上传。
- sing-box 与 Caddy 从各自官方 GitHub Releases 下载；本项目不是二者的官方项目。
