# Agent Skills Collection

A curated collection of AI Agent skills following the modern Antigravity / Agent Skills specification.

Each skill is an isolated directory containing a standard `SKILL.md` instruction file with YAML frontmatter (`name`, `description`) designed for progressive disclosure and autonomous workflow execution.

---

## 📁 Repository Structure

```text
skills/
├── confluence-docs-sync/
│   ├── SKILL.md                 # Keep Confluence documentation synchronized with code & Jira
│   └── resources/
│       └── page-registry.json   # Template mapping between Confluence pages and code paths
├── generate-tests/
│   └── SKILL.md                 # Generate unit and integration tests with pytest & mocking
├── github-review/
│   └── SKILL.md                 # Review GitHub PRs, apply reviewer feedback, and resolve threads
├── quality-gate/
│   └── SKILL.md                 # Automated quality audit and post-implementation verification
└── sync-wiki/
    └── SKILL.md                 # Synchronize codebase changes with GitHub Wiki documentation
```

---

## 🛠️ Available Skills

| Skill | Description | Path |
| :--- | :--- | :--- |
| **`confluence-docs-sync`** | Keeps Atlassian Confluence documentation in continuous sync with codebase changes (schemas, DTOs, API endpoints, MCP tools, and UI views) and Jira task statuses via Atlassian MCP. Includes [`page-registry.json`](./skills/confluence-docs-sync/resources/page-registry.json). | [`skills/confluence-docs-sync/`](./skills/confluence-docs-sync/SKILL.md) |
| **`generate-tests`** | Generate unit and integration tests for backend services and APIs following `pytest` conventions, AAA pattern, and mocking best practices. | [`skills/generate-tests/`](./skills/generate-tests/SKILL.md) |
| **`github-review`** | Review GitHub PRs, analyze reviewer comments and diffs, apply actionable code suggestions, run project checks, and resolve threads via GraphQL. | [`skills/github-review/`](./skills/github-review/SKILL.md) |
| **`quality-gate`** | Runs an exhaustive automated quality audit and verification pass (linting, build/typecheck, unit tests, strict typing, SSR/security compliance) after completing features, bugfixes, or refactoring. | [`skills/quality-gate/`](./skills/quality-gate/SKILL.md) |
| **`sync-wiki`** | Inspect codebase changes and synchronize project architecture, API specs, and deployment documentation with the GitHub Wiki repository. | [`skills/sync-wiki/`](./skills/sync-wiki/SKILL.md) |

---

## 📖 Skill Standard & Format

Skills follow the modern directory layout:

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
- `name`: Unique identifier matching the parent directory name (1–64 characters; lowercase letters, digits, and single hyphens, with no leading or trailing hyphen).
- `description`: Third-person summary explaining *what* the skill does and *when* the agent should invoke it.

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
