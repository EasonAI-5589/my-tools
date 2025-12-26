# My Tools

个人常用工具、配置和教程整理。

## 目录结构

```
my-tools/
├── remote-desktop/                    # 远程桌面相关
│   ├── tailscale-rdp.md              # Tailscale + 远程桌面配置教程
│   └── windows-user-migration.md     # Windows 用户文件迁移方案
├── proxy/                             # 代理/VPN 相关
│   ├── network-overview.md           # 🌐 网络配置总览（国内/新加坡双方案）
│   ├── clash-tailscale.md            # Clash 与 Tailscale 兼容配置
│   ├── clash-openvpn.md              # Clash 与 OpenVPN 兼容配置
│   └── clash-config/                 # 📦 Clash 配置备份
│       ├── rules.yaml                # 直连规则
│       └── verge.yaml                # Verge 主配置
└── README.md
```

## 快速导航

| 工具 | 说明 | 文档 |
|------|------|------|
| **网络总览** | 国内/新加坡双方案 | [📋 总览](proxy/network-overview.md) |
| Tailscale + RDP | Mac 远程控制 Windows | [教程](remote-desktop/tailscale-rdp.md) |
| 用户文件迁移 | Windows 多账户文件同步 | [方案](remote-desktop/windows-user-migration.md) |
| Clash + Tailscale | 代理与 Tailscale 共存 | [配置](proxy/clash-tailscale.md) |
| Clash + OpenVPN | 代理与公司 VPN 共存 | [配置](proxy/clash-openvpn.md) |

## 网络服务一览

| 服务 | 类型 | 用途 |
|------|------|------|
| WgetCloud | 代理 | 主力翻墙 |
| 呆鹅云 | 代理 | 备用翻墙 |
| Tailscale | VPN | 远程连接家里主机 |
| OpenVPN | VPN | 亦庄显卡集群 |
| 奇安信零信任 | VPN | 上庄显卡集群 |
| 飞连 | VPN | 字节显卡集群 |

## 环境

- macOS Tahoe (26.0)
- Windows 11 24H2
- Tailscale 1.92.3
- Clash Verge
