# series-skin，Tufte 风格中文教材皮肤

叠加在本仓库 `elegantbook.cls` 之上的中文教材排版层。类文件承担 monochrome 印刷色、封面版式、页脚钩子；本目录提供几何版心、边注体系、定理框、章扉、交叉引用与黑白图形样式。全部书稿内容（系列名、题词、肖像、水印图、页脚小字）由使用方通过钩子注入，模板文件里只有机制。

## 用法

```tex
\documentclass[
  lang=cn, mode=simple, scheme=chinese, chinesefont=nofont,
  color=monochrome, margintrue, 10pt
]{elegantbook}
\input{series-skin/series-skin.tex}   % 按依赖顺序加载全部模块
```

前置条件是 XeLaTeX。字体随项目自备，放在 `assets/fonts/`，清单见 `font-setup.tex`，改动 `\fontdir` 即可换目录。各模块之间没有内部引用，可按需逐个 `\input`，保持下表顺序即可。

## 模块与加载顺序

| 顺序 | 文件 | 内容 |
|---|---|---|
| 1 | `design-tokens.tex` | 象牙纸/墨色系色板（PaperIvory/InkBrown/InkMuted）与 16 开双边注版心尺寸 |
| 2 | `font-setup.tex` | 开源字体栈：思源宋/黑正文、楷体仿宋、Times/GoogleSans/Courier 西文 |
| 3 | `packages.tex` | 补充宏包（tabularx、subcaption、mathtools、float、marginfix、imakeidx 等）与图表标题中文格式 |
| 4 | `math-setup.tex` | unicode-math、`\boldsymbol` 映射到 `\symbf`、公式按章编号 `章-序号` |
| 5 | `layout-setup.tex` | 行距 1.35、段首缩进 `2\ccwd`、公式间距挂接字号命令、列表与浮动间距 |
| 6 | `tikz-styles.tex` | 黑白教科书风 TikZ 与 pgfplots 全局样式（axis、geo、curve、guide、vector 等） |
| 7 | `cref-zh.tex` | cleveref 中文交叉引用（第X章、第X节、定理、图、式等），`\autoref` 映射到 `\cref` |
| 8 | `tufte-skin.tex` | 双面 16 开几何、安静版式（titlesec 调整、双线页眉、菱形页码徽）、边注体系（sidenote、marginfigure、fullwidth）、脚注改边注、整页出血封面 `\maketitle` |
| 9 | `chapter-plate.tex` | 章扉整页版式（象牙底、细线框、顶部色条、大标题），自动插在每个编号 `\chapter` 之前 |
| 10 | `environments.tex` | 环境重建：定理类空心黑白方框与相邻检查、例题与习题灰底框、remark 与 hint 短注自动进边注、choices 选择题选项、exsol 习题解答收集、summary、wenxintishi、epigraph，以及可选的动态水印机制 |

## 钩子

| 钩子 | 位置 | 说明 |
|---|---|---|
| `\footerpromo{...}` / `\footerpromoplain{...}` | `elegantbook.cls` | 页脚页码下的小字行，前者用于常规构建，后者用于定义了 `\NoWatermark` 的构建；默认两者为空 |
| `\setvertfont[opts]{file}` | `elegantbook.cls` | side 封面的竖排 CJK 字体，走 OpenType vertical 特性；未设置时回退正体 |
| `\coverstyle{banner\|side}` | `elegantbook.cls` | 封面版式，banner 为默认的顶部横幅，side 为左侧竖排文字加右侧满幅图 |
| `\mascot{...}` | `elegantbook.cls` | banner 封面底部的吉祥物行 |
| `\SeriesFootNoteLine` | `tufte-skin.tex` | 页脚脚线之下的推广行；默认为空 |
| `\SeriesWatermarkSetup{prefix}{count}` | `environments.tex` | 开启动态水印，图片为 `<prefix>1.png` 至 `<prefix><count>.png`，位置由页码做确定性伪随机生成；定义 `\NoWatermark` 后调用为 no-op |
| `\SeriesTitleCN` | `chapter-plate.tex` | 章扉顶部的系列名；默认为空 |
| `\SeriesChapterPartCN` / `\SeriesChapterQuote` / `\SeriesChapterQuoteBy` / `\SeriesSetChapterPortraitPath` | `chapter-plate.tex` | 章扉的篇名、题词、题词署名、肖像图，文档里用 `\renewcommand` 按 `\ifcase` 提供；全部默认为空，题词为空时该区块整体省略 |

系列元数据、分卷结构、防盗版与读者指纹、按篇章分组的索引映射属于使用项目，在文档侧通过上表钩子或附加文件提供。
