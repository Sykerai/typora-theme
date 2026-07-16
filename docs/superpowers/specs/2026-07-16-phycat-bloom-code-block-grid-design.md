# Phycat 改良 Bloom 代码块结构设计

## 目标

将 Bloom 系列代码块的容器与顶部工具区结构迁移到 Phycat 公共主题，同时保留 Phycat 的视觉体系，并解决短代码块选中时出现滚动条、顶部样式随正文滚动以及语言选择框随正文滚动的问题。

## 范围

- 修改 `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`。
- 修改 `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.dark.css`。
- 仅作用于非 Mermaid 围栏代码块。
- 不修改各个 `phycat-*.css` 主题入口文件。

## 结构

- 非 Mermaid `.md-fences` 使用两行 CSS Grid：第一行是 32px 顶部工具区，第二行是 CodeMirror 正文区。
- 代码块外层继续使用 `max-height: 70vh`；只有整体内容超过限制时才出现纵向滚动。
- 顶部伪元素与 Typora 的 `.code-tooltip` 语言选择框位于同一个 Grid 顶部区域，并使用吸顶定位，在长代码滚动时保持在顶部。
- `.CodeMirror` 放入第二行；`.CodeMirror-scroll` 沿用 Bloom 的透明、可见内部层结构，不再承担新增纵向滚动。
- Mermaid 保持当前结构，不进入 Grid 和限高规则。

## 顶部栏与语言选择框

- 顶部栏保留 Phycat 现有的圆点、背景、边框和圆角风格。
- 不再通过 `attr(lang)` 常驻显示语言类型。
- 语言选择框采用 Bloom 行为：默认隐藏，只在代码块 `:hover` 或 `:focus-within` 时显示。
- 语言选择框固定在顶部区域右侧，不使用 Typora 原生的 `bottom: -2.5em`，因此不会制造短代码块底部假溢出。
- 显示与隐藏只改变透明度和交互状态，不改变代码块高度。

## Phycat 视觉保留

- 代码字体继续使用 JetBrains Mono。
- 围栏代码字号继续为 16px，现有行高保持不变。
- 亮色主题继续使用 `--element-color*` 变量；暗色主题继续使用 `--primary-color`、`--secondary-color` 及现有代码色变量。
- 保留亮色代码块主题色边框、暗色边框、现有圆角和语法高亮。
- 不引入 Bloom 的琥珀配色、阴影、字体大小、行高或悬停动画。
- 不修改表格、行内代码、HTML 块、YAML、引用块或其他组件。

## 验证

- 修改前建立失败检查，确认 Grid 顶部区和顶部语言框规则尚不存在。
- 修改后检查亮、暗文件各自仅包含一套新结构，旧的 `attr(lang)` 常驻标签和底部语言框定位被正确覆盖。
- 检查 CSS 花括号平衡，并确认 JetBrains Mono、16px 围栏字号、70vh 限高、亮暗边框、5px 表格滚动条及 Mermaid 排除规则仍然存在。
- 写回前分别备份亮、暗公共主题，写回后核对 SHA256 哈希。
- 在 Typora 中人工验证四种状态：短代码未选中、短代码选中、长代码滚动、鼠标悬停显示语言选择框。

## 回退

若 Grid 结构影响 Typora 编辑、语言切换或 CodeMirror 排版，则使用本次写回前的两个备份恢复亮、暗公共主题，不继续叠加补丁。
