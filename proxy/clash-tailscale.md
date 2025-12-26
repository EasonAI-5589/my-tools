# Clash Verge 与 Tailscale 兼容配置

解决 Clash Verge（TUN 模式）与 Tailscale 的网络冲突问题。

## 问题描述

Clash Verge 开启 TUN 模式后会接管所有网络流量，导致 Tailscale 无法正常工作。

## 解决方案

让 Tailscale 的流量（100.x.x.x）绕过 Clash 直连。

---

## 配置步骤

### 1. 添加规则直连

编辑 Clash Verge 的规则配置文件：

**路径**：`~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/`

找到你的 rules 配置文件（如 `rm8rjUmDn772.yaml`），添加：

```yaml
prepend:
  # Tailscale 直连规则
  - IP-CIDR,100.64.0.0/10,DIRECT,no-resolve
  - DOMAIN-SUFFIX,tailscale.com,DIRECT
  - DOMAIN-SUFFIX,tailscale.io,DIRECT
```

### 2. 添加系统代理绕过

编辑 `verge.yaml`，在 `system_proxy_bypass` 中添加 `100.*` 和 Tailscale 域名：

```yaml
system_proxy_bypass: localhost,127.*,10.*,100.*,172.16.*,...,*.tailscale.com,*.tailscale.io
```

### 3. 重启 Clash Verge

```bash
pkill -f "Clash Verge"
sleep 2
open -a "Clash Verge"
```

---

## 验证配置

```bash
# 测试 Tailscale 连通性
ping 100.96.157.50

# 应该看到正常响应，延迟 < 50ms
```

---

## 完整配置示例

### rules 配置文件

```yaml
# Profile Enhancement Rules Template for Clash Verge

prepend:
  # Tailscale 直连规则
  - IP-CIDR,100.64.0.0/10,DIRECT,no-resolve
  - DOMAIN-SUFFIX,tailscale.com,DIRECT
  - DOMAIN-SUFFIX,tailscale.io,DIRECT
  # 其他直连
  - DOMAIN-SUFFIX,happy.engineering,DIRECT

append: []

delete: []
```

### verge.yaml 关键配置

```yaml
enable_tun_mode: true
enable_system_proxy: true
system_proxy_bypass: localhost,127.*,10.*,100.*,172.16.*,172.17.*,172.18.*,172.19.*,172.20.*,172.21.*,172.22.*,172.23.*,172.24.*,172.25.*,172.26.*,172.27.*,172.28.*,172.29.*,172.30.*,172.31.*,192.168.*,<local>,*.tailscale.com,*.tailscale.io
```

---

## 原理说明

| 配置项 | 作用 |
|--------|------|
| `IP-CIDR,100.64.0.0/10,DIRECT` | Tailscale 使用的 CGNAT 地址段直连 |
| `DOMAIN-SUFFIX,tailscale.com` | Tailscale 控制平面域名直连 |
| `system_proxy_bypass: 100.*` | 系统代理层面绕过 Tailscale IP |
| `no-resolve` | 不解析 IP，避免 DNS 泄露 |

---

## 常见问题

### Q: 配置后还是不通

1. 确认 Clash Verge 已重启
2. 检查规则是否在 `prepend`（优先级最高）
3. 尝试关闭 TUN 模式测试

### Q: 其他代理软件

**Surge**：
```
[Rule]
IP-CIDR,100.64.0.0/10,DIRECT,no-resolve
```

**ClashX Pro**：
同样在配置文件中添加规则。
