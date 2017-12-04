---
title: Resolve common branch conflict
published: true
description: git branching
tags: #git #discuss
---

#Yes, I use git. Yes, I enjoy it. And no, I am not infallible.

I have been using git for 3-4 years now - since the beginning of my software developer’s journey. At first I was overwhelmed by large amount of commands and I couldn’t understand how all of these distributed branches/repositories worked together. Over time, I was getting deeper and deeper into knowing git better. Only now, I am ready to work with it, as a first-choice version control system tool. Still - I don't think that I know git as much as I want and need to, but I am learning new things every day.

At my current work we are using a most common (I think) branching convention:

* **Dev**
* **Test**
* **Stage**
* **Prod**

**Dev** branch is sort of a playground/sandbox for developers. A superior and more stable is a **Test** for testers - trying to break our code and raise first bugs/issues. **Stage** is for testing again, but this time by our end-client's Quality-Assurance team. Finally **Prod** is THE prod :)

#Problem 

When we find a conflict/problem with *merging*, it is not a big deal on our feature branch. One way we can *stash* our work and apply it on clear new branch, solve this problem by *pulling* changes from **dev** branch (and resolve conflicts on local branch) or take any other wonderful way/tool to get our code into **dev** branch. By default, we create a policies to protect branches like (**stage**, **test**, **prod**) from direct commits. The real problem is when conflict occurs on superior branch. Because of our default policies, we cannot *push* changes directly, but somehow we find ourselves in a place with conflict again. Let me outline the situation better.

One sunny day, we had to *push* our *hotfix* to **stage** for our client’s QA team. Developer who took this on her/his shoulders, *merged* the whole **test** branch into mentioned **stage**. She/he didn't realize that on inferior (test) branch were already some features that shouldn't be *pushed* into **stage** yet. So what unfortunate developer did? Do you know what command *revert* is? She/he knew it. After some commits with new features had been reverted, everything went well. Client got only needed changes, we did not expose new/unready features. Everyone was happy. Yhmm... no, not at all. Some time later, when our testers said that new fresh features were ready to be *pushed* into **stage** branch, we met a problem. Why? Because, last sunny day some commits were made on **stage** branch directly. None knew that, except one developer. What was the problem, you say? Because of the *revert*-commit, we already had in our git commits history, we could not deliver changes (from **test** branch) to our client. 

#Solution

So how did the git behave? *Merge* branches with no changes?! Finally, how to fix this situation? There was a way, but we needed to know how does the git work. Git compares the history of two branches and applies new commits which are not *merged* yet. Conflicts occur, when branches have different commits. History starts to dump. But we had a perfectly clear history with all commits covered from **test** on **stage**. This made me thinking about history. I reviewed all commits, found unlucky revert and then finally, we could start working on that. I *checkouted* new branch based on **stage**, did a *revert* to *revert*-commit, *pulled* changes from inferior branch and created a *pull request* for that. And it worked 🙂

#Final thoughts

After that, I tried to remember to:
* not to *squash* commits (at first try, I had squashed them, and it did not work), 
* create fix branch from superior, 
* pull changes from conflicted inferior branch, 
* resolve conflicts on your local machine, 
* then merge (create a pull request) into superior branch 
and … voila!

Did you meet any difficulties when using git? Please share it with me here.

This article was created after reading [Is git the best vcs?](https://dev.to/ben/is-git-the-be-all-and-end-all-of-version-control-4lp) Also helpful were all of tips received after my last article [Dealing with evenings bursts of creativity](https://dev.to/meanin/how-to-deal-with-evenings-bursts-of-creativity-pc).