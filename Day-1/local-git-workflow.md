# 📅 Day 1 — Local Git Workflow

> **Goal:** Master the 3 areas of Git and the core local workflow: init → status → add → commit → log.

---

## 📋 Topics Covered Today

- The 3 Areas of Git
- File States in Git
- Core Workflow Commands
- git log & history inspection
- Removing Files
- git diff (local stages)

---

## 1. The 3 Areas of Git (Local)

Git manages your project across 3 distinct areas. Understanding this is the most important concept in Git.

```
┌─────────────────────┐   git add    ┌─────────────────────┐   git commit  ┌──────────────────────┐
│   Working Directory │ ──────────►  │   Staging Area      │ ──────────►   │  Local Repository    │
│                     │              │   (Index)           │               │  (.git folder)       │
│   Your edits live   │ ◄──────────  │   Draft of next     │               │  Permanent history   │
│   here              │  git restore │   commit            │               │  stored here         │
└─────────────────────┘              └─────────────────────┘               └──────────────────────┘
```

| Area | What it is | Key point |
|------|-----------|-----------|
| **Working Directory** | Where you edit files normally | Git sees changes but doesn't track them yet |
| **Staging Area (Index)** | A "draft" of your next commit | You choose exactly what goes into the next commit |
| **Local Repository** | Permanent history in `.git` folder | All your commits live here |

---

## 2. File States in Git

```
[New File Created]
      │
      ▼
  Untracked  ──── git add ────►  Staged  ──── git commit ────►  Unmodified (Committed)
                                                                        │
                                                                   (edit file)
                                                                        │
                                                                        ▼
                                                                    Modified  ──── git add ────► Staged
```

| State | Meaning |
|-------|---------|
| **Untracked** | New file — Git has never seen it before |
| **Modified** | Git knows the file but it has unsaved changes vs last commit |
| **Staged** | Change is queued up for the next commit |
| **Unmodified / Committed** | File matches what is in the last commit — nothing to do |

---

## 3. 🔄 The Core Git Workflow

### Step 1: Check Status
```bash
git status
# Shows: which files are untracked, modified, or staged
```

### Step 2: Stage Your Changes
```bash
# Stage a specific file
git add filename.txt

# Stage ALL changes in the current folder and subfolders
git add .

# Stage all files matching a pattern
git add *.txt

# Stage only already-tracked modified/deleted files (skip untracked)
git add -u

# Stage all changes interactively (choose hunks)
git add -p
```

### Step 3: Check Status Again
```bash
git status
# Confirm what is in the staging area before committing
```

### Step 4: Commit
```bash
git commit -m "Your descriptive commit message"
```

> 📌 **Good commit message tips:**
> - Use present tense: `"Add login page"` not `"Added login page"`
> - Be specific: `"Fix null pointer in user auth"` not `"Fix bug"`
> - Keep first line under 72 characters

---

## 4. What Git Records in Every Commit

Each commit stores:

| Field | Value |
|-------|-------|
| **Author name** | From `git config user.name` |
| **Author email** | From `git config user.email` |
| **Commit message** | What you write with `-m` |
| **Timestamp** | Added automatically |
| **Parent commit ID** | Links this commit to the previous one |
| **Tree (snapshot)** | SHA1 hash of all staged file changes |

---

## 5. 📜 Viewing History with git log

```bash
# Full history with all details
git log

# Compact: one commit per line
git log --oneline

# Visual graph showing branches
git log --oneline --graph

# Last 5 commits only
git log --oneline -5

# Filter by author
git log --author="Your Name"

# History of a specific file
git log -- filename.txt

# Show what changed in each commit
git log -p

# Show stats (lines added/removed)
git log --stat
```

**Sample output of `git log --oneline --graph`:**
```
* a1b2c3d (HEAD -> main) Add user authentication
* e4f5a6b Fix login redirect bug
* 7c8d9e0 Initial project structure
```

---

## 6. 🔍 git diff — Compare Changes (Local Stages)

```bash
# Working Directory vs Staging Area (what you changed but haven't staged yet)
git diff

# Staging Area vs Local Repository (what you staged and will commit)
git diff --staged

# Working Directory vs Local Repository (HEAD)
git diff HEAD
```

> 💡 Use `git diff` before `git add` to review your changes, and `git diff --staged` before `git commit` to confirm what will be committed.

---

## 7. 🗑️ Removing and Moving Files

```bash
# Delete file AND stage the deletion in one command
git rm filename.txt

# Stop tracking a file but KEEP it on disk (useful for files you forgot to .gitignore)
git rm --cached filename.txt

# Rename or move a file
git mv oldname.txt newname.txt
```

> ⚠️ Git does **NOT** track empty directories.  
> To track a folder, add a placeholder file like `.gitkeep` inside it.

---

## 8. Practical Exercise — Your First Repo

```bash
# 1. Create project
mkdir my-first-repo
cd my-first-repo
git init

# 2. Create a file
echo "# My First Git Project" > README.md

# 3. Check status (should show README.md as untracked)
git status

# 4. Stage the file
git add README.md

# 5. Check status again (should show README.md as staged)
git status

# 6. Commit
git commit -m "Initial commit: add README"

# 7. View history
git log --oneline

# 8. Edit the file
echo "Created by: Your Name" >> README.md

# 9. See what changed
git diff

# 10. Stage and commit the change
git add README.md
git commit -m "Add author to README"

# 11. View history again
git log --oneline
```

---

## ✅ Day 1 Summary Checklist

```
✔ Understand the 3 areas: Working Directory → Staging Area → Local Repo
✔ Know the 4 file states: Untracked, Modified, Staged, Committed
✔ git status — always run this first
✔ git add — to stage changes
✔ git commit -m "message" — to save to local repo
✔ git log --oneline --graph — to view history
✔ git diff — to compare before staging
✔ git diff --staged — to review before committing
```

---

## 📌 Quick Reference — Day 1

```bash
git status                    # Check file states
git add <file>                # Stage specific file
git add .                     # Stage everything
git add -u                    # Stage tracked files only
git commit -m "message"       # Save staged changes to local repo
git log                       # Full history
git log --oneline             # Compact history
git log --oneline --graph     # Visual graph
git diff                      # Working dir vs staged
git diff --staged             # Staged vs last commit
git rm <file>                 # Delete and stage deletion
git rm --cached <file>        # Untrack but keep file
git mv old new                # Rename/move a file
```

---

*Previous → Day 0: Setup & Core Concepts*  
*Next → Day 2: GitHub & Remote Repositories*