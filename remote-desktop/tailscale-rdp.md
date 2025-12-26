# Tailscale + 远程桌面完整配置教程

通过 Tailscale 组网，从 Mac 远程控制 Windows 电脑。

## 优势

- **无需公网 IP**：Tailscale 自动打洞，穿透 NAT
- **安全加密**：WireGuard 协议，端到端加密
- **简单配置**：无需端口转发、DDNS
- **低延迟**：直连通信，延迟通常 < 10ms

---

## 第一部分：Tailscale 配置

### 1.1 安装 Tailscale

**Mac 端**：
```bash
brew install tailscale
# 或从 App Store 安装
```

**Windows 端**：
- 下载：https://tailscale.com/download/windows
- 安装后登录同一账号

### 1.2 验证连接

Mac 上查看设备列表：
```bash
/Applications/Tailscale.app/Contents/MacOS/Tailscale status
```

确认两台设备都在线，记住 Windows 的 Tailscale IP（格式：`100.x.x.x`）

### 1.3 测试连通性

```bash
ping 100.96.157.50  # 替换为你的 Windows IP
```

---

## 第二部分：Windows 远程桌面配置（被控端）

### ⚠️ 前提条件

- 需要 **Windows 11/10 专业版**（家庭版不支持）
- 检查版本：设置 → 系统 → 系统信息 → Windows 规格 → 版本

### 2.1 开启远程桌面

**快捷方式**：
1. 按 `Win + R`
2. 输入 `ms-settings:remotedesktop` 回车
3. 开启 **远程桌面** 开关 → 点击 **确认**

### 2.2 创建远程专用账户（推荐）

微软账户远程登录容易出问题，建议创建本地账户：

以管理员身份运行 CMD/PowerShell：
```cmd
net user remote 你的密码 /add
net localgroup "Remote Desktop Users" remote /add
net localgroup Administrators remote /add
```

这会创建：
- 用户名：`remote`
- 密码：`你的密码`
- 权限：管理员 + 远程桌面

### 2.3 检查防火墙

1. 按 `Win + R`，输入 `firewall.cpl` 回车
2. 点击 **"允许应用或功能通过 Windows Defender 防火墙"**
3. 找到 **"远程桌面"**
4. 确保 ✅ **专用** 和 **公用** 都勾选

### 2.4 网络配置（可选）

设置网络为专用网络可获得更好的连接性：
- 设置 → 网络和 Internet → 以太网/WiFi → 网络配置文件类型 → **专用网络**

---

## 第三部分：Mac 客户端配置（控制端）

### 3.1 安装 Windows App

从 App Store 下载 **Windows App**（微软官方远程桌面客户端）

### 3.2 添加电脑

1. 打开 Windows App
2. 点击 **+** → **添加电脑**
3. 填写：
   - **电脑名称**：`100.96.157.50`（Tailscale IP）
   - **用户账户**：添加用户账户
     - 用户名：`remote`（或 `.\remote`）
     - 密码：你设置的密码
4. 点击 **添加**

### 3.3 连接

双击保存的电脑，等待连接完成。

---

## 第四部分：代理共存配置

如果你使用 Clash/Surge 等代理软件，需要让 Tailscale 流量直连。

### 4.1 Clash Verge 配置

编辑规则配置文件，添加：
```yaml
prepend:
  # Tailscale 直连
  - IP-CIDR,100.64.0.0/10,DIRECT,no-resolve
  - DOMAIN-SUFFIX,tailscale.com,DIRECT
  - DOMAIN-SUFFIX,tailscale.io,DIRECT
```

编辑 `verge.yaml`，在 `system_proxy_bypass` 中添加 `100.*`：
```yaml
system_proxy_bypass: localhost,127.*,10.*,100.*,...
```

### 4.2 重启代理软件

配置修改后重启 Clash Verge 生效。

---

## 常见问题

### Q: 凭据无效/密码错误

**解决方案**：
1. 使用本地账户而非微软账户
2. 用户名格式试试 `.\remote` 或 `电脑名\remote`
3. 确认密码正确（区分大小写）

### Q: 无法连接到远程电脑

**检查清单**：
- [ ] Windows 远程桌面已开启
- [ ] Tailscale 两端都在线
- [ ] 防火墙允许远程桌面
- [ ] 代理软件已配置 Tailscale 直连

### Q: 连接很慢/卡顿

**优化方案**：
1. Windows App 设置中降低分辨率
2. 关闭视觉效果（动画等）
3. 检查网络带宽

### Q: 家庭版 Windows 不支持

**替代方案**：
- 升级到专业版
- 使用第三方软件：ToDesk、向日葵、RustDesk

---

## 快速命令参考

```bash
# 查看 Tailscale 状态
/Applications/Tailscale.app/Contents/MacOS/Tailscale status

# 测试连接
ping 100.96.157.50

# Windows 创建远程账户
net user remote 密码 /add
net localgroup "Remote Desktop Users" remote /add
net localgroup Administrators remote /add

# Windows 开启远程桌面
ms-settings:remotedesktop
```

---

## 我的设备信息

| 设备 | Tailscale IP | 系统 |
|------|--------------|------|
| macbook-air | 100.126.23.2 | macOS 26.0.1 |
| desktop | 100.96.157.50 | Windows 11 24H2 |
| iphone-15-pro | 100.113.208.97 | iOS 26.1.0 |
