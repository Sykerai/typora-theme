# Phycat Bloom Grid Code Block Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current Phycat fenced-code layout with a two-row Bloom-inspired Grid whose top bar and language selector remain fixed while long code scrolls.

**Architecture:** Non-Mermaid `.md-fences` becomes a two-row scrollable Grid: a 32px sticky top row and a CodeMirror body row. The existing Phycat pseudo-header occupies the top row, Typora's dynamically inserted `.code-tooltip` overlays that same row, and CodeMirror's internal scroll layer is made visible as in Bloom so it does not create an independent vertical scrollbar.

**Tech Stack:** Typora 1.9.5 CSS, CodeMirror, CSS Grid and sticky positioning, PowerShell structural assertions.

## Global Constraints

- Modify only `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css` and `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`.
- Apply the new structure only to non-Mermaid fenced code blocks.
- Keep `max-height: 70vh` and show vertical scrolling only when the full code block exceeds that height.
- Keep JetBrains Mono, 16px fenced-code text, current line heights, current Phycat token colors, light and dark borders, and the 5px table scrollbar.
- Keep the language selector hidden by default and visible only on `:hover` or `:focus-within`.
- Do not copy Bloom colors, shadows, font sizes, line heights, or hover animation.
- Back up both shared CSS files before writing.

---

### Task 1: Stage both shared themes and prove the Grid structure is absent

**Files:**
- Read: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Read: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`
- Create temporarily: `D:\史永康\Documents\phycat.light.bloom-grid.css`
- Create temporarily: `D:\史永康\Documents\phycat.dark.bloom-grid.css`

**Interfaces:**
- Consumes: the current verified shared Phycat CSS files.
- Produces: hash-identical staged copies for isolated editing.

- [ ] **Step 1: Copy and hash-check the current files**

Run:

```powershell
$light = 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css'
$dark = 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css'
$lightStage = 'D:\史永康\Documents\phycat.light.bloom-grid.css'
$darkStage = 'D:\史永康\Documents\phycat.dark.bloom-grid.css'
Copy-Item -LiteralPath $light -Destination $lightStage -Force
Copy-Item -LiteralPath $dark -Destination $darkStage -Force
if ((Get-FileHash $light).Hash -ne (Get-FileHash $lightStage).Hash) { exit 1 }
if ((Get-FileHash $dark).Hash -ne (Get-FileHash $darkStage).Hash) { exit 1 }
```

Expected: exit code 0 and both staged hashes match their sources.

- [ ] **Step 2: Run the failing structural assertion**

Run:

```powershell
$files = @('D:\史永康\Documents\phycat.light.bloom-grid.css', 'D:\史永康\Documents\phycat.dark.bloom-grid.css')
foreach ($file in $files) {
    $css = Get-Content -Raw -LiteralPath $file
    $hasGrid = [regex]::IsMatch($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)\s*\{[^}]*display:\s*grid;[^}]*grid-template-rows:\s*32px auto')
    $hasStickyTooltip = [regex]::IsMatch($css, '(?s)>\s*\.code-tooltip\s*\{[^}]*position:\s*sticky;[^}]*grid-row:\s*1')
    if (-not ($hasGrid -and $hasStickyTooltip)) { exit 1 }
}
```

Expected: exit code 1 because the current themes do not yet have the Grid and sticky language-selector structure.

---

### Task 2: Implement the light-theme Grid and top tools

**Files:**
- Modify: `D:\史永康\Documents\phycat.light.bloom-grid.css:1061-1106`

**Interfaces:**
- Consumes: Phycat light variables including `--element-color-shallow` and `--element-color-deep`.
- Produces: the light-theme version of the shared Grid contract.

- [ ] **Step 1: Replace the light pseudo-header and outer scrolling rules**

Apply these exact changes; all unshown declarations in the existing pseudo-header remain unchanged:

```css
.md-fences:not([lang=mermaid])::before {
    /* Replace: content: attr(lang); */
    content: "";
    grid-column: 1;
    grid-row: 1;
}

.md-fences:not([lang=mermaid]) {
    /* Add the first three declarations to the existing rule. */
    display: grid;
    grid-template-columns: minmax(0, 1fr);
    grid-template-rows: 32px auto;
    max-height: 70vh;
    overflow-x: hidden;
    overflow-y: auto
}
```

- [ ] **Step 2: Add the light CodeMirror and language-selector placement**

Add after the outer Grid rule:

```css
#write .md-fences:not([lang=mermaid]) > .cm-s-inner.CodeMirror {
    grid-column: 1;
    grid-row: 2;
    min-width: 0
}

#write .md-fences:not([lang=mermaid]) .CodeMirror-scroll {
    overflow: visible !important
}

