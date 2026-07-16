# Phycat Forest 调整记录

记录日期：2026-07-15

> 2026-07-15 后续调整：`phycat-forest.css` 已恢复为修改前的原始内容。正文宽度、20px 内边距、块级元素基准、表格工具栏和 Alert 标题等 Forest 专用覆盖均已删除。后续通用布局只在 `phycat.light.css` 与 `phycat.dark.css` 中维护；表格/代码排版规则已迁移至这两个基础文件。

## 表格排版

- 将表格内字体大小从 `14px` 调整为 `16px`。
- 将表格行高从 `1.6` 调整为 `2`。
- 不为表格内的行内代码单独设置字号；继续继承现有的 `.9em`，调整后约为 `14.4px`。

目标规则：

```css
#write table {
  font-size: 16px;
  line-height: 2;
}
```

状态：已于 2026-07-15 迁移至 `phycat.light.css` 和 `phycat.dark.css`，覆盖全部 Phycat 系列主题。

## 正文响应式宽度

- 在 `phycat.light.css` 和 `phycat.dark.css` 中统一使用 `width: auto` 与 `max-width: min(1200px, 90%)`。
- 小屏左右各保留约 `5%`；宽屏正文统一不超过 `1200px`，不再受多数主题原有 `950px` 上限限制。
- 保留现有内边距以及段落、表格、引用、列表和代码块布局。
- HTML 导出同步自适应；带目录的宽屏导出保留原侧栏布局，窗口不超过 `1200px` 时正文恢复居中自适应。
- 不直接设置 `width: 90%`，避免与 Typora 的 `width: inherit` 叠加导致段落二次缩窄。

状态：已于 2026-07-15 在浅色和暗色基础主题中实施，覆盖全部 Phycat 系列主题。

## 块级元素外部排版基准

- 参考 Drake Light，将段落、表格、引用块、围栏代码块、无序/有序列表、定义列表、任务列表和 GitHub Alert 的水平外边距统一为 `0`。
- 二至六级标题从相同左侧基准开始；一级标题继续居中。
- 主要块级元素限制为不超过正文内容区，并使用 `border-box` 计算盒模型。
- 保留标题装饰、列表缩进、任务复选框、引用块、代码块和 Alert 的内部结构与装饰。

状态：已于 2026-07-15 实施。

## 围栏代码块字号

- 将三个反引号创建的围栏代码块从 Typora 默认的 `.9rem`（约 `14.4px`）调整为 `1rem`（`16px`）。
- 围栏代码块和代码编辑器的行高统一为 `1.6`。
- 不改变行内代码的 `.9em` 设置。

目标规则：

```css
.md-fences {
  font-size: 1rem;
}
```

状态：已于 2026-07-15 迁移至 `phycat.light.css` 和 `phycat.dark.css`，覆盖全部 Phycat 系列主题。

## GitHub Alert 标题样式

现状：Alert 的图标和 `Tip`、`Warning`、`Note`、`Important` 等标题外面带有半透明白色胶囊形圆角框。

原因：主题的 `.md-alert-text-container` 明确设置了以下装饰：

```css
background-color: rgba(255, 255, 255, .7);
backdrop-filter: blur(2px);
padding: 2px 10px;
border-radius: 50px;
box-shadow: 0 1px 3px rgba(0, 0, 0, .03);
```

目标：保留整个 Alert 色块的外层圆角，但移除标题区域的白色背景、胶囊圆角、阴影、模糊效果和多余内边距，使图标与标题直接显示在色块背景上，与参考图一致。

状态：已于 2026-07-15 迁移至 `phycat.light.css` 和 `phycat.dark.css`，覆盖全部 Phycat 系列主题；`phycat-forest.css` 中不保留覆盖。

## 表格工具栏间距

现象：选中表格后，左上方的表格工具栏与表格上边缘距离过大。

初步原因：

- `phycat-forest` 的 `#write table` 使用了 `margin: 20px 0`，产生 `20px` 顶部外边距。
- 主题没有为 Typora 的表格工具栏 `.ty-table-edit` 设置位置校正。
- 作为对照，`drake-light` 使用了以下规则缩短工具栏与表格之间的距离：

```css
.ty-table-edit {
  margin-top: -1rem !important;
}
```

