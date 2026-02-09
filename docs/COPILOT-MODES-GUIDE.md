# GitHub Copilot 三种模式高效使用指南

## 模式详解

### 🖥️ Local Mode（本地模式）

**底层技术：**
- 本地小型语言模型（约 1-7B 参数）
- VS Code 自带，不需联网
- 延迟 < 100ms

**最佳使用场景：**

1. **代码补全（最常用）**
   ```typescript
   // 你输入: function calculate
   // Local 立即建议:
   function calculateDiscount(amount: number, percentage: number): number {
     return amount * (1 - percentage / 100);
   }
   ```

2. **简单重构**
   - 选中代码 → `Ctrl+I` → "提取为函数"
   - Local 快速完成

3. **代码片段**
   - 输入注释，Local 生成代码
   ```typescript
   // 验证邮箱格式
   const isValidEmail = (email: string) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
   ```

**优势：**
- ⚡ 极快响应
- 🔒 数据不离开本地
- 📡 离线可用

**局限：**
- 无法理解复杂业务逻辑
- 不能访问 @workspace 上下文
- 只能处理当前文件附近代码

---

### ⚙️ Background Mode（后台模式）

**底层技术：**
- 云端模型异步执行
- 任务队列管理
- 完成后推送通知

**最佳使用场景：**

1. **大规模代码迁移**
   ```
   你: "后台将所有 .js 文件迁移到 TypeScript"
   
   Background Agent:
   - 分析 50 个文件
   - 逐个添加类型
   - 完成后通知你
   
   你: 继续写其他代码（不被阻塞）
   ```

2. **批量测试生成**
   ```
   你: "在后台为 src/ 下所有组件生成单元测试"
   
   Background: 20 分钟后完成，生成 30 个测试文件
   ```

3. **依赖分析**
   ```
   你: "后台分析项目中未使用的依赖"
   
   Background: 扫描 node_modules，生成报告
   ```

**使用技巧：**

在聊天中明确说"后台"：
```
❌ "帮我重构所有组件" → 阻塞式
✅ "在后台重构所有组件" → 异步执行
```

或使用 @cli 模式：
```bash
# 会自动在后台运行
@cli npm audit fix
```

---

### ☁️ Cloud Mode（云端模式）

**底层技术：**
- Claude Sonnet 4.5 / GPT-4 级别模型
- 上下文窗口 200k+ tokens
- 深度推理能力

**最佳使用场景：**

1. **使用 Skills 系统（强烈推荐）**
   ```
   你: "按照 new-project Skill 启动一个电商项目"
   
   Cloud Agent:
   1. 调用 brainstorming Skill → 探索需求
   2. 生成架构设计文档
   3. 调用 writing-plans Skill → 实现计划
   4. 调用 test-driven-development → 逐任务实现
   ```

2. **复杂问题调试**
   ```
   你: "用户登录后购物车数据丢失，帮我调试"
   
   Cloud:
   1. 使用 systematic-debugging Skill
   2. 根因调查 → Session 管理问题
   3. 模式分析 → 对比正常流程
   4. 提供修复方案 + 测试
   ```

3. **架构设计讨论**
   ```
   你: "设计一个高并发秒杀系统"
   
   Cloud:
   - 分析需求（QPS, 库存扣减）
   - 提出多种方案（Redis 锁 vs MQ vs 分布式锁）
   - 权衡优劣
   - 生成技术选型文档
   ```

4. **学习新技术**
   ```
   你: "解释 React Server Components 的工作原理"
   
   Cloud: 深度讲解 + 代码示例
   ```

**独有能力：**
- ✅ 理解整个 @workspace
- ✅ 多轮对话上下文
- ✅ 调用 Skills 系统
- ✅ 生成完整功能（不是片段）
- ✅ 代码审查和重构建议

---

## 🚀 实战工作流

### 工作流 1: 开发新功能（推荐）

```
阶段 1: 规划（Cloud）
  → Ctrl+Alt+I 打开 Cloud 聊天
  → "按照 iteration Skill 开发用户登录功能"
  → Cloud 生成实现计划

阶段 2: 编码（Cloud + Local 协同）
  → Cloud 在聊天中指导每个任务
  → 你在编辑器写代码
  → Local 提供实时补全
  
阶段 3: 测试（Cloud）
  → Cloud 帮你写测试
  → Local 补全断言代码

阶段 4: 审查（Cloud）
  → "使用 code-review Skill 审查代码"
  → Cloud 深度分析
```

### 工作流 2: 紧急修 Bug

```
步骤 1: 快速定位（Cloud）
  → "这段代码报错了，帮我看看"
  → Cloud 分析错误
  
步骤 2: 修复（Local）
  → Cloud 建议修复方案
  → 你手动修改，Local 补全
  
步骤 3: 验证（Cloud）
  → Cloud 帮你写测试验证修复
```

### 工作流 3: 大规模重构

```
规划（Cloud）:
  → "我要重构用户模块，使用 writing-plans Skill"
  → Cloud 生成详细计划

执行（Background）:
  → "在后台执行重构计划的前 5 个任务"
  → Background 异步执行
  
验证（Cloud）:
  → Background 完成后，Cloud 审查结果
  → 运行测试确认
```

