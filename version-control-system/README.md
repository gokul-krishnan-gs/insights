# Git Documentation Guide

## What is Git?

Git is a version control system used to track code changes.

## Command Syntax

```
command <required> [optional]
```

Example: `mkdir dirname`

---

## Getting Help

```bash
git help
git help [term]
```

**Fun Fact:** Linus Torvalds created Git.

---

## Porcelain vs Plumbing Commands

- **Porcelain:** High-level commands (everyday use)
- **Plumbing:** Low-level commands (internal operations)

---

## Configuration

### Setting Up User Information

```bash
git config --global user.name "Gokul Krishnan"
git config --global user.email "gokulkrishnangs02@gmail.com"
```

### Getting Configuration Information

```bash
git config get user.name
git config get user.email
```

### Verify Configuration Details

```bash
git config --global --list
```

### Setting Default Branch Name

```bash
git config --global init.defaultBranch main
```

### View Global Configuration File

```bash
cat ~/.gitconfig
```

### Working with Local Configuration

```bash
# View local config
git config list --local
cat .git/config

# Get a single value
git config get <key>

# Remove a configuration value
git config unset <key>
```

### Configuration Levels (General to Specific)

1. **system:** `/etc/gitconfig` - Configures Git for all users
2. **global:** `~/.gitconfig` - Configures Git for all projects of a user
3. **local:** `.git/config` - Configures Git for a specific project
4. **worktree:** `.git/config.worktree` - Configures Git for part of a project

**Note:** More specific configurations override more general ones.

---

## Git Repository Basics

A Git repository (repo) represents a single project. It contains a hidden `.git` directory where Git stores all tracking and versioning information.

### File States in Git

- **untracked:** Not being tracked by Git
- **staged:** Marked for inclusion in the next commit
- **committed:** Saved to the repository's history

### Initialize a Repository

```bash
git init
```

This creates a hidden `.git` folder.

---

## Essential Workflow Commands

### Check Repository Status

```bash
git status
```

Shows which files are untracked, staged, and committed.

### Stage Files

```bash
git add <path-to-file>

# Stage all changes at once
git add .
```

Staging prepares files for the next commit. Only staged files are included in commits.

### Commit Changes

```bash
git commit -m "message write here"
```

A commit is a snapshot of the repository at a given point in time.

### Amend Commit Message

```bash
git commit --amend -m "A: add contents.md"
```

### Core Developer Workflow

Half of your workflow as a developer uses just 3 commands:

1. `git status`
2. `git add`
3. `git commit`

---

## Viewing History

### View Commit History

```bash
git log
```

Shows:
- Who made a commit
- When the commit was made
- What was changed

### Useful Log Flags

```bash
# Compact view
git log --oneline

# Show full ref names
git log --decorator=full

# Hide decorations
git log --decorate=no

# Visual graph of history
git log --oneline --graph --all

# Best visualization for branching
git log --oneline --graph --decorate --all

# View commits from a specific branch
git log branchname
```

---

## Understanding Git Objects

### Commit Hashes

Each commit has a unique identifier called a "commit hash" (SHA-1):

```
Example: 5ba786fcc93e8092831c01e71444b9baa2228a4f
```

### Inspecting Objects

```bash
# View contents of a commit
git cat-file -p <hash>

# View type of object
git cat-file -t <hash>
```

**Important:** Git stores an entire snapshot of files on a per-commit level, not just the changes.

### What is a Tree?

A tree represents a directory (folder) in Git. It contains:
- File names
- Permissions
- References to blob objects (files)
- References to other trees (subfolders)

```
tree (root)
 ├── blob a1b2c3  index.html
 ├── blob d4e5f6  style.css
 └── tree g7h8i9  src/
       ├── blob j1k2l3  app.js
       └── blob m4n5o6  utils.js
```

View a tree:
```bash
git cat-file -p <tree-hash>
```

All data is stored in the `.git/objects` directory.

---

## Branching

A Git branch is a pointer to a commit. It allows you to keep track of different changes separately.

**Think of it like:**
- **main branch:** Stable, production-ready code
- **other branches:** Playgrounds for features, bug fixes, or experiments

