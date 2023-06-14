---
layout: post
title: 'Git: Add all changes to commit with Examples'
date: '2023-06-14 23:43:48 +0530'
---

To add all changes to a commit in Git, you can use the following command:

```bash
git add --all
```

This command adds all changes, including modified files, deleted files, and new files, to the staging area, preparing them to be committed.

Alternatively, you can use the shorthand version:

```bash
git add -A
```

Both `git add --all` and `git add -A` accomplish the same task of adding all changes to the commit.

After executing the command, you can proceed with the commit using the `git commit` command:

```bash
git commit -m "Your commit message"
```

This will record the staged changes as a commit with the specified commit message.

Note that the `git add --all` or `git add -A` command should be used with caution, as it adds all changes, including potentially unintended changes. It's a good practice to review the changes before committing to ensure you're including only the desired modifications.