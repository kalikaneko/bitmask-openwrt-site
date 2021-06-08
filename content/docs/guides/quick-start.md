---
title: "Quick Start"
description: "How to get your VPN up and running."
lead: "How to get BitmaskVPN running in your router."
date: 2021-1-16T13:59:39+01:00
lastmod: 2021-1-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "guides"
weight: 150
toc: true
---

## Warning: beta! ☠️

{{< alert icon="👉" text="This project is in beta stage. Tinkerers welcome!" >}}

At the current state of the project, I will assume that you are familiar with a command line, and you can
find your way installing and configuring [OpenWRT](https://openwrt.org) in your
router — and, potentially, recovering from an accidental bricking 😬.

At a later phase, I want to add support for [LuCI](https://openwrt.org/docs/techref/luci) and/or ship our own web
interface. [Roadmap→]({{< relref "roadmap" >}})

If you are a developer, you might be interested in the [hacking→]({{< relref "hacking" >}}) section.

## Supported devices

The list of supported devices is quite modest in the beginning: I want to get 
first to a point in which the app is robust and reliable, and is able to
gracefully recover from failures.

This tiny list can  hopefully change with some community support. If there is
some popular demand, I can try to make a "blind" build for your architecture,
but there are certain details (like the specifics of leds or buttons) that will
need some assistance.

At some point, an attempt will be made to shave all the possible space to get
this into more size-constrained devices. We'll cross that bridge when we get there.

* [gl-inet mt-300n-v2](https://openwrt.org/toh/hwdata/gl.inet/gl.inet_gl-mt300n_v2)
* [gl-inet ar-750](https://openwrt.org/toh/hwdata/gl.inet/gl.inet_gl-ar750)

## Plug the internet cable

For the moment, the configuration files expect a link to the upstream internet
in the WAN port.

In the pre-configured images, the router gives you an address in the
192.168.1.0/24 range, and it's reachable at 192.168.0.1. When a successful
tunnel is formed, traffic will be routed through the tunnel interface.

## Install

If you are using a flashable image, you already should have the latest binaries in place. Otherwise, you need to add a custom feed to install `bitmask-vpn` with `opkg`.

### 1. Enable TLS

Note that you need [TLS support enabled in wget](https://www.leowkahman.com/2016/04/10/use-ssl-openwrt-opkg/) at the
moment, I might move the feed somewhere that doesn't redirect to https.

```bash
opkg update
opkg install wget
opkg install ca-certificates
opkg install libustream-openssl
```

### 2. Add feed

Edit your `/etc/opkg/customfeeds.conf`:

You need to select an architecture suitable for your router model.

For `ar-750`:

```bash
src leap https://sindominio.net/kali/openwrt/packages/mips_24kc/leap
src leap-openwrt https://sindominio.net/kali/openwrt/packages/mips_24kc/packages/
```

For `mt-300m-v2`:

```bash
src leap https://sindominio.net/kali/openwrt/packages/mipsel_24kc/leap
src leap-openwrt https://sindominio.net/kali/openwrt/packages/mipsel_24kc/packages/
```

### 3. Download key

{{< btn-copy text="wget https://sindominio.net/kali/openwrt/key-build.pub" >}}

```bash
wget https://sindominio.net/kali/openwrt/key-build.pub
```

If you feel like using gpg to verify that signature (you **should**), you can use:

```
wget https://sindominio.net/kali/openwrt/key-build.pub.asc
gpg --verify key-build.pub.asc
```

Now add the key to opkg's store:

{{< btn-copy text="opkg-key add key-build.pub" >}}

```bash
opkg-key add key-build.pub
```

### 4. Update and install

{{< alert icon="👉" text="If you had a previous version of openvpn-mbedtls you might want to uninstall it and get the one in kali's feed instead, since this one is compiled with management support." >}}

```bash
opkg update
opkg install bitmask-vpn
```

## Run

There's [an
attempt](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/scripts/bitmask)
at launching bitmaskd with `procd`, which you could symlink, but not quite
there yet - logs still need work. To be able to debug, I usually launch
`bitmaskd` in a tmux session and see what's going on.

If you pass a `DEBUG` flag, the openvpn logs will be additionally written to
`/tmp/bitmask-openvpn.log`.

{{< btn-copy text="DEBUG=2 /usr/bin/bitmaskd" >}}

```bash
DEBUG=2 /usr/bin/bitmaskd
```

### button/leds

The gl-inet models I'm targeting have a physical button - the button should be
detected and start the vpn (things can be a little slow). I'm also using one of
the leds (the middle one) to signal that the VPN tunnel reached the "CONNECTED"
state.

When you switch the VPN off, the middle led should be off too.

### api 

You can control the VPN locally from the router (it can be useful in a script
run via ssh).

```bash
curl localhost:8080/start
curl localhost:8080/status
curl localhost:8080/stop
```

### luci support

If you're using `useService=True`, bitmask will write a service file into
`/etc/openvpn`, enabled by default. With that, you can use `luci-app-openvpn`
to start and stop the service.

To install the luci app:

```bash
opkg update
opkg install luci-app-openvpn
```

### webui

There's an experimental webui (your package needs to have been built with webui support).

However, it does not any authentication at the moment, so that it exposes the
vpn control on whatever IP is configured on the lan interface. So please be
careful if playing with it.

```bash
DEBUG=1 WEBUI_INSECURE=1 bitmaskd
```

## Config

That should be all. You can modify the behaviour changing the [config file→]({{< relref "config" >}})
