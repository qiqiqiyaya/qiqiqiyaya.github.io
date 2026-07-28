---
title: Everything Claude Code (ECC) — 代理指令完全指南
date: 2026-07-28 10:00:00
tags:
  - Claude
  - AI编程
  - 开发工具
  - 代理
categories:
  - AI工具
---

# Everything Claude Code (ECC) — 代理指令

这是一款**生产就绪的 AI 编程插件**，提供 64 个专业化代理、261 项技能、84 条命令以及用于软件开发的自动化钩子工作流。

**版本：** 2.0.0

<!-- more -->

## 核心原则

1. **代理优先** — 将领域任务委托给专业代理
2. **测试驱动** — 在实现之前编写测试，要求 80% 以上覆盖率
3. **安全第一** — 绝不妥协安全性；验证所有输入
4. **不可变性** — 始终创建新对象，绝不修改现有对象
5. **先计划后执行** — 在编写代码之前为复杂功能制定计划

## 可用代理

| 代理 | 用途 | 何时使用 |
|------|------|----------|
| planner | 实现规划 | 复杂功能、重构 |
| architect | 系统设计和可扩展性 | 架构决策 |
| tdd-guide | 测试驱动开发 | 新功能、错误修复 |
| code-reviewer | 代码质量和可维护性 | 编写/修改代码后 |
| security-reviewer | 漏洞检测 | 提交前、敏感代码 |
| build-error-resolver | 修复构建/类型错误 | 构建失败时 |
| e2e-runner | 端到端 Playwright 测试 | 关键用户流程 |
| refactor-cleaner | 死代码清理 | 代码维护 |
| doc-updater | 文档和代码映射更新 | 更新文档 |
| cpp-reviewer | C/C++ 代码审查 | C 和 C++ 项目 |
| cpp-build-resolver | C/C++ 构建错误 | C 和 C++ 构建失败 |
| fsharp-reviewer | F# 函数式代码审查 | F# 项目 |
| docs-lookup | 通过 Context7 查阅文档 | API/文档问题 |
| go-reviewer | Go 代码审查 | Go 项目 |
| go-build-resolver | Go 构建错误 | Go 构建失败 |
| kotlin-reviewer | Kotlin 代码审查 | Kotlin/Android/KMP 项目 |
| kotlin-build-resolver | Kotlin/Gradle 构建错误 | Kotlin 构建失败 |
| database-reviewer | PostgreSQL/Supabase 专家 | 模式设计、查询优化 |
| python-reviewer | Python 代码审查 | Python 项目 |
| django-reviewer | Django 代码审查 | Django 应用、DRF API、ORM、迁移 |
| django-build-resolver | Django 构建、迁移和设置错误 | Django 启动、依赖、迁移、collectstatic 失败 |
| java-reviewer | Java 和 Spring Boot 代码审查 | Java/Spring Boot 项目 |
| java-build-resolver | Java/Maven/Gradle 构建错误 | Java 构建失败 |
| loop-operator | 自主循环执行 | 安全运行循环、监控停滞、干预 |
| harness-optimizer | Harness 配置调优 | 可靠性、成本、吞吐量 |
| rust-reviewer | Rust 代码审查 | Rust 项目 |
| rust-build-resolver | Rust 构建错误 | Rust 构建失败 |
| pytorch-build-resolver | PyTorch 运行时/CUDA/训练错误 | PyTorch 构建/训练失败 |
| mle-reviewer | 生产级 ML 流水线审查 | ML 流水线、评估、服务、监控、回滚 |
| typescript-reviewer | TypeScript/JavaScript 代码审查 | TypeScript/JavaScript 项目 |

## 代理编排

主动使用代理，无需用户提示：
- 复杂功能请求 → **planner**
- 刚编写/修改的代码 → **code-reviewer**
- 错误修复或新功能 → **tdd-guide**
- 架构决策 → **architect**
- 安全敏感代码 → **security-reviewer**
- 自主循环/循环监控 → **loop-operator**
- Harness 配置可靠性和成本 → **harness-optimizer**

对于独立操作，使用并行执行 —— 同时启动多个代理。

## 安全指南

**在任何提交之前：**
- 没有硬编码的秘密（API 密钥、密码、令牌）
- 所有用户输入均已验证
- 防止 SQL 注入（参数化查询）
- 防止 XSS（已转义 HTML）
- 启用 CSRF 保护
- 验证身份验证/授权
- 所有端点启用速率限制
- 错误消息不泄露敏感数据

