---
title: What is your git merge strategy?
published: false
tags: #git #help #bestpractices
cover_image: https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2018-mm-dd-git-merge-strategy/git-flow.png
---

# Branching

Branching out is a common first step as you work on a new task. `git checkout -b 'name of your brand new branch'` should be familiar for most of you. This routine keep root branch (master/ develop whatever) clean, until someone finishes her/his work. Then you have to somehow put your changes into the root branch.
![img](https://git-scm.com/figures/18333fig0315-tn.png)

# Merge
The default strategy, for integrating changes into a root branch is merge operation. Of course, if you meet any conflicts, you have to resolve them, but finally, the code base will be integrated. 
![merge](https://git-scm.com/figures/18333fig0317-tn.png)

# Squash
There is an option while merging changes, to use `--squash`. You will end up with one merge commit, put on top of the root branch history. From my perspective, this method is really risky. When a team does not stick to the pull requests/branch naming convention, it can have dramatic consequences. Imagine going back in the repository history through commit named `update`, `align with newer version`, `test`, `more tests`, etc. This option will simplify history view, but it is only for mature teams.
![squash](https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2018-mm-dd-git-merge-strategy/squash.png)

# Mix
While I was doing a research for this article, I found [this](https://blog.carbonfive.com/2017/08/28/always-squash-and-rebase-your-git-commits/). A really nice mix of the merge and squash in a single approach. Again, here you will need a mature team, to follow the naming convention, but is a nice consensus.

# Question?
Personally, I prefer to use standard merge. I have met teams, which use a `--squash` option for all of the feature branch merge and I can see the pros of this approach. I miss detailed git history in that scenario after all. What is your default merge strategy?


Images source: [git-branching-and-merging](https://git-scm.com/book/en/v1/Git-Branching-Basic-Branching-and-Merging)