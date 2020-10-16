---
layout: post
title: Setting Up a router and DNS server with CentOS Part 0
date: 2020-10-16 16:00
category: linux lab learning dns centos router
author: at-boy
tags: linux lab learning dns centos router bind
summary: Last entry we talked a little about SSH and certificates, but before we actually get to that we need to have a functioning network, to start with this will all be virtual just to test stuff out, we'll need a router that can router traffice between the internet and our network, and then we'll setup a DNS both to make some things easier but also because we'll need it later.
---

Last entry we talked a little about SSH and certificates, but before we actually get to that we need to have a functioning network, to start with this will all be virtual just to test stuff out.

So we'll need a router that can route traffice between the internet and our network, and then we'll setup a DNS server both to make some things easier but also because we'll need it later.

My choice of Linux distribution for this is for now CentOS 7, it is the plan for the finished labnet to support different OS'ses.

Because redundancy is always key, I'll create 2 DNS servers 'dns01' and 'dns02', a  master and a slave, the router will be called 'firewall' because it's also a firewall but for now it will be SPOF (Singel Point of Failure) which i acceptable because this is just a test setup.

So what we should end up with is something looking like this
```
            +----------+
            | firewall |
            +----------+
                 |
                 |
       +--------------------+
       |       switch       |
       +--------------------+
         |       |        |
         |       |        |
 +-------+  +----------+  +-------+
 | dns01 |  | server01 |  | dns02 |
 +-------+  +----------+  +-------+
```
Server01 is just a CentOS 7 configured with our two DNS servers as it's DNS servers to use, the network will not be configured with DHCP so everything will be staticly setup.

Switch in the diagram shown is just a bridge I created to connect all the internal network to.

The machine I'm running this setup on is at the moment running Ubuntu 20.04 which uses 'netplan' for network stuff, so to create the bridge all I had to do was edit the file '/etc/netplan/01-network-manager-all.yaml' to make it look likes this

```
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  bridges:
    testnet:
      dhcp4: no
```

Depending on your setup and choice of OS you might need to do this in some other way.

Next time we'll continue and actually setup the Firewall/Router