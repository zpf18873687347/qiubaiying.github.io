---
layout:     post
title:      【原创】Setting-ESAA-DEV-environment
subtitle:   Github
date:       2018-02-03
author:     Bob
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Git
---

# How to Setup ESAA development environment


## Install and Configure Git on local machine
1) Log into GitHub Enteprise.  There will be a sign-on link on the ESAA wiki home page
1) In order to use the Git commands you will need to first install Git on your local machine
Obtain the latest version of Git --> https://git-scm.com/download/win
When installing, use the default values specified by the install package with the exception of the default editor.  There you will want to select "Use Notepad ++ as Git's default editor".  See screen shot below.

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fpuo1ks54aj30dz0ap3zy.jpg)  

   
2) Once Git has been installed on your local machine specifiy your identity in the git config file.

    **Your Identity**


    The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating.  Enter the following commands from a command promt (i.e. Git CMD, Windows PowerShell).

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fpuo3lbnh8j30i1049aa5.jpg) 


```bash
> git config --global user.name "John Doe"
> git config --global user.email johndoe@example.com
```
Again, you need to do this only once if you pass the --global option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the --global option when you’re in that project.


Many of the GUI tools will help you do this when you first run them.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   **Checking Your Settings**


If you want to check your configuration settings, you can use the git config --list command to list all the settings Git can find at that point:

```bash
> git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
```
<BR>

## Establishing SSH Authentication
1) Establish SSH authentication method 
Following the instructions in the following URL --> https://help.github.com/enterprise/2.11/user/articles/connecting-to-github-with-ssh/

<BR>

## Cloning a Repository
The next step is to clone the repository that you want to contribute to.  Within ESAA there are 3 key Repo's.  They are as follows:
1) esaa-db
2) esaa-ui
3) esaa-tools

There is a fourth respository that you may want to clone and that is esaa-historical which has files that have been kept for reference purposes.

Refer to the following readme file for the list of TFS directories that were migrated to the respective Repo's -- https://github.houston.entsvcs.net/itam/esaa-migrate/blob/master/README.md

It's recommended that a seperate high level folder be created and that you clone the esaa repositories in this folder.  
For example:
1) Create a folder titled "Git Repositories"
2) Change directory to this folder
3) Run the git clone command from this folder
### Cloning the esaa-db Repo
If you plan on making changes to the files contained in the esaa-db respository, then enter the following command.
```bash
> git clone git@github.houston.entsvcs.net:itam/esaa-db.git 
```
When you run the git clone command it will create a esaa-db folder in the current directory.


### Cloning the esaa-ui Repo
If you plan on making changes to the files contained in the esaa-ui respository, then enter the following command.
```bash
> git clone git@github.houston.entsvcs.net:itam/esaa-ui.git 
```
When you run the git clone command it will create a esaa-ui folder in the current directory.

### Cloning the esaa-tools Repo
If you plan on making changes to the files contained in the esaa-tools respository, then enter the following command.
```bash
> git clone git@github.houston.entsvcs.net:itam/esaa-tools.git  
```
When you run the git clone command it will create a esaa-tools folder in the current directory.

### Cloning the esaa-historical Repo
**No** changes should be made to any of the files in the esaa-historical Repo.  These are for veiwing only.
```bash
> git clone git@github.houston.entsvcs.net:itam/esaa-historical.git  
```
When you run the git clone command it will create a esaa-historical folder in the current directory.


<BR>

## Setting the Origin Value
Set the **origin** value for the repo to work with ssh authentication.

Before running the command shown below, change the directory to the folder for that repo (eg. cd esaa-db)

Run the command to see how your remote is set.
```bash
> git remote -v 
```
If it doesn't show  git@github.houston.entsvcs.net:itam/xxx (where xxx is the repo name), you will need to enter the following command:

```bash
> git remote set-url origin git@github.houston.entsvcs.net:itam/xxx 
```
(where xxxx is the repo name -- for example the esaa-db repo would be entered as --> esaa-db.git)

Confirm that the remote has been changed:
```bash
> git remote -v
```
Should show the value listed above

At this point you are ready to start making changes.  Refer to the [ESAA Development Process](ESAA-development-process).

<BR>

## Installing TortiseGit
TortiseGit is a graphical user interface to more easily see differences between your local copy of a program and the program that is maintained in GitHub.

To install follow the instructions below.
1) Download using the following link --> https://tortoisegit.org/download/
2) On the first prompt select OpenSSH, Git default SSH client 
3) For all other prompts select the default options.

Once installed you can go to your files that are in your local git folders and right  mouse click and you will see the TortiseGit as an option available to select.
It should be noted that once TortiseGit is installed that your One Drive Sync icons may not display the green check marks as there could be a registry conflict with the green check marks from TortiseGit.
See following article -- https://www.pcworld.com/article/3098314/data-center-cloud/the-easiest-fix-for-missing-green-check-marks-in-onedrive.html   
