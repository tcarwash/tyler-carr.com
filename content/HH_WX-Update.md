title: Hamshack Hotline Weather Alert (update)
date: 2022-6-26 11:30
category: Software
tags: Homelab, Amateur Radio, Software, RTL_SDR
author: Tyler Carr
summary: An update to a previous post about streaming weather radio audio to Asterisk. 
keywords: Amateur Radio, Radio Amateur, Radioamador, Amateurfunk, Hamshack Hotline, Python, NOAA
status: published


In my last post ([Hamshack Hotline Weather Alert]({filename}/HH_WX.md) ... in October, yikes!) I described the way I set up an extension on my asterisk server to play a stream of the local weather broadcast. 

Since putting this system together, I've moved from a stream hosted online to one captured locally using an rtl_sdr dongle. 

## Set-Up

This replaces the 'Find a weather broadcast stream' part of my previous post, instead of using the URL of an external weather broadcast stream, I'm creating my own. 

### RTL_SDR Dongle and Antenna
I'm using a generic rtl dongle, these are a product originally intended to be used as a TV tuner for a market outside the US. Someone created drivers that allow it to be used as a fairly decent SDR (Software Defined Radio) to capture RF signals the original designers of the hardware never intended.

My rtl dongle is plugged into my server and passed through to one of my more multi-purpose VMs. The rtl gets a crudely made 2ish meter dipole made with a bnc to banana plug adapter, this is hung vertically down the back of a cabinet. 

### Software
This setup requires `icecast2`, `ezstream`, `lame`, and `rtl_sdr` to be installed

A script named wxstream.sh is placed in my `/usr/local/share/` directory with contents:

```
#!bash
/#!/bin/bash
rtl_fm -f 162.550m -M fm - 2> /dev/null | lame -r -s 24 -m m -b 24  --cbr - - | ezstream -c /home/tyler/kig98.xml
```

The rtl_fm command can be edited to change frequency to listen to whatever you're interested in capturing.

The xml file referenced in the previous script looks like this:

```
<?xml version="1.0" encoding="UTF-8"?>

<ezstream>

  <servers>
    <server>
      <hostname>127.0.0.1</hostname>
      <password>streampass</password>
    </server>
  </servers>

  <streams>
    <stream>
      <mountpoint>/KIG98.mp3</mountpoint>
      <format>MP3</format>
    </stream>
  </streams>

  <intakes>
    <intake>
      <type>stdin</type>
    </intake>
  </intakes>

</ezstream>
```
This could be named/edited for whatever makes sense for whatever stream is being captured. 

A systemd service is created as /etc/systemd/system/wxstream.service with contents:

```
[Unit]
Description=KIG98 Stream
Wants=icecast2.service

[Service]
Restart=always
ExecStart=/bin/bash /usr/local/bin/wxstream.sh

[Install]
WantedBy=multi-user.target
```
Both icecast2.service and wxstream.service are enabled at startup in systemd. 

With all this running, I have a mp3 stream of the live capture of KIG98 available at http://myserver:8000/KIG98.mp3 to which I can point an extension on my PBX. 

