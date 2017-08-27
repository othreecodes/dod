---
layout: post
title: It's Git, Init? (2)
excerpt: "How do you collaborate, or work on single projects? Here's a guide to working efficiently with git"
categories: articles
tags: [git, version-control, development, gitlab, github, branching]
comments: true
share: true
# modified: 2017-08-26 19:24:54.454961
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

The master is the trunk, of which you can branch off from. You start a new branch off of the original master so that you can make changes to the project without affecting the original master branch