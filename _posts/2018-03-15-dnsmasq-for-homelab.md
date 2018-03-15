---
author: Mike Pruett
comments: true
date: 2018-03-15 13:23:01+00:00
layout: post
slug: dnsmasq-for-homelab
title: Using dnsmasq for your Homelab
tags: [dns, homeserver, homelab]
categories: [Linux]
share: true
---

# Using dnsmasq for your Homelab

Like many of you, I run a homelab for a number of reasons. Not the least of which is for the following:

- Testing Configurations of Virtual Hypervisors
- Testing Software and Application Configurations
- A File Server for the Home
- Media Tools and Storage

Mostly, my lab is for the latter... as I host a 22TB Local Media server in my Homelab. The Media is being hosted using Plex, my all-time favorite media server software. Plex takes my library, and allows me to be able to view it from anywhere!

I also run a Pre-Execution Environment (PXE, or iPXE to be exact) for all of my Imaging needs. This has been running on a old Dell Optiplex 755 which I nabbed from work a few years ago. The Dell is not really powerful enough to run as a workstation, but IS powerful enough to host my Images, and my Nginx Reverse-Proxy (all this on only 2GB of ram!) ...but I digress

For the past few years, I have been running the bog-standard ISC DHCP server for my PXE needs. It runs really well on the old Optiplex, and has served me well. Up until recently...

## Enter Pi-Hole

