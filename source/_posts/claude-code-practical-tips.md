---
title: Claude Code 实用技巧
date: 2026-08-03
tags: [claude-code, ai, 效率工具]
categories: 工具
---

## 子代理（Subagent）的使用

### 如何启动子代理？

子代理就像在 Claude Code 中开启独立线程，不阻塞当前主对话线程。三种启动方式：

1. **CLI 命令** — `claude --agent {子代理名称}`
   例：`claude --agent captain`（启动名为 captain 的子代理）
2. **@ 快捷引用** — `@"{子代理名称}"`
   例：`@"doc-updater" 结合项目实际情况帮我更新 xxx 下的所有文档`
3. **自然语言描述** — 直接在对话中说"启动一个子代理，帮我做 xxx"，Claude 会自动创建并分配任务

### 实战示例

```bash
# 示例 1：自然语言启动
启动一个子代理，并结合当前上下文信息和项目实际情况来更新 xxx 下的所有文档

# 示例 2：@ 引用 + 名称
@"code-reviewer"

# 示例 3：@ 引用 + 名称 + 具体任务
@"code-reviewer" review 当前 git 待提交代码
```

> **提示**：子代理适合用来并行处理独立任务，比如同时做代码审查、文档更新、测试生成等，大幅提升效率。

---

## 项目级 MCP 配置

在项目根目录创建 `.mcp.json` 文件，即可为项目配置专属的 MCP 工具。

### 推荐配置

```json
{
    "mcpServers": {
        "next-devtools": {
            "command": "npx",
            "args": [
                "-y",
                "next-devtools-mcp@latest"
            ]
        },
        "context7": {
            "command": "npx",
            "args": [
                "-y",
                "@upstash/context7-mcp@latest",
                "--api-key",
                "i18nManager"
            ],
            "description": "实时文档查询 — Next.js/React/Express 等库文档"
        },
        "sequential-thinking": {
            "command": "npx",
            "args": [
                "-y",
                "@modelcontextprotocol/server-sequential-thinking"
            ],
            "description": "链式推理 — 复杂问题逐步分析"
        },
        "filesystem": {
            "command": "npx",
            "args": [
                "-y",
                "@modelcontextprotocol/server-filesystem",
                "D:\\My Respository\\i18n-manager"
            ],
            "description": "文件系统操作（已配置为本项目路径）"
        },
        "playwright": {
            "command": "npx",
            "args": [
                "-y",
                "@playwright/mcp",
                "--browser",
                "chrome"
            ],
            "description": "浏览器自动化与 E2E 测试"
        }
    }
}
```

### 各 MCP 用途一览

| MCP Server | 用途 |
|---|---|
| `next-devtools` | Next.js 开发调试工具集成 |
| `context7` | 实时查询前端框架/库最新文档 |
| `sequential-thinking` | 复杂问题的逐步链式推理 |
| `filesystem` | 安全读写项目文件 |
| `playwright` | 浏览器自动化操作与 E2E 测试 |

---

## 常用工作流技巧

1. **并行处理** — 同时启动多个子代理处理独立任务，避免排队等待
2. **MCP 按需配置** — 只启用当前项目需要的 MCP，避免加载多余工具拖慢响应
3. **@ 引用** — 熟练使用 `@"agent-name"` 语法快速调度子代理
4. **结合 git 工作流** — 用子代理做 pre-commit review、自动生成 commit message

---

## 🎯 在 Agent 面板中切换与操作

在 Agent 面板（`claude agents`）的主界面中，你可以通过以下方式选择和切换不同的 Agent 会话：

*   **选择与查看**：使用键盘的 **`↑`（上）** 和 **`↓`（下）** 方向键来选择不同的 Agent 会话。
*   **快速回复（不进入会话）**：选中一个会话后，按下 **`Space`（空格键）**，可以直接在面板中回复该 Agent，无需进入完整对话界面。
*   **进入完整会话**：在选中的会话上按 **`Enter`** 或 **`→`（右方向键）**，即可进入该 Agent 的完整对话模式。
*   **返回主面板**：在 Agent 的子会话或完整对话中，按 **`←`（左方向键）** 可以返回 Agent 主面板。
*   **快速切换固定 Agent**：使用 **`Alt + 1` 到 `Alt + 5`** 可以快速切换到第 1 到第 5 个被固定（Pinned）的 Agent。

## ⌨️ 在普通会话中管理 Agent

如果你在一个普通的 Claude 会话中，想把它放到后台作为一个 Agent 管理，可以使用以下方式：

*   **放入后台**：在会话中输入 **`/bg`** 命令，当前会话就会被放到后台，并出现在 Agent 面板中。
