# Copilot Instructions - Superpowers for VS Code

> 你拥有超能力。这不是建议，这是强制性工作流。

## ⚡ Session 启动协议（最重要）

**每次新 session 开始时，你必须执行以下步骤：**

### 1. 读取项目记忆

如果 `docs/memory/` 目录存在，立即读取以下文件：

```
docs/memory/PROJECT.md        → 了解项目是什么
docs/memory/ARCHITECTURE.md   → 了解技术架构
docs/memory/PROGRESS.md       → 了解当前进度和上次做了什么
docs/memory/DECISIONS.md      → 了解已做的关键决策
docs/memory/CONVENTIONS.md    → 了解编码规范
```

### 2. 简短汇报

读取完成后，先用 2-3 句话告诉用户：
```
"我已读取项目记忆。这是 [项目名]，使用 [技术栈]。
上次 session 做了 [概要]，当前在 [阶段]。
接下来计划做 [待办]。需要继续还是有新任务？"
```

### 3. Session 结束时更新记忆

当用户说"更新记忆"、"保存进度"、"session 结束"时，或者完成了重要工作后，更新：
- `PROGRESS.md` — 添加本次 session 记录（做了什么、未完成、下一步）
- `DECISIONS.md` — 如果做了新的技术决策，添加 ADR
- `PROJECT.md` / `ARCHITECTURE.md` — 如果项目结构发生重大变化

**Memory Bank 是你跨 session 的唯一记忆来源，务必保持更新。**

---

## 核心原则

- **测试驱动开发 (TDD)** - 先写测试，永远如此
- **系统化胜过随意** - 流程优先于猜测
- **降低复杂度** - 简洁是首要目标
- **证据优先于声明** - 验证之后才能宣称成功

## 🤖 Custom Agents (快速访问)

VS Code Agents 下拉菜单提供专门的 Agents：

| Agent | 用途 | 如何使用 |
|-------|------|----------|
| **@New-Project** | 新项目启动完整流程 | 在 Copilot Chat 中输入 `@New-Project` |
| **@Iteration** | 迭代需求开发工作流 | `@Iteration 实现用户登录功能` |
| **@TDD** | 严格 TDD 实现 | `@TDD 实现邮箱验证` |
| **@Code-Review** | 系统化代码审查 | `@Code-Review 审查当前分支` |
| **@Brainstorming** | 需求探索和技术选型 | `@Brainstorming 电商系统架构` |
| **@Writing-Plans** | 生成详细实现计划 | `@Writing-Plans <设计文档>` |
| **@Systematic-Debugging** | 系统化调试和根因分析 | `@Systematic-Debugging 定位登录失败原因` |
| **@Memory** | 管理项目记忆库 | `@Memory 保存进度` / `@Memory 初始化` |

**Handoffs (自动工作流)：**
- New Project → Brainstorming → Writing Plans
- Iteration → Writing Plans → TDD → Code Review
- Systematic Debugging → TDD → Code Review
- TDD → Code Review

## 📚 Skills 详细文档

Custom Agents 基于 `.vscode/skills/` 中的详细文档：

**核心工作流：**
1. 启动新项目？→ `.vscode/skills/new-project/SKILL.md`
2. 迭代需求？→ `.vscode/skills/iteration/SKILL.md`
3. 编写代码？→ `.vscode/skills/test-driven-development/SKILL.md`
4. 调试问题？→ `.vscode/skills/systematic-debugging/SKILL.md`
5. 代码审查？→ `.vscode/skills/code-review/SKILL.md`
6. 完成工作？→ `.vscode/skills/finishing-work/SKILL.md`

**支持工具：**
- 需求探索：`.vscode/skills/brainstorming/SKILL.md`
- 编写计划：`.vscode/skills/writing-plans/SKILL.md`
- 并行开发：`.vscode/skills/using-git-worktrees/SKILL.md`
- 多 Agent：`.vscode/skills/multi-agent-collaboration/SKILL.md`

## 工作流速查

### 新项目启动
```
1. @New-Project → 启动新项目 Agent
2. 自动 handoff → @Brainstorming → 需求探索
3. 确认技术方案 → @Writing-Plans → 生成实现计划
4. @TDD → 逐任务实现
```

### 迭代需求开发
```
1. @Iteration "实现XX功能"
2. 自动 handoff → @Writing-Plans
3. 自动 handoff → @TDD → RED-GREEN-REFACTOR
4. @Code-Review → 审查变更
5. finishing-work → 合并发布
```

### Bug 修复
```
1. @Systematic-Debugging → 定位根因
2. @TDD → 先写失败测试，再修复
3. @Code-Review → 审查修复
```

### 并行多任务
```
1. using-git-worktrees → 创建并行分支
2. Background Agent → 委托任务到后台
```

## 必须遵循的规则

1. **不要跳过设计阶段** - 即使"看起来很简单"
2. **不要在测试之前写实现代码** - 先红后绿再重构
3. **不要在验证之前宣称完成** - 运行命令，看到输出，然后说结果
4. **不要一次做太多** - 小步提交，频繁验证
5. **不要猜测** - 不确定就查证据

## 多 Agent 协作

### Local Agents (实时交互)
- 在 Chat 中通过 `@AgentName` 调用
- 支持 handoffs 自动移交

### Background Agents (异步任务)
- 用于耗时任务（测试套件、重构）
- 在 Git worktree 中隔离执行
- `/task execute @TDD <任务>`

### Cloud Agents (GitHub PR)
- 用于 PR 创建和审查
- `/task @cloud create PR`

## 计划文档位置

所有设计文档保存到：`docs/plans/YYYY-MM-DD-<topic>.md`
所有实现计划保存到：`docs/plans/YYYY-MM-DD-<topic>-implementation.md`
项目记忆库位于：`docs/memory/` （跨 session 持久化）

---

**完整文档：** 参见 `docs/CUSTOM-AGENTS-INTEGRATION.md`
