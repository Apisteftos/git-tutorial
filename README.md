## Git tutorial

> Git is a distributed version control system that tracks changes in source code
> during software development. It allows multiple developers to work together,
> maintain a complete history of changes, and revert to previous versions when needed.
> This tutorial covers the most common Git commands and workflows for daily development.

## Git Commands

### Initial Setup
```bash
git config --global user.email "kosagglis@gmail.com"    # Connect with Git email
git config --global user.name "Apisteftos"              # Connect with Git username
```

### Initialize Repository
```bash
git init                                                 # Initialize in root directory
git add file/dir                                         # Add file or directory
git commit -m "first commit"                            # Commit changes
git branch -M master                                     # Name the branch (master/main)
git remote add origin https://github.com/Apisteftos/Dijkstra.git
# Or use SSH:
git remote add origin git@github.com:Apisteftos/Dijkstra.git
git push -u origin master                                # Push to remote (-u sets upstream)
```

### If HTTPS Doesn't Work
```bash
git remote -v                                            # Check current remote
git remote set-url origin git@github.com:Apisteftos/Dijkstra.git  # Change to SSH
git remote -v                                            # Verify change
git push -u origin master
```

### Adding Files

#### git add -A vs git add .

| Command | What it does |
|---------|-------------|
| `git add -A` | Stages ALL changes — new, modified, deleted — anywhere in the repo |
| `git add .` | Stages ALL changes in the **current directory** and subdirectories only |
| `git add file.cpp` | Stages only one specific file |

**The difference:**
```bash
# if you're in a subdirectory like /examples/
git add .      # stages only changes in /examples/
git add -A     # stages changes in the ENTIRE repo including /include/, /tests/ etc.
```

**Always use `git add -A`** when working from the repo root — it stages everything.

```bash
git add include/dijkstra.cpp                             # Add specific files/directories
git add .                                                # Add all in current directory
git add -A                                               # Add ALL changes in entire repo
git commit                                               # Opens editor for commit message
# In nano: Ctrl+O to save, Enter, Ctrl+X to exit

# Or commit directly:
git commit -m "adding the dijkstra.cpp file"
git commit -a                                            # Commit all changes
git push origin master
git status                                               # See uncommitted or changed files
git push --force origin master                           # Force push if conflicts occur
```

### Review Before Committing

**Always review before you commit:**

```bash
# see WHICH files changed and how many lines
git diff --stat

# see WHAT changed in detail (unstaged changes)
git diff

# see what's STAGED — what will go into the commit
git diff --staged
```

**The correct order:**
```bash
git add -A           # 1. stage everything
git diff --staged    # 2. review BEFORE committing ← do this here
git commit -m "..."  # 3. commit only after reviewing
```

`git diff --staged` shows:
```
+ added lines (green)
- removed lines (red)
  unchanged lines
```

Press `q` to exit the diff viewer.

### Download Changes
```bash
git pull origin master                                   # Pull before pushing
```

### Branch Management

#### Verify and Switch Branches
```bash
git branch                                               # List local branches
git branch -a                                            # List all branches (local + remote)
git checkout main                                        # Switch to main branch
git checkout branch_name                                 # Switch to another branch
git checkout -b new_branch                               # Create and switch to new branch
```

#### Delete Branches
```bash
git branch -d branch_name                                # Delete branch
git branch -D branch_name                                # Force delete branch
git push origin --delete branch_name                     # Delete remote branch
```

### Git History and Visualization

#### Install gitk
```bash
sudo apt install gitk
gitk                                                     # Open gitk viewer
```

#### View History
```bash
git log                                                  # View commit log (B to scroll, Shift+Q to exit)
git log --oneline                                        # One line per commit
git log --oneline --graph                                # View updates in one line with graph
git log --oneline -5                                     # Show last 5 commits only
git diff                                                 # See changes after "git add"
git show a1b2c3                                          # Show what a specific commit changed
```

### Tags — Versioning Releases

Tags mark specific commits as version releases.

#### Create a tag
```bash
git tag v0.10.0                                          # lightweight tag
git tag -a v0.10.0 -m "Release v0.10.0"                 # annotated tag (recommended)
```

