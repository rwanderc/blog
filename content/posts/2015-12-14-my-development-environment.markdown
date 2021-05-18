---
shorturl:    "http://goo.gl/Zgzct3"
layout:      post
url:         "/my-development-environment/"
title:       "My Development Environment"
subtitle:    "what I think, what I use, what I do"
description: "The perspective of a software engineer about his development environment."
date:        2015-12-14 13:56:00
author:      "Wander Costa"
header_img:  "/img/post-bg-my-development-environment.jpg"
thumb_img:   "/img/post-thumb-my-development-environment.jpg"
tags:
- software development
- environment
---

Hi there! This is the very first post of my blog, and I decided to start with my development environment. Need to make it clear that this is my first blog, however, I ever wanted to create one. So, here we go...<!--more-->

By the way, it all is only based on my personal opinion and perceptions, so run your own tests to base your project decisions.

# Hardware

Nowadays, I have an old laptop, which is a Dell with Intel(R) Core(TM) i5-3337U CPU @ 1.80GHz, 6GB RAM, 1TB HD (not SSD) and a second monitor, with a greater resolution to make sure I can run many windows without much effort.

I do have another computer, which is an old desktop, but I rarely use it. I plan to, in a future, use that desktop as a DB server to run some massive tests since it has a couple of spare HDDs.

I also have an external 2TB network hard drive, that I bought in a work trip, which I use to store my backups and prevent me from losing my projects, docs and pics, as well as happened some times, some years ago. :sweat:

# OS

In 2004 I started working to a government organization, and in 2005 I started running Linux at this organization. Since that time, I got completely immersed in this open source world, which was considerably new stuff for me. The main reason I got really immersed was not just because it's cheaper (sometimes it's not, though), but because of the concept at all: the freedom to go deeper. I'm that kind of guy that enjoys more the path than the end... so I like to get deeper and deeper into learning to really understand and dominate it. So Linux turned out to be a good system for me.

On my current laptop, I only have Linux installed, which is a **CentOS 7**. I already used **Fedora** and **Ubuntu** with other laptops before, but I like the way **CentOS** provides me the stability that can be replicated when deploying servers. I like to use **GNOME**, but for some reason, I don't really remember quite well when I started using **KDE** as an interface.

# Development

I'm a **Java** programmer, even though I already programmed in C, Delphi, some Visual Basic, PHP and ASP (old school). So, my favorite IDE is **[NetBeans][netbeans]**. Currently, I use version 8.0.1.

Within Java, I use **Jetty** and **Tomcat** as application servers, however, I already used JBoss and GlassFish in previous projects. I found Jetty to be a very useful server, since you can use the standalone runnable version, making things much easier in many different scenarios. Or even with the installed version, it turns to be easier and faster, taking care of only the important components when deploying non-EE web projects (web services, for example).

My preferred database system is MySQL, but I'm also learning MongoDB. Of course, it's not a matter of comparison, as I cannot compare bananas with apples. I also use MySQL Workbench to make database management a little easier, but most of my interactions are done through command-line indeed.

I also use **Subversion** and **Git** for versioning, but I'm new with git protocol.

# IaaS

As I'm running some different projects, I also have some virtual machines over the internet, from Amazon AWS and also DigitalOcean. For computing, I found DigitalOcean cheaper, simpler and with excellent performance. Amazon AWS plan at the same price level has performance limitations that turn to be a pain when reached. However, for a relational database, Amazon AWS RDS performs much better if compared to a self-managed RDBS deployed with DigitalOcean.

[netbeans]:http://netbeans.org
