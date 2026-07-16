# Phycat Radiation Comment Contrast Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 提高 Phycat Radiation 代码注释在绿色选区上的可读性。

**Architecture:** 只覆盖 Radiation 入口主题中的注释颜色变量，不修改共享暗色主题。保留原选区背景，并以对比度计算和 CSS 静态检查验证。

**Tech Stack:** CSS、PowerShell、WCAG 对比度公式

## Global Constraints

- 只修改 `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-radiation.css`。
- `--code-comment` 最终按用户目视确认设为 `#999999`。
- 不修改 `--code-selected-bg: #447344`。

---

### Task 1: Radiation 注释颜色

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-radiation.css`

**Interfaces:**
- Consumes: 共享暗色主题对 `var(--code-comment)` 的使用。
- Produces: Radiation 专属的高对比度注释色。

- [x] **Step 1: 运行修改前检查**

确认当前值为 `#546e7a`，且其相对 `#447344` 的对比度低于 4.5:1。

- [x] **Step 2: 创建备份**

备份原始 `phycat-radiation.css`。

- [x] **Step 3: 最小修改**

```css
--code-comment: #999999;
```

- [x] **Step 4: 运行修改后检查**

确认变量值唯一、CSS 大括号平衡，且相对选中背景的对比度不低于 4.5:1。
