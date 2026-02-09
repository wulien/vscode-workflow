# 项目约定

> 编码规范和团队约定，确保不同 session 产出一致的代码风格。

## 命名约定

### 文件命名
- **组件**: `PascalCase.tsx` (例: `UserProfile.tsx`)
- **工具函数**: `camelCase.ts` (例: `formatDate.ts`)
- **常量**: `SCREAMING_SNAKE_CASE` (例: `API_BASE_URL`)
- **测试文件**: `*.spec.ts` 或 `*.test.ts`
- **样式文件**: `*.module.css` 或 `*.styles.ts`

### 代码命名
- **变量/函数**: `camelCase`
- **类/接口/类型**: `PascalCase`
- **常量**: `UPPER_SNAKE_CASE`
- **私有属性**: `_prefixed` 或 `#private`
- **布尔值**: `is/has/should` 前缀 (例: `isActive`, `hasPermission`)

## Git 约定

### 分支命名
```
feature/<name>     — 新功能
fix/<name>         — Bug 修复
refactor/<name>    — 重构
docs/<name>        — 文档
test/<name>        — 测试
```

### 提交信息
```
<type>(<scope>): <description>

类型: feat, fix, refactor, test, docs, chore, style, perf
示例: feat(auth): add JWT token refresh
```

## 代码风格

### 通用规则
- 缩进: [2 空格 | 4 空格 | Tab]
- 行宽: [80 | 100 | 120]
- 末尾分号: [是 | 否]
- 引号: [单引号 | 双引号]

### 模式和反模式

**推荐 ✅:**
- [具体的推荐模式]
- [例如: 使用 async/await 而不是 .then()]
- [例如: 优先使用函数组件而不是类组件]

**避免 ❌:**
- [具体的反模式]
- [例如: 不要在组件中直接调用 API]
- [例如: 避免 any 类型]

## 测试约定

### 测试结构
```
describe('[模块/组件名]', () => {
  describe('[方法/功能]', () => {
    it('should [预期行为] when [条件]', () => {
      // Arrange
      // Act
      // Assert
    });
  });
});
```

### 测试文件位置
- 单元测试: `tests/unit/` 或 `__tests__/`
- 集成测试: `tests/integration/`
- E2E 测试: `tests/e2e/`

## 错误处理约定

- [统一的错误处理方式]
- [例如: 使用自定义 Error 类]
- [例如: 统一的 API 错误响应格式]

## 环境变量

- 格式: `UPPER_SNAKE_CASE`
- 示例文件: `.env.example`
- 敏感信息: 永远不要提交到 Git

## 文档约定

- README 必须包含: 项目描述、安装、使用、开发
- 函数注释: JSDoc / docstring
- 复杂逻辑: 行内注释说明 WHY，不说明 WHAT

---

*最后更新: [YYYY-MM-DD]*
*更新者: [Session 描述]*
