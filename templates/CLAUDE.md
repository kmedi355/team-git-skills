# <project name>

<!-- Project-specific: fill this in. -->
<One or two sentences: what the project is, the deadline, and how many teammates. Note if most are new to git.>
<!-- End project-specific section. Everything below is generic. -->

Explain git steps in plain language; assume teammates may be new to git.

## Rules (always, even without a slash command)

- **Never commit or push to the default branch** (usually `main`; check with `gh repo view --json defaultBranchRef`). All work goes on a `name/short-description` branch and in through a pull request. If asked to commit while on the default branch, create a branch first (`start-work` skill).
- **Never push to a teammate's branch.** To change their work, branch off theirs and open a PR into their branch (`edit-teammate-code` skill).
- **Never commit secrets:** API keys, tokens, passwords, `.env` files. Keys go in `.env` (gitignored), with a blank placeholder in `.env.example`. If a key was already pushed, tell the user to revoke it at the provider immediately.
- **No large files** (videos, datasets, big images). Those belong outside git.
- **Never force-push, `reset --hard` a pushed branch, or rewrite shared history** unless the user explicitly asks after you've explained the consequences.
- **Don't merge a pull request** unless the user confirms a teammate reviewed it. Use the repo's allowed merge method, preferring squash.
- **Ask before anything visible to teammates**: posting PR comments or reviews, merging, deleting branches.
- **Don't assume repo details.** Get the repo, default branch, and user from git/`gh`; ask if they aren't available.

## Workflows

Each task has a skill in the team-git plugin (`/team-git:<skill>`), or in `.claude/skills/` if the skills were copied into this repo. Each matches a section of [CONTRIBUTING.md](CONTRIBUTING.md). Use the skill when the request matches:

| Request | Skill |
|---|---|
| Create the team's repo | `new-team-repo` |
| First-time setup | `setup` |
| What's going on / where are we | `team-status` |
| Start a task | `start-work` |
| Commit / push | `save-and-push` |
| Open or merge a PR | `open-pr` |
| Review a teammate's PR | `review-pr` |
| Change someone else's code | `edit-teammate-code` |
| Branch is behind | `update-branch` |
| Merge conflict | `resolve-conflict` |
| Undo a mistake | `undo-mistake` |

## Commit messages

Imperative and specific, under 72 characters: "Add signup form to landing page", not "updates".
