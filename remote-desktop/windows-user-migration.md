# Windows 用户文件迁移方案

将 Windows 微软账户（moruoguo）的文件迁移到本地账户（remote），实现远程桌面和本地使用相同的文件环境。

## 背景

| 账户 | 类型 | 用途 |
|------|------|------|
| `moruoguo5589@outlook.com` | 微软账户 | 本地日常使用 |
| `remote` | 本地账户 | 远程桌面连接 |

**问题**：两个账户文件不互通，远程登录看不到原来的桌面和文档。

**解决**：将原账户文件复制到 remote 账户。

---

## 迁移步骤

### 步骤 1：确认用户文件夹名

打开 CMD，运行：
```cmd
dir C:\Users
```

记下原账户的文件夹名（如 `moruoguo` 或 `moruo`）。

### 步骤 2：以管理员身份运行 PowerShell

按 `Win + X` → 选择 **终端(管理员)** 或 **PowerShell(管理员)**

### 步骤 3：执行迁移脚本

复制以下脚本，**修改 `$OLD` 为你的实际用户名**，然后粘贴执行：

```powershell
# ============================================
# Windows 用户文件迁移脚本
# ============================================

# 配置：修改这里的用户名
$OLD = "moruoguo"    # 原用户文件夹名（改成你的）
$NEW = "remote"      # 新用户文件夹名

$Source = "C:\Users\$OLD"
$Dest = "C:\Users\$NEW"

# 检查源文件夹
if (!(Test-Path $Source)) {
    Write-Host "错误：找不到 $Source" -ForegroundColor Red
    Write-Host "请检查用户名是否正确，运行 dir C:\Users 查看" -ForegroundColor Yellow
    exit
}

Write-Host "开始迁移：$Source -> $Dest" -ForegroundColor Green
Write-Host ""

# 要迁移的文件夹列表
$Folders = @(
    "Desktop",      # 桌面
    "Documents",    # 文档
    "Downloads",    # 下载
    "Pictures",     # 图片
    "Videos",       # 视频
    "Music"         # 音乐
)

foreach ($Folder in $Folders) {
    $Src = "$Source\$Folder"
    $Dst = "$Dest\$Folder"

    if (Test-Path $Src) {
        Write-Host "复制 $Folder ..." -ForegroundColor Cyan
        robocopy $Src $Dst /E /Z /R:1 /W:1 /MT:8 | Out-Null
        Write-Host "  ✓ $Folder 完成" -ForegroundColor Green
    } else {
        Write-Host "  - $Folder 不存在，跳过" -ForegroundColor Gray
    }
}

Write-Host ""
Write-Host "============================================" -ForegroundColor Green
Write-Host "迁移完成！" -ForegroundColor Green
Write-Host "============================================" -ForegroundColor Green
```

### 步骤 4（可选）：迁移应用配置

如果需要迁移应用设置（浏览器书签、软件配置等）：

```powershell
$OLD = "moruoguo"
$NEW = "remote"

# 迁移 AppData\Roaming（应用配置）
Write-Host "复制应用配置（可能需要几分钟）..." -ForegroundColor Cyan
robocopy "C:\Users\$OLD\AppData\Roaming" "C:\Users\$NEW\AppData\Roaming" /E /Z /R:1 /W:1 /MT:8 /XD "Microsoft" | Out-Null
Write-Host "✓ 应用配置迁移完成" -ForegroundColor Green
```

> ⚠️ 排除了 Microsoft 文件夹，避免覆盖系统设置

---

## 迁移内容说明

| 文件夹 | 内容 | 大小估计 |
|--------|------|----------|
| Desktop | 桌面文件和快捷方式 | 小 |
| Documents | 文档 | 中 |
| Downloads | 下载的文件 | 大 |
| Pictures | 图片 | 中 |
| Videos | 视频 | 大 |
| AppData/Roaming | 应用配置、浏览器数据 | 大 |

---

## 迁移后验证

1. 用 `remote` 账户登录（远程或本地）
2. 检查桌面是否有原来的文件
3. 检查文档文件夹

---

## 常见问题

### Q: 提示权限不足

确保以**管理员身份**运行 PowerShell。

### Q: 部分文件复制失败

正在使用的文件无法复制，可以：
1. 关闭相关程序后重试
2. 登出原账户后再迁移

### Q: 迁移后想保持同步

两个账户的文件是独立的，迁移是一次性复制。如果需要长期同步，建议：
- 使用 OneDrive 同步
- 或修复微软账户远程登录（见 [tailscale-rdp.md](tailscale-rdp.md)）

---

## 快速命令参考

```powershell
# 查看用户文件夹
dir C:\Users

# 单独复制桌面
robocopy "C:\Users\moruoguo\Desktop" "C:\Users\remote\Desktop" /E

# 单独复制文档
robocopy "C:\Users\moruoguo\Documents" "C:\Users\remote\Documents" /E

# 查看复制进度
robocopy "源" "目标" /E /Z /R:1 /W:1 /MT:8
```
