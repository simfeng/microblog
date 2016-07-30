---
layout: post
title: "mercurial usage"
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

    $ hg commit [-A] [-u 'name <email>']
    # -A: auto addremove


#### 5. New branch

create new branch with command

    $ hg branch new-branch-name

show all branches with command 

    $ hg branches


#### 关于hg merge的一些理解

head 切换、关闭 http://stackoverflow.com/questions/3688263/mercurial-beheading-a-head
每次commit 之后， head的值都会变化， parent 也会变化   

    changeset:   13:c6d5da9cd061
    tag:         tip
    parent:      10:f6270b506eac
    parent:      9:6c5926c93911
    user:        Feng Zhengquan
    date:        Sun Jul 24 17:44:46 2016 +0800
    summary:     merge

如上所示， head是13， parent是10， 当我们push的时候，如果

