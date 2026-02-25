#  Day 5 — Team Workflows & Best Practices

> **Goal:** Work effectively in a team — fix push rejections, cherry-pick commits, use .gitignore, understand forks, rebase, and protect your main branch.

---

##  Topics Covered Today

- Push Rejected — Causes & Fixes
- Pull Strategies: Fast-forward, Merge, Rebase
- git cherry-pick — apply specific commits
- cherry-pick vs merge comparison
- git rebase — linear history
- Interactive Rebase (squash, reword, edit, drop)
- .gitignore — exclude files from tracking
- Git Fork — contributing to others' repos
- Branch Protection Rules
- Setting Upstream for Branches

---

## 1.  Push Rejected — "Unable to Push"

### The Problem
```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'origin'
```

### Why It Happens
When two developers work on the same branch:
- Developer A pushes a commit
- Developer B (you) tries to push WITHOUT pulling Developer A's latest commit first
- Your local branch is **behind** the remote — Git blocks you to prevent overwriting Developer A's work
- The difference comes from mismatched local vs remote commit IDs

### The Solution: **Always Pull Before You Push**

```bash
# If there is only one remote branch
git pull

# If there are multiple branches
git pull origin <branch-name>

# Then push your changes
git push
```

---

## 2.  Pull Strategies — Fast-forward vs Merge vs Rebase

### Strategy 1: Fast-Forward Pull 
Works **only** when your local branch has NO new commits since the last pull — there is no commit divergence.

```
Remote: A ── B ── C
Local:  A ── B         (behind by 1)

After git pull (fast-forward):
Local:  A ── B ── C    (pointer just moved forward, no extra commit)
```

```bash
git pull    # fast-forward happens automatically when possible
```

---

### Strategy 2: Pull with Merge (--no-rebase)
When both local and remote have new commits (diverged), Git merges them and creates an **extra merge commit**.

```
Remote: A ── B ── C
Local:  A ── B ── D     (you committed D locally)

After git pull --no-rebase:
Local:  A ── B ── D ── M   (M is the merge commit)
                  \   /
                   C──
```

```bash
git pull --no-rebase
# or just: git pull  (merge is the default when diverged)
```

>  **Safer for teams unfamiliar with rebase.** Full history preserved.  
>  Creates an extra merge commit in history which can look cluttered.

---

### Strategy 3: Pull with Rebase (--rebase)
Applies your local commits **on top of** the fetched remote commits. Keeps history linear with no extra merge commit. Your local commits get **new commit hashes** (renamed).

```
Remote: A ── B ── C
Local:  A ── B ── D     (you committed D locally)

After git pull --rebase:
Local:  A ── B ── C ── D'  (D' is your commit replayed on top, new hash)
```

```bash
git pull --rebase
```

>  Cleaner linear history — looks like everyone worked in sequence.  
>  **Never rebase commits that have already been pushed to a shared remote branch** — it rewrites history and causes conflicts for teammates.

---

### Pull Strategy Comparison

| Strategy | Command | Creates Merge Commit? | History Style | Best For |
|----------|---------|----------------------|---------------|---------|
| Fast-forward | `git pull` | ❌ No | Linear | When no divergence |
| Merge | `git pull --no-rebase` | ✅ Yes | Branched | Safe team default |
| Rebase | `git pull --rebase` | ❌ No | Linear | Clean history preference |

---

## 3.  git cherry-pick — Apply Specific Commits

Cherry-pick lets you take **one specific commit** from any branch and apply it to your current branch — without merging the entire branch.

**Common use case:** A bug was fixed on a feature branch. You need that fix on `main` immediately without waiting for the full feature to be ready.

### How to Cherry-Pick

```bash
# Step 1: Find the commit hash you want
git log --oneline feature-branch
# abc1234 Fix critical null pointer bug
# def5678 Add new feature (not ready yet)

# Step 2: Switch to the target branch
git checkout main

# Step 3: Cherry-pick only the bugfix commit
git cherry-pick abc1234

# Step 4: Push
git push
```

