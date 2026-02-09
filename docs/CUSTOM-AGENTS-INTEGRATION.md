# Custom Agents 与 Skills 系统整合指南

> VS Code 官方 Custom Agents + obra/superpowers Skills = 终极开发工作流

## 📖 概述

### 两个系统的关系

| 系统 | 位置 | 格式 | 用途 |
|------|------|------|------|
| **Custom Agents** | `.github/agents/*.agent.md` | VS Code 官方格式 | **快速访问**，出现在 Agents 下拉菜单 |
| **Skills Documentation** | `.vscode/skills/*/SKILL.md` | superpowers 格式 | **详细参考**，完整流程说明 |

### 为什么两者并存？

**Custom Agents 提供：**
- ✅ UI 集成（Agents 下拉菜单）
- ✅ Handoffs（自动工作流移交）
- ✅ 工具限制（精确控制可用工具）
- ✅ 角色扮演（专门的 persona）

**Skills 文档提供：**
- ✅ 详细流程说明
- ✅ 示例和模板
- ✅ 最佳实践指南
- ✅ 可复用的参考资料

**组合效果：**
```
Custom Agent (快速入口) → 引用 → Skills 文档 (详细指导)
```

---

## 🎯 核心 Custom Agents

### 1. New Project Agent

**调用方式：**
```
用户: @New-Project 我想创建一个博客系统
```

**工作流：**
```
@New-Project (协调者)
  ↓ handoff
@Brainstorming (需求探索 + 技术选型)
  ↓ 用户确认方案
@New-Project (架构设计)
  ↓ handoff
@Writing-Plans (生成实现计划)
  ↓ 用户选择执行方式
@TDD (开始实现) 或 Background Agent (异步)
```

**文件：** [.github/agents/new-project.agent.md](../.github/agents/new-project.agent.md)
**详细文档：** [.vscode/skills/new-project/SKILL.md](../.vscode/skills/new-project/SKILL.md)

---

### 2. Iteration Agent

**调用方式：**
```
用户: @Iteration 实现用户登录功能
```

**工作流：**
```
@Iteration (需求分析 + Git 分支)
  ↓ handoff
@Writing-Plans (拆分 TDD 任务)
  ↓ handoff
@TDD (RED-GREEN-REFACTOR 循环)
  ↓ handoff
@Code-Review (自我审查)
  ↓
合并或创建 PR
```

**文件：** [.github/agents/iteration.agent.md](../.github/agents/iteration.agent.md)
**详细文档：** [.vscode/skills/iteration/SKILL.md](../.vscode/skills/iteration/SKILL.md)

---

### 3. TDD Agent

**调用方式：**
```
用户: @TDD 实现邮箱验证功能
```

**工作流：**
```
对每个功能：
1. 🔴 RED - 写失败的测试
2. ▶️ RUN - 确认失败
3. 🟢 GREEN - 最小实现
4. ✅ RUN - 确认通过
5. ♻️ REFACTOR - 优化（可选）
6. 💾 COMMIT - 提交
```

**特点：**
- 严格执行 TDD
- 拒绝"测试后补"
- 每步都要看到输出

**文件：** [.github/agents/tdd.agent.md](../.github/agents/tdd.agent.md)
**详细文档：** [.vscode/skills/test-driven-development/SKILL.md](../.vscode/skills/test-driven-development/SKILL.md)

---

### 4. Code Review Agent

**调用方式：**
```
用户: @Code-Review 审查当前分支的变更
```

**审查维度：**
1. ✅ Correctness (正确性)
2. 🔒 Security (安全性)
3. ⚡ Performance (性能)
4. 🔧 Maintainability (可维护性)
5. 🧪 Test Coverage (测试覆盖)
6. 🏗️ Architecture (架构符合度)

**输出：**
- Critical — 必须修复
- Important — 应该修复
- Minor — 建议改进

**文件：** [.github/agents/code-review.agent.md](../.github/agents/code-review.agent.md)
**详细文档：** [.vscode/skills/code-review/SKILL.md](../.vscode/skills/code-review/SKILL.md)

---

### 5. Brainstorming Agent

**调用方式：**
```
用户: @Brainstorming 电商系统的购物车模块
```

**5 阶段流程：**
1. 需求探索（苏格拉底式提问）
2. 技术选型研究（主动搜索最佳实践）
3. 方案对比（2-3 个方案 + 优劣分析）
4. 架构探讨（系统设计）
5. 确认和总结（输出设计文档）

