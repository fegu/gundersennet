---
_edit_last: "1"
_thumbnail_id: "28"
author: wpgundersen
categories:
  - freebsd
cover:
  alt: Teeworlds
  image: /wp-content/uploads/2012/07/splashtee.png
date: "2012-07-04T19:57:40+00:00"
guid: http://www.gundersen.net/?p=24
parent_post_id: null
post_id: "24"
tags:
  - compile
  - gaming
  - teeworlds
title: Compiling Teeworlds Server
url: /compiling-teeworlds-server/

---
This post was updated in December 2015 to account for clang in FreeBSD and the updated Teeworlds 0.6.3 (which contains an important server bugfix).

Teeworlds is a great game. It is free and runs on any platform, [download it now](http://teeworlds.com).

You will soon want to run your own server to host your own matches and levels. If you have a FreeBSD server without X, this is not straightforward. The FreeBSD Teeworlds package is game (client) and server bundled in one, and the Teeworlds server sources are meant for Linux. Here is how:

First, make sure you have Python installed. Then, fetch and unzip the sources:

\# fetch http://teeworlds.com/files/bam-0.4.0.zip
\# unzip bam-0.4.0.zip
\# rm bam-0.4.0.zip
\# fetch https://downloads.teeworlds.com/teeworlds-0.6.3-src.zip
\# unzip teeworlds-0.6.3-src.zip
\# rm teeworlds-0.6.3-src.zip

We need to remove the linux-ism before building. Edit bam-0.4.0/make\_unix\_clang.sh to remove -ldl near the end of the last line.

\# cd bam-0.4.0
\# ee make\_unix\_clang.sh

Then build it:

\# ./make\_unix\_clang.sh

It should build fine, now build the server itself:

\# cd teeworlds-0.6.3-src
$ ../bam-0.4.0/bam server\_release

If you get errors about mising g++, then install gcc48 (pkg install gcc48) and link gcc48 to gcc and g++48 to g++ in /usr/local/bin and retry. Likewise for python2.7 to python if necessary. You can now run the server:

./teeworlds\_srv -f teeworlds.cfg

A sample teeworlds.cfg file:

sv\_name "Underbuksepiratene"
sv\_high\_bandwidth 1
sv\_register 1
sv\_map dm7
sv\_rcon\_password "mypw"
sv\_maprotation dm1 dm2 dm6 dm7 dm8 dm9
sv\_rounds\_per\_map 2
sv\_motd "Welcome! Anything goes."
sv\_gametype mod

If you just want to play locally set sv\_register 0. If you want to make your server public make sure people can connect to it by UDP on port 8303.

Happy gaming!
