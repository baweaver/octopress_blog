---
layout: post
title: "Setting up Rails in Debian 7"
date: 2013-10-02 22:15
comments: true
categories: [Rails, Debian, Bootstrap]
---

In this tutorial we'll cover the entire process of setting up a basic
Rails environment on a clean install of Debian 7.1.

<!-- more -->

## What am I making?

You will be making a Debian 7.1 Box with Ruby 2.0, Rails 4.0, and Git.
At the time of this writing, these are the most recent versions.

## Virtual Box

The first thing we're going to need is Virtual Box. Feel free to set
this up as a standalone OS, the process will essentially be the same.

In my case, we just need:

``` bash
sudo apt-get install virtualbox
```

## Getting Debian

http://www.debian.org/

## Installing Debian

Unless otherwise noted, specify the default options on your install. I
will note the steps as I go along installing Debian 7.1 i386 on an
instance of Virtual Box with a Host OS of Linux Mint 14 x64. The steps
should not differ heavily with other Host OS platforms.

### Hostname and Domainname

Your host and domain names are completely up to you, but if this is just
a test I would suggest leaving them as the defaults for the time being.
They can be changed later on.

### User Accounts

The same will apply to the passwords and the other information
used for account setups. At this point on a test box I specify a trivial
password and other information, considering I'm installing on a VM that
will not see the light of day. I don't advocate doing such things on a
live server, the Ops will hit you or do nasty things to your home
directory if you do.

### Partitioning

Select Guided for the partitioning method, unless you're feeling brave
or know your way around Unix. This will be explained in detail in a
later tutorial, but for now it will be fine to accept the defaults.

As it will tell you, select all on same partition. There are quite a few
benefits towards seperate partitions, but if this is a test box or a VM
it will be irrelevant for now.

Finish the partitioning and write the changes onto the disk.

### Base System Install

After this point, the base system will begin to install. Now would be an
ideal time for coffee or other niceties you may desire as it will take
about 5-10 mintues to complete.

### Configuring Apt

This is another instance of selecting defaults unless you have
compelling reason not to. Chances are low that you will, and HTTP
proxies will be rare in most cases considering you'd be routing through
your Host's NIC.

### Select and Install Software

Now would be another great time to catch a break, as it's going to be
downloading a fair amount of packages from the package server. Make sure
to watch for the popularity contest prompt. Feel free to select as you
wish.

On the packages list, you can deselect using the space bar. ONLY select
SSH Server and Standard System Utilities. We want to keep this
lightweight for tests. In the case of a server, DO NOT select a Desktop
environment. Put simply, you're doing yourself a disservice as most
commercial servers will be running headless as is. Press Enter to
continue, and it will continue to retrieve the requested files.

Now that we're here, it's time to boot into our new system!

## Getting Rails Set Up

Go ahead and log into our test account. Now the first thing we're going
to want to get a hold of are a few programs:

``` bash Programs
sudo apt-get install git zsh vim
```

ZSH and Vim being preference, but will save you some headaches later on
down the road. Git is by far manditory for any form of Rails
Development. Learn Version Control, it will save you countless hours
later on.

Next we're going to want to get a hold of RVM, Ruby Version Manager, to
handle various Ruby installations.

``` bash RVM
curl -L https://get.rvm.io | bash
source /etc/profile.d/rvm.sh

rvm install 2.0
```

Notice the source command, you won't be getting very far without it.
This will take some time as it's building Ruby from source. Now to get
Rails running for us.

``` bash Rails Install
gem install rails -v 4.0 --no-rdoc --no-ri
```

We're explicitly leaving off the documentation, as it takes
substantially longer to compile. The thought behind this is that you
should have a hold of the great Obie Fernandez's [The Rails 4 Way](https://leanpub.com/tr4w) sitting on your
desk. No? Purchase it. I'll wait, and you have plenty of time before
Rails installs as well.

## Testing it out

Now we'll get a skeleton app up to demonstrate that we have everything
working. Make a directory for tests, and run

``` bash
rails new test-app
```

You should see a lot of code flash by, and a hang at bundle install.
This is retrieving all the extra libraries for Rails to get running.

I will warn you there's a potentially nasty bug lurking here, in that a
javascript environment will need to be installed, run the rollowing
commands:

``` bash NodeJS Install
sudo apt-get update
sudo apt-get install python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update

sudo apt-get install nodejs
```

After this, go ahead and give it a shot and watch it come to life!

``` bash Rails Server
rails s
```

Running into problems? Shoot me a tweet @keystonelemur and I'll add it to a footer
section of problems encountered and we'll get it all sorted out!
