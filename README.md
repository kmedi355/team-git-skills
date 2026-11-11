# team-git-skills

Claude Code skills that walk a project team through GitHub without anyone needing to know git: set up, branch, save and push safely, open and review pull requests, suggest changes to a teammate's work, fix conflicts, and undo mistakes. Built for class project teams where most people are new to git. Works in any GitHub repo.

## Install (once per computer)

In Claude Code, run:

```
/plugin marketplace add kmedi355/team-git-skills
/plugin install team-git@team-git-skills
```

Or from Terminal:

```bash
claude plugin marketplace add kmedi355/team-git-skills
claude plugin install team-git@team-git-skills
```

You'll also need [Claude Code](https://claude.com/claude-code) and a GitHub account. `/team-git:setup` handles the rest (git, the GitHub CLI, logging in, cloning).

## Getting your team going

1. **One person creates the repo:** open Claude Code and run `/team-git:new-team-repo`. It asks for a name and your teammates' GitHub usernames, then creates a private repo with a contributing guide, team rules for Claude, and sensible settings, and sends the invites.
2. **Everyone else:** accept the invite email, open Claude Code in any folder, and run `/team-git:setup`.
3. **Day to day:** use the commands below, or just say what you want ("push my changes", "what's everyone working on?").

## Commands

| Command | What it does |
|---|---|
| `/team-git:new-team-repo` | Create the team's repo with a guide, team rules, `.gitignore`, and invites |
| `/team-git:setup` | Install and log in to git/GitHub, set your name, clone the repo |
| `/team-git:team-status` | See open PRs, what teammates are working on, and whether you're behind (read-only) |
| `/team-git:start-work` | Start a task on a new branch off the latest main |
| `/team-git:save-and-push` | Commit and push, after checking for secrets and big files and that you're not on main |
| `/team-git:open-pr` | Open a pull request; merge it once a teammate has reviewed it |
| `/team-git:review-pr` | Summarize a teammate's PR, try it, and draft your review |
| `/team-git:edit-teammate-code` | Suggest changes to someone else's work through a PR into their branch, never pushing to it |
| `/team-git:update-branch` | Bring your branch up to date with main |
| `/team-git:resolve-conflict` | Walk through a merge conflict and finish the merge |
| `/team-git:undo-mistake` | Safely undo common mistakes, without force-pushing |

## The rules the skills follow

- Nobody commits to `main` (or whatever the default branch is called). Work goes on a branch and in through a pull request.
- Nobody pushes to a teammate's branch. Suggest changes with a PR into it instead.
- No secrets (API keys, passwords, `.env` files) and no big files in git.
- No force-pushing or rewriting history.
- Nothing is merged without a teammate's review, and Claude asks before anything teammates will see (comments, merges).
- Nothing is assumed: the repo, default branch, and your name come from git and GitHub, and Claude asks when it can't tell.

## Prefer not to install a plugin?

Copy the skills into your team's repo instead, and everyone who opens Claude Code there gets them automatically:

1. Copy `skills/*` into your repo's `.claude/skills/`.
2. Copy `templates/CONTRIBUTING.md` and `templates/CLAUDE.md` to your repo's root, and `templates/gitignore` to `.gitignore`. Fill in the top of `CLAUDE.md`.
3. In `CONTRIBUTING.md`, drop the `team-git:` prefix from the commands (e.g. `/save-and-push`).

The catch: the skills only exist once someone has cloned the repo, so first-time setup happens by hand (CONTRIBUTING.md walks through it).

## What's in here

```
.claude-plugin/   plugin and marketplace manifests
skills/           one folder per command
templates/        CONTRIBUTING.md, CLAUDE.md, and gitignore used by new-team-repo
```

## Feedback

Found a problem or have an idea? Open an issue on this repo.
