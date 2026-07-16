# Phycat Table Scroll Border Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make the fixed Phycat table frame keep one consistent thickness before and after horizontal scrolling.

**Architecture:** Keep the existing bordered scrolling container as the sole outer frame. Add matching light/dark structural selectors that suppress only the table cells' four perimeter borders while preserving all internal collapsed-grid borders.

**Tech Stack:** Typora CSS, PowerShell static assertions, `apply_patch`

## Global Constraints

- Modify only the shared `phycat.light.css` and `phycat.dark.css` files through workspace staging copies.
- Preserve theme-specific colors, internal `.1em` grid lines, `overflow-x: auto`, and the existing `8px` frame radius.
- Do not modify individual Phycat entry themes or unrelated table behavior.
- Create separate backups immediately before writing the actual shared files.
- Execute inline in the current task without subagents or additional approval checkpoints.

---

### Task 1: Remove duplicated table perimeter borders

**Files:**
- Create: `D:\史永康\Documents\phycat-table-border-stage\phycat.light.css`
- Create: `D:\史永康\Documents\phycat-table-border-stage\phycat.dark.css`
- Modify after verification: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css:1334`
- Modify after verification: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css:1654`

**Interfaces:**
- Consumes: Typora-generated table sections (`thead`, `tbody`, optional `tfoot`) inside the existing scrolling wrapper.
- Produces: One fixed outer frame plus unchanged internal collapsed-grid lines.

- [x] **Step 1: Create staging copies and run the failing static assertion**

Copy both actual shared CSS files to the staging directory. Assert that both staging files contain `/* 固定外框独立承担表格外围边线 */`; expected result before implementation: FAIL because the marker and perimeter selectors do not exist.

- [x] **Step 2: Add the minimal perimeter rules to both staging files**

Insert after the shared `td`/`th` border rule:

```css
/* 固定外框独立承担表格外围边线 */
#write table tr > :first-child {
    border-left: 0
}

#write table tr > :last-child {
    border-right: 0
}

#write table > thead:first-of-type > tr:first-child > *,
#write table:not(:has(> thead)) > tbody:first-of-type > tr:first-child > * {
    border-top: 0
}

#write table > tfoot:last-of-type > tr:last-child > *,
#write table:not(:has(> tfoot)) > tbody:last-of-type > tr:last-child > *,
#write table:not(:has(> tbody, > tfoot)) > thead:last-of-type > tr:last-child > * {
    border-bottom: 0
}
```

- [x] **Step 3: Run green static verification**

Assert in both staging files that the marker appears exactly once; the outer container still has `overflow-x: auto`, `.1em` border, and `8px` radius; `td`/`th` still have `.1em` borders; and opening/closing brace counts match. Expected result: PASS for both staging files.

- [x] **Step 4: Back up and write actual shared files**

Create timestamped `before-table-scroll-border` backups beside both actual files, then copy the verified staging files into place.

- [x] **Step 5: Verify actual files byte-for-byte and statically**

Compare each installed CSS file with its staging source by SHA-256. Repeat the marker-count, preserved-rule, and brace-balance assertions against the installed files. Expected result: both hashes match and all assertions PASS.

- [x] **Step 6: Hand off interactive verification**

Tell the user to switch themes or restart Typora, then horizontally scroll a wide table and confirm the left frame keeps the same apparent thickness while internal grid lines remain continuous.