### Branch Commands

```bash
# View current branch
git branch

# Create a new branch
git branch nameofbranch

# Switch to a branch
git switch nameofbranch

# Create and switch to a new branch
git switch -c newbranch

# Rename a branch
git branch -m oldname newname

# Delete a branch
git branch -d branch-name
```

---

## Merging

Merging combines work from one branch into another.

### How Merging Works

When you have:
- **main** with commits A, B, C
- **feature** with commits D, E (created after C)

Merging creates a **merge commit (M)** that combines both branches:

```
main: A → B → C → M (includes changes from D and E)
```

### Merge Commands

```bash
git switch main
git merge new-feature-branch

# Amend merge commit message if needed
git commit --amend -m "F: Merge branch 'add_classics'"
```

### Fast-Forward Merge

The simplest type of merge. Happens when the target branch hasn't moved since you created your feature branch.

Git simply moves the branch pointer forward without creating a merge commit.

---

## Rebasing

Rebase moves your commits onto the latest commit of another branch, creating a clean linear history.

**Rebase = Take your commits → Remove them → Replay them on a new position**

### Rebase Workflow

```bash
git switch featurebranch
# Work on feature
git switch main
git rebase featurebranch
```

Now the feature commits are added to the end of the main branch.

**Key Difference:**
- **Merging:** Creates a merge commit
- **Rebasing:** Creates a linear history by replaying commits

---

## Undoing Changes

One of the major benefits of Git is the ability to undo changes.

### Soft Reset

```bash
git reset --soft COMMITHASH
```

Go back to the commit, but **keep the changes** (staged).

### Hard Reset

```bash
git reset --hard COMMITHASH
```

Go back to the commit and **discard all changes**.

⚠️ **Warning:** Always be careful when using `git reset --hard`. It's powerful but dangerous!

---

## Quick Reference

### Most Used Commands

```bash
git status              # Check repo state
git add .               # Stage all changes
git commit -m "msg"     # Commit with message
git log --oneline       # View history
git branch              # List branches
git switch <branch>     # Switch branches
git merge <branch>      # Merge branch
```

### Remember

- A branch is just a pointer to a commit
- Git stores full snapshots, not just diffs
- The `.git` directory contains everything
- More specific configs override general ones
- 

---

## Remote Repositories

In git, another repo is called a **"remote"**.

### Adding a Remote

```bash
git remote add <name> <uri>
```

**Example:**
```bash
git remote add origin https://github.com/gokul-krishnan-gs/git-learning.git
```

`git remote` is the command used to manage URLs of remote repositories (like GitHub, GitLab, Bitbucket).

**Important:** Your local Git repo doesn't know where to push or pull until you add a remote.

### Show Remote Details

```bash
git remote show remotename
```

Shows the details of the remote info.

**Tip:** `origin` → your main GitHub repo

---

## Fetch

```bash
git fetch
```

Downloads the latest commits, branches, and metadata from a remote repository **without merging anything** into your local branch.

### Why developers use git fetch?

To see what changed before merging.

### Log Remote

```bash
git log [<remote>/<branch>]
```

### List Remote URLs

```bash
git ls-remote
```

Gives the URL where the remote was added.

**Note:** Git ≠ GitHub

---

## Push

The `git push` command pushes (sends) local changes to any "remote".

For example, to push our local main branch's commits to the remote origin's main branch:

```bash
git push origin main
```

### First Time Pushing

```bash
git push -u origin main
```

Then subsequently:

```bash
git push
```

---

## Pull

```bash
git pull [<remote>/<branch>]
```

### 🚀 What is git pull?

**git pull = git fetch + git merge**

It does TWO things automatically:

1. Downloads the latest changes from the remote branch
2. Merges those changes into your current branch

So your working directory changes immediately.

Developers merge branches on GitHub through pull requests.

`git pull` helps you get the latest code from the remote repository into your local machine.

### 🔥 Example

You are on main and want the latest code:

```bash
git pull origin main
```

Now your local main is updated with the remote main.

---

## Git Ignore

`.gitignore` is a file. Inside, if we give the name of files and folders, it doesn't track the changes.
