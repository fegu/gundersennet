---
_edit_last: "1"
_thumbnail_id: "444"
author: wpgundersen
categories:
  - freebsd
  - hardware
cover:
  alt: DRAC5
  image: /wp-content/uploads/2017/06/drac5.jpg
date: "2017-06-07T21:55:44+00:00"
guid: https://gundersen.net/?p=435
parent_post_id: null
post_id: "435"
tags:
  - dell
  - drac
  - drac5
  - hardware
  - java
  - poweredge
  - remote-management
  - server
title: 'Solved: Dell DRAC 5 not responding'
url: /solved-dell-drac-5-not-responding-to-httphttps/
wp_featherlight_disable: ""

---
I suddenly found myself in charge of an unresponsive old Dell PowerEdge 1950 server with a DRAC 5 remote management card from 2006. Don't ask. The DRAC was pingable, but did not respond to http/https. When that was sorted out, the remote console did not want to play nice, because Java. The solution was long-winded and might help others, so here it is. Start at whatever step suits you, it is all a case of IT archeology at this point.

**Get DRAC 5 working: Logs and Firmware**
A well-known trick of the trade with these older DRAC 5 cards is to clear the log. Log in using SSH, clear the logs using `racadm clrraclog` and restart using `racadm racreset`.

However clearing the log isn't always enough, in that case a firmware reflash (or upgrade) is necessary. First download firmware from Dell, setup a TFTP server somewhere, and have the DRAC card fetch it with `racadm fwupdate -g -p -a ipadress`.

If the firmware does not breathe life into the DRAC card, then we're out of options.

**Getting Console to work**
Assuming you get into remote management, chances are you want to interact with the console. Lucky you, a reason to install Java on your desktop again. After install (I used JRE 1.8), put your DRAC site in trusted sites in Configure Java, Exception Site List. While you're in there, check "Suppress offers when installing or updating java" as well.

[![](/wp-content/uploads/2017/06/FreeBSDConsole_450px.png)](/wp-content/uploads/2017/06/FreeBSDConsole.png)

If this doesn't suffice, perhaps you see "Error when reading from SSL socket connection", we need to tell Java not to be so uptight about security. This is a DRAC 5 from 2006 after all. Find the java security configuration text file, in my install it was at `c:\program files (x86)\java\jre.1.8.0_73\lib\security\java.security`.

Disable the disabling of older ciphers by putting # in front of the offending modern limitations like this: `#jdk.tls.disabledAlgorithms=SSLv3, RC4, MD5withRSA, DH keySize < 768`

Then finally, we have console.

**Getting remote media to work**
Half of the fun of remote console is to be able to mount a local ISO to install an OS. The secret sauce here is Firefox. Just use Firefox, and also under the virtual media config set it to auto-attach.

[![](/wp-content/uploads/2017/06/VirtualMedia-1.png)](/wp-content/uploads/2017/06/VirtualMedia-1.png)

Voila! I love it when a plan comes together.
