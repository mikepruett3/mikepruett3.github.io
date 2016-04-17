---
author: Mike Pruett
comments: true
date: 2009-08-04 17:24:49+00:00
layout: post
slug: trading-in-tapes-for-clouds
title: Trading in Tapes for Clouds...
tags: [cloud, storage, tape]
categories: [Storage]
share: true
---

You would not believe how many vendors & ISV's I talk to throught the workweek. On any given day, I usually have to tell some poor sap "NO", or "I am unintrested in your products, please stop calling"! It does take an enormous ammount of paitentce not to just hang up the phone, but I digress.

The other day I got a rather interesting call, this Vendor wanted to inform me of the Tremendous Cost Savings that my employer can obtain by using their new product. Normally I don't bother giving these people the time of day, but this company was someone that I actually wanted to work with (Although I never found the chance/time/$$$ to do so).

<!-- more -->

[Data Domain](http://www.datadomain.com) is a De-Duplication vendor who's products are targeted for a unique market. They have an appliance (ie box) which does De-Dupe on archival storage. I had looked at them back when my employer was first migrating from our extensive Symantec Backup Exec deployment, to a NetBackup Infrastructure. At the time, the $$$ was in range with the other disk storage that I had looked at, but I didn't really see the benefit.

My Employer makes extensive use of LTO Tape storage, and has a varied approach to DR. Tape is King in the Executives eyes.  As long as there are tapes stored somewere off-site, they were much too happy to continue that process. Executives are not concerned about how the backup's are done, nor how long it will take... just that there is a backup, and its on its happy way outside the office. (I can imagine that a lot of you have this problem.) Keep in mind, this has been our current practice for several years. It just works. It keeps my Exec's happy. Why would I want to change that? Ok, lets get back to the article shall we.

So now Data Domain is back, and looking to convince me that it is cheaper to store my Data in small cumulus clouds! How is that possible, Pór Qué? I, like many other Technology Managers, have heard of "The Cloud"... but why would I want to store my backup data there? Is there any true savings (CFO Hat on)!

Unless you have been in a cave for the past few years, Cloud Computing is on the rise. It seems to be the quintessential marking buzzword for this **millennia.** A throw back to the old "ASP" days, its nothing more that taking traditional services which would require infrastructure in-house, to be rented or purchased as needed for capacity growth. Typically you would see this a lot with Content Providers. The provider would store that latest Video file on their Public Webservers. However, when that video goes "Viral" the video is then transfered from those costly internally managed webservers, to "cheaper" commodity webservers hosted by some ISP. These "Cloud" webservers are then just rented for the time that you use it. Saving you the cost of having to purchase that infrastructure in the first-place. When the video is no longer popular, you then give back those webservers and pay the bill. All online, all automatic or its supposed to be anyways.

There are a lot of vendors whom are now just trying to expand their offerings into the cloud. Apparently Data Domain was one of them. Their pitch was clever, as they proposed that I could eliminate the use of tapes in my environment. This could save me some $$$ per month, so I thought I would actually listen this time.

Their pitch is to ditch the tape & the Offsite Data Archival vendor (a.k.a. Iron Mountain). By using multiple boxes from Data Domain, we would be able to send our NetBackup Disk Images over to a hot-site or collocated environment. This sounds cool, but our Disk Images can be in the 40 to 60 TB range. Of course these images would be de-duplicated, so the actual size to be transfered would be less. I am intrigued, but I needed to make a quick cost analysts before I sign the P.O.

We are backing up a total of 60TB of SAN Storage. Out of this 60TB, there is maybe a total of 500GB of Data Change per day. We do nightly, weekly, monthly, and yearly archive tapes on a regular basis. This usually accumulates to 35 to 40 LTO3 tapes per month. The average cost of a LTO3 tape is $40. So in a worst-case scenario, this equates to $1,600 a month operating cost (40 x $40 = $1,600). Off-Site service for us is relatively cheap. Iron Mountain charges about $400 a month if we are not doing anything to fancy. The Capital Costs to implement have not really expanded over the years, but for shits & giggles lets just say $6,000 per year.

So to match this in a Cloud Backup approach, using a Data Domain solution you will need two appliances at $20,000 (approx) each. Also, there is the cost of network connectivity between your environment & the colo. A standard 10MB Circuit should set you back $2,500 to $3,000 a month. There is also the cost of Maintenance, at 18% per year thats $3,600 each. You can see how this is does not really implore any cost savings. Actually, it would cost my employer more that if we just keep doing what we have allways done.

I have not reviewed any other services so please do not consider this to be the ultimate answer, but I just cant see a logical justification to switch to Cloud Backup Storage. Tapes are still more cost effective (shocker).

I will say that there are other reasons why you would want to utilize Cloud Backup Storage, other than Financial.



	
  * Ease of Management

	
  * Lower Risk due to Left/Lost/Damaged Tapes

	
  * Capability of doing Multiple Backups to Different (Desperate) sites


I am probably not done with this topic, but it did get my juices going. I am glad that Data Domain called, but I am still not ready to jump ship.

If you liked this article, or are just looking for some support with the "Anti-Cloud" movement... please comment & review.
