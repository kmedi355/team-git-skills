---
name: update-branch
description: Bring your branch up to date with the latest default branch (or the teammate branch it's based on) by merging it in. Use when someone says "my branch is out of date", "get the latest changes", "sync with main", or a PR says it's behind or has conflicts.
---

# Bring my branch up to date

Guide: if the repo has a CONTRIBUTING.md, the matching section is "My branch is out of date with main". The steps below work without it.

## 0. Context

- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, falling back to `git symbolic-ref --short refs/remotes/origin/HEAD` (drop the `origin/`). If both fail, ask.
- **What to update from** (`<base>`): if the branch has an open PR, its target (`gh pr view --json baseRefName -q .baseRefName`); otherwise `<default>`.
- **Current branch:** `git branch --show-current`. If it's `<default>`, just `git pull` and stop.

## 1. Nothing uncommitted

If `git status` shows uncommitted changes, commit them first with `save-and-push` (or ask whether to stash them).

## 2. Merge the latest in

```bash
git fetch origin <base>
git merge origin/<base>
```
Use merge, not rebase: the branch is usually already pushed, and rebasing would need a force-push.

## 3. Conflicts?

If it reports conflicts, switch to the `resolve-conflict` skill.

## 4. Push

If it merged cleanly, `git push` so the PR updates.
