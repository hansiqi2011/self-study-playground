---
type: template
title: 每日报告模板
---

# Session 报告模板

> 此文件为模板参考，实际报告由 agent 在每次 session 结束时自动生成到 `_reports/` 目录。

---

## 格式规范

文件名：`YYYY-MM-DD-HHmm.md`

```yaml
---
date: 2026-07-25
session_type: quiz | learn | exam
duration_approx: 30min
knowledge_points_reviewed: 8
knowledge_points_learned: 2
accuracy: 0.75
blindspots_found: 1
---
```

## 正文结构

### 概览
- 复习知识点数量和正确率
- 新学知识点数量
- 发现盲区数量

### 复习详情（表格）
| 知识点 | 结果 | 备注 |
|---|---|---|
| 知识点名 | ✅/❌ | 简要说明 |

### 学新详情
- 学习了哪些内容
- 即时检测结果

### 盲区发现
- 什么盲区、怎么处理的
- 是否创建了错题 note
- 计划调整

### 给家长的建议
- 当前薄弱环节
- 建议下一步方向
- 需要关注的问题
