# Phycat Forest Width Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 Phycat Forest 的编辑视图和 HTML 导出正文最大宽度统一调整为 1200px。

**Architecture:** 保留现有 CSS 结构，以主题入口文件中的 `--max-width` 作为唯一宽度配置。在主题入口文件中添加导出视图专属覆盖，使编辑与 HTML 导出保持一致，且不修改其他 Phycat 主题共用的基础文件；打印规则不变。

**Tech Stack:** Typora CSS、PowerShell 静态检查

## Global Constraints

- 正文最大宽度必须为 `1200px`。
- 窄窗口继续依靠 `max-width` 自适应收缩。
- 不修改现有内边距、侧边栏断点及打印规则。
- 写入前备份原文件。

---

### Task 1: 统一编辑与 HTML 导出宽度

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css:50`
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css`
- Create: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715`

**Interfaces:**
- Consumes: `phycat-forest.css` 定义的 CSS 自定义属性 `--max-width`
- Produces: 编辑与 HTML 导出共同使用的 `1200px` 最大正文宽度

- [ ] **Step 1: 运行修改前检查并确认当前状态**

```powershell
Select-String -Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Pattern '^\s*--max-width:\s*950px;'
Select-String -Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css' -Pattern '^\s*max-width:\s*950px;'
```

Expected: 两项检查均匹配；共用基础文件的导出规则仍是 `950px`，需要由 Forest 主题入口文件覆盖。

- [ ] **Step 2: 创建原文件备份**

```powershell
Copy-Item -LiteralPath 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Destination 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715'
```

Expected: 备份文件存在并与原文件哈希一致。

- [ ] **Step 3: 写入最小修改**

将入口主题变量改为：

```css
--max-width: 1200px;
```

在 `phycat-forest.css` 末尾添加 HTML 导出专属覆盖：

```css
.typora-export #write {
  max-width: var(--max-width);
}
```

- [ ] **Step 4: 验证修改与未触及规则**

```powershell
Select-String -Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Pattern '^\s*--max-width:\s*1200px;'
Select-String -Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Pattern '^\s*max-width:\s*var\(--max-width\);'
Select-String -Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css' -Pattern '@media print|@media screen and \(max-width:1200px\)'
```

Expected: Forest 入口文件中的变量和导出覆盖均匹配，共用基础文件的屏幕断点与打印规则仍存在。

- [ ] **Step 5: 在 Typora 中重新加载主题**

重新选择 `phycat-forest` 主题或重启 Typora，确认宽屏正文变宽、窄窗口无横向溢出。

Expected: 编辑视图与 HTML 导出最大正文宽度均为 `1200px`。

> 当前工作目录不是 Git 仓库，因此本任务不包含提交步骤。
