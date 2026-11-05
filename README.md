# Agent Skills Collection

A curated collection of AI Agent skills following the modern Antigravity / Agent Skills specification.

## 📁 Repository Structure

```text
skills/
├── quality-gate/
│   └── SKILL.md                 # Automated quality audit and post-implementation verification
├── github-review/
│   └── SKILL.md                 # Review GitHub PRs, apply reviewer feedback, and resolve threads
└── confluence-docs-sync/
    ├── SKILL.md                 # Keep Confluence documentation synchronized with code & Jira
    └── resources/
        └── page-registry.json   # Template mapping between Confluence pages and code paths
```

## 🛠️ Available Skills

### 1. [`quality-gate`](./skills/quality-gate/SKILL.md)
* **Description**: Runs an exhaustive automated quality audit and verification pass after completing features, bugfixes, or refactoring.
* **Checks**:
  - Mechanical gates: linting, TypeScript build/typecheck, unit test suites.
  - Architectural & code smell audits: strict typing (no `any`/`@ts-ignore`), SSR safety, security compliance.
  - Standardized verification summary report.

### 2. [`github-review`](./skills/github-review/SKILL.md)
* **Description**: Guides an AI agent through reviewing GitHub Pull Requests, analyzing reviewer comments and diffs, applying actionable code suggestions, running project checks, and resolving discussion threads.

### 3. [`confluence-docs-sync`](./skills/confluence-docs-sync/SKILL.md)
* **Description**: Keeps Atlassian Confluence documentation in continuous sync with codebase changes (schemas, DTOs, API endpoints, MCP tools, and UI views) and Jira task statuses via Atlassian MCP. Includes a customizable [`page-registry.json`](./skills/confluence-docs-sync/resources/page-registry.json) template.

---

## 📖 Skill Standard & Format

Each skill follows the modern agent skills specification:
- Contained in `skills/<skill-name>/` with a root `SKILL.md`.
- Starts with YAML frontmatter containing `name` and `description` (with trigger conditions).
- Uses modular subdirectories (`resources/`, `scripts/`, `references/`) for progressive disclosure.
