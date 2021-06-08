---
title: "Router roundup"
description: "Evaluating new hardware"
lead: "What routers should this project support next?"
date: "2021-05-20"
lastmod: "2021-05-20"
draft: false
#weight: 99
images: ["router_roundup.jpg"]
contributors: ["Kali Kaneko"]
---

# Beyond pocket-size

Even though the initial roadmap for this project was limited to pocket router
models (a couple of the cheap Gl-INET  travel routers), as a friend said "those
are little more than glorified toys". So, thinking in doing a more serious service
(protecting your guest traffic in your social center, bar, shared space),
I'm thinking about what other routers to play with.

I'm particularly interested in things with extra leds and buttons, and for sure
having an USB port is quite interesting. Another factor is easy-to-flash: nerds
already do fine with ssh access and command line scripts, but what I'm really
targeting here are usable web interfaces. So, again, flashing should be
a breeze.

For now, after fixing a couple of outstanding issues, I think I'll settle on
building additionally for these two, that are based on the same chipset:

* <a href="https://openwrt.org/toh/hwdata/gl.inet/gl.inet_gl-mt1300">GL-INET 1300</a> - still a travel router, but quite a beast for its size.
* <a href="https://openwrt.org/toh/hwdata/d-team/d-team_newifi_d2">Newifi-D2</a> - a cheaper router, but similar in hardware spec.

Other than that, I was interested also in cheap TP-Link models that, in some
places, seems to be all that the international export market is able to provide
at affordable prices. The cheapest options out there (TL-WR84N1) are already
not supported in the latest OpenWRT versions (although I guess with some
patience something can be done), but models like the ARCHER C6 seemed like good candidates.

{{< img src="roundup.jpg" alt="plants surrounding the router. one day, all this silicon will be back to us..." caption="<em>The family grows</em>" class="border-0" >}}

Anyway, this is only the result of my explorations.I'm not super up-to-date
with the latest and greatest in the Wireless World, so if you have any other
suggestions, please get in contact :)





