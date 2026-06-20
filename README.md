# PaperCreating Skills (PPC)

A Claude Code skill suite that orchestrates the full academic paper creation workflow — from toolchain validation and template analysis to final polished output and iterative revision with version control.

## Overview

Four skills working in sequence:

```
ppc-tools-skill ──→ ppc-template-skill ──→ ppc-edit-skill ──→ ppc-modify-skill
 (check tools)       (analyze template)      (build paper)       (revise & version)
```

| Skill | What it does |
|-------|-------------|
| **ppc-tools-skill** | Auto-detect 17 local tools (TeX Live, Pandoc, Zotero, chktex, textidote, Tesseract, Marp, etc.) — check existence, integrity, and invocation paths. Missing/broken tools? Lists them, asks before installing, tells you about special path requirements. |
| **ppc-template-skill** | Scan a paper template (`.tex` / `.docx` / `.pdf`) across 18 layout dimensions — fonts, margins, columns, headers, table styles, figure captions, references. No template? Proactively asks for your formatting requirements. Outputs a detailed layout blueprint. |
| **ppc-edit-skill** | Takes the layout blueprint + your paper content, builds a LaTeX source, and runs the full 11-step pipeline: compile → fix errors → bibliography → cross-references → polish figures → refine layout → language check (chktex + textidote) → self-audit → final export → delivery verification. |
| **ppc-modify-skill** | When you want changes: snapshots the current version, copies the layout blueprint, applies your modifications strictly as requested, re-runs the full ppc-edit pipeline, and archives the old version. Full version history with rollback support. |

## Prerequisites

The skills check for and work with these tools (auto-detected, not hardcoded):

- **Core**: TeX Live, Pandoc, Java JDK
- **Editor**: VS Code + LaTeX Workshop
- **Reference**: Zotero + Better BibTeX
- **Linting**: chktex, textidote
- **Conversion**: LibreOffice, Adobe Acrobat
- **Python**: pylatex, pygments, PyMuPDF, pdfplumber, pytesseract, python-pptx, python-docx
- **OCR**: Tesseract
- **Slides**: Marp CLI
- **Other**: Git, reference.docx, reference.pptx

## Installation

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/<your-username>/papercreating-skills.git \
  ~/.claude/skills/papercreating-skills
```

Or if you use the plugin system:

```bash
claude /plugin install <your-username>/papercreating-skills
```

## Usage

In a Claude Code session:

```
/ppc-tools-skill        # Step 1: Check your toolchain
/ppc-template-skill     # Step 2: Analyze template → layout blueprint
/ppc-edit-skill         # Step 3: Build the final paper
/ppc-modify-skill       # Step 4: Revise with version control
```

You can also use natural language triggers like "检查工具链", "分析模板", "生成论文", "修改论文".

## Workflow

```
1. /ppc-tools-skill     → Toolchain report (what's installed, what's missing)
2. /ppc-template-skill  → 排版大纲.md (layout blueprint)
3. /ppc-edit-skill      → main.pdf (final paper)
4. /ppc-modify-skill    → main_v002.pdf (revised paper, old version archived)
```

## License

MIT
