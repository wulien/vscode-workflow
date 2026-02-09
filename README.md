# VS Code Custom Agents + Skills 开发工作流

> 🚀 VS Code 官方 Custom Agents + obra/superpowers Skills = 终极开发环境

## 🎯 这是什么？

完整的 AI 驱动开发工作流系统，整合了：

1. **VS Code 官方 Custom Agents** — 专门的 AI 开发助手，出现在 Agents 下拉菜单
2. **obra/superpowers Skills** — 详细的开发流程文档和最佳实践
3. **自动化工作流** — Handoffs 实现 Agent 间无缝协作

**核心能力：**
- ✅ **TDD 优先** — 先写测试，永远如此
- ✅ **系统化调试** — 找根因，不修症状
- ✅ **证据驱动** — 运行验证，不靠猜测
- ✅ **多 Agent 协作** — 并行开发复杂任务
- ✅ **工作流自动化** — 从设计到发布的完整流程

---

## ⚡ 快速开始

### 1. 验证 Custom Agents

打开 VS Code Copilot Chat，点击 **Agents 下拉菜单**，应该看到：

- 🆕 **@New-Project** — 启动新项目完整流程
- 🔄 **@Iteration** — 迭代需求开发
- ✅ **@TDD** — 测试驱动开发
- 🔍 **@Code-Review** — 系统化代码审查
- 💡 **@Brainstorming** — 需求探索 + 技术选型
- 📝 **@Writing-Plans** — 生成详细实现计划

### 2. 第一次使用

```
在 Copilot Chat 中输入:

@New-Project 我想创建一个博客系统
```

**自动工作流：**
```
1. @New-Project 分析需求
   ↓
2. Handoff → @Brainstorming (需求探索 + 技术选型)
   ↓
3. @New-Project 生成架构设计
   ↓
4. Handoff → @Writing-Plans (生成 TDD 实现计划)
   ↓
5. 选择执行: @TDD 或 Background Agent
```

### 3. 迭代开发

```
@Iteration 实现用户登录功能
  → @Writing-Plans (拆分任务)
  → @TDD (逐任务实现)
  → @Code-Review (审查)
```

---

## 📚 完整文档

- 📘 [**完整最佳实践**](VSCode开发最佳实践完整流程.md) — 11 章节完整指南
- 🚀 [**Custom Agents 整合指南**](docs/CUSTOM-AGENTS-INTEGRATION.md) — Agents 详细用法和工作流示例
- 🎛️ [**Copilot Modes 指南**](docs/COPILOT-MODES-GUIDE.md) — Local/Background/Cloud 模式详解
- 🌐 [**全局配置指南**](docs/GLOBAL-SETUP.md) — 一次配置，所有项目可用

### Skills 详细文档 (`.vscode/skills/`)

**核心工作流：**
- [新项目启动](.vscode/skills/new-project/SKILL.md) — 6 阶段完整流程
- [迭代需求开发](.vscode/skills/iteration/SKILL.md) — 6 步敏捷迭代
- [测试驱动开发](.vscode/skills/test-driven-development/SKILL.md) — RED-GREEN-REFACTOR
- [系统化调试](.vscode/skills/systematic-debugging/SKILL.md) — 4 阶段根因分析
- [代码审查](.vscode/skills/code-review/SKILL.md) — 多维度审查清单
- [完成工作](.vscode/skills/finishing-work/SKILL.md) — 验证-合并-清理

**支持工具：**
- [头脑风暴](.vscode/skills/brainstorming/SKILL.md) — 苏格拉底式提问
- [编写计划](.vscode/skills/writing-plans/SKILL.md) — TDD 任务拆分
- [多 Agent 协作](.vscode/skills/multi-agent-collaboration/SKILL.md) — 并行开发模式
- [Git Worktrees](.vscode/skills/using-git-worktrees/SKILL.md) — 并行分支开发

---

## 🔥 核心功能

### Custom Agents (快速访问)

| Agent | 用途 | 示例 |
|-------|------|------|
| **@New-Project** | 新项目启动完整流程 | `@New-Project Todo 应用` |
| **@Iteration** | 迭代需求开发 | `@Iteration 添加用户登录` |
| **@TDD** | 严格测试驱动开发 | `@TDD 实现邮箱验证` |
| **@Code-Review** | 系统化代码审查 | `@Code-Review 当前分支` |
| **@Brainstorming** | 需求探索 + 技术选型 | `@Brainstorming 电商架构` |
| **@Writing-Plans** | 生成详细实现计划 | `@Writing-Plans <设计文档>` |