#### Push a tag
```bash
git push origin v0.10.0                                  # push specific tag
git push origin --tags                                   # push ALL local tags at once
git push gitea v0.10.0                                   # push to Gitea as well
```

#### List tags
```bash
git tag                                                  # list all tags
git tag -l "v*"                                          # list only version tags
```

#### Delete a tag (if you made a mistake)
```bash
git tag -d v0.11.0                                       # delete local tag (-d or --delete)
git push origin --delete v0.11.0                         # delete remote tag on GitHub
git push gitea --delete v0.11.0                          # delete remote tag on Gitea
```

**Full workflow to fix a wrong tag:**
```bash
# 1. delete local tag
git tag -d v0.11.0

# 2. delete from remotes
git push origin --delete v0.11.0
git push gitea --delete v0.11.0

# 3. recreate with correct message
git tag -a v0.11.0 -m "v0.11.0 — correct message here"

# 4. push again
git push origin v0.11.0
git push gitea v0.11.0
```

#### Editor for tag messages — VSCode vs vim

When you run `git tag -a v0.11.0` **without** `-m`, Git opens your default editor:

**If VSCode opens (TAG_EDITMSG tab):**
```
1. Type your message in the editor
2. Ctrl+S    ← save the file
3. Ctrl+W    ← close the tab
# Git creates the tag automatically after the tab is closed
```

**If vim opens:**
```
i            ← enter insert mode
# type your message
Esc          ← exit insert mode
:wq          ← save and quit
Enter        ← confirm
```

**To abort without creating the tag:**
```
# In VSCode — delete all content then Ctrl+S + Ctrl+W
# In vim
Esc
:q!          ← quit without saving → tag is NOT created
Enter
```

**Best practice — always use `-m` to avoid the editor:**
```bash
git tag -a v0.11.0 -m "v0.11.0 — bar, barh, set_xticks, Text kwargs"
# ← no editor opens, tag created immediately
```

#### What happens with multiple tags
```
v0.1.0  → first working version
v0.6.0  → detail.h, AxesGrid, step
v0.7.0  → fill, fill_between, Vec operators
v0.8.0  → errorbar
v0.9.0  → scatter
v0.10.0 → fill_between full kwargs, unit tests, CI ← latest
```

Each tag points to a specific commit. The dynamic README badge:
```markdown
![Version](https://img.shields.io/github/v/tag/Apisteftos/matplotlib-modern-cpp?label=version&color=blue)
```
automatically shows the **latest tag** — no manual README update needed.

### Deleting Files

```bash
git rm file/dir
git commit -m "message"
git push
```

### Remove Files from Git Tracking

1. Ensure file/directory is in `.gitignore`:
```bash
# Create .gitignore
# Add: dir_name/ or *.file
```

2. Remove from tracking:
```bash
git rm --cached "path/to/file"
git add .                                                # Add all files
git commit -m "message"                                  # Or: git commit -a (Ctrl+W to close VSCode)
git push
```

3. Verify file is removed from GitHub

### Unstage Files

```bash
git reset HEAD <file_path>                               # Unstage one file
git reset HEAD .                                         # Unstage all added files
git restore --staged file.cpp                            # Modern way to unstage
git status
```

### Undo Changes

```bash
git reset --soft HEAD~1                                  # Undo last commit, keep changes staged
git reset --mixed HEAD~1                                 # Undo last commit, keep changes unstaged
git reset --hard HEAD~1                                  # Undo last commit, discard changes (dangerous)
git restore file.cpp                                     # Discard changes in a file (cannot undo!)
```

### Multiple Remotes

```bash
git remote -v                                            # See all remotes
git remote add gitea http://your-nas/repo.git            # Add second remote
git push origin main                                     # Push to GitHub
git push gitea main                                      # Push to Gitea
git push origin main && git push gitea main              # Push to both at once
```

#### Remove a Remote

```bash
git remote -v                                            # 1. list remotes, find the name to remove
git remote remove gitea                                   # 2. remove it (or: git remote rm gitea)
git remote -v                                            # 3. verify it's gone
```

`gitea` here is just the remote's **name** — not a fixed keyword — so use whatever you actually called it when you ran `git remote add <name> <url>` (check with `git remote -v` first if you're not sure).

