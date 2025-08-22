---
_edit_last: "1"
_encloseme: "1"
_pingme: "1"
_wp_old_date: "2021-01-14"
_wp_old_slug: lenovo-x1-nano-in-practical-use
author: wpgundersen
categories:
  - hardware
  - windows
date: "2021-01-16T17:33:52+00:00"
guid: https://gundersen.net/?p=493
parent_post_id: null
post_id: "493"
tags:
  - lenovo
  - thinkpad
  - x1
  - x1-nano
title: Lenovo X1 Nano - actual use
url: /lenovo-x1-nano-hands-on-practical-use-review/
wp_featherlight_disable: ""

---
I have used the X1 Nano heavily for the last few days after configuring it with all the software I use as a software developer and manager. If you are considering the X1 Nano, but is wondering how it really is, this post is for you.

{{< figure src="/wp-content/uploads/2021/01/nano%5Flap%5Ftop.jpg" alt="" caption="" >}}

I got the first model available in Scandinavia, the Core i5 16/256GB with 4G and non-touch matte 2160px 16:10 screen with extended warranty (20UN002CMX). This came from the first shipment to Norway, while Lenovo's own web shop still just said "Coming soon".

You have probably read about ports, weight, screen, finish etc on all the preview reviews already. I will try to cover what they didn't.

### SSD

Being a software developer, the 256GB (WD SN530 - SDBPMPZ-256G) is too small so I will swap it for a 1TB drive as soon as I can. Why not already? The Nano uses a 42mm long M.2 drive (form factor 2242), and 1TB drives with PCI (and Evo) simply are not available yet in this size, at least not from any supplier I can find. It might also be the case that only so-called single-sided (thin) drives will fit, Lenovo has other models where this is a requirement. The replacement I am looking for is the same drive in 1TB edition, the SDBPMPZ-1T00.

Update March 2nd: For some reason I received an alternative drive Samsung PM991 MZALQ1T0HALB-000L2 in a box marked with Western Digital. Not too worried about the performance specs for my usage, but still strange. To clone and install I needed to disable Windows Bitlocker.

{{< figure src="https://gundersen.azureedge.net/wp-content/uploads/2021/03/nano-disk-450x328.png" alt="New drive and shipment box" caption="New drive and shipment box" >}}

### 4G WWAN

Had there been several models to choose from, I would have gone without the 4G WWAN. Lenovo seems to invariably choose WWAN modules with driver issues; it took several driver updates before mye previous one became stable. Also, it just takes a swipe and click to share WiFi from my mobile phone (which also has 5G).

True to form, the only "critical" driver update on the Nano right now is for the Fibocom 4G module. But it seems ok this time, though, perfectly stable so far. It comes with a SIM which can be bonded to a selection of mobile broadband suppliers. Since my current operator is on this list, I just added the SIM to my existing subscription.

### Size

I've been using Thinkpads exclusively since 2000, when I started out with an A21p. After 2013 I have been on the X1 Carbon series. This time, I went down a size from 14" to the 13" X1 Nano.

The 16:10 screen really makes a difference, it does not feel much smaller than a 14". When the switch from 4:3 to 16:9 was made way back when, I tried to hang on to 4:3 for as long as I could. Pleased to see the world coming to its senses again.

{{< figure src="/wp-content/uploads/2021/01/nnao%5Fsize%5Fvs%5Fx1gen3.jpg" alt="" caption="" >}}

I was surprised with the keyboard's shorter key travel. While the Nano is noticeably smaller, it is the same thickness as the X1 Carbon. It also has the same thickness all over ("box"), while the X1 Carbon is slightly wedged/triangular in its profile. The new keyboard takes a little bit getting used to. But it is more of a getting used to kind of thing, not a downgrade.

I don't use the screen and keyboard most of the time. To comfortably work at the office (both home-office and office-office), I got two docking stations (which comes with massive 135W power supplies). The docking station supports two monitors (these can be either HDMI or DP).

### Power supply

Speaking of power supplies, the one in the box has a monstrous cable. People buying ultra portables do so to carry them around, clearly the power supply should be ultra portable as well. I just replaced the wall connector cable for a far shorter one. On the positive side, the charger fast-charges my Android phone (Super Fast 2.0) so now I only have to carry around one charger.

{{< figure src="/wp-content/uploads/2021/01/nano%5Fps%5F450px.jpg" alt="" caption="" >}}

There is another, more pricey, alternative to the monster cable: Lenovo's own travel charger. I got this as well, but it is still bulky albeit in a different way.

### Battery

My use is varied. I don't often run CPU intensive tasks, but I often have a WSL2 (Windows Subsystem for Linux) Ubuntu in the background, so I'm not sure if my use is representative. Battery capacity so far seems ok, but not overwhelming. Charging is fairly quick, 25% (from 8% to 33%) in 20mins on the pictured 65W supply. From there it would take less than an hour to full charge if left alone (1h 15min total). With light use it took 1h 40min total from 8% to 100%.

The battery indicator in the Windows task bar is deceptive. 65% looks like 80%, you need to hover above or click it to get an actual reading.

Lenovo has used fairly sturdy power connectors in the past. Can't help but wonder if that flimsy USB-C connector will survive as many years of daily plug-ins/outs.

### Other stuff

The face ID feature works well. But it only works with the built-in camera, so while docked you will want to keep the laptop open and facing you to use it. Not only does it recognize you and unlocks, but it also locks fairly quickly when you are away.

### Conclusion

With this setup I have a durable workhorse with a comfortable setup at home/office and an extremely light travel companion to be whipped out at any time.

In a perfect world, I would have enjoyed 32GB RAM and more ports, but otherwise, this is just perfect.
