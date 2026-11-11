---
name: team-status
description: Read-only snapshot of a team repo - open PRs and who's waiting on review, teammates' active branches, what recently landed in the default branch, and whether your own branch is behind or has unpushed work. Use at the start of a work session, or when someone asks "what's going on", "where are we", "what's everyone working on", or "what do I need to review".
---

# Team status

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I want to see what's going on". The steps below work without it.

This skill only reads. Don't commit, push, merge, pull, or post anything; suggest the right skill instead.

## 0. Context

- **In a git repo?** `git rev-parse --is-inside-work-tree`. If not, use the `setup` skill.
- **Who you are:** `gh api user -q .login` (GitHub) and `git config user.name`.
- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, falling back to `git symbolic-ref --short refs/remotes/origin/HEAD` (drop the `origin/`).
- If `gh` isn't installed or logged in, say so, point to `setup`, and do the git-only steps below.

## 1. Refresh

Safe: only updates what git knows about the remote, never your files.
```bash
git fetch --prune origin
```

## 2. Open pull requests

```bash
gh pr list --json number,title,author,headRefName,baseRefName,updatedAt,reviewDecision,isDraft,url
```

## 3. Active branches without a PR

```bash
git for-each-ref --sort=-committerdate refs/remotes/origin --format='%(refname:short)|%(committerdate:relative)|%(authorname)|%(subject)'
```
Skip `origin/<default>`, `origin/HEAD`, and branches already in step 2.

## 4. Recently merged

```bash
git log origin/<default> --since="7 days ago" --format='%h|%an|%ar|%s'
```

## 5. You

```bash
git branch --show-current
git status --short
git rev-list --left-right --count origin/<default>...HEAD
```
If the branch has an upstream, also `git rev-list --count @{u}..HEAD` for unpushed commits.

## Report

Keep it short, grouped like this (omit empty groups):

- **Needs your review:** open, non-draft PRs by someone else with no approval yet. Lead with these, especially PRs whose `baseRefName` is one of the user's branches (teammates proposing changes to their work; they're the one who merges those).
- **Your open PRs:** whether each is approved, waiting, or needs changes.
- **Teammates are working on:** their branches/PRs, one line each with how recently.
- **Landed this week:** one line each.
- **You:** current branch; uncommitted changes; unpushed commits; how far behind `<default>`.

End with at most two suggested next steps that name the skill, e.g. "You're 3 commits behind → `/update-branch`", "Sam's PR #4 is waiting on you → `/review-pr`", "You have uncommitted changes → `/save-and-push`".