This only removes the local reference to that remote — it does **not** delete anything from GitHub/GitLab/Gitea itself, and any other remotes (e.g. `origin`) are untouched.

### Remove Sensitive Information from Commit History

```bash
# Optional: Backup repository
git clone --mirror <repository_url> backup-repo.git

# Remove sensitive file
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch sensitive-data.txt' \
  --prune-empty --tag-name-filter cat -- --all

# Clean up repository
git reflog expire --expire=now --all && git gc --prune=now --aggressive

# Remove original references
rm -rf .git/refs/original/

# Clean up unnecessary files
git reflog expire --expire=now --all && git gc --prune=now --aggressive

# Force-push rewritten history
git push origin --force --all
git push origin --force --tags

# Clone fresh copy
cd ..
git clone <repository_url>
cd <repository_name>
```

**Note:** All collaborators must re-clone the repository after this operation.

### Full Workflow Example

```bash
# 1. make your changes in VSCode

# 2. check what changed
git status

# 3. stage everything
git add -A

# 4. review what will be committed
git diff --staged
# press q to exit

# 5. commit
git commit -m "feat: add histogram and legend support

- Add HistConfig struct
- Add LegendConfig struct
- Add histImpl(), legendImpl() in Detail.h"

# 6. push to remotes
git push origin main
git push gitea main

# 7. if this is a version release — tag it
git tag -a v0.11.0 -m "Release v0.11.0"
git push origin v0.11.0
git push gitea v0.11.0
```

### .gitignore — Ignoring Files

`.gitignore` tells Git which files to never track or commit.

```bash
# create .gitignore in repo root
touch .gitignore
```

**Common entries for C++ projects:**
```gitignore
build/              # CMake build directory
*.o                 # object files
*.a                 # static libraries
*.so                # shared libraries
__pycache__/        # Python cache
*.pyc               # Python compiled files
tempCodeRunnerFile.py
.vscode/            # VSCode settings (optional — omit if you want to track)
*.png               # generated plot images (optional)
```

```bash
git status                    # ignored files won't appear here
git check-ignore -v file.cpp  # check WHY a file is ignored
```

---

### git stash — Save Work Temporarily

`git stash` saves your uncommitted changes temporarily so you can switch context without committing unfinished work.

```bash
git stash                     # save all uncommitted changes
git stash save "work in progress on scatter"  # save with a description
git stash list                # see all stashes
git stash pop                 # restore most recent stash and delete it
git stash apply               # restore most recent stash but keep it in list
git stash apply stash@{2}     # restore a specific stash
git stash drop stash@{0}      # delete a specific stash
git stash clear               # delete ALL stashes
```

**When to use:**
```bash
# you're working on fill_between but need to quickly fix a bug in plot
git stash                     # save fill_between work
# fix the plot bug
git add -A
git commit -m "fix: plot bug"
git stash pop                 # restore fill_between work
```

---

### git blame — See Who Changed What

`git blame` shows who last modified each line of a file and when.

```bash
git blame file.cpp            # show all lines with author and date
git blame -L 10,20 file.cpp   # show only lines 10-20
git blame --since=2.weeks file.cpp  # show changes in last 2 weeks
```

Output looks like:
```
a1b2c3 (Apisteftos 2026-07-05 14:23:11) void plotImpl(...) {
d4e5f6 (Apisteftos 2026-07-06 09:15:42)     PyPtr args(PyTuple_New(2));
```

Useful for debugging — find when a bug was introduced and who introduced it.

---

### git rebase — Rewrite History

`git rebase` moves your commits on top of another branch, creating a **linear history**.

**Visual explanation:**
```
BEFORE rebase:

main:     A → B → C
                    \
feature:             D → E → F

AFTER git rebase main (on feature branch):

main:     A → B → C
                    \
feature:             D' → E' → F'  (replayed on top of C)
```

**Compare with merge:**
```
MERGE — creates a merge commit (M):
main:     A → B → C --------→ M
                    \         /
feature:             D → E → F

REBASE — linear, clean history:
main:     A → B → C → D' → E' → F'
```

