---
layout:      post
title:       "My Development Environment"
subtitle:    "what I think, what I use, what I do"
description: "The perspective of a software engineer about his development environment."
date:        2015-12-14 13:56:00
author:      "Wander Costa"
header-img: "img/post-bg-mydevenv.jpg"
---

<p>Hi there! This is the very first post of my blog, and I decided to start with my development environment. Need to make it clear that this is my first blog, however I ever wanted to create one. So, here we go...</p>

<p>By the way, it all is only based in my personal opinion and perceptions, so run your own tests to base your project decisions.</p>

<h2 class="section-heading">Hardware</h2>

<p>Nowadays, I have an old laptop, which is an Dell with Intel(R) Core(TM) i5-3337U CPU @ 1.80GHz, 6GB RAM, 1TB HD (not SSD) and a second monitor, with greater resolution to make sure I can run many windows without much effort.</p>

<p>I do have another computer, which is an old desktop, but I rarely use it. I plan to, in a future, use that desktop as a DB server to run some massive tests, since it has a couple of spare HDDs.</p>

<p>I also have an external 2TB network hard drive, that I bought in a work trip, which I use to store my backups and prevent me from losing my projects, docs and pics, as well as it happened some times, some years ago. :sweat:</p>


<h2 class="section-heading">OS</h2>

<p>In 2004 I started working to a government organization, and in 2005 I started running Linux at this organization. Since that time, I got completly immersed into this open source world, which was considerably new stuff for me. The main reason I got really immersed was not just because it's cheaper (sometimes it's not, though), but because of the concept at all: freedom to go deeper. I'm that kind of guy that enjoys more the way than the end... so I like to get deeper and deeper into learning to really understand and dominate it. So Linux turned to be a good system for me.</p>

<p>On my current laptop, I only have Linux installed, which is a <b>CentOS 7</b>. I already used <b>Fedora</b> and <b>Ubuntu</b> with other laptops before, but I like the way CentOS provides me stability that can be replicated when deploying servers. I like to use GNOME, but for some reason I don't really remember quite well I use KDE as interface.</p>


<h2 class="section-heading">Development</h2>

<p>I'm a <b>Java</b> programmer, even though I already programmed in C, Delphi, some Visual Basic, PHP and ASP (old school). So, my favorite IDE is <a href="https://netbeans.org/"><b>NetBeans</b></a>. Currently I use version 8.0.1.</p>

<p>Within Java, I use <b>Jetty</b> and <b>Tomcat</b> as application servers, however I already used JBoss and GlassFish in previous projects. I found Jetty to be a very usefull server, since you can use the standalone runnable version, making things much easier in many different scenarios. Or even with the installed version, it turns to be easier and faster, taking care of only the important components when deploying non-EE web projects (web services, for example).</p>

<p>My prefered database system is MySQL, but I'm also learning MongoDB. Of course, it's not a matter of comparison, as I cannot compare bananas with apples. I also use MySQL Workbench to make database management a little easier, but most of my interactions are done through command-line indeed.</p>



<p>I also use <b>Subversion</b> and <b>Git</b> for versioning, but I'm new with git protocol.</p>


<h2 class="section-heading">IaaS</h2>

<p>As I'm running some different projects, I also have some virtual machines over the internet, from Amazon AWS and also DigitalOcean. For computing, I found DigitalOcean cheaper, simpler and with excellent performance. Amazon AWS plan at the same price level has performance limitations that turn to be a pain when reached. However, for relational database, Amazon AWS RDS performs much better if compared to a self-managed RDBS deployed with DigitalOcean.</p>
