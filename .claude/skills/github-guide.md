---
skill: github-guide
description: Walk new joiners through GitHub usage — from clone to PR. Explains each step interactively and can verify their setup.
user-invocable: true
---

# GitHub Guide for New Joiners

Help new team members learn GitHub workflows step by step. The full written guide lives at `docs/github-guide.md` in this repo.

## What to do

When the user invokes this skill, act as a friendly mentor walking them through GitHub basics. Follow these steps:

### 1. Assess their current setup

Check what they already have configured:

```bash
git --version
git config --global user.name
git config --global user.email
ssh -T git@github.com 2>&1 || true
gh --version 2>/dev/null || echo "gh CLI not installed"
```

Report what's ready and what needs setup.

### 2. Guide them through whatever they need

Based on their setup status and what they ask about, walk them through the relevant steps from the guide. The full step-by-step flow is:

1. **Git configuration** — `git config` for name and email
2. **SSH setup** — Generate key, add to agent, add to GitHub
3. **Cloning** — `git clone` a repository
4. **Branching** — Create a feature branch with good naming
5. **Making changes** — Edit files, check status, view diffs
6. **Staging** — `git add` specific files
7. **Committing** — Write good commit messages
8. **Pushing** — `git push -u origin <branch>`
9. **Creating a PR** — Via GitHub UI or `gh pr create`
10. **Handling review feedback** — Make changes, push again
11. **Keeping branch up to date** — `git fetch` + `git rebase`
12. **Merge and cleanup** — Delete old branches

### 3. Be interactive

- Ask the user which step they want help with, or start from the beginning
- After explaining each step, ask if they want to try it or move on
- If they run into errors, help them troubleshoot
- Point them to `docs/github-guide.md` for the full written reference

### 4. Key principles to emphasize

- Never commit directly to `main` — always use a branch
- Keep PRs small and focused
- Write clear commit messages in imperative mood
- Use `--force-with-lease` instead of `--force`
- Pull latest `main` before creating a new branch
- Run `git status` frequently to stay aware of your state
