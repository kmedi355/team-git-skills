---
name: open-pr
description: Get a finished branch merged - open a pull request into the default branch (or into a teammate's branch), and after a teammate reviews it, merge it and clean up. Use when someone says "open a PR", "get this into main", "merge my branch", or "it's ready".
---

# Get a change merged

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I want my change to go into main". The steps below work without it.

## 0. Context

- **In a GitHub repo with `gh` logged in?** `gh repo view --json nameWithOwner`. If it fails, use the `setup` skill.
- **Default branch** (`<default>`): `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`.
- **Target branch** (`<base>`): `<default>`, unless this branch was made off a teammate's branch (the `edit-teammate-code` flow), in which case it's their branch. If unsure, check `git log --oneline <default>..HEAD` for their commits and ask.
- **Allowed merge methods:** `gh repo view --json squashMergeAllowed,mergeCommitAllowed,rebaseMergeAllowed`. Prefer squash if allowed, then merge commit.

## Open the PR

1. **Everything committed and pushed?** `git status` should be clean and `git log @{u}..HEAD` empty. If not, run `save-and-push` first.

2. **Up to date with the target?**
   ```bash
   git fetch origin <base>
   git rev-list --left-right --count origin/<base>...HEAD
   ```
   If the first number (behind) is above 0, run `update-branch` first so the PR doesn't conflict.

3. **Review the full change** and summarize it for the user:
   ```bash
   git diff origin/<base>...HEAD --stat
   ```

4. **Open it.** If a PR already exists for this branch (`gh pr view`), share that link instead.
   ```bash
   gh pr create --base <base> --fill
   ```
   Give the user the link to send to a teammate (for a PR into a teammate's branch, that's the branch owner).

## Merge it (only after review)

5. Ask the user to confirm a teammate reviewed it. Don't merge on your own initiative: merging changes the target branch for everyone. A PR into a teammate's branch is theirs to merge, not the user's.

6. Check it can merge cleanly with `gh pr view --json mergeable,reviewDecision`. If it conflicts, run `update-branch` first.

7. Merge with the preferred allowed method, then update the local copy:
   ```bash
   gh pr merge <number> --squash    # or --merge if squash isn't allowed
   git checkout <base>
   git pull
   ```

8. **Branch cleanup: don't assume.** Check the repo setting:
   ```bash
   gh repo view --json deleteBranchOnMerge -q .deleteBranchOnMerge
   ```
   - **`true`:** GitHub deletes the PR's branch on GitHub by itself, usually within a few seconds of the merge, so a check right after merging can still see it. Tell the user GitHub removes it automatically; don't offer to delete it.
   - **`false`:** check whether it's still there with `git ls-remote --exit-code --heads origin <head-branch>` (exit code 2 means it's gone). If it's the user's own branch, delete it with `git push origin --delete <head-branch>`. If it's a teammate's, ask first.

   Either way, if the user has a local copy of the branch and it's their own, remove it with `git branch -d <head-branch>`.
