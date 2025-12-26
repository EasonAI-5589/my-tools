# My Tools

个人常用工具、配置和教程整理。

## 目录结构

```
my-tools/
├── remote-desktop/                    # 远程桌面相关
│   ├── tailscale-rdp.md              # Tailscale + 远程桌面配置教程
│   └── windows-user-migration.md     # Windows 用户文件迁移方案
├── proxy/                             # 代理相关
│   ├── clash-tailscale.md            # Clash 与 Tailscale 兼容配置
│   └── clash-openvpn.md              # Clash 与 OpenVPN 兼容配置
└── README.md
```

## 快速导航

| 工具 | 说明 | 文档 |
|------|------|------|
| Tailscale + RDP | Mac 远程控制 Windows | [教程](remote-desktop/tailscale-rdp.md) |
| 用户文件迁移 | Windows 多账户文件同步 | [方案](remote-desktop/windows-user-migration.md) |
| Clash + Tailscale | 代理与 Tailscale 共存 | [配置](proxy/clash-tailscale.md) |
| Clash + OpenVPN | 代理与公司 VPN 共存 | [配置](proxy/clash-openvpn.md) |

## 环境

- macOS Tahoe (26.0)
- Windows 11 24H2
- Tailscale 1.92.3
- Clash Verge