**特点：**
- 主动研究，不是简单回答
- 开放式提问，引导思考
- 多方案对比，让用户决策

**文件：** [.github/agents/brainstorming.agent.md](../.github/agents/brainstorming.agent.md)
**详细文档：** [.vscode/skills/brainstorming/SKILL.md](../.vscode/skills/brainstorming/SKILL.md)

---

### 6. Writing Plans Agent

**调用方式：**
```
用户: @Writing-Plans 基于 docs/plans/2026-02-09-auth-design.md 生成实现计划
```

**输出格式：**
```markdown
## Task 1: 用户数据模型

**文件操作:**
- 创建: `src/models/User.ts`
- 测试: `tests/models/User.spec.ts`

### Step 1: 🔴 编写失败的测试
[完整测试代码]

### Step 2: ▶️ 运行测试确认失败
命令: `npm test -- --grep "User"`
预期输出: FAIL - "User is not defined"

### Step 3: 🟢 编写最小实现
[完整实现代码]

### Step 4: ✅ 运行测试确认通过
[...]

### Step 5: 💾 提交
```bash
git add ...
git commit -m "feat(user): add User model"
```
```

**特点：**
- 精确的文件路径
- 完整的代码（不是伪代码）
- 明确的命令和预期输出
- 2-5 分钟的任务粒度

**文件：** [.github/agents/writing-plans.agent.md](../.github/agents/writing-plans.agent.md)
**详细文档：** [.vscode/skills/writing-plans/SKILL.md](../.vscode/skills/writing-plans/SKILL.md)

---

## 🔄 完整工作流示例

### 示例 1: 从零启动新项目

```bash
# 用户
我想创建一个 Todo 应用，要支持用户认证和权限管理

# Chat
@New-Project
```

**自动流程：**

1. **@New-Project** 分析需求，识别为新项目

2. **Handoff → @Brainstorming**
   ```
   [Brainstorming 主动研究]
   - 搜索 "Todo 应用最佳实践"
   - 对比 REST vs GraphQL
   - 研究认证方案（JWT vs Session）

   [苏格拉底式提问]
   Q: 这个应用是个人使用还是团队协作？
   Q: 预计用户规模？
   Q: 需要实时同步吗？

   [输出]
   docs/plans/2026-02-09-todo-app-design.md
   ```

3. **用户确认方案**
   ```
   用户: 采用方案 B (Node.js + PostgreSQL + JWT)
   ```

4. **@New-Project** 生成架构设计
   ```markdown
   ## 技术栈
   - Backend: Express + TypeScript
   - Database: PostgreSQL + TypeORM
   - Auth: JWT + bcrypt
   - Testing: Jest + Supertest

   ## 模块结构
   - auth (认证授权)
   - todo (Todo CRUD)
   - user (用户管理)
   ```

5. **Handoff → @Writing-Plans**
   ```
   [生成详细实现计划]
   docs/plans/2026-02-09-todo-app-implementation.md

   Task 1: 搭建项目基础结构
   Task 2: 配置数据库连接
   Task 3: 实现用户模型和认证
   Task 4: 实现 Todo CRUD
   Task 5: 集成测试
   ...
   ```

6. **用户选择执行方式**
   ```
   选项 1: 在本会话继续 → @TDD
   选项 2: 委托到后台 → Background Agent
   选项 3: 自己执行 → 保存计划
   ```

7. **@TDD** 或 **Background Agent** 执行

---

### 示例 2: 迭代需求开发

```bash
# 用户
@Iteration 在 Todo 应用中添加"任务优先级"功能
```

**自动流程：**

1. **@Iteration** 需求分析
   ```bash
   # 检查当前环境
   git status
   git checkout main
   git pull

   # 创建特性分支
   git checkout -b feature/todo-priority
   ```

2. **Handoff → @Writing-Plans**
   ```markdown
   ## Task 1: 数据库迁移
   - 添加 `priority` 字段到 todos 表
   - 创建迁移脚本

   ## Task 2: 更新 Todo 模型
   - 添加 priority 属性
   - 验证逻辑 (1-5)

   ## Task 3: 更新 API
   - POST/PUT 接受 priority
   - GET 返回 priority
   - 按优先级排序

   ## Task 4: 集成测试
   - 端到端测试优先级功能
   ```

