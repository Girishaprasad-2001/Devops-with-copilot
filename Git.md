# Git A to Z Commands Cheat Sheet

A complete Git command reference covering beginner, intermediate, and advanced commands used by Developers, DevOps Engineers, SREs, and CI/CD professionals.

---

# A. Git Installation & Configuration

## Check Git Version

```bash
git --version
```

## Configure Username

```bash
git config --global user.name "John Doe"
```

## Configure Email

```bash
git config --global user.email "john@example.com"
```

## View Configuration

```bash
git config --list
```

## Edit Configuration

```bash
git config --global --edit
```

---

# B. Initialize Repository

## Create New Repository

```bash
git init
```

## Clone Existing Repository

```bash
git clone https://github.com/user/repo.git
```

## Clone Specific Branch

```bash
git clone -b dev https://github.com/user/repo.git
```

---

# C. Status & Information

## Check Repository Status

```bash
git status
```

## Short Status

```bash
git status -s
```

## Repository Information

```bash
git remote -v
```

---

# D. Add Files

## Add Single File

```bash
git add file.txt
```

## Add Multiple Files

```bash
git add file1.txt file2.txt
```

## Add All Files

```bash
git add .
```

## Interactive Add

```bash
git add -i
```

---

# E. Commit Changes

## Commit Changes

```bash
git commit -m "Initial commit"
```

## Add and Commit

```bash
git commit -am "Updated files"
```

## Amend Previous Commit

```bash
git commit --amend
```

---

# F. Log Commands

## View Commit History

```bash
git log
```

## One Line History

```bash
git log --oneline
```

## Graph View

```bash
git log --oneline --graph --all
```

## Last 5 Commits

```bash
git log -5
```

---

# G. Branch Commands

## List Branches

```bash
git branch
```

## List Remote Branches

```bash
git branch -r
```

## List All Branches

```bash
git branch -a
```

## Create Branch

```bash
git branch feature
```

## Delete Branch

```bash
git branch -d feature
```

## Force Delete Branch

```bash
git branch -D feature
```

---

# H. Switch & Checkout

## Switch Branch

```bash
git checkout main
```

## New Branch and Switch

```bash
git checkout -b feature
```

## Using Switch Command

```bash
git switch feature
```

```bash
git switch -c feature
```

---

# I. Merge Branches

## Merge Branch

```bash
git merge feature
```

## No Fast Forward Merge

```bash
git merge --no-ff feature
```

## Abort Merge

```bash
git merge --abort
```

---

# J. Rebase Commands

## Rebase Branch

```bash
git rebase main
```

## Interactive Rebase

```bash
git rebase -i HEAD~5
```

## Abort Rebase

```bash
git rebase --abort
```

## Continue Rebase

```bash
git rebase --continue
```

---

# K. Stash Commands

## Save Changes

```bash
git stash
```

## Save With Message

```bash
git stash push -m "Work in progress"
```

## View Stashes

```bash
git stash list
```

## Apply Stash

```bash
git stash apply
```

## Remove Stash

```bash
git stash drop
```

## Remove All Stashes

```bash
git stash clear
```

---

# L. Remote Repository Commands

## Add Remote

```bash
git remote add origin https://github.com/user/repo.git
```

## Remove Remote

```bash
git remote remove origin
```

## Show Remote Details

```bash
git remote show origin
```

---

# M. Push Commands

## Push Changes

```bash
git push origin main
```

## Push New Branch

```bash
git push -u origin feature
```

## Force Push

```bash
git push --force
```

## Safer Force Push

```bash
git push --force-with-lease
```

---

# N. Pull Commands

## Pull Latest Changes

```bash
git pull
```

## Pull Specific Branch

```bash
git pull origin main
```

## Pull with Rebase

```bash
git pull --rebase
```

---

# O. Fetch Commands

## Fetch Changes

```bash
git fetch
```

## Fetch All

```bash
git fetch --all
```

## Prune Deleted Branches

```bash
git fetch --prune
```

---

# P. Diff Commands

## Compare Changes

```bash
git diff
```

## Staged Changes

```bash
git diff --staged
```

## Compare Branches

```bash
git diff main feature
```

## Compare Commits

```bash
git diff commit1 commit2
```

---

# Q. Tag Commands

## Create Tag

```bash
git tag v1.0
```

## Annotated Tag

```bash
git tag -a v1.0 -m "Release v1.0"
```

## List Tags

```bash
git tag
```

## Push Tags

```bash
git push origin --tags
```

---

# R. Reset Commands

## Soft Reset

```bash
git reset --soft HEAD~1
```

## Mixed Reset

```bash
git reset HEAD~1
```

## Hard Reset

```bash
git reset --hard HEAD~1
```

## Unstage File

```bash
git reset file.txt
```

---

# S. Restore Commands

## Restore File

```bash
git restore file.txt
```

## Restore Staged File

```bash
git restore --staged file.txt
```

## Restore Entire Working Tree

```bash
git restore .
```

---

# T. Cherry Pick

## Apply Single Commit

```bash
git cherry-pick commit_id
```

## Apply Multiple Commits

```bash
git cherry-pick commit1 commit2
```

---

# U. Undo Changes

## Undo Last Commit (Keep Changes)

```bash
git reset --soft HEAD~1
```

## Revert Commit

```bash
git revert commit_id
```

## Revert Latest Commit

```bash
git revert HEAD
```

---

# V. View File History

## File Commit History

```bash
git log file.txt
```

## Show Line History

```bash
git blame file.txt
```

## Show Commit Details

```bash
git show commit_id
```

---

# W. Working Tree Commands

## Clean Untracked Files

```bash
git clean -f
```

## Clean Directories

```bash
git clean -fd
```

## Preview Cleanup

```bash
git clean -n
```

---

# X. Advanced Search

## Find Text in Repository

```bash
git grep "database"
```

## Search Commits

```bash
git log --grep="bug fix"
```

## Find Commit by Author

```bash
git log --author="John"
```

---

# Y. Useful Aliases

## Create Alias

```bash
git config --global alias.st status
```

## Use Alias

```bash
git st
```

## Popular Aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph"
```

---

# Z. Advanced Production Commands

## Show All References

```bash
git show-ref
```

## View Reflog

```bash
git reflog
```

## Recover Deleted Commit

```bash
git reflog
git checkout <commit-id>
```

## Verify Repository

```bash
git fsck
```

## Garbage Collection

```bash
git gc
```

## Count Objects

```bash
git count-objects -v
```

---

# GitHub-Specific Commands

## Create
