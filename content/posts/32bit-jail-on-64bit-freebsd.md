---
_edit_last: "1"
author: wpgundersen
categories:
  - freebsd
date: "2012-09-08T17:58:11+00:00"
guid: http://www.gundersen.net/?p=75
parent_post_id: null
post_id: "75"
tags:
  - 32bit
  - 64bit
  - amd64
  - freebsd
  - i386
  - jail
title: 32bit jail on 64bit FreeBSD
url: /32bit-jail-on-64bit-freebsd/

---
Sometimes it can be necessary or preferable to run software in 32bit versions, even if the OS is 64bit (i.e. running the i386-version on amd64 OS).

As an example, software with deep memory structures mainly consisting of pointers, such as dictionaries of dictionaries of light-weight objects, will occupy almost twice the RAM on a 64bit OS. If multiple instances of, say, 3GB memory structures are needed, the 64bit penalty quickly adds up.

While the FreeBSD ports and packages collections are very convenient, they automatically select the "correct" version on install. Resorting instead to compiling from source, compiling 32bit binaries from source on a 64bit system can be fiddly and time-consuming (Python, I am looking at you).

Luckily, it is quick and easy to set up a 32bit jail on a 64bit FreeBSD. Here is an example, installing a FreeBSD 9.0 i386 jail on FreeBSD 9.0 amd64. This recipe goes for any jail, by the way. Only the fetch downloading the content is different.

Many jails are established by linking directories between the jail and host to save disk space. While it is possible to go this route for 32bit-on-64bit jails as well (see [http://en.jnlin.org/2008/06/07/12/](http://en.jnlin.org/2008/06/07/12/) for an example of this route) it will disable w, ps, top and other system utilities because of the bitness-mismatch. Since you likely won't require many 32bit jails, I prefer to go for the full enchilada. After all, it isn't any harder and the disk space requirement is not excessive.

\# Create jail directory with kernel
mkdir /usr/jails/32bitjail
cd /usr/jails/32bitjail
fetch ftp://ftp2.de.freebsd.org/pub/FreeBSD/releases/i386/i386/9.0-RELEASE/base.txz (or a more recent version)
tar xpf base.txz;rm -rf boot;rm base.txz

\# Find yourself an IP-address to use, for a server this will usually be static:
ifconfig vr0 10.0.0.96 netmask 255.255.255.255 alias

Note the network driver, here it is vr0, but it could be different in your box. Next, add the matching config to the host's /etc/rc.conf to persist this config on reboot:

ifconfig\_vr0\_alias0="inet 10.0.0.96 netmask 255.255.255.255"

And while you're in the host's rc.conf, add the jail config as well:

jail\_enable="YES"
jail\_list="32bitjail"
jail\_socket\_unixiproute\_only="YES"
jail\_sysvipc\_allowed="YES"

jail\_32bitjail\_rootdir="/usr/jails/32bitjail"
jail\_32bitjail\_hostname="32bitjail"
jail\_32bitjail\_ip="10.0.0.96"
jail\_32bitjail\_devfs\_enable="YES"
jail\_32bitjail\_devfs\_ruleset="devfsrules\_jail"

\# Save rc.conf and exit. Now for some generic jail config
mount -t devfs devfs /usr/jails/32bitjail/dev
cp /etc/resolv.conf /usr/jails/32bitjail/etc/

\# In the jail's rc.conf all we need is the keymap, just one line.

echo 'keymap="norwegian.iso.kbd"' > /usr/jails/32bitjail/etc/rc.conf

\# Then start the 32bit jail

/etc/rc.d/jail start 32bitjail

\# And log in to it

jls
jexec 1 sh # replace 1 with the jid given by jls
passwd # to set root pw