### 工作流 4: 多模块并行开发

```
控制器（Cloud - 主聊天）:
  → "使用 multi-agent-collaboration 并行开发 3 个模块"
  
Agent 1（Cloud - Chat 1）:
  → "实现用户模块"
  
Agent 2（Cloud - Chat 2）:
  → "实现订单模块"
  
Agent 3（Background）:
  → "后台生成所有模块的测试"
  
编码（Local）:
  → 在每个模块文件编码时，Local 提供补全
```

---

## 📊 性能对比

| 维度 | Local | Background | Cloud |
|------|-------|------------|-------|
| **响应速度** | < 100ms | 不阻塞 | 1-5s |
| **推理能力** | ⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **上下文理解** | 当前文件 | @workspace | @workspace + 多轮对话 |
| **适合任务长度** | < 1 分钟 | > 5 分钟 | 1-30 分钟 |
| **离线可用** | ✅ | ❌ | ❌ |
| **Skills 支持** | ❌ | 部分 | ✅ 完整 |
| **多文件编辑** | ❌ | ✅ | ✅ |
| **成本** | 免费（本地） | 正常 | 正常 |

---

## 🎯 最佳实践建议

### DO ✅

1. **默认使用 Cloud 聊天**
   - 打开 VS Code → `Ctrl+Alt+I` → 确保选择 @cloud
   - 这是最强大的模式

2. **让 Local 处理补全**
   - 不要关闭 Local 补全
   - 编码时让它自动提示

3. **长任务用 Background**
   - 批量操作明确说"后台"
   - 继续其他工作

4. **组合使用**
   ```
   Cloud: 规划和指导
   Local: 快速补全
   Background: 批量任务
   ```

5. **利用 Skills 系统**
   - 所有复杂工作流都通过 Cloud + Skills
   - "按照 XXX Skill" 开始工作

### DON'T ❌

1. **不要用 Local 做复杂推理**
   ```
   ❌ 在代码里注释: "// 设计一个分布式锁"
   ✅ Ctrl+Alt+I Cloud: "设计一个分布式锁方案"
   ```

2. **不要让 Cloud 等待批量任务**
   ```
   ❌ Cloud: "迁移 100 个文件"（阻塞）
   ✅ Background: "后台迁移 100 个文件"
   ```

3. **不要忽略模式选择**
   - 查看聊天面板顶部的模式指示
   - 不确定时选 @cloud

---

## 🔧 快速切换技巧

### 从任意模式切换到 Cloud

```
方式 1: 新建聊天
  → 点击 "+ New Chat Session"
  → 选择 @cloud

方式 2: 快捷键
  → Ctrl+Alt+I（确保打开的是 Cloud Chat）

方式 3: 命令面板
  → Ctrl+Shift+P → "Chat: Focus on Cloud Chat"
```

### 查看当前模式

聊天面板右上角会显示：
- `@local` - 本地模式
- `@cli` / Background - 后台模式  
- `@cloud` - 云端模式

---

## 💡 实用技巧

### 技巧 1: 上下文传递

```
Local → Cloud:
  Local 补全的代码不满意
  → 选中代码
  → Ctrl+I 打开内联聊天（Cloud）
  → "优化这段代码"

Cloud → Background:
  Cloud 生成计划
  → "在后台执行这个计划"
  → Background 异步执行
```

### 技巧 2: 并行工作

```
窗口 1: Cloud Chat - 设计讨论
窗口 2: 编辑器 - Local 补全编码
窗口 3: Background 任务 - 生成测试

你可以同时进行三件事！
```

### 技巧 3: 节省 Cloud 资源

```
简单任务用 Local:
  - 代码格式化
  - 简单重命名
  - 导入排序

复杂任务用 Cloud:
  - 架构设计
  - 复杂重构
  - 使用 Skills
```

---

## 🎓 学习建议

### 第 1 周: 熟悉基础

- 只用 Cloud + Local
- Cloud 做规划，Local 做补全
- 掌握 `Ctrl+Alt+I` 和 `Ctrl+I`

### 第 2 周: 引入 Background

- 识别长时间任务
- 尝试后台执行
- 学会并行工作

### 第 3 周: 组合优化

- 三种模式无缝切换
- 结合 Skills 系统
- 多聊天窗口协作

---

## 📝 速查表

| 我要... | 用什么模式 | 怎么做 |
|---------|-----------|--------|
| 快速补全代码 | Local | 直接输入，等待建议 |
| 设计新功能 | Cloud | `Ctrl+Alt+I` → "按照 iteration Skill" |
| 调试复杂 Bug | Cloud | `Ctrl+Alt+I` → "使用 systematic-debugging" |
| 批量重构 100 个文件 | Background | "在后台重构..." |
| 学习新技术 | Cloud | `Ctrl+Alt+I` → 提问 |
| 代码审查 | Cloud | "使用 code-review Skill" |
| 重命名变量 | Local | `F2` |
| 并行开发多模块 | Cloud + Background | 多聊天窗口 |

---

**记住：Cloud 是主力，Local 是助手，Background 是后勤。三者配合，效率翻倍！** 🚀
