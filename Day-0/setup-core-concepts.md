#  Day 0 — Setup & Core Concepts

> **Goal:** Understand what Git is, install it, configure your identity, and grasp the theory of Version Control Systems.

---

##  Topics Covered Today

- What is Version Control?
- Centralized vs Distributed VCS
- Git Hosting Platforms
- Installing Git
- Configuring Identity
- Initialising a Repository

---

## 1. What is Version Control?

A **Version Control System (VCS)** is software that tracks and manages changes to source code over time. It stores every modification in a special database. If a mistake is made, developers can go back in time, compare earlier versions, and fix the code — or roll back completely.

**Why do we need it?**
- Track who changed what and when
- Collaborate across a team without overwriting each other's work
- Roll back broken code to a working state
- Maintain parallel lines of development (branches)

---

## 2. Centralized vs Distributed VCS

### Centralized VCS (CVCS)
One central server holds the entire history. Developers check files in and out.

```
Developers  ──►  Central Server (single point of truth)
```

| Examples | SVN (Subversion), Perforce, IBM ClearCase |
|----------|------------------------------------------|

**Problem:** If the central server goes down, no one can work or access history.

---

### Distributed VCS (DVCS)
Every developer has a **full copy** of the entire repository including all history on their local machine.

```
Developer A (full repo)  ◄──►  Remote Server  ◄──►  Developer B (full repo)
```

| Examples | **Git** , Mercurial, Bazaar |
|----------|------------------------------|

>  **Git is the most popular VCS in the world.**  
> Created by **Linus Torvalds** (creator of the Linux kernel) with 3 design goals:
> - **Distributed** — every developer has the full repo
> - **Efficient** — fast for large codebases
> - **Safe from corruption** — SHA1 hashing protects every object

---

## 3. Git Hosting Platforms (Remote Servers)

You push your local repository to a hosting platform so your team can collaborate.

| Platform | Type | URL |
|----------|------|-----|
| **GitHub** | Cloud (most popular) | github.com |
| **GitLab** | Self-hosted or Cloud | gitlab.com |
| **BitBucket** | Cloud (Atlassian) | bitbucket.org |
| **Azure Source Repos** | Microsoft Cloud | dev.azure.com |
| **AWS CodeCommit** | Amazon Cloud | aws.amazon.com |
| **Gitolite** | Self-hosted | – |

>  **Public vs Private Repositories**
> - **Public** — Anyone can read the code. Used for open source projects.
> - **Private** — Only invited collaborators can access. Used for company/personal projects.

---

## 4.  Installing Git

### All Operating Systems
Download from: **[https://git-scm.com/downloads](https://git-scm.com/downloads)**  
Supports Windows, Linux, and Mac.

### Linux (RHEL / CentOS / Amazon Linux)
```bash
yum install git -y
```

### Linux (Ubuntu / Debian)
```bash
apt install git -y
```

### Mac (Homebrew)
```bash
brew install git
```

###  Verify Installation
```bash
git --version
# Expected output: git version 2.x.x
```

---

## 5.  Configure Your Identity

Before making any commits, Git needs to know who you are. This information is permanently recorded in every commit.

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "you@example.com"

# Check what is currently configured
git config --global user.name
git config --global user.email
```

>  `--global` = applies to ALL repositories on your machine.  
> Without `--global` it applies only to the current repository.

---

## 6.  Initialise a Repository

```bash
# Create a new project folder
mkdir my-project
cd my-project

# Initialise Git — creates a hidden .git folder
git init
```

`git init` creates a hidden `.git` directory. This is where Git stores:
- All your commit history
- Branch information
- Configuration
- Staging index

>  `git init` is **optional** if you are cloning an existing repo from GitHub (covered in Day 2).

---

## 7. How Git Stores Data (Theory)

Every object in Git is identified by a **SHA1 hash** — a 40-character string like `a3f2bc...`. Git uses this hash for:

| Object | What it stores |
|--------|---------------|
| **blob** | File contents |
| **tree** | Directory structure |
| **commit** | Author, message, timestamp, parent commit, tree |
| **tag** | A named reference to a commit |

Two different file contents **cannot** have the same hash. This is how Git guarantees data integrity.

---

##  Day 0 Summary Checklist

```
✔ Git installed → git --version works
✔ Identity configured → git config --global user.name & user.email
✔ Understand Centralized vs Distributed VCS
✔ Know GitHub, GitLab, Bitbucket are hosting platforms
✔ Know what git init does (creates .git folder)
✔ Understand SHA1 hashing ensures data integrity
```

---

##  Quick Reference — Day 0

```bash
# Install (Linux)
yum install git -y

# Verify
git --version

# Configure identity
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Check config
git config --global user.name
git config --global user.email

# Initialise new repo
git init
```

---

*Next → Day 1: Local Git Workflow*