Beginning this month, I began using [Pi-Hole](https://pi-hole.net/)! This software (incase you are unaware), is an Ad-Blocker for your entire network. The project is very slick, and definitely works. The main component of Pi-Hole is to run a DNS server in your environment, which will block access to known advertizement servers that are queried from hosts on your network.

In order to accomplish all of this on your local environment, Pi-Hole ships with a configurable DNS & DHCP server called dnsmasq. This is really the first time I had used dnsmasq, and I was pretty impressed. Before this, I was only running DHCP internally... and forwarding all DNS requests to Google (8.8.8.8, 8.8.4.4.). By default, Pi-Hole disables dnsmasq's built-in DHCP server (thank you!), to not conflict with any existing DHCP servers on your network. Just change your existing DHCP server to hand out the IP address of your Pi-Hole server as the DNS server for your Pool. Fantastic!

## Can't leave well enough alone!

At this point, I would have been fine to leave everything as is. I was blocking Ad's across my network, and was still able to use the ISC DHCP server for all of my iPXE needs. But I wanted just a little bit more!

I really wanted to be able to automagically map hosts with active DHCP leases into a locally resolvable domain on my home network. With the current setup, this was not achievable. dnsmasq was unable to update a local zone with leases that it did not create.

So... I know what I have to do! I have to replace ISC DHCP, with the DHCP server in dnsmasq. (easier said than done) I stopped the ISC service, and enabled the DHCP server in dnsmasq (Easily done on the PI-Hole admin page!) Everything seemed to be working just fine.

Little did I know that I just broke one massive thing!

## Getting iPXE to work on dnsmasq

Yep, all was good except for the one thing I needed ISC for in the first place! I started changing a whole bunch of settings in dnsmasq, with no success.

Using a VM to test, I could see PXE starting... and then fail almost immediately.

dnsmasq uses a standard Linux services configuration setup, with a base configuration file **/etc/dnsmasq.conf**, and a **/etc/dnsmasq.d** directory, were you can write your custom configurations so they do not get overwritten by a software upgrade. (Modifying the existing configuration is a no-no!)

Pi-Hole creates two new files in the **/etc/dnsmasq.d** directory, once you have configured both the DNS & DHCP services.

```
:/etc/dnsmasq.d$ ll
total 16K
-rw-r--r-- 1 root root 1.5K Mar 12 23:25 01-pihole.conf
-rw-r--r-- 1 root root  483 Mar 12 23:25 02-pihole-dhcp.conf
```

In this directory, we are going to create a new file, called **05-ipxe.conf** for all of our iPXE changes.

Ultimately, the issue was with handing out the **undionly.kpxe** file on netboot. By default, you only want to hand this file out once... and then once the iPXE client boots then hand out the correct ipxe file. This is called Chain-Loading, and its how you get iPXE to load without chaning the boot rom of each network card. (You can read more about this on the [iPXE](https://ipxe.org/) site.)

Normally, using ISC DHCP this is how you would accomplish this...

```
 if exists user-class and option user-class = "iPXE" {
      filename "http://my.web.server/real_boot_script.php";
  } else {
      filename "undionly.kpxe";
  }
```

Obviously this does not work with dnsmasq, as the configuration logic is different. Here is what I found on the iPXE forums that should accomplish this same thing.

```
enable-tftp
tftp-root=/var/lib/tftpboot

dhcp-match=IPXEBOOT,175
dhcp-option=175,8:1:1
dhcp-boot=tag:#IPXEBOOT,undionly.kpxe
```

Now this does work to somewhat break the boot loop, but it does not take into effect loading the existing menu's or bootstrap iPXE configurations you may already have. How did I fix this?

## My Configuration after several hours of RTFM!

Lets take a look at the configuration that I ultimately settled on...

*/etc/dnsmasq.d/05-ipxe.cfg*

```
# Enable dnsmasq-tftp and enable security
enable-tftp
tftp-secure

# Point to correct tftpboot directory (default is /var/lib/tftpboot)
tftp-root=/var/lib/tftpboot

# Sets a tag for the appropriate bootloader
dhcp-match=set:efi-x86_64,option:client-arch,7
dhcp-match=set:efi-x86_64,option:client-arch,9
dhcp-match=set:efi-x86,option:client-arch,6
dhcp-match=set:bios,option:client-arch,0

# Checks for iPXE Client Option 175
dhcp-match=set:ipxe,175

# Create a custom tag if BIOS and not iPXE
tag-if=set:ipxebios,tag:!ipxe,tag:bios

# Boot iPXE.efi if not BIOS and not iPXE
dhcp-boot=tag:!ipxe,ipxe.efi

# Boot undionly.kpxe if custom tag matches (BIOS and not iPXE)
dhcp-boot=tag:ipxebios,undionly.kpxe

# Everything else, boot boot.ipxe... which chain loads the regular boot iPXE menu
dhcp-boot="boot.ipxe"
```

The first few lines enable dnsmasq's built-in TFTP server, and sets the tftp-secure. (Kind of an oxymoron!) The next option points to your TFTPBOOT directory.

The next set of configurations set tags to match what type of PXE session is being run. If your host is connecting via EFI (either 64 or 32 bit), or BIOS boot (original PXE)... one of these "tags" is being populated.

The next config line checks for the DHCP Option 175, and sets the ipxe tag. (Similar to the **user-class** and **option user-class** from ISC)

Now we are creating a new tag, called **ipxebios** which is set only if the **ipxe** tag is empty, and the **bios** tag is populated. Believe it or not, this **tag-if** took me the most time to wrap my head around...

Next, we are booting the ipxe.efi boot rom for any system that does not have the **ipxe* tag (and not **bios** boot). This works for the hosts using EFI boot, instead of old PXE boot.

Now we are loading the **undionly.kpxe** rom for those systems which did not match any of the previous tag matching. (This breaks the infinite PXE boot loop!)

Finally, we are loading a **boot.ipxe** script, which chain loads my existing ipxe boot menu scripts from HTTP

*/var/lib/tftpboot/boot.ipxe*

```
#!ipxe

chain http://<my-ipxe-server>/bootmenu.ipxe
exit
```

And thats really it! Now my dnsmasq server operates in the same fashion as the ISC DHCP server did. I was also able to remove the tftpd-hba service as well.

## My iPXE Configs

Just for fun, I have also included all of my iPXE configs.

*/var/www/html/bootmenu.ipxe*

```
#!ipxe

# Global variables used by all other iPXE scripts
set protocol http
set server-ip 10.10.10.41
set boot-dir boot
set iso-dir iso
set sources-dir sources
set boot-url ${protocol}://${server-ip}
set menu-url ${boot-url}/menu.ipxe

# Figure out if client is 64-bit capable
cpuid --ext 29 && set arch x64 || set arch x86
cpuid --ext 29 && set archl amd64 || set archl i386

# Installer/ISO Versions
set memtest-ver 5.01
set memtest-image memtest86+-${memtest-ver}.iso
set spinrite-ver 6.0
set spinrite-image spinrite-${spinrite-ver}.iso
set sysrescue-ver 5.2.0
set sysrescue-image systemrescuecd-x86-${sysrescue-ver}.iso
set hdt-ver 0.3.6
set hdt-image hdt-${hdt-ver}.iso
set clonez-ver 2.5.2-31
set clonez-image clonezilla-live-${clonez-ver}-${archl}.iso
set dban-ver 2.3.0
set dban-image dban-${dban-ver}_i586.iso
set breakin-ver 4.26.1-53
set breakin-image breakin-${breakin-ver}.iso
set gparted-ver 0.30.0-1
set gparted-image gparted-live-${gparted-ver}-${archl}.iso
set ntpass-ver 140201
set ntpass-image cd${ntpass-ver}.iso

# Boot <boot-url>/menu.ipxe script if all other options have been exhausted
chain --autofree ${menu-url} ||
```

*/var/www/html/menu.ipxe*

```
#!ipxe

# Variables are specified in boot.ipxe.cfg

# Some menu defaults
set menu-timeout 5000
set submenu-timeout ${menu-timeout}
isset ${menu-default} || set menu-default exit

###################### MAIN MENU ####################################

:start
menu iPXE boot menu for ${initiator-iqn}
item --gap --             ------------------------- Operating systems ------------------------------
item --key 1 menu-instwin Install Windows
item --gap --             ------------------------- Tools and utilities ----------------------------
item --key r menu-recovery Recovery tools...
item --key d menu-diag    Diagnostics tools...
#item --key p pxelinux     Load PXELinux menu
item --gap --             ------------------------- Advanced options -------------------------------
item --key c config       Configure settings
item shell                Drop to iPXE shell
item reboot               Reboot computer
item
item --key x exit         Exit iPXE and continue BIOS boot
choose --timeout ${menu-timeout} --default ${menu-default} selected || goto cancel
set menu-timeout 0
goto ${selected}

:cancel
echo You cancelled the menu, dropping you to a shell

:shell
echo Type 'exit' to get the back to the menu
shell
set menu-timeout 0
set submenu-timeout 0
goto start

:failed
echo Booting failed, dropping to shell
goto shell

:reboot
reboot

:exit
exit

:config
config
goto start

:back
set submenu-timeout 0
clear submenu-default
goto start

:pxelinux
set 210:string tftp://${next-server}/
chain ${210:string}pxelinux.0 || goto failed
goto start

###################### INSTALL WINDOWS MENU ################################
:menu-instwin
menu Install Windows
item windows10       Install Windows 10 via PXE
item win10iso        Install Windows 10 via ISO
item
item --key 0x08 back Back to top menu...
iseq ${menu-default} menu-instwin && isset ${submenu-default} && goto menu-instwin-timed ||
choose selected && goto ${selected} || goto start
:menu-instwin-timed
choose --timeout ${submenu-timeout} --default ${submenu-default} selected && goto ${selected} || goto start

:windows10
echo Booting Windows 10 Installer from Boot Server - ${boot-server}
kernel ${boot-dir}/wimboot
initrd ${boot-dir}/bcd                     BCD
initrd ${boot-dir}/boot.sdi                boot.sdi
initrd ${sources-dir}/win10.${arch}.boot.wim   boot.wim
boot
goto start

:win10iso
echo Booting Windows 10 ISO Installer
initrd ${boot-url}/${iso-dir}/WIN10${arch}.iso
chain ${boot-url}/${boot-dir}/memdisk iso raw || goto failed
goto start

###################### RECOVERY MENU ################################

:menu-recovery
menu Recovery tools
item sysrcd          SystemRescueCD v${sysrescue-ver}
item spinrite        SpinRite v${spinrite-ver}
item clonezilla      CloneZilla v${clonez-ver}
item ntpass          NT Offile Password Recovery v${ntpass-ver}
item
item --key 0x08 back Back to top menu...
iseq ${menu-default} menu-recovery && isset ${submenu-default} && goto menu-recovery-timed ||
choose selected && goto ${selected} || goto start
:menu-recovery-timed
choose --timeout ${submenu-timeout} --default ${submenu-default} selected && goto ${selected} || goto start

:spinrite
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${spinrite-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

:sysrcd
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${sysrescue-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

:clonezilla
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${clonez-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

:ntpass
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${ntpass-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

###################### DIAGNOSTICS MENU #############################

:menu-diag
menu Diagnostic tools
item gparted       GParted v${gparted-ver}
item hdt           Hardware Detection Tool v${hdt-ver}
item memtest       Memtest86+ v${memtest-ver}
item breakin       Breakin v${breakin-ver}
item dban          Darik's Boot and Nuke v${dban-ver}
item
item --key 0x08 back Back to top menu...
iseq ${menu-default} menu-diag && isset ${submenu-default} && goto menu-diag-timed ||
choose selected && goto ${selected} || goto start
:menu-diag-timed
choose --timeout ${submenu-timeout} --default ${submenu-default} selected && goto ${selected} || goto start

:hdt
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${hdt-image} || read void
imgargs memdisk iso raw || read void
boot || read void
goto start

:memtest
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${memtest-image} || read void
imgargs memdisk iso raw || read void
boot || read void
goto start

:breakin
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${breakin-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

:dban
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${dban-image} || read void
#imgargs memdisk iso raw || read void
imgargs memdisk iso raw DBAN.BZI nuke="dwipe --method gutmann --rounds 2 --verify last" silent vga=785
boot || goto failed
goto start

:gparted
kernel ${boot-url}/${boot-dir}/memdisk || read void
initrd ${boot-url}/${iso-dir}/${gparted-image} || read void
imgargs memdisk iso raw || read void
boot || goto failed
goto start

```

There is a lot going on in this boot menu, most of which does not work over EFI! I leave EFI boot on for testing, but most of my hosts still use good old BIOS PXE boot. It just works.

I hope to have EFI fully fleshed out soon enough, but the fact that memdisk does not function in the EFI shell... that really breaks most of my menu items!

## Conclusions

There is a lot of ideas and concepts being thrown around in this post, and I fully expect that this post is really only usefull for a mere tenth of you reading it.

If anyone has any questions about this setup, please let me know. If you’d like to send me an e-mail, you can reach me at mpruett at nixdevil dot com. Also on Twitter, and Reddit (/u/torafuma).