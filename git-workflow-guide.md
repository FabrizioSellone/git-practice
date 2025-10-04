# 🚀 Git & GitHub CLI Workflow Guide

## 📋 Table of Contents
- Basic Concepts
- Initial Setup
- Common Workflow
- Working with Branches
- Useful Commands Reference

---

## 🎯 Basic Concepts

### Key Terms

**Untracked** → Git sees the file but ignores it completely

**Staging** → Files prepared for the next commit (preparation area)

**Tracking** → Git actively watches the file for changes (after first commit)

**Commit** → Save changes to your **local** Git history

**Push** → Upload local commits to **remote** repository (GitHub)

**Origin** → Nickname/alias for your remote repository on GitHub

**Branch** → Separate timeline of commits for working on features

---

## ⚙️ Initial Setup

### Install GitHub CLI
```bash
brew install gh
```

### Authenticate with GitHub
```bash
gh auth login
```
Choose SSH if you already have SSH keys set up.

### Configure Git Identity
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### Verify Configuration
```bash
git config --global --list
```

---

## 💻 Common Workflow

### 1. Create a New Repository
```bash
gh repo create project-name --public --clone
cd project-name
```

### 2. Check Repository Status
```bash
git status
```
Shows which files are untracked, staged, or modified.

### 3. Create/Edit Files
```bash
echo "# My Project" > README.md
```

### 4. Stage Files

**Stage a single file:**
```bash
git add filename.txt
```

**Stage multiple specific files:**
```bash
git add file1.txt file2.txt file3.txt
```

**Stage all files (most common):**
```bash
git add .
```
The `.` stages everything in the current directory and all subdirectories.

**Stage files by type:**
```bash
git add *.js
```

### 5. Commit Changes
```bash
git commit -m "Descriptive message about what you changed"
```

**Shortcut (stage + commit for tracked files):**
```bash
git commit -am "Your message"
```
Note: This only works for files already tracked, not new files.

### 6. Push to GitHub
```bash
git push origin master
```
- `origin` = your GitHub repository (remote)
- `master` = the branch you're pushing to

### 7. View Repository Online
```bash
gh repo view --web
```

---

## 🌿 Working with Branches

Branches allow you to develop features in parallel without affecting the main codebase.

### View All Branches
```bash
git branch
```
The current branch is marked with `*`.

### Create a New Branch
```bash
git branch feature-name
```

### Switch to a Branch
```bash
git checkout feature-name
```

**Create and switch in one command:**
```bash
git checkout -b feature-name
```

### Work on Your Feature
```bash
# Make changes to files
git add .
git commit -m "Add new feature"
```

### Push Branch to GitHub
```bash
git push origin feature-name
```

### Switch Back to Main Branch
```bash
git checkout master
```

### Merge Branch into Master
```bash
git checkout master
git merge feature-name
```

### Delete a Branch (after merging)
```bash
git branch -d feature-name
```

### Delete Remote Branch
```bash
git push origin --delete feature-name
```

---

## 📚 Useful Commands Reference

### Checking Status & History
```bash
git status                    # Check current status
git log                       # View commit history
git log --oneline            # Compact commit history
git diff                      # See unstaged changes
```

### Remote Repository Info
```bash
git remote -v                 # View remote repositories
git remote show origin        # Detailed remote info
```

### Undoing Changes

**Unstage a file:**
```bash
git reset filename.txt
```

**Discard changes in working directory:**
```bash
git checkout -- filename.txt
```

**Undo last commit (keep changes):**
```bash
git reset --soft HEAD~1
```

**Undo last commit (discard changes):**
```bash
git reset --hard HEAD~1
```

### Pulling Changes from GitHub
```bash
git pull origin master
```

### GitHub CLI Shortcuts
```bash
gh repo view                  # View repo details
gh repo view --web           # Open repo in browser
gh pr create                 # Create pull request
gh pr list                   # List pull requests
gh issue list                # List issues
```

---

## 🔄 Typical Development Workflow

```bash
# 1. Create feature branch
git checkout -b new-feature

# 2. Make changes and commit regularly
git add .
git commit -m "Implement part 1 of feature"

# More changes...
git add .
git commit -m "Complete feature implementation"

# 3. Push feature branch
git push origin new-feature

# 4. Create pull request (via GitHub CLI or web)
gh pr create

# 5. After approval, merge and clean up
git checkout master
git pull origin master
git branch -d new-feature
git push origin --delete new-feature
```

---

## 💡 Pro Tips

✅ **Commit often** with clear messages

✅ **Always check status** before committing: `git status`

✅ **Use branches** for new features or experiments

✅ **Pull before push** to avoid conflicts: `git pull origin master`

✅ **Never commit** sensitive data (passwords, API keys, etc.)

✅ Use `.gitignore` file to exclude files you don't want tracked

---

## 🆘 Common Scenarios

### Scenario: Made changes but want to start over
```bash
git checkout -- .
```

### Scenario: Committed to wrong branch
```bash
# Save the commit
git log  # Copy the commit hash

# Undo the commit
git reset --hard HEAD~1

# Switch to correct branch and apply
git checkout correct-branch
git cherry-pick <commit-hash>
```

### Scenario: Need to update branch with latest master
```bash
git checkout feature-branch
git merge master
```

---

**Remember:** Git saves locally, Push saves to GitHub! 🚀