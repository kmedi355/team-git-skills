---
name: resolve-conflict
description: Resolve git merge conflicts - find the conflicted files, explain both versions, combine them with the user's say-so, and finish the merge. Use when someone says "merge conflict", "both modified", or sees <<<<<<< markers.
---

# Resolve a merge conflict

Guide: if the repo has a CONTRIBUTING.md, the matching section is "I got a merge conflict". The steps below work without it.

## 0. Context

Confirm a merge is actually in progress: `git status` should say "You have unmerged paths". If not, there's nothing to resolve; explain what `git status` shows instead.

## 1. Find the conflicted files

```bash
git diff --name-only --diff-filter=U
```

## 2. Explain each one

Read the conflict blocks (between `<<<<<<<`, `=======` and `>>>>>>>`). Explain in plain terms what each side does. The top side is the current branch; the bottom is what's being merged in (its name is after `>>>>>>>`). `git log --oneline -3 MERGE_HEAD -- <file>` shows who changed it on the other side.

## 3. Decide with the user

Propose a combined version and show it. If it's not obvious which is right (both sides change behavior), tell them to check with the teammate who wrote the other side. Don't silently pick one.

## 4. Apply it

Remove every `<<<<<<<`, `=======` and `>>>>>>>` line, then confirm none remain:
```bash
git diff --check
```

## 5. Finish the merge

```bash
git add <file> ...
git commit --no-edit
git push
```

**Want to back out instead?** Before step 5, `git merge --abort` returns everything to how it was before the merge. Ask first; it discards any conflict fixes made so far.
