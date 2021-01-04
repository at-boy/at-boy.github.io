---
layout: post
title: Setting Up a router and DNS server with CentOS Part 1
date: 2021-01-04 15:26
category: linux lab learning dns centos router
author: at-boy
tags: linux lab learning dns centos router bind
summary: Last entry was a quick overflight of the plan for setting up a router/firwall and DNS server, now let's acutally do it, we'll start with the router/firewall and when that is done we'll move on to the DNS server.
---

Last entry was a quick overflight of the plan for setting up a router/firwall and DNS server, now let's acutally do it, we'll start with the router/firewall and when that is done we'll move on to the DNS server.

What we want to make is a setup that should look like this

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

## Setting up Firewall/Router
Our VM 'firewall' have two network interfaces 'eth0' wich is connected to the "outside" meaning it can talk to the the rest of my network and the internet and then there is 'eth1' which is connected to or bridge which simulates or internal network.
That way everything conected to the bridge have to go through 'firwall' to access things outside of the network.

First of we will enable IP forwarding, this is to allow network traffic to travel between different networks, which is done with the following command.

```
sysctl -w net.ipv4.ip_forward=1
```

This command enables IP forwarding here and now but if we restart it will not work anymore, so to make this permanent we will update or create the file '/etc/sysctl.d/ip_forward.conf' and make sure it contains the following.

```
net.ipv4.ip_forward = 1
```

The last thing we need to do on 'firewall' is adding some firewall rules, depending on your choice of OS how to do this might differ, but on CentOS 7 the easy way to controll your firewall settings is using the build in and already activated firewalld.

I'm not a networking expert so the following is my understanding of how stuff works, it may be wrong if so please let me know.

First of we need to do so traffic from the internal network that needs to reach the internet is changed so the source of the traffice look as if it comes from 'firewall', we are creating a NAT network, no rememeber NAT is not security and we could maybe do this another way, but this is the way I know. 

```
firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o eth0 -j MASQUERADE -s 10.0.0.0/24
```

I have choosen the IP-range '10.0.0.0/24' for the internal network, and '-s 10.0.0.0/24' is just limiting the firewall rule to only apply to packets from that range.

If we try to ping and external IP from 'server01' now that would work, but if we try to do an HTTPS request that would fail, we need to allow replies to be forwarded back to the internal network. 

```
firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -i eth1 -j ACCEPT
```

Now from 'server01' we can talk with the rest of the world, an important notice here, right now server01 is configured to use '91.239.100.100' as its DNS server because we haven't setup our own internal DNS server yet.

One thing we need to consider also is if we want to allow all kinds of traffic out of our network or not, right now the only way to get in is to SSH from the outside to 'firewall' and use that as a Jumphost/Bastion to connect to the internal servers/devices.
But from the inside of the network, right now anything is allowed to go to the outside, right now because this isn't the "final product" it's not super important, but something we need to keep in mind.

Next time we will take a look at actually setting up the internal DNS server(s) for our network.