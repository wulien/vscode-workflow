# VS Code 1.109.0 开发最佳实践完整流程

> 基于 VS Code 1.109.0（2026年1月）多代理开发特性
> 更新日期: 2026年2月8日

---

## 📋 目录

1. [开发环境准备](#1-开发环境准备)
2. [项目启动阶段](#2-项目启动阶段)
3. [日常编码流程](#3-日常编码流程)
4. [AI 代理协作开发](#4-ai-代理协作开发)
5. [代码质量保障](#5-代码质量保障)
6. [调试与测试](#6-调试与测试)
7. [版本控制最佳实践](#7-版本控制最佳实践)
8. [性能优化](#8-性能优化)
9. [部署与发布](#9-部署与发布)
10. [持续改进](#10-持续改进)
11. [Skills 系统 — AI 开发工作流引擎](#11-skills-系统--ai-开发工作流引擎)

---

## 1. 开发环境准备

### 1.1 必装扩展

#### 核心开发扩展
- **GitHub Copilot** - AI 代码补全和聊天
- **GitLens** - Git 增强功能
- **ESLint / Prettier** - 代码规范和格式化
- **REST Client / Thunder Client** - API 测试
- **Error Lens** - 实时错误显示

#### 语言特定扩展
根据你的技术栈选择：
- **Python**: Python, Pylance, Python Debugger
- **JavaScript/TypeScript**: 内置支持 + ES7 React/Redux snippets
- **Java**: Java Extension Pack
- **Go**: Go extension
- **Rust**: rust-analyzer

#### 效率提升扩展
- **Path Intellisense** - 路径自动补全
- **Auto Rename Tag** - 自动重命名配对标签
- **Better Comments** - 增强注释可读性
- **Todo Tree** - TODO 管理
- **Bookmarks** - 代码书签

### 1.2 工作区配置

创建 `.vscode/` 目录并配置：

**settings.json** - 项目级设置
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.inlineSuggest.enabled": true,
  "editor.suggest.preview": true,
  "files.autoSave": "onFocusChange",
  "git.autofetch": true,
  "terminal.integrated.defaultProfile.windows": "Git Bash",
  "workbench.colorTheme": "GitHub Dark Default",
  "editor.minimap.enabled": true,
  "editor.rulers": [80, 120],
  "editor.linkedEditing": true
}
```

**extensions.json** - 推荐扩展
```json
{
  "recommendations": [
    "github.copilot",
    "github.copilot-chat",
    "eamodio.gitlens",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode"
  ]
}
```

**tasks.json** - 常用任务
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "安装依赖",
      "type": "shell",
      "command": "npm install",
      "problemMatcher": []
    },
    {
      "label": "运行开发服务器",
      "type": "shell",
      "command": "npm run dev",
      "isBackground": true,
      "problemMatcher": {
        "pattern": {
          "regexp": "."
        },
        "background": {
          "activeOnStart": true,
          "beginsPattern": ".",
          "endsPattern": "."
        }
      }
    }
  ]
}
```

### 1.3 快捷键配置

掌握核心快捷键（Windows）：

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 命令面板 | `Ctrl+Shift+P` | 访问所有命令 |
| 快速打开文件 | `Ctrl+P` | 快速文件导航 |
| Copilot Chat | `Ctrl+I` | 打开内联聊天 |
| 侧边栏聊天 | `Ctrl+Alt+I` | 打开聊天面板 |
| 多光标 | `Alt+点击` | 多处编辑 |
| 代码补全 | `Ctrl+Space` | 触发智能提示 |
| 跳转定义 | `F12` | 查看定义 |
| 查找引用 | `Shift+F12` | 查找所有引用 |
| 重命名符号 | `F2` | 智能重命名 |
| 格式化文档 | `Shift+Alt+F` | 格式化代码 |
| 切换终端 | `` Ctrl+` `` | 显示/隐藏终端 |

---

## 2. 项目启动阶段

### 2.1 使用 AI 代理初始化项目

**步骤 1: 与 Copilot 对话设计架构**

```
提示词示例：
"我要创建一个 [项目类型] 项目，主要功能包括 [功能列表]。
请帮我：
1. 推荐技术栈
2. 设计项目结构
3. 初始化项目配置"
```

**步骤 2: 让代理生成项目脚手架**

```
提示词示例：
"基于上述架构，请创建：
1. 完整的目录结构
2. 配置文件（package.json, tsconfig.json 等）
3. 基础代码框架
4. README 文档"
```

**步骤 3: 设置开发环境**

```bash
# 在终端中执行（使用沙箱执行特性）
git init
npm install
git add .
git commit -m "Initial commit"
```

### 2.2 配置代理自定义工作流

利用 **Agent Orchestration** 创建自定义工作流：

**workflow.json** 示例：
```json
{
  "workflows": [
    {
      "name": "新功能开发",
      "steps": [
        "创建功能分支",
        "生成测试用例",
        "实现功能代码",
        "运行测试",
        "代码审查",
        "合并到主分支"
      ]
    },
    {
      "name": "Bug 修复",
      "steps": [
        "重现问题",
        "定位根因",
        "编写测试用例",
        "修复代码",
        "验证修复",
        "提交 PR"
      ]
    }
  ]
}
```

### 2.3 初始化知识库

创建 `.vscode/copilot-knowledge/` 目录：

**project-context.md**
```markdown
# 项目上下文

## 项目概述
[项目简介]

## 技术栈
- 前端: [框架]
- 后端: [框架]
- 数据库: [数据库]

## 代码规范
- 命名约定: camelCase for variables, PascalCase for classes
- 文件组织: feature-based modules
- 注释要求: JSDoc for all public APIs

## 常用模式
[项目特定的设计模式和最佳实践]
```

---

## 3. 日常编码流程

### 3.1 开始工作日

**Morning Routine Checklist:**

```bash
# 1. 拉取最新代码
git pull origin main

# 2. 检查依赖更新
npm outdated

# 3. 运行测试确保环境正常
npm test

# 4. 查看今日任务
# 使用 TODO Tree 扩展或项目管理工具
```

**利用 Copilot Memory:**
- Copilot 会记住你的编码习惯和项目上下文
- 在聊天中回顾昨天的进度："总结昨天的工作内容和今天的计划"

### 3.2 功能开发工作流

#### 阶段 1: 需求分析（与 AI 协作）

```
提示词模板：
"我要实现 [功能描述]。
请帮我：
1. 分析需求和边界条件
2. 识别可能的技术挑战
3. 建议实现方案（至少2个）
4. 评估各方案的优缺点"
```

#### 阶段 2: 设计接口和类型

```typescript
// 让 Copilot 帮助设计类型定义
// 提示：先写注释描述需求，然后让 Copilot 生成

/**
 * 用户服务接口
 * 负责用户的 CRUD 操作和认证
 */
interface IUserService {
  // Copilot 会自动建议方法...
}
```

#### 阶段 3: TDD（测试驱动开发）

```javascript
// 1. 先写测试（使用内联聊天 Ctrl+I）
describe('UserService', () => {
  it('should create a new user', async () => {
    // 让 Copilot 生成测试用例
  });
});

// 2. 运行测试（应该失败）
// 3. 实现功能代码
// 4. 运行测试（应该通过）
```

#### 阶段 4: 实现功能

**最佳实践：**
1. **小步提交** - 每完成一个小功能就提交
2. **使用内联聊天** - 对复杂逻辑使用 `Ctrl+I` 请求帮助
3. **代码片段** - 利用自定义 snippets 加速开发
4. **实时重构** - 发现代码异味立即重构

**编码中的 AI 协作技巧：**

```
场景 1: 不确定如何实现
→ 选中相关代码 → Ctrl+I → "如何优化这段代码的性能？"

场景 2: 需要生成样板代码
→ 写注释描述需求 → Tab 接受 Copilot 建议

场景 3: 理解陌生代码
→ 选中代码 → 右键 → "Copilot: Explain This"

场景 4: 发现 Bug
→ 选中错误代码 → Ctrl+I → "这段代码有什么问题？如何修复？"
```

### 3.3 代码审查前自查

使用 AI 进行自我审查：

```
提示词：
"请审查以下代码：
1. ✅ 正确性
2. ✅ 性能
3. ✅ 安全性
4. ✅ 可读性
5. ✅ 是否符合项目规范

[粘贴代码]"
```

---

## 4. AI 代理协作开发

### 4.1 代理会话管理策略

**本地代理（Local Agent）** - 快速响应
- 代码补全
- 简单重构
- 格式化和修复

**云端代理（Cloud Agent）** - 复杂任务
- 架构设计
- 复杂算法实现
- 全面代码审查

**后台代理（Background Agent）** - 长时任务
- 大规模重构
- 批量代码迁移
- 依赖升级

### 4.2 多代理协作示例

**场景：重构一个大型模块**

```
1. 主对话（Cloud Agent）：
   "分析 UserModule 的重构方案"
   
2. 委托给专门代理：
   "将数据库迁移任务委托给后台代理"
   
3. 同时进行：
   - 本地代理：处理简单的变量重命名
   - 云端代理：设计新的架构
   - 后台代理：执行数据迁移脚本
   
4. 汇总结果：
   "总结所有代理的工作结果和下一步行动"
```

### 4.3 Agent Skills 自定义

创建团队专用技能：

```json
{
  "agentSkills": [
    {
      "name": "generate-api-documentation",
      "description": "根据代码生成 OpenAPI 文档",
      "trigger": "@doc-api"
    },
    {
      "name": "security-scan",
      "description": "扫描代码中的安全漏洞",
      "trigger": "@security"
    },
    {
      "name": "performance-analysis",
      "description": "分析代码性能瓶颈",
      "trigger": "@perf"
    }
  ]
}
```

使用方式：
```
在聊天中输入: "@doc-api 为当前 API 生成文档"
```

### 4.4 Copilot Memory 最大化利用

**训练你的 AI 助手：**

```
会话开始时：
"记住：在这个项目中，我们使用 Repository Pattern，
所有数据库操作都通过 Repository 类进行。"

后续开发中：
Copilot 会自动按照 Repository Pattern 生成代码
```

**定期同步上下文：**
```
每周一次：
"总结本周的关键技术决策和新增的代码模式"
```

---

## 5. 代码质量保障

### 5.1 实时代码质量检查

**配置 Error Lens 扩展**
- 实时在代码行旁显示错误和警告
- 配合 ESLint/TSLint 使用

**使用 SonarLint**
- 检测代码异味
- 安全漏洞扫描
- 认知复杂度警告

### 5.2 自动化工具链

**Pre-commit Hooks (Husky)**
```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "pre-push": "npm test"
    }
  },
  "lint-staged": {
    "*.{js,ts}": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ]
  }
}
```

### 5.3 代码审查清单

**使用 AI 生成审查报告：**

```
提示词：
"对以下 PR 进行全面审查，生成审查报告：
- 🔍 代码质量
- 🛡️ 安全问题
- ⚡ 性能影响
- 📚 文档完整性
- ✅ 测试覆盖率

[PR diff]"
```

### 5.4 技术债务管理

**使用 TODO Tree 跟踪：**
```javascript
// TODO: 优化这个函数的性能
// FIXME: 处理边界情况
// HACK: 临时解决方案，需要重构
// XXX: 危险代码，需要立即修复
```

**定期 AI 审计：**
```
月度提示：
"扫描项目中的所有 TODO/FIXME 注释，
按优先级分类并建议处理顺序"
```

---

## 6. 调试与测试

### 6.1 智能断点和调试

**配置 launch.json**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "调试当前文件",
      "program": "${file}",
      "skipFiles": ["<node_internals>/**"],
      "console": "integratedTerminal"
    },
    {
      "type": "chrome",
      "request": "launch",
      "name": "调试 Chrome",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

**AI 辅助调试技巧：**

```
场景 1: 出现运行时错误
→ 复制错误堆栈 → 粘贴到 Copilot Chat
→ "分析这个错误并建议修复方案"

场景 2: 逻辑错误
→ 选中可疑代码 → Ctrl+I
→ "追踪这个函数的执行流程，找出可能的问题"

场景 3: 性能问题
→ "分析这段代码的时间复杂度，建议优化方案"
```

### 6.2 测试策略

**测试金字塔：**
- 单元测试 (70%): 快速，隔离
- 集成测试 (20%): 组件交互
- E2E 测试 (10%): 完整流程

**AI 生成测试用例：**

```javascript
// 1. 写函数
function calculateDiscount(price, customerType) {
  // 实现...
}

// 2. 请求 Copilot 生成测试
// Prompt: "为这个函数生成全面的测试用例，包括边界条件"

// 3. Copilot 生成的测试：
describe('calculateDiscount', () => {
  it('should apply 10% discount for regular customers', () => {
    expect(calculateDiscount(100, 'regular')).toBe(90);
  });
  
  it('should apply 20% discount for VIP customers', () => {
    expect(calculateDiscount(100, 'vip')).toBe(80);
  });
  
  it('should handle zero price', () => {
    expect(calculateDiscount(0, 'regular')).toBe(0);
  });
  
  it('should throw error for negative price', () => {
    expect(() => calculateDiscount(-10, 'regular')).toThrow();
  });
});
```

### 6.3 集成浏览器测试

**使用 VS Code 内置浏览器：**

1. 安装 "Live Preview" 扩展
2. 右键 HTML 文件 → "Show Preview"
3. 在编辑器内直接测试应用
4. 支持实时刷新和调试

**优势：**
- 无需切换窗口
- 内置开发者工具
- 支持多设备模拟

---

## 7. 版本控制最佳实践

### 7.1 Git 工作流

**分支策略：**
```
main (生产环境)
  ↑
develop (开发环境)
  ↑
feature/xxx (功能分支)
hotfix/xxx (紧急修复)
```

**使用 GitLens 增强功能：**
- File History: 查看文件完整历史
- Line Blame: 每行代码的作者和时间
- Compare: 对比不同版本
- Git Graph: 可视化分支历史

### 7.2 智能提交信息

**使用 AI 生成提交信息：**

```
1. 暂存更改: git add .
2. 在 Source Control 面板点击 ✨ 图标
3. Copilot 自动分析更改并生成提交信息
4. 格式遵循约定：
   feat: 新功能
   fix: 修复 bug
   docs: 文档更新
   style: 代码格式
   refactor: 重构
   test: 测试相关
   chore: 构建/工具相关
```

**示例提交信息：**
```
feat(auth): 添加 OAuth2 登录支持

- 实现 Google 和 GitHub 登录
- 添加 JWT token 管理
- 更新用户模型以支持外部认证

Closes #123
```

### 7.3 代码审查流程

**Pull Request 模板：**

```markdown
## 📝 更改描述
[描述本 PR 的主要更改]

## 🎯 相关 Issue
Closes #[issue number]

## ✅ 自查清单
- [ ] 代码已格式化
- [ ] 所有测试通过
- [ ] 添加了必要的测试
- [ ] 更新了文档
- [ ] 无安全漏洞
- [ ] 已通过 AI 代码审查

## 🤖 AI 审查结果
[粘贴 Copilot 的审查报告]

## 📸 截图（如适用）
[添加截图]

## 🔄 测试步骤
1. ...
2. ...
```

---

## 8. 性能优化

### 8.1 编辑器性能优化

**大型项目配置：**
```json
{
  "files.exclude": {
    "**/node_modules": true,
    "**/.git": true,
    "**/dist": true,
    "**/build": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true
  },
  "files.watcherExclude": {
    "**/node_modules/**": true
  },
  "typescript.tsserver.maxTsServerMemory": 4096
}
```

### 8.2 代码性能分析

**使用 AI 进行性能分析：**

```
提示词模板：
"分析以下代码的性能：
1. 时间复杂度
2. 空间复杂度
3. 潜在的性能瓶颈
4. 优化建议（至少3个）

[粘贴代码]"
```

### 8.3 外部索引支持

**配置代码搜索索引：**
```json
{
  "search.useIndexing": true,
  "search.maintainFileSearchCache": true,
  "search.collapseResults": "auto"
}
```

**优势：**
- 大型代码库搜索更快
- 支持语义搜索
- 智能结果排序

---

## 9. 部署与发布

### 9.1 CI/CD 集成

**使用 GitHub Actions（示例）：**

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test
      - run: npm run build

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Production
        run: |
          # 部署脚本
```

### 9.2 部署前检查清单

**让 AI 生成检查清单：**

```
提示词：
"为即将发布的版本生成部署检查清单，包括：
- 代码质量检查
- 测试覆盖率
- 性能基准测试
- 安全扫描
- 文档更新
- 回滚计划"
```

### 9.3 使用终端沙箱执行

**安全执行部署命令：**

VS Code 1.109.0 的终端沙箱功能：
- 自动检测危险命令
- 需要确认才能执行
- 提供命令解释

```bash
# VS Code 会弹出确认框
npm run deploy:production

# 显示命令将要做什么
# 请求你的批准
```

---

## 10. 持续改进

### 10.1 每周回顾

**本周总结模板（与 AI 协作）：**

```
提示词：
"分析本周的 Git 提交历史和代码变更，生成周报：

1. 📊 本周统计
   - 提交次数
   - 代码增删行数
   - 完成的功能

2. 🎯 关键成就
   - 主要功能
   - 解决的难题

3. 📚 学到的经验
   - 新技术/工具
   - 最佳实践

4. 🔄 改进建议
   - 下周优化点
   - 技术债务处理"
```

### 10.2 技能提升

**个人成长路线图：**

```markdown
## 短期目标（1-3个月）
- [ ] 掌握 [新技术/框架]
- [ ] 提高代码质量评分到 A
- [ ] 完成 [项目里程碑]

## 中期目标（3-6个月）
- [ ] 成为 [领域] 专家
- [ ] 贡献开源项目
- [ ] 学习架构设计

## 长期目标（6-12个月）
- [ ] 技术领导力
- [ ] 系统架构能力
- [ ] 团队指导能力
```

### 10.3 团队知识分享

**创建团队知识库：**

```
.vscode/team-knowledge/
  ├── architecture/
  │   ├── system-design.md
  │   └── tech-decisions.md
  ├── guidelines/
  │   ├── coding-standards.md
  │   └── review-checklist.md
  └── troubleshooting/
      └── common-issues.md
```

### 10.4 自动化工作流优化

**定期评估和改进：**

```
月度提示：
"分析我最常用的 VS Code 命令和操作，
建议可以自动化或优化的工作流"
```

---

## 🎯 快速参考卡

### 日常开发核心流程

```
1. 晨间启动
   → git pull → npm install → npm test

2. 功能开发
   → 与 AI 讨论设计 → 写测试 → 实现代码 → 自测

3. 代码提交
   → git add → AI 生成提交信息 → git push

4. 代码审查
   → 创建 PR → AI 审查 → 人工审查 → 合并

5. 晚间收尾
   → 提交所有代码 → 更新 TODO → 记录明日计划
```

### AI 协作关键提示词

| 场景 | 提示词模板 |
|------|-----------|
| 需求分析 | "分析 [需求]，建议 [数量] 个实现方案" |
| 代码生成 | "实现 [功能]，遵循 [规范]" |
| Bug 修复 | "这段代码有什么问题？[错误信息]" |
| 代码审查 | "审查这段代码的 [质量/安全/性能]" |
| 性能优化 | "分析性能瓶颈并建议优化方案" |
| 测试生成 | "为 [函数] 生成全面的测试用例" |
| 文档生成 | "为 [代码] 生成详细文档" |
| 重构建议 | "如何重构这段代码使其更 [简洁/高效/可读]" |

### 效率倍增快捷键

```
Ctrl+P          → 快速打开文件
Ctrl+Shift+P    → 命令面板
Ctrl+I          → 内联 AI 聊天
Ctrl+`          → 切换终端
Alt+↑/↓         → 移动行
Shift+Alt+↓     → 复制行
Ctrl+/          → 切换注释
Ctrl+D          → 选择下一个匹配项
F2              → 重命名符号
F12             → 跳转到定义
Ctrl+Space      → 触发建议
```

---

## 📌 进阶技巧

### 1. 工作区状态管理

使用 **Profiles** 功能为不同项目类型创建配置：
- Frontend Profile: React/Vue 相关扩展
- Backend Profile: Node/Python 相关扩展
- DevOps Profile: Docker/K8s 工具

### 2. 远程开发

利用 Remote Development：
- Remote - SSH: 连接远程服务器
- Remote - Containers: 容器内开发
- Remote - WSL: Windows 子系统开发

### 3. 多项目管理

使用 **Multi-root Workspaces**：
```json
{
  "folders": [
    { "path": "./frontend" },
    { "path": "./backend" },
    { "path": "./shared" }
  ]
}
```

### 4. 代码片段库

创建团队共享的 snippets：
```json
{
  "React Function Component": {
    "prefix": "rfc",
    "body": [
      "import React from 'react';",
      "",
      "interface ${1:Component}Props {",
      "  $2",
      "}",
      "",
      "export const ${1:Component}: React.FC<${1:Component}Props> = (props) => {",
      "  return (",
      "    <div>",
      "      $0",
      "    </div>",
      "  );",
      "};",
      ""
    ]
  }
}
```

---

## 🔒 安全最佳实践

### 1. 敏感信息保护

```json
// .gitignore
.env
.env.local
*.key
*.pem
secrets/

// settings.json
{
  "files.exclude": {
    "**/*.env": true
  }
}
```

### 2. 依赖安全扫描

```bash
# 定期运行
npm audit
npm audit fix
```

### 3. 代码签名

确保 Git 提交签名：
```bash
git config --global commit.gpgsign true
```

---

## 11. Skills 系统 — AI 开发工作流引擎

> 灵感来源: [obra/superpowers](https://github.com/obra/superpowers) — 面向 AI 编码代理的可组合技能框架
> Skills 位于 `.vscode/skills/` 目录，Copilot 会在每次会话中自动加载

### 11.1 架构概览

```
.github/
  copilot-instructions.md      ← 会话引导（自动加载）

.vscode/
  skills/
    new-project/SKILL.md       ← 启动新项目
    iteration/SKILL.md         ← 迭代需求开发
    brainstorming/SKILL.md     ← 头脑风暴/设计探索
    writing-plans/SKILL.md     ← 编写实现计划
    test-driven-development/SKILL.md  ← TDD 流程
    systematic-debugging/SKILL.md     ← 系统化调试
    code-review/SKILL.md       ← 代码审查
    multi-agent-collaboration/SKILL.md ← 多代理协作
    finishing-work/SKILL.md    ← 完成工作流程
  tasks.json                   ← 自动化任务
  settings.json                ← 项目设置
  extensions.json              ← 推荐扩展
```

### 11.2 核心原则

| 原则 | 含义 |
|------|------|
| **TDD** | 在写实现代码之前写测试，永远如此 |
| **系统化** | 流程优先于猜测 |
| **降低复杂度** | 简洁是首要目标 |
| **证据优先** | 运行命令、看到输出，然后说结果 |

### 11.3 两大入口工作流

#### 工作流 A: 启动新项目

```
brainstorming → 架构设计 → writing-plans → 搭建工程 → 开始开发
```

**使用方式：** 告诉 Copilot "按照 new-project Skill 启动项目"

1. **Brainstorming** — Copilot 主动研究、提出方案、苏格拉底式提问
2. **Architecture** — 生成设计文档保存到 `docs/plans/`
3. **Writing Plans** — 将设计拆分为可执行的 TDD 实现任务
4. **Scaffolding** — 生成项目脚手架、配置文件
5. **Development** — 按计划逐任务执行，每步验证

#### 工作流 B: 迭代需求

```
需求分析 → 创建分支 → writing-plans → TDD 实现 → 代码审查 → 合并发布
```

**使用方式：** 告诉 Copilot "按照 iteration Skill 开始这个需求"

1. **Preparation** — 拉取最新代码、确认环境
2. **Analysis** — 分析需求、识别影响范围
3. **Branch** — 创建特性分支
4. **Plan** — 生成实现计划
5. **TDD** — RED → GREEN → REFACTOR → COMMIT
6. **Review & Merge** — 代码审查 → 合并 → 清理

### 11.4 共用 Skills 速查

| Skill | 触发场景 | 核心内容 |
|-------|----------|----------|
| `brainstorming` | 新功能设计 | 5 阶段苏格拉底式探索 |
| `writing-plans` | 设计确认后 | 精确到文件路径的 TDD 实现任务 |
| `test-driven-development` | 写任何代码时 | RED-GREEN-REFACTOR-COMMIT |
| `systematic-debugging` | 遇到 Bug 时 | 4 阶段根因分析 |
| `code-review` | 完成任务后 | 多维度系统化审查 |
| `multi-agent-collaboration` | 大任务分解 | 并行分派/子代理驱动 |
| `finishing-work` | 准备合并时 | 验证-选择-执行-清理 |

### 11.5 多代理协作模式

VS Code 1.109.0 支持多代理并行工作：

| 模式 | 用途 | 操作 |
|------|------|------|
| **并行分派** | 多个独立模块 | 多个 Agent 同时实现不同模块 |
| **子代理驱动** | 按计划执行 | 控制器分派实现/审查代理 |
| **设计-实现分离** | 复杂功能 | 设计代理 → 确认 → 实现代理 |

### 11.6 计划文档管理

所有设计和计划保存到版本控制：

```
docs/plans/
  2026-02-08-user-auth.md              ← 设计文档
  2026-02-08-user-auth-implementation.md ← 实现计划
```

### 11.7 如何使用

**复制到你的项目中：**

```bash
# 复制整个 Skills 系统到你的项目
cp -r .vscode/skills/ <你的项目>/.vscode/skills/
cp .github/copilot-instructions.md <你的项目>/.github/copilot-instructions.md
```

**Copilot 会自动：**
- 读取 `.github/copilot-instructions.md` 了解项目规则
- 引用 `.vscode/skills/` 中的 Skill 指导工作流
- 用 TDD 方式写代码
- 在声称完成前用证据验证

---

## 📖 学习资源

- **官方文档**: https://code.visualstudio.com/docs
- **Copilot 文档**: https://docs.github.com/copilot
- **快捷键速查**: `Ctrl+K Ctrl+S` 打开键盘快捷方式
- **交互式教程**: `Ctrl+Shift+P` → "Help: Interactive Playground"

---

## 🎉 结语

这套最佳实践流程结合了 VS Code 1.109.0 的最新 AI 代理特性，能够帮助你：

✅ **提升效率** - 通过 AI 协作减少 50% 的重复性工作  
✅ **保证质量** - 自动化检查确保代码高质量  
✅ **快速学习** - AI 助手加速新技术掌握  
✅ **团队协作** - 标准化流程提升团队效率  
✅ **持续改进** - 定期回顾和优化工作流程  

**记住：工具只是辅助，关键在于持续实践和改进！**

---

**版本历史**
- v2.0 (2026-02-08): 新增 Skills 系统，整合 obra/superpowers 工作流框架
- v1.0 (2026-02-08): 基于 VS Code 1.109.0 初始版本

**贡献**
欢迎根据实际使用经验持续完善这份文档！
