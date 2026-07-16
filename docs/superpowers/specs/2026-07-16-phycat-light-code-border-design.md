# Phycat 亮色代码块边框设计

## 目标

为所有引用 `phycat.light.css` 的 Phycat 亮色主题增加清晰但不过分醒目的代码块外边框。

## 设计

- 只修改 `C:\Users\史永康\AppData\Roaming\Typora\themes\phycat\phycat.light.css`。
- 在现有 `.md-fences` 规则中增加 `border: 1px solid var(--element-color-shallow);`。
- 使用各亮色主题已有的 `--element-color-shallow` 变量，使边框自动匹配主题配色；例如 Forest 使用浅绿色。
- 不修改暗色公共主题、Mermaid 排除规则、滚动结构、顶部吸顶栏、圆角、字体、字号、行高、代码高亮和表格样式。

## 验证

- 修改前确认亮色 `.md-fences` 没有该边框声明。
- 修改后确认边框声明只出现一次、CSS 花括号平衡，且长代码块外层滚动和顶部吸顶规则仍然存在。
- 写回前备份原始 `phycat.light.css`，写回后核对文件哈希。
- 在 Typora 重新加载主题后，由用户确认实际视觉效果。
