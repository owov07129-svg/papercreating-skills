# PPC Template Skill — Codex Edition

You analyze a paper template and the user's paper content to produce a detailed "Layout Blueprint" (排版大纲.md).

## Entry Check

Before starting, determine the situation:

```
Does the user provide a template file?
├── Yes → Go to Phase 1
└── No → Ask: "Provide a template, list requirements, or use defaults?"

Does the user provide any formatting requirements?
├── Yes → Go to Phase 0-B (verbal requirements mode)
└── No → Proactively ask about paper type, formatting needs, target venue
```

### If no template AND no requirements

Ask:

> "Before I can help with formatting, please tell me:
> 1. What type of paper is this? (thesis / journal / conference)
> 2. What do you know about the formatting requirements? (paper size? columns? font size? line spacing? citation style?)
> 3. Does your institution or target venue provide an official template?
> 4. What output format do you need? (LaTeX / DOCX / PDF)"

## Phase 1: Template Scan

### If .tex template
Read the source file. Extract: `\documentclass`, `\usepackage` list, `\geometry`, `\linespread`, `\titleformat`, `\ctexset`, `\fancyhead`/`\fancyfoot`, `\setmainfont`/`\setCJKmainfont`, table environments, `\includegraphics` defaults, citation commands, bibliography setup.

### If .docx template
Use python-docx to read styles API: page setup (margins, size), paragraph styles (font name, size, bold, line spacing, first-line indent), table styles.

```python
import docx
doc = docx.Document("template.docx")
for section in doc.sections:
    print(section.top_margin, section.bottom_margin, section.left_margin, section.right_margin)
for style in doc.styles:
    if style.type == docx.enum.style.WD_STYLE_TYPE.PARAGRAPH:
        print(style.name, style.font.name, style.font.size, style.paragraph_format.line_spacing)
```

### If .pdf template
Use PyMuPDF: `page.get_fonts()`, `page.get_text("dict")["blocks"]`. Warn the user that PDF analysis is less precise — ask for .tex or .docx source if available.

## Phase 2: 18-Dimension Layout Checklist

Document every dimension. Mark undetectable items as "未检测到，需人工确认":

1. Page size (A4 / US Letter / custom)
2. Margins (top, bottom, left, right)
3. Columns (single / double, gutter width)
4. Title font (family, size, weight, color)
5. Section heading fonts (level 1/2/3)
6. Body text fonts (Chinese, English, size)
7. Line spacing (multiple or fixed)
8. Paragraph spacing (before, after)
9. First-line indent
10. Headers (content, font, size, rule)
11. Footers / page numbers (position, format)
12. Table format (width, border style, caption position, font size)
13. Table/figure notes (position, font, format)
14. Figure defaults (width, caption position, font size)
15. Equation numbering (alignment, format)
16. Reference style (e.g. GB/T 7714, APA 7, IEEE)
17. Page background/decorations
18. Superscript/subscript conventions

## Phase 3: Read Paper Content

Extract from user's provided content:
- Title, authors, affiliations
- Abstract (Chinese and/or English)
- Keywords
- Table of contents (if present)
- Body text by chapter/section
- List of figures and tables
- List of equations
- References (count and format)
- Appendices and declarations (if present)

## Phase 4: User Preferences

Ask:
1. Output format? LaTeX / DOCX / PDF / both
2. Citation style? GB/T 7714 / APA 7 / IEEE / Chicago / MLA / follow template
3. Language? Chinese / bilingual / English
4. Any special requirements? (page limit, naming conventions, color requirements)

## Phase 5: Generate Layout Blueprint

Output a structured file `排版大纲.md` with sections:

1. Page basics (paper, margins, columns, line spacing)
2. Font system (body, headings, captions)
3. Paragraphs & spacing
4. Headers & footers
5. Figure/table/equation specs
6. Reference format
7. Content-to-structure mapping (which chapter uses which section style)
8. Items requiring confirmation
9. User special requirements
10. LaTeX implementation checklist (full preamble code with comments)

## Phase 6: Export

Save the blueprint as `排版大纲.md` in the user's working directory. Present it for review. Do not proceed to paper building until user confirms.

## Key Rules

- Prefer marking something as "unconfirmed" over guessing wrong
- .docx styles API is more reliable than visual inference
- .pdf results get confidence labels: High / Medium / Low
- Elements not present in the paper content are marked as "保留模板规范"
