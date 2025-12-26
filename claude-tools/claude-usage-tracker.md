# Claude Usage Tracker

Mac 状态栏实时显示 Claude AI 使用量的原生应用。

## 基本信息

| 项目 | 信息 |
|------|------|
| **GitHub** | [hamed-elfayome/Claude-Usage-Tracker](https://github.com/hamed-elfayome/Claude-Usage-Tracker) |
| **Stars** | ⭐ 99+ |
| **协议** | MIT (免费开源) |
| **平台** | macOS 14+ (Sonoma) |
| **语言** | Swift/SwiftUI 原生 |
| **大小** | ~3MB |

## 功能

- ✅ Mac 状态栏实时显示 Token 用量
- ✅ 5 小时会话窗口追踪
- ✅ 周用量限制监控 (Max 账户)
- ✅ Opus 用量单独追踪
- ✅ Claude Code 终端状态栏集成
- ✅ 本地存储，无遥测，隐私优先
- ✅ 5 种图标样式 + 单色模式
- ✅ 阈值通知提醒

## 安装

```bash
# 下载
curl -L -o ~/Downloads/Claude-Usage.zip https://github.com/hamed-elfayome/Claude-Usage-Tracker/releases/latest/download/Claude-Usage.zip

# 解压并安装
unzip ~/Downloads/Claude-Usage.zip -d ~/Downloads/
mv ~/Downloads/"Claude Usage.app" /Applications/

# 移除安全限制（未签名应用）
xattr -cr "/Applications/Claude Usage.app"

# 启动
open "/Applications/Claude Usage.app"
```

## 配置

首次启动需要配置 Session Key：

1. 打开 https://claude.ai 并登录
2. 按 `F12` 或 `Cmd+Option+I` 打开开发者工具
3. 点击 **Application** 标签
4. 左侧 `Storage` → `Cookies` → `https://claude.ai`
5. 找到 `sessionKey` 行，双击复制 Value（以 `sk-ant-sid-` 开头）
6. 粘贴到 Claude Usage Tracker 输入框
7. 点击 **Validate**

## 状态栏显示

```
🔋 19%  ⏱️ 4:32
```

- 百分比：当前 5 小时窗口使用量
- 时间：距离重置剩余时间

## Claude Code 终端状态栏集成

支持在 Claude Code 终端显示实时用量，需配置 `~/.claude/settings.json`。

## 常见问题

### 应用打不开

```bash
xattr -cr "/Applications/Claude Usage.app"
```

### Session Key 过期

重新获取 sessionKey 并更新。

## 替代方案

| 工具 | 类型 | 说明 |
|------|------|------|
| [claude-code-usage-bar](https://github.com/leeguooooo/claude-code-usage-bar) | 终端状态栏 | Python 脚本，集成到 tmux/zsh |
| [SessionWatcher](https://www.sessionwatcher.com/) | Mac App | 收费闭源 |

## 更新

```bash
# 重新下载安装即可
curl -L -o ~/Downloads/Claude-Usage.zip https://github.com/hamed-elfayome/Claude-Usage-Tracker/releases/latest/download/Claude-Usage.zip
```
