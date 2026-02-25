# 📅 Day 4 — Undoing Things & Advanced Tools

> **Goal:** Master reverting mistakes at every stage — working directory, staging area, and local repo. Learn git stash in depth, git diff in all variants, and how to read git log like a pro.

---

## 📋 Topics Covered Today

- Reverting Changes — The 3 Zones
- git restore (working dir & staging)
- git reset — 3 modes (Mixed, Soft, Hard)
- git revert — safe undo for public branches
- reset vs revert — when to use which
- git stash — full workflow
- git diff — all variants
- git log — advanced reading

---

## 1. 🔙 Reverting Changes — Overview of the 3 Zones

Every undo in Git targets a specific zone. Pick the right command for where your change currently lives.

```
┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│   Working Directory  │     │   Staging Area       │     │   Local Repository   │
│                      │     │                      │     │                      │
│  git restore <file>  │     │ git restore --staged │     │  git reset HEAD~1    │
│  (discard edits)     │     │  (unstage file)      │     │  (undo commit)       │
└──────────────────────┘     └──────────────────────┘     └──────────────────────┘
```

---

## 2. Zone 1 — Revert from Working Directory

Discard edits to a file and bring it back to the last committed state.

```bash
# Modern command (Git 2.23+)
git restore <filename>

# Old equivalent — still works
git checkout -- <filename>
```

> ⚠️ **WARNING: This is permanent.** Your edits are gone forever — there is no undo for this undo.  
> Only use it when you're sure you want to discard your changes.

**Example:**
```bash
# You accidentally deleted content from app.py
git restore app.py   # Reverts app.py to last committed version
```

---

## 3. Zone 2 — Revert from Staging Area (Unstage)

You staged a file but changed your mind — move it back to working directory without losing your edits.

```bash
# Modern command
git restore --staged <filename>

# Old equivalent
git reset HEAD <filename>
```

> ✅ Your edits are **NOT lost** — the file goes back to "modified" state in working directory.

**Example:**
```bash
# You staged wrong file
git restore --staged wrong-file.txt   # Unstage it (edits still exist)
```

---

## 4. Zone 3 — Revert from Local Repository (Undo Commits)

### Basic: Undo Last Commit
```bash
# Undo the last 1 commit — changes go back to working directory
git reset HEAD~1

# Undo last 2 commits
git reset HEAD~2
```

---

## 5. git reset — 3 Modes Explained

`git reset` undoes commits and moves what was committed back to different areas depending on the mode.

### Syntax
```bash
git reset --<mode> <commit-id or HEAD~n>
```

---

### Mode 1: Mixed (Default)
Removes commits from local repo AND staging area. Changes go to **working directory**.

```bash
git reset --mixed HEAD~1
# OR simply:
git reset HEAD~1
```

```
Before: [Working Dir] → [Staging] → [Commit A] → [Commit B (HEAD)]
After:  [Working Dir (B's changes here)] → [Staging empty] → [Commit A (HEAD)]
```

> ✅ Changes are **kept** in working directory. You can re-stage and re-commit.

---

### Mode 2: Soft
Removes commits from local repo only. Changes go to **staging area**.

```bash
git reset --soft HEAD~1
```

```
Before: [Working Dir] → [Staging] → [Commit A] → [Commit B (HEAD)]
After:  [Working Dir] → [Staging (B's changes here)] → [Commit A (HEAD)]
```

> ✅ Changes are **kept** in staging area — ready to re-commit immediately (useful to rewrite a commit message).

---

### Mode 3: Hard
Removes commits from local repo, staging area, AND working directory. **Everything is gone.**

```bash
git reset --hard HEAD~1
git reset --hard <commit-id>
```

```
Before: [Working Dir] → [Staging] → [Commit A] → [Commit B (HEAD)]
After:  [Working Dir (empty)] → [Staging (empty)] → [Commit A (HEAD)]
```

> ☠️ **WARNING: Changes are PERMANENTLY lost.** Use with extreme caution.

---

### git reset — Summary Table

