# Phycat Code Auto Height Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 让短围栏代码块按内容自动高度展示，仅在内容真正超过现有限高时出现竖向滚动条。

**Architecture:** 在亮、暗共享 CSS 中将非 Mermaid `.CodeMirror-scroll` 设为内容自适应的原生纵向滚动容器。`height: auto` 消除短代码假溢出，最大高度和 `overflow-y: auto` 让长代码在 CodeMirror 实际接收滚轮事件的层内滚动。

**Tech Stack:** CSS、Typora、CodeMirror

## Global Constraints

- 仅修改 `phycat.light.css` 与 `phycat.dark.css`。
- 将 `height: auto`、`max-height: calc(70vh - 32px)` 与 `overflow-y: auto !important` 设置在 `.CodeMirror-scroll`，并保留固定工具栏结构。
- Mermaid 与表格样式不受影响。
- 修改外部主题文件前必须建立备份。

---

### Task 1: 非 Mermaid 代码块自动高度

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`
- Modify: `D:\史永康\Documents\docs\typora\phycat-forest-pending-changes.md`

**Interfaces:**
- Consumes: 非 Mermaid `.md-fences` 中的 `.CodeMirror-scroll` 内部滚动区域。
- Produces: 内容自适应的内部原生滚动容器，短内容无滚动条、长内容可用滚轮和拖动条滚动。

- [ ] **Step 1: 验证当前使用不可操作的外层滚动**

检查亮、暗共享文件，预期 `.cm-s-inner.CodeMirror` 外层原生滚动规则各出现一次，目标内部完整规则均为 0。

- [ ] **Step 2: 写入最小规则**

```css
.md-fences:not([lang=mermaid]) .CodeMirror-scroll {
    height: auto;
    max-height: calc(70vh - 32px);
    overflow-y: auto !important
}
```

同时删除主题中非 Mermaid `.cm-s-inner.CodeMirror` 的外层滚动规则。

- [ ] **Step 3: 验证暂存文件**

确认内部完整规则各出现一次、外层滚动规则均为 0、花括号平衡。

- [ ] **Step 4: 备份并写回**

保存修改前备份，再用已验证的暂存文件覆盖亮、暗共享主题。

- [ ] **Step 5: 最终核验并记录**

确认目标文件与暂存文件哈希一致、备份存在，并更新 Phycat 变更记录。
