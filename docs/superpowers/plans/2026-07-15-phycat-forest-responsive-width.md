# Phycat Forest Responsive Width Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 让 Phycat Forest 正文在小屏占可用宽度的 90%，在大屏不超过 1200px，并保持编辑与 HTML 导出一致。

**Architecture:** 保留 `--max-width: 1200px` 作为宽屏上限，在主题入口文件中为 `#write` 增加 `width: 90%` 和水平居中覆盖，并用导出专属规则覆盖共享主题的小屏 `width: 100%`。共享的 `phycat.light.css` 与打印规则不修改。

**Tech Stack:** Typora CSS、PowerShell 静态验证

## Global Constraints

- 小屏正文宽度为可用区域的 90%。
- 大屏正文宽度最大为 1200px。
- HTML 导出使用相同的 90% 宽度和 1200px 上限。
- 保留打印规则以及现有表格、代码字体、提示块和工具栏调整。
- 修改前创建备份。

---

### Task 1: 正文响应式宽度

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css`
- Create: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-responsive-width`

**Interfaces:**
- Consumes: `--max-width: 1200px` 与共享主题中的 `#write` 规则
- Produces: `#write { width: 90%; max-width: var(--max-width); margin-left: auto; margin-right: auto; }`

- [ ] **Step 1: 运行修改前检查并确认失败**

```powershell
$css = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
if ($css -match '(?s)#write\s*\{[^}]*width:\s*90%') { throw 'unexpected pass' } else { 'RED: responsive width rule is absent' }
```

预期：输出 `RED: responsive width rule is absent`。

- [ ] **Step 2: 创建修改前备份**

```powershell
Copy-Item -LiteralPath 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Destination 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-responsive-width'
```

- [ ] **Step 3: 添加最小覆盖规则**

在主题变量结束后、HTML 导出规则前加入：

```css
/* 正文响应式宽度：小屏保留 5% 外边距，宽屏不超过 1200px */
#write {
  width: 90%;
  max-width: var(--max-width);
  margin-left: auto;
  margin-right: auto;
}
```

导出规则同步设置 90% 宽度与居中：

```css
.typora-export #write {
  width: 90%;
  max-width: var(--max-width);
  margin-left: auto;
  margin-right: auto;
}
```

- [ ] **Step 4: 运行修改后检查并确认通过**

```powershell
$css = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
[pscustomobject]@{
  Width90 = $css -match '(?s)#write\s*\{[^}]*width:\s*90%'
  Max1200 = $css -match '--max-width:\s*1200px'
  ExportResponsive = $css -match '(?s)\.typora-export\s+#write\s*\{[^}]*width:\s*90%;[^}]*max-width:\s*var\(--max-width\)'
  TablePreserved = $css -match '(?s)#write table\s*\{[^}]*font-size:\s*16px;[^}]*line-height:\s*2'
  JetBrainsPreserved = $css -match 'font-family:\s*"JetBrains Mono"'
}
```

预期：全部属性为 `True`。

- [ ] **Step 5: 验证备份与 CSS 结构**

```powershell
$before = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-responsive-width'
$after = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
[pscustomobject]@{
  BackupExcludesNewRule = $before -notmatch '(?s)#write\s*\{[^}]*width:\s*90%'
  BracesBalanced = (($after.ToCharArray() | Where-Object { $_ -eq '{' }).Count -eq ($after.ToCharArray() | Where-Object { $_ -eq '}' }).Count)
}
```

预期：两个属性均为 `True`。
