---
name: test-driven-development
description: 当实现任何功能或修复 Bug 时使用，在编写实现代码之前。
---

# 测试驱动开发 (TDD) Skill

## 概述

**先写测试，永远如此。** 这不是建议，这是强制性流程。

## The Iron Law

```
没有失败的测试，就没有实现代码。
```

在测试之前写了实现代码？删掉。重新开始。

**没有例外：**
- 不是"简单的补充"
- 不是"只是一个辅助函数"
- 不是"太简单不需要测试"

## 核心循环

```
RED:      编写测试 → 运行 → 看到失败（且失败原因正确）
GREEN:    编写最小实现 → 运行 → 看到通过
REFACTOR: 优化代码 → 运行 → 确认仍然通过
COMMIT:   git add → git commit
```

### RED 阶段详解

1. **写一个测试** — 描述你期望的行为
2. **运行测试** — 必须看到失败
3. **确认失败原因** — 是功能缺失导致的失败，不是语法错误

```javascript
// 示例: 先写测试
describe('calculateDiscount', () => {
  it('should apply 10% discount for orders over $100', () => {
    const result = calculateDiscount(150, 'standard');
    expect(result).toBe(135);
  });
});
// 运行 → FAIL: "calculateDiscount is not defined" ✅ 正确的失败原因
```

### GREEN 阶段详解

1. **写最小的代码让测试通过** — 不多不少
2. **运行测试** — 必须看到通过
3. **不要过度设计** — 只满足当前测试

```javascript
// 示例: 最小实现
function calculateDiscount(amount, type) {
  if (amount > 100) {
    return amount * 0.9;
  }
  return amount;
}
// 运行 → PASS ✅
```

### REFACTOR 阶段详解

1. **改善代码质量** — 但不改变行为
2. **运行测试** — 必须仍然通过
3. **如果测试失败** — 撤销重构

### COMMIT

```bash
git add <相关文件>
git commit -m "feat: add discount calculation for orders over $100"
```

## 快速参考

| 情况 | 做法 |
|------|------|
| 不知道怎么测试 | 先写你期望的 API。先写断言。 |
| 测试太复杂 | 设计太复杂。简化接口。 |
| 必须 mock 所有东西 | 代码耦合太紧。使用依赖注入。 |
| 测试 Setup 太大 | 提取辅助函数。还复杂？简化设计。 |
| "太简单不需要测试" | 这就是你需要测试的原因。 |

## 验证检查清单

完成前必须确认：

- [ ] 每个新函数/方法都有测试
- [ ] 看到每个测试失败过
- [ ] 每个测试因为正确的原因失败（功能缺失，不是错别字）
- [ ] 写了最小代码使测试通过
- [ ] 所有测试通过
- [ ] 输出干净（无错误、无警告）
- [ ] 边界条件和错误情况已覆盖
- [ ] 已提交

**不能全部打勾？你跳过了 TDD。重新开始。**

## 常见借口和反驳

| 借口 | 反驳 |
|------|------|
| "太简单了" | 越简单越应该测试 |
| "时间不够" | TDD 总体上节省时间 |
| "我先写代码，后补测试" | 那不是 TDD |
| "这只是原型" | 原型会变成产品代码 |
| "测试框架还没配好" | 那就先配测试框架 |
