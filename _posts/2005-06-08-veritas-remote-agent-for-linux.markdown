---
author: Mike Pruett
comments: true
date: 2005-06-08 22:59:00+00:00
layout: post
slug: veritas-remote-agent-for-linux
title: Veritas Remote Agent for Linux
tags: [storage, symantec, tape, veritas]
categories: [Storage]
share: true
---

One of my projects this year is to find a new backup solution for our Oracle servers.

We currently use a software piece called "Lone Tar". Great for backing up a single server, back in 1993... but with todays larger tape capacities, and the advent of tape libraries LT just doesn't cut the mustard!!

Currently, I am using a set of BASH shell scripts and CRONTAB to schedule this whole ordeal. Concidering I have to bring the Oracle DB & APPS offline inorder to backup, Full backups take close to 7 hours. (Just to backup, not even Verivy.) In fact, the Verification process failes each Saturday, as CRONTAB has brought the DB backup by the time it starts. Can you say...FRUSTRATING!!!

Hence the quest to find new "Linux Friendly" backup software. I have already looked at Legato, CA, and Veritas. Legato is WAY to expensive for 4 servers, and CA is just crap. (Never really liked CA software to begin with.) I had thought of using some Open Source backup software like Backula or amanda... but I have not been able to find a single person on the net who has a working solution using our hardware. (Servers - HP Proliant DL380's , Tape Library - HP SSL1016)

When we implemented Oracle on RHEL3 over a year ago, there really wasn't any commerical backup software that would support / work with RHEL. I tried to use ARKEIA early on... (Having heard so many great things about it.) , but it did not cut the mustard. (Didn't even recognize our Tape Library.)  What to do?!?!?!

As our current IT Backup Software standard is Veritas BackupExec on the windows/netware side, I was hopefull of a offering from them. In 2004, the only "Linux Friendly" app they had was "Netbackup 5". Which would have set me back $$20K. Less than Legato, still more than I wanted to pay to backup 4 machines.

When Veritas released its version 10 of BackupExec earlier this year, I had hope again!

Veritas had a Remote Agent for Linux, that was Certified to work with RHEL3 & 4. (We might upgrade you know.) After upgrading from 9.1 to 10... it has been my project to test the agents to make sure that they actually will work!! WOO-HOO!!!

I know what your thinking... now I will be using a Windows box to backup my Linux servers! As outreagous as it sounds, it will work for us. Since I allready have a Win2K server that could be sacrificed to the cause, it would actually be cheaper to use this than Legato!

So now begins the testing. I have allready re-formatted my test server, and loaded RHEL3. Stay tuned to see if it actually works!!!
