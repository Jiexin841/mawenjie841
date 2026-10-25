# 讲义（A版必修一）项目结构说明

采用「单一主文件 + 片段拼接」结构，用 XeLaTeX 编译。

## 编译

```
xelatex 讲义.tex      # 跑两遍以正确生成目录
```

## 目录结构

```
讲义.tex                     主文件：导言区（字体/元信息）+ 文档骨架 + \chapter/\input
环境演示.tex                 环境效果演示（standalone，单独编译，不影响讲义）
elegantbook.cls              文档类（不要动）
LXGWZhenKaiSlabGB-Regular.ttf  正文字体（不要动，路径依赖 Path=./）
figure/  image/              图片目录，cls 中已设为自动搜索路径
backup/                      重构前的单文件备份
chapters/
  00-模板.tex                新建章节时复制这个文件
  01-集合与常用逻辑用语.tex
  02-一元二次函数、方程和不等式.tex
  03-函数的概念与性质.tex
  04-指数函数与对数函数.tex
  05-三角函数.tex
```

## 新增一章的步骤

1. 复制 `chapters/00-模板.tex`，改名为 `chapters/06-平面向量.tex`（编号补零）。
2. 把模板里的示范内容整体替换成新正文。**只从 `\section` 开始写**，不要写 `\chapter`、不要写 `\documentclass` / `\begin{document}` / `\usepackage`。
3. 在 `讲义.tex` 的 `\mainmatter` 之后、`\end{document}` 之前，按顺序加两行：

```latex
\chapter{平面向量}
\input{chapters/06-平面向量}
```

4. 编译，检查目录与页码。

## 约定

- 路径一律用正斜杠 `/`，不要用反斜杠 `\`（`\` 是 LaTeX 转义符）。
- `\chapter` 只写在主文件里，片段文件里只写 `\section` 及以下。
- 定理/定义/例题的编号由主文件统一维护，拆分后仍然连续，不需要在片段里手动设置计数器。
- 片段文件首尾不要留多余空行。
- 不再使用的章节：注释掉主文件里的 `\chapter` + `\input` 两行即可，不要删除片段文件。

## 环境速查

想不起来某个环境长什么样、要几个参数、标签怎么写，就编译 `环境演示.tex`：

```
xelatex 环境演示.tex   # 跑两遍
```

打开 `环境演示.pdf`（11 页）逐页对照，看到合适的盒子后，
把对应的源码片段复制到 `chapters/` 下你自己的片段文件里即可。
它是独立文档，不 `\input` 任何片段，所以随便改都不会影响正式讲义。

标签速查：

| 环境 | 标签写法 | 引用写法 |
| --- | --- | --- |
| `definition` `theorem` `lemma` `corollary` `axiom` `postulate` `proposition` | `\begin{definition}[标题]{短名}` | `\ref{def:短名}`（前缀自动加） |
| 同上（另一种） | `\begin{definition}[标题]\label{任意名}` | `\ref{任意名}` |
| `example` `exercise` `problem` | 环境内 `\label{exa:1}` | `\ref{exa:1}` |
| `note` `remark` `proof` `solution` `assumption` `conclusion` `property` | 不带参数、不编号 | — |
| `custom` | `\begin{custom}{标题}` | — |
