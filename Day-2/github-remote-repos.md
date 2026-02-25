#  Day 2 — GitHub & Remote Repositories

> **Goal:** Connect your local repo to GitHub. Push, pull, clone, and authenticate securely using SSH and Personal Access Tokens.

---

##  Topics Covered Today

- What is GitHub?
- The 4th Area of Git — Remote Repository
- Clone vs Init + Remote Add
- git push, git pull, git clone
- Managing Remote Connections
- Authentication: SSH Key, PAT Token, HTTPS
- Switching Auth Methods
- Public vs Private Repositories

---

## 1. What is GitHub?

GitHub is one of the most popular platforms for developers to **share code and collaborate** on projects. It is free and easy to use, and has become central to the open-source software movement.

GitHub hosts your Git repository on the cloud so that:
- Multiple developers can access the same codebase
- Your code is backed up remotely
- You can collaborate via Pull Requests and code reviews

---

## 2. The 4th Area of Git — Remote Repository

Adding a remote turns your local Git setup into a distributed team workflow.

```
┌──────────────────────┐   git push   ┌──────────────────────────┐
│   Local Repository   │ ──────────►  │   Remote Repository      │
│   (.git on your PC)  │              │   (GitHub / GitLab etc.) │
│                      │ ◄──────────  │                          │
└──────────────────────┘   git pull   └──────────────────────────┘
```

| Command | Direction | Purpose |
|---------|-----------|---------|
| `git push` | Local → Remote | Send your commits to remote |
| `git pull` | Remote → Local | Get teammates' commits from remote |
| `git clone` | Remote → Local | Download the entire repo for the first time |

---

## 3. Two Starting Scenarios

### Scenario A — Repo exists on GitHub (clone it)
```bash
# Download the entire repo to your local machine — first time only
git clone https://github.com/username/repository.git

# This automatically creates a folder named 'repository'
cd repository

# Clone also sets up 'origin' remote automatically
git remote -v
```

### Scenario B — You have a local repo, push it to GitHub
```bash
# Step 1: Create a new EMPTY repo on GitHub (no README, no .gitignore)

# Step 2: Link your local repo to the remote
git remote add origin https://github.com/username/repository.git

# Step 3: Push your commits for the first time + set upstream
git push -u origin main
# -u sets the default so future pushes just need: git push
```

---

## 4. Daily Push/Pull Workflow

```bash
# Get latest changes from remote (fetch + merge into local)
git pull

# Pull from a specific remote and branch
git pull origin main

# Push your local commits to remote
git push

# Push to a specific remote and branch
git push origin main
```

>  **Golden Rule: Always pull before you push.**  
> This prevents push rejections when teammates have committed before you.

---

## 5. 🔍 Managing Remote Connections

```bash
# Check your current remote connections (shows fetch and push URLs)
git remote -v

# Add a new remote
git remote add origin https://github.com/username/repo.git

# Remove an existing remote
git remote remove origin

# Change/update a remote URL
git remote set-url origin https://github.com/username/new-repo.git
```

>  **`origin`** is just the default name for your remote. You can name it anything but `origin` is the industry convention.

---

## 6.  Authentication — 3 Methods

###  Method 1: SSH Key (Recommended )

SSH is the most secure and convenient method once set up — no password prompts.

```bash
# Step 1: Generate an SSH key pair on your machine
ssh-keygen
# Press Enter to accept defaults
# Keys saved to:
#   ~/.ssh/id_rsa       → Private key (NEVER share this)
#   ~/.ssh/id_rsa.pub   → Public key (share this with GitHub)

# Step 2: Copy your PUBLIC key content
cat ~/.ssh/id_rsa.pub
# Copy the entire output starting with "ssh-rsa ..."

# Step 3: Add to GitHub
# GitHub → Settings → SSH and GPG Keys → New SSH Key
# Paste your public key and give it any name (e.g. "My Laptop")

# Step 4: Clone or set remote using the SSH URL
git clone git@github.com:username/repository.git

# Or set remote to SSH format
git remote add origin git@github.com:username/repository.git
```

