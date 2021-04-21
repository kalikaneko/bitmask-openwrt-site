---
title: "Dev Pointers ☕"
description: "Hacking on BitmaskVPN."
lead: "Tips for development, and how to send contributions."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "hacking"
weight: 600
toc: true
---

## Getting the source

The main repo lives at:

```
https://0xacab.org/kali/bitmask-openwrt
```

I can also push it to github or some other equally uninteresting places if that
makes things easier for people.

## Development

Get yourself a recent nim version and have a look at the compile targets in the
makefile. The openwrt process just pass the `ARCH` variable along to the nim
compiler. Only `mips` and `mipsel` are recognized now, but more things should
be possible.

I normally develop inside a Vagrant box with debian, but it should be easy to
develop with qemu to have a lightweight environment with openwrt without having
to upload things to a slow router.

## Building the package

If you are interested in getting this to run on a different architecture, 
there are some [config files](https://0xacab.org/kali/bitmask-openwrt/-/tree/master/docs/devices) that
you can use as a base. You can use the OpenWRT build tree, or one of the SDKs.

If you are using one of the pre-compiled toolchains, you need to put the
toolchain binaries in your path. It might be me, but I've needed to add
a couple of symlinks in a couple of occasions to get things running.

### Setup buildsystem

Check out the [excellent openwrt documentation](https://openwrt.org/docs/guide-developer/build-system/use-buildsystem).

### Clone bitmask-openwrt

From the top of your OpenWRT tree:

```bash
git clone https://0xacab.org/kali/bitmask-openwrt ../bitmask-openwrt
echo "src-link `pwd`/leap_packages" >> feeds.conf.default
mkdir -p leap_packages/net/network
ln -s `pwd`/../bitmask-openwrt leap_packages/net/network/bitmask-vpn
```

### Update and install the feeds

```bash
./scripts/feeds update -a
./scripts/feeds install bitmask-vpn
```

### Compile

Now you should be able to build the package:

```bash
make package/bitmask-vpn/compile
```
Or, if something goes wrong:

```bash
make package/bitmask-vpn/{clean,compile} -j1 V=s
```

## Building images

Until I find what's wrong with the way I was trying to use the OpenWRT build
tool, I [hacked together a little script](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/add-files.sh) to
generate config files for a pristine environment. It's a bit rough (the proper
way would be to let openwrt install the package to root), but it can serve as
a start do do your own thing (it should work with the [Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder))

That said, you can build a flashable image with:

```
cp ../bitmask-openwrt/add-files.sh . && ./add-files.sh
wget .../AR750.config -O .config
make download
make -j$(($(nproc)+1)) world
```

## Adding hardware support

Perhaps one of the easiest things to do is to look for the specifics for leds
and buttons for the router of your choice. There are a [couple of
strings](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/src/hardware.nim#L11)
that need to be added for each model.

Another more involved step is to adapt the config files (sensible defaults for
network/firewall) for specific targets, and see what constrains come from there.

## Privacy

Some notes about how some features are implemented, or I plan to implement.

### killswitch

The simplest option was [not to
route](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/config/firewall#L45)
between the LAN and the VPN interface by default. I prefer if the physical
toggle button cuts all access to the internet. It's up to you to configure
separate vlans or different wireless networks to get access to the clearnet in
parallel to the tunnel.

### dns leaks

There's one script included that redirects all DNS traffic through the gateway
DNS server. You can check in one of the [dns leak
test](https://www.dnsleaktest.com/) pages, you only should see the provider for
the VPN gateway in there.

### spam-free internet

One of my goals with this project is to spend some time researching and
maintaining an alternative firmware that is useful and can work with little
configuration: something like what GL-iNET is doing, but community-based and
perhaps oriented towards a more reduced public.

That includes being able to provide a spam- (and tracking-) free browsing
experience to people using your router (think people hanging in your bar or
social center). I dunno, one perhaps would like to block access to facebook,
for a change.

I've been looking at options, for the moment I think I'm happy with
`simple-adblock`, and I'm including it in the images I'm building, but not
trying to enable it by default. If you have ideas, get in touch.

### censorship circumvention

A very low hanging fruit was to ship `tor` in the image, and use it to
bootstrap the VPN connection if the option is enabled in the config. But
I acknowledge this won't be enough, and I want to explore other options (trying
to diagnose if pluggable transports might be needed, configuring tor to use
them, or attempting to port the `obfs4` support in a way that is not a killer for
space). Some of the domain-fronting techniques that are in the roadmap for the
other Bitmask implementations might be useful too, if they can be ported
without too much of a hassle.

## Patches

Feel free to [open a MR](https://0xacab.org/kali/bitmask-openwrt/-/merge_requests).
Patches via email are fine too. If I don't reply after a couple of weeks, please
do ping me. I'm not always in front of the computer.

Oh and if you ever [need a code of conduct, we
already](http://www.montypython.50webs.com/scripts/Holy_Grail/Scene8.htm) got
[one](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/docs/code-of-conduct.txt). 
