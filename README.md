# VS Code Skills 系统 - AI 驱动的开发工作流

> 灵感来源: [obra/superpowers](https://github.com/obra/superpowers)
> 适配 VS Code + GitHub Copilot 的可组合技能框架

---

## 🎯 这是什么？

一套完整的 AI 开发工作流系统，让 GitHub Copilot 按照最佳实践引导你开发：

- ✅ **TDD 优先** — 先写测试，永远如此
- ✅ **系统化调试** — 找根因，不修症状
- ✅ **证据驱动** — 运行验证，不靠猜测
- ✅ **多代理协作** — 并行开发复杂任务
- ✅ **工作流自动化** — 从设计到发布的完整流程

---

## ⚡ 快速开始

### 方式 1: 使用模板（最简单）

```bash
# 克隆这个仓库作为新项目的起点
git clone https://github.com/wulien/vscode-workflow.git my-new-project
cd my-new-project
rm -rf .git
git init

# 打开 VS Code，告诉 Copilot
code .
# "按照 new-project Skill 启动项目"
```

### 方式 2: 全局配置（推荐）

一次设置，所有项目可用：

```powershell
# 1. 一次性全局设置
pwsh .\setup-global-skills.ps1

# 2. 在任何新项目中运行
init-skills

# 3. 开始使用
code .
# 告诉 Copilot: "按照 iteration Skill 开始开发"
```

[📖 完整全局配置指南](./GLOBAL-SETUP.md)

---

## 📦 包含什么？

### 核心 Skills

| Skill | 用途 | 触发方式 |
|-------|------|----------|
| **new-project** | 启动新项目完整流程 | "按照 new-project Skill 启动项目" |
| **iteration** | 迭代需求开发 | "按照 iteration Skill 开始" |
| **test-driven-development** | TDD 工作流 | Copilot 自动使用 |
| **systematic-debugging** | 系统化调试 | "帮我调试这个 Bug" |
| **code-review** | 代码审查 | "审查这段代码" |
| **multi-agent-collaboration** | 多代理并行开发 | "并行开发这些模块" |
| **finishing-work** | 完成合并流程 | "完成这个任务" |
| **using-git-worktrees** | 多分支并行工作 | "使用 git worktrees" |
| **writing-plans** | 编写实现计划 | "帮我写实现计划" |
| **brainstorming** | 需求探索设计 | Copilot 自动调用 |

### 配置文件

- `.github/copilot-instructions.md` — Copilot 会话引导
- `.vscode/workflows.json` — 6 个预定义工作流
- `.vscode/tasks.json` — 常用开发任务
- `.vscode/settings.json` — 推荐编辑器设置
- `.vscode/extensions.json` — 推荐扩展列表

---

## 🚀 使用示例

### 场景 1: 启动新项目

```
你: "我要创建一个博客系统，使用 new-project Skill"

Copilot:
1. 进入头脑风暴阶段...
2. 生成架构设计文档 → docs/plans/2026-02-09-blog-system.md
3. 技术栈选型: Next.js + TypeScript + PostgreSQL
4. 编写实现计划 → docs/plans/2026-02-09-blog-system-implementation.md
5. 开始 TDD 实现...
```

### 场景 2: 开发新功能

```
你: "添加用户评论功能，用 iteration Skill"

Copilot:
1. 分析需求和影响范围
2. 创建特性分支: feature/user-comments
3. 生成实现计划（15 个 TDD 任务）
4. 逐任务实现: 先测试 → 实现 → 验证 → 提交
5. 代码审查
6. 合并到 main
```

### 场景 3: 修复 Bug

```
你: "登录失败了，帮我调试"

Copilot (自动使用 systematic-debugging):
1. 根因调查 → 收集错误日志
2. 模式分析 → 对比正常登录流程
3. 假设验证 → Token 过期检查失败
4. TDD 修复:
   - 编写测试重现 Bug
   - 修复过期检查逻辑
   - 验证所有测试通过
5. 提交修复
```

### 场景 4: 多模块并行开发

```
你: "同时开发用户、订单、支付三个模块"

Copilot (使用 multi-agent-collaboration):
1. 建议使用 git worktrees
2. 创建 3 个独立 worktrees
3. 在 3 个 VS Code 窗口并行开发
4. 每个模块独立 TDD
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

- [📖 完整开发最佳实践](./VSCode开发最佳实践完整流程.md)
- [🔧 全局配置指南](./GLOBAL-SETUP.md)
- [💡 工作流定义](./vscode/workflows.json)
- [🎯 Skills 目录](./.vscode/skills/)

---

## 🛠 技术栈

- **VS Code** 1.109.0+ (支持多代理特性)
- **GitHub Copilot** (Chat + Edits)
- **Git** (支持 worktrees)
- **PowerShell** (初始化脚本)

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
