# meruru

单文件 sing-box 交互管理脚本，面向 Debian / Ubuntu，兼容使用 systemd 的常见 RPM 发行版，支持 amd64 / arm64。

支持 VLESS / AnyTLS REALITY、Hysteria2 端口跳跃、TUIC、Shadowsocks-2022、Trojan TLS、VLESS WS + TLS，以及一键落地转发、SNI 优选和 Caddy 自有域名站点。

首次以 root 运行脚本，自动检查并安装必要依赖、独立 sing-box 内核和管理命令。之后输入 `M` 或 `m` 打开面板；可在运行管理中更改快捷命令。

## 安装

以 root 运行，任选一种：

```bash
curl -fL --retry 3 https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/meruru -o /root/meruru && bash /root/meruru
```

```bash
wget -O /root/meruru https://raw.githubusercontent.com/shinkumiyo/itsmeruru/main/meruru && bash /root/meruru
```

安装后输入 `meruru`、`M` 或 `m`。快捷命令可在运行管理中修改。

## 更新与旧版迁移

- 主菜单 **16 → 检查更新**：更新管理脚本、sing-box 或 Caddy；支持回退上一版管理脚本。
- 更新内核前检查节点配置兼容性；启动失败会尝试恢复旧程序。停止的服务不会因更新而自动启动。
- 已安装旧版的机器运行上述安装命令，会自动迁移到 `/etc/meruru`，服务名改为 `meruru.service`、`meruru-caddy.service`。
- 原 `/etc/sbx` 保留为备份，节点端口、密码、UUID 和已有 WS 路径不变。切换正在运行的服务时会有短暂中断；迁移失败会尝试恢复旧服务。
- 原 `sbx` 下载地址保留兼容安装入口，实际管理程序统一使用 `meruru`。

## 使用提示

- 云厂商安全组需要自行放行端口；Hysteria2 跳跃范围中的所有 UDP 端口都要放行。
- Trojan TLS 和 VLESS WS + TLS 需要自己的域名；Caddy 负责自动证书和 HTTPS 站点。
- SNI 筛查不保证所有国内网络可达，也不保证完全识别第三方 CDN。
- 节点配置保存在 `/etc/meruru`，包含密码和密钥，请勿公开上传；旧版迁移备份也含同样的敏感信息。
- sing-box 与 Caddy 从各自官方 GitHub Releases 下载；本项目不是二者的官方项目。
