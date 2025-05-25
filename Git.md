# Git

## Introduction

Git is a distributed version control system designed to handle everything from small to very large projects with speed and efficiency. Git was created by Linus Torvalds in 2005 for development of the Linux kernel and has since become the most widely used version control system for software development.

## Installation

### macOS

```bash
# Using Homebrew
brew install git

# Verify installation
git --version
```

### Linux

```bash
# Debian/Ubuntu
sudo apt-get update
sudo apt-get install git

# Fedora
sudo dnf install git

# Verify installation
git --version
```

### Windows

1. Download the installer from [git-scm.com](https://git-scm.com/download/win)
2. Run the installer, following the prompts
3. Verify installation by opening Git Bash and running `git --version`

## Configuration

### Initial Setup

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor "vim"
```

### View Configuration

```bash
# List all configurations
git config --list

# View specific configuration
git config user.name
```

### Configuration Levels

```bash
git config --system    # System-wide configuration
git config --global    # User-level configuration
git config --local     # Repository-specific configuration
```

## Basic Git Workflow

### Creating a New Repository

```bash
# Initialize a new repository
git init

# Clone an existing repository
git clone https://github.com/username/repository.git
git clone git@github.com:username/repository.git  # Using SSH
```

### Basic Commands

```bash
# Check status of working directory
git status

# Add files to staging area
git add filename.txt        # Add specific file
git add .                   # Add all files
git add directory/          # Add all files in directory
git add -p                  # Interactively select changes to add

# Commit changes
git commit -m "Commit message"
git commit -a -m "Commit message"  # Add and commit tracked files in one step

# Push changes to remote
git push origin main
```

### Working with Remote Repositories

```bash
# List remote repositories
git remote -v

# Add a remote repository
git remote add origin https://github.com/username/repository.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repository.git

# Fetch changes from remote
git fetch origin

# Pull changes from remote (fetch + merge)
git pull origin main

# Push changes to remote
git push -u origin main  # -u sets upstream for current branch
```

## Branches

### Working with Branches

```bash
# List branches
git branch            # Local branches
git branch -r         # Remote branches
git branch -a         # All branches

# Create a new branch
git branch branch-name

# Switch to a branch
git checkout branch-name
git switch branch-name  # Git 2.23+

# Create and switch to a new branch
git checkout -b new-branch
git switch -c new-branch  # Git 2.23+

# Delete a branch
git branch -d branch-name       # Safe delete (if merged)
git branch -D branch-name       # Force delete
```

### Merging

```bash
# Merge a branch into current branch
git merge branch-name

# Merge with no fast-forward
git merge --no-ff branch-name

# Abort a merge with conflicts
git merge --abort
```

### Rebasing

```bash
# Rebase current branch onto another branch
git rebase main

# Interactive rebase
git rebase -i HEAD~3  # Rebase last 3 commits

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

## Inspecting Repository

### Viewing History

```bash
# View commit history
git log

# Compact view
git log --oneline

# View history with graph
git log --graph --oneline --all

# View history for specific file
git log -- filename.txt

# Show changes over time for specific file
git log -p filename.txt
```

### Examining Changes

```bash
# Show changes between commits
git diff                     # Working directory vs staging area
git diff --staged            # Staging area vs last commit
git diff HEAD                # Working directory vs last commit
git diff commit1 commit2     # Between two commits
git diff branch1...branch2   # Between branches (common ancestor)
```

### Checking Specific Commits

```bash
# Show commit details
git show commit-hash

# Show file at specific commit
git show commit-hash:filename.txt
```

## Undoing Changes

### Unstaging Files

```bash
# Unstage a file
git restore --staged filename.txt  # Git 2.23+
git reset HEAD filename.txt        # Older Git versions
```

### Discarding Changes

```bash
# Discard changes in working directory
git restore filename.txt      # Git 2.23+
git checkout -- filename.txt  # Older Git versions

# Restore file to specific commit
git restore --source=commit-hash filename.txt  # Git 2.23+
```

### Reset Commits

```bash
# Soft reset (move HEAD, keep changes staged)
git reset --soft HEAD~1

# Mixed reset (move HEAD, unstage changes)
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (move HEAD, discard changes)
git reset --hard HEAD~1
```

### Reverting Commits

```bash
# Create a new commit that undoes changes
git revert commit-hash

# Revert multiple commits
git revert start-commit-hash..end-commit-hash
```

### Amending Commits

```bash
# Modify the last commit
git commit --amend -m "New commit message"

# Add files to the last commit
git add forgotten-file.txt
git commit --amend --no-edit
```

## Stashing Changes

```bash
# Stash current changes
git stash

# Stash with message
git stash save "Work in progress for feature X"

# List stashes
git stash list

# Apply most recent stash
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Drop a stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

## Advanced Git Features

### Git Hooks

Hooks are scripts that run automatically when certain Git events occur.

Common hooks:

- pre-commit
- commit-msg
- pre-push

Located in `.git/hooks/` directory.

### Git Submodules

```bash
# Add a submodule
git submodule add https://github.com/username/repository.git path/to/submodule

# Initialize and update submodules
git submodule init
git submodule update

# Clone repository with submodules
git clone --recurse-submodules https://github.com/username/repository.git
```

### Git LFS (Large File Storage)

```bash
# Install Git LFS
brew install git-lfs  # macOS with Homebrew
git lfs install       # Initialize Git LFS

# Track large files
git lfs track "*.psd"
git lfs track "*.iso"

# Make sure to commit .gitattributes file
git add .gitattributes
```

### Git Worktrees

```bash
# Create a new worktree
git worktree add ../path-to-worktree branch-name

# List worktrees
git worktree list

# Remove a worktree
git worktree remove ../path-to-worktree
```

## Git Best Practices

### Commit Messages

1. Use imperative mood ("Add feature" not "Added feature")
2. First line should be 50 characters or less
3. Separate subject from body with a blank line
4. Body should explain what and why, not how

Example:

```
Add user authentication feature

- Implement JWT token-based authentication
- Add login and registration endpoints
- Include middleware for protected routes

This resolves issue #42
```

### Branching Strategies

#### Git Flow

- `main`/`master`: Production code
- `develop`: Development code
- `feature/*`: New features
- `release/*`: Preparing for a release
- `hotfix/*`: Urgent fixes for production

#### GitHub Flow

- `main`/`master`: Always deployable
- Topic branches for features and fixes
- Pull requests for code review
- Deploy after merging to main

#### Trunk-Based Development

- Short-lived feature branches
- Frequent merges to the main branch
- Feature flags for incomplete features

### Helpful Tips

1. Commit frequently with focused changes
2. Pull before pushing to avoid conflicts
3. Use `.gitignore` for files that shouldn't be tracked
4. Regularly clean up local and remote branches
5. Use tags for marking versions

## Troubleshooting

### Common Issues

```bash
# Fix corrupted repository
git fsck

# Recover lost commits (after hard reset)
git reflog
git cherry-pick commit-hash

# Fix detached HEAD state
git checkout branch-name

# Force push with lease (safer than force push)
git push --force-with-lease

# Clean untracked files
git clean -n  # Dry run
git clean -fd  # Remove untracked files and directories
```

### Git Garbage Collection

```bash
# Run garbage collection
git gc

# Aggressive garbage collection
git gc --aggressive

# Prune unreachable objects
git prune
```

## Advanced Git Commands

### Cherry-pick

```bash
# Apply a specific commit to the current branch
git cherry-pick commit-hash

# Cherry-pick without committing
git cherry-pick -n commit-hash
```

### Bisect

```bash
# Start bisect
git bisect start

# Mark current version as bad
git bisect bad

# Mark a known good commit
git bisect good commit-hash

# After finding the problem commit
git bisect reset
```

### Blame

```bash
# See who changed each line of a file
git blame filename.txt

# Ignore whitespace
git blame -w filename.txt

# Show specific line range
git blame -L 10,20 filename.txt
```

### Archive

```bash
# Create a zip archive of the repository
git archive --format=zip HEAD > archive.zip

# Create a tarball of a specific branch
git archive --format=tar branch-name > archive.tar
```

## Git Tools and Integrations

### GUI Clients

- GitKraken
- Sourcetree
- GitHub Desktop
- VS Code Git integration
- Git Extensions

### CI/CD Integration

- GitHub Actions
- GitLab CI/CD
- Jenkins
- CircleCI
- Travis CI

### Code Review Tools

- GitHub Pull Requests
- GitLab Merge Requests
- Gerrit
- Review Board

## Further Resources

### Official Documentation

- [Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)

### Interactive Learning

- [Learn Git Branching](https://learngitbranching.js.org/)
- [GitHub Learning Lab](https://lab.github.com/)

### Cheat Sheets

- [GitHub Git Cheat Sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
- [Atlassian Git Cheat Sheet](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)
