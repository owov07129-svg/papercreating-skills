# PPC Tools Skill — Codex Edition

You are checking whether the user's local paper typesetting toolchain is complete and functional.

**Rule: Only check, don't auto-install. List missing tools and ask before installing.**

## Procedure

### Step 1: Auto-detect each tool

For each tool below, use these detection strategies in order:
1. `which <cmd>` or `where <cmd>` (is it in PATH?)
2. Search common install paths for the user's OS
3. Check environment variables (e.g. `JAVA_HOME`, `TEXMFHOME`)
4. If not found → mark as MISSING

### Step 2: Three-layer check per tool

| Layer | What to verify |
|-------|---------------|
| Existence | File/command present? |
| Integrity | `--version` or equivalent works? No errors? |
| Path | Record absolute path. In PATH or need full path to invoke? |

### Step 3: Tool Manifest (17 items)

#### Core Engines
1. **TeX Live** — Check: `xelatex --version`, `biber --version`, `latexmk --version`
2. **Pandoc** — Check: `pandoc --version` ≥ 3.0
3. **Java JDK** — Check: `java -version` ≥ 17; verify `JAVA_HOME` env var

#### Editor & Plugin
4. **VS Code** — Check: `code --version`
5. **LaTeX Workshop** — Check: `code --list-extensions | grep latex-workshop`; verify settings.json has valid LaTeX recipes

#### Reference Management
6. **Zotero** — Check: version ≥ 7
7. **Better BibTeX** — Check: .xpi file integrity (`unzip -t`)

#### Linting & Grammar
8. **chktex** — Check: `chktex --version`
9. **textidote** — Check: `java -jar <path> --version` (requires Java)

#### Format Conversion
10. **LibreOffice** — Check: `soffice --version`
11. **Acrobat** — Check: Python COM `Dispatch('AcroExch.App')` or app launch test

#### Python Ecosystem
12. **Python + Packages** — Check: python available, then import: pylatex, pygments, fitz, pdfplumber, pytesseract, pptx, docx, win32com (Windows only)

#### OCR
13. **Tesseract** — Check: `tesseract --version` ≥ 5; `--list-langs` includes user's language

#### Slides
14. **Marp CLI** — Check: `marp --version` ≥ 4

#### Other
15. **Git** — Check: `git --version`
16. **reference.docx** — Check: file exists; if not, generate: `pandoc --print-default-data-file reference.docx > reference.docx`
17. **reference.pptx** — Check: file exists; if not, generate: `pandoc --print-default-data-file reference.pptx > reference.pptx`

### Step 4: Report

Output a table:

```
| # | Tool | Status | Path | In PATH? | Invocation | Notes |
|---|------|:------:|------|:--------:|------------|-------|
| 1 | TeX Live | ✅ | /path/to/xelatex | ✅ | `xelatex` | v3.14 |
```

Status: ✅ = all 3 layers pass / ⚠️ = exists but broken/version issue / ❌ = not found

### Step 5: Handle missing tools

List all ❌ and ⚠️ items. Ask user:
- Do you want to install/fix these?
- Where do you want them installed?
- Warn about tools with special path requirements (Acrobat → must be system drive; VS Code → prefer default location)

Do NOT install without explicit user confirmation.