**秘密管理：** 永远不要硬编码秘密。使用环境变量或秘密管理器。在启动时验证所需的秘密。立即轮换任何暴露的秘密。

**如果发现安全问题：** 停止 → 使用 security-reviewer 代理 → 修复关键问题 → 轮换暴露的秘密 → 审查代码库是否存在类似问题。

## 编码风格

**不可变性（关键）：** 始终创建新对象，绝不修改。返回应用了更改的新副本。

**文件组织：** 多用小文件，少用大文件。典型 200-400 行，最多 800 行。按功能/领域组织，不按类型。高内聚，低耦合。

**错误处理：** 在每一级处理错误。在 UI 代码中提供用户友好的消息。在服务器端记录详细的上下文。永远不要静默吞掉错误。

**输入验证：** 在系统边界验证所有用户输入。使用基于模式的验证。快速失败并给出清晰消息。永远不要信任外部数据。

**代码质量检查清单：**
- 函数小（<50 行），文件聚焦（<800 行）
- 无深度嵌套（>4 层）
- 正确的错误处理，无硬编码值
- 可读性强，命名标识符良好

## 测试要求

**最低覆盖率：80%**

所需测试类型（全部）：
1. **单元测试** —— 单个函数、工具、组件
2. **集成测试** —— API 端点、数据库操作
3. **端到端测试** —— 关键用户流程

**TDD 工作流（强制）：**
1. 先写测试（红色）—— 测试应失败
2. 编写最小实现（绿色）—— 测试应通过
3. 重构（改进）—— 验证覆盖率达到 80%+

故障排除：检查测试隔离 → 验证模拟 → 修复实现（除非测试有误）。

## 开发工作流

1. **计划** —— 使用 planner 代理，识别依赖和风险，分阶段进行
2. **TDD** —— 使用 tdd-guide 代理，先写测试，实现，重构
3. **审查** —— 立即使用 code-reviewer 代理，处理关键/高优先级问题
4. **将知识记录在正确的位置**
   - 个人调试笔记、偏好和临时上下文 → 自动记忆
   - 团队/项目知识（架构决策、API 更改、运行手册） → 项目现有文档结构
   - 如果当前任务已经生成了相关文档或代码注释，则不要在其他地方重复相同信息
   - 如果没有明显的项目文档位置，在创建新的顶层文件之前先询问
5. **提交** —— 约定式提交格式，全面的 PR 摘要

## 工作流表面策略

- `skills/` 是规范的工作流表面。
- 新的工作流贡献应首先放在 `skills/` 中。
- `commands/` 是遗留的斜杠入口兼容表面，仅在迁移或跨工具集对等性仍需要存根时才应添加或更新。

## Git 工作流

**提交格式：** `<type>: <description>` —— 类型：feat, fix, refactor, docs, test, chore, perf, ci

**PR 工作流：** 分析完整提交历史 → 起草全面摘要 → 包含测试计划 → 使用 `-u` 标志推送。

## 架构模式

**API 响应格式：** 一致的信封结构，包含成功指示器、数据负载、错误消息和分页元数据。

**仓库模式：** 将数据访问封装在标准接口后面（findAll, findById, create, update, delete）。业务逻辑依赖抽象接口，而非存储机制。

**骨架项目：** 搜索经过实战检验的模板，使用并行代理评估（安全性、可扩展性、相关性），克隆最佳匹配，在已验证的结构内迭代。

## 性能

**上下文管理：** 对于大型重构和多文件功能，避免使用最后 20% 的上下文窗口。较低敏感度的任务（单次编辑、文档、简单修复）可容忍较高的利用率。

**构建故障排除：** 使用 build-error-resolver 代理 → 分析错误 → 增量修复 → 每次修复后验证。

## 项目结构

```
agents/          — 64 个专业子代理
skills/          — 261 个工作流技能和领域知识
commands/        — 84 条命令
hooks/           — 基于触发器的自动化
rules/           — 始终遵循的指南（通用 + 各语言）
scripts/         — 跨平台 Node.js 实用程序
mcp-configs/     — 14 个 MCP 服务器配置
tests/           — 测试套件
```

`commands/` 保留在仓库中以供兼容，但长期方向是技能优先。

## 成功指标

- 所有测试通过，覆盖率达到 80% 以上
- 无安全漏洞
- 代码可读且可维护
- 性能可接受
- 满足用户需求