### Cherry-Pick a Range of Commits
```bash
git cherry-pick <oldest-hash>^..<newest-hash>
# Example:
git cherry-pick abc123^..def456
```

---

## 4. cherry-pick vs merge — Comparison

| Feature | `git cherry-pick` | `git merge` |
|---------|------------------|------------|
| **Purpose** | Apply specific commit(s) from one branch | Combine **all** changes from one branch |
| **History Impact** | Adds only selected commits | Merges complete branch history |
| **Common Use Case** | Hotfixes, picking specific features/bugfixes | Feature complete — bring all dev work in |
| **Example** | "Just give me that one bugfix" | "Feature is done, merge everything" |

---

## 5.  git rebase — Linear History

Rebase moves (replays) your commits to start from the tip of another branch, making history look like everyone worked in sequence.

```bash
# You're on feature branch, you want main's latest commits included
git checkout feature
git rebase main

# After rebase, merge back cleanly (will be fast-forward)
git checkout main
git merge feature
```

**Before rebase:**
```
main:    A ── B ── C
feature: A ── B ── D ── E
```

**After `git rebase main` (on feature branch):**
```
main:    A ── B ── C
feature: A ── B ── C ── D' ── E'   (D and E replayed on top of C)
```

---

## 6.  Interactive Rebase — Edit History

Interactive rebase lets you rewrite, combine, reorder, or delete commits before sharing them. Use it to clean up your commit history before merging.

```bash
# Interactively manage the last 3 commits
git rebase -i HEAD~3

# Interactively manage the last 4 commits
git rebase -i HEAD~4
```

This opens a text editor showing your commits:
```
pick a1b2c3 Add login page
pick d4e5f6 Fix typo in login
pick 7g8h9i Add validation to login
```

### Available Actions

| Action | What it does |
|--------|-------------|
| `pick` | Keep the commit as-is |
| `reword` | Keep the commit but change the message |
| `edit` | Pause and make changes to the commit |
| `squash` | Combine this commit INTO the previous one (keeps both messages) |
| `fixup` | Combine into previous commit (discards this message) |
| `drop` | Remove the commit entirely |

### Common Interactive Rebase Tasks

#### Squash multiple commits into one
```
pick  a1b2c3 Add login page
squash d4e5f6 Fix typo in login      ← squash into above
squash 7g8h9i Add validation          ← squash into above
```
Result: One clean commit "Add login page"

#### Reword a commit message
```
reword a1b2c3 Fix typo in login page  ← opens editor to rename
pick   d4e5f6 Add validation
```

#### Drop a commit
```
pick   a1b2c3 Add login page
drop   d4e5f6 Debug code (shouldn't be here)
pick   7g8h9i Add validation
```

>  **Never interactive-rebase commits already pushed to a shared remote branch.**

---

## 7.  .gitignore — Exclude Files from Tracking

The `.gitignore` file tells Git which files and folders to **never track or stage** — even if they exist in your project folder.

### Create .gitignore
```bash
# Create in the root of your repository
touch .gitignore
```

### Common .gitignore Patterns
```gitignore
# Ignore all .log files anywhere in the project
*.log

# Ignore a specific folder
temp/
node_modules/

# Java — ignore build output
target/

# Python — ignore cache
__pycache__/
*.pyc
venv/

# .NET — ignore build output
bin/
obj/

# Environment files (NEVER commit secrets!)
.env
.env.local

# OS files
.DS_Store        # Mac
Thumbs.db        # Windows

# IDE files
.idea/
.vscode/
*.suo
```

### Key Facts about .gitignore

- Git will **not track** files and folders listed in `.gitignore`
- The `.gitignore` file itself **IS tracked** by Git — commit it!
- Patterns are case-sensitive on Linux/Mac
- Use `*` as wildcard: `*.log` ignores all log files
- Use `/` to target directories: `temp/` ignores the temp folder
- Use `!` to un-ignore: `!important.log` tracks this one log file

