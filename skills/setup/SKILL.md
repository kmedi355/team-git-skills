---
name: setup
description: One-time machine setup to work in a team GitHub repo - install git and gh, log in to GitHub, set git name/email, get access to and clone the repo. Use when someone says "set me up", "first time", "how do I get the repo", or git/gh isn't installed or authenticated.
---

# One-time setup

Guide: if the repo has a CONTRIBUTING.md, the matching section is "One-time setup". The steps below work without it.

Check each step and only do what's missing. Never type passwords or tokens for the user; logins happen in their browser.

## 0. Which repo?

Work out the repo (`OWNER/REPO`) before anything else:
- If the current folder is a git repo: `gh repo view --json nameWithOwner -q .nameWithOwner`, or parse `git remote get-url origin` (`github.com[:/]OWNER/REPO(.git)`).
- Otherwise, if the user mentioned a repo or a `github.com/...` link, use that.
- Otherwise **ask**: "What's the GitHub repo? It's in the URL: github.com/OWNER/REPO." Don't guess.

## 1. Tools installed?

```bash
git --version; gh --version
```
If either is missing, install with the platform's package manager: macOS `brew install git gh` (no `brew`? tell them to install Homebrew from brew.sh and stop), Windows `winget install Git.Git GitHub.cli`, Linux see cli.github.com.

## 2. Logged in to GitHub?

```bash
gh auth status
```
If not, tell the user to run `! gh auth login` themselves (GitHub.com → HTTPS → Yes → Login with a web browser). It's interactive, so don't run it for them.

## 3. Git identity set?

```bash
git config --global user.name; git config --global user.email
```
If either is empty, ask for their name and the email on their GitHub account, then set them with `git config --global`.

## 4. Access to the repo?

```bash
gh repo view OWNER/REPO --json nameWithOwner
```
"Could not resolve" means no access yet: they need to accept the invite email, or send the repo owner (`OWNER`, or whoever runs the project) their GitHub username (`gh api user -q .login`).

## 5. Repo cloned?

If they're not already inside it, ask where they want it (default: home folder), then:
```bash
gh repo clone OWNER/REPO
```

Finish by telling them the next steps are `team-status` (see what's going on) and `start-work` (begin a task).
