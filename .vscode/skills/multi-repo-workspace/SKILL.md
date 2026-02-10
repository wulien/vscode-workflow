# 多仓库工作区 Skill — Multi-Repo Workspace

> 在一个 VS Code 工作区中管理前端、后端等多个仓库，统一 AI 工作流。

## 适用场景

- 前端 + 后端分仓库，但在同一个工作区开发
- Monorepo 中有多个独立子项目
- 微服务架构，多个服务仓库
- 任何需要在一个工作区管理多个代码库的场景

## 核心设计

### VS Code 的指令加载机制

```
VS Code 加载优先级（全部合并到上下文）：

1. workspace-root/.github/copilot-instructions.md   ← 全局规则
2. workspace-root/.github/instructions/*.instructions.md  ← 按文件类型
3. workspace-root/AGENTS.md                          ← 全局 Agent 指令
4. subfolder/AGENTS.md                               ← 子目录专属指令（实验性）
5. .github/agents/*.agent.md                         ← Custom Agents
```

**关键：VS Code 会合并所有层级的指令，不是覆盖。**

### 推荐的目录结构

```
my-project/                          ← Workspace 根目录
│
├── my-project.code-workspace        ← 多根工作区文件
│
├── .github/
│   ├── copilot-instructions.md      ← 统一入口 + Memory Bank 启动协议
│   ├── agents/                      ← 共享 Agents
│   │   ├── tdd.agent.md
│   │   ├── code-review.agent.md
│   │   ├── memory.agent.md
│   │   └── ...
│   └── instructions/                ← 按文件类型的指令
│       ├── frontend.instructions.md ← applyTo: "frontend/**/*.{ts,tsx,vue}"
│       ├── backend.instructions.md  ← applyTo: "backend/**/*.{py,go,java}"
│       ├── database.instructions.md ← applyTo: "**/*.sql"
│       └── testing.instructions.md  ← applyTo: "**/*.{test,spec}.*"
│
├── .vscode/
│   ├── skills/                      ← 共享 Skills
│   └── settings.json                ← 工作区统一设置
│
├── docs/
│   └── memory/                      ← 统一 Memory Bank（项目全貌）
│       ├── PROJECT.md
│       ├── ARCHITECTURE.md
│       ├── PROGRESS.md
│       ├── DECISIONS.md
│       └── CONVENTIONS.md
│
├── frontend/                        ← Git Repo 1
│   ├── .git/
│   ├── AGENTS.md                    ← 前端专属 AI 指令
│   ├── package.json
│   └── src/
│
├── backend/                         ← Git Repo 2
│   ├── .git/
│   ├── AGENTS.md                    ← 后端专属 AI 指令
│   ├── requirements.txt / go.mod
│   └── src/
│
└── infra/                           ← Git Repo 3（可选）
    ├── .git/
    ├── AGENTS.md
    └── terraform/
```

## 设置步骤

### Step 1: 创建工作区文件

```json
// my-project.code-workspace
{
  "folders": [
    { "path": "." },           // 根目录（含统一配置）
    { "path": "frontend" },
    { "path": "backend" },
    { "path": "infra" }        // 可选
  ],
  "settings": {
    // 启用嵌套 AGENTS.md（实验性功能）
    "chat.useNestedAgentsMdFiles": true,
    "chat.useAgentsMdFile": true,
    // 启用 instructions 文件
    "github.copilot.chat.codeGeneration.useInstructionFiles": true,
    // instructions 文件搜索位置
    "chat.instructionsFilesLocations": [
      ".github/instructions"
    ]
  }
}
```

### Step 2: 创建统一 copilot-instructions.md

根目录的 `.github/copilot-instructions.md` 负责：
1. Memory Bank 启动协议（自动读取记忆）
2. 全局规则（TDD、命名规范等）
3. 多仓库上下文说明

```markdown
## 🏗️ 工作区结构

这是一个多仓库工作区：
- `frontend/` — 前端应用 [技术栈]
- `backend/` — 后端服务 [技术栈]
- `infra/` — 基础设施 [IaC 工具]

当用户操作涉及跨模块时，注意模块间的 API 契约和数据一致性。
```

### Step 3: 为每个子仓库创建 AGENTS.md

每个子仓库的 `AGENTS.md` 包含该仓库特定的 AI 指令。

**frontend/AGENTS.md 示例：**
```markdown
# Frontend Agent Instructions

## 技术栈
- Framework: React 19 + TypeScript
- 状态管理: Zustand
- 样式: Tailwind CSS
- 测试: Vitest + Testing Library

## 编码规范
- 使用函数组件 + Hooks，不用类组件
- 组件文件用 PascalCase
- 优先用 Server Components，需要交互时用 'use client'
- 所有 API 调用通过 src/api/ 目录封装

## 目录约定
- src/components/ — 可复用组件
- src/pages/ — 页面组件
- src/hooks/ — 自定义 Hooks
- src/api/ — API 请求封装
- src/types/ — TypeScript 类型定义
```

**backend/AGENTS.md 示例：**
```markdown
# Backend Agent Instructions

## 技术栈
- Language: Python 3.12
- Framework: FastAPI
- ORM: SQLAlchemy 2.0
- 测试: pytest + httpx

## 编码规范
- 使用 async/await
- API 路由按领域组织（users/, orders/）
- 所有 API 返回 Pydantic model
- 使用依赖注入获取 DB session

## 目录约定
- app/api/ — API 路由
- app/models/ — 数据库模型
- app/schemas/ — Pydantic schemas
- app/services/ — 业务逻辑
- app/core/ — 配置和安全
```

