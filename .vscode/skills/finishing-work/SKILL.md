---
name: finishing-work
description: 当实现完成、所有测试通过、需要整合工作时使用。引导完成合并/PR/清理流程。
---

# 完成工作 Skill

## 概述

引导完成开发工作的最后阶段：验证 → 选择方式 → 执行 → 清理。

## The Iron Law

```
在验证之前，不要宣称完成。
运行命令。看到输出。然后说结果。
```

## 流程

### Step 1: 验证测试

```bash
# 运行完整测试套件
npm test  # 或项目对应的测试命令

# 运行构建
npm run build  # 或项目对应的构建命令
```

**如果测试失败：** 停止。先修复。不要进入 Step 2。
**如果构建失败：** 停止。先修复。

### Step 2: 验证需求

```
需求验证：
- [ ] 重读原始需求/计划
- [ ] 逐条核对已实现的功能
- [ ] 确认不多不少（无范围蔓延）
- [ ] 确认测试覆盖了所有需求
```

### Step 3: 选择完成方式

**提供 4 个选项：**

| 选项 | 适用场景 | 操作 |
|------|----------|------|
| 1. 本地合并 | 个人项目/小团队 | merge 到 main |
| 2. 创建 PR | 团队协作 | push + 创建 PR |
| 3. 保留分支 | 还需要工作 | 不操作 |
| 4. 丢弃 | 方案不可行 | 删除分支 |

### Step 4: 执行选择

#### 选项 1: 本地合并

```bash
git checkout main
git pull origin main
git merge <feature-branch>
npm test  # 合并后再次测试
git push origin main
git branch -d <feature-branch>
```

#### 选项 2: 创建 PR

```bash
git push -u origin <feature-branch>

# PR 标题和描述
# feat: [功能简述]
#
# ## 变更内容
# - [变更 1]
# - [变更 2]
#
# ## 测试
# - [x] 单元测试通过
# - [x] 本地验证通过
#
# Closes #[issue-number]
```

#### 选项 3: 保留分支

报告当前状态:
- 已完成的任务
- 剩余的任务
- 下次继续的步骤

#### 选项 4: 丢弃（需二次确认）

```bash
# 必须明确确认才能执行
git checkout main
git branch -D <feature-branch>
```

### Step 5: 清理

```bash
# 清理已合并的分支
git branch --merged | grep -v main | xargs git branch -d

# 清理远程已删除的跟踪分支
git remote prune origin
```

## 验证前完成清单

在声称**任何**完成之前：

| 声明 | 需要的证据 |
|------|-----------|
| "测试通过了" | 测试命令输出：0 failures |
| "构建通过了" | 构建命令：exit 0 |
| "Bug 修好了" | 测试原始症状：通过 |
| "需求完成了" | 逐条核对清单 |

**绝不** 使用 "应该"、"大概"、"似乎"。

## 红线

- **绝不** 测试没过就合并
- **绝不** 没确认需求就声称完成
- **绝不** 不合并后测试就推送
- **绝不** 不确认就删除分支
