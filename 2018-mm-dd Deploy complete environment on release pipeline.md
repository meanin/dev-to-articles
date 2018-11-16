---
title: Deploy complete environmnent
published: false
description: 
tags: #devops #azure #dotnetcore
---

# Introduction

I hope that everyone knows about CI/CD processes. Continuous integration helps teams work in parallel on the same project. Most common scenario is to use some git server and integrate work as a pull request from a feature branch to development one. Many tools provide easy way to build an artifact (installation package) from a merged code base. Continuous deployment is a way to maintain application state with newest code base on the desired environment. 

Imagine that before pushing new changes to production, you want to make all of the tests on a dedicated environment. Or maybe on every newly created environment you want to seed some data? Sometimes on this specific environment data stored/missing can introduce unecessary bugs or unwanted situations which require you attention, before it can be considered as tested/usable.

# Idea

Now I came with an idea to create a brand new environment, just for testing purpose. 

![img](https://i.chzbgr.com/full/7957231104/h7D6B95AD/)
###### Source: https://cheezburger.com/7957231104/that-damned-apple
In that way, you can always be sure, that there is no old data, but only data that is required to system work in the right way. Nothing will interfere with test cases, no blocker on a fresh start. 

I am using Microsoft Azure Cloud, as it is most familiar for me. In the simplest case, I want to create a storage (azure table storage), pass its connection string to the key vault, seed data there and finally deploy application which will consume the data.

# Solution

### 1. Create storage account
There are few ways to create a storage account on Microsoft Azure Cloud.