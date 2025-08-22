---
_edit_last: "1"
author: wpgundersen
categories:
  - webtech
  - windows
date: "2015-03-02T13:49:05+00:00"
guid: https://gundersen.net/?p=363
parent_post_id: null
post_id: "363"
tags:
  - certificate
  - https
  - iis
  - iis7
  - server
  - ssl
  - wildcard
  - windows
  - windows-server
title: SSL Host Header with wildcard certificate on IIS7 solved
url: /ssl-host-header-with-wildcard-certificate-on-iis7-explained/

---
When adding a HTTPS site on IIS7, the Host header field is disabled. If you have one IP per site, as used to be the requirement, this is not a problem. But when you want to host multiple sites on one IP, it is a show stopper.

In my case I had a wildcard certificate, and ran into this when adding my second site. It was important not to cause any down-time on the already running site.

![SSL Cert Rename](/wp-content/uploads/2015/03/cert_rename_1.png)

It turns out that IIS7 will disable the Host Header field when the name (friendly name) of the wildcard certificate is anything else than \*.domain.tld. How unexpected.

The solution is to change the friendly name of the certificate. Luckily, renaming the certificate and adding a host name to any existing sites can be done without service interruption.

First, start the MMC (Start, Run, mmc) and add the Certificate snap-in.

![Add MMC snap-in](/wp-content/uploads/2015/03/cert_rename_2.png)

Choose Computer account, and then Local computer.

Next, use MMC to browse to Certificates, Personal, Certificates, right-click the offending certificate and select Properties.

![Friendly Rename](/wp-content/uploads/2015/03/cert_rename_3.png)

A friendly rename will not impact any running sites.

If the IIS manager is running, restart it (but no sites needs a restart). Now, magically, the Host header field is enabled.

![A friendly rename solves everything](/wp-content/uploads/2015/03/cert_rename_end.png)

Shame on you, Microsoft, for linking specific functionality to a non-specific name field.
