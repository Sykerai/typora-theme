# Phycat 代码块自动高度设计

## 目标

消除强制修改 CodeMirror 内部滚动层所产生的假竖向滚动条，同时保留长代码块折叠、固定工具栏和固定语言类型标签。

## 设计

- 修改共享文件 `phycat.light.css` 与 `phycat.dark.css`，覆盖全部 Phycat 亮色和暗色主题。
- Typora 自带 `.CodeMirror { height: auto }`，不再添加重复的自动高度规则。
- 不使用 `.cm-s-inner.CodeMirror` 外层滚动，因为 CodeMirror 会接管并重置该外层的滚动位置。
- 对非 Mermaid 围栏代码块中的 `.CodeMirror-scroll` 同时设置 `height: auto`、`max-height: calc(70vh - 32px)` 与 `overflow-y: auto !important`。
- `height: auto` 覆盖 Typora 内部滚动层的 `height: 100%`，避免短代码产生假溢出；滚动仍发生在 CodeMirror 实际接收滚轮事件的内部层。
- Mermaid、表格滚动条、代码字号、代码字体、行高、颜色和工具栏布局不变。

## 验证

- 两个共享文件中 `.CodeMirror-scroll` 规则各出现一次，同时包含 `height: auto`、目标限高和 `overflow-y: auto !important`。
- 两个共享文件中不再存在 `.cm-s-inner.CodeMirror` 外层滚动规则。
- CSS 花括号平衡，修改前文件保留备份。
