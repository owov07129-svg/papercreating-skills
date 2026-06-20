---
name: ppc-edit-skill
description: "根据 ppc-template-skill 确定的排版大纲，将论文内容或 .tex 源文件按完整写作步骤（编译→消错→参考文献→交叉引用→图表优化→版式精调→语言校对→自查→格式导出→交付校验）生成最终论文。触发器：生成论文、排版论文、最终输出、ppc-edit、导出论文、论文终稿。"
version: "2.0.0"
last_updated: "2026-06-20"
status: active
data_access_level: redacted
task_type: open-ended
related_skills:
  - ppc-tools-skill
  - ppc-template-skill
  - ppc-modify-skill
---

# PPC Edit Skill — 论文终稿生成

根据排版大纲，使用本地工具链，按标准写作步骤生成最终论文。

## 前置条件

1. **排版大纲**必须已经由 `ppc-template-skill` 生成并经用户确认
2. **论文内容**（文本/图表/公式/参考文献）已就绪
3. **本地工具链**已通过 `ppc-tools-skill` 检查并修复

## 工具路径获取

所有工具的调用路径以 `ppc-tools-skill` 检测到的路径为准。如果工具不在 PATH 中，使用完整路径调用。在开始编译前，先确认以下工具可用：

- `xelatex`（或 `pdflatex` / `lualatex`）
- `biber`（或 `bibtex`）
- `latexmk`（可选但推荐）
- `pandoc`（如需格式转换）

---

## 执行流程（11 步，3 轮循环）

### 第一轮：技术编译（让代码跑通）

#### Step 1：构建 LaTeX 源文件

根据排版大纲中的"十、LaTeX 实现清单"构建源文件：

```latex
\documentclass[a4paper,12pt]{ctexart}  % 或其他模板指定的文档类

% === 宏包加载（按排版大纲逐条加载） ===
\usepackage{geometry}
\geometry{...}           % 页码设置 — 来自排版大纲"页面基础"
\usepackage{fancyhdr}    % 页眉页脚
\usepackage{graphicx}    % 图片
\usepackage{booktabs}    % 三线表
\usepackage{amsmath}     % 数学公式
\usepackage[backend=biber]{biblatex}  % 参考文献
\usepackage{hyperref}    % 超链接
% ... 其他按排版大纲列出的宏包

% === 字体设置 ===
% （按排版大纲"字体体系"配置）

% === 页眉页脚 ===
\pagestyle{fancy}
\fancyhead[L]{...}
\fancyhead[R]{...}
\fancyfoot[C]{\thepage}

% === 参考文献资源 ===
\addbibresource{references.bib}

\begin{document}

% 标题
\title{...}
\author{...}
\date{...}
\maketitle

% 摘要
\begin{abstract}
...
\end{abstract}

% 正文内容（按排版大纲的"内容与结构映射"组织）
\section{引言}
...
\section{方法}
...
\subsection{子章节}
...

% 参考文献
\printbibliography

\end{document}
```

**工具**: 使用 VS Code + LaTeX Workshop（提供语法高亮、自动补全和环境模板）。如果用户使用其他编辑器亦可。

#### Step 2：首次编译

```bash
xelatex -synctex=1 -interaction=nonstopmode main.tex
```

或在 VS Code 中 `Ctrl+Alt+B`。

**检查** `.log` 文件，记录所有 Error 和 Warning。

#### Step 3：消错误

```
错误类型排查顺序：
1. 拼写 → \useackage → \usepackage
2. 缺失宏包 → ! LaTeX Error: File 'xxx.sty' not found → 补充或删除
3. 括号不匹配 → { 和 } 数量不一致
4. 环境未闭合 → \begin{table} 缺 \end{table}
5. 数学模式 → $...$ 交叉
6. 图片路径 → File 'fig1.png' not found
```

每次改完一个错误就重新编译，直到无 Error。

#### Step 4：参考文献

```bash
xelatex main.tex    # ① 生成 .aux
biber main          # ② 从 .bib 提取引用 → 生成 .bbl
xelatex main.tex    # ③ 将文献列表写入 PDF
xelatex main.tex    # ④ 解析交叉引用变化
```

**常见问题**:
- `Citation 'xxx' undefined` → 检查 `.bib` 中的 key 是否匹配
- 参考文献列表空白 → biber 未运行；确认 latexmk 配置中包含 biber
- 参考文献排序错误 → 检查 biblatex 的 `sorting` 选项

#### Step 5：交叉引用解决

```
检查清单:
□ 无 \ref{} 显示为 ?? 的引用
□ 无 \cite{} 显示为 [?] 的引用
□ 图表编号连续无跳号
□ .log 中搜索 "Warning: Reference" → 逐条处理
```

