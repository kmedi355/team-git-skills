---
name: edit-teammate-code
description: Propose changes to code a teammate wrote, without touching their branch - comment on their PR, or branch off their branch and open a PR into it for them to approve, or start a fresh branch for code already merged. Use when someone says "edit Sam's code", "fix their branch", "change something someone else wrote", or tries to push to a teammate's branch.
---

# Edit someone else's code

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I want to edit someone else's code". The steps below work without it.

**Never push commits directly to a teammate's branch.** Propose changes; the owner decides.

## 0. Context

- **In a GitHub repo with `gh` logged in?** `gh repo view --json nameWithOwner`. If it fails, use the `setup` skill.
- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`.
- **Your name:** first name from `git config user.name`, lowercased; ask if empty.
- **Which code:** find their PR with `gh pr list` (ask which, if unclear). Note its number, author, and branch (`headRefName`, called `<their-branch>` below). If there's no open PR, the code is probably already in `<default>`: that's case C.

Then pick the case; ask if unclear.

## A. Small suggestion on an open PR

Don't edit. Use the `review-pr` skill to leave a comment.

## B. Bigger change to a teammate's open PR

1. **Save your own work first:** check `git status`; commit with `save-and-push` or ask about stashing.
2. **Branch off theirs:**
   ```bash
   gh pr checkout <number>
   git pull
   git checkout -b <name>/<their-branch>-<what-you-changed>
   ```
   Keep the name short, e.g. `alex/sam-signup-form-copy-edits`.
3. **Make the edits**, then commit and push with the `save-and-push` skill (it pushes your new branch, not theirs).
4. **Open a PR into their branch**, not `<default>`:
   ```bash
   gh pr create --base <their-branch> --fill
   ```
   Add a line to the PR body saying it's a suggested change to PR #<number>.
5. **Hand it to them.** Give the user the link to send to the author. The author reviews and merges it into their branch (`review-pr`, then `open-pr`'s merge steps). If the author's PR merges into `<default>` first, GitHub automatically retargets this PR to `<default>`.

## C. Code that's already in the default branch

Treat it as a new task: `start-work`, make the change, `save-and-push`, `open-pr`. Suggest asking the original author to review (`git log --format='%an' -- <file> | head -1` shows who last touched it).
