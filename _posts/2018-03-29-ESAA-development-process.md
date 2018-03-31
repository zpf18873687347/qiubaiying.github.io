---
layout:     post
title:     【原创】ESAA-Development-process
subtitle:   Github
date:       2018-01-29
author:     Bob
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Git
---
 How ESAA development is organized
**Golden rules:**  
* Never ever work on the __master__ branch. This branch should always reflects the code currently running on production.
* Development occurs on the __next__ branch
* Avoid committing directly on the next branch.
* Always use Pull Requests to push changes: this applies to new features (for the next release) as well as production's bug fixes. Pull Request must be reviewed by a peer.

## Branching model
Only 2 working branches exist on the repo: __master__ and __next__.  
Additional branches can be created for previous releases; We can also have feature or hotfix branches which can be used to propose a modification on one of the 2 working branches.  

## Working on a feature
### Create a feature branch from _next_  
Before starting my work I need to make sure I have the latest version of the code (from next) and create a branch to to work on isolation 

From a command prompt, enter the following commands.

```bash
developer@workstation> cd <folder containing git repository>
developer@workstation> git checkout next  
developer@workstation/next> git pull origin next  
developer@workstation/next> git checkout -b feature/aldea123
```

### Work on my feature
I code, test, commit locally (when I want to isolate a change or take a snapshot), test locally, correct my code if needed, test again, commit... until I'm satisfied with my changes.  
Git commands I need to master to achieve these steps are: `git add` `git commit` `git revert` and of course `git pull` and `git merge`.  

At this stage my local git logs will look like this in TortoiseGit:  
![](https://ws1.sinaimg.cn/large/006tNbRwgy1fptrod5zckj30oj04ydgo.jpg)  

I'm going to work on my isolated branch for a while (from 1 week to 1 month let's say) so I need to keep it in sync with the remote _next_ branch! This is well described on the GitHub help blog.  
Typically I want to use rebase command (instead of merge) to make sure my local changes are applied on top of what have been pushed by others:  
```bash
developer@workstation/feature/aldea123> git checkout next
developer@workstation/next> git pull
developer@workstation/next> git checkout feature/aldea123
developer@workstation/feature/aldea123> git rebase next
```

**Note:** If I'm working on this feature with another developer (not common but this can happen from time to time) then I need to regularly push my commits on the remote repo and pull changes done by my colleague.  

### Publish my work for review
Once I'm satisfied with my modifications I need to push my branch to the remote repo and create a Pull Request to have it reviewed and merged into next.  
1. Review my git logs and decide whether I need to rework the history using `git rebase -i`:for example I would remove useless log messages and squash all my commits into one consistent commit in order to keep the history clear and facilitate the code review. **Once my commits are pushed I can't change them.**
2. Push it to the remote
    ```bash
    developer@workstation> git push origin feature/aldea123
    ```  
3. Once the branch is pushed it will appear on GitHub. Switch to the branch you just pushed and click on the button "Compare & Pull Request".  
![](https://ws2.sinaimg.cn/large/006tNbRwgy1fptrpdcidlj30yu0b7gn3.jpg)  
GitHub propose me a default value for the base branch, I make sure it set it to _next_, then I can look at my changes (diff), enter a title and description (here I can attach the Technical requirement doc to give reviewers a bit of context) and submit the PR.  
At this point anybody can review my code, pull the branch and commit modifications (as long as they have the correct permissions).  
4. The creation of the PR will automatically trigger the jenkins job xxxxxxxx. This will not validate that the feature meets the business requirements (not yet) but will ensure my code can be built successfully.  **Note:**  This is a future state and this functionality does not currently exist.
5. If the build passed then I will notify the reviewer(s).  

Lots of documentation can be found on GitHub about how to interact with the others on the PR. There is lots of nice and fun features!

### Rework my PR
In case my changes do not pass the tests (jenkins job failed) or the techlead review I will need to rework my changes.  

```bash
developer@workstation/next> git pull
developer@workstation/next> git checkout feature/aldea123
developer@workstation/feature/aldea123> git pull --rebase
developer@workstation/feature/aldea123> git merge next   // in case there were modifications on next
developer@workstation/feature/aldea123> rework, rework, rework, build, test
developer@workstation/feature/aldea123> git add {updated_files}
developer@workstation/feature/aldea123> git commit
developer@workstation/feature/aldea123> git push
```

Anytime I push changes to the feature branch the jenkins jobs will automatically start to build and test my modifications.

### Review the change
As a techlead/reviewer I should review the modifications brought by this pull request: I can either use Github to review the commits and the file modified, or I can pull the branch on my workstation and review the changes locally.  
  
```bash
reviewer@workstation/next> git pull origin feature/aldea123
```

If changes are needed, I will ask the developer to rework the code. In this case I use the GitHub review tool to add comments directly on the modifications (similar to MS Word's comments) which facilitate the exchange with the developer(s).

### Approve and merge the PR
As a techlead/reviewer I am empowered to accept the changes and execute the merge. The only condition is that the jenkins build is successful.  Approve the pull request. 
Once I'm sure the change can be merged into _next_ branch I will click on "Squash and Merge". This is important to use this option (and not one of the other 2 options) in order to keep a clean history.  
The Jenkins job will start to build/test and package the _next_ branch which contains the modifications.  **Note:** This is future state and this functionality does not currently exist.
Now the feature branch can be deleted: it does not have to be deleted right now but will need to be removed at some point.  
At this stage the git logs in TortoiseGit will look like this:  
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fpunptjv3ej30ow057jsx.jpg)  

## Release candidate (EIT deployment) -- Future state
When SIT/UAT are ready to start (features are ready for manual tests) as a technical lead I will manually trigger the deployment to EIT100. This will create a git tag to identify the commit at the origin of the last build.  
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fpunrogpaej30o604zaaw.jpg)  
There will likely be several release candidates deployed to EIT resulting in several rc tags being created in Git.  

## Fixing a production bug
The process to work on a hotfix is similar to the feature implementation described above. The only difference is the branch is created from master.  
`developer@workstation> git checkout master`  
`developer@workstation/next> git pull`  
`developer@workstation/next> git checkout -b hotfix/IM123`  
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fpunsu91mnj30o2067my9.jpg)  

At the end of the process, make sure to retrieve the hotfix from master and merge it in to next.  
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fpuntq9sd6j30o1071mya.jpg)

## Releasing code to production
Releasing the code to production means 2 things on the git side:
1. Create a tag based on the version number
2. Merge next into master  

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fpunuumbewj30o507tjsk.jpg)   
