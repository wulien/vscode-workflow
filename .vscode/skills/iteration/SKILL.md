---
name: iteration-development
description: 迭代开发需求时使用。从需求分析到代码合并的完整流程，适用于已有项目的功能开发、Bug修复和优化改进。
---

# 迭代需求开发 Skill

## 概述

在已有项目中进行功能迭代的完整流程。包含需求分析、分支管理、TDD 实现、代码审查和合并发布。

## 触发条件

- 在现有项目中添加新功能
- 修复 Bug
- 性能优化或重构
- "帮我实现 XXX 功能"
- "这个 bug 怎么修"

## 完整流程

```
准备阶段: 拉取最新代码 → 检查环境
    ↓
阶段 1: 需求分析 → 确认范围
    ↓
阶段 2: 创建特性分支
    ↓
阶段 3: 编写实现计划
    ↓
阶段 4: TDD 实现（循环）
    ↓
阶段 5: 自查与代码审查
    ↓
阶段 6: 合并与清理
```

---

## 准备阶段: 环境就绪

**每次开始工作前必须执行：**

```bash
# 1. 拉取最新代码
git pull origin main

# 2. 安装依赖（如果有更新）
npm install  # 或对应的包管理器

# 3. 运行测试确保基线通过
npm test

# 4. 检查当前分支状态
git status
```

**如果测试不通过：** 先修复已有问题，不要在broken的基线上开始新工作。

---

## 阶段 1: 需求分析

### 对于功能需求

1. **澄清需求**
   - 这个功能要解决什么问题？
   - 谁是用户？使用场景？
   - 验收标准是什么？
   - 有什么边界条件？

2. **评估影响**
   - 需要修改哪些模块？
   - 是否有破坏性变更？
   - 是否需要数据迁移？

3. **确定范围**
   - 最小可行功能 (MVP) 是什么？
   - 哪些可以后续迭代？
   - DRY / YAGNI 检查

### 对于 Bug 修复

**引用 Skill:** `skills/systematic-debugging/SKILL.md`

1. **重现问题** — 确认你能稳定重现
2. **定位根因** — 不要修症状
3. **确定修复范围** — 最小变更

---

## 阶段 2: 创建特性分支

### 分支命名约定

```
feature/<简短描述>     # 新功能
fix/<简短描述>         # Bug 修复
refactor/<简短描述>    # 重构
perf/<简短描述>        # 性能优化
docs/<简短描述>        # 文档
```

### 创建分支

```bash
# 从最新的 main 分支创建
git checkout main
git pull origin main
git checkout -b feature/<name>
```

### 验证基线

```bash
# 确认在新分支上测试全部通过
npm test
```

---

## 阶段 3: 编写实现计划

**引用 Skill:** `skills/writing-plans/SKILL.md`

### 计划模板

```markdown
# [功能名] 实现计划

**目标:** [一句话描述]
**分支:** feature/<name>
**预计任务数:** N

---

### Task 1: [组件名]

**文件:**
- 创建: `exact/path/to/file.ts`
- 修改: `exact/path/to/existing.ts`
- 测试: `tests/exact/path/to/test.ts`

**步骤 1: 编写失败的测试**
[具体测试代码]

**步骤 2: 运行测试确认失败**
运行: `npm test -- --grep "test name"`
预期: FAIL

**步骤 3: 编写最小实现**
[具体实现代码]

**步骤 4: 运行测试确认通过**
运行: `npm test -- --grep "test name"`
预期: PASS

**步骤 5: 提交**
```bash
git add <files>
git commit -m "feat: <description>"
```
```

### 计划保存位置

```
docs/plans/YYYY-MM-DD-<feature-name>.md
```

---

## 阶段 4: TDD 实现

**引用 Skill:** `skills/test-driven-development/SKILL.md`

### 每个任务的循环

```
RED:    写失败的测试 → 运行 → 确认失败
GREEN:  写最小实现 → 运行 → 确认通过
REFACTOR: 优化代码 → 运行 → 确认仍然通过
COMMIT: git add → git commit
```

### 实现检查清单（每个任务）

```
任务实现检查：
- [ ] 测试先写
- [ ] 看到测试失败
- [ ] 失败原因是预期的（功能缺失，不是语法错误）
- [ ] 写了最小代码使测试通过
- [ ] 所有测试通过
- [ ] 输出干净（无错误、无警告）
- [ ] 代码已提交
```

### 提交信息约定

```
类型(作用域): 简短描述

类型:
  feat:     新功能
  fix:      Bug 修复
  docs:     文档更新
  style:    格式调整（不影响逻辑）
  refactor: 重构
  perf:     性能优化
  test:     测试
  chore:    构建/工具
```

---

## 阶段 5: 自查与代码审查

**引用 Skill:** `skills/code-review/SKILL.md`

### 自查清单

```
自查清单：
- [ ] 所有测试通过
- [ ] 没有遗留的 TODO/FIXME
- [ ] 代码已格式化 (Shift+Alt+F)
- [ ] 没有未使用的导入
- [ ] 没有 console.log / 调试代码
- [ ] 边界条件已处理
- [ ] 错误处理完整
- [ ] 文档/注释已更新
```

### AI 代码审查

```
请审查这次变更：

1. 正确性 — 逻辑是否正确？
2. 安全性 — 有安全隐患吗？
3. 性能 — 有性能问题吗？
4. 可维护性 — 代码容易理解和修改吗？
5. 测试覆盖 — 测试足够吗？
```

### 处理审查反馈

- **Critical (必须修):** 立即修复，不能合并
- **Important (应该修):** 在合并前修复
- **Minor (建议修):** 记录为后续优化

---

## 阶段 6: 合并与清理

**引用 Skill:** `skills/finishing-work/SKILL.md`

### 合并前检查

```bash
# 1. 确保所有测试通过
npm test

# 2. 确保构建通过
npm run build

# 3. 拉取最新 main 并合并
git checkout main
git pull origin main
git checkout feature/<name>
git merge main

# 4. 解决冲突后再次测试
npm test

# 5. 合并到 main
git checkout main
git merge feature/<name>

# 6. 推送
git push origin main

# 7. 删除特性分支
git branch -d feature/<name>
git push origin --delete feature/<name>
```

### 或者: 创建 Pull Request

```bash
git push -u origin feature/<name>
# 在 GitHub/GitLab 上创建 PR
```

---

## 快速参考

| 阶段 | 关键动作 | 必须产出 |
|------|----------|----------|
| 准备 | git pull, npm test | 基线通过 |
| 分析 | 澄清需求,评估影响 | 明确的范围 |
| 分支 | 创建特性分支 | 干净的工作空间 |
| 计划 | 拆分任务,写步骤 | 计划文档 |
| 实现 | RED-GREEN-REFACTOR | 通过的测试+提交 |
| 审查 | 自查+AI审查 | 审查报告 |
| 合并 | 测试→合并→清理 | 合入main |

## 红线（绝不可以）

- **绝不** 在基线不通过时开始新工作
- **绝不** 在没有计划的情况下开始实现
- **绝不** 在测试之前写实现代码
- **绝不** 跳过代码审查直接合并
- **绝不** 在测试不通过时合并代码
- **绝不** 在一个提交里塞入太多变更
