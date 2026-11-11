---
name: start-work
description: Start a new task on a fresh branch off the latest default branch (usually main). Use when someone says "start working on X", "new branch", "begin a task", or is about to make changes while on the default branch.
---

# Start working on something

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I want to start working on something". The steps below work without it.

## 0. Context

- **In a git repo?** `git rev-parse --is-inside-work-tree`. If not, use the `setup` skill.
- **Default branch** (called `<default>` below): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, falling back to `git symbolic-ref --short refs/remotes/origin/HEAD` (drop the `origin/`). If both fail, ask.
- **Your name for the branch:** first name from `git config user.name`, lowercased. If it's empty or looks like a placeholder, ask.
- **The task:** if the user didn't say what they're working on, ask.

## 1. Don't lose uncommitted work

Run `git status`. If there are uncommitted changes, ask whether they belong to this new task (carry them over) or to the current branch (commit them there first with the `save-and-push` skill). Don't discard anything.

## 2. Get the latest and branch off it

```bash
git checkout <default>
git pull
git checkout -b <name>/<short-description>
```
`<short-description>`: 2-4 lowercase words joined by hyphens describing the task (e.g. `landing-page`, `fix-signup-form`). If that branch already exists, ask whether to continue on it (`git checkout <branch>`) or pick a new name.

## 3. Wrap up

Confirm the branch they're on and remind them to use `save-and-push` when they have something to save.
