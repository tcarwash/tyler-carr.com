title: DORCBot
date: 2021-10-2
category: Software
tags: Software, Amateur Radio, Homelab
author: Tyler Carr
keywords: Amateur Radio, Ham Radio, Amateurfunk, Radioamador, Radio Amateur, Discord, Python, CI/CD, Github
summary: DORCBot, the Digital Oddballs Radio Club discord chat bot. 
status: published

## My introduction to Chatbots, Asynchronous programming
One of my pet 'projects' is an online community (currently on Discord) called the [Digital Oddballs Radio Club (DORC)](https://oddballs.digital/)

We're a group of hams who are into computers, programming and all kinds of other fun stuff. Being such a group, congregating in an online chatroom, I figured we needed an equally DORCy chatbot.

[DORCBot](https://github.com/tcarwash/DORCbot) sits in our Discord server, and accepts several different commands. DORCBot will tell you what the solar forcast is using the `!solar` command. DORCBot will also give you a list of the 5 most recent DORCs who were spotted/reported on the air by other hams, this uses data from one of my other projects. 

![DORCBot Help Output]({static}/images/db1.png)

DORCBot is meant to be a way for members of the club to bring their development projects into the chatroom.

## Propagation, MOF
One of the neatest things that DORCBot will do uses an API and data model developed by one of our members. Andrew, KC2G's [Propagation Site](https://prop.kc2g.com) gives predictions of useable frequencies over the entire globe, it is an amazing project you should definitely check out.

DORCBot will grab data from Andrew's API to return the Maximum Observed Frequency between two stations. If you have your Discord display name in `name_callsign` format, any time you give a function that normally takes two callsigns as input only one callsign, DORCBot will assume you want to use your own callsign as the other argument, and pull it from your display name. This means that if I set my display name to `Tyler AG7SU` all I have to do to get a prediction of propagation to any other station/grid is issue the command `!mof CALLSIGN` or `!mof GRID`

![DORCBot !mof Output]({static}/images/db2.png)

## Lessons, New Experiences
Before working on DORCBot I had had no experience with Github Actions, CI/CD or working on development projects with other people. Each of these have been great learning experiences for me. 

### CI/CD
DORCBot is a Python program, for ease of deployment it is running inside a docker container. When a commit is pushed/a Pull Request sent to the [Github repository](https://github.com/tcarwash/dorcbot) tests are run and a new docker image is built and tagged with the commit hash. This means that I can pull and test a specific commit/PR seconds after it has been pushed.

When a commit is pushed with a `vX.X` tag to the `main` branch A production docker image is built and tagged with the version number and also with `latest`

Using [Watchtower](https://github.com/containrrr/watchtower) on the server that runs the production instance of DORCBot means that not long after a new production image is built it is in production and available on our Discord server.  

### Chatbots
Before DORCBot, most of my limited programming experience has been with small command line programs, or with web applications, Flask in particular. A chatbot, at least the way I wrote DORCBot, is a lot like a command line application, it mostly ingests/outputs text. Instead of print statements to output text, however, everything has to go through an extra layer, integration with Discord's API. 

## More to Come
As part of being a place for DORCs to bring their projects together/into the chat room, DORCBot is under continued development. New features are being added whenever we think to do so. 
