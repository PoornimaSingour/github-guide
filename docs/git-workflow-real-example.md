# Git Workflow: Real-World Example — Rebase, Squash & Push

A practical walkthrough of git commands used while working on a bug fix.

---

## 1. Review Changes

| Command | Purpose |
|---------|---------|
| `git diff main...HEAD -- <file>` | View code changes on your branch |
| `git log --oneline main...HEAD` | List commits on your branch vs main |

## 2. Check If Rebase Is Needed

| Command | Purpose |
|---------|---------|
| `git fetch origin main` | Fetch latest main from remote |
| `git log --oneline main..HEAD` | Show your commits ahead of main |
| `git log --oneline HEAD..origin/main` | Check how far behind main you are |
| `git merge-base HEAD origin/main` | Find where your branch diverged |
| `git log <merge-base>..origin/main -- <dir>/` | Check if main changed the same files |

## 3. Rebase

| Command | Purpose |
|---------|---------|
| `git rebase origin/main` | Rebase your branch onto latest main |
| `git log --oneline -3` | Verify the rebase result |

## 4. Stage, Squash & Commit

| Command | Purpose |
|---------|---------|
| `git status --short` | Check uncommitted changes |
| `git diff --staged --stat` | Review staged changes |
| `git diff --stat` | Review unstaged changes |
| `git add <file>` | Stage specific files |
| `git commit --amend -m "message"` | Squash into the previous commit |
| `git log -1 --format="Author: %an <%ae>"` | Verify commit author |
| `git diff --stat HEAD~1` | Verify final commit contents |

## 5. Push

| Command | Purpose |
|---------|---------|
| `git push --force-with-lease origin <branch>` | Force-push rebased branch (safer than --force) |
