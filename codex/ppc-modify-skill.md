# PPC Modify Skill — Codex Edition

You handle paper revisions while preserving all previous versions.

**Prerequisites:** A paper has been built by ppc-edit-skill (main.tex + main.pdf + 排版大纲.md exist).

**Core rule: Old versions are read-only. All changes happen on copies.**

---

## Procedure

### Step 1: Version Snapshot

Before making any changes, freeze the current version:

```bash
mkdir -p versions/v001
cp 排版大纲.md versions/v001/
cp main.tex versions/v001/
cp main.pdf versions/v001/
cp references.bib versions/v001/ 2>/dev/null
cp -r figures/ versions/v001/figures/ 2>/dev/null
```

Version numbering:
- First modification → `v001` (snapshot of the original paper)
- Second modification → `v002`
- Never overwrite old version directories

Create/update `versions/CHANGELOG.md`:

```markdown
## Version History

| Version | Date | Type | Summary |
|---------|------|------|---------|
| v001 | 2026-06-21 14:30 | Original | Initial paper from ppc-edit-skill |
```

### Step 2: Collect Modification Requests

Ask the user to specify:

> "What do you want to change?
> 1. What to change? (content / format / structure / references / figures)
> 2. How to change it? (specific description)
> 3. Where? (whole paper / specific section / specific figure)"

If the user gives vague instructions like "fix the formatting," prompt with examples:

> "Please be specific. For example:
> - Format: 'Change body text to 12pt', 'Set line spacing to 1.5'
> - Content: 'Add a paragraph about XYZ in Chapter 2'
> - Structure: 'Swap Chapter 3 and Chapter 4'
> - References: 'Switch to APA 7', 'Add citation for Smith 2024'"

### Step 3: Copy the Layout Blueprint

```bash
cp 排版大纲.md 排版大纲_v002.md
```

Only modify the COPY. The original `排版大纲.md` stays untouched.

### Step 4: Apply Changes to the Copy

#### Format changes

Update the corresponding section in the copied blueprint:

```
User: "Change body text to 小四 (12pt)"
  → Blueprint Section 2 → Update to "中文正文: 宋体 小四 (12pt)"

User: "Change margins to top 2.5cm, bottom 2cm, left 3cm, right 2.5cm"
  → Blueprint Section 1 → Update \geometry{...}
```

#### Content changes

Annotate in the content mapping section:

```
User: "Add a discussion paragraph about XYZ in Chapter 2"
  → Blueprint Section 7 → Add:
    [Modification v002] Chapter 2 → Add Section 2.4 "XYZ Discussion", ~500 words
```

#### Structural changes

Update chapter ordering:

```
User: "Swap Chapter 3 and Chapter 4"
  → Blueprint Section 7 → Reorder:
    3. [Former Chapter 4] Experiments
    4. [Former Chapter 3] Methods
```

#### Reference changes

Update citation style:

```
User: "Switch to APA 7"
  → Blueprint Section 6 → Update to "APA 7"
  → Blueprint Section 10 → Update biblatex parameters
```

#### Multi-type changes

If the user requests multiple changes of different types, append a change log to the blueprint:

```markdown
## N. v002 Change Log

| # | Request | Type | Affected Sections | Status |
|---|---------|------|-------------------|:------:|
| 1 | Body text to 小四 | Format | 2, 7 | ✅ |
| 2 | Add XYZ discussion in Ch2 | Content | 7 | ✅ |
| 3 | Switch to APA 7 | Reference | 6, 10 | ✅ |
```

### Step 5: Confirm with User

Show a change summary before compiling:

> "Here's a summary of the changes (full blueprint at `排版大纲_v002.md`):
>
> **Format changes**: X items
> **Content changes**: Y items
> **Structural changes**: Z items
> **Reference changes**: W items
>
> Ready to recompile. Proceed?"

Do NOT compile until the user confirms.

### Step 6: Recompile

Using the modified blueprint (`排版大纲_v002.md`) as the sole authority, run the full ppc-edit-skill pipeline (all 11 steps).

Output:
```
main_v002.tex       # New source
main_v002.pdf       # New PDF
references.bib      # Shared bibliography
```

### Step 7: Update Version Log

Append to `versions/CHANGELOG.md`:

```markdown
| v002 | 2026-06-21 16:00 | User revision | Body text to 12pt; added XYZ discussion; switched to APA 7 |
```

---

## Directory Layout

```
working-directory/
├── 排版大纲.md               ← Original blueprint (read-only)
├── 排版大纲_v002.md           ← Modified blueprint (this round)
├── 排版大纲_v001.md           ← Previous round's blueprint
├── main.tex                  ← Latest source
├── main.pdf                  ← Latest PDF
├── versions/
│   ├── CHANGELOG.md
│   ├── v001/                 ← Original paper snapshot
│   └── v002/                 ← Previous version snapshot
├── references.bib
└── figures/
```

## Multi-Round Revisions

```
v001 (original) → v002 (1st revision) → v003 (2nd revision) → ...
```

Each round: snapshot current → copy blueprint → modify copy → recompile.

## Rollback

If the user wants to go back:

```bash
cp versions/v001/main.tex main.tex
cp versions/v001/排版大纲.md 排版大纲.md
xelatex main.tex && biber main && xelatex main.tex && xelatex main.tex
```

## Key Rules

1. Never touch old files — all modifications are on copies
2. Follow user requirements strictly, don't "improve" on them
3. Always confirm changes before compiling
4. Full version history in versions/ — any version can be restored
5. Even for "one-character" changes, run the full compile pipeline
6. Tool paths from ppc-tools-skill detection
