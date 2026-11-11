---
name: undo-mistake
description: Safely undo git mistakes - discard uncommitted edits, unstage files, fix the last commit, recover from committing on the default branch or a teammate's branch - without force-pushing. Use when someone says "undo", "I messed up", "wrong branch", or "committed something by accident".
---

# I messed something up

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I messed something up". The steps below work without it.

## 0. Context

- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, falling back to `git symbolic-ref --short refs/remotes/origin/HEAD` (drop the `origin/`). If both fail, ask.
- **What happened:** `git status` and `git log --oneline -5`, plus `git log @{u}..HEAD` to see what's not pushed yet. Explain it to the user before changing anything.

Prefer the least destructive fix. Ask before anything that throws away work.

| Situation | Fix |
|---|---|
| Throw away uncommitted edits to a file | `git restore <file>`. **Confirm first: this can't be undone.** |
| Staged a file by mistake | `git restore --staged <file>` (keeps the edits) |
| Typo in the last commit message, **not pushed yet** | `git commit --amend -m "<new message>"` |
| Forgot a file in the last commit, **not pushed yet** | `git add <file>` then `git commit --amend --no-edit` |
| Committed on `<default>` (or a teammate's branch) by accident, **not pushed yet** | `git branch <name>/<desc>` (saves the work), then `git reset --hard origin/<branch-you-were-on>`, then `git checkout <name>/<desc>`. Confirm with the user before the reset. |
| A bad commit **already pushed** to your own branch | `git revert <commit>` then push. This adds an undo commit; history stays intact. |
| Pushed to `<default>` or a teammate's branch by accident | Don't try to fix it with force-push. Tell the user to let the team know; usually `git revert <commit>` on that branch (with the owner's OK) is the fix. |
| A secret (API key, password) was committed | Not pushed: unstage it or amend it out. Pushed: tell the user to revoke the key at the provider **now** and tell the team. Removing it from git doesn't make it safe again. |

**Never** run `git push --force`, `git reset --hard` on a pushed branch, or rewrite shared history without the user explicitly asking after you've explained what it does. If it's not on this list, stop and suggest asking the team.
