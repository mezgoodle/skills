---
name: github-review
description: >-
  Review GitHub pull requests, analyze reviewer comments and diffs, apply actionable feedback,
  run verification checks, and resolve review threads.
---

# GitHub PR Review & Auto-Correction Skill

This skill guides the AI agent through inspecting incoming pull request feedback, applying actionable code suggestions, verifying project integrity, and resolving GitHub review threads.

## Workflow

### Step 1: Context Retrieval
- Retrieve PR details using GitHub MCP tools or GitHub CLI (`gh`):
  - Current PR diff: `gh pr diff <PR_NUMBER>`
  - Unresolved review threads and comments:
    ```bash
    gh api graphql -F owner=":owner" -F name=":repo" -F prNumber=<PR_NUMBER> -f query='
      query($owner: String!, $name: String!, $prNumber: Int!) {
        repository(owner: $owner, name: $name) {
          pullRequest(number: $prNumber) {
            reviewThreads(first: 50) {
              nodes {
                id
                isResolved
                comments(first: 10) {
                  nodes { body path line author { login } }
                }
              }
            }
          }
        }
      }'
    ```

### Step 2: Semantic Analysis
- Match every comment and review suggestion to the exact target file and line number.
- Categorize comments into:
  - **Actionable**: Code suggestions with specific diffs or clear improvement instructions.
  - **Clarification / Discussion**: Questions, design feedback, or requests for more information.

### Step 3: Execution
- For each **Actionable** suggestion:
  - Inspect the local code context.
  - Apply the requested improvements or fixes cleanly.
  - Preserve repository formatting and architectural standards.

### Step 4: Verification
- Execute mandatory mechanical checks (linting, typechecking, tests) in all touched workspaces.
- If checks fail:
  - Inspect error output, fix issues, and re-verify until all checks pass.

### Step 5: Resolving Review Threads & Pushing Changes
- Commit changes with a descriptive message referencing the feedback.
- Push changes to the remote branch: `git push origin <branch-name>`
- Resolve successfully addressed review threads via `gh api graphql`:
  ```bash
  gh api graphql -F threadId="<THREAD_NODE_ID>" -f query='
    mutation($threadId: ID!) {
      resolveReviewThread(input: {threadId: $threadId}) {
        thread { id isResolved }
      }
    }'
  ```
- Post a summary report of resolved items and committed changes.
