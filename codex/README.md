# PaperCreating Skills for Codex (OpenAI)

These are adapted prompts for use with OpenAI Codex CLI (`codex`).

## How It Differs from the Claude Code Version

| Feature | Claude Code (`/ppc-*`) | Codex (this directory) |
|---------|------------------------|------------------------|
| **Trigger** | Slash command | Natural language |
| **Format** | `SKILL.md` with YAML frontmatter | Plain markdown prompt files |
| **Invocation** | `/ppc-tools-skill` | Copy the prompt, then type "Run the toolchain check" |
| **Tool paths** | Auto-detected or from system PATH | Auto-detected or from system PATH (same behavior) |

## Usage

### Method A: One-shot prompt (quick)

Copy the content of the desired `codex/ppc-*.md` file into your Codex session and say:

> "Run this workflow step by step."

### Method B: Custom Instructions (persistent)

Copy the content into Codex's custom instructions / system prompt so every session auto-loads the skills.

### Method C: Use the orchestrator

Copy `codex/orchestrator.md` into your session for a guided prompt that routes to the right skill.

## Files

```
codex/
├── README.md
├── orchestrator.md       # One prompt that routes to any skill
├── ppc-tools-skill.md     # Toolchain health check
├── ppc-template-skill.md  # Template analysis
├── ppc-edit-skill.md      # Paper typesetting
└── ppc-modify-skill.md    # Revision with version control
```

## Example Session

```
You: I need to format my paper. It's written in Markdown, and I have
    a .docx template from my university.

Copy ppc-template-skill.md into context, then say: "Run this workflow."

Codex: [Analyzes template...] Here's your layout blueprint...

Copy ppc-edit-skill.md, then say: "Build the paper from this blueprint."

Codex: [Compiles...] main.pdf is ready.
```
