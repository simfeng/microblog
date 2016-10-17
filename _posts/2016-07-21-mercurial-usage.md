---
layout: post
title: "mercurial usage"
date: 2016-07-21 13:54:26
image: '/assets/img/'
description:
main-class: "mercurial"
color: '#EB7728'
tags:
- hg
- mercurial
categories:
twitter_text:
introduction: "introduce how to init, clone, modify, create, merge by mercurial, this just a process"
---

## Part 1.  Main Process

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

if got any question about config, try to solve it by

    $ hg help config

#### 5. Update source

Step 1: list changsets from source

    $ hg incoming [repository]

Step 2: check-in new changsets from source or other repository

    $ hg pull [repository]

Step 3: update working directory with pulled changset

    $ hg update 

#### 6. resolve conflicts

step 1: Merge change across branch and notify merge confilcts

    $ hg merge 

step 2: Try to remerge unresolved files

    $ hg resolve [-a] [files] # -a=try all unresolved

step 3: Show all branch heads or heads of speified revision

    $ hg heads [-r Rev]

#### 7. modify

now you can edit your file    

- hg copy oldfile newfile
- hg rename oldfile newfile
- hg remove files
- hg add [files]
- hg addremove    
    # automatic add and remove

#### 8. show status

step 1. show status

    $ hg status

    M 0724a
    A newfile
    R 0724b
    ? newfile_not_hg_add
    ! oldfile_not_hg_remove

|status  |  explanation|
|:--------:| :------------: |
|A | new file|
|M | modify |
|R | remove file|
|？ | new file not hg add yet |
|！| delete file not hg remove yet|

step 2. show difference

show diff or files files between changesets(default current against tip)

    $ hg diff [-r RevA [-r RevB]] [files]

#### 9. commit your change

List changesets to destination

    $ hg outgoing

create a new changeset in local repository

    $ hg commit [-A] [-u 'name <email>']
    # -A: auto addremove

#### 10. push new changeset

    $ hg push

#### 11. show commit log

    $ hg log [-r Rev[:toRev]] [-p]
    # show changesets(-p=with diff)

show the latest changset
 

    $ hg tip
    # hg log -r tip

## Part 2. Branches Manage

#### 1. New branch

create new branch with command

    $ hg branch new-branch-name
    $ hg commit -m "new branch"

push new branch
    
    $ hg push --new-branch

when use `hg push -f`, mercurial will  __inactive__ current branch and push new branch, so, recommend `hg push --new-branch`

show all branches with command 

    $ hg branches


show current branch

    $ hg branch

#### 4. switch branch

    $ hg update branchname

#### 5. merge branch

    $ hg merge branchname
    # merge in changeset from another branch

![mercurial cheetset](/assets/img/mercurial_usage/hg.jpg)


## Useful Links

[25 Tips for Intermediate Mercurial Users](http://antonym.org/2010/04/25-tips-for-intermediate-mercurial-users.html)