---

###  Method 2: Personal Access Token (PAT)

Used when SSH is not available. A token replaces your password.

```bash
# Step 1: Generate a token on GitHub
# GitHub → Settings → Developer Settings
#        → Personal Access Tokens → Tokens (classic) → Generate new token
# Select scopes: repo (minimum required)
# Copy the token — you won't see it again!

# Step 2: Use the token when pushing
git push https://<YOUR_TOKEN>@github.com/username/repository.git

# Or configure the remote URL to include the token
git remote set-url origin https://<YOUR_TOKEN>@github.com/username/repo.git
```

---

### Method 3: HTTPS with Username/Password

Only works if **two-factor authentication (2FA) is NOT enabled** on GitHub. Generally not recommended — GitHub deprecated password auth.

---

## 7.  Switching Authentication Methods

If you cloned with HTTPS and want to switch to SSH:

```bash
# Check current remote
git remote -v
# Output: origin  https://github.com/username/repo.git (fetch)

# Remove the HTTPS remote
git remote remove origin

# Add new SSH remote
git remote add origin git@github.com:username/repo.git

# Verify
git remote -v
# Output: origin  git@github.com:username/repo.git (fetch)
```

---

## 8. Setting Upstream for New Branches

When you create a new local branch and push it for the first time, you must tell Git which remote branch to link it with:

```bash
# Push + set upstream in one command (first push of a branch)
git push -u origin feature-branch

# Or set upstream separately
git push --set-upstream origin feature-branch

# After upstream is set, just use:
git push
git pull
```

---

## 9. Public vs Private Repositories

| Type | Who can see | Who can push | Best for |
|------|------------|--------------|---------|
| **Public** | Everyone | Only collaborators with write access | Open source, portfolios |
| **Private** | Only invited members | Only collaborators with write access | Company projects, personal code |

---

## 10. Practical Exercise — Push to GitHub

```bash
# 1. Create a repo on GitHub named "git-practice" (keep it empty)

# 2. On your machine — create local repo
mkdir git-practice
cd git-practice
git init

# 3. Create a file and commit
echo "# Git Practice Repo" > README.md
git add README.md
git commit -m "Initial commit"

# 4. Add GitHub remote
git remote add origin https://github.com/YOUR_USERNAME/git-practice.git

# 5. Push to GitHub
git push -u origin main

# 6. Verify on GitHub — refresh your repo page

# 7. Make a change and push again
echo "Learning Git with DevOps" >> README.md
git add README.md
git commit -m "Add description to README"
git push
```

---

##  Day 2 Summary Checklist

```
✔ Understand GitHub as a remote Git hosting platform
✔ git clone — download a repo for the first time
✔ git remote add origin <url> — link local to remote
✔ git push — send commits to remote
✔ git pull — get updates from remote
✔ git remote -v — check remote connection
✔ git remote remove origin + git remote add — switch auth method
✔ SSH key setup → ssh-keygen → paste public key to GitHub
✔ PAT token — generated from GitHub Developer Settings
✔ git push -u origin main — first push + set upstream
```

---

##  Quick Reference — Day 2

```bash
git clone <url>                   # Clone remote repo locally
git remote -v                     # Show remote connections
git remote add origin <url>       # Add a remote
git remote remove origin          # Remove a remote
git remote set-url origin <url>   # Change remote URL
git push                          # Push to remote
git push -u origin main           # Push + set upstream
git push origin <branch>          # Push specific branch
git pull                          # Pull from remote
git pull origin main              # Pull specific branch
ssh-keygen                        # Generate SSH key pair
cat ~/.ssh/id_rsa.pub             # View public key to copy
```

---

*Previous → Day 1: Local Git Workflow*  
*Next → Day 3: Branching & Merging*