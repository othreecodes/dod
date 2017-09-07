---
layout: post
title: It's Git, Init? (1)
excerpt: "How do you collaborate, or work on single projects? Here's a guide to working efficiently with git"
categories: articles
tags: [git, version-control, development, gitlab, github]
comments: true
share: true
modified: 2017-08-26 19:24:54.454961
---

Soo...

I work with git a lot, and frankly I don't see why any developer would want to create a project and not work with git.
But chill...


# WTH is [`Git`](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics) ?

Git is a system that records changes to a file or set of files over time so that you can recall specific versions later.

Many people’s version-control method of choice is to copy files into another directory `(perhaps a time-stamped directory, if they’re clever)`. This approach is very common because it is so simple, but it is also incredibly error prone. It is easy to forget which directory you’re in and accidentally write to the wrong file or copy over files you don’t mean to.

# Getting Started With GIT
There are a lot of [Git hosting sites](https://git.wiki.kernel.org/index.php/GitHosting) but to work with git locally on your system, you would need to get it installed. 

If you want to install the basic Git tools on Linux via a binary installer, you can generally do so through the basic package-management tool that comes with your distribution. If you’re on Debian you could use `apt-get`

```bash 
$ apt-get install git
```

If you're on Windows , Install [Git bash](https://git-for-windows.github.io/)

If you're on OSX, assuming you have [Homebrew](http://brew.sh/) installed

```bash
$ brew install git
```

# My first git project 
Next confirm you have git installed, Open a terminal and type. (Windows users will have open git bash, so sad... If you use windows and you wanna join Team Unix [Read this](https://www.google.com.ng/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&cad=rja&uact=8&ved=0ahUKEwiJ-vTqiOPVAhVX5mMKHUIhCZcQFgguMAI&url=https%3A%2F%2Fbuiltvisible.com%2Fthe-ubuntu-installation-guide%2F&usg=AFQjCNGlEkZ0WgLCaRSW5CFDa4fWFNm-lA))
<img src="https://media.licdn.com/mpr/mpr/AAEAAQAAAAAAAAgZAAAAJGVkNDUzNGI5LTMzYzAtNGEwZS04Mzk0LTI3NTc0NmU5NjU0Mg.png">

> *WINDOWS ONLY: Open the context menu by right clicking in the directory of your project folder, then click to open git bash in that directory and you're good to go.*

```bash
dod@othree:~$ git --version
git version 2.14.1
```
If you get an output showing the version then you have successfully installed git.
Now lets do some Magic.

### Create a new project directory and initialize git

```bash
dod@othree:~$ mkdir project
dod@othree:~$ cd project
dod@othree:~/project$ git init
Initialized empty Git repository in /home/dod/project/.git/ 
```

>PS: If you're using Git Bash (Windows), It these steps would work for you too.

## `git init`
The `git init` command creates a new Git repository (basically a .git directory with subdirectories for objects, refs/heads, refs/tags, and template files) .It can be used to convert an existing, unversioned project to a Git repository or initialize a new, empty repository.  Most other Git commands are not available outside of an initialized repository, so this is usually the first command you'll run in a new project.

# Repository Contents

The purpose of Git is to manage a project, or a set of files, as they change over time. Git stores this information in a data structure called a repository.

A git repository contains, among other things, the following:
 
- A set of commit objects. (We'll go through this shortly )
- A set of references to commit objects, called heads.

The Git repository is stored in the same directory as the project itself, in a subdirectory called .git.

>There is only one .git directory, in the root directory of the project. The repository is stored in files alongside the project. It contains all of the necessary Git metadata for the new repository. 

You can always check the status of your repository by running `git status`

```bash
dod@othree:~$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```
## `git status`
The git status command displays the state of the working directory and the staging area. It lets you see which changes have been staged, which haven't, and which files aren't being tracked by Git.

> "`On branch master`"  We'll talk about branching in subsequent tutorials

# Commit !
## A commit object contains three things:

- A set of files, reflecting the state of a project at a given point in time.
- References to parent commit objects.
- An SHA1 name, a 40-character string that uniquely identifies the commit object. The name is composed of a hash of relevant aspects of the commit, so identical commits will always have the same name.

### Now Lets create a new file called "readme.md"
open your fav' text editor and while in your repository directory and type some stuff into it  

```
This is a the read me file
```
>`/home/dod/project/readme.md` 

[![Current Folder]({{site.url}}/images/fileloc.png)]({{site.url}}/images/fileloc.png "Current Folder Structure")

Now were going to add this file to our repository using the `git add` command but first let's check the status of our repository again 
```bash 
dod@othree:~/project$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	readme.md

nothing added to commit but untracked files present (use "git add" to track)
dod@othree:~/project$ git add readme.md

```

## `git add <file>`
`git add`  adds a modified and a new (untracked) file in the current directory and all subdirectories to the staging area (a.k.a. the index), thus preparing them to be included in the next git commit .

Finally lets make a commit 
```bash
dod@othree:~/project$ git commit -m "Initial commit"
[master 2eb2d88] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 readme.md 
```
>Your output may be a bit more but you can ignore that...for now

## `git commit -m <commit-message>`
Basically git commit records changes to the repository.
The "`-m`" flag specifies the commit message.

Now we can check out our progress using `git log`
```bash
dod@othree:~/project$ git log
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Sat Aug 26 18:55:42 2017 +0100

    initial commit

``` 

## `git log` 
By default, with no arguments, git log lists the commits made in that repository in reverse chronological order – that is, the most recent commits show up first. As you can see, this command lists each commit with its SHA-1 checksum, the author’s name and email, the date written, and the commit message.

Now go ahead and make more commits and check the run git log.
We'll go deeper in subsequent tutorials>

Let me know if I missed anything in the comments section.

# TL;DR
Git is a system that records changes to a file or set of files over time so that you can recall specific versions later.

You can install `git` via a package manger or [Git bash](https://git-for-windows.github.io/) for windows.

`git init`: This command creates an empty Git repository - basically a .git directory with subdirectories for objects, refs/heads, refs/tags, and template files. 

`git status`: The git status command displays the state of the working directory and the staging area.

`git add`: adds a modified and a new (untracked) file in the current directory and all subdirectories to the staging area .

`git commit`: Basically git commit records changes to the repository.

`git log`: git log lists the commits made in that repository in reverse chronological order 