---
_edit_last: "1"
_thumbnail_id: "299"
author: wpgundersen
categories:
  - freebsd
cover:
  alt: Unreal Tournament 99 GOTY
  image: /wp-content/uploads/2014/10/Unreal450.png
date: "2014-10-19T13:56:19+00:00"
guid: http://www.gundersen.net/?p=298
parent_post_id: null
post_id: "298"
tags:
  - freebsd
  - unreal-tournament
title: Unreal Tournament Server on FreeBSD
url: /unreal-tournament-server-on-freebsd/

---
Unreal Tournament from 1999 (Game of the Year - GOTY - Edition) is now 15 years old and still a great multiplayer game, especially at LAN parties. It is free to download so you can share this gem with anyone. If you play it regularly you will want to host a server.

This guide for FreeBSD closely follows the [Linux guide](http://wiki.unrealadmin.org/Server_Install_linux), but as always FreeBSD needs a couple of adjustments.

First of all make a new user with adduser. All default settings, I call mine ut99.

You will also need the files as well as the patch from v436 to v451 (to get functioning web admin).

```
wget http://ut-files.com/Entire_Server_Download/ut-server-436.tar.gz
tar xfz ut-server-436.tar.gz
cd ut-server
wget http://www.ut-files.com/Patches/UTPGPatch451LINUX.tar.tar
tar xfj UTPGPatch451LINUX.tar.tar
wget http://ut-files.com/Entire_Server_Download/server_scripts/asu-0.6.tar.gz
tar xfz asu-0.6.tar.gz
chmod +x asu.sh
cd System
ln -s libSDL-1.1.so.0 libSDL-1.2.so.0
```

For some reason (Linux...) the first line of asu.sh specifies sh, but the script later tests for bash. We need to install bash if you don't already have it

```
pkg install bash
```

then change the first line to

```
#!/usr/local/bin/bash
```

Now run asu.sh to set some options. First choose I, answer the prompts, then S for more prompts, then X to exit. Default options are ok; just remember to enter the user you added when prompted for it. Whatever you enter here can be changed later in System/UnrealTournament.ini.

asu.sh creates a script ucc.init which we will use to run the server. But before anything will run we need to

```
chmod +x ucc.init
chmod +x ucc
cd ..
chown -R ut99 ut-server
```

If you already have Linux compatibility installed, you are good to go. But if not, you will get ELF binary type "3" not known, which means:

```
kldload linux
pkg install linux_base-f10
echo 'linux_enable="YES"' >> /etc/rc.conf
```

At last, start the server with

```
./ucc.init start
```

If you want to move the server files later, note that ucc.init contains the absolute path two places. These will need to be updated.

Even though the server starts we are not done yet. Some of the maps in the Maps directory are zero-length files. These must be copied from the original game download. Most commonly these are DM-Morbias\]\[.unr, DM-Curse\]\[.unr and DM-Deck16\]\[.unr.

If you want to make the server available from the internet, the ports to be NATed are 7777-7783 (UDP) as well as the web admin port, most commonly 5580 (TCP). The web admin port can be set in UnrealTournament.ini. Make sure to set the web admin username and password there as well.
