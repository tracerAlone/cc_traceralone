# agents.md

本文档描述在 cc_traceralone 项目中可使用的 AI Agent 类型、职责划分及协作规范。

## Agent 类型

### 1. general-purpose
**用途：** 通用任务处理，适合跨文件的复杂多步骤任务。  
**工具权限：** 全部工具。  
**适用场景：**
- 修复跨模块的 bug
- 端到端功能实现
- 代码库范围内的关键词搜索（当简单 Grep/Glob 无法满足时）

---

### 2. Explore
**用途：** 快速探索代码库结构，回答关于代码的问题。  
**工具权限：** 除 Agent、ExitPlanMode、Edit、Write、NotebookEdit 以外的全部工具。  
**适用场景：**
- 查找特定文件或目录（`src/**/*.ts`）
- 搜索代码中的关键词或 API 端点
- 回答"这段代码是怎么工作的"类问题
- 不需要写入文件的只读调研

**吞吐量级别：**
- `quick` — 基础搜索，速度最快
- `medium` — 中等深度探索
- `very thorough` — 跨多个位置的全面分析

---

### 3. Plan
**用途：** 软件架构设计，输出实现方案。  
**工具权限：** 除 Agent、ExitPlanMode、Edit、Write、NotebookEdit 以外的全部工具。  
**适用场景：**
- 在动手之前设计实现策略
- 识别关键文件和依赖
- 评估架构权衡
- 输出分步骤计划

---

### 4. claude-code-guide
**用途：** 回答关于 Claude Code CLI、Claude Agent SDK 和 Claude API 的问题。  
**工具权限：** Glob、Grep、Read、WebFetch、WebSearch。  
**适用场景：**
- Claude Code 功能、Hooks、斜杠命令、MCP Server、设置、IDE 集成
- 构建自定义 Agent
- Anthropic SDK 的使用方式

> **注意：** 若同类问题已有正在运行的 claude-code-guide agent，优先通过 SendMessage 继续对话，而不是新建 agent。

---

### 5. statusline-setup
**用途：** 配置 Claude Code 状态栏设置。  
**工具权限：** Read、Edit。  
**适用场景：** 修改 Claude Code 状态行显示。

---

## Agent 使用规范

### 何时启动 Agent

| 情况 | 推荐做法 |
|------|---------|
| 已知目标文件 | 直接使用 Read / Grep / Glob，无需 Agent |
| 跨文件开放式调研 | 使用 Explore Agent |
| 需要设计方案 | 使用 Plan Agent |
| 多个独立子任务 | 并行启动多个 Agent（单条消息多个 tool call） |
| 依赖上一步结果 | 串行启动，等待前一个完成再启动下一个 |

### Prompt 编写要求

- 明确说明目标和背景，不要让 Agent 猜测意图。
- 注明是**只读调研**还是**需要写入文件**。
- 如果需要简短回答，明确说明字数限制（如"200字以内"）。
- 包含已知的文件路径、行号、已排除的方向，避免重复劳动。

### 后台运行

- 对于与当前工作完全独立的任务，使用 `run_in_background: true`。
- 后台 Agent 完成后会自动通知，无需轮询或 sleep。

### 继续已有 Agent

- 通过 `SendMessage` + Agent ID/名称 继续已有 Agent，保留完整上下文。
- 新建 Agent 不保留历史上下文，Prompt 必须自包含。

---

## GitHub 操作规范

所有 GitHub 交互（PR、Issue、评论、CI 状态）必须通过 `mcp__github__*` 工具完成。

| 操作 | 工具 |
|------|------|
| 读取 PR / Issue | `mcp__github__pull_request_read` / `mcp__github__issue_read` |
| 创建 PR | `mcp__github__create_pull_request` |
| 发表评论 | `mcp__github__add_issue_comment` |
| 推送文件 | `mcp__github__push_files` |
| 搜索代码 | `mcp__github__search_code` |

**限制：** 所有操作只能针对 `traceralone/cc_traceralone`，禁止操作其他仓库。

---

## 安全与权限边界

- 仅协助授权的安全测试、CTF、防御性安全和教育场景。
- 拒绝生成破坏性技术、DoS 攻击、批量目标或恶意检测规避代码。
- 双重用途安全工具（C2 框架、凭据测试、漏洞利用开发）需要明确授权上下文。
- 不猜测或生成外部 URL，除非对编程任务有明确帮助。

---

## 更新说明

当项目新增 Agent 类型、工具或工作流时，请同步更新本文档。
