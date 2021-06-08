---
title: "User interfaces"
description: "new user interfaces"
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

What I wanted is the full web experience, and luci is kind of for power-users
(although it's super easy to hook yourself with luci, which I probably will
want to explore further, for things like authentication).

Since I was already using [prologue](https://github.com/planety/Prologue),
I thought it would be a good idea to ship some simple static files and try my hand with javascript. It's been ages I don't do browser stuff, but the challenge seemed interesting.

{{< img src="webui.png" alt="bitmaskvpn control panel" caption="<em>The experimental webui, in a rare moment in which it wasn't crashing.</em>" class="border-0" >}}

## Some challenges for a minimalistic UI

I'm not a frontend person, and somehow I've managed to avoid learning modern
javascript completely. I always relied on quick jQuery for the simple things
I usually need to do. For this, I wanted to attempt vanilla javascript as much
as it wasn't painful. But I also want things to look more or less nice, across
all kind of devices- again, taking into account that I'm not a designer, hehe.
This means that I'm willing to compromise some space for the promise of more
functional UI. That seems to be a recurrent dilemma nowadays.

My needs are simple in principle: I wanted to display the gateway selector in
a map, and give some quick feedback about what's the public IP of the router
(including where do sites *think* we're connecting from). In principle I'll
ignore permissions, so let's assume that this is only for personal routers so
that you don't mind getting the interface exposed in the public UI (and let's
do something about that later). In a more advanced case, I still would like to
show the connection status in a public interface, and maybe request admin
permissions for changing it - that should be delegated to luci. So, with these
requirements,  I went to the shop to buy for tools.

* A minimalistic css framework: [mini.css](https://minicss.org/) quickly caught
  my eye. Small and looks good. Alas, not maintained anymore, but if it gets the
  job done I won't get too picky.
* A visualization tool: after discarding openlayers, I picked
  [DataMaps](http://datamaps.github.io/). True, it depends on d3.js, but
  I think the trade-off still falls on the side of simplicity.
* For the rest, I decided to write vanilla javascript where possible.

## Optional webui support

Although I got quite frustrated by a crash in [nim's
httpx](https://0xacab.org/kali/bitmask-openwrt/-/issues/1) (to be fair, I'm
pretty sure my use of the threadpool dispatcher has something to do), I'm more
or less happy with the result so far: all the ui assets together are around 300kB, and
that's including the whole d3 framework which I think it's going to be very
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

If you want to try it, run the daemon with:

DEBUG=1 WEBUI_INSECURE=1 bitmaskd

# Pending stuff

I think this is a good working prototype with the expectations I had. LuCI
covers the basics, and I've got something that more or less shows the relevant
information and offers some interactivity.

However, I need to revisit the whole backend. It's not ok that concurrent ajax
requests crash my app, so either I fix that or I change the library I'm using.
In any case, now that I've decided not to handle openvpn in its own thread,
everything can be greatly simplified - since I'm already missing the days of
twisted, I might even give a try to [chronos](https://github.com/status-im/nim-chronos), which seems to be getting
some traction in the nim ecosystem.

Another thing with a lot of potential for improvement is my usage of
javascript: I'm already entering callback hell, so I know I need to embrace
promises. On the other hand, I wonder how well [nim's javascript
backend](https://nim-lang.org/docs/backends.html#backends-the-javascript-target)
will work in asynchronous mode. And I also need a sane way of updating
visual components across the whole app. So I guess my next exploration swill
dive into getting my crappy prototype into something more production ready,
either by trying something like [nuxt.js](https://nuxtjs.org/) or by going down
the nim-to-javascript path.

Until next time!