| Mode | Local Repo | Staging Area | Working Directory | Changes lost? |
|------|-----------|--------------|-------------------|--------------|
| `--soft` | ✅ Reverted | ✅ Kept (staged) | ✅ Kept | No |
| `--mixed` (default) | ✅ Reverted | ✅ Reverted | ✅ Kept | No |
| `--hard` | ✅ Reverted | ✅ Reverted | ✅ Reverted | **YES ☠️** |

---

## 6. git revert — Safe Undo for Public Branches

`git revert` creates a **new commit** that reverses the changes of a specific commit. It does NOT rewrite history — making it safe for shared remote branches.

```bash
# Revert a single commit (opens editor for commit message)
git revert <commit-hash>

# Revert without opening the editor
git revert --no-edit <commit-hash>

# Revert a range of commits (oldest to newest)
git revert <oldest-hash>^..<newest-hash>
# Example:
git revert abc123^..def456
```

**How it works:**
```
Before: A ── B ── C (HEAD)    (C is the bad commit)
After:  A ── B ── C ── D      (D is a new commit that undoes C's changes)
```

---

## 7. git reset vs git revert — When to Use Which

| | `git reset` | `git revert` |
|--|------------|-------------|
| **What it does** | Removes / moves commits | Creates a new reversal commit |
| **Rewrites history?** | ✅ Yes | ❌ No |
| **Safe for shared/remote branches?** | ❌ No — breaks teammates | ✅ Yes |
| **Best for** | Local commits not yet pushed | Commits already pushed to shared branch |
| **Risk** | Can cause conflicts for teammates | Very safe |

> 📌 **Rule of thumb:**  
> - Committed locally only → use `git reset`  
> - Already pushed to remote → use `git revert`

---

## 8. 📦 git stash — Temporary Shelf for Your Work

Stash saves your uncommitted changes in a temporary stack so you can switch context (branches, urgent tasks) without committing half-done work.

### Basic Stash Commands

```bash
# Save current changes to stash (working dir + staging area)
git stash

# List all saved stashes
git stash list
# Output: stash@{0}: WIP on main: abc1234 Your last commit message
#         stash@{1}: WIP on main: def5678 Previous save

# View the diff of a specific stash
git stash show -p stash@{0}
```

### Restoring a Stash

```bash
# Restore stash AND keep it in stash list (safe to apply multiple times)
git stash apply stash@{0}

# Restore stash AND remove it from stash list (most common usage)
git stash pop stash@{0}

# Or simply pop the most recent stash
git stash pop
```

### Removing Stashes

```bash
# Delete a specific stash (without applying)
git stash drop stash@{0}

# Delete ALL stashes
git stash clear
```

---

### Stash Workflow — Common Scenarios

#### Scenario 1: Urgent bugfix while working on a feature
```bash
# You're mid-feature and an urgent bug is reported
git stash                    # Save your in-progress work
git checkout main            # Switch to main
# ... fix the bug, commit, push ...
git checkout feature-branch  # Return to feature
git stash pop                # Restore your work exactly where you left off
```

#### Scenario 2: Blocked branch switch (from Day 3)
```bash
git stash                    # Save conflicting uncommitted changes
git checkout feature         # Now the switch works
git stash pop                # Restore saved changes on the new branch
```

---

### apply vs pop — What's the Difference?

| Command | Restores changes | Removes from stash list |
|---------|-----------------|------------------------|
| `git stash apply` | ✅ Yes | ❌ No (stash stays) |
| `git stash pop` | ✅ Yes | ✅ Yes (stash removed) |

> Use `apply` when you want to apply the same stash to multiple branches.  
> Use `pop` (default) for regular one-time restore.

---

## 9. 🔍 git diff — Full Coverage

```bash
# 1. Working Directory vs Staging Area
#    Shows: what you changed but haven't staged yet
git diff

# 2. Staging Area vs Local Repository (last commit)
#    Shows: what you've staged that will go into the next commit
git diff --staged

# 3. Working Directory vs Local Repository (HEAD)
#    Shows: all changes (staged + unstaged) vs last commit
git diff HEAD

# 4. Local Repo vs Remote Repo
#    Shows: what you have locally that remote doesn't
git diff main origin/main

# 5. Between two branches
#    (run from dev branch to see what's different vs main)
git diff main
```

