# How to Resolve Git Push Rejection Issues

## Problem Description

When attempting to push code to a remote repository, you may encounter the following error:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:anker12138/kaizi_notebook.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

## Root Cause

This error typically occurs when:
- Someone else (or you on another machine) has pushed new commits to the same branch
- The remote branch contains updates that you don't have locally
- Local and remote branches have diverged

## Solution Methods

### Method 1: Using git pull (Recommended)

This is the most common and safest approach. It automatically merges remote changes.

```bash
# 1. Pull remote changes and auto-merge
git pull origin main

# 2. If there are conflicts, resolve them and commit
git add .
git commit -m "Resolve merge conflicts"

# 3. Push your changes
git push origin main
```

### Method 2: Using git fetch + git merge

This method allows you to review remote changes before merging.

```bash
# 1. Fetch remote changes (without auto-merge)
git fetch origin

# 2. Review changes on the remote branch
git log origin/main

# 3. Merge remote changes
git merge origin/main

# 4. If there are conflicts, resolve them
git add .
git commit -m "Resolve merge conflicts"

# 5. Push changes
git push origin main
```

### Method 3: Using git rebase (For Linear History)

If you want to maintain a clean linear commit history, use rebase.

```bash
# 1. Pull remote changes using rebase
git pull --rebase origin main

# 2. If conflicts occur, resolve them and continue
git add .
git rebase --continue

# 3. Push changes
git push origin main
```

**Note**: If issues occur during rebase, use `git rebase --abort` to cancel the rebase.

## Resolving Merge Conflicts

When using any of the above methods, conflicts may occur if local and remote changes affect the same files:

```bash
# 1. View conflicted files
git status

# 2. Edit conflicted files and resolve conflict markers
# Conflict marker format:
# <<<<<<< HEAD
# Your changes
# =======
# Remote changes
# >>>>>>> origin/main

# 3. Mark conflicts as resolved
git add <conflicted-file>

# 4. Complete the merge
git commit
```

## Best Practices

### 1. Pull Before Push
Make it a habit to pull before pushing:
```bash
git pull origin main
git push origin main
```

### 2. Use Branch Workflow
- Don't work directly on the main branch
- Create feature branches for development
- Merge to main through Pull Requests

```bash
# Create and switch to a new branch
git checkout -b feature/my-feature

# Work on the new branch and commit
git add .
git commit -m "Add new feature"

# Push to remote
git push origin feature/my-feature
```

### 3. Regularly Sync with Remote
Even if not pushing immediately, regularly fetch remote updates:
```bash
git fetch origin
```

### 4. Check Status with git status
Check your local repository status before pushing:
```bash
git status
```

## Special Cases

### Force Push (Use with Caution)

**Warning**: Force pushing overwrites remote history and may cause loss of others' work. Only use when:
- You are the sole contributor
- You are certain you want to discard remote changes

```bash
# Force push (overwrites remote history)
git push -f origin main

# Or use a safer option (only force push if remote has no new commits)
git push --force-with-lease origin main
```

### If You Make a Mistake

If you accidentally make a wrong merge or push:

```bash
# View commit history
git log

# Revert to a previous commit (keep changes)
git reset --soft HEAD~1

# Or completely revert (delete changes, use with caution)
git reset --hard HEAD~1
```

## Summary

- **Simplest solution**: `git pull` then `git push`
- **Pull before push** is the best way to avoid this issue
- **Use branch workflow** to reduce conflicts
- **Use force push with caution** as it may cause data loss

Remember: Git error messages usually provide hints for solutions. Carefully reading these hints often helps you find the answer.
