# Phycat Forest Block Alignment Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 参考 Drake Light，让 Phycat Forest 的主要块级元素采用统一的水平排版基准，同时保留内部缩进和标题装饰。

**Architecture:** 只在 `phycat-forest.css` 入口文件中增加主题专属覆盖，不修改共享 `phycat.light.css`。通过零水平外边距、`max-width: 100%` 和 `border-box` 统一外部边界；一级标题及各元素内部 padding 保持原样。

**Tech Stack:** Typora CSS、PowerShell 静态验证

## Global Constraints

- 覆盖段落、二至六级标题、表格、引用块、围栏代码块、无序列表、有序列表、定义列表、任务列表和 GitHub Alert。
- 一级标题继续居中，二、三级标题继续使用 `fit-content` 装饰。
- 任务复选框、列表缩进、引用和代码内部 padding 不变。
- 正文宽度继续为 `90%`，最大 `1200px`。
- 表格字号、代码字号、JetBrains Mono、HTML 导出和打印规则不变。
- 修改前创建备份。

---

### Task 1: 统一块级元素水平排版基准

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css`
- Create: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-block-alignment`

**Interfaces:**
- Consumes: `#write` 的响应式宽度与共享主题中各元素的垂直间距、内部 padding 和装饰规则
- Produces: 主要块级元素零水平外边距与不超过正文内容区的统一覆盖

- [ ] **Step 1: 运行修改前检查并确认失败**

```powershell
$css = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
if ($css -match '统一主要块级元素的外部排版基准') { throw 'unexpected pass' }
'RED: block alignment override is absent'
```

预期：输出 `RED: block alignment override is absent`。

- [ ] **Step 2: 创建修改前备份**

```powershell
Copy-Item -LiteralPath 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css' -Destination 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css.bak-20260715-before-block-alignment'
```

- [ ] **Step 3: 添加最小覆盖规则**

```css
/* 统一主要块级元素的外部排版基准 */
#write p,
#write blockquote,
#write ul,
#write ol,
#write dl,
#write table,
#write .md-fences,
#write .md-alert {
  margin-left: 0;
  margin-right: 0;
  max-width: 100%;
  box-sizing: border-box;
}

#write h2,
#write h3,
#write h4,
#write h5,
#write h6 {
  margin-left: 0;
  margin-right: 0;
}

#write .task-list-item {
  max-width: 100%;
  box-sizing: border-box;
}
```

- [ ] **Step 4: 验证目标选择器与声明**

```powershell
$css = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
$targets = @('#write p','#write blockquote','#write ul','#write ol','#write dl','#write table','#write .md-fences','#write .md-alert','#write h2','#write h3','#write h4','#write h5','#write h6','#write .task-list-item')
$missing = $targets | Where-Object { $css -notmatch [regex]::Escape($_) }
[pscustomobject]@{
  AllTargetsPresent = $missing.Count -eq 0
  HorizontalMarginsZero = $css -match '(?s)统一主要块级元素.*?margin-left:\s*0;.*?margin-right:\s*0;'
  MaxWidthProtected = $css -match '(?s)统一主要块级元素.*?max-width:\s*100%;.*?box-sizing:\s*border-box;'
}
```

预期：全部属性为 `True`。

- [ ] **Step 5: 运行回归检查**

```powershell
$css = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat-forest.css'
$shared = Get-Content -Raw 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css'
[pscustomobject]@{
  ResponsiveWidth = $css -match '(?s)^#write\s*\{[^}]*width:\s*90%;[^}]*max-width:\s*var\(--max-width\)'
  Max1200 = $css -match '--max-width:\s*1200px;'
  H1StillCentered = $shared -match '(?s)#write h1\s*\{[^}]*margin:\s*1em auto \.8em;'
  H2FitContent = $shared -match '(?s)#write h2\s*\{[^}]*width:\s*fit-content;'
  Table16Line2 = $css -match '(?s)#write table\s*\{[^}]*font-size:\s*16px;[^}]*line-height:\s*2;'
  Fence16 = $css -match '(?s)\.md-fences\s*\{[^}]*font-size:\s*1rem;'
  JetBrainsMono = $css -match 'font-family:\s*"JetBrains Mono"'
  BracesBalanced = ([regex]::Matches($css,'\{').Count -eq [regex]::Matches($css,'\}').Count)
}
```

预期：全部属性为 `True`。
