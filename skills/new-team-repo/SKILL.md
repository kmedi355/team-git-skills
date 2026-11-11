---
name: new-team-repo
description: Create a new GitHub repo for a team project, ready for beginners - private repo, .gitignore, README, CONTRIBUTING.md and CLAUDE.md from this plugin's templates, squash-only merges, and teammate invites. Use when someone says "create a repo for our team/group/project", "set up a repo for my team", or "start a new team project on GitHub".
---

# Create a team repo

For whoever in the group creates the repo. Teammates then use the `setup` skill.

## 0. Gather what you need (ask for anything missing, one question at a time)

- **GitHub logged in?** `gh auth status`. If not, use the `setup` skill first. If several accounts are logged in, confirm which one should own the repo (`gh auth switch` changes the active one).
- **Owner:** the active account (`gh api user -q .login`), or an organization the user names.
- **Repo name:** ask. Lowercase, hyphens, short (e.g. `capstone-app`). Check it's free: `gh repo view <owner>/<name>` should fail.
- **One-line description:** what the project is and when it's due.
- **Visibility:** private, unless the user explicitly asks for public.
- **Stack:** Node, Python, or not sure yet (only affects `.gitignore`).
- **Teammates' GitHub usernames:** optional; invites can be sent later.

Show the plan (owner/name, visibility, description, invites) and get a yes before creating anything. Creating a repo and sending invites are visible to others.

## 1. Create and clone

Ask where to put it (default: home folder), then:
```bash
gh repo create <owner>/<name> --private --description "<description>" --clone
cd <name>
```

## 2. Add the starter files

This plugin's templates are in `templates/` next to this `skills/` folder (resolve the path from this SKILL.md's location: `../../templates/`).

- `.gitignore`: copy `templates/gitignore`. For Python, the Python block is already included; nothing to add.
- `CONTRIBUTING.md`: copy `templates/CONTRIBUTING.md` as-is.
- `CLAUDE.md`: copy `templates/CLAUDE.md`, then fill in the project-specific intro at the top (project, deadline, team size) from what the user told you.
- `README.md`: write a short one: the one-line description, "Setup: send <owner> your GitHub username, accept the invite, then follow CONTRIBUTING.md. The repo is `<owner>/<name>`", and a pointer to CONTRIBUTING.md for how the team works.

Commit and push straight to the default branch. This first commit is the only exception to "never push to main", since there's nothing to branch from yet.
```bash
git add .gitignore README.md CONTRIBUTING.md CLAUDE.md
git commit -m "Add README, contributing guide, team rules, and .gitignore"
git push -u origin HEAD
```

## 3. Settings

```bash
gh repo edit <owner>/<name> --enable-squash-merge --enable-merge-commit=false --enable-rebase-merge=false --delete-branch-on-merge
```

Try to protect the default branch (require a PR):
```bash
gh api -X PUT repos/<owner>/<name>/branches/<default>/protection --input - <<'EOF'
{"required_status_checks":null,"enforce_admins":false,"required_pull_request_reviews":{"required_approving_review_count":0},"restrictions":null}
EOF
```
On a free account with a private repo, GitHub refuses this ("Upgrade to GitHub Pro"). That's fine: tell the user the "no pushing to main" rule is enforced by CLAUDE.md and the team's habits instead. Don't make the repo public to get around it.

## 4. Invite teammates

For each username the user gave:
```bash
gh repo add-collaborator <owner>/<name> <username> --permission push
```
If that subcommand isn't available in their `gh` version, use `gh api -X PUT repos/<owner>/<name>/collaborators/<username> -f permission=push`. Each teammate gets an email to accept.

## 5. Hand-off

Give the user the repo URL and a short message they can paste to the team:
> Repo: https://github.com/<owner>/<name>. Accept the GitHub invite, then follow CONTRIBUTING.md. With Claude Code: install the team-git plugin, open Claude Code anywhere and run `/team-git:setup`.