**Basic usage:**
```bash
git checkout feature
git rebase main               # rebase feature onto main
```

**Interactive rebase — squash messy commits:**
```bash
git rebase -i HEAD~3          # rewrite last 3 commits
```

Opens editor:
```
pick a1b2c3 feat: add scatter
pick d4e5f6 fix: typo
pick g7h8i9 fix: another typo

# Commands:
# pick   = keep commit as is
# squash = merge into previous commit
# reword = change commit message
# drop   = delete commit entirely
```

Change to:
```
pick a1b2c3 feat: add scatter
squash d4e5f6 fix: typo
squash g7h8i9 fix: another typo
```

Result — 3 messy commits become 1 clean commit.

**When to use rebase vs merge:**

| Situation | Use |
|---|---|
| Keeping history clean and linear | `rebase` |
| Working on a shared branch | `merge` |
| Before opening a Pull Request | `rebase` |
| After pushing to remote | **never rebase** |

**Golden rule:**
```
NEVER rebase commits already pushed to a shared remote.
Rebase rewrites history — others' history diverges → chaos.

Safe:   rebase local commits not yet pushed
Unsafe: rebase commits already on origin/main
```

---

### Merge Conflicts — How to Resolve

Merge conflicts happen when two branches change the same line differently.

```bash
git merge feature             # merge feature into current branch
# CONFLICT: Merge conflict in include/Axes.h
```

**Git marks the conflict in the file:**
```cpp
<<<<<<< HEAD
void plot(const PlotConfig& config) {   // your version
=======
void plot(PlotConfig config) {          // incoming version
>>>>>>> feature
```

**Resolve:**
1. Open the file
2. Choose which version to keep (or combine both)
3. Remove the conflict markers `<<<<<<<`, `=======`, `>>>>>>>`
4. Save the file

```bash
git add include/Axes.h        # mark as resolved
git commit -m "merge: resolve conflict in Axes.h"
```

**Useful tools:**
```bash
git diff                      # see all conflicts
git mergetool                 # open visual merge tool
git merge --abort             # abort merge and go back to before
git status                    # see which files have conflicts
```

---

### SSH Key Setup — Connect Without Password

SSH keys let you push/pull without typing your password every time.

**Generate SSH key:**
```bash
ssh-keygen -t ed25519 -C "kosagglis@gmail.com"
# press Enter for default location (~/.ssh/id_ed25519)
# optionally set a passphrase
```

**Copy public key:**
```bash
cat ~/.ssh/id_ed25519.pub
# copy the entire output
```

**Add to GitHub:**
```
GitHub → Settings → SSH and GPG keys → New SSH key → paste key → Save
```

**Test connection:**
```bash
ssh -T git@github.com
# Hi Apisteftos! You've successfully authenticated.
```

**Add to Gitea (your NAS):**
```
Gitea → Settings → SSH Keys → Add Key → paste key → Save
```

**Switch existing repo from HTTPS to SSH:**
```bash
git remote set-url origin git@github.com:Apisteftos/matplotlib-modern-cpp.git
git remote set-url gitea git@your-nas:Apisteftos/matplotlib-modern-cpp.git
```

---

### git cherry-pick — Apply a Specific Commit

`git cherry-pick` applies a specific commit from another branch onto your current branch.

```bash
git log --oneline feature     # find the commit hash you want
git cherry-pick a1b2c3        # apply that commit to current branch
git cherry-pick a1b2c3 d4e5f6 # apply multiple commits
git cherry-pick a1b2c3..g7h8i9 # apply a range of commits
git cherry-pick --abort       # abort if conflicts
```

**When to use:**
```bash
# you fixed a bug on feature branch
# you want that fix on main too without merging everything
git checkout main
git cherry-pick a1b2c3        # apply only the bug fix commit
```

---

### Commit Message Conventions

| Prefix | When to use |
|--------|-------------|
| `feat:` | new feature added |
| `fix:` | bug fixed |
| `test:` | tests added/changed |
| `ci:` | CI/CD changes |
| `docs:` | documentation only |
| `refactor:` | code restructured, no feature/fix |
| `chore:` | maintenance, cleanup |
| `v0.10.0:` | version bump commit |

---
