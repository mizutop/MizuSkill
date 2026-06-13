---
name: verification-loop
description: Use when a role needs to verify their work is complete before handing off. Activates during phase transitions — checks the phase-specific checklist, runs self-validation, and ensures all @handoff tags are current before the next role begins.
---

# 验证循环 (Verification Loop)

## 概述

在每次角色交付前执行验证循环，确保输出物符合质量标准。

## 使用场景

- 角色完成交付物，准备交接给下一角色时
- 阶段切换前的自我验证
- 代码提交前的合规检查

## 验证流程

1. **加载检查清单** ─ 读取 `pipeline/checklist.md` 对应阶段检查项
2. **逐项验证** ─ 检查每项是否满足
3. **记录结果** ─ PASS/FAIL + 严重级别 + 备注
4. **问题修复** ─ 所有 FAIL 项必须修复后重新验证
5. **签注通过** ─ 所有项通过后标记验证完成

## 输出

```markdown
## 验证结果

| # | 检查点 | 状态 | 严重级别 | 备注 |
|---|--------|------|---------|------|
| 1 | ... | PASS | - | |
| 2 | ... | FAIL | P1 | 原因... |
```

---

*Mizuof 工作室 Meta-Skill: verification-loop v1.0*
