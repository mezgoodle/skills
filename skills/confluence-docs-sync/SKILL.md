---
name: confluence-docs-sync
description: >-
  Synchronize project documentation in Confluence with the actual codebase and Jira tasks.
  Activate this skill whenever database schemas, DTOs, API endpoints, MCP tools, AI services,
  or views are modified or added, when completing a feature or preparing a PR, or when the user
  asks to update, review, or sync Confluence documentation.
---

# Confluence Documentation Synchronization Skill

This skill guides the AI agent in keeping Confluence documentation continuously synchronized with the codebase and Jira backlog.

## 1. Page Registry & Watch Paths

The configuration for Confluence space and page mappings is defined in [`page-registry.json`](./resources/page-registry.json):

* Configure your Atlassian Confluence `cloudId`, `spaceKey`, and `spaceId` in `resources/page-registry.json`.
* Map documentation pages to their corresponding code directories (`watchPaths`).

Example mapping table:

| Topic / Component | Description | Monitored Code Paths |
|---|---|---|
| **Domain Models & Entities** | Schemas, DTOs, interfaces, and database models | `backend/src/**/*.schema.ts`, `backend/src/**/dto/*.dto.ts` |
| **Multi-View UI System** | UI views, table/list/board components | `frontend/src/components/views/**/*`, `frontend/src/pages/**/*` |
| **Embedded MCP Server** | Custom MCP tools, SSE endpoints, client configs | `backend/src/mcp/**/*`, `backend/src/**/*.tool.ts` |
| **AI Assistant / Services** | LLM orchestrators, system prompts, function calling | `backend/src/ai/**/*`, `frontend/src/components/ai/**/*` |
| **Overall Architecture** | Architecture shifts, stack changes, Docker, CI/CD | `package.json`, `README.md`, `docker-compose.yml` |
| **Backlog & Traceability** | Jira traceability matrix, status transitions | Completed tasks, new stories |

---

## 2. Synchronization Workflow

When triggered (automatically after completing features, or upon explicit user request), execute the following steps:

### Step 1: Detect Changed Components
1. Inspect git status and changed files:
   ```bash
   git status --porcelain
   git diff --name-only HEAD~1
   ```
2. Match modified files against the `watchPaths` configured in `resources/page-registry.json`.
3. Identify which specific Confluence page(s) require updates.

### Step 2: Extract Changes from Code
* **Schemas & DTOs**: Check if any fields, types, indexes, or validation decorators were added, removed, or changed.
* **MCP Tools**: Check if tool names, input schemas, or descriptions were modified.
* **AI Orchestrator**: Check if model versions, system prompts, or tool execution loops changed.
* **Views & UI**: Check if new views, filters, or interaction patterns were implemented.

### Step 3: Update Confluence via Atlassian MCP
1. Fetch format rules if needed using `getContentFormatGuide` with `toolName: "updateConfluencePage"`.
2. Prepare the updated body fragment in clean HTML or Markdown format.
3. Call `updateConfluencePage` tool on `atlassian-mcp-server`:
   - `cloudId`: Target Atlassian Cloud ID
   - `pageId`: Target page ID from the page registry
   - `body`: Updated page content
   - `contentFormat`: `"html"` (or `"markdown"`)
   - `versionMessage`: Concise summary of the change, e.g.:
     `"Update Task schema with customFieldValues implementation"`

### Step 4: Update Traceability Matrix (if Jira status changed)
If a Jira task or story was completed or transitioned during the task, update the corresponding Traceability Matrix page to reflect the completed state.

### Step 5: Report to User
Provide a brief summary of updated sections and include direct links to the modified Confluence pages.