### 自动化工作流 (Handoffs)

```
@New-Project → @Brainstorming → @Writing-Plans → @TDD
@Iteration → @Writing-Plans → @TDD → @Code-Review
TDD → Code Review
```

### 多模式协作

- **Local Agents** — 实时交互开发
- **Background Agents** — 异步后台任务
- **Cloud Agents** — GitHub PR 集成

### VS Code 配置

- `.github/copilot-instructions.md` — Copilot 总引导文件
- `.github/agents/*.agent.md` — 6 个 Custom Agents
- `.vscode/skills/*/SKILL.md` — 10 个详细 Skills 文档
- `.vscode/workflows.json` — 6 个预定义工作流
- `.vscode/tasks.json` — 常用开发任务
- `.vscode/settings.json` — 推荐编辑器设置
- `.vscode/extensions.json` — 推荐扩展列表

---

## 🚀 实战示例

### 场景 1: 启动新项目

```
你: @New-Project 我要创建一个博客系统

[自动工作流]
1. @New-Project 分析需求
   ↓
2. Handoff → @Brainstorming
   - 主动研究博客系统最佳实践
   - 技术选型: Next.js + PostgreSQL + Markdown
   - 生成设计文档 → docs/plans/2026-02-09-blog-system.md
   ↓
3. @New-Project 架构设计
   ↓
4. Handoff → @Writing-Plans
   - 拆分为 25 个 TDD 任务
   - 生成实现计划 → docs/plans/2026-02-09-blog-system-implementation.md
   ↓
5. 选择执行: @TDD 或 Background Agent
```

### 场景 2: 迭代开发

```
你: @Iteration 添加用户评论功能

[自动工作流]
1. @Iteration 需求分析
   - 创建分支: feature/user-comments
   ↓
2. Handoff → @Writing-Plans
   - Task 1: 评论数据模型
   - Task 2: 评论 API
   - Task 3: 评论 UI
   - ...
   ↓
3. Handoff → @TDD
   对每个任务:
   🔴 写测试 → ▶️ 确认失败 → 🟢 实现 → ✅ 通过 → 💾 提交
   ↓
4. Handoff → @Code-Review
   - 审查变更
   - 输出问题列表（Critical/Important/Minor）
   ↓
5. 修复问题 → 合并
```

### 场景 3: Bug 修复

```
你: 发现 bug，登录时空密码也能提交

[工作流]
1. 参考 systematic-debugging Skill
   - Phase 1: 重现 bug
   - Phase 2: 定位根因（验证逻辑缺失）
   ↓
2. @TDD 修复
   🔴 test/auth.spec.ts: 测试空密码被拒绝
   ▶️ 运行测试 → FAIL ✅
   🟢 src/auth/validation.ts: 添加非空检查
   ✅ 运行测试 → PASS ✅
   💾 git commit -m "fix(auth): reject empty password"
   ↓
3. @Code-Review 验证修复
```

### 场景 4: 并行开发

```
你: 同时开发用户、订单、支付三个模块

[使用 Background Agents]
1. @Iteration "实现用户模块"
   → [点击 "委托到后台" handoff]
   → Background Agent 1 在 worktree-1 中执行

2. @Iteration "实现订单模块"
   → Background Agent 2 在 worktree-2 中执行

3. @Iteration "实现支付模块"
   → Background Agent 3 在 worktree-3 中执行

4. 所有 Agent 并行工作，完成后通知
5. 完成后集成并测试
```

---

## 🎓 核心原则

### TDD — 先写测试

```
❌ 错误: 写实现 → 补测试
✅ 正确: 写测试 → 看失败 → 写实现 → 看通过
```

### 系统化胜过随意

```
❌ 错误: 试试这个，试试那个
✅ 正确: 根因调查 → 假设 → 验证 → 修复
```

### 证据优先

```
❌ 错误: "应该修好了"
✅ 正确: 运行测试 → 看到 PASS → 然后说完成
```

