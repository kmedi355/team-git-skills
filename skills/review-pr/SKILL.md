---
name: review-pr
description: Review a teammate's pull request - list open PRs, summarize what one changes, flag problems, try it locally, and leave a review. Use when someone says "review a PR", "look at Sam's change", or "what PRs are open".
---

# Review a teammate's change

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I want to review a teammate's change". The steps below work without it.

## 0. Context

**In a GitHub repo with `gh` logged in?** `gh repo view --json nameWithOwner`. If it fails, use the `setup` skill. Note who the user is: `gh api user -q .login`.

## 1. Find it

```bash
gh pr list
```
If there's more than one and the user didn't say which, ask. Highlight PRs targeting the user's own branch (`baseRefName`): those are teammates proposing changes to their work.

## 2. Read it

```bash
gh pr view <number>
gh pr diff <number>
```
Summarize for the user: what it changes, anything that looks wrong or unclear, and any secrets or big files that slipped in.

## 3. Try it locally if useful

Commit or stash your own work first (check `git status`), then `gh pr checkout <number>`. Afterwards, go back with `git checkout <previous-branch>`.

## 4. Leave the review

Draft the comment with the user and show it before posting; posting is visible to the whole team.
```bash
gh pr review <number> --comment --body "<text>"    # questions or suggestions
gh pr review <number> --approve --body "<text>"    # looks good
```
GitHub won't let you approve your own PR; if the user is the author, leave a comment instead.

If the PR targets the user's own branch and they approve, they're the one who merges it: use the `open-pr` skill's merge steps.