计划：实施时先加入与 `drake-light` 相同的工具栏校正规则，并在 Typora 中目视检查；如间距仍偏大，再小幅调整表格顶部外边距。不得影响表格上下文之间的正常段落间距。

状态：已于 2026-07-15 实施；采用 `drake-light` 的 `-1rem` 校正规则。

## 段落与后续表格间距

- 浅色基础主题原来使用段落 `margin: 10px 10px`、表格 `margin: 20px 0`，暗色基础主题则分别为 `0 10px` 和 `0`。
- 将 `phycat.light.css` 的段落和表格外边距直接调整为与 `phycat.dark.css` 相同。
- 删除 `phycat-forest.css` 中原先针对 `p + figure.table-figure` 的单独间距覆盖。
- 所有 Phycat 主题现在统一保留 Typora 表格外层 `figure` 的默认间距。

状态：已于 2026-07-15 在浅色基础主题中完成统一，覆盖全部 Phycat 系列主题。

## 代码字体

- 围栏代码块、代码编辑器、语言标签和行内代码统一优先使用 `JetBrains Mono`。
- 保留 Cascadia Code、Lucida Console、Consolas、Courier 和系统等宽字体作为回退。
- 随主题保存 JetBrains Mono 普通与斜体可变字体，并保留 SIL OFL 1.1 许可证。
- 围栏代码块字号保持 `1rem`（`16px`），行内代码字号保持 `.9em`。

状态：已于 2026-07-15 迁移至 `phycat.light.css` 和 `phycat.dark.css`，覆盖全部 Phycat 系列主题。

## 浅色任务列表对号居中

- 浅色主题原来使用百分比定位和复合平移绘制对号，缩放后容易产生视觉偏移。
- 改为与暗色主题相同的固定几何尺寸和旋转方式，并根据浅色圆圈的偏移量重新定位。
- 只修改浅色主题内部对号，不改变圆圈、任务列表缩进和暗色主题。

状态：已于 2026-07-15 在 `phycat.light.css` 中实施，覆盖全部浅色 Phycat 主题。

## 关闭浅色主题自动编号

- 暗色主题入口文件中的标题和 TOC 自动编号变量原本均已注释。
- 浅色主题中 Cherry、Mauve、Mint、Prussian、Sky 已经关闭；本次补充注释 Caramel、Forest、Sakura 中仍启用的变量。
- 8 个浅色主题与 3 个暗色主题现在均不存在生效的 `--autonum-*` 声明。
- 保留基础文件中的计数器逻辑，后续如需恢复编号，只需重新启用对应主题入口变量。

状态：已于 2026-07-15 完成。

## 表格横向滚动时固定最外层边框

- 在 `phycat.light.css` 与 `phycat.dark.css` 中，将主题边框设置到表格的外层滚动容器上。
- 原生 Markdown 表格使用 `figure.table-figure`；HTML 表格仅匹配 `.md-htmlblock-container:has(table)`，不会影响普通 HTML 块。
- 表格内容和内部网格仍可横向滚动，最外层圆角边框保持在正文区域内不随内容移动。
- 亮色主题沿用 `--element-color-shallow`，暗色主题沿用主色透明混合，未改变表格列宽、字号、行高与内部配色。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-16 完成。

## 表格与代码块滚动条按边框宽度内缩

- 已撤回容器外沿 `clip-path` 以及滑块自身透明边两种方案，避免滑块有色尺寸被永久缩短。
- 表格横向轨道仅在左右按实际外框宽度 `.1em` 内缩。
- 非 Mermaid 代码块纵向轨道仅在上下按实际外框宽度 `2px` 内缩。
- 滑块自身不再添加边框或裁剪，原有粗细、颜色与有色尺寸保持不变；表格内部网格和代码块 Bloom Grid 结构保持不变。
- 修改覆盖 `phycat.light.css` 与 `phycat.dark.css`，并分别保留本次修改前备份。

状态：已于 2026-07-16 完成；实际文件与暂存文件 SHA-256 一致，静态验证失败项为 0。

## 全部 Typora 主题版本管理迁移至 Git

