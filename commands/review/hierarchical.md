---
allowed-tools: Task
description: Comprehensive multi-stage code review using specialized subagents
argument-hint: [files-or-directories]
user-invocable: true
---

## Context

- Current branch: !`git branch --show-current`
- Git status: !`git status --porcelain`
- Base commit: !`git merge-base HEAD origin/main 2>/dev/null || git rev-parse HEAD~10 2>/dev/null || echo "unknown"`
- Changes since base: !`BASE=$(git merge-base HEAD origin/main 2>/dev/null) && git log --oneline $BASE..HEAD`
- Files changed since base: !`BASE=$(git merge-base HEAD origin/main 2>/dev/null) && git diff --name-only $BASE..HEAD`

## Requirements

- Use **@tech-lead-reviewer** — architectural impact assessment — to scope architectural risk before launching specialized reviews.
- Run parallel reviews with:
  - **@code-reviewer** — logic correctness, tests, error handling.
  - **@security-reviewer** — authentication, data protection, validation.
  - **@ux-reviewer** — usability and accessibility (skip if purely backend/CLI).
- Consolidate findings by priority (Critical → High → Medium → Low) and confidence (High → Medium → Low).
- Offer optional implementation support and ensure final suggestions respect SOLID principles.
- **Use atomic commits for logical units of work**: Each commit should represent one complete, cohesive change.
- Title: entirely lowercase, <50 chars, imperative mood (e.g., "add", "fix", "update"), conventional commits format (feat:, fix:, docs:, refactor:, test:, chore:)
  - Scope (optional): lowercase noun, 1-2 words. Must match existing scopes in git history.
- Body: blank line after title, ≤72 chars per line, must start with uppercase letter, standard capitalization and punctuation. Describe what changed and why, not how.
- Footer (optional): Must start with uppercase letter, standard capitalization. Reference issues/PRs (Closes #123, Fixes #456, Linked to PR #789). Use BREAKING CHANGE: prefix for breaking changes.

### Examples

```
feat(auth): add google oauth login flow

- Introduce Google OAuth 2.0 for user sign-in
- Add backend callback endpoint `/auth/google/callback`
- Update login UI with Google button and loading state

Add a new authentication option improving cross-platform
sign-in.

Closes #42. Linked to #38 and PR #45
```

```
fix(api): handle null payload in session refresh

- Validate payload before accessing `user.id`
- Return proper 400 response instead of 500
- Add regression test for null input

Prevents session refresh crash when token expires.

Fixes #105
```

```
feat(auth): migrate to oauth 2.0

- Replace basic auth with OAuth 2.0 flow
- Update authentication middleware
- Add token refresh endpoint

BREAKING CHANGE: Authentication API now requires OAuth 2.0 tokens. Basic auth is no longer supported.

Closes #120. Linked to #115 and PR #122
```

## Your Task

**IMPORTANT: You MUST use the Task tool to complete ALL tasks.**

1. Perform a leadership assessment with **@tech-lead-reviewer** — architectural impact assessment — to map risk areas and determine which specialized agents to involve.
2. Launch the required specialized reviews in parallel via the Task tool, collect outcomes, and resolve conflicting feedback.
3. Present a consolidated report with prioritized recommendations, ask whether the user wants fixes implemented, and if so, execute optimizations and testing before summarizing results.

### Review Flow

- **Technical Leadership Assessment**: Evaluate architecture, technical debt, scalability, and maintainability impact.
- **Parallel Specialized Reviews**:
  - **@code-reviewer** — logic correctness, tests, error handling.
  - **@security-reviewer** — authentication, data protection, validation.
  - **@ux-reviewer** — usability and accessibility (skip if purely backend/CLI).
- **Consolidated Analysis**: Merge findings, prioritize by impact/confidence, and produce actionable improvements.
- **Optional Implementation**: Address security, quality, or UX issues as requested, then run tests and validations.
- **Final Optimization**: Engage **@code-simplifier** — code simplification and optimization — to refactor implemented fixes, remove redundancy, and verify compliance with SOLID principles before finalizing the summary.