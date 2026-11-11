# How to work in this repo

Simple recipes for everything you'll need. Run the commands in Terminal (Mac) unless it says otherwise.

**The golden rule:** never work directly on `main`. Every change goes on its own branch, then into `main` through a pull request (PR).

The examples say `main` because that's GitHub's default. If your repo's main branch has a different name (like `master`), use that instead. The Claude Code skills figure this out for you.

**Using Claude Code?** Install the team-git plugin once (see https://github.com/kmedi355/team-git-skills), then each section below has a matching command, e.g. `/team-git:save-and-push`. Or just describe what you want ("push my changes") and Claude picks the right one. The commands follow these same steps, plus safety checks for secrets and big files.

---

## One-time setup

> Claude Code: `/team-git:setup` works from any folder, even before you've cloned the repo. It will ask for the repo name.

1. **Make a GitHub account** at github.com if you don't have one, and send your username to whoever owns the repo.
2. **Accept the invite** to the repo from your email.
3. **Install the tools:**
   ```bash
   brew install git gh
   ```
   (No Homebrew? Install it from brew.sh first.)
4. **Log in to GitHub from Terminal:**
   ```bash
   gh auth login
   ```
   Pick: GitHub.com → HTTPS → Yes → Login with a web browser.
5. **Tell git your name and email** (use the email on your GitHub account):
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```
6. **Download the repo.** Its name is in the URL: `github.com/OWNER/REPO`.
   ```bash
   gh repo clone OWNER/REPO
   cd REPO
   ```

---

## I want to see what's going on

> Claude Code: `/team-git:team-status`

Good at the start of each work session.

```bash
git fetch --prune
gh pr list                              # open PRs and who's waiting on review
git log origin/main --oneline -10       # what recently landed in main
git status                              # your own uncommitted changes
```

## I want to start working on something

> Claude Code: `/team-git:start-work`

```bash
git checkout main
git pull
git checkout -b yourname/short-description
```

Branch names: lowercase with hyphens, like `alex/landing-page` or `sam/fix-signup-form`. One branch per task.

## I want to save and push my change

> Claude Code: `/team-git:save-and-push`

```bash
git status                 # see what changed
git diff                   # look at the actual changes
git add <file>             # stage specific files (or `git add .` once you've checked git status)
git commit -m "Add signup form to landing page"
git push -u origin yourname/short-description
```

Commit message tip: finish the sentence "If applied, this commit will ___". "Add signup form" is good; "updates" or "fix" is not.

After the first push, plain `git push` is enough for that branch.

## I want my change to go into main

> Claude Code: `/team-git:open-pr`

```bash
gh pr create --fill
```

Then send the link to a teammate. They look it over, and once it looks good:

```bash
gh pr merge --squash --delete-branch
```

(Or click **Squash and merge** on the PR page on github.com. If your repo doesn't allow squash, use `--merge` instead.)

Then go back to `main` and get the latest:

```bash
git checkout main
git pull
```

## I want to review a teammate's change

> Claude Code: `/team-git:review-pr`

```bash
gh pr list                 # see open PRs
gh pr checkout <number>    # get their branch on your computer to try it
```

Leave comments on the PR page on github.com. Approve when it looks good.

## I want to edit someone else's code

> Claude Code: `/team-git:edit-teammate-code`

Never push straight to a teammate's branch. Propose your change and let them decide.

**Small suggestion:** leave a comment on their PR instead of changing it yourself.

**Bigger change to their open PR:** make your own branch off theirs, then open a PR *into their branch*:

```bash
gh pr checkout <number>                          # get their branch
git checkout -b yourname/their-branch-what-you-changed
# make your edits
git add <file>
git commit -m "Tighten error message on signup form"
git push -u origin yourname/their-branch-what-you-changed
gh pr create --base <their-branch> --fill        # PR into THEIR branch, not main
```

Send them the link. If they like it, they merge it into their branch, and it rides along with their PR. If their PR gets merged first, GitHub automatically points yours at `main` instead.

**Change to code that's already in main:** treat it like any new task. Start a new branch from `main` (see "I want to start working on something") and ask the original author to review.

## My branch is out of date with main

> Claude Code: `/team-git:update-branch`

Someone merged into `main` since you started. Pull their changes into your branch:

```bash
git checkout main
git pull
git checkout yourname/short-description
git merge main
```

## I got a merge conflict

> Claude Code: `/team-git:resolve-conflict`

Git couldn't automatically combine two people's edits to the same lines.

1. Run `git status`. Files with conflicts are listed under "both modified".
2. Open each one. You'll see blocks like this:
   ```
   <<<<<<< HEAD
   your version
   =======
   their version
   >>>>>>> main
   ```
3. Edit it to the version you want and delete the `<<<<<<<`, `=======` and `>>>>>>>` lines.
4. Finish up:
   ```bash
   git add <file>
   git commit
   git push
   ```

Not sure which version is right? Ask the teammate who wrote the other one.

## I messed something up

> Claude Code: `/team-git:undo-mistake`

- **Undo changes to a file you haven't committed yet:** `git restore <file>`
- **Unstage a file you added by mistake:** `git restore --staged <file>`
- **Anything else:** stop and ask in the group chat before trying force-push or reset commands.

---

## Don'ts

- **Never commit secrets:** passwords, API keys, tokens, or `.env` files. The `.gitignore` blocks `.env`, but double-check `git status` before every commit. If a key does get pushed, tell the team right away so it can be revoked.
- **No big files:** videos, datasets, and large images go in Google Drive. If you wouldn't edit it in a text editor, it probably doesn't belong in git.
- **Don't push to main directly.** Always use a branch and a PR.
- **Don't push to a teammate's branch.** Branch off it and open a PR into it instead.