---

### 第二轮：质量打磨（让论文好看）

#### Step 6：图表优化

| 检查项 | 操作 | 工具 |
|--------|------|------|
| 图片分辨率 | 所有图片 ≥ 300 DPI | 文件属性查看 |
| 图表位置 | 浮动体是否异常漂移 | `[htbp]` 参数调整 |
| 表格宽度 | 是否超出页面 | `\resizebox{\textwidth}{!}{...}` 或 `tabularx` |
| 标题完整性 | 图表标题是否有充分说明力 | 人工审查 |
| 矢量格式 | 图表尽量用 PDF 矢量图而非 PNG | 各绘图软件的矢量导出 |

#### Step 7：版式精调

按排版大纲逐项对照：

```
页面级别:
□ 页边距符合要求
□ 行距恰当（1.5 倍或模板指定值）
□ 页眉页脚正确
□ 无孤行（单行在页首/页尾）

段落级别:
□ 段间距均匀
□ 中英文混排协调（XeLaTeX 自动处理）
□ URL 自动断行（\url{} 宏包）

字体级别:
□ 无字体缺失导致的方块
□ 粗体/斜体/等宽区分清晰
□ 数学符号全部正常渲染
```

#### Step 8：语言校对

| 检查对象 | 工具 | 说明 |
|----------|------|------|
| LaTeX 代码 | **chktex** | `chktex main.tex` |
| 英文语法 | **textidote** | `java -jar <textidote-path> --check en main.tex` |
| 中文语法 | 人工 | 逐段通读 |
| 数学 | 人工 | 符号一致性、变量定义是否齐全 |

chktex 常见警告处理：
- `Wrong length of dash` → `--`（短破折号）vs `---`（长破折号）
- `Command terminated with space` → 改为 `\LaTeX{}` 加空括号
- `You should use \` to end quotations` → 引号用 `` '' 而非 `"`

#### Step 9：提交前自查清单

```
□ 所有 \cite{} 都有对应 .bib 条目
□ 所有 \ref{} 都指向存在的 \label{}
□ 无重复 \label{}
□ 无 Underfull / Overfull hbox 警告（或已确认可接受）
□ 图片清晰、表格完整
□ 页数满足要求
□ 目录/图表索引已更新（多编译一次）
□ 元数据 (\title{} \author{} \date{}) 正确
□ PDF 可独立打开（不依赖本机文件路径）
□ 字体全部嵌入（用 PDF 阅读器查看文档属性中的字体列表）
```

---

### 第三轮：最终交付

#### Step 10：最终编译与导出

```bash
# 彻底清理所有中间文件
latexmk -C main.tex

# 从头清爽编译
latexmk -xelatex main.tex
```

**如果用户需要非 PDF 格式：**

| 目标格式 | 方法 |
|----------|------|
| **PDF**（默认） | 最终编译产物 |
| **DOCX**（Pandoc） | `pandoc main.tex -o paper.docx --reference-doc=reference.docx` |
| **DOCX**（Acrobat 精确） | Acrobat 打开 PDF → 导出为 Word |
| **DOCX**（免费） | `soffice --headless --convert-to docx main.pdf` |
| **Markdown** | `pandoc main.tex -t markdown -o paper.md` |

#### Step 11：交付校验

```
□ PDF 元数据完整（标题/作者/关键词）
□ 字体全部嵌入
□ 链接可点击（目录/引用/URL 在 PDF 中超链接有效）
□ 图片不丢失
□ 页数/字号/页边距符合排版大纲
□ 参考文献格式一致
□ 文件名符合要求
□ 在另一设备上测试打开（如果有条件）
```

若用户要求论文答辩 PPT，额外执行：

```bash
# 根据论文内容生成 Marp 幻灯片
marp slides.md --pptx -o defense.pptx
```

---

## 输出

1. **主 PDF 文件** — 最终编译产物
2. **源文件包** — `main.tex` + `references.bib` + 图片文件
3. **格式导出文件**（如用户要求）— `.docx` / `.md` / `.pptx`
4. **编译日志摘要** — 残留 Warning 说明（如无可忽略）

## 关键规则

1. **排版大纲是唯一权威** — 每一步都以排版大纲为准，不自行发挥
2. **每轮确认** — 第一轮技术编译结束后展示 PDF 给用户看，确认再进第二轮
3. **错误不攒着** — 发现一个编译错误立刻修，不要等全部写完再回头
4. **诚实标注** — 排版大纲中"待确认项"如果影响排版，标出来让用户决定
5. **工具路径** — 以 `ppc-tools-skill` 检测到的路径为准