3. **Handoff → @TDD** 逐任务执行
   ```
   Task 1: 数据库迁移
     🔴 写迁移测试
     🟢 创建迁移脚本
     ✅ 运行迁移
     💾 提交

   Task 2: 更新 Todo 模型
     🔴 测试 priority 验证
     🟢 实现验证逻辑
     ✅ 测试通过
     💾 提交

   ...
   ```

4. **Handoff → @Code-Review**
   ```markdown
   # Code Review Report

   ## 优点
   - ✅ 完整的测试覆盖
   - ✅ 数据库迁移有回滚脚本

   ## 问题

   ### Important
   1. **src/todo/todo.service.ts:45**
      - 未验证 priority 范围
      - 建议: 添加 1-5 范围检查

   ## 评估
   - 是否可以合并: 修复后可以
   ```

5. **修复问题，最终合并**
   ```bash
   git checkout main
   git merge feature/todo-priority
   git push origin main
   ```

---

### 示例 3: Bug 修复

```bash
# 用户
发现一个 bug：创建 Todo 时，空标题也能提交
```

**流程：**

1. **参考 systematic-debugging Skill**
   ```markdown
   ## Phase 1: 重现
   - 写失败的测试（RED）

   ## Phase 2: 定位
   - 检查 Todo 验证逻辑

   ## Phase 3: 修复
   - @TDD 修复验证

   ## Phase 4: 验证
   - 确认修复 + 回归测试
   ```

2. **@TDD 执行修复**
   ```
   🔴 test/todo.spec.ts: 测试空标题被拒绝
   ▶️ 运行测试 → FAIL ✅
   🟢 src/todo/todo.validation.ts: 添加非空检查
   ✅ 运行测试 → PASS ✅
   💾 git commit -m "fix(todo): reject empty title"
   ```

---

## 🔧 高级特性

### 1. Background Agents（异步任务）

**使用场景：**
- 大规模重构
- 运行完整测试套件
- 实现多个并行功能

**用法：**
```bash
# 从 Iteration 或 Writing Plans 委托到后台
@Iteration "实现评论功能"
  → [点击 "委托到后台" handoff]
  → Background Agent 在 Git worktree 中执行

# 或直接调用
/task execute @TDD <实现计划>
```

**工作原理：**
```
1. 创建 Git worktree (隔离分支)
2. Background Agent 在后台执行计划
3. 完成后通知 + 提供合并选项
```

**参考 Skill：** [using-git-worktrees](../.vscode/skills/using-git-worktrees/SKILL.md)

---

### 2. Cloud Agents（GitHub PR）

**使用场景：**
- 创建 Pull Request
- PR 审查和反馈
- CI/CD 触发

**用法：**
```bash
# 从 Iteration 创建 PR
@Iteration "实现登录功能"
  → [完成实现]
  → [点击 "创建 PR" handoff]
  → Cloud Agent 创建 GitHub PR

# 或直接使用
/task @cloud create PR for feature/login
```

---

### 3. 多 Agent 协作

**并行开发：**
```
主 Agent: 负责核心功能
  ├─ Background Agent 1: 编写文档
  ├─ Background Agent 2: 更新测试
  └─ Background Agent 3: 重构旧代码
```

**参考 Skill：** [multi-agent-collaboration](../.vscode/skills/multi-agent-collaboration/SKILL.md)

---

## 📁 文件结构

```
Project/
├── .github/
│   ├── copilot-instructions.md       # 总引导文件
│   └── agents/                        # Custom Agents
│       ├── new-project.agent.md
│       ├── iteration.agent.md
│       ├── tdd.agent.md
│       ├── code-review.agent.md
│       ├── brainstorming.agent.md
│       └── writing-plans.agent.md
│
├── .vscode/
│   └── skills/                        # 详细 Skills 文档
│       ├── new-project/SKILL.md
│       ├── iteration/SKILL.md
│       ├── test-driven-development/SKILL.md
│       ├── code-review/SKILL.md
│       ├── brainstorming/SKILL.md
│       ├── writing-plans/SKILL.md
│       ├── systematic-debugging/SKILL.md
│       ├── finishing-work/SKILL.md
│       ├── using-git-worktrees/SKILL.md
│       └── multi-agent-collaboration/SKILL.md
│
└── docs/
    ├── plans/                         # 设计和实现计划
    │   ├── YYYY-MM-DD-<topic>.md                    # 设计文档
    │   └── YYYY-MM-DD-<topic>-implementation.md     # 实现计划
    └── CUSTOM-AGENTS-INTEGRATION.md   # 本文档
```

