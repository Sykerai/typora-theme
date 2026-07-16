# Phycat Light Code Border Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a 1px theme-aware outer border to fenced code blocks in all Phycat light themes.

**Architecture:** Modify only the shared light-theme `.md-fences` rule. Use the existing `--element-color-shallow` variable so each importing light theme supplies its own border color without per-theme overrides.

**Tech Stack:** Typora CSS, PowerShell verification, Windows filesystem.

## Global Constraints

- Modify only `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`.
- Add exactly `border: 1px solid var(--element-color-shallow);` to `.md-fences`.
- Do not modify the dark shared theme, Mermaid behavior, scrolling, sticky header, border radius, fonts, line height, syntax highlighting, or tables.
- Back up the external theme file before writing.

---

### Task 1: Add the light-theme code-block border

**Files:**
- Modify: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css:1086`
- Reference: `D:\史永康\Documents\docs\superpowers\specs\2026-07-16-phycat-light-code-border-design.md`

**Interfaces:**
- Consumes: `--element-color-shallow` supplied by each Phycat light theme.
- Produces: a single outer border declaration on `.md-fences`.

- [ ] **Step 1: Stage the current shared light CSS**

Run:

```powershell
Copy-Item -LiteralPath 'C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css' -Destination 'D:\史永康\Documents\phycat.light.code-border.css' -Force
```

Expected: the staged file exists and initially matches the external file hash.

- [ ] **Step 2: Run the failing border assertion**

Run:

```powershell
$css = Get-Content -Raw -LiteralPath 'D:\史永康\Documents\phycat.light.code-border.css'
if (-not [regex]::IsMatch($css, '(?s)\.md-fences\s*\{[^}]*border:\s*1px solid var\(--element-color-shallow\)')) { exit 1 }
```

Expected: exit code 1 because the light `.md-fences` rule has no outer border yet.

- [ ] **Step 3: Add the minimal declaration**

Change the staged `.md-fences` rule to:

```css
.md-fences {
    position: relative;
    z-index: 1;
    overflow: visible;
    border: 1px solid var(--element-color-shallow)
}
```

- [ ] **Step 4: Run the passing assertions**

Run assertions confirming that the border occurs exactly once, CSS braces are balanced, the outer scrolling rule still occurs once, the sticky header rule still occurs once, and the 5px table scrollbar rule remains present.

Expected: all assertions return `True` and the command exits with code 0.

- [ ] **Step 5: Back up and write the external theme file**

Create:

```text
C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css.bak-20260716-before-code-border
```

Then copy the verified staged file to `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`.

- [ ] **Step 6: Verify the external result and clean staging**

Confirm the external file matches the staged SHA256 hash, the backup exists, and the five structural assertions from Step 4 still pass. Remove only `D:\史永康\Documents\phycat.light.code-border.css` after verification.

Expected: external and staged hashes match, all assertions pass, and the staged file is removed.

No commit step is included because `D:\史永康\Documents` is not a Git repository.
