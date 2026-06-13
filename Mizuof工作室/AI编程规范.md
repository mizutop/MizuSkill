# Mizuof AI 编程规范

> 版本 v2.0 | 生效日期 2025-07-01
> 规范 AI 在工作室中的代码生成、注释、上下文传递和角色交接行为

---

## 1. 核心原则

1. **AI 写的每一行代码都必须有归属** ─ 每个模块标注负责角色和生成会话
2. **上下文是最珍贵的资源** ─ 代码和注释必须承载足够上下文，减少 AI 下次推理成本
3. **可被下个 AI 接手** ─ 任何代码必须能让一个没有历史会话的新 AI agent 直接理解

---

## 2. 注释规范

### 2.1 文件头注释

每个文件必须有文件头，格式：

```javascript
/**
 * @file        {文件名}
 * @module      {模块名}
 * @role        {负责角色名}
 * @session     {会话ID/日期}
 * @dependency  {依赖的外部模块}
 * @description {该文件的核心职责，1-2句}
 * @handoff     {交接给谁时的注意事项}
 */
```

### 2.2 函数/方法注释

```javascript
/**
 * {函数核心作用一句话}
 * @param {类型} paramName - {参数说明，包括边界值含义}
 * @returns {类型} - {返回值说明}
 * @throws {错误类型} - {什么情况下会抛错}
 * @example
 *   // 输入/输出示例
 *   call(1) => result
 * @note {可选：实现考量、优化空间、已知限制}
 */
```

### 2.3 模块交接注释

在模块顶部使用 `@handoff` 标签标注交接信息：

```javascript
// @handoff-to: {下一个角色名}
// @handoff-condition: {下一个角色可以开始工作的条件}
// @pending-decisions: [{未决事项1}, {未决事项2}]
// @known-issues: [{已知问题1, severity: P0/P1/P2}]
```

### 2.4 代码段注释规则

| 场景 | 注释要求 | 示例 |
|------|---------|------|
| 复杂逻辑 (>5行) | 必须写"为什么这样做" | `// 使用移位而非乘法，因x86上快3倍` |
| 边界条件 | 标注条件和原因 | `// level=0时跳过，防止除零` |
| Magic Number | 必须定义常量并注释 | `const MAX_LEVEL = 99; // 设计上限` |
| TODO | 统一格式 | `// TODO({角色名}): {描述} @{日期}` |
| FIXME | 统一格式 | `// FIXME({角色名}): {描述} @{日期} severity:P1` |
| HACK | 必须说明原因和改进计划 | `// HACK: 因Safari不支持XX，改用YY @{日期}` |

---

## 3. 命名规范

| 类别 | 规范 | 反例 |
|------|------|------|
| 变量 | camelCase, 全英文, 自解释 | `let a = 1` → `let playerLevel = 1` |
| 常量 | UPPER_SNAKE_CASE | `const MAX_HP = 9999` |
| 函数 | 动词开头 camelCase | `getPlayerName()` ✅ / `playerName()` ❌ |
| 类/构造器 | PascalCase | `class SoulRingSystem` |
| 文件 | kebab-case | `soul-ring-system.js` ✅ |
| CSS 变量 | `--{context}-{property}` | `--color-primary`, `--spacing-md` |
| CSS 类 | BEM: `block__element--modifier` | `card__title--active` |
| 事件名 | `{模块}:{动作}` | `soul:absorb`, `player:levelup` |

### 3.1 禁止行为

- 禁止中文命名（变量/函数/文件）
- 禁止拼音缩写（`zhanghao` → `account`）
- 禁止无意义的单字母变量（循环计数器 `i, j, k` 除外）
- 禁止硬编码中文文案在 JS 逻辑中 ─ 使用常量或配置

---

## 4. 上下文传递协议

### 4.1 会话启动注释

每个 AI 新会话启动时，工作室必须提供：

```markdown
<!-- context-start
  项目: {项目名}
  当前阶段: {阶段名}
  角色: {角色名}
  依赖输入: [{输入文件1}, {输入文件2}]
  上一个输出: {上一个角色交付的文件}
  未决事项: [{事项1}, {事项2}]
  工作室技能: [{需绑定的 Skill 名}]
  context-end -->
```

### 4.2 阶段交接检查清单

在每次角色交接时，交付方必须在交付物中包含：

- [ ] 所有 `@handoff` 标签已更新
- [ ] 所有 `TODO/FIXME/HACK` 已更新为最新状态
- [ ] 已知问题清单已更新
- [ ] 未做决策已记录 `@pending-decisions`
- [ ] 交接条件已满足

### 4.3 跨会话持久化

当工作需要跨多个 AI 会话完成时：

1. 每个会话结束时产出 `会话摘要.md`，包含：已完成项、当前状态、下步计划
2. 新会话开始时，必须读取上一个会话的 `会话摘要.md`
3. 所有重要决策必须记录在 `templates/技术决策记录(ADR).md`

---

## 5. 错误处理规范

```javascript
// ✅ 正确：明确错误场景
try {
    const data = loadFromStorage(key);
    if (data === null) throw new StorageError('KEY_NOT_FOUND', key);
    return validateData(data);
} catch (e) {
    if (e instanceof StorageError) {
        return getDefaultValue(key); // 降级策略
    }
    throw e; // 未知错误向上传播
}

// ❌ 错误：吞掉所有异常
try { ... } catch (e) { /* 什么都不做 */ }
// ❌ 错误：console.log 代替错误处理
try { ... } catch (e) { console.log(e); }
```

---

## 6. 模块化原则

```
每个模块必须暴露：
  init(config)      — 初始化，接收配置对象
  destroy()         — 销毁，解绑所有事件和定时器
  getState()        — 返回可序列化的当前状态（用于存档）
  setState(data)    — 从存档恢复状态

可选：
  on(event, cb)     — 事件监听
  off(event, cb)    — 取消监听
```

### 6.1 模块间通信

- 禁止模块之间直接调用函数
- 使用事件总线（EventBus）通信
- 事件名格式：`{模块}:{动作}` (例: `soul:absorbed`)

---

## 7. AI 代码生成约束

| 约束 | 说明 |
|------|------|
| 禁止幻觉 API | 必须使用已验证的 API，不虚构不存在的库或方法 |
| 禁止硬编码 | 所有可配置值必须提取为常量或配置对象 |
| 禁止死代码 | 不生成不被调用的函数或变量 |
| 重复次数 ≤ 2 | 相同逻辑出现 2 次以上必须提取为通用函数 |
| 单函数 ≤ 50行 | 超过 50 行必须拆分为子函数 |
| 单文件 ≤ 300行 | 超过 300 行必须拆分模块 |
| JS 禁止 var | 只使用 const / let |
| 必须预处理 | 所有外部输入必须做空值/类型检查 |

---

## 8. 工作室技能调用规范

当工作室激活某个角色时，必须加载绑定到该角色的 Mizu_skill：

```markdown
<!-- skill-binding
  role: {角色名}
  skills: [{skill1}, {skill2}, {skill3}]
  skill-path: Mizu_skill/{group}/{skill-name}/
-->
```

---

*Mizuof 工作室 AI 编程规范 v2.0 ─ 让每个 AI 都站在上一个 AI 的肩膀上*