- Git 仓库范围纠正为整个 `C:\Users\史永康\AppData\Roaming\Typora\themes` 目录，而不是单独的 `phycat` 子目录。
- 默认分支为 `main`，全部 118 个正式主题文件和资源均已纳入首次基线提交。
- 根仓库基线提交为 `a4911ba60b47ad3a876d16c2cd5f82eff14d37b2`，提交说明为 `chore: establish Typora themes baseline`。
- 根目录 `.gitignore` 已忽略 `*.bak-*`，避免旧式逐次备份再次进入版本管理。
- 先前 `phycat` 子目录中的 63 个备份与 `themes` 根层剩余的 16 个备份均已删除，共计 79 个；正式主题文件未改变。
- 误建在 `phycat` 子目录的嵌套仓库及其临时迁移元数据均已清理，当前不存在嵌套仓库。
- 后续所有主题调整在验证通过后直接提交到 `themes` 根仓库，不再默认创建独立备份文件。
- 当前仅使用本地 Git 历史，未配置远程仓库，也未向外部推送。

状态：已于 2026-07-16 完成整个 `themes` 目录的 Git 迁移；仓库完整性检查通过，工作区干净。

## 表格滚动前后外框粗细一致

- 保留 Markdown 与 HTML 表格滚动容器的固定 `.1em` 圆角外框。
- 去除表格外围单元格与固定外框重复绘制的左、右、上、下边线，内部 `.1em` 网格线保持不变。
- 横向滚动时由固定容器独立承担可见外框，避免表格位于最左端时两层边线叠加、滚动后又变细。
- 修改覆盖 `phycat.light.css` 与 `phycat.dark.css`，并分别保留本次修改前备份。

状态：已于 2026-07-16 完成；实际文件静态检查与暂存文件 SHA-256 一致性检查均通过。

## 改良 Bloom Grid 代码块结构

- 在共享的 `phycat.light.css` 与 `phycat.dark.css` 中，将非 Mermaid 围栏代码块改为两行 Grid：顶部固定为 32px 样式栏，代码正文位于第二行。
- 顶部样式栏和语言选择框使用粘性定位；长代码滚动时保持在代码块右上方，不随代码正文移走。
- 语言选择框默认隐藏，仅在鼠标悬停代码块或代码块获得焦点时显示。
- 代码正文继续由最外层代码块承担纵向滚动，最大高度为 `70vh`；CodeMirror 内层不再建立独立滚动容器，避免短代码块选中后出现假滚动条。
- 仅借鉴 Bloom 的代码块结构；颜色、边框、JetBrains Mono、16px 字号、语法高亮和亮暗主题变量均继续使用 Phycat 原有设计。
- Mermaid 代码块、表格滚动条及其他组件不在本次结构调整范围内。
- 修改前备份：`phycat.light.css.bak-20260716-before-bloom-grid`、`phycat.dark.css.bak-20260716-before-bloom-grid`。

状态：已于 2026-07-16 写入共享主题文件；静态结构与文件哈希已验证，等待 Typora 中重新加载主题后进行交互确认。

## 代码块外边框微调

- 在共享的 `phycat.light.css` 与 `phycat.dark.css` 中，将 `.md-fences` 最外层边框统一由 `1px` 调整为 `2px`。
- 将亮暗主题的代码块最外层圆角统一为 `10px`；亮色继续使用 `var(--element-color-shallow)`，暗色继续使用原有 `color-mix(...)` 边框颜色。
- Bloom Grid、滚动方式、语言选择框、JetBrains Mono、16px 字号、语法高亮和 Mermaid 均未改变。
- 本次修改前备份：`phycat.light.css.bak-20260716-before-border-2px`、`phycat.dark.css.bak-20260716-before-border-2px`。

状态：已于 2026-07-16 写入共享主题文件。

## 亮色代码块主题边框

- 仅在 `phycat.light.css` 的 `.md-fences` 外层增加 `1px` 实线边框。
- 边框颜色使用 `var(--element-color-shallow)`，由各 Phycat 亮色主题自动提供对应的浅主题色。
- 暗色主题、Mermaid、滚动结构、顶部吸顶栏、圆角、字体、行高、代码高亮和表格样式均未修改。
- 写回前保留 `phycat.light.css.bak-20260716-before-code-border` 备份。

状态：已于 2026-07-16 完成。

