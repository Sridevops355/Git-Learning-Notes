# 📅 Day 3 — Branching & Merging

> **Goal:** Create branches for parallel development, merge them back safely, and confidently resolve merge conflicts.

---

## 📋 Topics Covered Today

- What is a Branch and why use it
- HEAD and DETACHED HEAD
- Branch Commands (create, switch, rename, delete)
- Merging: Fast-forward vs Three-way
- Merge Conflicts — full resolution steps
- Blocked Checkout — how to fix it

---

## 1. What is a Branch?

A branch is an **independent line of development**. Think of the `main` branch as the stable production trunk. Every feature, bugfix, or experiment gets its own branch so it doesn't disturb the main codebase.

```
main:    A ── B ── C ──────────────── F  (merge commit)
                    \                /
feature:             D ── E ────────
```

**Real-world usage:**
- Each developer works on their own branch independently
- When work is complete and reviewed, it gets merged into `main`
- Default branch name is `main` or `master`

---

## 2. Why Use Branches? — Benefits

| Benefit | Explanation |
|---------|-------------|
| **Isolation** | Changes in one branch don't affect others |
| **Early issue detection** | Test and review before merging to main |
| **Overcome conflicts** | Work in parallel without stepping on each other |
| **Easy rollback** | Delete a bad branch — main stays clean |
| **Code review** | Code verified & approved before deployment |

---

## 3. HEAD — The Current Position Pointer

- **HEAD** is a pointer that tells Git which branch (and commit) you are currently on.
- When you checkout a branch, HEAD moves to point at that branch.
- **DETACHED HEAD** = HEAD points directly to a commit hash instead of a branch name.
  - This happens when you checkout a specific commit: `git checkout abc1234`
  - In this state you can VIEW files but should NOT make new commits
  - To recover: `git checkout main` (or any branch name)

---

## 4. 🌿 Branch Commands

```bash
# List all local branches (* marks the current branch)
git branch

# List all branches including remote-tracking branches
git branch -a

# Create a new branch (stays on current branch)
git branch dev

# Switch to an existing branch
git checkout dev

# Create AND switch to new branch in one command (most common)
git checkout -b dev

# Rename a branch
git branch -m oldname newname

# Delete a branch (safe — refuses if unmerged changes exist)
git branch -d dev

# Force delete a branch (even with unmerged changes — data loss risk!)
git branch -D dev
```

---

## 5. 🔀 Merging Branches

Merge brings changes from one branch **into** another. Always **checkout the target branch first**, then merge the source into it.

```bash
# Step 1: Switch to the branch you want to merge INTO
git checkout main

# Step 2: Merge your feature branch into main
git merge dev
```

---

### Types of Merges

#### Type 1: Fast-Forward Merge
Happens when the target branch (main) has NO new commits since the feature branch was created — the branch history is linear.

```
Before:  main: A ── B
                     \
         dev:         C ── D

After fast-forward merge into main:
         main: A ── B ── C ── D   (pointer just moved forward, no extra commit)
```

```bash
git checkout main
git merge dev   # fast-forward happens automatically
```

#### Type 2: Three-Way Merge (Merge Commit)
Happens when BOTH branches have new commits since they diverged. Git creates a new "merge commit" with two parents.

```
Before:  main: A ── B ── E
                     \
         dev:         C ── D

After three-way merge:
         main: A ── B ── E ── F  (F is the merge commit with 2 parents: E and D)
                     \       /
         dev:         C ── D
```

```bash
git checkout main
git merge dev   # creates a merge commit automatically
```

#### Type 3: Force a Merge Commit (even when fast-forward is possible)
```bash
git merge --no-ff dev   # Always create a merge commit
```

> 💡 Many teams use `--no-ff` to keep a clear record in history of when features were merged.

---

## 6. ⚠️ Resolving Merge Conflicts

### When do conflicts happen?
- **Same branch:** Two developers edited the **same line** of the **same file** and one pushes after the other
- **Different branches:** Two branches both changed the same part of the same file, and you try to merge them

### Step-by-Step Conflict Resolution

#### Step 1: Identify the Conflicting Files
```bash
git status
# Look for: "both modified: filename.txt"
# Or Git shows: CONFLICT (content): Merge conflict in filename.txt
```