### Step 4: 创建按文件类型的 Instructions

**`.github/instructions/frontend.instructions.md`：**
```markdown
---
applyTo: "frontend/**/*.{ts,tsx}"
---
# Frontend TypeScript 规范

- 禁止使用 `any` 类型，使用 `unknown` + 类型守卫
- React 组件 props 必须定义 interface
- 事件处理函数命名: `handleXxx` (如 `handleClick`, `handleSubmit`)
- 自定义 Hook 命名: `useXxx`
- 导入顺序: React → 第三方库 → 本地模块 → 类型
```

**`.github/instructions/backend.instructions.md`：**
```markdown
---
applyTo: "backend/**/*.py"
---
# Backend Python 规范

- 使用 type hints（参数和返回值）
- 异步函数用 `async def`
- 错误处理用 HTTPException
- 数据库操作用 repository pattern
- 所有 service 函数写 docstring
```

### Step 5: 配置 VS Code 设置

```json
// .vscode/settings.json
{
  "chat.useNestedAgentsMdFiles": true,
  "chat.useAgentsMdFile": true,
  "github.copilot.chat.codeGeneration.useInstructionFiles": true,
  "chat.instructionsFilesLocations": [
    ".github/instructions"
  ]
}
```

## Memory Bank 在多仓库中的策略

### 统一记忆 vs 分散记忆

| 策略 | 适用场景 | 优缺点 |
|------|---------|--------|
| **统一记忆**（推荐） | 前后端紧密耦合 | ✅ 全貌视角 ❌ 文件较大 |
| **分散记忆** | 子项目高度独立 | ✅ 精确 ❌ 缺全貌 |
| **混合策略** | 大型项目 | ✅ 两者兼得 ❌ 维护成本 |

### 推荐：统一记忆 + 子项目标记

在统一的 `docs/memory/ARCHITECTURE.md` 中按模块描述：

```markdown
## 前端 (frontend/)

- **技术栈**: React 19 + TypeScript + Tailwind
- **入口**: frontend/src/main.tsx
- **状态管理**: Zustand
- **API 对接**: frontend/src/api/client.ts → backend API

## 后端 (backend/)

- **技术栈**: Python 3.12 + FastAPI + SQLAlchemy
- **入口**: backend/app/main.py
- **数据库**: PostgreSQL
- **API 文档**: http://localhost:8000/docs

## 前后端交互

- API Base URL: `http://localhost:8000/api/v1`
- 认证方式: JWT Bearer Token
- 共享类型定义: `docs/api-spec/openapi.yaml`
```

## 跨仓库工作流

### 场景: 添加新功能（前后端联动）

```
@Iteration 添加用户评论功能

[工作流]
1. @Writing-Plans 拆分任务:
   - Task 1: 后端 — 评论 API (CRUD)
   - Task 2: 后端 — 评论数据模型
   - Task 3: 前端 — 评论组件
   - Task 4: 前端 — API 对接
   - Task 5: 集成测试

2. 注意跨模块的 API 契约:
   - 先定义 API 接口 (OpenAPI / TypeScript interface)
   - 后端实现 API
   - 前端对接 API
   
3. 提交策略:
   - 后端独立提交到 backend/ 仓库
   - 前端独立提交到 frontend/ 仓库
   - 两边 PR 关联（描述中互相引用）
```

### 场景: API 变更（Breaking Change）

```
@Code-Review 审查 API 变更

[检查清单]
1. 后端 API 变更了什么？
2. 前端哪些调用受影响？
3. 是否需要版本号升级？
4. 迁移策略是什么？
```

## 实用命令

### Git 操作（多仓库）

```bash
# 查看所有仓库状态
git -C frontend status && git -C backend status

# 所有仓库拉取最新
git -C frontend pull && git -C backend pull

# 创建同名特性分支
git -C frontend checkout -b feature/comments
git -C backend checkout -b feature/comments
```

### VS Code Tasks（可以加到 tasks.json）

```json
{
  "label": "Start All Services",
  "dependsOn": ["Start Frontend", "Start Backend"],
  "dependsOrder": "parallel"
},
{
  "label": "Start Frontend",
  "type": "shell",
  "command": "cd frontend && npm run dev",
  "isBackground": true
},
{
  "label": "Start Backend",
  "type": "shell",
  "command": "cd backend && python -m uvicorn app.main:app --reload",
  "isBackground": true
}
```

## 故障排查

### Q: 子目录的 AGENTS.md 没有生效？

**A:** 确保启用了实验性设置：
```json
"chat.useNestedAgentsMdFiles": true
```

### Q: .instructions.md 文件没有加载？

**A:** 检查：
1. `applyTo` 的 glob 路径是否正确（相对于工作区根目录）
2. `chat.instructionsFilesLocations` 包含 `.github/instructions`
3. 使用 Copilot 诊断视图查看（右键 Chat → Diagnostics）

### Q: Memory Bank 应该放在哪个仓库？

**A:** 放在工作区根目录（不属于任何子仓库），或者创建一个专门的 `workspace-config` 仓库。

---

**参考：** VS Code Custom Instructions 文档
**集成指南：** `docs/CUSTOM-AGENTS-INTEGRATION.md`
