---
name: self-review
description: Use when any role needs to audit their own output for quality and completeness before handoff. Covers code quality, design consistency, documentation completeness, and AI编程规范 compliance.
---

# 自检 (Self-Review)

## 概述

在交付前进行自我审计，确保输出物达到工作室质量标准。

## 使用场景

- Developer 完成代码后，提交审查前
- Designer 完成设计文档后
- 任何角色准备交接前

## 自检维度

### 代码自检
- [ ] 所有模块暴露 `init/destroy` 接口
- [ ] `destroy` 中解绑所有事件和定时器
- [ ] 所有外部输入有空值保护
- [ ] Magic Number 已替换为命名常量
- [ ] 文件头 `@handoff` 标签已更新
- [ ] 遵循 `AI编程规范.md`

### 设计自检
- [ ] 所有公式有推导过程
- [ ] 所有数值表有边界标注
- [ ] 所有状态已覆盖
- [ ] 表格有单位标注

### 文档自检
- [ ] 文件头包含版本/作者/日期/状态
- [ ] 所有 TODO 有负责人和日期
- [ ] 所有交叉引用正确

---

*Mizuof 工作室 Meta-Skill: self-review v1.0*
