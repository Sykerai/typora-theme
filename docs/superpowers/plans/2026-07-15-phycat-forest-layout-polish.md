# Phycat Forest Layout Polish Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 完成已确认的表格、围栏代码块、表格工具栏和 GitHub Alert 标题样式调整。

**Architecture:** 只在 `phycat-forest.css` 入口文件末尾添加主题专属覆盖，保留共用的 `phycat/phycat.light.css` 不变，避免影响其他 Phycat 主题。用可重复的 PowerShell 静态断言完成修改前失败、修改后通过的验证，并保留当前主题文件备份。

**Tech Stack:** Typora CSS、PowerShell 静态断言

## Global Constraints

- 表格字体为 `16px`，行高为 `2`。
- 表格内行内代码继续使用现有 `.9em`，不得添加单独覆盖。
- 围栏代码块字体为 `1rem`（`16px`）。
- 表格工具栏使用与 `drake-light` 一致的 `margin-top: -1rem !important`。
- Alert 外层色块圆角保留；标题容器的白底、模糊、胶囊圆角、阴影和额外内边距移除。
- 共用基础文件 `phycat/phycat.light.css` 不得修改。
- 写入前备份当前已经包含 `1200px` 宽度优化的主题文件。

---

### Task 1: 添加 Forest 主题专属排版覆盖

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css`
- Create: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-layout-polish`
- Test: PowerShell 对目标 CSS 的选择器和值进行静态断言

**Interfaces:**
- Consumes: 入口文件导入的 `phycat/phycat.light.css` 默认规则
- Produces: 仅对 `phycat-forest` 生效的后置 CSS 覆盖

- [ ] **Step 1: 运行修改前断言并确认失败**

检查入口文件是否已经同时包含 `#write table` 的 `16px/2`、`.md-fences` 的 `1rem`、`.ty-table-edit` 的负边距，以及 `.md-alert-text-container` 的透明重置。

Expected: 断言失败，因为这些覆盖当前不存在。

- [ ] **Step 2: 备份当前主题并校验哈希**

备份 `phycat-forest.css`，并确认备份哈希与修改前目标文件一致。

Expected: 哈希一致，且备份中保留 `--max-width: 1200px` 和 HTML 导出宽度覆盖。

- [ ] **Step 3: 添加最小 CSS 覆盖**

```css
/* 表格与代码块排版 */
#write table {
  font-size: 16px;
  line-height: 2;
}

.md-fences {
  font-size: 1rem;
}

/* 缩短表格工具栏与表格之间的距离 */
.ty-table-edit {
  margin-top: -1rem !important;
}

/* GitHub Alert 标题直接显示在色块背景上 */
.md-alert-text-container {
  background-color: transparent;
  -webkit-backdrop-filter: none;
  backdrop-filter: none;
  padding: 0;
  border-radius: 0;
  box-shadow: none;
}
```

- [ ] **Step 4: 运行修改后静态断言**

Expected: 四组目标规则全部匹配；`#write code:not(.md-fencescode)` 仍只在基础文件中使用 `.9em`，入口文件没有表格行内代码覆盖；CSS 花括号平衡。

- [ ] **Step 5: 验证原有宽度优化和共享文件完整性**

Expected: `--max-width: 1200px` 和 `.typora-export #write` 仍存在；共享基础文件的哈希与修改前一致。

- [ ] **Step 6: 在 Typora 中目视检查**

重新加载 `phycat-forest` 后检查普通表格、含行内代码的表格、围栏代码块、表格工具栏，以及五类 GitHub Alert。

Expected: 所有目标样式符合约束，普通引用块和其他 Phycat 主题不受影响。

> 当前工作目录不是 Git 仓库，因此本任务不包含提交步骤。
