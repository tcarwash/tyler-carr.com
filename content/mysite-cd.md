title: This site -- Continuous Delivery
date: 2021-08-10
category: Meta
author: Tyler Carr
keywords: homelab, meta, development, CI/CD, linux
tags: Meta, CI/CD, Development
summary: A new category, about this site's continuous delivery
status: draft

## A new category of content -- on this site 
Because this site is as close as anything I work on comes to 'production,' I've decided that a new category is in order.

"Meta" will be a place for articles/posts directly related to this site. I'm hoping to showcase some of the techniques I'm using to deliver this site.
This will sometimes look like a changelog, and it will sometimes just be explanation of a way I decided to go about something, or it may be something else completely.

This might also come with a more conversational tone and shorter, more frequent posts... We'll see. 

## A little about Pelican
This site uses a python program called [Pelican]('https://github.com/getpelican/pelican') as a static site generator. Pelican, so far, is really great. 
I like that it uses tooling that I'm familiar with (Python, Jinja2, Markdown) from flask and other projects. This has made things like [customizing themes]('https://github.com/tcarwash/blue-penguin-dark') a lot easier.
I chose Pelican after looking specifically for a static site generator using tooling I would be comfortable with, hoping that I could customize; tweak; and potentially contribute to the project I chose. 

Pelican takes content written in markdown (or restructured text) and converts it into sane and well organized html, applying a theme in the process. 
The simple/procedural nature of the process allows for all of the content creation for this site to be done in a terminal text editor (vim right now) and for site generation and deployment to be automated. 

## Continuous Delivery at tyler-carr.com
Until recently, this site has lived on a single Linode VPS (migration/epansion should be the topic of a post soon). Content was served directly out of a sub-directory of where it was created, This was nice and simple and could be done over ssh mostly without issue, but with the slow (read: slooooooow) connection at my house writing content could be kind of a pain sometimes. With this site being served from more than one server now, and behind a load balancer (for learning... not out of necessity) syncronization of content became more of an issue. I'm also a pretty big fan of automation and really interested in CI/CD so I decided to try to come up with a slightly better solution

What I came up with is a to put the content directory (raw markdown) in a github repository. This has a few benefits, one is that it allows me to edit copy out-of-band so to speak, I can create blog posts regardless of what my WAN connection is doing at any given point. It also will be a single source of 'truth' for my site content. I run a development mirror of this site sometime in order to try larger changes, and I like it to have the exact same content as the main site.  

A cron job pulls the main branch of the repo, runs pelican over it and then distributes the output files to the mirror server(s) 

I think this setup will allow me to work on content more regularly and in more ways. 


[[[[ CONTINUE ]]]]
