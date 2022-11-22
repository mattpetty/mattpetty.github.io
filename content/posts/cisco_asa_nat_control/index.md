---
title: "Cisco_asa_nat_control"
date: 2022-11-07T13:46:18-05:00
draft: true
toc: false
images:
tags:
  - untagged
---


## Introduction

The history of **NAT** ([network address translation](https://en.wikipedia.org/wiki/Network_address_translation)) behaviors on Cisco's ASA firewalls has certainly... *evolved* over the years. I'll be covering the basics of specific NAT types, and how they've changed, in a future blog post, but today I want to present a slightly obscure, lesser understood NAT feature of ASA firewalls, one that has perhaps been more changed by time than any other NAT feature.

## NAT Control

Enter **NAT Control**, which over the course of its lifespan went from being a required default, to an optional feature, to ultimately being retired and disabled altogether. Not every firewall engineer knows about NAT control, but like every major change to NAT behavior, it can have a significant effect on your mental model of how you might expect the firewall to function.

## Evolution

Beginning with the predecessor to the ASA, the [Cisco PIX firewall](https://en.wikipedia.org/wiki/Cisco_PIX) *required* NAT for any traffic passing between interfaces (or a NAT exemption), up through the maximum PIX OS version 6.3. Once Cisco converted both the PIX and ASA firewalls to the newer ASA codebase starting with 7.0(1), the **nat-control** feature became available so that this default NAT behavior could be enabled or disabled. Once disabled, there doesn't need to be any NAT configuration in place for traffic to pass between the firewall's interfaces, as long as existing access-lists, security-levels, and routing entries permit that traffic to do so. (granted, if you're considering the common scenario of an internet-edge facing firewall that's NATing private IP space to public, you'll still need to NAT anyway. Or move to IPv6 already!!).

So, what is NAT control, exactly? Beginning with the predecessor to the Cisco ASA firewall, the Cisco PIX (link)‘s default (and unchangeable) behavior was to *require* network address translation for *all* traffic passing between any two firewall interfaces. An IP packet could not simply pass through the firewall untouched unless a specific NAT exemption was created (essentially, a rule saying “don’t NAT this specific traffic”).

Meme: One does not simply pass through NAT-control

With the introduction of the Cisco ASA and version 7.0(1) OS code in 2005, NAT-control became a configurable option.

By mid-2010, 8.3(1) released & disabled entirely

## Displacement

By the time Cisco made the massive NAT reboot of the ASA 8.3 code train around mid-2010, nat-control was no more. That release marked the end of the availability of the **nat-control** feature and it was no longer even a configurable option.
