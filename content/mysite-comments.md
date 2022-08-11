title: Comments powered by ISSO 
date: 2021-08-11
category: Meta
author: Tyler Carr
keywords: homelab, linux
tags: Meta, homelab
summary: A new site feature -- Comments! 
status: published

You can now submit comments when viewing an individual post. I wanted to add this just in case questions came up about a post, or if there were any corrections needed. 

Comments are powered by a project called [isso](https://isso-comments.de) which is a self-hostable alternative to something like [disqus](https://disqus.com)

ISSO is running in a docker container on my main Linode instance. Because it uses a database and is not 'mission critical' and fails pretty gracefully I don't have it duplicated on mirror server(s). The system was pretty easy to set up, just a docker-compose file and a couple lines added to one of the templates in my pelican theme. 

Leave a comment and tell me you're reading and we can see how this works! 
