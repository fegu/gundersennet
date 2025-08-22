---
_edit_last: "1"
_thumbnail_id: "309"
author: wpgundersen
categories:
  - freebsd
cover:
  alt: OpenVPN OpenBSD FreeBSD
  image: /wp-content/uploads/2014/11/OpenVPN_BSDs.png
date: "2014-11-03T22:04:26+00:00"
guid: http://www.gundersen.net/?p=306
parent_post_id: null
post_id: "306"
tags:
  - freebsd
  - openvpn
  - pf
  - vpn
title: OpenVPN server on FreeBSD with pf firewall
url: /openvpn-server-on-freebsd-with-pf-firewall/

---
FreeBSD 10, with the new and improved packet filter/firewall pf, and OpenVPN are all great products. But I had a not so great time making them play together - especially with a Windows 8 client. As with everything, it is easy when you know how.

Most OpenVPN examples seem to be using the tap interface and ethernet bridging. To keep things simple, I wanted to go with the default ip-routed tun interface. Apart from being default, thus requiring less config fiddling, it fits nicely with pf and requires one less kernel module.

After making it all play together, I also wanted the connecting clients to access the internet through the VPN connection, necessitating some routing. The last step is not necessary if all the resources the VPN clients will need are on the server itself. A similar step will be required if the clients should access other servers close to the VPN entry-point.

The steps outlined here should also work on FreeBSD 9.

## Preparing certificates

Installing is as easy as

```
pkg install openvpn
```

But before we can go ahead and start it, we need keys and certificates. Decide on somewhere to keep the server's private keys and other crucial files, I opted for

```
mkdir /root/sslCA
chmod 700 /root/sslCA
```

This needs to be put in /etc/ssl/openssl.cnf as

```
dir            = /root/sslCA    # Where everything is kept
```

OpenSSL also requires some folders and a serial number

```
cd /root/sslCA
mkdir certs private newcerts
echo 1024 > serial
touch index.txt
```

Then, let's generate the 10 year Certificate Authority which will be used to sign the certificates

```
openssl req -new -x509 -days 3650 -extensions v3_ca -keyout private/cakey.pem -out cacert.pem -config /etc/ssl/openssl.cnf
```

OK, that authority can now sign the server certificate. This is a two-step process where we first generate a request for a certificate, then the actual certificate. Note that while you can put almost anything in the fields when asked, you will need at least a common name for each, and the organization must be the same for both certificates.

```
openssl req -new -nodes -out server.req -keyout private/server.key -config /etc/ssl/openssl.cnf
openssl ca -out server.crt -infiles server.req -config /etc/ssl/openssl.cnf
```

Answer yes \[y\] where asked.

If you want to use password-based authentication for the clients, these are all the certificates you need. But if you want certificate based authentication, then each client will also need a certificate. Just create another request and certificate in the same manner, but with "server" replaced by "client". You will need to copy those files (and the CA cert) to the client later.

The last piece of the plumbing puzzle is the random Diffie-Hellman parameters.

```
openssl dhparam -out dh1024.pem 1024
```

## Configure OpenVPN

We will base our config on the sample file, so copy it first.

```
cp /usr/local/share/examples/openvpn/sample-config.files/server.conf /usr/local/etc/openvpn/openvpn.conf
```

Most settings are defaults. In particular we avoid some work with tap and bridges since we are using a tunneled ip routed interface. But we need to tell OpenVPN where we put everything. Look for the lines in openvpn.conf starting with ca, cert, key and dh.

```
ca   /root/sslCA/cacert.pem
cert /root/sslCA/server.crt
key  /root/sslCA/private/server.key
dh   /root/sslCA/dh1024.pem
```

If you want the client to use the VPN as the default gateway, i.e. not only to access server resources, but perhaps also to surf the web, you will need to put these two lines either in the server or client config. You will also need some routing, see below.

