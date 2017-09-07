---
layout: post
title: It's Git, Init? (2)
excerpt: "How do you collaborate, or work on single projects? Here's a guide to working efficiently with git"
categories: articles
tags: [git, version-control, development, gitlab, github, branching]
comments: true
share: true
modified: 2017-09-07 19:24:54.454961
---

Soo...

In my last post, I wrote about getting started with git. If you haven't read that it's [here]() 
In this post, we'll be going through working with branches in git as well as moving through commit histories.
There's this cool article that goes into the anatomy of git commit. You should check it out 

[THE ANATOMY OF A GIT COMMIT](https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html)

# Branching
>Branching is the way to work on different versions of a repository at one time.
A branch is a version of your repository

Now let's get back into our project directory and run `git status`

```bash
dod@othree:~/project$ git status
On branch master
nothing to commit, working tree clean
```
This says that we are on the `master` branch, but hell, When did we even create a branch?
The master branch was created when we ran `git init` in our project dir. It's the default branch.
As you initially make commits, you're given a master branch that points to the last commit you made. Every time you commit, it moves forward automatically.

<!-- HEAD is like a pointer that points to the current branch. When you checkout a different branch, HEAD changes to point to the new one -->

## Think of it like a Tree

<img src="http://static.tvtropes.org/pmwiki/pub/images/snoopy_laughing_at_his_joke.jpg">

The master is the trunk, of which you can branch off from. You start a new branch off of the original master so that you can make changes to the project without affecting the original master branch.

To create a new branch you run the command with the name `testing` we run the command

```bash
dod@othree:~/project$ git branch testing
```
It doesn't give any output, but if we run `git branch --list` we see the names of branches we have and the branch we are currently working on

```bash
dod@othree:~/project$ git branch --list
* master
  testing
```

## `git branch`
If --list is given, or if there are no non-option arguments, existing branches are listed; the current branch will be highlighted with an asterisk.

`git branch <name>` Creates a new branch with the specified name. 
This creates a new pointer to the same commit you’re currently on.

<img src="https://git-scm.com/book/en/v2/images/two-branches.png">

To switch to this new created branch we use the `git checkout` command
```bash
dod@othree:~/project$ git checkout testing
Switched to branch 'testing'
dod@othree:~/project$ git status
On branch testing
nothing to commit, working tree clean
```
Now we have successfully switched to the `testing` branch 

## `git checkout`
Once you checkout to a particular branch, the `HEAD` points to that branch.
Then you can begin to work on that verision (branch) of your repository.
> There's a lot more that `git checkout` does, we'll be going over them subsequently

Now lets make a few more commits shall we ?

<img src="https://i.imgflip.com/1vgpwy.jpg" title="GIT IS EASY?"/>

So now on our `testing`, Let's edit our readme.md and also add more files to our repository.
> HINT: Use `git add .` to stage all files before committing them 

```bash
dod@othree:~/project$ git add .
dod@othree:~/project$ git commit -am "added badys files"
[master b1b6e4] added bady files
 2 files changed, 3 insertions(+)
 create mode 100644 Maximbady.txt
 create mode 100644 chakalaka.txt
dod@othree:~/project$ git log
commit b1b6e40ab12734fd5995e1884239b16aa6655e3d (HEAD -> testing)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Thu Sep 7 20:01:40 2017 +0100

    added badys files

commit e4f55bf96a99585e0d6c24f0c1b166f84ec649a0 (master)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Sat Aug 26 18:55:42 2017 +0100

    initial commit
```

From the terminal output, we can infer that,
1. The `HEAD` is currently at testing i.e we are on the testing branch 
2. Our Commit(s) from the master branch at the point of "`checking out`" are also on the testing branch

Now lets make one more commit 
```bash
dod@othree:~/project$ echo name > me.txt # just creating a random file with `name` in it
dod@othree:~/project$ git add .
[testing bbb0978] added my file
 1 file changed, 1 insertion(+)
 create mode 100644 me.txt
dod@othree:~/project$ git log
commit bbb097852554222fde426537cd18f5470c1f67ff (HEAD -> testing)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Thu Sep 7 20:05:34 2017 +0100

    added my file

commit b1b6e40ab12734fd5995e1884239b16aa6655e3d
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Thu Sep 7 20:01:40 2017 +0100

    added badys files

commit e4f55bf96a99585e0d6c24f0c1b166f84ec649a0 (master)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Sat Aug 26 18:55:42 2017 +0100

    initial commit
```

After every commit, `HEAD` moves up to that particular commit. So now, let's switch back to master and see what we have.

```bash
postgres@othree:~/project$ git checkout master
Switched to branch 'master'
postgres@othree:~/project$ git log
commit e4f55bf96a99585e0d6c24f0c1b166f84ec649a0 (HEAD -> master)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Sat Aug 26 18:55:42 2017 +0100

    initial commit
postgres@othree:~/project$ 

```

All commits from the testing branch aren't brought into master, and our working directory is updated to the state of the master branch. You can say that the testing branch is ahead of the master.

## Now that we are able to switch/move through branches, how do we move through commits?
It's easy, fist let us get back into out testing branch (git checkout testing).
Git log shows this
```bash
dod@othree:~/project$ git log
commit bbb097852554222fde426537cd18f5470c1f67ff (HEAD -> testing)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Thu Sep 7 20:05:34 2017 +0100

    added my file

commit b1b6e40ab12734fd5995e1884239b16aa6655e3d
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Thu Sep 7 20:01:40 2017 +0100

    added badys files

commit e4f55bf96a99585e0d6c24f0c1b166f84ec649a0 (master)
Author: Obi Uchenna David <daviduchenna@outlook.com>
Date:   Sat Aug 26 18:55:42 2017 +0100

    initial commit
```

To "switch" through commits we use the `git checkout <commit-name>` command
> The commit name  is An SHA1 name, a 40-character string that uniquely identifies the commit object. In layman terms, That long rubbish that's created with every commit.

```bash
dod@othree:~/project$ git checkout b1b6e40ab12734fd5995e1884239b16aa6655e3d
Note: checking out 'b1b6e40ab12734fd5995e1884239b16aa6655e3d'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at b1b6e40... added badys files
```

> HINT: Run git status and git log. You'll get some info on what's going on

# detached HEAD
In a nutshell, detached HEAD state occurs when you try to checkout something that is not a local branch. It can be a commit, a tag or a remote branch.
When you make changes and commit them, these changes do not belong to any branch but that doesn’t mean the commits are deleted if you eventually switch to another branch.

As displayed in the command line interface, you have to check out a new branch to retain any changes and commits made like so

```bash
dod@othree:~/project$ git checkout -b temp_branch_name
```

Now you can make and save changes at this state.

We'll go through `merging` these changes in the next tutorial.
Let me know if I missed anything in the comments section.

# TL;DR
- A branch is a version of your repository
- `git branch <name>` Creates a new branch with the specified name.
- `git checkout [branch|commit]` helps us to switch through branches and commits
- detached HEAD state occurs when you try to checkout something that is not a local branch. It can be a commit, a tag or a remote branch.
- I'm Awesome ...😎⁠⁠⁠⁠


