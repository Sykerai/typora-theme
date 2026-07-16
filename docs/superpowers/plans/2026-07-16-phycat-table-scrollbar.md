# Phycat Table Scrollbar Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 Phycat 原生与 HTML 表格的横向滚动条统一为 5px，同时保持固定外框与主题配色。

**Architecture:** 在亮、暗共享 CSS 中给两类表格滚动容器增加局部 WebKit 滚动条尺寸规则。颜色与圆角继续由现有全局主题规则提供。

**Tech Stack:** CSS、Typora/Chromium WebKit 滚动条伪元素

## Global Constraints

- 仅修改 `phycat.light.css` 与 `phycat.dark.css`。
- 只改变表格横向滚动条高度，不改变表格布局或其他组件滚动条。
- 修改外部主题文件前必须建立备份。

---

### Task 1: 表格局部滚动条尺寸

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`
- Modify: `D:\史永康\Documents\docs\typora\phycat-forest-pending-changes.md`

**Interfaces:**
- Consumes: `figure.table-figure` 与 `.md-htmlblock-container:has(table)` 两个现有滚动容器。
- Produces: 两类表格容器专属的 `::-webkit-scrollbar { height: 5px }` 规则。

- [ ] **Step 1: 验证当前缺少表格专属滚动条高度规则**

检查两个共享文件，预期目标组合选择器的 `::-webkit-scrollbar` 规则数量均为 0。

- [ ] **Step 2: 写入最小 CSS 规则**

```css
#write figure.table-figure::-webkit-scrollbar,
#write .md-htmlblock-container:has(table)::-webkit-scrollbar {
    height: 5px
}
```

- [ ] **Step 3: 写回前验证**

确认两个暂存文件中目标规则各出现一次、高度为 `5px`、花括号平衡。

- [ ] **Step 4: 备份并写回主题文件**

分别保存修改前备份，再以已验证的暂存文件覆盖亮、暗共享文件。

- [ ] **Step 5: 最终验证与记录**

确认目标文件与暂存文件哈希一致、备份存在，并在 Phycat 变更记录中记录本次调整。

