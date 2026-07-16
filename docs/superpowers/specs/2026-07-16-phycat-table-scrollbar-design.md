# Phycat 表格细滚动条设计

## 目标

让 Phycat 原生 Markdown 表格和 HTML 表格的横向滚动条与代码块的细滚动条观感一致，同时保持表格外层边框固定。

## 设计

- 修改共享文件 `phycat.light.css` 与 `phycat.dark.css`，覆盖全部 Phycat 亮色和暗色主题。
- 分别匹配 `figure.table-figure` 与 `.md-htmlblock-container:has(table)` 的 `::-webkit-scrollbar`。
- 只设置横向滚动条高度为 `5px`；不重定义滑块、轨道颜色和交互状态，使其继续继承各主题已有样式。
- 不改变表格宽度、外框、内部网格、字号、行高或代码块样式。

## 验证

- 两个共享文件中目标规则各出现一次。
- 规则同时覆盖原生表格和 HTML 表格，且高度为 `5px`。
- CSS 花括号数量保持平衡，修改前文件保留备份。