### When to Use Each

| Command | Use it when... |
|---------|---------------|
| `git diff` | Before `git add` — review what you changed |
| `git diff --staged` | Before `git commit` — confirm what will be committed |
| `git diff HEAD` | See ALL local changes at once |
| `git diff main origin/main` | Check if you're ahead/behind remote |
| `git diff main` | Compare your feature branch against main |

---

## 10. 📜 git log — Advanced Reading

```bash
# Standard full log
git log

# One commit per line (most used)
git log --oneline

# Visual branch graph
git log --oneline --graph

# Visual graph with all branches
git log --oneline --graph --all

# Last 5 commits
git log --oneline -5

# Filter by author
git log --author="Your Name"

# Filter by date
git log --after="2024-01-01" --before="2024-12-31"

# Search commit messages
git log --grep="bugfix"

# Show what changed in each commit (patch)
git log -p

# Show files changed and lines added/removed
git log --stat

# History of a specific file
git log -- filename.txt
```

---

## 11. Practical Exercise — Undo Scenarios

```bash
# Setup: create some commits to practice with
mkdir undo-practice && cd undo-practice
git init
echo "line 1" > file.txt && git add . && git commit -m "commit 1"
echo "line 2" >> file.txt && git add . && git commit -m "commit 2"
echo "line 3" >> file.txt && git add . && git commit -m "commit 3"

git log --oneline
# a1b2c3 commit 3
# d4e5f6 commit 2
# 7g8h9i commit 1

# Practice 1: Soft reset — undo commit 3, keep changes staged
git reset --soft HEAD~1
git status  # file.txt should be in staging

# Practice 2: Mixed reset — undo commit 2, keep changes in working dir
git reset --mixed HEAD~1
git status  # file.txt should be modified (unstaged)

# Practice 3: Revert — safe undo
git log --oneline
git revert --no-edit <commit-hash>  # Creates a new undo commit
git log --oneline  # Notice the new revert commit added
```

---

## ✅ Day 4 Summary Checklist

```
✔ git restore <file> — discard working dir changes (permanent!)
✔ git restore --staged <file> — unstage (changes kept)
✔ git reset HEAD~1 — undo last commit (mixed mode default)
✔ git reset --soft — undo commit, keep changes staged
✔ git reset --mixed — undo commit, keep changes in working dir
✔ git reset --hard — undo commit AND destroy changes (careful!)
✔ git revert — safe undo by creating new commit (use for pushed commits)
✔ git stash — save changes temporarily
✔ git stash pop — restore stashed changes
✔ git stash list / show / drop / clear
✔ git diff — compare working dir vs staged
✔ git diff --staged — compare staged vs last commit
✔ git diff main origin/main — compare local vs remote
```

---

## 📌 Quick Reference — Day 4

```bash
# Restore
git restore <file>              # Discard working dir changes
git restore --staged <file>     # Unstage a file

# Reset
git reset HEAD~1                # Undo last commit (mixed — default)
git reset --soft HEAD~1         # Undo commit, keep staged
git reset --mixed HEAD~1        # Undo commit, keep in working dir
git reset --hard HEAD~1         # Undo commit + delete changes ☠️
git reset --hard <commit-id>    # Reset to specific commit

# Revert
git revert <hash>               # Safe undo (new commit)
git revert --no-edit <hash>     # No editor popup
git revert <old>^..<new>        # Revert a range

# Stash
git stash                       # Save changes
git stash list                  # List stashes
git stash show -p stash@{0}     # See stash contents
git stash pop                   # Restore + remove from list
git stash apply stash@{0}       # Restore + keep in list
git stash drop stash@{0}        # Delete without applying
git stash clear                 # Delete all stashes

# Diff
git diff                        # Working dir vs staged
git diff --staged               # Staged vs last commit
git diff HEAD                   # All changes vs last commit
git diff main origin/main       # Local vs remote
git diff main                   # Current branch vs main
```

---

*Previous → Day 3: Branching & Merging*  
*Next → Day 5: Team Workflows & Best Practices*