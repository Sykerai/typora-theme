# Phycat Radiation 代码注释对比度设计

## 目标

提高 `phycat-radiation` 围栏代码块中注释文字在普通背景和选中背景上的可读性。

## 设计

- 仅修改 `phycat-radiation.css` 中的 `--code-comment` 变量。
- 最终按 Typora 实际显示效果，将颜色由 `#546e7a` 调整为 `#999999`。
- 保留 `--code-selected-bg: #447344`，不改变选区视觉，也不影响其他 Phycat 主题。
- 修改前创建原文件备份；修改后检查变量唯一性、CSS 大括号平衡，以及注释色相对普通背景和选中背景的 WCAG 对比度。

## 验收标准

- `--code-comment` 的生效值为 `#999999`。
- 在 Typora 的普通状态与绿色选中状态下均能按用户确认的视觉效果辨认。
- 其他主题文件保持不变。
