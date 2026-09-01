# 🚀 Bongo DevOps Core

> **A hands-on Git & GitHub practice repository** — documenting every command, every concept, every mistake, and every fix. Built while learning DevOps from scratch.

---

## 👨‍💻 About This Repo

This repository is a **live record** of my Git & GitHub learning journey. Every commit, every branch, every conflict — done intentionally, with full understanding of what's happening under the hood.

---

## 📚 What I Practiced

### 🔧 1. Environment Setup
Getting Git ready for the first time.

```bash
git config --global user.name "t-tazbir"
git config --global user.email "thaminultazbir2@gmail.com"
```

- Configured global identity so every commit carries my name
- Set up **SSH key** for passwordless GitHub authentication
- Understood the difference between `--global` and project-level config

---

### 📁 2. Repository Initialization & First Push

```bash
mkdir bongo_devops_core
cd bongo_devops_core
git init
touch README.md
git add .
git commit -m "chore: initial repository setup"
git remote add origin "<SSH_URL>"
git push -u origin main
```

- Created a local repo from scratch
- Linked it to GitHub using SSH remote
- Understood what `origin` and `-u` flag mean

---

### 🔒 3. Secrets & .gitignore

```bash
vim .env
vim .gitignore
git status
```

- `.env` holds secrets (API keys, passwords) — **never pushed to GitHub**
- `.gitignore` tells Git which files to ignore
- Learned that `.gitignore` itself **should** be pushed — so teammates follow the same rules

---

### 🌿 4. Branching & Feature Development

```bash
git checkout -b feature/system_optimization
touch kernel_tuning.txt
git add .
git commit -m "Kernel added"
git push origin feature/system_optimization
```

- Created an isolated branch for new work
- Pushed branch to GitHub
- Opened a **Pull Request** and merged into `main` via GitHub UI
- Understood why branches exist: protect `main`, work in parallel

---

### 🔄 5. Fetching & Syncing with Remote

```bash
git fetch origin
git merge origin/main
```

- After merging PR on GitHub, synced local `main` with remote
- Learned the difference between `git fetch` (safe, no merge) vs `git pull` (fetch + merge)

---

### 🐛 6. Multiple Bug Fixes with Separate Commits

```bash
git add web_fix.conf
git commit -m "Fixed Website frontend"
git add db_fix.conf
git commit -m "Fixed Database"
git push -u origin main
```

- Each fix committed **separately** — one commit, one purpose
- Keeps history clean and blame traceable

---

### 🕵️ 7. Searching Inside Git History

```bash
git log -S "8080" -p
```

- Searched for a specific string (`8080`) across all commits
- `-S` flag finds commits where that string was added or removed
- `-p` shows the actual diff

---

### 📦 8. Git Stash — Context Switching

```bash
vim feature.py        # started new work, didn't commit
git stash -u          # stashed everything including untracked files
vim main.py           # fixed critical bug
git add main.py
git commit -m "Critical bug fixed for main.py"
git push origin main
git stash pop         # brought feature.py work back
```

- Used `git stash` to temporarily shelve work
- Learned that `git stash` only saves **tracked** files
- Used `git stash -u` to include **untracked** files too
- `git stash pop` = restore + delete from stash

| Command | What it does |
|---|---|
| `git stash` | Stash tracked changes |
| `git stash -u` | Stash tracked + untracked |
| `git stash list` | See all stashes |
| `git stash pop` | Restore and remove from stash |
| `git stash apply` | Restore but keep in stash |

---

### 🧹 9. Squash Merge — Clean History

```bash
git checkout -b feature/system_optimization
# made 3 messy commits:
git commit -m "typo fix"
git commit -m "typo fix again"
git commit -m "Now actually fixed"

git checkout main
git merge --squash feature/system_optimization
git commit -m "Optimized System Configuration"
git push origin main
```

- 3 messy commits → 1 clean commit in `main`
- Production history stays professional
- Teammates don't see "typo fix" 10 times

```
Without squash:  main ← typo fix ← typo fix again ← Now actually fixed
With squash:     main ← Optimized System Configuration ✅
```

---

### ⚔️ 10. Merge Conflict — On Purpose

```bash
# On main:
echo "main: System optimization enabled" > optimization.txt
git commit -m "Update line 1 from main"

# On feature branch (same line, different text):
echo "feature: Performance tuning active" > optimization.txt
git commit -m "Update line 1 from feature"

# Merge → CONFLICT!
git checkout main
git merge feature/conflict_test
```

Git showed:
```
<<<<<<< HEAD
main: System optimization enabled
=======
feature: Performance tuning active
>>>>>>> feature/conflict_test
```

**Resolution:**
```bash
echo "System optimization enabled with performance tuning" > optimization.txt
git add optimization.txt
git commit -m "Resolve merge conflict in optimization.txt"
git push origin main
```

- Learned that conflict happens when **both branches modify the same line differently**
- Fast-forward merge does NOT create conflicts (only one branch moved)
- Manually removed `<<<<`, `====`, `>>>>` markers and chose the final content

---

### 💀 11. Reflog Recovery — Bringing Back "Deleted" Work

```bash
echo "this work will be lost... or will it?" > recovery.txt
git add recovery.txt
git commit -m "Important work that will be deleted"

# Nuclear command:
git reset --hard HEAD~1   # commit is "gone"! 😱

# Recovery:
git reflog                # Git's secret diary — shows everything
git reset --hard 214c6a0  # back from the dead 😎

git push origin main
```

- `git reset --hard` moves HEAD but **doesn't delete data immediately**
- `git reflog` records every HEAD movement — even "deleted" commits
- Any lost commit can be recovered within Git's cleanup window (~90 days)

```
git log    →  shows clean history
git reflog →  shows EVERYTHING, including "deleted" commits
```

---

## 🧠 Key Concepts Understood

| Concept | One-liner |
|---|---|
| `origin` | Your fork / remote repo name |
| `upstream` | The original repo you forked from |
| `HEAD` | Where you currently are in history |
| `Fast-forward` | No conflict — one branch simply ahead of other |
| `Squash` | Many commits → one clean commit |
| `Stash` | Temporary drawer for unfinished work |
| `Reflog` | Git's secret diary — nothing is truly lost |
| `.gitignore` | Rules for what Git should ignore |
| `SSH key` | Digital key-lock — no password needed |

---

## 🔁 The Git Workflow I Now Follow

```
1. git fetch origin          → sync with remote first
2. git checkout -b feature/x → new branch for new work
3. git add <file>            → stage only what's needed
4. git commit -m "message"   → one commit, one purpose
5. git push origin feature/x → push to remote
6. Pull Request on GitHub    → review before merging
7. git branch -d feature/x   → clean up after merge
```

---

## 📌 Commands Quick Reference

```bash
# Setup
git config --global user.name "name"
git config --global user.email "email"
ssh-keygen -t ed25519 -C "email"

# Basics
git init
git status
git add .
git commit -m "message"
git push origin main

# Branching
git checkout -b feature/x
git branch -a
git branch -D branch-name

# Remote
git remote add origin <url>
git fetch origin
git merge origin/main

# Stash
git stash -u
git stash pop
git stash list

# History & Recovery
git log --oneline
git log -S "keyword" -p
git reflog
git reset --hard <hash>

# Advanced
git merge --squash feature/x
git merge --no-ff feature/x
```

---

<div align="center">

**Built with curiosity, broken with confidence, fixed with understanding.**

*— t-tazbir*

</div>
