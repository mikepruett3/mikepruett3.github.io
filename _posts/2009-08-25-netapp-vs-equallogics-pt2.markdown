---
author: Mike Pruett
comments: true
date: 2009-08-25 17:03:52+00:00
layout: post
slug: netapp-vs-equallogics-pt2
title: NetApp vs Equallogics - Part II
tags: [storage, netapp]
categories: [Storage]
share: true
---

Alright, back to this one...

So I mentioned in the previous post, that I was going to point out the differences in the NetApp offering. NetApp was actually not on my shortlist right away. To me, they had always been a provider of NAS appliances. I had looked at their stuff several years earlier. At that time, I was looking for Storage for an Exchange 5.5 to 2000 migration. (Not my current employer!) This was a large deployment, over 5,000 seats. The Exchange Infrastructure was composed of 25 different Exchange Mailbox servers, all using Direct-Attached (DAS) storage. Back then we had looked at EMC, and NetApp. The only problem with the NetApp offering at the time, was not an actual NetApp issue. Microsoft had listed in their documentation that Exchange 2000 required DAS or SAN storage, I.E. a block level device. NetApp only offered NAS appliances at the time. This pretty much forced us to go with an EMC solution. Years later, Microsoft is still pushing that line... but NetApp has gotten a whole-lot better.

<!-- more -->

At my current employment, we use a local Systems-Integrator for all of our big projects. Its not that we couldn't do the work ourself, but the IT department consists of only 7 people. Half of which are on the Desktop side. We had engaged our SI to work on our Upcoming VMware deployment, and our Rep was curious on our Storage selection. When I discussed with him some of the vendors we had looked at, he briefly mentioned that I should look at NetApp. Considering my previous background, I told him that I was uninterested in a NAS appliance, and that we needed more Block-Level Storage. He corrected me, and stated that the NetApp portfolio had grown to include Fiber Channel, iSCSI, as well as CIFS & NFS shares. When I asked him about storage performance, he also mentioned that NetApp offered FC & SATA disks. Now we have worked with this SI for a long time, and I did value our Rep's opinion. By the way, if you are in Orange County and are looking for a great team to work with... I cannot  recommend [VL Systems](http://www.vlsystems.com) enough!

So now with XioTech out of the way, our decisions where down to Pillar Data, EMC & NetApp. We entertained the local NetApp Rep at our offices, while he was demonstrating some of the power that the appliances had to offer. Through that discussion, I was able to put together a set of expanded requirements that my employer needed. One of the biggest requirements was the need for De-Duplicated storage. Let me rephrase that, we wanted De-Duplicated Primary Storage! When we presented this question to the other two vendors, they both balked and told us that we were crazy!

"You can't De-Dupe Primary Storage! You will impact performance on the array.", said both vendors. EMC at least had some sort of positive rebuttal, as they had just purchased Data Domain. "We can De-Dupe, but not on Primary Storage." Again, these were not the answers we wanted to hear. All of their griping, just made the NetApp offering more sweet. Ultimately the real decision came down to price. (Like Always!)

Both EMC & Pillar had done an excellent job of discounting their offerings, down to the $180k to $200k range. This would have met our budget by just a hair. But when the NetApp quote came in, both the IT Director & I were floored! The Systems Integrator had done an awesome job selling our company to NetApp, and really gave them an understanding on how we do business. He assured NetApp that once we had selected a new Primary Storage platform, that we would continue expansion on that device. (Told you that VLS is a great partner to work with.) NetApp's quote came in at less than $150k. Coupled with the fact that we could De-Dupe Primary Storage... and we were sold!

So now you know how we went from a Equallogics array to a NetApp appliance. I think that its prudent we cover some of the major differneces between the two...



	
  1. The Equallogics arrays only offer iSCSI protocols for block-level host connectivity, were as the NetApp appliances offer Fiber Channel & iSCSI. (Soon to offer FCoE)

	
  2. NetApp appliances also offer CIFS & NFS shares to be directly hosted off of the Storage Controllers (i.e. Filers), the EQL arrays require an additional server to attempt this feat.

	
  3. EQL arrays offer SATA disks in an array (at the time, the do offer SAS & SSD's now), NetApp offers Fiber Channel, SATA, and SAS disks.

	
  4. When expanding storage, EQL array's are limited to GigE connectivity for the interconnects, NetApp supports GigE, 10GB, and FC.

	
  5. Because of their iSCSI only connectivity, the EQL arrays are outfitted with several GigE Host-Facing interconnects, NetApp supports several connectivity options... you decide what works.


I have to say that if you are looking for a workhorse array that you can expand with, the NetApp appliances is a solid offering. So far we have consolidated about 80% of our storage needs on to the NetApp filers... and have reaped a 40% De-Duplication in total storage. So for every 1TB of storage, we are actually storing approximately 410GB. Thats a huge cost savings, and allows our department to lower its costs per year!

Please feel free to comment. I am interested in your feedback.

You might want to go back, and read [Part I]({{site.url}}/netapp-vs-equallogics-pt1)...