#### Step 2: Open the File — Read the Conflict Markers
```
<<<<<<< HEAD
Your changes (from the current branch)
=======
Incoming changes (from the branch being merged)
>>>>>>> feature-branch
```

| Marker | Meaning |
|--------|---------|
| `<<<<<<< HEAD` | Start of YOUR version |
| `=======` | Divider between the two versions |
| `>>>>>>> branch-name` | End of INCOMING version |

#### Step 3: Resolve Manually
- Choose your version, their version, or combine both
- **Delete ALL conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`)
- Save the file

**Example — before:**
```
<<<<<<< HEAD
color: blue;
=======
color: red;
>>>>>>> feature-branch
```

**Example — after resolving (keeping blue):**
```
color: blue;
```

#### Step 4: Stage the Resolved File
```bash
git add filename.txt
```

#### Step 5: Commit and Push
```bash
git commit -m "Resolved merge conflict in filename.txt"
git push
```

#### Final Step: Test Your Code
After resolving, always **test the application** to make sure:
- Merged changes work correctly together
- No logic was accidentally overwritten
- No new bugs introduced

---

### Conflict Resolution — Summary Table

| Step | Command | Purpose |
|------|---------|---------|
| 1 | `git status` | Find all conflicted files |
| 2 | Open file in editor | Read conflict markers |
| 3 | Edit manually | Choose correct code, remove markers |
| 4 | `git add <file>` | Mark conflict as resolved |
| 5 | `git commit -m "..."` | Save the resolution |
| 6 | `git push` | Share resolution with team |
| 7 | Test | Verify merged code works |

---

## 7. 🚫 Blocked Checkout — Real World Scenario

When you try to switch branches with uncommitted changes to a file that also changed in the target branch, Git **blocks you** to prevent data loss:

```bash
# What you see when Git blocks you:
error: Your local changes to the following files would be overwritten by checkout:
    demo.txt
Please commit your changes or stash them before you switch branches.
Aborting
```

### Why does this happen?
Git refuses to switch because it would have to overwrite your uncommitted local changes — losing your work forever.

### ✅ Fix Option 1: Stash (save temporarily and restore later)
```bash
git stash              # Saves your changes aside
git checkout feature   # Switch branches safely
git stash pop          # Restore your saved changes on the new branch
```

### ✅ Fix Option 2: Commit your work first
```bash
git add demo.txt
git commit -m "WIP: save before switching branch"
git checkout feature
```

---

## 8. Practical Exercise — Branch Workflow

```bash
# 1. Start on main, create a feature branch
git checkout -b feature-login

# 2. Make changes and commit
echo "Login page code" > login.html
git add login.html
git commit -m "Add login page"

# 3. Switch back to main
git checkout main

# 4. Simulate a teammate's change on main
echo "Home page update" >> README.md
git add README.md
git commit -m "Update README on main"

# 5. Merge feature-login into main (will be three-way merge)
git merge feature-login

# 6. View the result
git log --oneline --graph

# 7. Clean up the feature branch
git branch -d feature-login
```

---

## ✅ Day 3 Summary Checklist

```
✔ Branches = independent lines of development
✔ Default branch is main or master
✔ git branch dev — create a branch
✔ git checkout dev — switch to branch
✔ git checkout -b dev — create + switch (shortcut)
✔ git merge dev — merge dev into current branch
✔ Fast-forward = no merge commit, linear history
✔ Three-way = both diverged, creates merge commit
✔ git merge --no-ff = always create merge commit
✔ Conflict markers: <<<<<<< HEAD / ======= / >>>>>>>
✔ Conflict resolution: edit → git add → git commit → git push
✔ Blocked checkout: fix with git stash or commit first
✔ git branch -D dev — force delete a branch
```

---

## 📌 Quick Reference — Day 3

```bash
git branch                  # List local branches
git branch -a               # List all branches (local + remote)
git branch dev              # Create branch
git checkout dev            # Switch to branch
git checkout -b dev         # Create + switch
git branch -m old new       # Rename branch
git branch -d dev           # Safe delete
git branch -D dev           # Force delete
git merge dev               # Merge dev into current branch
git merge --no-ff dev       # Merge with explicit commit
git stash                   # Save changes temporarily
git stash pop               # Restore stashed changes
git log --oneline --graph   # View branch history visually
```

---

*Previous → Day 2: GitHub & Remote Repositories*  
*Next → Day 4: Undoing Things & Advanced Tools*