---
title: "User interfaces"
description: "luci integration and experimental webui"
lead: "Controlling the VPN through a web interface"
date: "2021-06-05"
lastmod: "2021-06-05"
draft: false
#weight: 90
contributors: ["Kali Kaneko"]
---

# LuCI integration

The third milestone in this project (which has been gracefully bootstrapped by
[nlnet](https://nlnet.nl/project/bitmask/), although it has taken
some time for me to finish it due to all the pandemic weird times!) was
oriented to investigate ways to offer a nice control dashboard for the VPN.

I was thinking about how to approach this for some time, and learning a bit
about how luci apps are made. Then, at some point, I said to myself that
I should "Keep It Simple, Stupid". So, it took me some time to realize, but the
simplest thing to do was *not to build another app*, but simply let
[luci-app-openvpn](https://openwrt.org/packages/pkgdata/luci-app-openvpn)
to handle the connections in its own style.

So now if you use "useService=true" (enabled by default in the new config), we
will not attempt to launch an openvpn process, but use openwrt's openvpn
service instead, and bitmask-vpn will write a service config file in
/etc/openvpn/provider.ovpn, from where the service can now be controlled.

It's completely ugly, you can do very little more than start, stop, and edit
things by hand, which is not very user-friendly, but hey, <em>it works</em>.

{{< img src="luci.png" alt="how to start the vpn from luci" caption="<em>Any successfully bootstrapped provider will appear as an openvpn service now.</em>" class="border-0" >}}

# A more interesting webui prototype

Well, that was not much fun.

Not only that: the whole point of the work we've been doing in the platform
side, to do load balancing and direct users to the gateway that is more suited
to their needs, is to let users choose easily between the "recommended"
gateway, or a manual override of their choice. Maybe I'm wrong, but I don't
know any open source vpn package for openwrt that lets you do anything similar.

All this with some caveats, because I wouldn't want user choice to massively play
against a fair distribution of resources (it's fun how anarchist
discussions permeate technology). This basically means that, as soon as
[menshen](https://0xacab.org/leap/menshen) is deployed by Riseup, Calyx or any
others, I want to be able to show load information live next to any widget for
gateway selection. This is part of a broader usability research that [simply
secure](https://simplysecure.org/) has been helping us to do. After the
findings in that collective process, we're already implementing some changes in
both [android](https://0xacab.org/leap/bitmask_android) and
[desktop](https://0xacab.org/leap/bitmask-vpn) clients.

What I wanted is the full web experience, and luci is kind of for power-users
(although it seems to be quite easy to hook your app against luci, which
I probably will want to explore further, for things like authentication).

Since I was already using [prologue](https://github.com/planety/Prologue),
I thought it would be a good idea to start by shipping some simple static files
and write some bits of javascript. It's been ages I don't do browser stuff, but the
challenge seemed interesting.

{{< img src="webui.png" alt="bitmaskvpn control panel" caption="<em>The experimental webui, in a rare moment in which it wasn't crashing.</em>" class="border-0" >}}

## Some challenges for a minimalistic UI

I'm not a frontend person: I've always relied on jQuery snippets for the simple
things I usually need to do. That's why I wanted to attempt vanilla javascript
(*as long as it is not too painful*).

I also like things to look nice, and working on all devices.

This means that I'm ok to compromise *some* space for the promise of a more
functional UI. That seems to be a recurrent dilemma nowadays.

My needs are simple: display the gateway selector in a map, and give some quick
feedback about what's the public IP of the router.

To simplify, I will ignore permissions for now. If this is your travel router, I trust
you to defend your lan port with your whole body if needed. We will
authenticate when the time comes.

So, let's go shopping:

* A minimalistic css framework: [mini.css](https://minicss.org/) quickly caught
  my eye. Small and looks good. Alas, not maintained anymore, but if it gets the
  job done I won't get too picky.
* A visualization tool: after discarding openlayers, I picked
  [DataMaps](http://datamaps.github.io/). True, it depends on d3.js, but
  I think the trade-off still falls on the side of simplicity.
* For the rest, I decided to write vanilla javascript where possible.

## Build with webui support

I ended up being quite frustrated by a crash in [nim's
httpx](https://0xacab.org/kali/bitmask-openwrt/-/issues/1) (to be fair, I'm
pretty sure my use of the threadpool dispatcher has something to do)...

On the frontend side, I'm happy with the result so far: all the ui assets
together are around 300kB, and
that's including the whole d3.js lib which I think it's going to be very
useful later on (for instance, I'm thinking on live traffic graphs, if I can
get my head around the enter/leave d3 syntax). I think that's manageable for
all but the most restricted routers - perhaps something even simpler, with no
map, can be done for those cases withouth complicating things too much.

If not for the beauty of the produced code, at least I learned about how to
pass configuration options to the [package
Makefile](https://0xacab.org/kali/bitmask-openwrt/-/blob/master/Config.in#L5)
-did I mention I'm getting in love with how intuitive openwrt build system is?.
Nice! Now I can let people build experimental stuff optionally, which will
probably come handy for more advanced features down the road (obfs4 support,
for instance).

Config options at build time works, although I should probably
explore the right mechanism to build different variants (`-base` and `-webui`),
but let's keep things simple for now.

If you want to try it, run the daemon with:

```bash
DEBUG=1 WEBUI_INSECURE=1 bitmaskd
```

# Iterating from here

I think this is a good working prototype for the goals I had set. **LuCI
covers the basics**, and I've got something that more or less shows the relevant
information and offers some interactivity.

However, **I need to revisit the whole backend**. It's not ok that concurrent ajax
requests crash my app (but indeed [it's everybody's responsibility to fix
faulty commons](https://github.com/dom96/httpbeast/pull/35#issuecomment-722005280)), so
either I fix that (for which I have to find the underlying reason) or I change
the library I'm using. In any case, now that I've decided not to handle openvpn
in its own thread, everything can be greatly simplified. And since I'm already
missing the days of twisted, I might even give a try to
[chronos](https://github.com/status-im/nim-chronos), which seems to be getting
some traction in the nim ecosystem nowadays.

Another thing with a lot of potential for improvement is my usage of
javascript: I'm already entering callback hell, so I know I need to embrace
promises. On the other hand, I wonder how well [nim's javascript
backend](https://nim-lang.org/docs/backends.html#backends-the-javascript-target)
will work in asynchronous mode - or rather, how an stupid developer like me
will get used to work with it. And I also need a sane way of updating
visual components across the whole app withouth repeating myself. So I guess my
next exploration swill dive into getting my crappy prototype into something
more production ready, either by trying something like
[nuxt.js](https://nuxtjs.org/) or by going down the nim-to-javascript path.

Thanks for reading, and until next time!
