# 🚀 Git & GitHub CLI Workflow Guide

## 📋 Table of Contents
- Basic Concepts
- Initial Setup
- Common Workflow
- Converting Existing Project to Git
- Working with Branches
- Useful Commands Reference

---

## 📦 Converting Existing Project to Git

If you have an existing project on your laptop and want to start using Git:

### Step 1: Navigate to Your Project
```bash
cd /path/to/your/project
```

### Step 2: Initialize Git Repository
```bash
git init
```
This creates a `.git` folder in your project directory.

### Step 3: Add All Files
```bash
git add .
```

### Step 4: Make Initial Commit
```bash
git commit -m "Initial commit"
```

### Step 5: Create GitHub Repository and Link It
```bash
gh repo create project-name --public --source=. --remote=origin
```
Options:
- `--public` or `--private` for repository visibility
- `--source=.` uses current directory
- `--remote=origin` sets up the remote connection

### Step 6: Push to GitHub
```bash
git push -u origin dev
```
The `-u` flag sets upstream tracking for future pushes.

### Optional: Create .gitignore
Before adding files, create a `.gitignore` to exclude unnecessary files:
```bash
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore
echo ".DS_Store" >> .gitignore
git add .gitignore
```

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

**Main/Dev** → The primary branch (previously called "master", now commonly "main" or "dev")

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
git push origin dev
```
- `origin` = your GitHub repository (remote)
- `dev` = the branch you're pushing to

**First time pushing a branch:** Use `-u` to set upstream tracking:
```bash
git push -u origin dev
```
After this, you can just use `git push` without specifying origin and branch name.

**What `-u` does:** Creates a tracking relationship between local and remote branch, allowing shortcuts like `git push` and `git pull` without specifying the remote.

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

**Pro tip:** Push to current branch automatically:
```bash
git push origin HEAD
```

---

## 🔀 Merging Branches

### Method 1: Merge via Pull Request (Team Workflow)

**Step 1: Create Pull Request**
```bash
gh pr create
```
Follow the prompts to add title and body.

**Step 2: View Pull Request**
```bash
gh pr list                # List all PRs
gh pr view 1              # View PR #1
gh pr view --web          # Open in browser
```

**Step 3: Merge Pull Request**
```bash
gh pr merge 1
```
Choose merge method:
- **Create a merge commit** - Preserves full history (recommended)
- **Rebase and merge** - Linear history
- **Squash and merge** - Combines commits into one

Delete the branch when prompted.

### Method 2: Merge Locally (Quick Workflow)

**Switch to target branch and merge:**
```bash
git checkout dev
git merge feature-name
```

**Fast-forward merge** happens when no conflicts exist - dev simply moves forward.

**Push to GitHub:**
```bash
git push origin dev
```

**Delete local branch:**
```bash
git branch -d feature-name
```

---

## ⚠️ Handling Merge Conflicts

Conflicts occur when the same file is modified differently in two branches.

### Identifying Conflicts
```bash
git merge feature-branch
# Output: CONFLICT (content): Merge conflict in filename.txt
```

### Viewing Conflict
```bash
cat filename.txt
```
You'll see:
```
<<<<<<< HEAD
Your current branch content
=======
Incoming branch content
>>>>>>> feature-branch
```

### Resolving Conflicts

**Step 1: Edit the file**
Remove conflict markers and keep desired content:
```bash
nano filename.txt
# Or manually edit to combine or choose content
```

**Step 2: Mark as resolved**
```bash
git add filename.txt
```

**Step 3: Complete the merge**
```bash
git commit -m "Merge feature-branch: Resolve conflicts"
```

### Abort a Merge
If you want to cancel the merge:
```bash
git merge --abort
```

### View Merge History
```bash
git log --oneline --graph
```
Shows visual representation of branch merges.

---

## 🎯 Branch Best Practices

✅ **Always create a branch** for new features or fixes

✅ **Use descriptive names** like `add-login-page` or `fix-navbar-bug`

✅ **Keep branches focused** - one feature per branch

✅ **Merge or delete** branches after they're done

✅ **Pull before creating** a new branch to start from latest code:
```bash
git checkout dev
git pull origin dev
git checkout -b new-feature
```

✅ **Regularly sync** feature branches with dev:
```bash
git checkout feature-branch
git merge dev
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
git pull origin dev
```

Or after setting upstream with `-u`:
```bash
git pull
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
git push -u origin new-feature

# 4. Create pull request (via GitHub CLI or web)
gh pr create

# 5. After approval, merge and clean up
git checkout dev
git pull origin dev
git branch -d new-feature
git push origin --delete new-feature
```

---

## 💡 Pro Tips

✅ **Commit often** with clear messages

✅ **Always check status** before committing: `git status`

✅ **Use branches** for new features or experiments

✅ **Pull before push** to avoid conflicts: `git pull origin dev`

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

### Scenario: Need to update branch with latest dev
```bash
git checkout feature-branch
git merge dev
```

---

## 🔧 Additional Tips

### Changing Default Branch Name
If your repository still uses "master", you can rename it to "dev" or "main":
```bash
# Rename locally
git branch -m master dev

# Push renamed branch and set upstream
git push -u origin dev

# Delete old branch on GitHub
git push origin --delete master
```

### Set Default Branch for New Repositories
```bash
git config --global init.defaultBranch dev
```

---

**Remember:** Git saves locally, Push saves to GitHub! 🚀