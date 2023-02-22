title: Chromebook Cluster
date: 2022-6-26 13:10 
category: Homelab
author: Tyler Carr
keywords: Cluster, Chromebook Cluster, Dell Chromebook 11, Homelab 
tags: Homelab, Docker, Cluster, Chromebook Cluster
summary: A description of the Chromebook 11 cluster I built using roadkill chromebooks. 
status: published

![My Chromebook Cluster](https://content.ag7su.com/file/ag7su-web/ccluster-1.jpg)

Today I'm finally going to get around to describing the Four node chromebook cluster I built last year. I had always been interested in clusters, especially clusters built out of single board computers. Due to the price of amassing several machines to cluster, however, it never seemed all that viable of a thing to try. 

## The Minimum Viable Chromebook
Last year some time, I came across several Dell Chromebook 11s that were being tossed. These devices had broken screens/keyboards/etc, and were no longer supported by ChromeOS. Because of this they were not worth fixing to be sold, and had no usefulness left for their intended purpose. 

![Inside view of Dell Chromebook 11](https://content.ag7su.com/file/ag7su-web/ccluster-4.jpg)

These Chromebooks (Dell CB1C13) have the following specs:

- Intel Celeron 2955U
- 4GB Ram
- 16GB SSD


My idea to build a cluster out of these started with an attempt to find what I called the 'Minimum Viable Chromebook.' I started stripping parts off of a Chromebook motherboard until it wouldn't boot anymore. With this model of Chromebook it turned out that, unlike some other models, the battery; keyboard; daughter-board; screen; and touchpad (basically everything except the motherboard) were all not necessary for the device to boot. These devices are actively cooled by a fan/heat pipe, and all of their connections once the daughter-board is removed are on one side, making them even more enticing for service in a cluster. 
![Bare Chromebook motherboard](https://content.ag7su.com/file/ag7su-web/ccluster-3.jpg)

With all extraneous parts removed from my first subject (read: victim) I started working on installing linux on the device. There are really great instructions avavailable for this, making it a pretty simple process. The full set of instructions can be found on the [Arch Wiki](https://wiki.archlinux.org/title/Dell_Chromebook_11) but it basically goes as follows:

- Enable developer mode
- Become root
- Patch the BIOS
- Boot installation media and install as normal, some hardware may not work... some hardware may not be present

I opted to install Arch linux on this first machine because I wanted to have a lightweight system and I had dotfiles already created to make a comfy but minimal environment. I dubbed this system the minimum viable chromebook, or Minnie for short. 

## When life gives you chromebooks... Cluster them!
We ended up having a lot of these boards going into the dumpster. Since I was having fun installing linux on them, and I do one in a lunch break, I decided these might be my opportunity to build/experiment with a cluster. 

Going with the theme set by Minnie soon I had Mickey, Goofy and Pluto. 

The boards were all stripped, and their cooling fans mounted directly to the motherboard reusing screws and standoffs from the case it came out of. The boards were mounted to eachother using brass standoffs and screws.

![Chromebook cluster cooling/mounting solution](https://content.ag7su.com/file/ag7su-web/ccluster-2.jpg)
### Lets talk about the layout
Now that we're talking about four linux machines, we should start talking about network layout. 

Since I did this work on my lunch breaks at my desk, and I didn't want this to actually be on our work network, I had to come up with a way for these machines to be able to be networked and to have access to the internet for updates/software downloads. 

Minnie, being the first machine, became the controller node. Minnie had a comfy/lightweight desktop environment that made it a really nice entry point to the cluster. My solution to the networking issue is as follows:

1. All nodes get a USB NIC
2. Minnie gets a makeshift WiFi antenna (Ham radio trick, dipole made from the coax) and connects to the hotspot on my android phone 
3. Minnie runs a DHCP server on it's ethernet interface with NAT Masquerading
4. Static leases and hostfile entries for each of the other nodes

Now all the nodes need is power, and a connection to a network switch in order to be networked with the others and also have access to the internet. This makes expansion of the cluster REALLY simple. Provision a node, plug it in, give it a static lease, done.

Software/OS provisioning on these devices is just like any other linux cluster. I went with what I think is a pretty standard beowulf cluster using Open MPI. I also ran these nodes in a docker swarm so workloads could be set up either way. 

## Okay... but what does it do?
I have no need for a cluster. 

The whole point of this project was, because they were there and because I could. As such, I never really had any interesting workloads to run on this thing. The only workload that ever got deployed was a Monte Carlo Pi Estimation that served as a demonstration of "Look how bad the estimation is with one node, now it's not as bad with four nodes."

The cluster is now an 8-core 16GB of RAM paperweight, an interesting proof-of-concept though!

