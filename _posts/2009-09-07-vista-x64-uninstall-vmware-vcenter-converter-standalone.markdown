---
author: Mike Pruett
comments: true
date: 2009-09-07 22:16:32+00:00
layout: post
slug: vista-x64-uninstall-vmware-vcenter-converter-standalone
title: Vista x64 Uninstall VMware vCenter Converter Standalone
tags: [vmware]
categories: [Virtualization]
share: true
---

If you are using Vista x64 and are trying to uninstall VMware vCenter Converter Standalone like I was, you may run into the following issue uninstalling...

{% highlight ruby %}
Error 1321. The Installer has insufficient privileges to modify the file
C:\ProgramData\VMware\VMWare vCenter Converter Standalone\ssl\rui.key
{% endhighlight %}

This aparently is due to a permissions issue on the following file:

{% highlight ruby %}
C:\ProgramData\VMware\VMWare vCenter Converter Standalone\ssl\rui.key
{% endhighlight %}

The minute you give yourself permissions to that file, you then can successfully uninstall it. Yay!!

Thanks to those guys at the [VMTN forums](http://communities.vmware.com/message/1344322)! You guys rock!!
