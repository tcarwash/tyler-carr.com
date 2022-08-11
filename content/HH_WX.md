title: Hamshack Hotline Weather Alert
date: 2021-10-11
category: Homelab
tags: Homelab, Amateur Radio, Software
author: Tyler Carr
summary: A method for streaming weather radio to an Asterisk extension and update BLF to display notifications.
keywords: Amateur Radio, Radio Amateur, Radioamador, Amateurfunk, Hamshack Hotline, Python, NOAA
status: published

## Introduction
### Hamshack Hotline
Hamshack Hotline (HH) is a SIP phone system set up by a group of Amateur Radio Operators (hams). There are different use cases for HH, for me it is an excuse to run a PBX at home and play with phones.

### My PBX
I run an instance of FreePBX in a VM on my main server (Dell R610) FreePBX is CentOS + Asterisk + a nice web UI.

Some of what I did to build my Weather Alert extension is done from the UI and some is done by manipulating Asterisk's config files directly.  

### Goal
My goal was to create an extension button on my hard phones (desk sets) that would play a stream of the local NOAA Weather Radio broadcast. I wanted the LED controlled by the Busy Lamp Field (BLF) for that button to light up with a specific pattern if there is a weather alert for my area. This would give me visual indication of potential hazardous weather affecting the area, and one press access to the weather broadcast to listen to.

## How-To
For this how-to I will assume you are running FreePBX, I'm on version 15.0.17.55

## Set up Music On Hold (MoH), queue and extension
- Find a weather broadcast stream
    - I found mine by inspecting the player element on [weatherusa.net/radio](https://www.weatherusa.net/radio)
    - If a station nearby you is in the list on [weatherusa.net/radio](https://www.weatherusa.net/radio) you *may* be able to use this link: https://radio.weatherusa.net/NWR/KIG98.mp3 substituting `KIG98` with the callsign of the NOAA station near you
    - NOAA doesn't release their own audio streams of broadcasts, availability of streams depends on someone setting up a listening station to record and stream the broadcast. Look around, and hopefully you can find something useful. 
    - *Steve [K4HG](https://www.qrz.com/db/k4hg) shares how to [set up a stream useable for MoH captured directly from an rtl-sdr](http://www.findu.com/rtl-moh.txt)*
    - *If you're having issues (no audio, errors in asterisk logs) make sure you are accessing your audio stream via HTTP not HTTPS, mpg123 appears to have trouble with HTTPS*

- Create a MoH Category
    1. From your FreePBX Admin page navigate to Settings > Music on Hold
    2. Click Add Category
    3. Give the Category a name `Weather` is what I chose
    4. Choose `Custom Application` for Type
    5. Click Submit
    6. Edit the newly created Category
    7. Input `/usr/bin/mpg123 -q -s --mono -r 8000 -f 8192 -b 1024 YOUR_STREAM` substituting your stream url for `YOUR_STREAM`
- Create a queue
    1. From your FreePBX Admin page navigate to Applications > Queues
    2. Click Add Queue
    3. Give the Queue a number and a name
    4. Select the MoH category you created above for 'Music On Hold Class'
    5. Choose 'MoH Only'
- Create an extension
    1. From your FreePBX Admin page navigate to Applications > Extensions > Virtual Extensions
    2. Click 'Add New Virtual Extension'
    3. Choose an unused extension number for 'User Extension', I chose `6000`
    4. Choose a 'Display Name', I chose `WX Station`
    5. On the Find Me/Follow Me tab chose the following options:
        - Enabled: Yes
        - Initial Ring Time: 0
        - Ring Time: 0
        - Destinations:
            - Queues: QUEUE_YOU_CREATED_ABOVE
- Test
    - Calling the extension you created above, you should hear the audio stream you passed in when you created the MoH Category

### Edit Config Files, Create Custom Device State
- Open `/etc/asterisk/extensions_custom.conf` for editing
- add the following lines:
```
#!bash
[ext-local-custom]
exten => UNUSED_EXTENSION,hint,Custom:DEVICE
    same => 1,Goto(from-internal,ABOVE_EXTENSION,1)
```
Substitute:
    - UNUSED_EXTENSION: Another unused extension
    - DEVICE: Create a name for the Custom Device State
    - ABOVE_EXTENSION: The Extension you created in the above section

*example from my PBX:*
```
#!bash
[ext-local-custom]
exten => 101,hint,Custom:WXAlert
    same => 1,Goto(from-internal,6000,1)
```

### Install/Set-up control script
The control script queries NOAA's API for active alerts for a selected 'zone'
```
#!console
usage: wxalert [-h] -z ZONE -d DEVICE

optional arguments:
  -h, --help            show this help message and exit
  -z ZONE, --zone ZONE  NOAA Weather Zone
  -d DEVICE, --device DEVICE
                        Custom device state to target
```
If there is an active alert it will set the state of the device passed into it with the `-d` flag to an appropriate state for the highest severity of active alerts.

Source code for the control scipt can be found [here](https://github.com/tcarwash/asterisk_wxalert)

The fastest way to install the control script is to `sudo pip install asterisk_wxalert`

Once installed you can add an entry into the root user's crontab so the script runs regularly:
```
#!bash
*/5 * * * * /usr/bin/wxalert -z ZONE -d DEVICE
```
*example from my PBX:*
```
#!bash
*/5 * * * * /usr/bin/wxalert -z WAZ039 -d WXAlert
```

Your NOAA Forecast Zone can be found on [this site](https://www.weather.gov/pimar/PubZone)

It will be in the form WAZ039 (2 Letter state, The letter Z, then the 3 digits from the map of your state above)

### Phone Set-Up
Phone set-up will differ between phones, but for the most part, pointing one of your BLF buttons at the extension created in `/etc/asterisk/extensions_custom.conf` *in my case `101`* should get you the desired behavior. The BLF LED will illuminate with the status set by the control script, and pressing the button will call the extension that immediately forwards to the MoH Queue.

## Conclusion
Hopefully this gets you at least part of the way toward a working implementation of this system. I will treat this as a living document, and will update it based on my own experiences and feedback from anyone who tries to follow the directions. Feel free to email me at the address listed on my QRZ page, or message me on the HH Discord server. 

Thanks and 73, Tyler - AG7SU
