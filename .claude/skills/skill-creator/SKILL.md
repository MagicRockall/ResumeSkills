---
name: skill-creator
description: Create new Claude Code skills (SKILL.md files) for this repository. Use when the user wants to add a new skill, create a custom skill from scratch, or package existing knowledge into a reusable skill file. Triggers: "create a skill", "add a skill", "make a skill for", "package this as a skill", "new skill".
---

# Skill Creator

## What Is a Skill?

A skill is a `SKILL.md` file that gives Claude specialized knowledge and workflows for a specific task. When placed in `.claude/skills/<skill-name>/SKILL.md`, it is automatically loaded in every Claude Code session for this repository.

Skills can also be placed in `.agents/skills/<skill-name>/SKILL.md` for compatibility with other AI agents (Cursor, Windsurf, Amp, etc.).

---

## Skill File Structure

Every `SKILL.md` must start with a YAML frontmatter block followed by the skill content:

```markdown
---
name: skill-name-in-kebab-case
description: One sentence describing what this skill does and WHEN to use it. Include trigger phrases like "use when user says X, Y, Z".
---

# Skill Title

## When to Use This Skill
[Trigger conditions and keywords]

## Core Capabilities
[What this skill enables Claude to do]

## [Main Content Sections]
[Frameworks, templates, examples, checklists]

## Output Format
[How to structure responses when using this skill]

## Implementation Checklist
[Step-by-step process]
```

---

## Frontmatter Rules

| Field | Required | Rules |
|-------|----------|-------|
| `name` | Yes | kebab-case, matches folder name |
| `description` | Yes | Start with a verb. Include when-to-use triggers. Keep under 300 chars. |

**Good description example:**
```
Analyze job postings, calculate match scores, identify gaps, and create application strategy. Use when user says "analyze this job", "should I apply", "match score".
```

**Bad description example:**
```
This skill helps with jobs.
```

---

## Skill Quality Checklist

### Content
- ✅ Clear trigger conditions ("when to use this skill")
- ✅ Specific output format with examples
- ✅ Concrete templates the model can fill in
- ✅ Checklists for implementation steps
- ✅ Before/after examples where relevant
- ✅ Edge cases handled

### Format
- ✅ Frontmatter block at top (name + description)
- ✅ Hierarchical headers (##, ###)
- ✅ Code blocks for templates and examples
- ✅ Tables for comparisons and options
- ✅ Bullet lists for rules and checklists

### Scope
- ✅ Single responsibility — one skill, one domain
- ✅ Not too generic ("help with coding") — be specific
- ✅ Not too narrow ("fix this one bug") — be reusable
- ✅ Actionable — model knows exactly what to do

---

## Creation Process

### Step 1: Define the skill

Ask:
- What task does this solve?
- What trigger phrases will activate it?
- What should the output look like?
- What knowledge or templates are needed?

### Step 2: Choose placement

```
.claude/skills/<name>/SKILL.md     → Claude Code only
.agents/skills/<name>/SKILL.md     → All AI agents (universal)
```

For most cases, use `.claude/skills/` — it's loaded automatically in Claude Code on the web.

### Step 3: Write the SKILL.md

Use this minimal template:

```markdown
---
name: your-skill-name
description: What it does and when to use it. Triggers: "phrase 1", "phrase 2".
---

# Your Skill Title

## When to Use This Skill

Use when the user:
- [Condition 1]
- [Condition 2]
- Mentions: "[keyword 1]", "[keyword 2]"

## Core Capabilities

- [Capability 1]
- [Capability 2]

## [Main Framework or Process]

[Content]

## Output Format

[Template or example of what to produce]

## Checklist

- ✅ [Step 1]
- ✅ [Step 2]
- ✅ [Step 3]
```

### Step 4: Install the skill

```bash
# Create the directory and file
mkdir -p .claude/skills/your-skill-name
# Write SKILL.md to that directory

# Commit and push so it persists across sessions
git add .claude/skills/your-skill-name/SKILL.md
git commit -m "Add your-skill-name skill"
git push
```

### Step 5: Verify

The skill appears in the available skills list in the next session. Test it by triggering one of its keywords.

---

## Multi-File Skills

For skills that need reference files, scripts, or assets:

```
.agents/skills/your-skill-name/
├── SKILL.md              ← main skill (auto-loaded)
├── references/
│   └── guide.md          ← referenced from SKILL.md
├── scripts/
│   └── validate.py       ← run by the agent on demand
└── assets/
    └── template.svg      ← shown to user on demand
```

Reference files are NOT auto-loaded — the SKILL.md must explicitly point to them:
```markdown
For detailed patterns, see `references/guide.md`.
```

---

## Example: Minimal Skill

```markdown
---
name: git-commit-writer
description: Write clear, conventional git commit messages. Use when user says "write a commit message", "commit message for", "what should I commit".
---

# Git Commit Writer

## When to Use This Skill

Use when user wants to write a commit message for their changes.

## Commit Message Format

Follow Conventional Commits:

\`\`\`
<type>(<scope>): <short summary>

<body — what changed and why>

<footer — breaking changes, issue refs>
\`\`\`

Types: feat, fix, docs, style, refactor, test, chore

## Rules

- Summary line: max 72 characters, imperative mood ("add" not "added")
- Body: explain WHY, not what (the diff shows what)
- One logical change per commit

## Output Format

Provide the commit message ready to copy, wrapped in a code block.
```

---

## Publishing Your Skill

To share a skill with the community:

1. Create a public GitHub repo (e.g. `yourname/my-skills`)
2. Place skills in `skills/<skill-name>/SKILL.md`
3. Others can install with: `npx skills add yourname/my-skills`

Follow the same `SKILL.md` structure — the `skills` CLI reads the `name` and `description` from the frontmatter automatically.
