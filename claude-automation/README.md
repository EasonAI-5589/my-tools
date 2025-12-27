# Claude 自动化工具

让 Claude Code 一整晚自动工作的解决方案。

## 工具栈

| 工具 | 版本 | 用途 |
|------|------|------|
| [claude-flow](https://github.com/ruvnet/claude-flow) | v2.7.47 | 多 Agent 编排、后台任务 |
| [happy-coder](https://github.com/slopus/happy) | v0.13.0 | 手机端监控、推送通知 |

## 安装

```bash
# claude-flow（多 Agent 编排）
npm install -g claude-flow
claude-flow hive-mind init

# happy-coder（手机监控）
npm install -g happy-coder
```

## 核心命令

### claude-flow 常用命令

```bash
# 后台运行任务
claude-flow swarm "你的任务描述" --background

# 查看任务状态
claude-flow hive-mind status

# 断点续跑
claude-flow hive-mind resume <session-id>

# 查看所有会话
claude-flow hive-mind sessions

# 停止任务
claude-flow hive-mind stop <swarm-id>
```

### 执行策略

```bash
# 研究型任务（只读分析）
claude-flow swarm "分析代码架构" --strategy research --analysis

# 开发型任务
claude-flow swarm "实现新功能" --strategy development

# 并行加速（2.8-4.4x）
claude-flow swarm "重构项目" --parallel --max-agents 5

# 实时监控
claude-flow swarm "任务" --monitor
```

## 使用场景

### 场景 1：批量论文阅读

```bash
# 创建任务
claude-flow swarm "
从 Zotero 获取以下论文并生成阅读笔记：
1. OpenVLA
2. RT-2
3. PaLM-E
每篇论文按 Section 1-4 格式输出到 ~/notes/
" --background --strategy research
```

### 场景 2：代码重构

```bash
# 重构整个模块
claude-flow swarm "
重构 src/utils/ 目录：
1. 提取公共函数
2. 添加类型注解
3. 编写单元测试
4. 更新文档
" --background --strategy development --parallel
```

### 场景 3：代码审查

```bash
# 只读分析模式
claude-flow swarm "
审查最近 10 个 commit：
1. 检查安全漏洞
2. 检查性能问题
3. 生成审查报告
" --analysis --strategy research
```

### 场景 4：文档生成

```bash
claude-flow swarm "
为项目生成完整文档：
1. API 文档
2. 架构说明
3. 部署指南
" --background
```

## 与 Happy Coder 配合使用

**推荐工作流**：

```
┌─────────────────────────────────────┐
│  电脑端：claude-flow 后台跑任务      │
│  claude-flow swarm "xxx" --background│
└─────────────────────────────────────┘
                 ↓
┌─────────────────────────────────────┐
│  手机端：Happy 监控进度              │
│  - 实时查看输出                      │
│  - 接收推送通知                      │
│  - 处理权限请求                      │
└─────────────────────────────────────┘
```

### 启动 Happy 监控

```bash
# 电脑端启动 happy（替代 claude）
happy

# 手机端下载 Happy App
# iOS: App Store 搜索 "Happy Claude Code"
# Android: Google Play 搜索 "Happy Coder"
```

### 兼容性说明

| 功能 | claude-flow | happy |
|------|-------------|-------|
| 后台任务 | ✅ | ❌ |
| 多 Agent | ✅ | ❌ |
| 手机监控 | ❌ | ✅ |
| 推送通知 | ❌ | ✅ |
| MCP 工具 | ✅ | ✅ |

**注意**：claude-flow 硬编码调用 `claude` CLI，如需完全用 happy 替代，可创建别名：

```bash
# ~/.zshrc
alias claude='happy'
```

## 配置文件位置

```
~/.hive-mind/
├── config.json      # 主配置
├── hive.db          # 任务数据库（SQLite）
├── memory.db        # Agent 记忆
└── sessions/        # 会话存档
```

## 夜间任务脚本

创建定时任务脚本 `overnight.sh`：

```bash
#!/bin/bash
# overnight.sh - 夜间自动化任务

LOG_FILE="$HOME/logs/overnight-$(date +%Y%m%d).log"
mkdir -p ~/logs

echo "=== 开始夜间任务 $(date) ===" >> $LOG_FILE

# 任务 1：论文阅读
claude-flow swarm "阅读 Zotero 最新 5 篇论文" \
    --background \
    --strategy research \
    >> $LOG_FILE 2>&1

# 等待完成
sleep 300

# 任务 2：代码审查
claude-flow swarm "审查本周所有 commit" \
    --background \
    --analysis \
    >> $LOG_FILE 2>&1

echo "=== 任务结束 $(date) ===" >> $LOG_FILE
```

使用 cron 定时执行：

```bash
# 每晚 11 点运行
crontab -e
0 23 * * * /Users/guoyichen/my-tools/claude-automation/overnight.sh
```

## 故障排查

### 任务卡住

```bash
# 查看状态
claude-flow hive-mind status

# 强制停止
claude-flow hive-mind stop --all
```

### 查看日志

```bash
# 查看最近会话
claude-flow hive-mind sessions

# 查看具体会话日志
cat ~/.hive-mind/sessions/<session-id>/log.json
```

## 参考链接

- [claude-flow GitHub](https://github.com/ruvnet/claude-flow) ⭐ 10.9k
- [happy-coder GitHub](https://github.com/slopus/happy)
- [Anthropic 长时间运行 Agent 指南](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