---

## 🎓 最佳实践

### 1. 何时使用 Custom Agents

**✅ 使用 Custom Agents (快速入口)：**
- 启动新工作流
- 需要 handoffs 自动移交
- 需要工具限制（如只读 Agent）

**✅ 引用 Skills 文档 (详细指导)：**
- Agent 内部引用详细流程
- 用户自己学习完整方法
- 其他团队成员参考

### 2. Handoffs 设计原则

**好的 Handoff：**
```yaml
handoffs:
  - label: 开始 TDD 实现                    # 清晰的动作
    agent: TDD                            # 明确的目标 Agent
    prompt: 按照计划逐任务执行 TDD 循环     # 具体的指令
    send: false                           # 手动确认
```

**避免：**
```yaml
handoffs:
  - label: 下一步        # ❌ 太模糊
    agent: SomeAgent
    prompt: 继续          # ❌ 不具体
    send: true           # ❌ 自动发送可能不安全
```

### 3. 工具限制策略

**只读 Agent (研究型)：**
```yaml
tools: ['search', 'fetch', 'read']
```

**写入 Agent (实现型)：**
```yaml
tools: ['read', 'edit', 'run']
```

**全能 Agent (协调型)：**
```yaml
tools: ['search', 'fetch', 'read', 'edit', 'run', 'agent']
```

### 4. 命名规范

**Custom Agents:**
- `new-project.agent.md` → `@New-Project`
- `tdd.agent.md` → `@TDD`
- 文件名小写连字符，Agent 名称 PascalCase

**Skills:**
- `new-project/SKILL.md`
- 文件名小写连字符，SKILL.md 大写

---

## 🚀 快速开始

### For 用户

1. **确认 Custom Agents 可用**
   ```
   在 VS Code Copilot Chat 中，点击 Agents 下拉菜单
   应该看到: @New-Project, @Iteration, @TDD, etc.
   ```

2. **启动新项目**
   ```
   @New-Project 我想创建一个...
   ```

3. **迭代开发**
   ```
   @Iteration 实现XX功能
   ```

4. **查看详细文档**
   ```
   打开 .vscode/skills/<skill-name>/SKILL.md
   ```

### For 团队

1. **复制到项目根目录**
   ```bash
   cp -r .github/agents <your-project>/.github/
   cp -r .vscode/skills <your-project>/.vscode/
   cp .github/copilot-instructions.md <your-project>/.github/
   ```

2. **定制 Agents**
   - 修改 `.github/agents/*.agent.md`
   - 调整 handoffs 和 tools
   - 更新 Skills 文档

3. **提交到代码仓库**
   ```bash
   git add .github/ .vscode/
   git commit -m "feat: add Custom Agents workflow"
   ```

---

## 📚 参考资料

- **VS Code 官方文档:**
  - [Custom Agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
  - [Multi-Agent Systems](https://code.visualstudio.com/docs/copilot/copilot-subagents)
  - [Background Agents](https://code.visualstudio.com/docs/copilot/copilot-background-agents)
  - [Cloud Agents](https://code.visualstudio.com/docs/copilot/copilot-cloud-agents)

- **obra/superpowers:**
  - [GitHub Repo](https://github.com/obra/superpowers)
  - [Skills 设计理念](https://github.com/obra/superpowers#skills)

- **本项目文档:**
  - [VSCode开发最佳实践完整流程.md](../VSCode开发最佳实践完整流程.md)
  - [Copilot Modes Guide](./COPILOT-MODES-GUIDE.md)

---

## 🆘 故障排查

### Q: Custom Agents 不出现在下拉菜单？

**A:** 检查：
1. VS Code 版本 >= 1.109.0
2. 文件位置：`.github/agents/*.agent.md`
3. YAML frontmatter 格式正确
4. 重新加载 VS Code

### Q: Handoffs 不工作？

**A:** 检查：
1. `agents` 字段包含目标 Agent 名称
2. Agent 名称大小写匹配
3. 目标 Agent 文件存在

### Q: 如何禁用某个 Agent？

**A:** 在 YAML frontmatter 添加：
```yaml
user-invokable: false
```

### Q: 如何让 Agent 只读？

**A:** 限制 tools：
```yaml
tools: ['search', 'fetch', 'read']
```

---

**版本：** 1.0.0
**更新日期：** 2026-02-09
**维护者：** wulien