## 亮色表格网格线主题化

- 保留 Drake 的完整网格、自动列宽和横向滚动底层结构。
- 将亮色表格网格线从 Drake 固定颜色 `#dfe2e5` 改为 Phycat 变量 `var(--element-color-shallow)`。
- Caramel、Cherry、Forest、Mauve、Mint、Prussian、Sakura、Sky 的表格边框现在分别使用各自主题颜色。
- 暗色表格原本已经由 `var(--primary-color)` 生成网格线，因此无需修改。

状态：已于 2026-07-16 完成，并保留修改前备份。

## Bloom 式长代码块限高显示

- 在 `phycat.light.css` 与 `phycat.dark.css` 中为普通围栏代码块增加 `max-height: 70vh` 和 `overflow: auto`。
- 短代码块仍然完整显示；长代码块超过当前屏幕高度 70% 后改为代码块内部滚动。
- Mermaid 图表通过 `.md-fences:not([lang=mermaid])` 排除，不受代码块限高影响。
- 该功能是 Bloom 使用的限高滚动方式，不包含点击折叠按钮或 JavaScript。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-16 完成。

### 长代码块滚动层级修正

- 初版将 `max-height` 和 `overflow` 设置在整个 `.md-fences`，导致顶部样式栏、语言标签和类型选择框随代码一起滚动。
- 现已恢复外层 `.md-fences` 的可见溢出，仅对内部 `.CodeMirror-scroll` 设置 `max-height: calc(70vh - 32px)` 和纵向滚动。
- 顶部 32px 样式栏及 Typora 控件保持固定，只有代码正文滚动。

状态：已于 2026-07-16 修正，并保留修正前备份。

## 亮色分割线采用 Bloom 结构

- 将 `phycat.light.css` 的 3px 虚线结构替换为 Bloom 亮色系列的中央渐显结构，最终高度设为 3px。
- 渐变由“两端透明—中央主题色—两端透明”构成，中央颜色使用 `var(--element-color-shallow)`，随 8 个亮色 Phycat 主题变化。
- 保留 Phycat 当前 `30px 0` 的上下间距，没有采用 Bloom 的 64px 大间距。
- 关闭原有缩放、透明度变化和 `hr:hover` 动作，常态直接展示最终分割线。
- 暗色共享文件未修改。

状态：已于 2026-07-16 完成，并保留修改前备份。

## 使用 Drake 表格底层重构 Phycat 表格

- 用 `drake-light.css` 的完整单元格边框思路替换 Phycat 原来的“独立外框 + 右/下浅色边线”结构。
- 亮色表格使用 Drake 的 `.1em solid #dfe2e5` 网格线；暗色表格使用与主题主色混合的 `.1em` 网格线。
- 表格显式使用 `border-collapse: collapse`、自动列宽和正常断行，使内部横线、竖线形成连续网格。
- 保留 Phycat 的亮暗配色、表头背景、单元格内边距、字号、行高和悬停效果。
- Markdown 表格外层与 HTML 块容器使用 `overflow-x: auto` 承载超宽表格，避免裁切或直接溢出正文。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-15 完成。

## 浅色标题标记左边缘对齐

- 截图中标题二圆角块与标题四、五、六的标记已经位于正文左侧基准线。
- 实际偏移来自标题三竖线的绝对定位 `left: -6px`。
- 将 `phycat.light.css` 的 `#write h3::before` 调整为 `left: 0`，使标题二圆角块与标题三至六的装饰标记左边缘统一。
- 标题二的圆角背景、文字内边距、颜色和尺寸均未改变。

状态：已于 2026-07-15 完成，并保留修改前备份。

## 宽表格自动布局与列裁切修复

- Phycat 原表格同时使用 `width: 100%`、`overflow: hidden` 和表头 `white-space: nowrap`，宽 HTML 表格的后续列可能被推到可视区域外并裁切。
- 参考 `drake-light.css`，在 `phycat.light.css` 与 `phycat.dark.css` 中启用 `table-layout: auto` 和 `word-break: initial`。
- 将表格裁切改为 `overflow: visible`，并将表头改为 `white-space: normal`，允许浏览器根据正文宽度正常分配列宽和换行。
- 保留原有表格字号、行高、配色、边框、圆角、内边距和悬停样式。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-15 完成。

