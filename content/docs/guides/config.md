---
title: "Configuration ⚙️"
description: "How to configure BitmaskVPN."
lead: "Common configuration options."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "guides"
weight: 190
toc: true
---


## Config file

You can configure the server adding options to `bitmask.cfg`, either on the
same folder as the binary if you're doing tests, or in a system-wide folder in `/etc/bitmask/bitmask.cfg`. 

### Location selection

You can select the `auto` algorithm to select gateway, or pick your preferred exit
location.

```
[Locations]
auto=false
preferred=montreal
```

You can get a list of valid locations with:

```
curl localhost:8080/locations
```

### Use Openvpn Service

Use openvpn service to start and stop the vpn.

```
useService=True
```

This is the preferred way. The other option, spawning a tread of our own, will be deprecated soon.


### Change Provider

RiseupVPN is the default and what I'm mostly testing against ([please donate!](https://https://riseup.net/vpn/donate)). You can also use the VPN by [The Calyx Institute](https://calyxinstitute.org/):

```
[Providers]
default=calyx
api="https://api.calyx.net:4430"
ca="https://calyx.net/ca.crt"
```

But please do note that calyx only has one gateway by the time being. Be responsible.


### Use Tor

If you have `tor` and `torsocks` installed in the router, you can use the Tor
network to fetch configuration and certificates.

{{< btn-copy text="useTor=true" >}}
```
useTor=true
```

## Environment vars

I'll be using some random environment variables to try out things that are not
too mature, but use them at your own risk.

```bash
UDP=1 /usr/bin/bitmaskd
```
