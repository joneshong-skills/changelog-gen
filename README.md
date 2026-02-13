# Changelog Gen

Transform git commit history into polished, customer-facing release notes. Categorize changes, filter internal noise, and produce structured markdown.

## Overview

Convert raw git commits into professional release notes that focus on user-facing changes. This skill handles:
- Parsing commits from a specified range
- Categorizing by type (features, fixes, improvements, breaking changes)
- Filtering out internal noise (CI, tests, refactoring)
- Rewriting technical messages into customer-friendly language

## Workflow

### 1. Determine Scope
Specify a commit range:
- "since last release"
- "since v2.4.0"
- "last 2 weeks"
- "between v1.0 and v2.0"

### 2. Categorize
Map commits to categories based on conventional commit prefixes:
- `feat:` / `feature:` → New Features
- `fix:` / `bugfix:` → Fixes
- `improve:` / `enhance:` / `perf:` → Improvements
- `BREAKING CHANGE` / `!:` → Breaking Changes
- `security:` → Security

### 3. Filter Noise
Exclude internal-only changes:
- `chore:`, `ci:`, `build:`, `deps:` — infrastructure
- `test:`, `spec:` — test-only changes
- Internal refactoring (no user-facing impact)

### 4. Rewrite for Users
Transform technical commit messages into customer-friendly language:
- Lead with the **benefit**, not the implementation
- Use **bold feature names**
- Omit code-level details
- Past tense for fixes, present for features

### 5. Format & Deliver
Output as structured markdown with:
- Version/date header
- Categorized sections
- User-facing descriptions
- Optional: alternative formats (GitHub release, email, app store)

## Example Output

```markdown
# Release Notes — v2.5.0

## New Features

- **Team Workspaces**: Create separate workspaces for different projects.
  Invite team members and assign roles for better collaboration.

- **Keyboard Shortcuts**: Press `?` to see all available shortcuts.

## Improvements

- **Faster Search**: Results now load 2x faster across all content types
- **Better Notifications**: Notification preferences now sync across devices

## Fixes

- Fixed issue where large file uploads would timeout after 30 seconds
- Resolved timezone display errors in scheduled posts
```

## Style Adaptation

If your repo has `CHANGELOG.md` or `CHANGELOG_STYLE.md`, the skill reads it first and matches:
- Heading structure
- Category names and order
- Entry format
- Tone and voice

## When to Use

- Creating release notes for a new version
- Publishing update documentation
- Communicating changes to end users
- Preparing app store update text

## License

Provided as-is for changelog generation workflows.
