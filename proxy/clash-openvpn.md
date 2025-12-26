# Clash Verge 与 OpenVPN 兼容配置

解决 Clash Verge 与公司/学校 OpenVPN 的网络冲突问题。

## 环境信息

| 项目 | 值 |
|------|-----|
| VPN 类型 | OpenVPN Connect |
| 服务器 | ssl-vpn.x-humanoid-cloud.com |
| 服务器 IP | 180.76.235.174 |
| 端口 | 1194 (UDP) |
| 私有 IP 段 | 10.51.0.0/16 |

---

## 问题描述

Clash Verge 开启 TUN 模式后会接管所有流量，导致 OpenVPN 隧道流量也被代理，造成：
- VPN 连接不稳定
- 内网资源无法访问
- 连接速度变慢

---

## 解决方案

让 OpenVPN 相关流量绕过 Clash 直连。

### 1. 添加规则直连

编辑 Clash Verge 的规则配置文件，添加：

```yaml
prepend:
  # OpenVPN (公司VPN) 直连规则
  - IP-CIDR,10.51.0.0/16,DIRECT,no-resolve      # VPN 内网 IP 段
  - IP-CIDR,180.76.235.174/32,DIRECT,no-resolve # VPN 服务器 IP
  - DOMAIN-SUFFIX,x-humanoid-cloud.com,DIRECT   # VPN 相关域名
```

### 2. 系统代理绕过

确保 `verge.yaml` 的 `system_proxy_bypass` 包含：

```yaml
system_proxy_bypass: ...,10.*,*.x-humanoid-cloud.com,180.76.235.174,...
```

### 3. 重启 Clash Verge

```bash
pkill -f "Clash Verge" && sleep 2 && open -a "Clash Verge"
```

---

## 完整规则配置

```yaml
# Profile Enhancement Rules Template for Clash Verge

prepend:
  # Tailscale 直连规则
  - IP-CIDR,100.64.0.0/10,DIRECT,no-resolve
  - DOMAIN-SUFFIX,tailscale.com,DIRECT
  - DOMAIN-SUFFIX,tailscale.io,DIRECT
  # OpenVPN (公司VPN) 直连规则
  - IP-CIDR,10.51.0.0/16,DIRECT,no-resolve
  - IP-CIDR,180.76.235.174/32,DIRECT,no-resolve
  - DOMAIN-SUFFIX,x-humanoid-cloud.com,DIRECT
  # 其他直连
  - DOMAIN-SUFFIX,happy.engineering,DIRECT
  - DOMAIN-SUFFIX,cluster-fluster.com,DIRECT

append: []

delete: []
```

---

## 验证配置

### 1. 检查 VPN 连接

连接 OpenVPN 后，检查是否分配到内网 IP：
```bash
ifconfig | grep "10.51"
```

### 2. 测试内网访问

```bash
# ping VPN 网关或内网服务器
ping 10.51.0.1
```

### 3. 确认流量走向

访问内网资源时，应该走 VPN 隧道，不经过 Clash 代理。

---

## 多 VPN 共存架构

```
┌─────────────────────────────────────────────────────┐
│                    网络流量                          │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────┐   规则匹配   ┌─────────────────┐   │
│  │ Clash Verge │ ──────────→ │ 10.51.0.0/16    │   │
│  │ (TUN 模式)  │             │ → OpenVPN 直连   │   │
│  └─────────────┘             ├─────────────────┤   │
│         │                    │ 100.64.0.0/10   │   │
│         │                    │ → Tailscale 直连 │   │
│         │                    ├─────────────────┤   │
│         │                    │ 其他流量         │   │
│         └──────────────────→ │ → 代理节点       │   │
│                              └─────────────────┘   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 常见问题

### Q: VPN 连接后无法访问内网

检查：
1. Clash 规则是否生效（重启 Clash Verge）
2. VPN 是否正常连接（检查 OpenVPN Connect 状态）
3. 内网 IP 段是否正确（可能不是 10.51.0.0/16）

### Q: 如何查看实际内网 IP 段

连接 VPN 后运行：
```bash
# macOS
netstat -rn | grep utun

# 或查看路由表
route -n get 10.51.0.1
```

### Q: 其他公司 VPN 如何配置

替换以下信息即可：
- VPN 服务器 IP/域名
- 内网 IP 段（如 10.x.x.x、172.x.x.x、192.168.x.x）

---

## 相关文档

- [Clash 与 Tailscale 兼容配置](clash-tailscale.md)
- [Tailscale + 远程桌面教程](../remote-desktop/tailscale-rdp.md)
