---
name: changelog-gen
description: "changelog, gen, generate, create, release, notes, write, 產生 changelog, 寫 release notes, 更新日誌"
version: 0.1.0
---

# Changelog Gen

Transform git commit history into polished, customer-facing release notes.
Categorize changes, filter internal noise, and produce structured markdown.

## Agent Delegation

Delegate git log analysis to `explorer` agent, changelog formatting to `writer` agent.

```
Main context (scope determination, style guide detection, offer post-generation options)
  └─ Task(subagent_type: explorer, prompt: "Run git log [range] --oneline --no-merges in [repo path]. Categorize each commit as: Breaking/Feature/Improvement/Fix/Security/Noise. Return categorized list only, one line per commit.")
  └─ Task(subagent_type: writer, prompt: "Format this categorized commit list into user-facing release notes for [version/date]. Style: [matched style]. Rewrite each entry to lead with the benefit. Return only the markdown changelog section.")
```

## Workflow

### Step 1 — Determine Scope

Ask for or infer the commit range:

| Input | Git Command |
|-------|-------------|
| "since last release" | `git log $(git describe --tags --abbrev=0)..HEAD` |
| "since v2.4.0" | `git log v2.4.0..HEAD` |
| "last 2 weeks" | `git log --since="2 weeks ago"` |
| "between v1.0 and v2.0" | `git log v1.0..v2.0` |

Use `--oneline --no-merges` for clean output. Add `--format="%h %s"` for hash + subject.

### Step 2 — Categorize Commits

Map each commit to one category based on its prefix or content:

| Category | Conventional Commit Prefixes | Display Heading |
|----------|------------------------------|-----------------|
| Breaking Changes | `BREAKING CHANGE`, `!:` | Breaking Changes |
| New Features | `feat:`, `feature:` | New Features |
| Improvements | `improve:`, `enhance:`, `perf:`, `refactor:` (user-visible) | Improvements |
| Bug Fixes | `fix:`, `bugfix:`, `hotfix:` | Fixes |
| Security | `security:`, `vuln:` | Security |

### Step 3 — Filter Noise

Exclude commits that are irrelevant to end users:

- `chore:`, `ci:`, `build:`, `deps:` — infrastructure
- `test:`, `spec:` — test-only changes
- `docs:` (internal docs) — unless it's user-facing documentation
- `refactor:` (no user-visible change) — internal restructuring
- Merge commits, version bumps, changelog updates

When in doubt, include the commit but simplify its description.

### Step 4 — Rewrite for Users

Transform technical commit messages into customer-friendly language:

| Technical Commit | User-Facing Entry |
|-----------------|-------------------|
| `fix: resolve race condition in ws handler` | Fixed issue where real-time updates could occasionally disconnect |
| `feat: add RBAC middleware` | Team Workspaces: assign roles (admin, editor, viewer) to team members |
| `perf: add redis cache to /api/search` | Search results now load 2x faster |

Principles:
- Lead with the **benefit**, not the implementation
- Use **bold feature names** for new features
- Omit code-level details (file names, function names, class names)
- Write in past tense for fixes ("Fixed..."), present for features/improvements

### Step 5 — Format Output

```markdown
# Release Notes — v2.5.0

## New Features

- **Team Workspaces**: Create separate workspaces for different projects.
  Invite team members and assign roles for better collaboration.

- **Keyboard Shortcuts**: Press `?` to see all available shortcuts.
  Navigate faster without touching your mouse.

## Improvements

- **Faster Search**: Results now load 2x faster across all content types
- **Better Notifications**: Notification preferences now sync across devices

## Fixes

- Fixed issue where large file uploads would timeout after 30 seconds
- Resolved timezone display errors in scheduled posts
- Corrected badge count not clearing after reading notifications
```

Heading format adapts to context:
- Version release: `# Release Notes — v2.5.0`
- Date range: `# Updates — Week of March 10, 2025`
- Sprint: `# Sprint 14 Release Notes`

### Step 6 — Present and Offer Options

After generating, offer:
- **Append to CHANGELOG.md** — prepend to existing file (most recent first)
- **Adjust tone** — more technical (developer audience) or simpler (end-user audience)
- **Add section** — breaking changes migration guide, upgrade instructions
- **Alternative formats** — GitHub release body, app store update text, email announcement

## Style Guide Adaptation

If the repo contains `CHANGELOG.md` or `CHANGELOG_STYLE.md`, read it first and match:
- Heading structure and nesting
- Category names and ordering
- Entry format (bullets vs paragraphs)
- Tone and voice

## Quick Reference

### Conventional Commits Detection

```
<type>[optional scope][!]: <description>
```

Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`.
The `!` after type/scope indicates a breaking change.

### Empty Changelog

If no user-facing commits exist in the range, report:
"No user-facing changes in this range. All commits were internal (tests, CI, refactoring)."

## Continuous Improvement

This skill evolves with each use. After every invocation:

1. **Reflect** — Identify what worked, what caused friction, and any unexpected issues
2. **Record** — Append a concise lesson to `lessons.md` in this skill's directory
3. **Refine** — When a pattern recurs (2+ times), update SKILL.md directly

### lessons.md Entry Format

```
### YYYY-MM-DD — Brief title
- **Friction**: What went wrong or was suboptimal
- **Fix**: How it was resolved
- **Rule**: Generalizable takeaway for future invocations
```

Accumulated lessons signal when to run `/skill-optimizer` for a deeper structural review.
