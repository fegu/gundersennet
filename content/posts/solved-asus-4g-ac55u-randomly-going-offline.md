---
_edit_last: "1"
_thumbnail_id: "420"
author: wpgundersen
categories:
  - hardware
cover:
  alt: ASUS 4G AC55U
  image: /wp-content/uploads/2016/11/ASus4G-AC55U_450px.png
date: "2016-11-13T13:31:45+00:00"
guid: https://gundersen.net/?p=418
parent_post_id: null
post_id: "418"
tags:
  - 3g
  - 4g
  - ac55u
  - asus
  - bug
  - coverage
  - firmware
  - lte
  - offline
  - signal
title: 'Solved: Asus 4G-AC55U randomly going offline'
url: /solved-asus-4g-ac55u-randomly-going-offline/
wp_featherlight_disable: ""

---
This Asus 4G router can be great, if configured correctly.

It has excellent sensitivity. Previously, I needed an external (outdoors) antenna to get 3G and 4G (LTE) coverage at the cabin; but the AC55U is sensitive enough to get the same reception indoors. I have another one at a remote shooting range to provide WiFi there.

But sometimes, even with the (at writing) most recent firmware 3.0.0.4.378\_8071, it suddenly disconnects from the mobile provider or refuses to route traffic. At first I wrote it off as random service issues with the mobile coverage. After all, these are rural areas with weak signal. But the outages kept appearing while my cell phone worked fine.

Turns out, the firmware has a couple of problems.

**PROBLEM 1: BUG IN METERING**

Since the mobile connection is metered, I made use of the ability to notify me by SMS of approaching usage limits. To avoid overcharge fees I also set a hard limit to shut down the mobile connection.

But there is an overflow issue in the firmware, triggering the forced shut-down seemingly at random.

{{< figure align="alignnone" width=2146 src="/wp-content/uploads/2016/11/Screenshot%5F20161029-113929.png" alt="Extreme (wrong) data usage" caption="Extreme (wrong) data usage" >}}

**The necessary work-around is to NOT use the Data Usage Limit feature, it has to be disabled by setting it to zero (see above screenshot).**

Since then, uninterrupted operation both at the cabin and the shooting range.

**PROBLEM 2: NOT AUTO-UPDATING ROUTING TABLE**

If you experience the router losing connection for minutes at a time for no reason, then try pinging some random server. If ping works the chances are good the problem is the routing table.

When the mobile provider updates the IP of the router, the router often fails to update the routing table, resulting in no traffic. The routing table can be found under the menu System Administration / Routing Table. As an example, I used my operator's smartphone APN telenor.smart with the dial-code \*99# (see picture above). The IP would change every few hours resulting in loss of traffic for several minutes. Changing the APN to internet and no dial-code solved this issue. I am not sure if it was "solved" just by greatly reducing the frequency of IP changes, or if the lack of dial code is relevant.

In any case, rebooting the router will update the routing table.
