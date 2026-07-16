# Phycat Forest 块级元素外部排版基准设计

## 问题

Phycat Forest 对不同块级元素分别设置了水平外边距。例如普通段落使用 `margin: 10px 10px`，而表格和引用块使用水平外边距 `0`。这会让正文段落、表格、代码块和其他内容的外部排版基准不一致。

## 目标

- 参考 Drake Light，以 `#write` 内容区作为主要块级元素的统一水平排版基准。
- 普通段落、二至六级标题、表格、引用块、围栏代码块、无序列表、有序列表、定义列表、任务列表和 GitHub Alert 不再增加水平外边距。
- 一级标题继续居中。
- 保留标题装饰的 `fit-content` 宽度，不将标题色块强制拉满。
- 保留引用块、代码块、列表、任务复选框和 Alert 的内部缩进、图标及装饰。
- 不改变正文容器的 `width: 90%`、`max-width: 1200px` 和居中规则。
- 不改变打印规则、表格字号、代码字号和 JetBrains Mono 字体。

## 设计

仅在 `phycat-forest.css` 中增加主题专属覆盖，不修改共享的 `phycat.light.css`：

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

一级标题不加入覆盖，以保留 `margin-left/right: auto` 的居中效果。任务列表通过外层 `ul` 统一外部边界，任务项规则仅防止内容越界，不改变复选框的位置和动画。

## 验证

- 所有目标元素均使用零水平外边距或一级标题的居中例外。
- 表格、引用块、代码块和 Alert 外框不超过 `#write` 内容区。
- 普通列表和任务列表保留原有缩进，任务复选框不越过正文内容区。
- 二、三级标题的装饰仍为 `fit-content`，一级标题仍居中。
- 正文响应式宽度、HTML 导出、打印、表格排版及代码字体规则保持不变。
