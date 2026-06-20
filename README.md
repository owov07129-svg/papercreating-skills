# PaperCreating Skills (PPC)

> **我们不帮你写论文，我们帮你把论文排得像 Nature 一样漂亮。**
>
> **We don't write your paper — we make your already-written paper look like it belongs in Nature.**

[中文](#中文) | [English](#english)

---

## 中文

### 这个项目是什么

**PaperCreating Skills（PPC）** 是一套 Claude Code 技能组，专注于**论文排版自动化**——把你已经写好的论文内容（文本、图表、公式、参考文献），按照你指定的模板或格式要求，一键生成排版精美、格式规范的最终 PDF/DOCX 交付件。

### 它不做什么

- ❌ **不生成论文内容** — 不帮你写摘要、引言、方法、结论
- ❌ **不做文献检索** — 不帮你找论文、读论文
- ❌ **不提供科研建议** — 不评价你的研究质量

### 它做什么

- ✅ **自动检查本地工具链** — 17 项工具一键体检，缺什么装什么
- ✅ **精确分析排版模板** — 18 个排版维度，从页边距到参考文献格式，不放过任何细节
- ✅ **全自动编译排版** — 11 步流水线：编译→消错→参考文献→交叉引用→图表优化→版式精调→语言校对→自查→导出→校验
- ✅ **迭代修改与版本管理** — 每次修改保留完整快照，随时回退到任意历史版本

### 核心理念

> **把精力用在真正的科研上，排版格式的事交给工具。**
>
> 无论你是投稿期刊、提交毕业论文、还是准备会议论文，PPC 帮你搞定从"内容就绪"到"格式完美"的全过程。
> 你只需要关心你的研究质量，格式问题我们处理。

### 四个技能

```
ppc-tools-skill ──→ ppc-template-skill ──→ ppc-edit-skill ──→ ppc-modify-skill
  (检查工具)          (分析模板)              (排版生成)          (修改迭代)
```

| 技能 | 一句话 | 触发方式 |
|------|--------|----------|
| **ppc-tools-skill** | 检查 17 项本地工具是否可用、完好、路径正确 | `/ppc-tools-skill` 或 "检查工具链" |
| **ppc-template-skill** | 扫描你的论文模板（.tex/.docx/.pdf），分析 18 维排版细节，生成排版大纲 | `/ppc-template-skill` 或 "分析模板" |
| **ppc-edit-skill** | 按排版大纲 + 你的论文内容，11 步生成格式完美的终稿 | `/ppc-edit-skill` 或 "生成论文" |
| **ppc-modify-skill** | 修改论文时自动版本快照、按你的要求调整大纲、重新编译、保留旧版 | `/ppc-modify-skill` 或 "修改论文" |

### 你需要准备什么

你已经写好的论文内容（以下任一形式）：

- Markdown / Word / 纯文本格式的论文
- `.tex` 源文件
- 结构化的论文大纲

加上一个模板（以下任一形式）：

- 学校/期刊提供的 `.tex` / `.docx` 模板
- 格式规范的 PDF 样例
- 口头描述的排版要求（如"宋体小四 1.5 倍行距 A4 单栏 GB/T 7714"）

### 安装

```bash
git clone https://github.com/owov071229-svg/papercreating-skills.git \
  ~/.claude/skills/papercreating-skills
```

### 使用流程

```
1. /ppc-tools-skill     → 工具链检测报告
2. /ppc-template-skill  → 排版大纲.md
3. /ppc-edit-skill      → main.pdf（格式完美的终稿）
4. /ppc-modify-skill    → main_v002.pdf（修改版，旧版已归档）
```

### 依赖工具

技能会自动检测系统中的以下工具（非硬编码路径）：

**核心引擎**：TeX Live、Pandoc、Java JDK
**编辑器**：VS Code + LaTeX Workshop
**文献管理**：Zotero + Better BibTeX
**检查校对**：chktex、textidote
**格式转换**：LibreOffice、Adobe Acrobat
**Python 生态**：pylatex、pygments、PyMuPDF、pdfplumber、pytesseract、python-pptx、python-docx
**OCR**：Tesseract
**幻灯片**：Marp CLI

---

## English

### What This Is

**PaperCreating Skills (PPC)** is a Claude Code skill suite focused on **automated paper typesetting**. It takes your already-written paper content (text, figures, tables, formulas, references) and a template or formatting specification, then produces a beautifully typeset final PDF/DOCX ready for submission.

### What It Does NOT Do

- ❌ **Does NOT generate paper content** — doesn't write your abstract, introduction, methods, or conclusion
- ❌ **Does NOT do literature search** — doesn't find or read papers for you
- ❌ **Does NOT provide research advice** — doesn't evaluate your research quality

### What It DOES Do

- ✅ **Auto-check local toolchain** — one-click health check for 17 tools, installs what's missing with your consent
- ✅ **Precisely analyze formatting templates** — 18 layout dimensions, from margins to citation styles, no detail missed
- ✅ **Fully automated typesetting pipeline** — 11 steps: compile → fix errors → bibliography → cross-references → polish figures → refine layout → language check (chktex + textidote) → self-audit → export → delivery verification
- ✅ **Iterative revision with version control** — full snapshots at each revision, rollback to any historical version

### Core Philosophy

> **Focus on your research. Let the tools handle the formatting.**
>
> Whether you're submitting to a journal, defending your thesis, or preparing a conference paper, PPC handles everything from "content ready" to "format perfect." You worry about your science — we worry about the typesetting.

### Four Skills

```
ppc-tools-skill ──→ ppc-template-skill ──→ ppc-edit-skill ──→ ppc-modify-skill
  (check tools)       (analyze template)      (build paper)        (revise & version)
```

| Skill | One-liner | Trigger |
|-------|-----------|---------|
| **ppc-tools-skill** | Health-check 17 local tools — existence, integrity, invocation path | `/ppc-tools-skill` |
| **ppc-template-skill** | Scan your paper template (.tex/.docx/.pdf), analyze 18 layout dimensions, output a layout blueprint | `/ppc-template-skill` |
| **ppc-edit-skill** | Take the layout blueprint + your paper content, run 11-step pipeline to produce a perfectly formatted final PDF | `/ppc-edit-skill` |
| **ppc-modify-skill** | Snapshot → modify blueprint per your instructions → recompile → archive old version. Full version history with rollback. | `/ppc-modify-skill` |

### What You Need

Your paper content (already written), in any of:

- Markdown / Word / plain text
- `.tex` source files
- Structured outline

Plus a template (any of):

- `.tex` / `.docx` template from your institution or target venue
- Well-formatted PDF sample
- Verbal formatting requirements (e.g., "Times New Roman 12pt, double-spaced, US Letter, single-column, APA 7")

### Installation

```bash
git clone https://github.com/owov071229-svg/papercreating-skills.git \
  ~/.claude/skills/papercreating-skills
```

### Workflow

```
1. /ppc-tools-skill     → Toolchain health report
2. /ppc-template-skill  → 排版大纲.md (layout blueprint)
3. /ppc-edit-skill      → main.pdf (perfectly formatted final paper)
4. /ppc-modify-skill    → main_v002.pdf (revised version, old version archived)
```

### Supported Tools

The skills auto-detect the following tools on your system (no hardcoded paths):

**Core engines**: TeX Live, Pandoc, Java JDK
**Editor**: VS Code + LaTeX Workshop
**Reference management**: Zotero + Better BibTeX
**Linting & grammar**: chktex, textidote
**Format conversion**: LibreOffice, Adobe Acrobat
**Python ecosystem**: pylatex, pygments, PyMuPDF, pdfplumber, pytesseract, python-pptx, python-docx
**OCR**: Tesseract
**Slides**: Marp CLI

---

## License

MIT
