---
layout: post
title: "mercuial usage"
date: 2016-07-21 13:54:26
image: '/assets/img/'
description:
main-class:
color:
tags:
categories:
twitter_text:
introduction:
---


#### 1. Create new [Repository](http://translate.google.cn/#en/zh-CN/repository)

first way    

    $ mkdir learn_hg
    $ hg init learn_hg

second way, make above commands to one

    $ hg init learn_hg

or    
    
    $ mkdir learn_hg
    $ cd !$
    $ hg init

#### 2. Move your Repository to Server

If you use [Bitbucket](bitbucket.org) as our server    
Step 1. create a new repo on Bitbucket with any name you want, maybe `hg_learn`   
Step 2. start to push      
way 1: ssh, remember to add your public key.

        $ hg push ssh://hg@bitbucket.org/your-username/hg_learn

way 2: https, need password

        $ hg push https://your-username@bitbucket.org/your-username/hg_learn

#### 3. clone a repo

two ways

if you have set public key

    $ hg clone ssh://hg@bitbucket.org/your-username/hg_learn

else

    $ hg clone https://your-username@bitbucket.org/your-username/hg_learn

you can rename the repo's name

    $ hg clone ssh://hg@bitbucket.org/your-username/hg_learn test_repo    

#### 4. About Config
    
if the local repository is cloned from server repo, there is `hgrc` file in `.hg` directory, and `.hg` at local repo's root directory.    
if the local repo is from `hg init`, we may need to touch it.

    $ touch .hg/hgrc

then, start to edit it.

if got anyquestion about config, try to solve it by

    $ hg help config

#### 6. Update source

Step 1: list changsets from source

    $ hg incoming [repository]

Step 2: check-in new changsets from source or other repository

    $ hg pull [repository]

Step 3: update working directory with pulled changset

    $ hg update 

#### 关于hg merge的一些理解

每次修改文件都会创建一个新的head， 就算是没有commit也会出现。   
head 切换、关闭 http://stackoverflow.com/questions/3688263/mercurial-beheading-a-head

#### 5. New branch

create new branch with command

    $ hg branch new-branch-name

show all branches with command 

    $ hg branches


