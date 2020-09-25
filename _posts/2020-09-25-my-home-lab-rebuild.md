---
layout: post
title: My Home Lab rebuild
date: 2020-09-25 15:12 +0200
category: linux lab learning
author: at-boy
tags: linux ansible ssh lab network learning 
summary: Going to rebuild my home lab known as labnet this is a quick rundown of the goals and intentions of the project
---

A little over a year ago I moved and in that process I had to take down the small part of my computer lab which I still had running.

The plan was to rebuild it after moving, but now a year later I haven't really done so, I've been using parts of it now and then when needed but it's always quite time-consuming when starting something new or returning to something after a long while.

But during the last 6 months I have been needing it more more, so now I have decided that instead of just booting up a machine or appliance here and there, I want to rebuild my computer lab in a way that makes it easier both to maintain but also start up new projects.

## The Goals
So lets create some goals for this project.

# **Remotely Accessible**
So one thing that I always wanted was for my lab to be accessible remotely, that way instead of spinning up VM's and such on my laptop or other computer at hand, I would be able to access the lab and do it there.

That way my also laptop wouldn't need a lot of ressources unless I'm going somewhere with bad or no internet.

This would need to be done in a way that doesn't compromise my home network in any way!

# **User Privilege and Access Control**
Often it is more fun to do projects together with other people, so we need a way to create new users and control their privileges and what they can access.

# **Separate projects from each other**
A way to keep different projects seperated from each other so that they can't access each others data and such.

This is not something that is strongly needed, but it could be nice.

# **Virtual Machines and Containers**
It would need some kinda platform for running virtual machines and containers, this could either be one platform or two.

The most important part here is that a choosen technology needs to have an API that makes provisioning and deploying new stuff easy.

# **Scaleable, easy to add and remove hardware**
In my original lab it wasn't easy to add new machines or appliances, adding new a machines to the network should be as close to as easy as possbile to just connecting the network cable and power and then the system should figure out the rest.

But it should also be easy to remove something, often in my old lab removing something ment making sure there was backup of important stuff and such, which takes us to the next point.

# **Backup, monitoring and alerting**
There needs to be a functional backup system that take periodic and on-demand backups of everything on a given target in the network and also an easy way to restore something from a backup or access files in a backup.

We need to know that backup is working and running correctly so we need some monitoring and alerting both to know that backup is working but also to know that services and such are running correctly and if they aren't being alerted.

# **Easy/Automatic power-on and shutdown of single devices**
So it's a waste of power to have everything powered on all the time if it is not being used, so it would be nice if it was possible for the system to evaluate what is being used and what is not and then maybe alerting users about shutdown and if users do nothing then shutdown unsed stuff.

In regards to that it could also be nice to have some auto scaling so that forexample a cluster handlng virtual machines could be scaled up if needed or down if ressources are not being used.

To that end it should be easy for users to power-on stuff but also mark something for shutdown and/or termination

## What now
In the next post I'll try to break down some of the goals into tasks and then prioritize them and then hopefully start implementing stuff.