---
title: "Navigating Git: Beginner's Tips and Tricks for QA Engineers "
excerpt: "Git Commands Tips"
comments: false
related: true
header:
    image: /assets/images/xctest_page_object_thumbnail.png
    overlay_image: /assets/images/Git-Logo-2Color.png
    overlay_filter: 0.25
date: 2023-07-30
toc: true
toc_label: "Git Tips and Tricks"
tags:
  - git
---
## Git Tips and Tricks

This is going to be a small cheatsheet for `git` commands that I find myself using *most* of the time. In this blog post, we'll explore some essential Git tips and tricks that will help you streamline your QA automation process.

## 1. **Creating and Cloning Repositories**

To get started with Git, you'll need to create a repository for your automation project. Use the following commands to initialize a new repository and clone an existing one:

```html
# Initialize a new repository locally
git init

# Clone an existing repository from a remote URL
git clone <remote_repository_url>
```

## 2. **Branching and Merging**

Branching is a core feature in Git that allows you to work on new features or bug fixes without affecting the main codebase. Use branches to keep your changes isolated until they are ready to be merged. Here's how you can create and switch to a new branch:

```html
# Create a new branch
git branch <branch_name>

# Switch to the new branch
git checkout <branch_name>
```

Once your changes are complete and tested, merge them back into the main branch:

```bash
# Switch to the main branch
git checkout main

# Merge the feature branch into the main branch
git merge <feature_branch>
```

## 3. **Staging and Committing Changes**

Before saving your changes permanently, stage them for commit using the `git add` command:

```bash
# Stage all changes in the working directory
git add .

# Stage specific files or directories
git add <file1> <file2> <directory>
```

Commit the staged changes with a descriptive message:

```bash
git commit -m "Add login test automation script"
```

## 4. **Viewing History and Diffs**

Git provides powerful tools to review project history and visualize code changes. Use the following commands to view commit history and differences between versions:

```bash
# View commit history
git log

# View the changes made in a specific commit
git show <commit_hash>
```

## 5. **Working with Remote Repositories**

Collaboration is a key aspect of software development, and Git makes it easy to work with remote repositories. Here are some essential commands:

```bash
# Add a remote repository
git remote add <remote_name> <remote_repository_url>

# Push local changes to a remote repository
git push <remote_name> <branch_name>

# Pull changes from a remote repository
git pull <remote_name> <branch_name>
```

## 6. **Handling Conflicts**

Conflicts may arise when merging branches with conflicting changes. Git provides tools to resolve conflicts manually. After resolving conflicts, commit the changes to complete the merge.

## 7. **Ignoring Files**

Some files, such as temporary build artifacts or sensitive data, should not be tracked by Git. Create a `.gitignore` file in the project root to specify which files and directories to ignore:

```
# .gitignore
*.log
/target/
/credentials.txt
```

## 8. **Undoing Changes**

If you need to undo changes, Git offers various options:

```bash
# Discard changes in a file
git checkout -- <file>

# Revert a commit (creates a new commit to undo changes)
git revert <commit_hash>

# Reset to a previous commit (careful, as this removes commits from history)
git reset --hard <commit_hash>
```

If you have any other Git tips or tricks that you find helpful, feel free to share them in the comments below. Happy automating!