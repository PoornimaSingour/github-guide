# GitHub Guide for New Joiners

A step-by-step guide to using GitHub — from cloning a repository to creating your first Pull Request.

---

## Prerequisites

Before you begin, make sure you have:

1. **A GitHub account** — sign up at [github.com](https://github.com)
2. **Git installed** — check with `git --version`. If not installed:
   - **macOS**: `brew install git` or `xcode-select --install`
   - **Linux**: `sudo apt install git` (Debian/Ubuntu) or `sudo dnf install git` (Fedora/RHEL)
   - **Windows**: Download from [git-scm.com](https://git-scm.com)
3. **SSH key configured** (recommended) or a GitHub Personal Access Token

---

## Step 1: Configure Git (One-Time Setup)

Set your name and email so commits are attributed to you:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify the configuration:

```bash
git config --list
```

---

## Step 2: Set Up SSH Authentication (Recommended)

SSH lets you push/pull without entering your password each time.

### Generate an SSH key

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Press Enter to accept the default file location, then set a passphrase (or press Enter for none).

### Add the key to your SSH agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Add the public key to GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output, then go to **GitHub → Settings → SSH and GPG keys → New SSH key** and paste it.

### Test the connection

```bash
ssh -T git@github.com
```

You should see: `Hi <username>! You've successfully authenticated`.

---

## Step 3: Clone a Repository

Cloning downloads a full copy of a repository to your machine.

```bash
# Using SSH (recommended)
git clone git@github.com:<owner>/<repo>.git

# Using HTTPS
git clone https://github.com/<owner>/<repo>.git
```

Then enter the cloned directory:

```bash
cd <repo>
```

---

## Step 4: Understand the Repository Structure

After cloning, explore the repo:

```bash
# See the current branch
git branch

# See all branches (including remote)
git branch -a

# Check the status of your working directory
git status

# View recent commit history
git log --oneline -10
```

---

## Step 5: Create a New Branch

Never work directly on `main`. Always create a feature branch:

```bash
# Make sure you're on the latest main
git checkout main
git pull origin main

# Create and switch to a new branch
git checkout -b <branch-name>
```

**Branch naming conventions:**

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/<short-description>` | `feature/add-login-page` |
| Bug fix | `fix/<short-description>` | `fix/header-alignment` |
| Docs | `docs/<short-description>` | `docs/update-readme` |

---

## Step 6: Make Changes

Edit files using your preferred editor (VS Code, vim, etc.), then check what changed:

```bash
# See which files were modified
git status

# See the exact changes (line-by-line diff)
git diff
```

---

## Step 7: Stage Your Changes

Staging selects which changes to include in your next commit.

```bash
# Stage specific files
git add <file1> <file2>

# Stage all changed files
git add .

# Check what's staged
git status
```

**Tip:** Use `git diff --staged` to review exactly what you're about to commit.

---

## Step 8: Commit Your Changes

A commit saves a snapshot of your staged changes with a message describing what you did.

```bash
git commit -m "Short description of what changed"
```

**Writing good commit messages:**

- Use the imperative mood: "Add feature" not "Added feature"
- Keep the first line under 72 characters
- If needed, add a blank line then a longer description

```bash
# Multi-line commit message
git commit -m "Add user login page

- Create login form component
- Add form validation
- Connect to auth API endpoint"
```

---

## Step 9: Push Your Branch to GitHub

Upload your branch to the remote repository:

```bash
# First push (sets the upstream tracking branch)
git push -u origin <branch-name>

# Subsequent pushes (after upstream is set)
git push
```

---

## Step 10: Create a Pull Request (PR)

A Pull Request asks the team to review and merge your changes into `main`.

### Option A: Using the GitHub Web UI

1. Go to the repository on GitHub
2. Click the **"Compare & pull request"** button (appears after you push a branch)
3. Fill in:
   - **Title**: A clear, concise summary of the change
   - **Description**: What changed and why, how to test it
4. Select reviewers
5. Click **"Create pull request"**

### Option B: Using the GitHub CLI (`gh`)

Install the GitHub CLI first: `brew install gh` (macOS) or see [cli.github.com](https://cli.github.com)

```bash
# Authenticate (one-time setup)
gh auth login
```

#### Creating a PR

```bash
# Basic PR — opens an interactive prompt for title and body
gh pr create

# PR with title and description
gh pr create --title "Add login page" --body "Description of changes"

# PR with a multi-line description
gh pr create --title "Add login page" --body "## Summary
- Added login form component
- Connected to auth API

## Test plan
- Verify form validation works
- Check error states"

# Create a PR and open it in the browser to fill in details
gh pr create --web

# Create a draft PR (not ready for review yet)
gh pr create --draft --title "WIP: Add login page" --body "Still working on this"

# Create a PR with specific reviewers
gh pr create --title "Add login page" --reviewer username1,username2

# Create a PR with labels
gh pr create --title "Fix header bug" --label "bug,urgent"

# Create a PR targeting a specific base branch (instead of main)
gh pr create --base develop --title "Add login page"

# Create a PR with an assignee
gh pr create --title "Add login page" --assignee @me
```

#### Viewing and Listing PRs

```bash
# List all open PRs in the repo
gh pr list

# List your own PRs
gh pr list --author @me

# List PRs awaiting your review
gh pr list --search "review-requested:@me"

# View details of a specific PR
gh pr view 42

# View the current branch's PR
gh pr view

# View a PR in the browser
gh pr view 42 --web

# See the diff of a PR
gh pr diff 42
```

#### Reviewing PRs

```bash
# Approve a PR
gh pr review 42 --approve

# Request changes
gh pr review 42 --request-changes --body "Please fix the validation logic"

# Leave a comment (without approving or requesting changes)
gh pr review 42 --comment --body "Looks good overall, minor suggestion on line 15"
```

#### Updating and Managing PRs

```bash
# Check out a PR branch locally (to test or review it)
gh pr checkout 42

# Mark a draft PR as ready for review
gh pr ready 42

# Add reviewers to an existing PR
gh pr edit 42 --add-reviewer username1,username2

# Add labels to an existing PR
gh pr edit 42 --add-label "needs-review"

# Update the PR title or body
gh pr edit 42 --title "Updated title" --body "Updated description"

# Add a comment on a PR
gh pr comment 42 --body "Updated the validation logic as requested"

# Re-request a review after making changes
gh pr edit 42 --add-reviewer username1
```

#### Merging and Closing PRs

```bash
# Merge a PR (default merge commit)
gh pr merge 42

# Merge with a squash (combines all commits into one)
gh pr merge 42 --squash

# Merge with rebase
gh pr merge 42 --rebase

# Merge and delete the branch afterward
gh pr merge 42 --squash --delete-branch

# Close a PR without merging
gh pr close 42

# Reopen a closed PR
gh pr reopen 42
```

#### Checking PR Status

```bash
# See CI/check status on the current branch's PR
gh pr checks

# See CI/check status on a specific PR
gh pr checks 42

# Watch checks until they complete
gh pr checks 42 --watch
```

---

## Step 11: Address Review Feedback

After creating a PR, reviewers may request changes:

```bash
# Make the requested changes in your editor
# Then stage, commit, and push again
git add .
git commit -m "Address review feedback: fix validation logic"
git push
```

The PR updates automatically with your new commits.

To see what reviewers said:

```bash
# View review comments
gh pr view 42 --comments

# Check the current review status
gh pr status
```

---

## Step 12: Keep Your Branch Up to Date

If `main` has new changes while you're working, sync your branch:

```bash
# Fetch the latest changes from remote
git fetch origin

# Rebase your branch on top of the latest main
git rebase origin/main

# If there are conflicts, resolve them, then:
git add <resolved-file>
git rebase --continue

# Push the updated branch (force push needed after rebase)
git push --force-with-lease
```

**Note:** Use `--force-with-lease` instead of `--force` — it's safer because it won't overwrite changes someone else pushed to your branch.

---

## Step 13: Merge and Clean Up

Once your PR is approved and merged (usually done via the GitHub UI):

```bash
# Switch back to main
git checkout main

# Pull the latest (includes your merged changes)
git pull origin main

# Delete your local branch
git branch -d <branch-name>

# Delete the remote branch (if not auto-deleted)
git push origin --delete <branch-name>
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Clone a repo | `git clone <url>` |
| Check status | `git status` |
| Create a branch | `git checkout -b <name>` |
| Stage changes | `git add <file>` |
| Commit | `git commit -m "message"` |
| Push | `git push -u origin <branch>` |
| Pull latest | `git pull origin main` |
| View history | `git log --oneline` |
| See changes | `git diff` |
| Switch branch | `git checkout <branch>` |
| Delete branch | `git branch -d <name>` |
| Create PR | `gh pr create --title "title" --body "desc"` |
| Create draft PR | `gh pr create --draft` |
| List open PRs | `gh pr list` |
| View a PR | `gh pr view 42` |
| Check out a PR | `gh pr checkout 42` |
| Approve a PR | `gh pr review 42 --approve` |
| Merge a PR | `gh pr merge 42 --squash --delete-branch` |
| PR CI status | `gh pr checks` |
| Add reviewers | `gh pr edit 42 --add-reviewer user1` |
| Comment on PR | `gh pr comment 42 --body "message"` |

---

## Common Pitfalls

1. **Committing to `main` directly** — Always create a branch first
2. **Large, unfocused PRs** — Keep PRs small and focused on one change
3. **Vague commit messages** — Be specific about what changed and why
4. **Not pulling before starting work** — Always `git pull origin main` before creating a branch
5. **Using `--force` instead of `--force-with-lease`** — The latter is safer for shared branches
6. **Forgetting to stage files** — Run `git status` before committing to verify

---

## Getting Help

```bash
# Get help on any git command
git help <command>

# Example
git help rebase
```

Useful resources:
- [GitHub Docs](https://docs.github.com)
- [Git Reference](https://git-scm.com/docs)
- [Oh Shit, Git!?!](https://ohshitgit.com) — for fixing common mistakes
