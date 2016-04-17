---
author: Mike Pruett
comments: true
date: 2005-07-07 22:08:00+00:00
layout: post
slug: veritas-lra-final
title: Veritas RedHat Linux Remote Agent – FINAL UPDATE!
tags: [storage, symantec, tape, veritas]
categories: [Storage]
share: true
---

So.. Veritas finally has the answer.  
  
It was a problem with the tape backup device that we were using. Tried to backup/restore from disk... and it worked! Now I have to work up a proposal for a new backup server for Oracle!! Whoo Hoo!!!  
  
On a lighter note, I have been reviewing the documentation for "pigsentry". Its a perl script that can be configured to send out an e-mail when certain "attacks" are logged in snort. I believe that this is the key for my IDS rebuild. Here is my software list so-far:

  

* SNORT  
  

* PigSentry - for monitoring   
  

* SnortSAM - to issue shun's to our PIX firewall   
  

* Snortalog - for reporting

  
I know most people would suggest ACID, but I find that its too slow on older machines. (PIII 800 w/ 512MB ram... how old is that!) ACID has some nice features, but I find that the mySQL interface is too slow, and really needs to be on a dual-proc machine for live environments.  
  
I will keep everyone posted!

