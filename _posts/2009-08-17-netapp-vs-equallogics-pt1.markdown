---
author: Mike Pruett
comments: true
date: 2009-08-17 00:24:09+00:00
layout: post
slug: netapp-vs-equallogics-pt1
title: NetApp vs Equallogics - Part I
tags: [netapp, storage]
categories: [Storage]
share: true
---

A Twitter follower ([@paulstringy](http://twitter.com/Paulstringy)) prompted me to write this post. Based on a conversation about the differences between NetApp & Equallogic appliances. Truth be told, there is quite a lot to separate the two. I don't even consider them to be in the same class.

<!-- more -->

Equallogic has been around for about 6 or 7 years now, and has penetrated the SMB market with their PS series arrays. There offerings were tantalizing because of the feature set that is offered. These features separated the units from other products in that space. The PS series offers features like Snapshots, Mirroring, and Replication. When compaired to HP's MSA series, or offerings from Quantum there is a clear distinction as to whom the winner is.

I first heard about EQL about 5 years ago. At that time we were looking for a alternative to purchasing a costly EMC array. Something to provide a robust feature set, different from the MSA1000's that we already had. There were several companies on my short list at the time, but the top two spots were EQL, and Intransa Storage.

Intransa had a similar product to the PS line, but did not offer any Replication features at the time. As I am allways looking for ways to make are data more secure, the choice was pretty obvious. The budget was the next thing to consider. A typical Enterprise SAN normally has a typical Enterprise budget to go along with it. Both the EQL & Intransa units seemed to be at the same price point (under $60k).

A few reference calls later, and we purchased/implemented the PS array in November of 2005. We did have to make some slight alterations to our environment in order for it to work. Previous to this point, our Ethernet Network consisted of 100mbps interconnects. This was OK for the average server. We knew that we wanted to go to Gig someday, but were forced by the demand of iSCSI.

Our MSA1000 was interconnected on a 1Gbps Fiber Channel fabric. My engineers & I understood FC Fabrics. We didn't forsee the need to build a seperate Ethernet Fabric. So we implemented on the existing 100Mbps network (which housed our Users & Server traffic), and failed miserably. We soon realized our mistake, and the spent an additional $45k on a network upgrade (which was not budgetted for, of course!).

Then we were expecting great performance from the array, but our hopes were sqashed again! This time we had listened to our sales rep (Bad Idea!) He had sold us on the idea that the inital ROI for any iSCSI deployment was the fact that there were was no need for costly Host Bus Adapters. Generally in a Fiber Channel fabric, each server requires a separate HBA. This HBA then interconnects the server to the Storage Fabric. The result is that you have a separate, high-speed storage fabric network... which is only dealing with storage related traffic. With our Ethernet Network upgraded to support a separate Ethernet Storage Fabric, we should have no need for a HBA. (or so we were told!)

The problem with that last statement is that its a iSCSI myth! However you would not believe how many Sales Reps had tried to feed me that line of BS! Without some sort of dedicated processor for storage networking, the server's CPU's are then regulated to provide those processor cycles. This can be a problem if your servers were not ordered with additional processor capacity to support that load. In order to do things right, you need to invest in a Ethernet Network Adapter which supports dedicated processors to offload the TCP/IP overhead generally associated with that traffic. These are typically called ToE cards, and can be as pricey as Fiber Channel HBA's.

The other problem with this equation... even if you are using ToE cards in every server, your still transferring data as fast as that medium will travel. GigE still only runs at a theoretical 1Gbps. Most newer FC fabrics support speeds of 2, 4, 6, and even 8 Gbps. The only way that iSCSI would pay off right now... would be if your infrastructure could incur the cost of a 10Gbps upgrade. Each server would need to be re-wired to support 10Gbps, and a specialized HBA would need to be installed. At this point, FC Fabrics end up being cheaper than iSCSI. (Just by Infrastructure cost alone!)

Getting back to our example... We added the ToE cards to each server and were still unsatisfied with the performance! Thought the process of elimination, we determined the storage bottleneck boiled down to the limitations of iSCSI on a 1Gbps fabric. Due to budget limitations we were unable & unwilling to bite off the cost of upgrading to 10Gbps network. At the time (2006), a small 10Gbps 24 port switch would have set us back an additional $75k. The HBA's would have been at least $1,500 a piece. At that point my distaste with iSCSI could go on no longer. I made the mandate to my engineers... NO MORE iSCSI!!! This was after 2 and a half years of no improvement on performance. In fact, the Equallogic's array was no faster than our existing MSA1000.

Unfortunately, little did I know that we were unable to acquire a new SAN until summer of 2008. By this point, I am sure that everyone in my group was tired of hearing me complain. Even my boss, and his boss was fed up with my whining! As my employer began out Green IT Initiative, a new SAN was included as apart of my budget for that year. I was overjoyed! A bit apprehensive, I began again compiling my shortlist. This time my goal was to acquire a Enterprise Rated SAN, that would allow us to grow with a minimal purchase upfront. My top two this time included Pillar Data, Xiotech, and EMC.

Pillar had a promising product, but this was the company loosly founded by Larry Ellison (Oracle Fame). There RFP was extremely promising, and did meet my critera. They were my 1st choice for several months.

Xiotech had a very interesting product offering. I first saw their product demoed at a local VMware event in Anaheim. The thought of a storage array that could heal itself is intoxicating. Coupled with the fact that there was no maintenance for 5 years, this was almost too easy. However there pricing was outrageous, and they were unwilling to sweeten the deal. Then their Sales Rep started to pull a Michele Pfeiffer from Fatal Attraction. The moment that I did not return his call, he would just show up at my office that day. I finally lost it, and called his manager and told them that I was through! They were promptly removed from the shortlist, and the xiotech.com domain was added to the Spam Filter.

EMC was, not to my suprise, selling me the same BS that I was getting back in the early 90's. Back when they were still Data General. I  couldn't argue with their performance stats. I couldn't argue with their feature set. Personally I wasn't happy with their Sales presentation. It just felt old, and tired. Admittly I had no intention of signing with them... but I kept them in the loop just incase.

Astute readers will notice that I have not said a single thing about NetApp at this point. There is a reason for that.

You will need to come back and read Part 2 of this post. (Only because its late, and I really think that I am a little long winded at this hour.)

Please feel free to comment. I am interested in your feedback.

**EDIT**: [Part II]({{ site.url }}/netapp-vs-equallogics-pt2) is here!