## 高亮文本直接显示最终样式

- 将 `phycat.light.css` 与 `phycat.dark.css` 中高亮文本原来的悬停最终效果合并到常态 `#write mark`。
- 高亮底纹常态直接铺满，不再从 40% 动画扩展到 100%。
- 关闭高亮的 `transition`，并注释所有生效的 `#write mark:hover` 规则。
- 亮色主题中高亮内部加粗文字的两个悬停规则也已注释，最终样式改为常态显示。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-15 完成。

## Radiation 代码注释选中对比度

- `phycat-radiation.css` 的注释色最终由 `#546e7a` 调整为中性灰 `#999999`。
- 保留代码选中背景 `#447344`，不改变其他语法颜色或其他 Phycat 主题。
- `#999999` 相对普通代码背景的对比度为 5.00:1；选中状态以 Typora 中的实际显示效果和用户目视确认为准。

状态：已于 2026-07-15 完成，并保留修改前备份。

## 关闭加粗与行内代码悬停动作

- 在 `phycat.light.css` 与 `phycat.dark.css` 中整体注释 `#write strong:hover` 规则。
- 同时整体注释 `#write code:not(.md-fencescode):hover` 规则。
- 鼠标移到加粗文字或行内代码时，不再变色、出现下划线、缩放或发光；元素的常态样式保持不变。
- 修改覆盖全部 Phycat 亮色与暗色主题，并为两个共享文件分别保留修改前备份。

状态：已于 2026-07-15 完成。

## 表格横向滚动条细化

- 在 `phycat.light.css` 与 `phycat.dark.css` 中为原生 Markdown 表格和 HTML 表格增加局部滚动条尺寸规则。
- 将两类表格的横向滚动条高度设为 `5px`，与代码块的细滚动条观感保持一致。
- 滑块颜色、轨道、圆角与交互状态继续沿用各主题现有规则。
- 未改变表格固定外框、宽度、内部网格、字号或行高，并保留两个共享文件的修改前备份。

状态：已于 2026-07-16 完成。

## 代码块外层自适应滚动

- 核对 Typora 1.9.5 自带 `codemirror.css` 后确认，CodeMirror 的内部 `.CodeMirror-scroll` 由编辑器脚本参与高度计算，不适合作为主题新增的原生纵向滚动容器。
- 将内部层强制设置为 `overflow-y: auto` 会产生少量脚本尺寸补偿，即使代码较短也会出现假滚动条。
- 在 `phycat.light.css` 与 `phycat.dark.css` 中移除内部滚动规则，改由非 Mermaid `.md-fences` 外层使用 `max-height: 70vh` 和 `overflow-y: auto`。
- 顶部 32px 样式栏使用 `position: sticky; top: 0`，长代码滚动时保持在顶部；边框仍属于最外层，不会随正文移动。
- 短代码块按内容自然高度显示，不产生内部假滚动；长代码块由已验证可响应滚轮的外层滚动。
- Mermaid、代码字号、字体、行高、颜色以及表格样式均未改变，并保留两个共享文件的修改前备份。

状态：已于 2026-07-16 完成。

## 左侧实时大纲迁移 mdmdt 树状结构

- 将 `phycat.light.css` 与 `phycat.dark.css` 的实时左侧大纲原位替换为 mdmdt 的树状连接线、层级缩进、展开箭头和节点几何结构。
- 实时大纲容器与标题标签字号统一调整为 `16px`。
- 悬停与当前项交互同步采用 mdmdt 方式，移除实时大纲原有胶囊、星标、闪烁动画、边框和发光阴影。
- Phycat 强调背景为半透明色，因此仅由节点容器绘制一层背景；标签层保持透明，避免重叠区域产生第二种深色。
- 亮色继续使用 Phycat 的 `--element-color-*` 变量，暗色继续使用 `--primary-color` 透明混合；未引入 mdmdt 配色。
- 保留大纲筛选、搜索命中和无折叠模式兼容规则；文件树、导出侧栏和侧栏宽度不变。
- 为两个共享文件分别保留修改前备份。

状态：已于 2026-07-16 完成；实际文件与暂存文件 SHA-256 一致，静态验证失败项为 0。
