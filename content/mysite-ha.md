title: Now with more availability  
date: 2021-08-11
category: Meta
author: Tyler Carr
keywords: homelab, ha, high availibility, cloudflare, reverse proxy, linux
tags: Meta, homelab
summary: This site now higher availibility
status: published

Even though it isn't necessary, I decided to add an additional server to my Linode fleet. This server will, among other things, serve a mirror of this site. 

I figured this would be a worthwhile learning excersize, and should provide more uptime which is never a bad thing. 

Now, content is stored in a repo and pulled down, 'pelicanized' and the resulting html copied over to the mirror server, more detail on this [here](https://tyler-carr.com/this-site-continuous-delivery.html). The two VPSs are in different Linode datacenters (US-West and US-Central) and a Cloudflare load balancer sits in front of them to proxy requests. Depending on your geographic location you should be served either out of a nearby Cloudflare cache or the VPS that is nearest to you. 

I can't give you back the time you spend reading my posts... but I can at least save you a millisecond or two. 