---

## 📚 文档

- [📖 **完整开发最佳实践**](./VSCode开发最佳实践完整流程.md) — 11 章节完整指南
- [🚀 **Custom Agents 整合指南**](./docs/CUSTOM-AGENTS-INTEGRATION.md) — Agents 详细用法和工作流示例
- [🎛️ **Copilot Modes 指南**](./docs/COPILOT-MODES-GUIDE.md) — Local/Background/Cloud 模式
- [🔧 **全局配置指南**](./docs/GLOBAL-SETUP.md) — 一次配置所有项目
- [📂 **Skills 目录**](./.vscode/skills/) — 10 个详细流程文档

---

## 📞 文件结构

```
.
├── .github/
│   ├── copilot-instructions.md       # Copilot 总引导
│   └── agents/                        # Custom Agents
│       ├── new-project.agent.md
│       ├── iteration.agent.md
│       ├── tdd.agent.md
│       ├── code-review.agent.md
│       ├── brainstorming.agent.md
│       └── writing-plans.agent.md
│
├── .vscode/
│   ├── skills/                        # Skills 详细文档
│   │   ├── new-project/SKILL.md
│   │   ├── iteration/SKILL.md
│   │   ├── test-driven-development/SKILL.md
│   │   ├── systematic-debugging/SKILL.md
│   │   ├── code-review/SKILL.md
│   │   ├── brainstorming/SKILL.md
│   │   ├── writing-plans/SKILL.md
│   │   ├── finishing-work/SKILL.md
│   │   ├── multi-agent-collaboration/SKILL.md
│   │   └── using-git-worktrees/SKILL.md
│   │
│   ├── tasks.json                     # VS Code 任务
│   ├── settings.json                  # 推荐设置
│   ├── extensions.json                # 推荐插件
│   └── workflows.json                 # 工作流定义
│
├── docs/
│   ├── CUSTOM-AGENTS-INTEGRATION.md   # 整合指南
│   ├── COPILOT-MODES-GUIDE.md         # 模式详解
│   ├── GLOBAL-SETUP.md                # 全局配置
│   └── plans/                         # 设计文档、实现计划
│
├── VSCode开发最佳实践完整流程.md
├── README.md
├── setup-global-skills.ps1        # 全局安装脚本
└── update-global-skills.ps1       # 更新脚本
```

---

## 🔧 技术要求

- **VS Code** 1.109.0+ (支持 Custom Agents 和多 Agent 特性)
- **GitHub Copilot** (Chat + Edits)
- **Git** 2.25.0+ (支持 worktrees)
- **PowerShell** 7.0+ (Windows) 或 Bash (macOS/Linux)

---

## 🐞 故障排查

### Custom Agents 不出现在下拉菜单？

1. 检查 VS Code 版本 >= 1.109.0
2. 确认文件位置：`.github/agents/*.agent.md`
3. 检查 YAML frontmatter 格式
4. 重新加载 VS Code (`Ctrl+Shift+P` → `Reload Window`)

### Handoffs 不工作？

1. 检查 `agents` 字段包含目标 Agent 名称
2. 确认 Agent 名称大小写正确
3. 确认目标 Agent 文件存在

### Skills 文档不生效？

1. 检查 `.github/copilot-instructions.md` 存在
2. 重启 Copilot Chat（关闭重开）
3. 检查 Skills 文件路径正确

---

## 🤝 贡献

欢迎改进 Skills 系统！

1. Fork 本仓库
2. 创建特性分支: `git checkout -b feature/improve-tdd-skill`
3. 提交变更（使用本项目的 Skills！）
4. 推送分支: `git push origin feature/improve-tdd-skill`
5. 创建 Pull Request

---

## 📄 许可

MIT License

---

## 🙏 致谢

- [obra/superpowers](https://github.com/obra/superpowers) — 原始灵感来源
- VS Code Team — 强大的多代理开发能力
- GitHub Copilot — AI 配对编程

---

## 📞 反馈

遇到问题或有建议？

- 提交 [Issue](https://github.com/wulien/vscode-workflow/issues)
- 参与 [Discussions](https://github.com/wulien/vscode-workflow/discussions)

---

**开始使用 Skills 系统，让 AI 按最佳实践引导你开发！** 🚀
