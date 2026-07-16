# Phycat Code Border Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将所有 Phycat 亮暗主题的围栏代码块统一为 2px 外边框和 10px 圆角。

**Architecture:** 只修改两个共享 CSS 文件中的 `.md-fences` 基础规则，从而覆盖所有引用共享亮色或暗色底层的 Phycat 主题。保留各主题现有边框颜色变量及 Bloom Grid 结构。

**Tech Stack:** Typora CSS、PowerShell 静态验证、SHA-256 文件校验。

## Global Constraints

- 亮色边框颜色继续使用 `var(--element-color-shallow)`。
- 暗色边框颜色继续使用 `color-mix(in srgb, var(--secondary-color), transparent 80%)`。
- `.md-fences` 的边框宽度统一为 `2px`，圆角统一为 `10px`。
- 不修改 Grid、滚动、字体、字号、语法高亮、Mermaid 或表格规则。
- 写入外部主题目录前创建本次修改前备份。

---

### Task 1: 更新代码块外边框

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`
- Verify: `C:\Users\史永康\AppData\Roaming\Typora\themes\docs\typora\phycat-theme-maintenance-log.md`

**Interfaces:**
- Consumes: 两个共享 CSS 中现有 `.md-fences` 规则。
- Produces: 所有 Phycat 主题统一的 2px 外边框和 10px 圆角。

- [ ] **Step 1: 创建暂存副本并运行失败检查**

  将两个共享 CSS 复制到 `D:\史永康\Documents`，检查 `.md-fences` 是否同时包含 `border: 2px ...` 和 `border-radius: 10px`。预期现有文件检查失败。

- [ ] **Step 2: 修改暂存副本**

  亮色 `.md-fences` 使用：

  ```css
  border: 2px solid var(--element-color-shallow);
  border-radius: 10px
  ```

  暗色 `.md-fences` 使用：

  ```css
  border: 2px solid color-mix(in srgb, var(--secondary-color), transparent 80%);
  border-radius: 10px
  ```

- [ ] **Step 3: 验证暂存副本**

  检查两个文件的目标边框、10px 圆角、花括号平衡，以及 `display: grid`、`max-height: 70vh`、`JetBrains Mono`、`font-size: 16px` 均保留。预期全部通过。

- [ ] **Step 4: 备份并写入共享文件**

  创建 `phycat.light.css.bak-20260716-before-border-2px` 和 `phycat.dark.css.bak-20260716-before-border-2px`，随后将暂存副本覆盖到共享文件。

- [ ] **Step 5: 验证实际文件并记录**

  比较暂存文件与实际文件 SHA-256，重新运行全部静态检查；在变更记录中注明 2px 边框、10px 圆角和备份名称。验证成功后删除暂存副本。
