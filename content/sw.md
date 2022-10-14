title: This site -- Continuous Delivery
date: 2021-08-10
category: Meta
author: Tyler Carr
keywords: homelab, meta, development, linux, amateur radio, 
tags: Meta, Development, Amateur Radio
summary: Space weather + Homeassistant 
status: draft

## Introducing swpclib and homeassistant-noaa_space_weather

This is a half a**ed unveiling of a couple (one medium sized) project I've been working on lately. 
I've been wanting to be able to incorporate space weather data into my homeassistant automations for a while. Mostly out of an interest in understanding things like the Solar Flux Index, A; K indices, and how they relate to the Sunspot number and ultimately -- real world radio propagation. 

In seeking out this data, I realized that there wasn't an existant open-source python (or otherwise) library for retriving information from NOAA (the primary source for such data). A first step to homeassistant integration became creating such a library. 

## swpclib

I set out to create some sort of library to interface with the NOAA SWPC (Space Weather Prediction Center) API. In order to be used in Homeassistant, this needed to be an async library. Something I have had minimal experience with. 


