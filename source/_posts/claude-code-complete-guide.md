---
title: Claude Code 命令速查指南
date: 2026-07-15 01:30:00
tags:
  - Claude
  - AI编程
  - 开发工具
categories:
  - AI工具
---

Claude Code 内置 50+ 命令，本文一页速查，覆盖所有斜杠命令、CLI 标志和快捷键。

<!-- more -->

## 三种命令形态

**CLI 命令**（终端启动）：`claude`、`claude -c`、`claude --print "问题"`

**斜杠命令**（会话内 `/`）：`/init`、`/compact`、`/model`

**键盘快捷键**（直接生效）：`Ctrl+C` 取消、`Ctrl+R` 搜索历史、`Shift+Tab` 切换模式

---

## 核心命令（10 个）

| 命令 | 功能 | 用法 |
|------|------|------|
| `/init` | 创建 CLAUDE.md 项目记忆 | `/init` |
| `/compact` | 压缩上下文回收空间 | `/compact` 或 `/compact retain 关键内容` |
| `/clear` | 硬重置，清除对话历史 | `/clear` |
| `/model` | 切换模型 | `/model opus\|sonnet\|haiku` |
| `/cost` | 查看 Token 消耗和费用 | `/cost` |
| `/context` | 查看上下文用量百分比 | `/context` |
| `/diff` | 查看当前会话代码更改 | `/diff` 或 `/diff src/file.ts` |
| `/help` | 列出所有可用命令 | `/help` |
| `/memory` | 编辑 CLAUDE.md | `/memory`，`# 规则` 快速追加 |
| `/resume` | 恢复历史会话 | `/resume` 或 `claude --resume` |

**模型选择：** Sonnet 日常编码 → Opus 复杂任务 → Haiku 简单操作

**黄金规则：** 上下文 70-80% 就主动 `/compact`，别等到 95%。

---

## 进阶命令（9 个）

| 命令 | 功能 | 用法 |
|------|------|------|
| `/btw` | 不打断上下文提问 | `/btw 问题内容` |
| `/fast` | 极速模式（速度优先） | `/fast` 切换 |
| `/plan` | 计划模式（审批后执行） | `Shift+Tab` 或 `/plan` |
| `/fork` | 临时分支探索想法 | `/fork` |
| `/rewind` | 撤销对话/代码 | `Esc Esc` 打开菜单 |
| `/todos` | 持久化任务列表 | `Ctrl+T` 切换显示 |
| `/simplify` | 代码审查 | `/simplify` |
| `/output-style` | 调整响应风格 | `/output-style` |
| `/permissions` | 管理自动审批 | `/permissions` |

**子 Agent 调用方式：**

| 方式 | 示例 |
|------|------|
| 直接委派 | `请用 doc-maintainer 检查文档一致性` |
| 指定 Agent 名称 | `请用 code-reviewer 审查最近的改动` |
| `/` 命令前缀 | 直接输入 `/agent-name` 调用 |

**三种模式：** Normal（每次确认）→ Auto-Accept（自动执行）→ Plan（只读方案）

**关键点**：只要在对话中明确说"用 XX agent 做 YY"，我就会在**后台启动它，主线对话不受影响**。完成后你会收到 <task-notification> 通知，我会把结果摘要告诉你。

---

## CLI 启动选项

| 选项 | 功能 |
|------|------|
| `claude --print "问题"` | 一次性查询，不进入交互 |
| `claude -c` | 恢复当前目录最近会话 |
| `claude --from-pr 123` | 从 PR 恢复会话 |
| `--append-system-prompt "..."` | 追加自定义系统指令（推荐） |
| `--system-prompt "..."` | 替换全部系统指令（危险） |
| `--dangerously-skip-permissions` | 跳过所有权限确认（仅限容器） |
| `--output-format json` | JSON 格式输出 |
| `--agents '{...}'` | 启动时预定义子 Agent |

---

## 键盘快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+C` | 取消当前生成 |
| `Ctrl+R` | 搜索命令历史 |
| `Shift+Tab` | 切换模式 |
| `Esc Esc` | 回退菜单 |
| `Ctrl+T` | 任务列表 |
| `Alt+M` | 切换模式（同 Shift+Tab） |
| `Alt+P` | 上一条消息 |
| `Alt+N` | 下一条消息 |
| `Alt+B` | 后退对话 |
| `Alt+F` | 前进对话 |
| `Shift+Enter` | 多行输入 |
| `@ + 路径` | 文件自动补全 |
| `! + 命令` | 直接执行 bash |
| `# + 文字` | 添加到记忆中 |

---

## 隐藏功能

| 命令 | 功能 |
|------|------|
| `/vim` | 启用 Vim 键位绑定 |
| `/remote-control` | 通过 claude.ai 远程控制本地会话 |
| `/export` | 导出会话为可搜索文档 |
| `/usage-report` | 生成月度使用报告 |

---

## 典型工作流

**功能开发：** `/init` → `/memory` → 实现 → `/diff` → `! npm test` → `/cost` → 提交

**调试修复：** `claude -c` → 贴错误 → `/btw` 穿插提问 → 修复 → `/diff` → 测试 → `/compact`

**大型重构：** `Shift+Tab` 切 Plan 模式 → 描述需求 → 审批 → `/context` 监控 → 70% 时 `/compact` → `/diff` + `/simplify` → `/export`

---
