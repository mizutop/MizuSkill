# 交付物清单
> Mizuof 工作室 v2.0 ─ 标准化交付物命名规范和存放路径

---

## 文件命名规范

```
{类型}-{模块/版本}-{日期}.{扩展名}

类型缩写:
  PRD       → 产品需求文档
  ADR       → 架构决策记录
  DESIGN    → 系统设计文档
  GDD       → 游戏设计文档
  INTERACT  → 交互设计文档
  UI        → UI 组件规范
  UX        → UX 评估报告
  ANIM      → 动画方案
  TEST      → 测试计划/用例
  BUG       → Bug 报告
  RELEASE   → 发布说明
  MEETING   → 会议纪要
  META      → 元数据/技能遵从报告
```

示例：
- `PRD-v1.0-20250701.md`
- `ADR-001-技术栈选择-20250701.md`
- `DESIGN-灵魂系统-20250701.md`
- `TEST-灵魂吸收-20250701.md`
- `BUG-001-20250701.md`
- `RELEASE-v1.0.0-20250701.md`

---

## 路径映射

| 角色 | 输出物 | 存放路径 | 模板引用 |
|------|--------|---------|---------|
| Producer | PRD | `projects/{项目}/docs/` | `templates/方案计划书.md` |
| Architect | ADR | `projects/{项目}/docs/adr/` | `templates/技术决策记录(ADR).md` |
| Designer | GDD/设计文档 | `projects/{项目}/docs/design/` | `templates/GDD模板.md` |
| Interaction | 交互流程图 | `projects/{项目}/docs/interaction/` | `templates/交互流程图模板.md` |
| UI | UI 组件规范 | `projects/{项目}/docs/ui/` | `templates/UI组件规范模板.md` |
| UX | UX 评估报告 | `projects/{项目}/docs/ux/` | `templates/UX评估报告模板.md` |
| Visual Arch | 视觉系统 | `projects/{项目}/docs/brand/` | 内联 |
| Animator | 动画方案 | `projects/{项目}/docs/animation/` | 内联 |
| Developer | 代码 | `projects/{项目}/js/` 或 `css/` | 无 |
| Test Arch | 测试框架 | `projects/{项目}/test/` | 无 |
| QA Guardian | Bug 报告 | `projects/{项目}/docs/bugs/` | `templates/Bug报告模板.md` |
| QA Guardian | 测试计划 | `projects/{项目}/docs/test/` | `templates/提测单模板.md` |
| DevOps | 发布说明 | `projects/{项目}/docs/releases/` | `templates/发布说明模板.md` |
| Meta-Guard | 技能报告 | `projects/{项目}/docs/meta/` | 内联 |
| 全员 | 会议纪要 | `projects/{项目}/meetings/` | `meetings/` 下各模板 |

---

## 交付物签名区模板

每个交付物末尾必须包含：

```markdown
---

## 签收

| 角色 | 姓名 | 签字 | 日期 | 备注 |
|------|------|------|------|------|
| Producer | | | | |
| Architect | | | | |
| Developer | | | | |
| QA Guardian | | | | |

**签字即表示已阅读并同意本交付物内容。**
```

---

## 版本记录模板

每个交付物头部必须包含：

```markdown
> 版本: {版本号} | 作者: {角色} | 日期: {YYYY-MM-DD} | 状态: {草稿/评审中/已冻结/已发布}
```

---

*Mizuof 工作室 ─ Deliverables v2.0*