>  **Generate .gitignore for your tech stack automatically:**  
> [https://www.toptal.com/developers/gitignore](https://www.toptal.com/developers/gitignore)

---

## 8.  Git Fork — Contributing Without Write Access

**Forking** creates your own personal copy of someone else's repository under your GitHub account. You can make any changes to your fork without affecting the original.

### When to Fork
- Contributing to an open-source project you don't own
- Experimenting with changes on a public repo
- Making customizations that are not suitable for the original repo

### Fork Workflow
```
Original Repo (someone else's)
        │
        ▼
   Fork to YOUR GitHub account  ← Click "Fork" button on GitHub
        │
        ▼
   git clone your-fork-url      ← clone YOUR fork locally
        │
        ▼
   git checkout -b my-feature   ← create a branch in your fork
        │
   Make changes → commit → push to your fork
        │
        ▼
   Open a Pull Request          ← propose your changes to the original repo
        │
        ▼
   Original repo owner reviews and merges (or rejects)
```

### Fork vs Clone vs Branch

| | Fork | Clone | Branch |
|-|------|-------|--------|
| **Creates copy where?** | Your GitHub account | Your local machine | Inside same repo |
| **Affects original?** | ❌ Never | ❌ Never | ❌ Not until merged |
| **Write access needed?** | ❌ No | ❌ No (for public) | ✅ Yes |
| **Use case** | Open source contribution | Start working on a repo | Feature development on your own repo |

---

## 9.  Branch Protection Rules

Branch protection rules prevent **unauthorized or accidental changes** to critical branches like `main`, `master`, or `release`. This is configured on GitHub under: `Settings → Branches → Add Rule`.

### Common Protection Rules

| Rule | Purpose |
|------|---------|
| **Require pull request before merging** | No direct `git push` to this branch allowed — all changes must go via PR |
| **Require approvals** | At least 1 (or more) reviewer must approve the PR before merge |
| **Require status checks to pass** | CI/CD pipeline (tests, linting) must pass before merge allowed |
| **Require linear history** | Only allow rebase merges — no merge commits |
| **Restrict who can push** | Only specific users or teams can merge |
| **Require signed commits** | All commits must be GPG signed for identity verification |

### Why Branch Protection Matters (DevOps context)
- Enforces code review → better code quality
- CI/CD must pass → no broken code reaches main
- Audit trail → every change has a linked PR with discussion
- Prevents accidental `git push --force` overwriting shared history

---

## 10. Setting Upstream for New Branches

```bash
# Push a new branch for the first time + set upstream tracking
git push -u origin feature-branch

# Alternative way to set upstream
git push --set-upstream origin feature-branch

# After upstream is set, simple commands work:
git push   # pushes to the linked remote branch
git pull   # pulls from the linked remote branch

# Check which remote branch your local branch is tracking
git branch -vv
```

---

## 11. Complete Team Workflow — Putting It All Together

```bash
# 1. Start fresh — pull latest main
git checkout main
git pull

# 2. Create a feature branch
git checkout -b feature-user-auth

# 3. Work on your feature — make commits
git add .
git commit -m "Add JWT authentication"
git add .
git commit -m "Add token refresh logic"

# 4. Meanwhile, teammates pushed to main — rebase to stay current
git fetch origin
git rebase origin/main

# 5. Push your branch
git push -u origin feature-user-auth

# 6. Open a Pull Request on GitHub for code review

# 7. After approval, merge PR on GitHub

# 8. Clean up locally
git checkout main
git pull
git branch -d feature-user-auth
```

---

##  Day 5 Summary Checklist

```
✔ Push rejected → git pull first, then git push
✔ Fast-forward: no divergence, pointer just moves forward
✔ --no-rebase: safe merge, creates extra merge commit
✔ --rebase: linear history, local commits replayed on top
✔ cherry-pick: apply specific commits from any branch
✔ cherry-pick vs merge: specific commits vs full branch history
✔ git rebase main: update feature branch with latest main
✔ git rebase -i HEAD~n: clean up commits before sharing
✔ Interactive rebase actions: pick, reword, squash, drop
✔ .gitignore: list files/folders to never track
✔ Commit .gitignore so whole team uses same exclusions
✔ Fork: your own copy of someone else's repo on GitHub
✔ Branch protection rules: enforce PR, reviews, CI/CD checks
✔ git push -u origin branch: push + set upstream at once
```

---

##  Quick Reference — Day 5

```bash
# Fix push rejection
git pull                            # pull before push
git pull --no-rebase                # pull with merge commit
git pull --rebase                   # pull with rebase

# Cherry-pick
git cherry-pick <hash>              # Apply specific commit
git cherry-pick <old>^..<new>       # Apply range of commits

# Rebase
git rebase main                     # Replay commits on top of main
git rebase -i HEAD~3                # Interactive: last 3 commits
# Actions: pick | reword | squash | fixup | edit | drop

# .gitignore
touch .gitignore                    # Create file
# Add patterns: *.log, temp/, node_modules/, .env

# Upstream
git push -u origin <branch>         # Push + set upstream
git push --set-upstream origin <b>  # Same
git branch -vv                      # Check tracking info
```

---

##  Master Cheat Sheet — All 6 Days

| Command | Description |
|---------|-------------|
| `git init` | Initialise new repo |
| `git config --global user.name` | Set/check name |
| `git config --global user.email` | Set/check email |
| `git status` | Show working tree status |
| `git add <file>` / `git add .` | Stage changes |
| `git commit -m "msg"` | Commit staged changes |
| `git log --oneline --graph` | Visual history |
| `git clone <url>` | Clone remote repo |
| `git remote -v` | Show remotes |
| `git remote add origin <url>` | Add remote |
| `git remote remove origin` | Remove remote |
| `git push` / `git push -u origin main` | Push to remote |
| `git pull` | Pull from remote |
| `git pull --rebase` | Pull with rebase |
| `git branch dev` | Create branch |
| `git checkout dev` / `git checkout -b dev` | Switch / Create+Switch |
| `git branch -D dev` | Delete branch |
| `git merge dev` | Merge branch |
| `git merge --no-ff dev` | Merge with commit |
| `git restore <file>` | Discard working dir changes |
| `git restore --staged <file>` | Unstage |
| `git reset HEAD~1` | Undo last commit (keep changes) |
| `git reset --hard HEAD~1` | Undo + delete changes ☠️ |
| `git revert <hash>` | Safe undo (new commit) |
| `git stash` | Save changes temp |
| `git stash pop` | Restore stash |
| `git stash list/show/drop/clear` | Manage stashes |
| `git diff` | Working dir vs staged |
| `git diff --staged` | Staged vs last commit |
| `git diff main origin/main` | Local vs remote |
| `git cherry-pick <hash>` | Apply specific commit |
| `git rebase main` | Replay on top of branch |
| `git rebase -i HEAD~n` | Interactive rebase |

---

##  What to Learn Next

| Topic | What it covers |
|-------|---------------|
| **Git Tags** | Mark release versions: `git tag v1.0.0` |
| **Pull Requests (PRs)** | Team code review workflow on GitHub |
| **Git Hooks** | Auto-run scripts on git events (pre-commit, pre-push) |
| **Git Submodules** | Include one repo inside another |
| **Git Reflog** | Recover "lost" commits after hard resets |
| **Git Bisect** | Binary search to find the commit that broke something |
| **GitHub Actions** | CI/CD pipelines triggered by git events |
| **Git Worktree** | Work on multiple branches simultaneously |

---

>  **VS Code Tip:** Install the **GitLens** extension for visual blame, history, heatmaps, and branch management directly inside your editor.

---

*Previous → Day 4: Undoing Things & Advanced Tools*  
*Course Complete! 