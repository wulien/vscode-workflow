# Copilot Instructions - Superpowers for VS Code

> 你拥有超能力。这不是建议，这是强制性工作流。

## 核心原则

- **测试驱动开发 (TDD)** - 先写测试，永远如此
- **系统化胜过随意** - 流程优先于猜测
- **降低复杂度** - 简洁是首要目标
- **证据优先于声明** - 验证之后才能宣称成功

## Skills 系统

你的开发技能库位于 `.vscode/skills/` 目录。

**在任何任务之前，你必须检查是否有相关的 Skill：**

1. 启动新项目？→ 使用 `skills/new-project/` 
2. 迭代需求？→ 使用 `skills/iteration/`
3. 编写代码？→ 使用 `skills/test-driven-development/`
4. 调试问题？→ 使用 `skills/systematic-debugging/`
5. 代码审查？→ 使用 `skills/code-review/`
6. 完成工作？→ 使用 `skills/finishing-work/`

**工作流速查：**

```
新项目：brainstorming → 架构设计 → writing-plans → 搭建工程 → 开始开发
迭代需求：需求分析 → 创建分支 → writing-plans → TDD实现 → 代码审查 → 合并发布
Bug修复：systematic-debugging → TDD修复 → 验证 → 合并
```

## 必须遵循的规则

1. **不要跳过设计阶段** - 即使"看起来很简单"
2. **不要在测试之前写实现代码** - 先红后绿再重构
3. **不要在验证之前宣称完成** - 运行命令，看到输出，然后说结果
4. **不要一次做太多** - 小步提交，频繁验证
5. **不要猜测** - 不确定就查证据

## 多 Agent 协作

当任务可以分解为独立子任务时：
- 使用 `skills/multi-agent-collaboration/` 中的协作模式
- 每个 Agent 负责一个独立域
- 主 Agent 负责协调和整合

## 计划文档位置

所有设计文档保存到：`docs/plans/YYYY-MM-DD-<topic>.md`
所有实现计划保存到：`docs/plans/YYYY-MM-DD-<topic>-implementation.md`
