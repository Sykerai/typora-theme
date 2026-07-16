# Phycat mdmdt Outline Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the live Phycat outline sidebar with mdmdt's tree structure and interaction while preserving Phycat light/dark colors.

**Architecture:** Replace the existing live-outline block in each shared CSS file in place. Use identical structural selectors in light and dark, with only connector, hover, and active colors mapped to the corresponding Phycat variables.

**Tech Stack:** Typora CSS, PowerShell static assertions, `apply_patch`

## Global Constraints

- Modify only `phycat.light.css` and `phycat.dark.css` through workspace staging copies.
- Do not modify export-sidebar, file-tree, search-list, sidebar-width,正文, table, or code-block rules.
- Remove the old capsule, star, animation, active border, and glow rules from the live outline block.
- Preserve Phycat light/dark color variables and create separate backups before installation.
- Execute inline without subagents or additional approval checkpoints.

---

### Task 1: Replace the live outline block

**Files:**
- Create: `D:\史永康\Documents\phycat-mdmdt-outline-stage\phycat.light.css`
- Create: `D:\史永康\Documents\phycat-mdmdt-outline-stage\phycat.dark.css`
- Modify after verification: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`
- Modify after verification: `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`

**Interfaces:**
- Consumes: Typora `.outline-content`, `.outline-item`, `.outline-children`, `.outline-expander`, active, filter, and search-hit DOM classes.
- Produces: mdmdt-compatible tree connectors, indentation, expanders, hover, and active behavior with Phycat colors.

- [x] **Step 1: Create staging copies and run the failing assertion**

Assert that the staging files do not yet contain `/* mdmdt 树状大纲结构，保留 Phycat 配色 */`, still contain `outline-twinkle`, and do not contain the mdmdt child-connector selector. Expected: FAIL.

- [x] **Step 2: Replace the old live-outline blocks**

Replace from the old `#outline-content .outline-h2::after` reset through the live `outline-twinkle`/star rules. Insert the mdmdt selectors for content padding, nested lists, items, expanders, labels, L-shaped node connectors, child vertical connectors, first/last-node corrections, hover/active geometry, filter mode, and search-hit mode. Map colors exactly as specified in the design document.

- [x] **Step 3: Run staging verification**

Assert marker count one, identical structural-selector counts, theme-specific color mappings, removal of `outline-twinkle`, star content, `border-radius: 50px`, and live-outline glow, preservation of export-sidebar rules, and balanced braces. Expected: PASS for both files.

- [x] **Step 4: Back up and install**

Create timestamped `before-mdmdt-outline` backups beside each actual shared file, then copy the verified staging files into place.

- [x] **Step 5: Verify installed files**

Compare staging and installed SHA-256 hashes and repeat all structural, removal, preservation, color, and brace assertions. Expected: both hashes match and zero failures.

- [x] **Step 6: Hand off interactive verification**

Ask the user to restart or switch theme, then test multi-level expansion, hover, current-title changes, outline filtering, search hits, long-label clipping, and outline scrolling.
