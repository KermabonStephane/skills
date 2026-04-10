---
name: Git Workflow
description: Standard Git workflow for feature development — branch naming, commit message format, PR creation, and review process.
---

# Git Workflow Guide

This skill defines the standard process for contributing code: from creating a branch to merging a reviewed PR.

## 1. Create a Branch

Always branch off `main` (or the project's main branch). Never commit directly to `main`.

**Naming convention**: `<type>/<short-description>`

| Type        | When to use                            |
|-------------|----------------------------------------|
| `feat/`     | New feature                            |
| `fix/`      | Bug fix                                |
| `chore/`    | Tooling, deps, config                  |
| `refactor/` | Code restructuring, no behavior change |
| `docs/`     | Documentation only                     |
| `test/`     | Tests only                             |

```bash
git switch main
git pull origin main
git switch -c feat/add-firefox-extension
```

## 2. Commit Message Format

Follow the **Conventional Commits** specification.

### Structure

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

### Rules

- **type**: same set as branch types — `feat`, `fix`, `chore`, `refactor`, `docs`, `test`
- **scope**: the package or area affected (e.g. `chrome-extension`, `domain`, `auth-google`)
- **summary**: imperative mood, lowercase, no period — "add firefox adapter", not "Added Firefox adapter."
- **body**: explain *why*, not *what* — the diff already shows what changed
- **footer**: reference issues with `Closes #<n>` or `Refs #<n>`

### Examples

```
feat(firefox-extension): add MV3 manifest and popup scaffold

Bootstraps the Firefox extension package mirroring the chrome-extension
structure. Uses the same Preact popup and background worker pattern.

Closes #1
```

```
fix(github-storage): handle rate-limit 403 on index fetch

GitHub returns 403 (not 429) when the secondary rate limit is hit.
Retry with exponential back-off up to 3 times before surfacing the error.
```

```
chore(deps): upgrade vite to 6.x
```

### Breaking changes

Add `!` after the type/scope and a `BREAKING CHANGE:` footer:

```
feat(auth-google)!: drop chrome.identity fallback for MV2

BREAKING CHANGE: MV2 support removed. All auth flows now require MV3
service worker context.
```

## 3. Push and Create a PR

Push your branch and open a PR against `main`:

```bash
git push -u origin feat/add-firefox-extension
gh pr create \
  --title "feat(firefox-extension): add MV3 extension scaffold" \
  --body "$(cat <<'EOF'
## Summary
- Bootstraps `@bookmark/firefox-extension` package
- MV3 manifest, Preact popup, background service worker
- Shares domain and application layers with the Chrome extension

## Test plan
- [ ] Extension loads in Firefox without errors
- [ ] Popup renders and lists bookmarks
- [ ] Background worker connects to storage port

Closes #1
EOF
)"
```

### PR title format

Same as commit: `<type>(<scope>): <short summary>` — it becomes the squash-merge commit message.

### PR checklist before requesting review

- [ ] All tests pass locally (`pnpm test`)
- [ ] No linting errors (`pnpm lint`)
- [ ] Self-reviewed the diff — no debug logs, no commented-out code
- [ ] PR description explains *why*, not just *what*
- [ ] Linked to the relevant issue (`Closes #n`)
- [ ] The changelog is updated

## 4. Waiting for Review

- **Do not** force-push after requesting review — it invalidates existing comments.
- **Do** push fixup commits in response to review feedback; squash before merge if needed.
- Address all review comments before re-requesting review.
- If a conversation is resolved by a commit, reply with the commit SHA before marking it resolved.

### Responding to feedback

```bash
# Small fix in response to a comment
git commit -m "fixup: address review comment on error handling"
git push
```

## 5. Merge

Use **squash merge** to keep `main` history clean — one logical change = one commit.

```bash
gh pr merge <number> --squash --delete-branch
```

The squash commit message = PR title (Conventional Commits format).

## 6. After Merge

```bash
git switch main
git pull origin main
git branch -d feat/add-firefox-extension   # clean up local branch
```
