---
title: "FAQ 🔥"
description: "Answers to frequently asked questions."
lead: "Answers to frequently asked questions (or so I think)."
date: 2021-1-06T08:49:31+00:00
lastmod: 2021-1-06T08:49:31+00:00
draft: false
images: []
menu:
  docs:
    parent: "help"
weight: 630
toc: true
---

## Bitmask?

Bitmask is the name of the provider-agnostic client for [LEAP
VPN](https://leap.se). It is some kind of nerd joke about masked superheros
that maintain their privacy by toggling bits very fast... Or maybe not, what do
I know.

## But how many "bitmask" are there in the wild?

There is one implementation [for
Android](https://0xacab.org/leap/bitmask_android), one [for
Desktop](https://0xacab.org/leap/bitmask-vpn), and [this
one](https://0xacab.org/kali/bitmask-openwrt) for embedded devices.

## What providers can I use?

Nowadays, there are only two stable ships sailing these waters: RiseupVPN and
CalyxVPN. This might or might not change in the future.

## What is the relation of this project with Riseup?

At the moment, this project is not officially endorsed by Riseup Labs. I just
thought that it would be cool to have one more implementation to maintain and
debug, and embarked on this little experiment. If you use the service
[RiseupVPN](https://riseup.net/en/vpn) provides for free (which implies you are
accepting their [terms of service](https://riseup.net/tos), please
[donate](https://riseup.net/vpn/donate) to them to keep the lights on.

Did I mention how important is to [donate](https://riseup.net/vpn/donate)? 😇

## What is the relation of this project with Calyx?

The Calyx Institute also provides a [free VPN service](https://calyx.net/) as
part of its non-profit mission, and they use the same protocol for
configuration than RiseupVPN. Their mission is to educate the public about
privacy in digital communications and to develop tools that anyone can use. You
can [join them](https://calyxinstitute.org/membership) - and if you happen to
live in the US, you might be interested in their [unlimited 4G
internet](https://calyxinstitute.org/membership/internet) via a device that I'd
like to support one day.

## Why did you write this in Nim? Why not just a bunch of config files?

I wanted to learn a new language, and to get started in openwrt development.
Having a concrete thing to do involving both sounded like a great idea. I think
[Nim](https://nim-lang.org/) it's a beautiful language and I want to use it
more: the expresiveness of python with the performance (and size) of C. I know
some people will argue that a bunch of shell scripts are enough to do the same
I am doing, but I plan to extend the program to include nice web uis and some
logic that —I think— will just look ugly in plain sh. But again, that's just my
opinion, YMMV.

My goal, in the long term, is to be able to provide an experience that needs
absolutely no access to the command line, either by using a luci package or
some other web interface.

## Will this ever be an official OpenWRT package?

That depends. Again, if people finds this useful, and once I'm comfortable with
the quality of the package, I will submit it to openwrt.

## Is there any other documentation?

Yep. 

- [leap.se:](https://leap.se) LEAP Encryption Access Project, the people behind
  this "start your own VPN provider, with resiliency and obfuscation" thing.
- [lilypad:](https://0xacab.org/leap/container-platform/lilypad) this is the
  most recent iteration of what it was known as "The LEAP Platform", and what
  you should explore if you want to start your own VPN provider. A more
  down-sized, one-stop solution should be possible picking things here and
  there, too.

## Can I get support?

Yes! Well, kind of. It can take some time to implement something new, I'd be
super happy to hear about your experience, or your ideas to extend support to
a certain router. Please open an issue and let's discuss:

- [Issue tracker](https://0xacab.org/kali/bitmask-openwrt/)

## Who are you?

I'm [kali](https://0xacab.org/kali). I've been working for LEAP (the
non-profit that develops RiseupVPN, CalyxVPN etc) for quite some time. I'm
interested in the idea of building accessible VPN infrastructure for everybody.

## Can I contact you?

You can ping me on irc. I'm kali at the indymedia network; you can find me at
`#leap` and `#bitmask-dev`. My email is in the commits.

