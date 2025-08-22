---
_edit_last: "1"
_thumbnail_id: "104"
author: wpgundersen
categories:
  - freebsd
  - windows
cover:
  alt: Pidgin on Lync
  image: /wp-content/uploads/2013/03/itworks.png
date: "2013-03-26T21:38:18+00:00"
guid: http://www.gundersen.net/?p=101
parent_post_id: null
post_id: "101"
tags:
  - freebsd
  - lync
  - microsoft
  - office365
  - pidgin
  - sip
  - support
  - windows-8
title: 'Pidgin to Lync integration: solved'
url: /pidgin-and-lync-problem-solved/

---
I had been trying to get the open source instant messenger client [Pidgin](http://pidgin.im "Pidgin") to connect to Lync using [SIPE](http://sipe.sourceforge.net "SIPE"). However, they wouldn't play nice.

The Pidgin GUI kept saying "Web ticket request to https:// webdir0e- ext.online.lync.com:443/ CertProv/ CertProvisioningService.svc failed" while the debug log from running pidgin --debug ended with an XML containing "Web ticket request error - SIP URI mismatch" - after confirming username and password to be ok.

My environment was Pidgin 2.10.7 and SIPE 1.15.0 on Win8 connecting to Office 365 (no local AD).

The great interwebs did not have a lot of information, but the case was eventually solved by heroic effort from Microsoft support engineer Sankaranarayanan (v-9sak). He went above and beyond on my support request, consulted superiors, spent several hours with me on the phone while we worked together on debugging and fixing this - and this on a problem and client (Pidgin) that Microsoft does not even support!

I am duly impressed both by Microsoft and the-man-whose-name-contains-a-lot-of-a's in this case.

The problem was due to my primary email being different from my account user name. Possibly because we were early adopters of BPOS (previous version of Office 365), possibly because my first-time signin on Lync was using my primary email, possibly because of something else entirely, Lync signed in with my primary email while Outlook and everything else signed in with my username. Pidgin required that my username be my Office 365 username (or else it would give password error), but it would give the elusive SIP URI mismatch error if I put anything else than blank or my username in the logon field.

While Microsoft told me it was possible to fix this by changing my Lync logon from primary email to username, the best solution for me was to change my username to match my primary email address.

This consisted of backing up my email into a new PST-file (not needed), deleting everything under C:\\Users\\mywindowsuser\\AppData\\Local\\Microsoft\\Office\\15.0\\Lync and changing the username part-by-part first the last part from domain.no to domain.onmicrosoft.com, then the first part, then the last part back again - and logging in to portal.microsoftonline.com between the steps, checking that my Lync settings was being propagated. I am not sure how much of this was "just to be sure".

In the end my Pidgin settings was just like SIPE tells us to:

{{< figure align="alignnone" width=339 src="//gundersen.net/wp-content/uploads/2013/03/itworks%5Fbasic.png" alt="" caption="" >}}

On the General tab:

Protocol: Office Communicator
Username: Office365-username of the form name@domain.no
Logon: blank
Password: duh
Local alias: blank

On the advanced tab:

Server: blank
Connection type: automatic
Useragent: UCCAPI/4.0.7577.314 OC/4.0.7577.314 (Microsoft Lync 2010)
Authentication: TLS-DSK
Single sign-on unchecked

NOTE: For Lync 2013 a reader has reported that you need useragent
UCCAPI/15.0.4420.1017 OC/15.0.4420.1017 (Microsoft Lync)

{{< figure align="alignnone" width=339 src="//gundersen.net/wp-content/uploads/2013/03/itworks%5Fadvanced.png" alt="" caption="" >}}

Nothing else.

Thanks to Stefan Becker for looking at my debug log output, and to Tor Arne Helgesen for the Lync 2013 user agent string.
