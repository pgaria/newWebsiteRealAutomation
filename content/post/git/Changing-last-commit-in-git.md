---
title: "Git: Changing Last Commit Using --amend."
date: 2018-03-28
draft: false
author: Pawan Garia
description: "If a commit message contains unclear, incorrect, or sensitive information, you can amend it locally and push a new commit with a new message to GitHub."
categories: [ "Tutorials" ]
keywords: "git,commit,amend"
tags: [
    "git",
    "commit"
]
---
This tutorial will cover various methods of rewriting and altering Git history. Git uses a few different methods to record changes. If you ever find that you accidentally left something out of your last commit, be it a file or an extra change to a file that you just committed, don't worry. It can easily be fixed using the `git commit --amend`.

The git commit --amend command is a convenient way to modify the most recent commit. It lets you combine staged changes with the previous commit instead of creating an entirely new commit. It can also be used to simply edit the previous commit message without changing its snapshot. But, amending does not just alter the most recent commit, it replaces it entirely, meaning the amended commit will be a new entity with its own ref. To Git, it will look like a brand new commit

All you have to do is stage the extra changes like you would for a normal commit in git. So let's update the last commit in real time with the example:

### Step 1:
You have the file edited and now we have to first add the file.
```
git add .
```
### Step 2:
And then just commit with the `--amend` argument.
```
git commit --amend
```
This will prompt you with the previous commit message as a default, where you can update the commit message or leave the previous commit message.
![git-amend-commit-edit-window](/img/git/git-amend-commit-edit-window.png)

When you're done you can check `git log --stat` to see your amended commit with the extra changes.

### Step 3:
As we have already pushed the previous commit to GitHub, we have to force push our new amend commit changes. Use the `push --force` command to force push over the old commit.
```
git push --force origin master
```
I strongly discourage force pushing, since this changes the history of your repository. If you force push, people who have already cloned your repository will have to manually fix their local history.

### Step 4:
It's time to see our new changes in the git log Again.
```
git log
```
You will be able to see the commit you added and changes in the logs and previous commit is gone.

You can read more about the rewriting the history in more details in the [Git Tutorials](https://www.atlassian.com/git/tutorials/rewriting-history). You should understand the implications of rewriting history if you amend a commit that has already been published.