#write .md-fences:not([lang=mermaid]) > .code-tooltip {
    grid-column: 1;
    grid-row: 1;
    position: sticky;
    top: 0;
    align-self: start;
    justify-self: end;
    z-index: 8;
    display: inline-flex;
    align-items: center;
    height: 32px;
    margin: 0;
    padding: 0 12px;
    min-width: 0;
    bottom: auto !important;
    right: auto;
    left: auto;
    float: none !important;
    opacity: 0;
    pointer-events: none;
    transform: none;
    background: transparent;
    box-shadow: none
}

#write .md-fences:not([lang=mermaid]):hover > .code-tooltip,
#write .md-fences:not([lang=mermaid]):focus-within > .code-tooltip {
    opacity: 1;
    pointer-events: auto
}

#write .md-fences:not([lang=mermaid]) > .code-tooltip .ty-cm-lang-input {
    min-width: 140px;
    padding: 0 6px;
    line-height: 24px;
    color: var(--element-color-deep);
    background: #f8f8f8;
    border: 1px solid var(--element-color-shallow);
    border-radius: 4px;
    text-align: center
}
```

- [ ] **Step 3: Verify the light staged result**

Run:

```powershell
$css = Get-Content -Raw -LiteralPath 'D:\史永康\Documents\phycat.light.bloom-grid.css'
$checks = @(
    ([regex]::Matches($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)\s*\{[^}]*display:\s*grid;[^}]*grid-template-rows:\s*32px auto').Count -eq 1),
    ([regex]::Matches($css, '(?s)>\s*\.code-tooltip\s*\{[^}]*grid-row:\s*1;[^}]*position:\s*sticky').Count -eq 1),
    (-not [regex]::IsMatch($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)::before\s*\{[^}]*content:\s*attr\(lang\)')),
    ([regex]::Matches($css, '\{').Count -eq [regex]::Matches($css, '\}').Count),
    $css.Contains('font-family: "JetBrains Mono"'),
    $css.Contains('font-size: 16px'),
    $css.Contains('border: 1px solid var(--element-color-shallow)'),
    $css.Contains('max-height: 70vh'),
    [regex]::IsMatch($css, '(?s)table-figure::\-webkit-scrollbar[^}]*height:\s*5px')
)
if ($checks -contains $false) { exit 1 }
```

Expected: every assertion is `True` and the command exits with code 0.

---

### Task 3: Implement the dark-theme Grid and top tools

**Files:**
- Modify: `D:\史永康\Documents\phycat.dark.bloom-grid.css:1366-1430`

**Interfaces:**
- Consumes: Phycat dark variables including `--primary-color`, `--secondary-color`, and the existing dark code colors.
- Produces: the dark-theme version of the same shared Grid contract.

- [ ] **Step 1: Replace the dark pseudo-header and outer scrolling rules**

Apply these exact changes; all unshown declarations in the existing pseudo-header remain unchanged:

```css
.md-fences:not([lang=mermaid])::before {
    /* Replace: content: attr(lang); */
    content: "";
    grid-column: 1;
    grid-row: 1;
}

.md-fences:not([lang=mermaid]) {
    /* Add the first three declarations to the existing rule. */
    display: grid;
    grid-template-columns: minmax(0, 1fr);
    grid-template-rows: 32px auto;
    max-height: 70vh;
    overflow-x: hidden;
    overflow-y: auto
}
```

- [ ] **Step 2: Add the dark CodeMirror and language-selector placement**

Add after the outer Grid rule:

```css
#write .md-fences:not([lang=mermaid]) > .cm-s-inner.CodeMirror {
    grid-column: 1;
    grid-row: 2;
    min-width: 0
}

#write .md-fences:not([lang=mermaid]) .CodeMirror-scroll {
    overflow: visible !important
}

#write .md-fences:not([lang=mermaid]) > .code-tooltip {
    grid-column: 1;
    grid-row: 1;
    position: sticky;
    top: 0;
    align-self: start;
    justify-self: end;
    z-index: 8;
    display: inline-flex;
    align-items: center;
    height: 32px;
    margin: 0;
    padding: 0 12px;
    min-width: 0;
    bottom: auto !important;
    right: auto;
    left: auto;
    float: none !important;
    opacity: 0;
    pointer-events: none;
    transform: none;
    background: transparent;
    box-shadow: none
}

#write .md-fences:not([lang=mermaid]):hover > .code-tooltip,
#write .md-fences:not([lang=mermaid]):focus-within > .code-tooltip {
    opacity: 1;
    pointer-events: auto
}

