title: swpclib and homeassistant-noaa_space_weather
date: 2022-10-13
category: Meta
author: Tyler Carr
keywords: homelab, meta, development, linux, amateur radio, 
tags: Meta, Development, Amateur Radio
summary: Space weather + Homeassistant 
status: draft

# Introducing swpclib and home-assistant_noaa-space-weather

This is a half baked unveiling of a couple projects I've been working on lately. 
I've been wanting to be able to incorporate space weather data into my homeassistant automations for a while. Mostly out of an interest in understanding things like the Solar Flux Index, A; K indices, and how they relate to the Sunspot number and ultimately -- real world radio propagation. 

In seeking out this data, I realized that there wasn't a python library for retriving information from [NOAA's Space Weather Prediction Center](https://www.swpc.noaa.gov/) (the primary source for such data). A first step to homeassistant integration became creating such a library. 

A home assistant integration will make it easier for me to add these numbers to graphs in graphana, or display them on the pixel display shack clock I intend to build at some point. 

## swpclib

I set out to create some sort of library to interface with the NOAA SWPC (Space Weather Prediction Center) API. In order to be used in Homeassistant, this needed to be an async library. Something I have had minimal experience with. 

[swpclib](https://github.com/tcarwash/swpclib) is what I came up with, it is an async python library that will pull the data I'm interested in from the NOAA space weather prediction center API. It should be pretty easily extensible if there is ever additional data required/desired. 

swpclib also exposes a cli tool `space_weather` that will print some of the data to a terminal, I might make this more interesting/prettier at some point for fun. It should make a fun companion to my terminal logging program pylogcq.

## Home-assistant_NOAA-Space-Weather

[Home-assistant_NOAA-Space-Weather](https://github.com/tcarwash/home-assistant_noaa-space-weather) is the home assistant integration I built, it uses swpclib to pull A-index, K-index, Solar Flux Index, Sunspot Number, 1-Day X-Class flare probability, and 1-Day M-Class flare probability as home assistant sensors. As additional data is made available through swpclib it is easy to add more sensors. 

This is my first home assistant custom component, so it was a learning experience. I started with a cookie-cutter project that was much less up-to-date than I realized, I'm working through the steps I took to modernize the cookie-cutter template so that hopefully I can contribute back upstream. 

The integration is installable as a HACS custom repository or manually as a custom component. Instructions are provided in the readme. 

## Contributing

I am more than happy to accept help on either of these projects. swpclib is where help would be most useful, anyone wanting additional data available from the NOAA SWPC API to be included in swpclib can create a method to return the desired data and submit a PR. 

Both projects really could use tests, I would love help writing those as I don't have much experience with writing good tests. I would also love help getting the custom component up to spec to be included in the HACS default repo. 

If any of this sounds interesting to you, please reach out on github or via email to me at tcarwash@gmail.com


