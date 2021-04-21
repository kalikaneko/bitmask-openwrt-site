---
title: "Flashing ⚡"
description: "Flashing a device"
lead: "How to flash your device"
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "guides"
weight: 300
toc: true
---

## How to flash your device

{{< alert icon="👉" text="This could brick your router, or make you lose your configuration. Proceed with care." >}}

Nothing fancy here, really, if you're looking for this I assume you [know your drill](https://openwrt.org/docs/guide-user/installation/sysupgrade.cli). For the moment, I'm working with routers that support the beautiful `sysupgrade`. Otherwise you will have to upgrade via `uboot`, `serial` etc.

Here's a list of the [latest images](https://sindominio.net/kali/openwrt/images/) I've built: look for one
matching your router. At some point in the not-so-distant future, the CI should be in charge of the builds.

- If you have some custom modifications you dont want to lose, you can add them
  a list of files to `/etc/sysupgrade.conf`
- Add the `-n` option for NOT preserving your config files -> It will wipe
  everything, including configs / network etc: `sysupgrade -v -n ...`.

The script I use to flash is something like:

```bash
export IMG=openwrt-ramips-mt76x8-glinet_gl-mt300n-v2-squashfs-sysupgrade.bin
scp bin/targets/ramips/mt76x8/$IMG root@router:/tmp/
ssh root@router "sysupgrade $IMG"
```

And then just sit back and admire the [blinkenlights](https://en.wikipedia.org/wiki/Blinkenlights)...


## Go further

Since you're not afraid of flashing, why not step up and [start hacking→]({{< relref "hacking" >}}) on the project?

