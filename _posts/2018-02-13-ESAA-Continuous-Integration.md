---
layout:     post
title:     【原创】ESAA-Continuous-Integration
subtitle:   Github
date:       2018-02-13
author:     Bob
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Git
---
# Continuous integration  
The build, test and packaging of the AM solution is fully automated thanks to Jenkins. The chocolatey packages are hosted on a Nexus (Sonatype) server and the release artifacts (other than the install package) are hosted on GitHub.  The integration relies on an oracle DB hosted on a docker container.  
Here are the urls of the **Jenkins** and **Nexus** servers:  

* [Artifact respository](https://ec4w01091.itcs.entsvcs.net/nexus) _Not yet used_
* [Jenkins Build server](http://ec4w01325.itcs.entsvcs.net:8080/view/ESAA/)
* Docker host server: ec4t01926.itcs.entsvcs.net:2375. _Not yet used_

## Infra
The continuous integration of the ESAA solution relies on GitHub, Jenkins for the first version. It has to be extended to work with Docker to test/simulate the database deployments.  
![](https://ws3.sinaimg.cn/large/006tNbRwgy1fpwa6mdzajj30r909cjrq.jpg)  
The servers are all hosted on the Entreprise Service cloud infrastructure and the scripts/cookbook used for the installations are hosted on the [infra](https://github.houston.entsvcs.net/itam/infra) repo.

## Jenkins maintenance

To Be defined

# Build Description

To Be defined

## Build process
Every commit on the branches _next_ or _master_ of the repo will trigger the execution of a Jenkins job.  

Below is the build & test process of the AM application. We should end up with the same type of flow for ESAA.  
![](https://ws4.sinaimg.cn/large/006tNbRwgy1fpwa8co0ddj30pf0jimxx.jpg)  

The test DBs can be hosted on a **docker** container which can runs a specific version of the ESAA database. For example if the next release (currently in development) is ESAA 47 then the test db should run ESAA 46. The docker container is always stopped and reloaded before each build to ensure the integration always occurs on the same version of the database This is important to reproduce an environment as close as possible to the production.  
If the installation and tests of the new package successes then the build is marked as passed. Otherwise the job is marked as failed,  and the last person who committed on GIT is noticed by email.  

Note: this applies to the current and next versions but also to the pull requests.

## Package publication and deployment to EIT100

To be defined
