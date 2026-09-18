---
description: https://github.com/moxie0/sslstrip
---

# SSLStrip

[https://github.com/kimocoder/sslstrip/](https://github.com/kimocoder/sslstrip/)

{% hint style="warning" %}
SSLStrip is largely of historical interest now. It depends on a victim reaching a site over cleartext HTTP first, and between HSTS, HSTS preloading and browsers defaulting to HTTPS, that opening rarely exists on the modern web. Understand the technique, but do not plan an engagement around it without testing against the specific target first.
{% endhint %}

SSLStrip takes advantage of the tendency for many HTTPS sites to receive an HTTP request from clients first that's then redirected to HTTPS.

* Victim types in "example.com" instead of "https://example.com". This starts the connection request as a simple HTTP request that then gets redirected to HTTPS by the server.

SSLStrip maintains the HTTPS connection to the real server while serving plain HTTP back to the victim, capturing that first cleartext request and placing itself in the middle of the two.

It does this by rewriting the traffic passing back to the victim, stripping HTTPS references out of `href` attributes and downgrading 3xx redirect responses that would send the victim to HTTPS.

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
4. Gain MitM position via ARP manipulation, putting yourself in the middle of all traffic between any devices on the LAN.
   1. `ettercap -TqM arp:remote /<Local IP Range>// /<Local IP Range>//`
5. Read the SSLStrip log file to see what it found.
   1. `cat sslstrip.log`

{% hint style="info" %}
HTTP Strict Transport Security (HSTS) headers teach browsers to always expect HTTPS traffic between themselves and the server. If the user has already received the HSTS header from that site, SSLStrip won't work in this way.

Two things to keep in mind when picking a target:

* **Preloading.** Major sites (Google, Facebook, Instagram, Twitter and many others) are on the HSTS preload list, which is compiled into Chrome, Firefox and Edge. Those never have a cleartext first request, even on a browser that has never visited them, so they are not viable targets at all.
* **max-age.** For non-preloaded sites the protection lasts as long as the header's `max-age`, commonly one year, counted from the last visit and refreshed each time. A user who visits regularly stays protected indefinitely.
{% endhint %}
