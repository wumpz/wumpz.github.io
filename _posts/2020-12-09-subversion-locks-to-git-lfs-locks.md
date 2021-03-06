---
title:  "Subversion Locks to Git LFS Locks"
categories: [scm]
tags: [subversion, lock, git, lfs]
classes: wide
---

Since Git is somehow state of the art of source code management systems, we wanted to switch from Subversion to it. All the easy branching and high performance commits (I know its only local :) ) sounds really good. If we had only source code files in our repository the decision to switch would be a no brainer. By the way the migration of our Subversion source code repositories was very easy to do.

But we have a lot of binary files within our Subversion repository. So we are in a real merge problem here. There is no meaningful way to merge two versions of a binary file. Fortunately Subversion has this nice locking mechanism which is also well supported by all kind of client software tools, e.g. **TortoiseSVN**.

The file lock technique removes the need to merge binary files, since only one is able to change the file and while locking the file, it will be actualized to the current version. Additionally the lockable files are read only for the client until the lock is done.

Bit does Git offer this as well? Natively not, but the Git LFS extension does.

# Git LFS

[Git Large File Storage (LFS)](https://git-lfs.github.com/) is used to better store large binary files within your Git Repository. All those files within your Git repository are replaced by some text pointers that point to the storage and the storage itself is done at a different location. By the way I liked this [Atlassian tutorial](https://www.atlassian.com/git/tutorials/git-lfs) of Git LFS much more.

We come to it later, why we did not use this LFS stuff. But Git LFS comes with another nice feature: [Locking Git LFS files](https://www.atlassian.com/git/tutorials/git-lfs#locking-files).

Here a quote from this tutorial:

> Unfortunately, there is no easy way of resolving binary merge conflicts. With Git LFS file locking, you can lock files by extension or by file name and prevent binary files from being overwritten during a merge.

So we are finally there to replace Subversion locking. But you may think, in Git there are multiple branches, so how does this help?

It works for **all branches at the same time** or cross branch. So if you lock a file in one branch, another user on another branch on maybe another computer is not able to lock this file as well. The files are identified by their absolute path within the Git repository.

# Simple Git LFS Quickstart

First of all you need a Git LFS supporting server. We use **Gitea** which is easy to setup, a docker image is available, and gives you a GitHub like experience out of the box.

I will not explain how to setup a simple new Git repository, but to use Git LFS you have to first initialize it.

```bash
git lfs install
```

To make Git LFS handle files you have to configure it to track it, e.g. to track ** *.avi ** files you have to do

```bash
git lfs track "*.avi"
```

The configuration is stored within the **.gitattributes** file in your repository and holds one line for each trackable and looks liked

```bash
*.avi filter=lfs diff=lfs merge=lfs -text
```

Doing a push does all you need. So Git LFS takes control.

# Locking Git LFS Files

To activate the locking you have to configure the tracking like

```bash
git lfs track "*.avi" --lockable
```

now the configuration in **.gitattributes** looks like

```bash
*.avi filter=lfs diff=lfs merge=lfs -text lockable
```

> Note: Only after a complete checkout or a new clone of your repository from your server all **lockable** files are set read only.

# Migrate the whole history of Subversion to Git LFS

We want to do it right. So the complete history goes into Git including author correction metadata and stuff. I will skip the commands and configuration around `git svn clone` here. After this transfer completes you transform your repository using the following commands:

```bash
# dry run of your migration
git lfs migrate info --everything --include="*.avi,*.jpg"

# perform migration
git lfs migrate import --everything --include="*.avi,*.jpg" --verbose
```
With the first command you could check, what Git LFS will change and with the second the change is actually done. The complete history is rewritten and all files matching our include filter are replaced by Git LFS and stored elsewhere.

# Using locks in Git LFS

There are several Git commands to use this locking techniques.

```bash
# to lock a file
git lfs lock movie.avi

# to unlock a file
git lfs unlock movie.avi

# to list all locks of your repository
git lfs locks
```

There are also some tools good tools that provide this locking technique, for instance **TortoiseGIT**. Some tools support using Git LFS but does not support those locking commands. So in these cases you have to use the command line.


# First Conclusion

As simple as this, the configured system worked as expected. Locks are treated globally, if all users use the same server.

So problem solved ...

# The problem of the repository size

So all seemed fine until we looked at the repository sizes. Our **700MB Subversion** repository, was migrated to a **Git** repository of nearly the same size and then was inflated to nearly **12GB in Git LFS**.  After some investigation it turned out, that this has to do with the way Git LFS stores the tracked files. You have to now, that there is no more diff processing in place like in normal Git. So every version of our binary files holds a complete copy of this file.

Our binary files were not so big, but have a lot of changes and therefore result in a lot of stored versions in Git LFS.

# Solution

Wouldn't it be cool if we could use this file locking without this Git LFS storage and keep the small Git repository size?

**In fact you can.**

There are two rules you have to follow.

1. Just edit the lines in your **.gitattributes** to look like

    ```bash
    *.avi lockable
    ```

2. Due to an open issue of Git or Git LFS you have to add at least one *real* trackable and include one file that is stored in LFS due to this one rule. Git somehow does not process die **.gitattributes** rules without it.

    ```bash
    *.avi lockable
    .MyDummyFile filter=lfs diff=lfs merge=lfs -text lockable
    ```

    and don't forget to add a **.MyDummyFile** to your repository.

With this simple trick we can use **Locking Git LFS Files** without storing the files in LFS. Testing our repository size results in **700MB** again. In fact the normal Git storage works for versioned binary files quite well.

# Conclusion

If you consider to use Git LFS you should do a live test with your files. So you get a better idea, how this will affect your repository performance and size. Git LFS is for really large files that do not change much and should be stored elsewhere. For file of sizes of some meg bytes the normal Git storage is the better choice. The interesting part here is that **Git LFS File Locking* can be used without LFS storage.
