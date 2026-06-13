---
name: communication-protocol
description: 角色间通信协议 — 定义统一的消息格式、通信通道和响应时效。当角色需要向其他角色提问、澄清需求或传递信息时激活。
---

# 角色间通信协议 v1.0

## 激活时机

- 角色需要提问/澄清上游输出物
- 角色需要通知下游注意事项
- 角色需要向 Producer 汇报阻塞

---

## 消息格式

所有角色通信使用以下统一格式：

```
@from: {角色名}
@to: {角色名}
@priority: P0/P1/P2
@subject: {一句话标题}
@body: {具体内容}
@response-needed: yes/no
@deadline: {日期时间 或 "无"}
```

---

## 通信通道

| 场景 | 通道 | 响应时间 |
|------|------|---------|
| 阶段内协同 | 直接 `@to` | 2h 内回复 |
| 跨阶段问题 | 通过 `@Producer` 中转 | 4h 内回复 |
| P0 紧急 | 直接通信 + 同步 `@Producer` | 30min 内回复 |
| Meta-Guardian 审计 | `@to: 全员` | 4h 内回复 |

---

## 示例

```
@from: Developer
@to: UI Designer
@priority: P1
@subject: 按钮颜色不确定
@body: 组件规范中 "primary" 按钮有两种蓝色（#4a8aff 和 #3d7eff），请确认使用哪个
@response-needed: yes
@deadline: 2025-06-12 18:00
```

---

*Mizuof 工作室 ─ Communication Protocol v1.0*
