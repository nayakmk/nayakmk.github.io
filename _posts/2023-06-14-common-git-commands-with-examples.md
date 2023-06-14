---
layout: post
title: 'Git: Common Commands with Examples'
date: '2023-06-14 23:37:02 +0530'
categories: [GIT]
tags: [command]
---

Here are some common Git commands with examples to illustrate their usage:

1. **git init**: Initializes a new Git repository in the current directory.
```bash
git init
```

2. **git clone [repository URL]**: Creates a local copy of a remote repository.
```bash
git clone https://github.com/user/repo.git
```

3. **git add [file]**: Adds a file or changes to the staging area.
```bash
git add myfile.txt
```

4. **git commit -m "[commit message]"**: Records the staged changes to the repository with a commit message.
```bash
git commit -m "Add new feature"
```

5. **git status**: Shows the status of the working directory and staging area.
```bash
git status
```

6. **git push**: Pushes the committed changes to a remote repository.
```bash
git push
```

7. **git pull**: Fetches and merges changes from a remote repository to the local repository.
```bash
git pull
```

8. **git branch**: Lists all local branches and indicates the current branch.
```bash
git branch
```

9. **git checkout [branch]**: Switches to the specified branch.
```bash
git checkout feature-branch
```

10. **git merge [branch]**: Merges the specified branch into the current branch.
```bash
git merge feature-branch
```

11. **git log**: Shows the commit history.
```bash
git log
```

12. **git remote add [name] [repository URL]**: Associates a remote repository with a name.
```bash
git remote add origin https://github.com/user/repo.git
```

13. **git remote -v**: Lists all remote repositories and their URLs.
```bash
git remote -v
```

14. **git diff**: Shows the differences between the working directory and the staging area.
```bash
git diff
```

15. **git reset [file]**: Unstages a file from the staging area.
```bash
git reset myfile.txt
```

16. **git revert [commit]**: Creates a new commit that undoes the changes introduced by the specified commit.
```bash
git revert abcdefg
```

17. **git stash**: Temporarily saves changes that are not ready to be committed.
```bash
git stash
```

18. **git branch -d [branch]**: Deletes the specified branch.
```bash
git branch -d feature-branch
```

These examples demonstrate how these common Git commands can be used in various scenarios. Remember to adjust the commands based on your specific repository and branch names.