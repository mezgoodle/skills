# Agent Skills Collection

A curated collection of agent skills, runbooks, and workflows conforming to the modern Antigravity / AI Agent Skills standard.

Each skill is an isolated directory containing a standard `SKILL.md` instruction file with YAML frontmatter (`name`, `description`) designed for progressive disclosure and autonomous workflow execution.

---

## 🗂️ Skills Directory

| Skill | Description | Path |
| :--- | :--- | :--- |
| **`generate-tests`** | Generate unit and integration tests following `pytest` conventions and mocking best practices. | [`skills/generate-tests/`](./skills/generate-tests/SKILL.md) |
| **`github-review`** | Review GitHub PRs, analyze reviewer feedback, apply auto-corrections, run test suites, and resolve threads via GraphQL. | [`skills/github-review/`](./skills/github-review/SKILL.md) |
| **`sync-wiki`** | Inspect codebase changes and synchronize project architecture, API specs, and deployment documentation with GitHub Wiki. | [`skills/sync-wiki/`](./skills/sync-wiki/SKILL.md) |

---

## 📦 Skill Structure

Skills follow the standard directory layout:

```text
skills/<skill-name>/
├── SKILL.md          # Main instruction file with YAML frontmatter (Required)
├── scripts/          # Helper scripts and automation utilities (Optional)
├── examples/         # Reference implementations and patterns (Optional)
├── resources/        # Templates or config assets (Optional)
└── references/       # In-depth reference manuals and guides (Optional)
```

### `SKILL.md` Specification

Every skill starts with a YAML frontmatter block containing:
- `name`: Lowercase hyphenated unique identifier.
- `description`: Third-person clear summary explaining *what* the skill does and *when* the agent should invoke it.

```markdown
---
name: my-skill-name
description: Clear, third-person explanation of what this skill does and when to activate it.
---

# My Skill Title

Workflow instructions, prerequisites, step-by-step procedures, and verification commands.
```

---

## 🚀 How to Use

To use these skills in your workspace or project:
1. Copy the desired skill folder into your project's `.agents/skills/` (or global `~/.gemini/config/skills/`).
2. The agent automatically detects the skill description and activates it on-demand via progressive disclosure.
