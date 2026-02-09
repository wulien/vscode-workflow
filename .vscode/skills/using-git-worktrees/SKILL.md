---
name: using-git-worktrees
description: 使用 Git worktrees 实现多分支并行工作。适合需要同时处理多个任务的场景。
---

# 使用 Git Worktrees Skill

## 概述

Git worktrees 允许你同时在多个分支工作，而不需要频繁切换或 stash。每个 worktree 是独立的工作目录，对应一个分支。

## 适用场景

- 需要同时在多个特性分支工作
- 紧急修复打断正在开发的功能
- 在一个分支跑测试，同时在另一个分支开发
- 对比不同分支的代码
- 多代理并行开发（每个 Agent 独立 worktree）

## 核心概念

```
project/                     ← 主 worktree (main 分支)
  ├── .git/
  └── src/

project-feature-login/       ← 独立 worktree (feature/login 分支)
  └── src/

project-hotfix/              ← 独立 worktree (hotfix/bug-123 分支)
  └── src/
```

**关键点：**
- 所有 worktrees 共享同一个 `.git` 目录
- 但每个 worktree 有独立的工作区
- 可以同时在不同 worktree 运行不同命令

## 基本操作

### 创建 Worktree

```bash
# 基于现有分支创建 worktree
git worktree add ../project-feature-login feature/login

# 创建新分支并关联 worktree
git worktree add -b feature/payment ../project-feature-payment

# 查看所有 worktrees
git worktree list
```

### 命名约定

```
主项目目录: project/
Worktrees:   project-<branch-name>/

示例:
project/                    ← main
project-feature-login/      ← feature/login
project-feature-payment/    ← feature/payment
project-hotfix-123/         ← hotfix/bug-123
```

### 删除 Worktree

```bash
# 1. 确保已在其他 worktree（不能在要删除的 worktree 中）
cd ../project

# 2. 删除 worktree（保留分支）
git worktree remove ../project-feature-login

# 3. 如果需要，删除分支
git branch -d feature/login

# 清理残留的 worktree 记录
git worktree prune
```

## 工作流模式

### 模式 1: 主开发 + 紧急修复

```bash
# 在 main worktree 开发新功能
cd project
git checkout -b feature/new-thing
# ... 正在开发中 ...

# 突然需要紧急修复！
cd ..
git worktree add project-hotfix hotfix/critical-bug
cd project-hotfix
# 修复 bug，测试，提交
git push origin hotfix/critical-bug

# 回到原来的工作，无需 stash
cd ../project
# 继续开发
```

### 模式 2: 多特性并行开发

```bash
# 准备工作环境
git worktree add ../project-feature-auth feature/auth
git worktree add ../project-feature-payment feature/payment
git worktree add ../project-refactor-db refactor/database

# 三个终端/VS Code 窗口分别打开
# Agent 1 在 project-feature-auth 工作
# Agent 2 在 project-feature-payment 工作
# Agent 3 在 project-refactor-db 工作

# 完成后逐个审查和合并
```

### 模式 3: 开发 + 测试分离

```bash
# 主 worktree 用于开发
cd project
# ... 修改代码 ...

# 在独立 worktree 跑测试（不影响开发）
cd ../project-testing
git pull  # 拉取最新 main
npm test  # 跑完整测试套件

# 开发不受阻塞
```

## VS Code 集成

### 打开多个 Worktree

**方式 1: 多窗口**
```
文件 → 新建窗口 → 打开文件夹 → 选择不同 worktree
```

**方式 2: 工作区**
```json
// project.code-workspace
{
  "folders": [
    { "path": "project" },
    { "path": "project-feature-login" },
    { "path": "project-feature-payment" }
  ]
}
```

### 终端组织

```
终端 1 (project):              开发服务器
终端 2 (project-feature-login): 特性开发
终端 3 (project-testing):       测试运行
```

## 注意事项

### 依赖管理

每个 worktree 有独立的 `node_modules/`:

```bash
# 在每个 worktree 中分别安装依赖
cd project-feature-login
npm install

cd ../project-feature-payment
npm install
```

**优点：** 依赖版本隔离
**缺点：** 占用更多磁盘空间

### 避免冲突

```
✅ 好的实践：
- 每个 worktree 对应不同的分支
- 不要在多个 worktree 同时修改同一个文件

❌ 坏的实践：
- 多个 worktree 指向同一个分支（会混乱）
- 忘记删除不用的 worktrees
```

### 清理策略

定期检查和清理：

```bash
# 查看所有 worktrees
git worktree list

# 删除已合并的 feature worktrees
git worktree remove ../project-feature-login
git branch -d feature/login

# 清理记录
git worktree prune
```

## 快速参考

| 命令 | 说明 |
|------|------|
| `git worktree add <path> <branch>` | 创建 worktree |
| `git worktree add -b <new-branch> <path>` | 创建新分支和 worktree |
| `git worktree list` | 列出所有 worktrees |
| `git worktree remove <path>` | 删除 worktree |
| `git worktree prune` | 清理残留记录 |

## 何时不用 Worktrees

- 简单的单任务开发
- 磁盘空间紧张
- 团队不熟悉 worktrees
- 项目依赖安装很慢（每个 worktree 都要装）

## 何时应该用 Worktrees

- 经常被紧急任务打断
- 需要并行开发多个功能
- 多代理协作开发
- 需要对比不同分支代码
- 在一个分支跑长时间测试，同时继续开发
