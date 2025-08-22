---
_edit_last: "1"
_thumbnail_id: "258"
author: wpgundersen
categories:
  - freebsd
  - webtech
cover:
  alt: Netflix Mad Men
  image: /wp-content/uploads/2014/04/Netflix_MadMen.png
date: "2014-04-14T13:04:16+00:00"
guid: http://www.gundersen.net/?p=256
parent_post_id: null
post_id: "256"
tags:
  - freebsd
  - pfsense
title: American Netflix on any device without VPN or site-wide DNS changes - using pfSsense
url: /american-netflix-on-ipad-and-chromecast-without-vpn-using-pfsense/

---
It seems like everyone is finding ways to watch American Netflix content. While it is easy with one of many plugins on the computer, it gets harder on devices such as iPad or PS3 and even more so on Chromecast with its hard-coded google DNS. Some people will also want a solution for the home router, covering all devices at once.

The most common fix is to get a VPN. Usable from the computer, iPad, Chromecast or a sufficiently advanced router, a VPN routes your traffic through a server in the US. The main drawback is speed. Most VPN providers are over-subscribed and, in general, it will be hard to get Super HD streaming - at least with any stability and on more than one device. You will also want to separate your other browsing traffic from the VPN to avoid the speed loss, and avoid having all websites think you are from the US. All in all, a solution with drawbacks.

Another common fix is to get a custom DNS provider such as [unblock-us.com](http://unblock-us.com) or [unotelly.com](http://unotelly.com). After the extremely easy sign-up you just change your DNS server settings to point to theirs. The custom DNS will return the addresses to their own US-based servers (reverse proxies) for all Netflix-related lookups. All other traffic flows normally.

This avoids the speed loss of a VPN as the actual streaming goes directly from Netflix's servers to your home. It also does not affect your other web traffic. Usually a cheaper service than a VPN, this seems like the perfect solution. It does, however, have a slight security implication: the DNS provider can see all your lookups and could in theory log or divert some of it.

What we want is to divert only relevant lookups to the proxy provider's DNS. This is accomplished in pfSense by adding a domain override in the DNS Forwarder. The web gui for the domain override adds wildcards, so an entry for netflix.com will also cover all subdomains. We accomplish our task by adding the following to Services \| DNS Forwarder \| Domain Overrides and making sure the box " **Query DNS servers sequentially**" is checked:

```
netflix.com 208.122.23.23
netflix.net 208.122.23.23
rhapsody.com 208.122.23.23
pandora.com 208.122.23.23
hulu.com 208.122.23.23
```

The IP address is for the DNS server of unblock-us. Only the first two lines are necessary for Netflix, the others are for Rhapsody, Pandora and Hulu.

For the unbehaving Chromecast, with its hard-coded Google DNS, we are forced to add a NAT rule to reroute all DNS requests. In Firewall \| NAT \| Port Forward, add a new rule with these settings:

RDR: Unchecked
Interface: LAN
Protocol: UDP/TCP
Source: IP of your Chromecast
Source port: any
Destination: any
Destination port: DNS (53)
Redirect target IP: the DNS server of unblock-us or similar service
Redirect target port: DNS (53)

![Chromecast pfSense NAT](//gundersen.net/wp-content/uploads/2014/04/Chromecast_pfSense_NAT.png)

All in all this is a perfect solution. Switching regions can be done either in the DNS provider's web interface, or by disabling the pfSense settings.
