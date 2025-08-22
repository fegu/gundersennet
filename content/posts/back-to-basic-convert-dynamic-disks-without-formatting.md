---
_edit_last: "1"
author: wpgundersen
categories:
  - windows
date: "2012-07-04T19:11:31+00:00"
guid: http://www.gundersen.net/?p=13
parent_post_id: null
post_id: "13"
tags:
  - hard-disk
  - hdd
  - windows
title: 'Back to basic: convert dynamic disks without formatting'
url: /back-to-basic-convert-dynamic-disks-without-formatting/

---
For the last time a dynamic disk in Windows has gone rogue on me. I won't go into why I even used them, but here is how to get out.

If for some reason you are stuck with an "Invalid Dynamid Disk", unable to use the disk manager in the computer management applet of control panel's administrative tools (or, at least unable to have it convert back to basic disk without formatting), then this is for you.

Step 1: The Free Route

First, get the great Freeware utility HxD from [http://mh-nexus.de/en/hxd/](http://mh-nexus.de/en/hxd/). Go to the download page, scroll to the portable version, download and run as administrator. Use the Extras menu to open your physical disk and find the byte at location 1C2. Change it from 42 (Dynamic disk) to 07 (Basic disk), save and reboot. This has worked for me (and countless others, judging by the prevalence of this tip in the blogosphere).

Step 2: Paying to get out

If this doesn't work, change it back. Then get Aomei Dynamic Disk Manager Pro. It is not free, but it saved me once and I promise the relief I felt when it worked totally made the asking price worth it.

Now, remember never convert to dynamic disk again.

{{< figure align="alignnone" width=450 src="//gundersen.net/wp-content/uploads/2012/07/dynamicdisk.png" alt="" caption="" >}}
