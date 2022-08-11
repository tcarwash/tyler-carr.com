title: My Homelab
name: homelab
date: 2021-06-21
category: Homelab
author: Tyler Carr
keywords: homelab, development, Dell, r610, freebsd, pfsense, linux
tags: homelab
summary: A description of my homelab.
status: published

I've been experimenting with self hosting, and hosting at home for several years now. My 'lab' has evolved significantly throughout the years, starting with the cheapest VPS I provider I could find I started learning things like remote access via ssh and basic security (often the hard way). 

Eventually my lab moved into my home and onto an old roadkill laptop.  I started learning about docker, reverse proxies, firewall rules and networking. Services in this era of my lab consisted of 'off the shelf' containers and some applications I wrote myself. Lots of Python, Flask and Docker. This 'server' (Carr-Lab1) is still running but hosts very little now. 

In April of this year (2021) I convinced my wife to let me buy some old enterprise gear. This newest iteration of my lab consists of a second hand Dell Poweredge r610 [2x 6 core CPUs and 32GB of ram] running Proxmox as a hypervisor. With this new (to me) machine, I'm loving the hands on experience I'm getting with gear I don't normally get to play with at work. Services running on this machine are Homeassistant ('supervised' version in it's own Debian VM), hastebin, hedgedoc, plex, and babybuddy (we have our second daughter, Evelyn, on the way in July). A 5 node K3S kubernetes cluster is running with rancher as it's only current workload. I run a virtualized instance of pfSense on the server as well, which lets me better isolate lab VMs and reduces how much configuration I do on my main network in support of the lab. 

My home network, which is also a semi-experimental component of my lab, runs behind a bare-metal installation of pfSense on netgate hardware (recently replacing a **janky** install on an atomic pi *freeBSD support for realtek NICs bit me*). I run VLANs for my main LAN (trusted devices), a guest LAN (untrusted, throttled), and a LAN for my IOT devices (**REALLY** untrusted, severely throttled). Due to where I live (rural), bandwidth is limited (3mbps/1mbps), throttling the IOT devices keeps an untimely update from interrupting youtube or other browsing. An app I wrote re-rolls displays and updates the guest LAN passphrase on my Aruba AP.

