---
title: "A Quick Note About Cisco AnyConnect Split-Tunnel Access Lists"
date: 2023-05-03T21:18:05-04:00
draft: false
toc: false
images:
tags:
  - cisco, networking, security, anyconnect, vpn
---

Just a quick note when configuring access-lists for the [split-tunneling](https://www.cisco.com/c/en/us/support/docs/security/anyconnect-secure-mobility-client/119006-configure-anyconnect-00.html#anc9) feature of Cisco AnyConnect VPN Client. In order for the feature to actually *work*, the ACL must be **Standard**, not Extended.

When configuring split-tunneling, you are given the choices of **Tunnel All**, **Tunnel Network List**, and **Exclude Network List**. The latter two options require specifying an ACL for what to tunnel or exclude.

If you mistakenly use an *extended* access-list here, the AnyConnect VPN Client will revert to **Tunnel All** mode, even if you've chosen the **Tunnel** (or **Exclude**) **Network List** option. The ACL must be a *standard ACL*, because in essence, the ACL is just a list of routes to be advertised to the end-user's VPN client to determine what traffic to route through the AnyConnect virtual VPN adapter.

This tidbit brought to you by a few hours of me pulling my hair out over an afternoon thinking I was going crazy, made worse by the ASDM's near-total lack of support for standard access-lists nowadays.
