---
name: save-and-push
description: Commit and push changes safely - checks you're not on the default branch, scans for secrets and big files, writes a clear commit message, pushes the branch. Use when someone says "save my work", "commit", "push", or "push my changes".
---

# Save and push a change

Guide: if the repo has a CONTRIBUTING.md, the matching sections are "I want to save and push my change" and "Don'ts". The steps below work without it.

## 0. Context

- **In a git repo?** `git rev-parse --is-inside-work-tree`. If not, use the `setup` skill.
- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, falling back to `git symbolic-ref --short refs/remotes/origin/HEAD` (drop the `origin/`). If both fail, ask.
- **Current branch:** `git branch --show-current`.

## 1. Not on the default branch, and not on a teammate's

- If the current branch is `<default>`: stop and use the `start-work` skill to create a branch first; uncommitted changes come along.
- If the branch looks like someone else's (a different name prefix, or `gh pr view --json author` shows another author): don't push to it. Use the `edit-teammate-code` skill instead.

## 2. Look before staging

```bash
git status
git diff
```
Summarize what changed for the user in a sentence or two.

## 3. No secrets

For every new or modified file: if it's `.env`, `.env.*` (except `.env.example`), `*.key`, `*.pem`, or under `secrets/`, don't stage it. Skim the added lines in `git diff` for anything that looks like an API key, token, or password. If you find one, stop and tell the user exactly where; don't stage it.

## 4. No big files

```bash
git status --porcelain | awk '{print $2}' | xargs -I{} du -h {} 2>/dev/null
```
Anything over a few MB (videos, datasets, large images): ask whether it should live outside git (e.g. Google Drive) instead.

## 5. Stage and commit

Stage specific files (`git add <file> ...`); `git add .` is fine only after steps 2-4 showed nothing unexpected.

Commit message: finish the sentence "If applied, this commit will ___". Imperative, specific, under 72 characters ("Add signup form to landing page"). Never "fix", "updates", or "WIP". If it needs "and", consider two commits. Show the message before committing.
```bash
git commit -m "<message>"
```

## 6. Push

```bash
git push -u origin <branch>
```
Tell the user it's pushed and that the `open-pr` skill gets it reviewed and merged when it's ready.
