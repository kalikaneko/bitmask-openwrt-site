---
title: "Troubleshooting 🔧"
description: "Solutions to common problems."
lead: "Solutions to common problems."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "help"
weight: 620
toc: true
---

## Expect it to crash

I'm more or less happy with the status of the current release, and I'm using it
daily between my laptop and the rest of the world. But I've already seen it
getting stuck and needing manual intervention: nothing that cannot be solved by
logging in to the router and restarting the program.

With the firewall I'm shipping as an example, the VPN fails close, so I'm not
too worried about risks, but please use it with care.

Beyond debugging the cause for the crashes and improving retries, I think
a simple external watchdog that monitors the traffic through the tun interface
can solve most of the issues.

## Config file woes

Try deleting `/etc/bitmask/bitmask.cfg`, it will re-create it in the next run.

## Logs

You can increase `openvpn` verbosity by passing a higher integer to bitmaskd:

```bash
DEBUG=4 /usr/bin/bitmaskd
```

But beware of the huge file in `/tmp/bitmask-openvpn.log`.

