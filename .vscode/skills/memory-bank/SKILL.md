# Memory Bank Skill — 跨 Session 项目记忆

> obra/superpowers 的 Memory Bank 模式，适配 VS Code Copilot。
> 让每个新 session 都能继承之前的上下文。

## 核心理念

Copilot 的每个 session 都从零开始。Memory Bank 通过 **结构化的 Markdown 文件** 持久化项目知识，
让 `copilot-instructions.md` 在每次 session 启动时自动加载这些文件。

```
Session 1: 讨论架构 → 选了 Next.js → 更新记忆 → 退出
Session 2: 自动读取记忆 → "上次选了 Next.js" → 无缝继续 ✅
```

## Memory Bank 结构

```
docs/memory/
├── PROJECT.md        # 项目是什么（名称、技术栈、目标用户）
├── ARCHITECTURE.md   # 技术架构（模块、数据流、设计模式）
├── PROGRESS.md       # 当前进度（Session 记录、待办、阻塞）
├── DECISIONS.md      # 关键决策（ADR 格式，为什么选 A 不选 B）
└── CONVENTIONS.md    # 编码规范（命名、Git、代码风格）
```

### 每个文件的职责

| 文件 | 回答什么问题 | 更新频率 |
|------|------------|---------|
| **PROJECT.md** | "这个项目做什么？用什么技术？" | 项目初期频繁，之后很少 |
| **ARCHITECTURE.md** | "代码怎么组织？模块怎么交互？" | 架构变更时 |
| **PROGRESS.md** | "上次做了什么？现在该做什么？" | **每次 session 结束** |
| **DECISIONS.md** | "为什么选这个方案？" | 做重要技术决策时 |
| **CONVENTIONS.md** | "代码应该怎么写？" | 建立规范时 |

## 工作流

### Phase 1: 新项目初始化

当用户把本框架嵌入新项目后，第一次 session：

1. Copilot 读取 `copilot-instructions.md`
2. 发现 `docs/memory/` 存在但是模板状态
3. 提示用户：
   ```
   "检测到 Memory Bank 是模板状态。让我帮你初始化项目记忆。
    请告诉我：
    1. 项目名称和目标？
    2. 技术栈选择？
    3. 当前开发阶段？"
   ```
4. 根据用户回答，填充 `PROJECT.md` 和其他文件

### Phase 2: 每次 Session 开始（自动）

`copilot-instructions.md` 中的启动协议自动执行：

```
1. 读取 docs/memory/ 下所有文件
2. 用 2-3 句话汇报：
   "我已读取项目记忆。这是 [项目名]，使用 [技术栈]。
    上次做了 [概要]，当前在 [阶段]。
    接下来计划做 [待办]。需要继续还是有新任务？"
```

### Phase 3: Session 期间

正常开发，不需要额外操作。

但当发生以下情况时，应更新记忆：
- ✅ 完成了重要功能
- ✅ 做了关键技术决策
- ✅ 架构发生变化
- ✅ 发现了重要 Bug

### Phase 4: Session 结束（手动触发）

用户说 **"更新记忆"**、**"保存进度"** 或 **"session 结束"** 时：

1. **更新 `PROGRESS.md`**（必须）：
   ```markdown
   ### Session 2026-02-09 15:30 — 实现用户认证

   **做了什么:**
   - 完成 JWT Token 生成和验证
   - 实现登录 API
   - 添加认证中间件

   **未完成:**
   - 注册 API 的邮箱验证

   **下一步:**
   - 完成邮箱验证
   - 实现密码重置
   ```

2. **更新 `DECISIONS.md`**（如果做了新决策）：
   ```markdown
   ### ADR-003: 选择 JWT 而不是 Session

   **日期**: 2026-02-09
   **状态**: 已采纳

   **背景**: 需要无状态认证，支持跨域和移动端。

   **决定**: 使用 JWT + Refresh Token 方案。

   **影响**: 需要处理 Token 刷新和黑名单。
   ```

3. **更新其他文件**（如果有变化）

## 记忆维护原则

### 1. 保持简洁

```markdown
✅ 好: "使用 JWT 认证，因为需要无状态 + 跨域支持"
❌ 差: "我们经过了漫长的讨论，考虑了很多方案，最终..."
```

### 2. 面向未来的自己

写记忆时想象：**3 周后的你打开项目，需要知道什么？**

### 3. PROGRESS.md 是最重要的

- 这是唯一需要**每次 session 更新**的文件
- 保持最近 10-15 条 session 记录
- 超过的可以归档或删除

### 4. 不要重复代码中的信息

```markdown
✅ 好: "认证模块在 src/auth/，使用 JWT + bcrypt"
❌ 差: [复制粘贴 50 行代码到记忆文件]
```

### 5. ADR 只记录有意义的决策

```markdown
✅ 值得记录: "选 PostgreSQL 而不是 MongoDB，因为需要复杂关联查询"
❌ 不必记录: "选择用 const 而不是 let"
```

## 记忆读取优先级

当 session 开始时，按以下顺序理解项目：

```
1. PROJECT.md        → 我在做什么项目？
2. PROGRESS.md       → 上次做到哪了？接下来做什么？
3. ARCHITECTURE.md   → 代码怎么组织的？
4. DECISIONS.md      → 为什么做了这些选择？
5. CONVENTIONS.md    → 代码该怎么写？
```

**如果时间紧迫，至少读 PROJECT.md + PROGRESS.md。**

## 与 Custom Agents 的集成

### @Memory Agent（主动操作）

用户可以通过 `@Memory` Agent 显式管理记忆：

```
@Memory 保存进度          → 更新 PROGRESS.md
@Memory 记录决策          → 添加 ADR 到 DECISIONS.md  
@Memory 更新架构          → 更新 ARCHITECTURE.md
@Memory 查看当前状态      → 读取并汇总所有记忆文件
@Memory 初始化            → 填充模板文件
```

### 其他 Agents 的记忆行为

| Agent | 读取记忆 | 写入记忆 |
|-------|---------|---------|
| **@New-Project** | 检查是否已有记忆 | 初始化所有记忆文件 |
| **@Iteration** | 读取进度和架构 | 完成后更新进度 |
| **@TDD** | 读取规范 | — |
| **@Code-Review** | 读取架构和规范 | — |
| **@Brainstorming** | 读取项目背景 | 更新决策 |
| **@Memory** | 读取全部 | 更新全部 |

## 故障排查

### Q: 新 session 没有自动读取记忆？

**A:** 检查 `.github/copilot-instructions.md` 中是否包含 Session 启动协议。

### Q: 记忆文件太大了？

**A:** 
- `PROGRESS.md`: 只保留最近 10-15 条 session 记录
- `DECISIONS.md`: 已废弃的 ADR 标记为 `已废弃` 但保留
- 其他文件: 保持精简，指向代码而不是复制代码

### Q: 多人协作时记忆冲突？

**A:**
- 记忆文件提交到 Git，和代码一起版本控制
- 冲突时以最新的为准
- `PROGRESS.md` 按时间顺序追加，很少冲突

---

**参考：** obra/superpowers Memory Bank 概念
**集成指南：** `docs/CUSTOM-AGENTS-INTEGRATION.md`
