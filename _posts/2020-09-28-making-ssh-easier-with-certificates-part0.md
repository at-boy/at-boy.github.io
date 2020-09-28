---
layout: post
title: Making SSH easier with certificates Part 0
date: 2020-09-28 17:15
category: linux lab learning ssh certificates
author: at-boy
tags: linux ansible ssh lab network learning certificates
summary: Part of my home lab rebuild is to make it easy to spin up new virtual machines and control who can access them. Since we'll mostly be spinning up Linux machines the primary way of access is SSH and back in 2016 I came across this very interesting article from Facebook https://engineering.fb.com/security/scalable-and-secure-access-with-ssh/
---

Part of my home lab rebuild is to make it easy to spin up new virtual machines and control who can access them.

Since we'll mostly be spinning up Linux machines the primary way of access is SSH and back in 2016 I came across this very interesting article from Facebook https://engineering.fb.com/security/scalable-and-secure-access-with-ssh/

I have played around with SSH user and host certificates a couple of times just to understand how it works and to figure out if it makes sense.

A some what newer article about the subject can be found here https://smallstep.com/blog/use-ssh-certificates/

I highly recommend reading both articles, the explain the use of SSH certificates quite well.

## What do we wish to accomplish
Anybody who works with or have worked with SSH knows the hassel of managing SSH keys, yea you can automate pushing public-keys out with Ansible, Puppet or such, but it's no really easy to manage or control when you scale it up.

This is where SSH user certificates comes in, instead of having to add public-keys to .ssh/authorized_keys you instead configure your SSH daemons to accept certificates signed by a Certificate Authority (CA)

Now such a certificate doesn't just grant access it is possible to configure settings in the certificate that states what precisely this certificates allows for a user to do to a certain degree, which can be quite usefull.

The next part of SSH certificates that is quite useful is host certificates, we have all seen the following message the first time we SSH to a new host

```The authenticity of host 'hostname (1.2.3.4)' can't be established.
ECDSA key fingerprint is SHA256:+QQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQ.
Are you sure you want to continue connecting (yes/no)?```

The point to this message is that you should beforehand have recieved the host key fingerprint so that you can verify that you are connecting to the correct host, the problem often is that most don't verify the key before connecting.

It is a really bad habbit and most of us have been there none of us are perfect, I myself used to just press yes to get on with my work, at first in my career it was because I didn't know better, but at some point i did know better at it was just clean stupidity but that changed about 5 or 6 years ago.

With SSH host certificates we can sign our SSH hosts keys with our CA and tell our clients to trust certificates for a given host or group of hosts that are signed by our trusted CA, this way when we login we know that we have connected to the correct host.

## Software and implementation
For my home lab I want it to be quite easy both to add new users that can gain access to my lab and for the to use certificate based authentification when accessing a give physical or virtual machine, actually not sure if this would work for network devices, time will tell.

So what I have been looking at is HashiCorp Vault and step/step-ca from Smallstep Labs, both are tools with similar functionality which provides a way to implement SSH certificates while also providing other functionality which might come in handy.

I will try to implement both systems and then evaluate on them on the following criterias

- How long does it take to setup

- How easy is it to manage

- How well is it documented

- Does it have an API

- Does it generate useful logs

- Does the same tool solve other tasks