```
push "redirect-gateway def1 bypass.dhcp"
push "dhcp-option DNS 8.8.8.8"
```

If you want the clients to be able to see each other, uncomment the line

```
client-to-client
```

Now, prepare to run by adding to /etc/rc.conf:

```
openvpn_enable="YES"
openvpn_config="/usr/local/etc/openvpn/openvpn.conf"
```

And then, ignition:

```
/usr/local/etc/rc.d/openvpn start
```

## Routing

If all you want is for the clients to access the server, no routing is necessary. If you don't already have pf (or another firewall) enabled, consider yourself done with the server setup. However, if you have a firewall and/or want the clients to access the internet, or other servers in your LAN, you are in for  some pf magic.

The packet filter pf just recently got SMP support in FreeBSD. It perfectly illustrates how the FreeBSD community values performance, now having a faster port than the native OpenBSD version. If only we could port the "match" functionality as well.

Type ifconfig and identify the name of your network interfaces.

create a file /etc/pf.conf containing

```
# default openvpn settings for the client network
vpnclients = "10.8.0.0/24"
#put your wan interface here (it will almost certainly be different)
wanint = "vtnet0"
# put your tunnel interface here, it is usually tun0
vpnint = "tun0"
# OpenVPN by default runs on udp port 1194
udpopen = "{1194}"
icmptypes = "{echoreq, unreach}"

set skip on lo
# the essential line
nat on $wanint inet from $vpnclients to any -> $wanint

block in
pass in on $wanint proto udp from any to $wanint port $udpopen
# the following two lines could be made stricter if you don't trust the clients
pass out quick
pass in on $vpnint from any to any
pass in inet proto icmp all icmp-type $icmptypes
```

Note that all incoming TCP from the WAN is blocked with this setup, add a line resembling the "pass in for proto udp" one to open specific TCP ports.

Rule 1 of firewall design is "Never blindly copy-paste rules". While the above has been designed to not expose copy-pasters to too much danger, it is certainly better to extract the essentials and adapt the pass rules to your own needs. If you want IPv6 you have to.

Since pf is included by default, all we need is some rc.conf settings

```
gateway_enable="YES"
pf_enable="YES"
pf_rules="/etc/pf.conf"
```

And then, enable packet forwarding, enable pf as our firewall, and start it

```
sysctl net.inet.ip.forwarding=1
pfctl -ef /etc/pf.conf
```

Nevermind any messages about missing ALTQ, we don't need QoS (yet).

## Configuring Clients

Thanks to using the tun interface and keeping to the defaults, the client configuration can be kept close to the defaults. Just search online for tutorials for your operating system. If your client is Windows 8, read on.

The Windows client includes an OpenVPN GUI program. I could not get that to show any GUI. If you can't either, don't despair. We will go old school and edit the settings file instead. If you don't want to install the OpenVPN GUI, you don't need it. You do, however, need the TAP adapter (selected by default) even though we have configured the server for TUN.

After installing the Windows OpenVPN client, copy the client.ovpn file from c:\\Program Files\\OpenVPN\\sample-config to ..\\config. It will need the CA certificate, client key and client certificate from the server to accompany it.

Edit the client.ovpn file slightly (it needs to be done as administrator). Look for the following lines and edit accordingly.

```
remote 1.2.3.4 # put your server IP or name here
# the certificate files from the server (separate client certificate, not server)
ca   ca.crt
cert client.crt
key client.key
# remember to keep these lines commented out if they appear
;ns-cert-type server
;remote-cert-tls server
```

At the end add

```
#this line sets the VPN as default gateway
redirect-gateway def1
#these lines stroke Windows 8 the right way
route-method exe
route-delay 5
route-metric 550
```

To start the connection we need to run (as administrator) from the config directory

```
"c:\program files\openvpn\bin\openvpn.exe" --config client.ovpn
```

The default OpenVPN network is 10.8.0.0 with the server at 10.8.0.1. Try pinging it.
