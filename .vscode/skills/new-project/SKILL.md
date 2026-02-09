---
name: new-project-initialization
description: 启动新项目时使用。引导完成从需求分析、架构设计、工程搭建到开发规划的全部准备工作。
---

# 新项目初始化 Skill

## 概述

新项目从零到可开发状态的完整流程。不要跳过任何阶段，即使"看起来很简单"。

## 触发条件

- 创建一个全新的项目/产品/服务
- 从零开始搭建工程
- "帮我建一个 XXX"

## 完整流程

```
阶段 1: 需求探索 (Brainstorming)
    ↓
阶段 2: 架构设计 (Architecture Design)
    ↓
阶段 3: 技术选型 (Tech Stack Decision)
    ↓
阶段 4: 工程搭建 (Project Scaffolding)
    ↓
阶段 5: 开发规划 (Implementation Planning)
    ↓
阶段 6: 开发启动 (Development Kickoff)
```

---

## 阶段 1: 需求探索 (Brainstorming)

**引用 Skill:** `skills/brainstorming/SKILL.md`

**核心原则：** 在写任何代码之前，先搞清楚要做什么。

### 步骤

1. **自主调研** — 在提问之前，先研究项目上下文
   - 查看已有的文档、README、相关代码
   - 理解问题域和用户需求

2. **苏格拉底式提问** — 通过问题引导而非直接给方案
   - "这个功能要解决什么问题？"
   - "预期用户是谁？使用场景是什么？"
   - "有哪些约束条件（时间、技术、资源）？"

3. **分段验证设计** — 将设计分成可消化的小块展示
   - 每部分不超过一页
   - 等用户确认后再继续

4. **保存设计文档**
   ```
   docs/plans/YYYY-MM-DD-<project-name>-design.md
   ```

### 设计文档模板

```markdown
# [项目名] 设计文档

## 问题定义
[要解决什么问题]

## 用户与场景
[谁会用？怎么用？]

## 核心功能
[功能清单，按优先级排序]

## 非功能需求
[性能、安全性、可维护性等]

## 约束与假设
[已知的限制条件]

## 风险评估
[主要风险和应对策略]
```

---

## 阶段 2: 架构设计

**在设计被确认后执行。**

### 步骤

1. **绘制系统架构图**
   - 使用 Mermaid 语法（VS Code 原生支持预览）
   - 包含核心模块和数据流

2. **定义目录结构**
   ```
   project-root/
   ├── src/             # 源代码
   ├── tests/           # 测试
   ├── docs/            # 文档
   │   └── plans/       # 设计和计划
   ├── .vscode/         # VS Code 配置
   │   ├── skills/      # 开发技能库
   │   ├── tasks.json   # 任务配置
   │   ├── launch.json  # 调试配置
   │   └── settings.json
   ├── .github/
   │   ├── copilot-instructions.md
   │   └── workflows/   # CI/CD
   └── README.md
   ```

3. **定义核心接口/类型**
   - 先定义接口，再实现
   - 类型即文档

4. **评审架构**
   - 展示给用户确认
   - 记录技术决策理由 (ADR)

---

## 阶段 3: 技术选型

### 决策模板

| 维度 | 选项 A | 选项 B | 选项 C |
|------|--------|--------|--------|
| 技术方案 | | | |
| 优点 | | | |
| 缺点 | | | |
| 学习成本 | | | |
| 社区成熟度 | | | |
| 长期维护 | | | |
| **推荐** | | | |

### 产出

- 确定的技术栈清单
- 每个选择的理由
- 记录到设计文档的 "技术选型" 章节

---

## 阶段 4: 工程搭建

### 步骤检查清单

```
工程搭建进度：
- [ ] 1. 初始化项目 (git init, package.json 等)
- [ ] 2. 安装核心依赖
- [ ] 3. 配置开发工具链
- [ ] 4. 搭建 VS Code 环境 (tasks.json, launch.json, settings.json)
- [ ] 5. 配置 Git (hooks, .gitignore)
- [ ] 6. 配置 CI/CD 基础
- [ ] 7. 创建 README.md
- [ ] 8. 复制 Skills 系统到项目
- [ ] 9. 创建初始提交
- [ ] 10. 验证环境可运行
```

### VS Code 环境配置

**必须创建的文件：**

1. `.vscode/settings.json` — 项目级设置
2. `.vscode/tasks.json` — 常用任务（构建、测试、运行）
3. `.vscode/launch.json` — 调试配置
4. `.vscode/extensions.json` — 推荐扩展
5. `.github/copilot-instructions.md` — Copilot 上下文

**验证工程搭建：**
```bash
# 必须全部通过才算完成
npm test    # (或对应的测试命令)
npm run build   # (或对应的构建命令)
npm run dev     # (或对应的开发命令)
```

---

## 阶段 5: 开发规划

**引用 Skill:** `skills/writing-plans/SKILL.md`

### 步骤

1. **将功能拆分为里程碑**
2. **每个里程碑拆分为任务**
3. **每个任务拆分为 2-5 分钟的步骤**
4. **保存计划文档**
   ```
   docs/plans/YYYY-MM-DD-<project-name>-implementation.md
   ```

---

## 阶段 6: 开发启动

### 启动检查清单

```
开发启动检查：
- [ ] 设计文档已审批
- [ ] 实现计划已完成
- [ ] 工程环境可运行
- [ ] 测试框架可执行
- [ ] Git 分支已创建
- [ ] 第一个任务已明确
```

### 开始第一个任务

切换到 **迭代需求 Skill** (`skills/iteration/SKILL.md`) 开始执行计划中的第一个任务。

---

## 红线（绝不可以）

- **绝不** 跳过需求探索直接开始码代码
- **绝不** 没有设计文档就开始搭建工程
- **绝不** 没有验证环境就开始开发
- **绝不** 没有计划就开始实现功能
- **绝不** 把所有功能放在一个大任务里