#write .md-fences:not([lang=mermaid]) > .code-tooltip .ty-cm-lang-input {
    min-width: 140px;
    padding: 0 6px;
    line-height: 24px;
    color: var(--primary-color);
    background: #282a36;
    border: 1px solid color-mix(in srgb, var(--secondary-color), transparent 70%);
    border-radius: 4px;
    text-align: center
}
```

- [ ] **Step 3: Verify the dark staged result**

Run:

```powershell
$css = Get-Content -Raw -LiteralPath 'D:\史永康\Documents\phycat.dark.bloom-grid.css'
$checks = @(
    ([regex]::Matches($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)\s*\{[^}]*display:\s*grid;[^}]*grid-template-rows:\s*32px auto').Count -eq 1),
    ([regex]::Matches($css, '(?s)>\s*\.code-tooltip\s*\{[^}]*grid-row:\s*1;[^}]*position:\s*sticky').Count -eq 1),
    (-not [regex]::IsMatch($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)::before\s*\{[^}]*content:\s*attr\(lang\)')),
    ([regex]::Matches($css, '\{').Count -eq [regex]::Matches($css, '\}').Count),
    $css.Contains('font-family: "JetBrains Mono"'),
    $css.Contains('font-size: 16px'),
    $css.Contains('border: 1px solid color-mix(in srgb, var(--secondary-color), transparent 80%)'),
    $css.Contains('max-height: 70vh'),
    [regex]::IsMatch($css, '(?s)table-figure::\-webkit-scrollbar[^}]*height:\s*5px')
)
if ($checks -contains $false) { exit 1 }
```

Expected: every assertion is `True` and the command exits with code 0.

---

### Task 4: Back up, write, verify, and hand off for Typora visual testing

**Files:**
- Modify externally: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Modify externally: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`
- Update: `D:\史永康\Documents\docs\typora\phycat-forest-pending-changes.md`

**Interfaces:**
- Consumes: the two verified staged CSS files.
- Produces: backed-up shared themes and a documented rollback point.

- [ ] **Step 1: Create exact backups and write the staged files**

Create:

```text
C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css.bak-20260716-before-bloom-grid
C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css.bak-20260716-before-bloom-grid
```

Then copy each staged file over its matching shared theme.

- [ ] **Step 2: Run the full external structural verification**

Run:

```powershell
$pairs = @(
    @('C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css', 'D:\史永康\Documents\phycat.light.bloom-grid.css'),
    @('C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css', 'D:\史永康\Documents\phycat.dark.bloom-grid.css')
)
$all = $true
foreach ($pair in $pairs) {
    $target = $pair[0]
    $stage = $pair[1]
    $css = Get-Content -Raw -LiteralPath $target
    $checks = @(
        ((Get-FileHash $target).Hash -eq (Get-FileHash $stage).Hash),
        ([regex]::Matches($css, '\{').Count -eq [regex]::Matches($css, '\}').Count),
        ([regex]::Matches($css, '(?s)\.md-fences:not\(\[lang=mermaid\]\)\s*\{[^}]*display:\s*grid;[^}]*grid-template-rows:\s*32px auto').Count -eq 1),
        ([regex]::Matches($css, '(?s)>\s*\.code-tooltip\s*\{[^}]*grid-row:\s*1;[^}]*position:\s*sticky').Count -eq 1),
        [regex]::IsMatch($css, '(?s)>\s*\.code-tooltip\s*\{[^}]*opacity:\s*0;[^}]*pointer-events:\s*none'),
        [regex]::IsMatch($css, '(?s):hover\s*>\s*\.code-tooltip,.*?:focus-within\s*>\s*\.code-tooltip\s*\{[^}]*opacity:\s*1;[^}]*pointer-events:\s*auto'),
        $css.Contains('font-family: "JetBrains Mono"'),
        $css.Contains('font-size: 16px'),
        [regex]::IsMatch($css, '(?s)table-figure::\-webkit-scrollbar[^}]*height:\s*5px')
    )
    if ($checks -contains $false) { $all = $false }
}
if (-not (Test-Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css.bak-20260716-before-bloom-grid')) { $all = $false }
if (-not (Test-Path 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css.bak-20260716-before-bloom-grid')) { $all = $false }
if (-not $all) { exit 1 }
```

Expected: all checks pass with exit code 0.

- [ ] **Step 3: Update the change log and remove only staged files**

Document the Grid structure, Bloom source, Phycat visual mapping, tooltip visibility behavior, backup names, and manual verification states in `D:\史永康\Documents\docs\typora\phycat-forest-pending-changes.md`. Remove only:

```text
D:\史永康\Documents\phycat.light.bloom-grid.css
D:\史永康\Documents\phycat.dark.bloom-grid.css
```

- [ ] **Step 4: Request Typora interaction verification**

Ask the user to reload the theme and verify:

1. Short code unselected: no vertical scrollbar and no language selector.
2. Short code hovered or focused: language selector appears at the top-right without creating a scrollbar.
3. Long code scrolled: top background and language selector remain fixed.
4. Language editing works and Mermaid remains unchanged.

If any of these fail, restore both `before-bloom-grid` backups instead of adding further CSS patches.

No commit step is included because `D:\史永康\Documents` is not a Git repository.
