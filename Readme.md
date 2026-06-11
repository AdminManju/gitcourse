# 🚀 Introduction to Git & GitHub

> A complete reference guide covering Git version control and GitHub collaboration — from first setup to advanced workflows, with **real-world examples** throughout.

---

## 📋 Table of Contents

- [What is Git?](#what-is-git)
- [What is GitHub?](#what-is-github)
- [Git vs GitHub](#git-vs-github)
- [Installation & Setup](#installation--setup)
- [Core Concepts](#core-concepts)
- [Git Workflow](#git-workflow)
- [Essential Commands](#essential-commands)
  - [Repository Setup](#repository-setup)
  - [Staging & Committing](#staging--committing)
  - [Branching](#branching)
  - [Merging & Rebasing](#merging--rebasing)
  - [Remote Repositories](#remote-repositories)
  - [Undoing Changes](#undoing-changes)
  - [Stashing](#stashing)
  - [Tagging](#tagging)
  - [Inspection & Logging](#inspection--logging)
- [GitHub Features](#github-features)
  - [Fork & Pull Request Workflow](#fork--pull-request-workflow)
  - [Issues](#issues)
  - [GitHub Actions (CI/CD)](#github-actions-cicd)
  - [GitHub Pages](#github-pages)
- [.gitignore](#gitignore)
- [Git Configuration](#git-configuration)
- [Advanced Tips](#advanced-tips)
- [Common Errors & Fixes](#common-errors--fixes)
- [Cheat Sheet](#cheat-sheet)

---

> 🏗️ **Running Example:** Throughout this guide, we follow **ShopEase** — a team of 4 developers building an e-commerce web app. You'll see Priya (frontend), Arjun (backend), Sara (DevOps), and Kiran (intern) use Git & GitHub on real tasks.

---

## What is Git?

**Git** is a free, open-source **distributed version control system** created by Linus Torvalds in 2005. It tracks changes in files over time, letting you:

- Revert files or entire projects to a previous state
- Compare changes over time
- See who last modified something and when
- Work on multiple features simultaneously without breaking things
- Collaborate with teams without overwriting each other's work

Every developer has a **full copy** of the project history on their machine — no single point of failure.

### 💡 Real-World Analogy

Think of Git like **Google Docs version history** — but for your entire codebase. If Priya accidentally deletes the checkout page, she can roll back to yesterday's version in seconds instead of rewriting everything from memory.

---

## What is GitHub?

**GitHub** is a **cloud-based hosting platform** for Git repositories. It adds a layer of collaboration tools on top of Git:

| Feature | Description |
|---|---|
| Remote Hosting | Store your repos in the cloud |
| Pull Requests | Propose, review, and merge code changes |
| Issues | Track bugs, features, and tasks |
| Actions | Automate CI/CD pipelines |
| Pages | Host static websites for free |
| Codespaces | Cloud dev environments |
| Wiki | Project documentation |
| Security | Vulnerability scanning, Dependabot |

> Other similar platforms: **GitLab**, **Bitbucket**, **Azure DevOps**

### 💡 Real-World Analogy

If Git is Microsoft Word's Track Changes feature, GitHub is **SharePoint or Google Drive** — the shared storage where the whole team accesses, reviews, and approves those changes.

---

## Git vs GitHub

| | Git | GitHub |
|---|---|---|
| Type | Software (CLI tool) | Web platform / service |
| Works offline? | ✅ Yes | ❌ Requires internet |
| Owned by | Community (open source) | Microsoft |
| Function | Version control | Collaboration & hosting |
| Install needed? | ✅ Yes | ❌ No (browser-based) |

---

## Installation & Setup

### Install Git

**Windows:**
```bash
# Download from https://git-scm.com/download/win
# Or use winget:
winget install --id Git.Git -e --source winget
```

**macOS:**
```bash
# Using Homebrew
brew install git

# Or via Xcode CLI tools
xcode-select --install
```

**Linux (Debian/Ubuntu):**
```bash
sudo apt update
sudo apt install git
```

**Verify installation:**
```bash
git --version
# git version 2.44.0
```

---

### Configure Git (First-Time Setup)

Run these once after installing Git — they stamp your identity on every commit.

```bash
git config --global user.name "Priya Sharma"
git config --global user.email "priya@shopease.dev"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"

# Verify everything looks right
git config --list
# user.name=Priya Sharma
# user.email=priya@shopease.dev
# init.defaultbranch=main
# core.editor=code --wait
```

> 📝 **Why this matters:** Every `git commit` you make will show `Author: Priya Sharma <priya@shopease.dev>`. Your teammates and GitHub use this to know who wrote what.

---

### Authenticate with GitHub

**Option 1 — HTTPS with Personal Access Token (PAT):**
```bash
# When Git prompts for a password, paste your PAT instead
# Generate at: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
# Scopes needed: repo, workflow
```

**Option 2 — SSH (Recommended for daily use):**
```bash
# 1. Generate SSH key
ssh-keygen -t ed25519 -C "priya@shopease.dev"
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (/home/priya/.ssh/id_ed25519): [press Enter]
# Enter passphrase: ****

# 2. Start the agent and add your key
eval "$(ssh-agent -s)"
# Agent pid 59566
ssh-add ~/.ssh/id_ed25519

# 3. Copy the public key
cat ~/.ssh/id_ed25519.pub
# ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... priya@shopease.dev

# 4. Paste it on GitHub → Settings → SSH and GPG keys → New SSH key

# 5. Test the connection
ssh -T git@github.com
# Hi priya-sharma! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Core Concepts

### The Three States of Git

Every file in a Git project lives in one of three states:

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  Working Directory  →  Staging Area (Index)  →  Repository      │
│                                                                  │
│   cart.js (edited)       cart.js (staged)       cart.js v1→v2   │
│   (Modified)             (git add)              (git commit)     │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

**Real-world walkthrough:**

1. Priya edits `src/cart.js` — it's now **Modified** (Working Directory)
2. She runs `git add src/cart.js` — it moves to **Staged** (Index)
3. She runs `git commit -m "feat: add quantity selector to cart"` — it's now **Committed** (Repository)

| State | Description |
|---|---|
| **Working Directory** | Files you're actively editing on disk |
| **Staging Area** | Files marked to go into the next commit |
| **Repository** | Committed snapshots stored in `.git/` |

### Key Terminology

| Term | Real-World Meaning |
|---|---|
| **Repository (repo)** | The `shopease-web` project folder tracked by Git |
| **Commit** | A save point — like `"added payment gateway"` at 3:45pm |
| **Branch** | Priya's `feature/cart-redesign` — her own copy to work in |
| **HEAD** | "You are here" — points to the commit you're currently on |
| **Remote** | The `shopease-web` repo hosted on GitHub |
| **Origin** | The GitHub URL your local repo was cloned from |
| **Clone** | Kiran downloads the full project to his laptop for the first time |
| **Fork** | A contributor copies the repo to their own GitHub account |
| **Merge** | Priya's cart changes get combined into `main` |
| **Rebase** | Replaying Priya's commits on top of the latest `main` |
| **Pull Request (PR)** | Priya asks Arjun to review and approve her changes |
| **Upstream** | The original `shopease-web` repo a contributor forked from |

---

## Git Workflow

### Typical Solo Workflow

```
1. git init              ← Kiran starts tracking his practice project
2. (writes index.html)
3. git add .             ← Stages all new files
4. git commit -m "init" ← First save point
5. git push              ← Uploads to his GitHub
```

### Team Collaboration Workflow (ShopEase)

```
1. git clone git@github.com:shopease/shopease-web.git   ← Kiran joins the team
2. git switch -c feature/add-wishlist                   ← Works in his own branch
3. (writes wishlist feature)
4. git add . && git commit -m "feat: add wishlist page" ← Saves progress
5. git push origin feature/add-wishlist                 ← Puts it on GitHub
6. Opens Pull Request: feature/add-wishlist → main
7. Priya reviews → requests changes → Kiran updates
8. Arjun approves → PR merged
9. git switch main && git pull origin main              ← Everyone syncs
```

---

## Essential Commands

### Repository Setup

```bash
# ── Starting fresh ──────────────────────────────────────

# Sara creates a new repo for ShopEase's internal tooling
mkdir shopease-tools
cd shopease-tools
git init
# Initialized empty Git repository in /home/sara/shopease-tools/.git/

# ── Joining an existing project ──────────────────────────

# Kiran joins the team and gets the full codebase
git clone git@github.com:shopease/shopease-web.git
# Cloning into 'shopease-web'...
# remote: Enumerating objects: 1842, done.
# Receiving objects: 100% (1842/1842), 4.23 MiB | 5.10 MiB/s, done.

# Clone into a custom folder name
git clone git@github.com:shopease/shopease-web.git my-shopease

# Shallow clone — faster for large repos (only latest snapshot)
git clone --depth 1 git@github.com:shopease/shopease-web.git
```

---

### Staging & Committing

```bash
# ── Scenario: Priya has finished the cart redesign ───────

# See what's changed
git status
# On branch feature/cart-redesign
# Changes not staged for commit:
#   modified:   src/components/Cart.jsx
#   modified:   src/styles/cart.css
# Untracked files:
#   src/components/CartItem.jsx

# Stage only the component files (not the style yet)
git add src/components/Cart.jsx
git add src/components/CartItem.jsx

# Stage everything at once
git add .

# Double-check what's staged before committing
git status
# Changes to be committed:
#   modified:   src/components/Cart.jsx
#   new file:   src/components/CartItem.jsx
#   modified:   src/styles/cart.css

# Commit with a clear message
git commit -m "feat: redesign cart with quantity selector and item removal"
# [feature/cart-redesign 3f2a9c1] feat: redesign cart with quantity selector
#  3 files changed, 89 insertions(+), 14 deletions(-)

# Oops — Priya forgot to include a helper file
git add src/utils/cartHelpers.js
git commit --amend --no-edit
# Rewrites the last commit to include cartHelpers.js — no new commit created

# Stage parts of a file interactively (pick specific lines)
git add -p src/components/Cart.jsx
# Git shows each "hunk" and asks: Stage this hunk [y,n,q,a,d,s,?]?
```

> **Good commit messages matter:**
> ```
> ✅ feat: add quantity selector to cart items
> ✅ fix: prevent duplicate items in wishlist on refresh
> ✅ docs: update API integration guide for payment module
>
> ❌ fixed stuff
> ❌ WIP
> ❌ asdfgh
> ```
> Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

---

### Branching

```bash
# ── Scenario: ShopEase team manages several features in parallel ──

# See all branches (* = where you are now)
git branch
# * main
#   feature/cart-redesign
#   hotfix/payment-null-error

# See branches on GitHub too
git branch -a
# * main
#   feature/cart-redesign
#   remotes/origin/main
#   remotes/origin/feature/cart-redesign
#   remotes/origin/feature/add-wishlist

# Arjun starts working on the new search API
git switch -c feature/search-api
# Switched to a new branch 'feature/search-api'

# ... Arjun writes code, then an urgent bug is reported ...

# Switch to main to create a hotfix branch
git switch main
git switch -c hotfix/payment-null-error
# Switched to a new branch 'hotfix/payment-null-error'

# After fixing the bug, delete the hotfix branch
git switch main
git branch -d hotfix/payment-null-error
# Deleted branch hotfix/payment-null-error (was a8c2f1d)

# Delete a branch on GitHub too
git push origin --delete hotfix/payment-null-error

# Rename a branch (e.g., Kiran used the wrong naming convention)
git branch -m wishlist feature/add-wishlist
```

---

### Merging & Rebasing

```bash
# ── Scenario: Priya's cart feature is approved, time to merge ──

git switch main
git merge feature/cart-redesign
# Updating 8d3b1a2..3f2a9c1
# Fast-forward
#  src/components/Cart.jsx    | 67 ++++++++++++++++++---
#  src/components/CartItem.jsx| 45 +++++++++++++++++
#  src/styles/cart.css        | 28 +++++++++--
#  3 files changed, 127 insertions(+), 13 deletions(-)

# ── Merge with a commit (preserves branch history) ──────
git merge --no-ff feature/search-api -m "merge: integrate search API feature"

# ── Scenario: Arjun rebases his branch before opening a PR ──
# His branch is 3 commits behind main — he wants a clean history

git switch feature/search-api
git fetch origin
git rebase origin/main
# Successfully rebased and updated refs/heads/feature/search-api.

# ── Interactive rebase — squash 3 messy commits into 1 ──
git rebase -i HEAD~3
# Opens editor:
# pick a1b2c3 wip: rough draft of search endpoint
# pick d4e5f6 fix typo
# pick g7h8i9 fix another typo
#
# Change to:
# pick a1b2c3 feat: add product search API with filters
# fixup d4e5f6 fix typo
# fixup g7h8i9 fix another typo
# → Squashes all 3 into 1 clean commit

# ── Cherry-pick — apply ONE commit from another branch ──
# Arjun wrote a logging utility in feature/search-api
# Sara needs it in her devops branch NOW, without merging everything

git switch feature/devops-monitoring
git cherry-pick g7h8i9
# [feature/devops-monitoring 1c2d3e4] feat: add request logger utility
```

> **Merge vs Rebase — when to use which:**
> - Use **merge** for integrating completed features into `main` (preserves history)
> - Use **rebase** to update your feature branch with latest `main` before a PR (clean history)
> - ⚠️ Never rebase `main` or any branch other people are working on

---

### Remote Repositories

```bash
# ── See where your repo points ──────────────────────────
git remote -v
# origin  git@github.com:shopease/shopease-web.git (fetch)
# origin  git@github.com:shopease/shopease-web.git (push)

# ── Kiran adds a remote after initializing locally ──────
git remote add origin git@github.com:shopease/shopease-web.git

# ── Sara fetches to see what the team pushed overnight ──
git fetch origin
# remote: Enumerating objects: 12, done.
# From github.com:shopease/shopease-web
#    8d3b1a2..3f2a9c1  main                 -> origin/main
#  * [new branch]      feature/add-wishlist -> origin/feature/add-wishlist

# Fetch doesn't change your files — inspect first
git log origin/main --oneline
# 3f2a9c1 feat: redesign cart with quantity selector
# 8d3b1a2 feat: add product listing pagination
# ...

# Then merge when ready
git merge origin/main

# ── Pull = fetch + merge in one step ────────────────────
git pull origin main
# Already up to date.

# Pull using rebase (cleaner history)
git pull --rebase origin main

# ── Push Priya's feature branch ─────────────────────────
git push origin feature/cart-redesign
# Enumerating objects: 9, done.
# To github.com:shopease/shopease-web.git
#  * [new branch]  feature/cart-redesign -> feature/cart-redesign

# Set upstream so future pushes are just `git push`
git push -u origin feature/cart-redesign
# Branch 'feature/cart-redesign' set up to track 'origin/feature/cart-redesign'
# Now Priya can just type: git push

# ── Safe force push after a rebase ──────────────────────
# (Use --force-with-lease, not --force — it's safer)
git push --force-with-lease origin feature/search-api
```

---

### Undoing Changes

```bash
# ── Scenario: Kiran made several kinds of mistakes ──────

# MISTAKE 1: Edited the wrong file — discard changes immediately
git restore src/pages/HomePage.jsx
# File is back to the last committed state

# MISTAKE 2: Staged a file by accident — unstage it
git restore --staged src/config/database.js
# Still modified, but no longer staged (won't be in the next commit)

# MISTAKE 3: Committed but didn't push yet — undo the commit
git reset --soft HEAD~1
# Commit is gone, but all changes are still staged (safe)

# MISTAKE 4: Committed the wrong files — undo commit and unstage
git reset --mixed HEAD~1
# Commit is gone, changes back to working directory (need to re-add)

# MISTAKE 5: Accidentally committed 200 lines of debug code
git reset --hard HEAD~1
# ⚠️ DESTRUCTIVE — commit AND changes are gone forever

# MISTAKE 6: A bug was introduced 3 days ago in a shared branch
# Can't use reset (others have pulled it) — use revert instead
git log --oneline
# 9f3b2a1 feat: new checkout flow    ← latest
# 7c1d4e8 fix: update tax calculation ← this one introduced the bug
# 5a8b2c0 feat: add promo code support

git revert 7c1d4e8
# Creates a NEW commit: "Revert 'fix: update tax calculation'"
# Safe to push to shared branches

# MISTAKE 7: Deleted a file but didn't commit yet
git restore src/components/DeletedComponent.jsx
# File is recovered from the last commit
```

> ⚠️ **Safety Rule:** `git reset --hard` + `git push --force` on shared branches = your teammates lose their work. Always use `git revert` on shared branches like `main`.

---

### Stashing

Stash is your "save for later" drawer. Use it when you need to switch tasks mid-way.

```bash
# ── Scenario: Arjun is halfway through the search feature
# when an urgent bug report comes in for production ───────

git status
# modified: src/api/searchController.js  (half-done, not ready to commit)

# Stash the work-in-progress
git stash push -m "WIP: search filters — not done yet"
# Saved working directory and index state On feature/search-api: WIP: search filters

# Switch to main, fix the production bug
git switch main
git switch -c hotfix/null-cart-crash
# ... fix the bug, commit, push, PR merged ...

# Come back to the search feature
git switch feature/search-api
git stash list
# stash@{0}: On feature/search-api: WIP: search filters — not done yet

git stash pop
# Restored — searchController.js is back as Arjun left it

# ── Multiple stashes — Sara is juggling two tasks ────────
git stash push -m "WIP: docker config"
git stash push -m "WIP: nginx ssl setup"
git stash list
# stash@{0}: WIP: nginx ssl setup
# stash@{1}: WIP: docker config

# Apply a specific stash (without removing it from the list)
git stash apply stash@{1}

# Remove a stash after you're done with it
git stash drop stash@{1}
```

---

### Tagging

Tags mark release points — like bookmarks in your project history.

```bash
# ── ShopEase ships version 1.0.0 ────────────────────────

# List existing tags
git tag
# v0.8.0
# v0.9.0-beta

# Create an annotated release tag (recommended)
git tag -a v1.0.0 -m "Release v1.0.0 — official launch with cart, checkout, and payment"
# Includes tagger name, date, and message (unlike lightweight tags)

# Push the tag to GitHub (creates a GitHub Release)
git push origin v1.0.0
# To github.com:shopease/shopease-web.git
#  * [new tag]  v1.0.0 -> v1.0.0

# Push all tags at once
git push origin --tags

# ── Tagging a past commit ────────────────────────────────
# Oops — team forgot to tag last week's beta
git log --oneline
# 9f3b2a1 feat: new checkout flow
# 7c1d4e8 release: prep for v0.9.0-beta  ← this one
# 5a8b2c0 feat: add promo code support

git tag -a v0.9.0-beta 7c1d4e8 -m "Beta release for internal testing"

# View tag details
git show v1.0.0
# tag v1.0.0
# Tagger: Arjun Mehta <arjun@shopease.dev>
# Date: Mon Jun 10 18:30:00 2024
# Release v1.0.0 — official launch with cart, checkout, and payment
```

---

### Inspection & Logging

```bash
# ── See what's been happening in the repo ───────────────

git log --oneline --graph --all --decorate
# * 9f3b2a1 (HEAD -> main, origin/main) feat: new checkout flow
# * 3f2a9c1 feat: redesign cart with quantity selector
# | * d1c2b3a (feature/search-api) feat: add product search API
# |/
# * 8d3b1a2 feat: add product listing pagination

# Filter to see only Priya's commits
git log --author="Priya" --oneline
# 3f2a9c1 feat: redesign cart with quantity selector
# a2b1c0d style: update cart button colors
# f8e7d6c feat: add empty cart message

# See commits from the last week
git log --since="1 week ago" --oneline

# Search commit messages
git log --grep="payment" --oneline
# 9f3b2a1 feat: new checkout flow
# b4c3d2e fix: handle payment timeout error

# See what changed in a specific commit
git show 3f2a9c1
# commit 3f2a9c1...
# Author: Priya Sharma <priya@shopease.dev>
# Date:   Mon Jun 10
#
#     feat: redesign cart with quantity selector
#
# diff --git a/src/components/Cart.jsx ...

# ── Find who broke something (blame) ───────────────────
git blame src/api/paymentController.js
# a3b2c1d (Arjun Mehta  2024-06-08 14:22:11) const charge = await stripe.charge({
# f7e6d5c (Priya Sharma  2024-06-09 10:05:33)   amount: cart.total * 100,
# 9g8h7i6 (Kiran Rao    2024-06-10 16:40:02)   currency: 'inr',

# ── Find which commit introduced a bug (bisect) ─────────
# Arjun knows v0.9.0-beta worked, but HEAD is broken
git bisect start
git bisect bad                  # current HEAD is broken
git bisect good v0.9.0-beta     # this tag was fine

# Git checks out the midpoint commit — Arjun tests the app
# If it works:
git bisect good
# If broken:
git bisect bad
# Repeat until Git pins down the exact commit

git bisect reset                # Done — back to HEAD

# ── See what changed between branches ──────────────────
git diff main..feature/search-api
# Shows all differences between the two branches

git diff --stat main..feature/search-api
# src/api/searchController.js  | 87 ++++++++++++
# src/routes/api.js            |  6 ++-
# 2 files changed, 91 insertions(+), 2 deletions(-)
```

---

## GitHub Features

### Fork & Pull Request Workflow

This is how open-source contributions work — and also how external contractors contribute to ShopEase.

```
Real scenario: A contractor named Dev wants to add
an "Express Delivery" badge to ShopEase's product cards.
He doesn't have write access, so he forks the repo.
```

```bash
# Step 1: Dev forks shopease/shopease-web on GitHub
# → Creates dev-contractor/shopease-web (his own copy)

# Step 2: Clone his fork locally
git clone git@github.com:dev-contractor/shopease-web.git
cd shopease-web

# Step 3: Add the original repo as "upstream"
git remote add upstream git@github.com:shopease/shopease-web.git

git remote -v
# origin    git@github.com:dev-contractor/shopease-web.git (fetch)
# origin    git@github.com:dev-contractor/shopease-web.git (push)
# upstream  git@github.com:shopease/shopease-web.git (fetch)
# upstream  git@github.com:shopease/shopease-web.git (push)

# Step 4: Create a feature branch (never work on main directly)
git switch -c feature/express-delivery-badge

# Step 5: Make changes
# ... edits ProductCard.jsx and Badge.css ...
git add .
git commit -m "feat: add express delivery badge to product cards"

# Step 6: Sync with upstream before pushing (team may have merged changes)
git fetch upstream
git rebase upstream/main

# Step 7: Push to his fork
git push origin feature/express-delivery-badge

# Step 8: Open a Pull Request on GitHub:
# → Base: shopease/shopease-web main
# → Compare: dev-contractor/shopease-web feature/express-delivery-badge
# → Title: "feat: add express delivery badge to product cards"
# → Description: Links to issue #47, screenshots, test instructions

# Step 9: Priya reviews, requests changes
# Dev makes updates, pushes again — PR auto-updates

# Step 10: Arjun approves, merges the PR

# Step 11: Dev cleans up
git switch main
git pull upstream main          # sync his main with shopease main
git branch -d feature/express-delivery-badge
git push origin --delete feature/express-delivery-badge
```

---

### Issues

GitHub Issues are how ShopEase tracks bugs and features.

```bash
# In commit messages, reference issues to auto-link them
git commit -m "fix: prevent cart total going negative — closes #83"
# When this PR merges, GitHub automatically closes Issue #83

git commit -m "feat: add promo code support — resolves #61, refs #72"
# resolves → closes the issue
# refs     → just links it, doesn't close

# PR description example:
# ## What does this PR do?
# Adds promo code input to the checkout page.
#
# ## Related Issues
# Closes #61
# Related to #72
#
# ## Testing
# 1. Go to /checkout
# 2. Enter promo code SAVE10
# 3. Verify 10% discount is applied
```

**Issue template** — ShopEase uses `.github/ISSUE_TEMPLATE/bug_report.md`:
```markdown
## Bug Description
A clear description of what the bug is.

## Steps to Reproduce
1. Go to '/checkout'
2. Add item to cart
3. See error

## Expected Behavior
Cart total should show correctly.

## Screenshots
(paste here)

## Environment
- OS: Ubuntu 22.04
- Browser: Chrome 124
- App version: v1.2.3
```

---

### GitHub Actions (CI/CD)

ShopEase uses GitHub Actions to automatically test every PR and deploy to production.

```yaml
# .github/workflows/ci.yml
# Runs on every PR to main — no broken code ever gets merged

name: ShopEase CI Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    name: Run Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js 20
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run unit tests
        run: npm test -- --coverage

      - name: Upload coverage report
        uses: codecov/codecov-action@v4

  build:
    name: Build & Deploy
    needs: test                # Only runs if tests pass
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'   # Only on main branch

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'

      - run: npm ci
      - run: npm run build

      - name: Deploy to production server
        run: |
          echo "Deploying ShopEase v${{ github.sha }} to prod..."
          # rsync / docker push / kubectl apply etc.
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_SSH_KEY }}
```

> 💡 Every time Priya opens a PR, GitHub Actions automatically runs all tests. If they fail, the PR can't be merged until she fixes them — no broken code reaches `main`.

---

### GitHub Pages

ShopEase uses GitHub Pages to host their public component documentation.

```bash
# After setting up in GitHub → Settings → Pages:
# Source: Deploy from branch → main → /docs

# Build and copy docs
npm run build:docs              # generates HTML into /docs
git add docs/
git commit -m "docs: update component library docs"
git push origin main
# GitHub auto-publishes to: https://shopease.github.io/shopease-web/
```

---

## .gitignore

ShopEase's `.gitignore` — things that should NEVER be committed:

```gitignore
# ── Node.js ─────────────────────────────────────────────
node_modules/
npm-debug.log*
yarn-error.log

# ── Build output ────────────────────────────────────────
dist/
build/
.next/
.nuxt/

# ── Environment & Secrets ───────────────────────────────
# CRITICAL: Never commit these — they contain API keys!
.env
.env.local
.env.production
*.pem
secrets.json

# ── IDE files ───────────────────────────────────────────
.vscode/
.idea/
*.swp
.DS_Store
Thumbs.db

# ── Test coverage ───────────────────────────────────────
coverage/
.nyc_output/

# ── Logs ────────────────────────────────────────────────
logs/
*.log
```

```bash
# ── Real scenario: Kiran accidentally committed .env ────

# Step 1: Stop tracking it
git rm --cached .env
# rm '.env'

# Step 2: Add to .gitignore so it never happens again
echo ".env" >> .gitignore

# Step 3: Commit the fix
git add .gitignore
git commit -m "chore: untrack .env and add to gitignore"

# Step 4: Push — and immediately rotate any secrets that were exposed!
git push origin main

# Check why a file is being ignored
git check-ignore -v src/config/local.js
# .gitignore:12:*.local.js  src/config/local.js
```

> ⚠️ **If you push secrets to GitHub:** Even after removing them, assume they're compromised. Rotate all API keys and tokens immediately.

---

## Git Configuration

```bash
# ── ShopEase team's recommended config ──────────────────

# Identity (required)
git config --global user.name "Priya Sharma"
git config --global user.email "priya@shopease.dev"

# Pull strategy — rebase keeps history cleaner
git config --global pull.rebase true

# Auto-correct typos (e.g. "git psuh" → "git push" after 2 seconds)
git config --global help.autocorrect 20

# Better diffs
git config --global diff.algorithm histogram

# Colorize output
git config --global color.ui auto

# ── Aliases — type less, do more ────────────────────────
git config --global alias.st "status -sb"
git config --global alias.co checkout
git config --global alias.sw switch
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.unstage "restore --staged"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.aliases "config --get-regexp alias"

# Usage:
git st          # → git status -sb (compact status)
git lg          # → git log --oneline --graph --all --decorate
git undo        # → undo last commit, keep changes staged
git unstage .   # → unstage everything

# ── View your config ────────────────────────────────────
git config --list
# user.name=Priya Sharma
# user.email=priya@shopease.dev
# pull.rebase=true
# alias.lg=log --oneline --graph --all --decorate
# ...

# Edit config file directly
git config --global --edit
```

---

## Advanced Tips

### Interactive Rebase — Clean Up History Before a PR

```bash
# ── Kiran's commit history is messy ─────────────────────
git log --oneline
# a1b2c3d save
# d4e5f6g fix
# g7h8i9j another fix
# j1k2l3m wip
# m4n5o6p feat: add wishlist page  ← the actual feature start

# Before opening a PR, squash all 5 into 1 clean commit
git rebase -i HEAD~5

# Editor opens:
# pick m4n5o6p feat: add wishlist page
# pick j1k2l3m wip
# pick g7h8i9j another fix
# pick d4e5f6g fix
# pick a1b2c3d save

# Change to:
# pick m4n5o6p feat: add wishlist page
# fixup j1k2l3m wip
# fixup g7h8i9j another fix
# fixup d4e5f6g fix
# fixup a1b2c3d save

# Result: One clean commit — "feat: add wishlist page"
# Now open the PR with a professional history ✅
```

### Worktrees — Work on Two Branches Simultaneously

```bash
# ── Arjun needs to fix a production bug while also
# working on the search feature ──────────────────────────

# Instead of stashing and switching:
git worktree add ../shopease-hotfix hotfix/cart-null-crash
# Prepares a separate folder with the hotfix branch checked out

# Now Arjun has TWO working copies:
# ~/shopease-web/         → feature/search-api (in progress)
# ~/shopease-hotfix/      → hotfix/cart-null-crash (fix here)

# Work in each folder independently — no stashing needed!
cd ../shopease-hotfix
# ... fix the bug, commit, push ...

# Clean up when done
cd ../shopease-web
git worktree remove ../shopease-hotfix
```

### Submodules — Shared Code Libraries

```bash
# ── ShopEase extracts their UI component library into a
# separate repo that multiple projects use ───────────────

git submodule add git@github.com:shopease/ui-components.git libs/ui
# Adds the ui-components repo inside shopease-web at libs/ui/

# Clone a repo AND its submodules
git clone --recurse-submodules git@github.com:shopease/shopease-web.git

# Pull latest changes from the submodule repo
git submodule update --remote --merge

git add .gitmodules libs/ui
git commit -m "chore: update ui-components to latest"
```

### Useful One-Liners

```bash
# See what files changed in the last commit
git diff --name-only HEAD~1
# src/components/Cart.jsx
# src/styles/cart.css

# Count commits by each team member
git shortlog -sn
#    47  Arjun Mehta
#    38  Priya Sharma
#    21  Sara Khan
#     9  Kiran Rao

# Find all commits that touched a specific file
git log --follow --oneline src/api/paymentController.js
# 9f3b2a1 feat: new checkout flow
# b4c3d2e fix: handle payment timeout error

# See which branch a commit is on
git branch --contains 3f2a9c1
# * main
#   feature/cart-redesign

# Export the entire repo as a zip (for sharing without Git history)
git archive --format=zip HEAD > shopease-v1.0.0.zip

# Find the size of your repo's history
git count-objects -vH
# count: 1842
# size: 4.23 MiB
```

---

## Common Errors & Fixes

### `fatal: not a git repository`
```bash
# You're in the wrong folder, or forgot to init/clone
pwd
# /home/kiran/projects   ← one level too high!

cd shopease-web
git status
# On branch main — works now ✅
```

### `error: failed to push some refs`
```bash
# Arjun pushed while Priya was about to push — remote has new commits
git push origin main
# error: failed to push some refs to 'github.com:shopease/shopease-web.git'
# hint: Updates were rejected because the remote contains work that you do
# not have locally.

# Fix: pull first (with rebase to keep history clean)
git pull --rebase origin main
git push origin main
# ✅ Everything up to date
```

### Merge Conflict
```bash
# Both Priya and Arjun edited src/api/index.js on different branches
git merge feature/search-api
# CONFLICT (content): Merge conflict in src/api/index.js
# Automatic merge failed; fix conflicts and then commit the result.

# Open src/api/index.js — you'll see:
# <<<<<<< HEAD (Priya's version)
# import cartRouter from './cartRoutes';
# =======
# import searchRouter from './searchRoutes';
# >>>>>>> feature/search-api (Arjun's version)

# Fix: keep BOTH (that's what they actually want)
# import cartRouter from './cartRoutes';
# import searchRouter from './searchRoutes';

git add src/api/index.js
git commit -m "merge: resolve conflict in api/index.js — keep both routers"
# [main 4a5b6c7] merge: resolve conflict in api/index.js
```

### `detached HEAD state`
```bash
# Kiran ran `git checkout 3f2a9c1` to inspect an old commit
# Now Git says:
# HEAD is now at 3f2a9c1 feat: redesign cart
# You are in 'detached HEAD' state...

# If he makes commits here, they'll be LOST when switching branches!
# Fix option 1 — create a branch to save the work:
git switch -c temp/explore-old-cart

# Fix option 2 — just go back to main without keeping changes:
git switch main
```

### `git pull` refused — unrelated histories
```bash
# Sara created a repo on GitHub with a README, then also
# ran git init locally — now they have different roots

git pull origin main
# fatal: refusing to merge unrelated histories

git pull origin main --allow-unrelated-histories
# Merge made by the 'ort' strategy. ✅
```

### Accidentally committed to `main`
```bash
# Kiran forgot to create a branch and committed directly to main
git log --oneline
# a1b2c3d feat: add wishlist page  ← Kiran's commit, should be on a branch
# 9f3b2a1 merge: cart redesign

# Fix: create the branch, then remove from main
git branch feature/add-wishlist         # creates branch pointing at a1b2c3d
git reset --hard HEAD~1                 # removes commit from main
git push origin main --force-with-lease # update remote main

# Kiran's work is safe on feature/add-wishlist ✅
git switch feature/add-wishlist
git push origin feature/add-wishlist
```

---

## Cheat Sheet

```
SETUP
  git config --global user.name "Priya Sharma"
  git config --global user.email "priya@shopease.dev"

START
  git init                             Start tracking a project
  git clone git@github.com:org/repo    Get a copy from GitHub

STAGE & COMMIT
  git status                           See what's changed
  git add src/Cart.jsx                 Stage a file
  git add .                            Stage everything
  git commit -m "feat: add cart"       Save a snapshot
  git commit --amend --no-edit         Add to last commit

BRANCH
  git branch                           List branches
  git switch -c feature/search-api     Create + switch
  git switch main                      Switch to main
  git branch -d feature/search-api     Delete branch

MERGE & REBASE
  git merge feature/cart-redesign      Merge into current
  git rebase origin/main               Rebase onto latest main
  git cherry-pick a1b2c3d              Apply one commit

REMOTE
  git remote -v                        Show remotes
  git fetch origin                     Download (don't merge)
  git pull --rebase origin main        Sync + rebase
  git push origin feature/my-branch    Upload branch
  git push -u origin feature/my-branch Upload + set tracking

UNDO
  git restore src/Cart.jsx             Discard file changes
  git restore --staged src/Cart.jsx    Unstage a file
  git reset --soft HEAD~1              Undo commit (keep staged)
  git reset --hard HEAD~1              Undo commit (discard all)
  git revert 9f3b2a1                   Safe undo on shared branch

STASH
  git stash push -m "WIP: search"     Save mid-work
  git stash pop                        Restore latest stash
  git stash list                       Show all stashes

LOG
  git log --oneline --graph --all      Visual history
  git blame src/payment.js             Who wrote each line
  git diff main..feature/search-api    See branch differences
```

---

## 📚 Resources

| Resource | Link |
|---|---|
| Official Git Docs | https://git-scm.com/doc |
| Pro Git Book (free) | https://git-scm.com/book |
| GitHub Docs | https://docs.github.com |
| GitHub Skills (interactive) | https://skills.github.com |
| .gitignore templates | https://gitignore.io |
| Conventional Commits | https://conventionalcommits.org |
| Oh My Git! (visual game) | https://ohmygit.org |
| Visualize Git (interactive) | https://git-school.github.io/visualizing-git |

---

<div align="center">

**Happy Coding! 🎉**

*If this helped you, consider starring ⭐ the repo!*

</div>