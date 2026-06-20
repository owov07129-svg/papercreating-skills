# PPC Edit Skill — Codex Edition

You build a perfectly formatted paper from the user's content and a Layout Blueprint (排版大纲.md).

**Prerequisites:** Layout Blueprint confirmed, paper content ready, local toolchain checked.

## Tool Paths

Use whatever ppc-tools-skill detected. If a tool is not in PATH, use its full path.

---

## Pipeline (11 Steps × 3 Rounds)

### Round 1: Technical Compilation

#### Step 1: Build main.tex

Generate the LaTeX source from the Layout Blueprint's Section 10 (LaTeX Implementation Checklist). Include all packages, geometry, fonts, header/footer, and bibliography setup exactly as specified.

#### Step 2: First Compile

```bash
xelatex -synctex=1 -interaction=nonstopmode main.tex
```

#### Step 3: Fix Errors

Fix in order of appearance:
1. Typos (e.g. `\useackage` → `\usepackage`)
2. Missing packages (`File 'xxx.sty' not found`)
3. Mismatched braces
4. Unclosed environments
5. Math mode errors
6. Missing image files

Re-compile after each fix. Do not batch fixes.

#### Step 4: Bibliography

```bash
xelatex main.tex
biber main
xelatex main.tex
xelatex main.tex
```

Watch for: undefined citations, empty bibliography, incorrect sort order.

#### Step 5: Cross-references

Scan for: any `??` from `\ref{}`, any `[?]` from `\cite{}`, any numbering gaps. Fix and re-compile.

---

### Round 2: Polish

#### Step 6: Figure & Table Optimization

- All images ≥ 300 DPI
- Floating bodies not drifting (`[htbp]` adjustments)
- Tables not overflowing (`\resizebox` or `tabularx`)
- Captions complete and descriptive
- Vector format preferred over raster (PDF over PNG)

#### Step 7: Layout Refinement

Check against the Layout Blueprint:
- Margins correct
- Line spacing correct
- Headers/footers present
- No orphan/widow lines
- Paragraph spacing uniform
- CJK+English mixed text rendering correctly
- URLs break properly (`\url{}` package)
- Fonts render correctly, no tofu/boxes

#### Step 8: Language Check

| Check | Tool | Command |
|-------|------|---------|
| LaTeX code | chktex | `chktex main.tex` |
| English grammar | textidote | `java -jar <textidote-path> --check en main.tex` |
| Chinese | Manual | Read through for typos and grammar |
| Math | Manual | Verify symbol consistency, variable definitions |

Handle chktex results:
- `Wrong length of dash` → use `--` for en-dash, `---` for em-dash
- `Command terminated with space` → `\LaTeX{}`
- Quote style → use `` and '' not "

#### Step 9: Pre-submission Checklist

Verify:
```
□ All \cite{} have matching .bib entries
□ All \ref{} point to existing \label{}
□ No duplicate \label{}
□ No Underfull/Overfull hbox warnings (or documented as acceptable)
□ All images clear, tables complete
□ Page count meets requirements
□ TOC/LOF/LOT updated (compile one more time)
□ Title, author, date metadata correct
□ PDF opens standalone (no dependency on local paths)
□ All fonts embedded (check PDF properties)
```

---

### Round 3: Deliver

#### Step 10: Final Compile & Export

```bash
latexmk -C main.tex           # Clean all intermediate files
latexmk -xelatex main.tex      # Fresh compile
```

Export to other formats if requested:

| Format | Method |
|--------|--------|
| DOCX (via Pandoc) | `pandoc main.tex -o paper.docx --reference-doc=reference.docx` |
| DOCX (via Acrobat) | Open PDF → Export to Word |
| DOCX (free) | `soffice --headless --convert-to docx main.pdf` |
| Markdown | `pandoc main.tex -t markdown -o paper.md` |

For defense/presentation slides:

```bash
marp slides.md --pptx -o defense.pptx
```

#### Step 11: Delivery Verification

```
□ PDF metadata complete (title, author, keywords)
□ All fonts embedded
□ Hyperlinks functional (TOC, references, URLs)
□ No missing images
□ Page count, font sizes, margins match Layout Blueprint
□ Reference format consistent
□ Filename meets convention
```

---

## Output

1. `main.pdf` — final typeset paper
2. `main.tex` + `references.bib` + figures/ — source package
3. Format exports (if requested)
4. Build log summary (residual warnings explained)

## Key Rules

1. The Layout Blueprint is authority — follow it exactly
2. Show the compiled PDF after Round 1, wait for confirmation before Round 2
3. Fix errors one at a time, recompile after each
4. Don't make formatting decisions — the Blueprint decides
5. Use tool paths from ppc-tools-skill detection
