---
type: meta
title: Frontmatter 规范
---

# 知识点 Note Frontmatter 规范

本文档定义知识点 markdown 文件中 YAML frontmatter 的完整字段规范。

---

## 必填字段

| 字段 | 类型 | 说明 | 示例 |
|---|---|---|---|
| `type` | string | 文件类型 | `knowledge-point` |
| `textbook` | string | 所属课本名称 | `高级中学课本 数学 高中一年级 第一学期` |
| `chapter` | number | 章序号 | `1` |
| `section` | string | 节编号 | `"1.2"` |
| `title` | string | 知识点标题 | `子集的定义与性质` |
| `difficulty` | number | 难度等级 1-5 | `3` |
| `tags` | list | 分类标签 | `[集合, 基础概念]` |
| `prerequisites` | list | 前置知识点（wikilink 格式） | `["[[集合的概念]]"]` |

## 复习状态字段

| 字段 | 类型 | 说明 | 初始值 |
|---|---|---|---|
| `last_review` | date | 上次复习日期 | 空 |
| `next_review` | date | 下次应复习日期 | 空 |
| `interval_days` | number | 当前间隔天数 | `0` |
| `ease_factor` | float | SM-2 难度因子 | `2.5` |
| `review_count` | number | 累计复习次数 | `0` |
| `correct_streak` | number | 当前连续正确次数 | `0` |
| `mastery` | float | 掌握度 0.0-1.0 | `0` |

## 可选字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `error_log` | list | 错误记录（轻量级） |
| `notes` | string | agent 备注 |
| `exam_relevant` | boolean | 是否高考重点 |
| `created` | date | 创建日期 |

## 复习状态生命周期

```
未学习 → 首次学习完成 → 进入复习循环
  (mastery=0)  (mastery=0.3)   (mastery 逐步上升)
```

### 状态转换

| 事件 | mastery 变化 | interval 变化 | ease_factor 变化 |
|---|---|---|---|
| 首次学习通过 | → 0.3 | → 1天 | 保持 2.5 |
| 复习答对 | +0.1（上限1.0） | × ease_factor | +0.1（上限3.0） |
| 复习答错 | -0.2（下限0） | → 1天（重置） | -0.2（下限1.3） |
| Agent 裁量：犹豫答对 | +0.05 | 不变 | 不变 |
| Agent 裁量：计算失误 | -0.05 | ÷ 2 | 不变 |

## 错误日志格式

```yaml
error_log:
  - date: 2026-07-25
    type: 概念混淆 | 计算失误 | 单点遗忘 | 前置缺失 | 方法缺陷
    brief: "一句话描述错误"
```

## 难度等级定义

| 等级 | 含义 | 描述 |
|---|---|---|
| 1 | 基础 | 直接记忆/简单应用 |
| 2 | 常规 | 需要一步推理 |
| 3 | 中等 | 需要多步推理或技巧 |
| 4 | 较难 | 需要综合运用多个知识点 |
| 5 | 挑战 | 竞赛级/压轴级 |

## 完整示例

```yaml
---
type: knowledge-point
textbook: 高级中学课本 数学 高中一年级 第一学期
chapter: 1
section: "1.2"
title: 子集的定义与性质
difficulty: 2
tags:
  - 集合
  - 基础概念
prerequisites:
  - "[[集合的概念]]"
  - "[[集合的表示方法]]"
# --- 复习状态 ---
last_review: 2026-07-25
next_review: 2026-07-27
interval_days: 3
ease_factor: 2.6
review_count: 4
correct_streak: 3
mastery: 0.7
# --- 错误记录 ---
error_log:
  - date: 2026-07-20
    type: 概念混淆
    brief: "混淆了⊆和∈的用法"
---
```
