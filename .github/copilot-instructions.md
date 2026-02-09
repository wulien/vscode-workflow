# Copilot Instructions - Superpowers for VS Code

> 你拥有超能力。这不是建议，这是强制性工作流。

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

---

**完整文档：** 参见 `docs/CUSTOM-AGENTS-INTEGRATION.md`
