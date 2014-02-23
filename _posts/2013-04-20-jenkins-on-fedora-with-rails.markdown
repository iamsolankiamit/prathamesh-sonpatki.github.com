---
layout: post
title: "jenkins on fedora with rails"
date: 2013-04-20 23:46
comments: true
tags: jenkins ci rails
---

Installing `jenkins` on `Fedora` is quite easy.

``` bash
$ sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
$ sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
$ sudo yum install jenkins
$ sudo service jenkins start/stop/restart
```
<!-- more -->


Start jenkins by

``` bash
$ sudo service jenkins start
```

You will get a success message.

``` bash
Starting Jenkins                                           [  OK  ]
```

By default jenkins starts on port 8080. Visit `http://localhost:8080`.
You will be greeted by jenkins index page.

### Setting up System

#### Set `shell` for jenkins user

``` bash
$ sudo usermod -s /bin/bash jenkins
```

#### Set `home` for jenkins user

``` bash
$ sudo usermod -m /var/lib/jenkins jenkins
```

Now we have to perform next steps as `jenkins` user.
You can give superuser permissions to `jenkins` also.

#### Install RVM

``` bash
$ sudo su - jenkins
$ curl -L https://get.rvm.io | bash -s stable --rails --autolibs=enabled
```

##### Complete script is [here](https://gist.github.com/prathamesh-sonpatki/5428304)

#### Load RVM

``` bash
$ vi /var/lib/jenkins/.bashrc
$ [ -s "/var/lib/jenkins/.rvm/scripts/rvm" ] && source "/var/lib/jenkins/.rvm/scripts/rvm" # This loads RVM into a shell session.
```
#### Configuring path

Make sure that you have configured your `bashrc` properly so that `PATH` has everything that is needed to run a Rails
project. I spent a lot of time with `javascript runtime not found`
error even if i had `nodejs` installed.
Later i realized that my `bashrc` was not containing `path` to `node executable`.

### Gitlab hooks

I am using Gitlab as the repository server.

Create new key with `ssh-keygen`.

### Gitlab plugin

#### Install

From the jenkins UI, you can install gitlab plugin. More info can be found [here](https://wiki.jenkins-ci.org/display/JENKINS/Gitlab+Hook+Plugin).

#### Build now hook

Add this web hook on your Gitlab project:

```
http://yourserver/gitlab/build_now
```

Now whenever there will be commit, gitlab will send a request to
jenkins to trigger the build.

### Build Configuration

We can run rake tasks, shell scripts once the build is triggered. This
configuration can be done on the project page on jenkins server.

I prefer running a shell script which will do `all` the steps required
to run specs. This script can be stored into version control system
like git so that it will be available to everyone in your project.

I have created a sample ci script for a Rails 3.2+ project with
PostgreSQL database
[here](https://gist.github.com/prathamesh-sonpatki/5516512).

### Post Build configuration

It allows to add email ids of project members who will get email after
every build. It generates report for last build using various plugins.

Same steps can be used with *CentOS* to setup jenkins.


Happy hacking!
