---
_edit_last: "1"
author: wpgundersen
categories:
  - hardware
date: "2012-07-04T18:45:20+00:00"
guid: http://www.gundersen.net/?p=4
parent_post_id: null
post_id: "4"
tags:
  - backup
  - crash
  - disk
  - hard-drive
  - hdd
  - intel
  - lenovo
  - ssd
title: 'Solid State Drives: the best of drives, the worst of drives'
url: /solid-state-drives-the-best-of-drives-the-worst-of-drives/

---
There is no denying that solid state hard drives have a lot of advantages. They are fast, silent and runs cooler than ordinary drives. But when they fail, they fail hard. Really hard.

![](//gundersen.net/wp-content/uploads/2012/07/ssd2.jpg)

The Intel 160GB SSD stock drive in my Lenovo T420s failed in May 2012, less than one year after the manufacturing date. And when I say failed, I mean it. The laptop suddenly responded slowly, then after a few seconds completely froze, gave a blue-screen and died. It would not reboot.

During BIOS POST the disk identified itself as an IntelBootloader with a size of 8MB.

![](//gundersen.net/wp-content/uploads/2012/07/intelbootloader.png)

I mounted the drive in a different laptop with a very handy SATA-to-USB adapter, but no amount of trickery (MBRFix64, HDAT2, EASEUS recovery, Aomei manager - or cursing) changed the fact that it was dead.

Luckily my laptop carries two drives, and this was only my system drive. I had backups of the important files and "just" had to spend one entire working day reinstalling a complete working system with all my applications and settings.

Lenovo replaced my disk without requiring me to send it in. I have since learned that my experience was not atypical, and that this is, in fact, how SSDs fail. Keeping in mind that this was neither very early, as in within-first-30-days early, nor an old drive, backups are paramount when relying on solid state disks.

Gone are the days of partly failed drives and crashes being merely an inconvenience, now the entire drive is gone. If you don't do backups, you are looking at an interesting future indeed.
