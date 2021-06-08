---
title: "What's next"
description: "reflecting on the roadmap"
lead: ""
date: "2021-06-07"
lastmod: "2021-06-08"
draft: false
contributors: ["Kali Kaneko"]
---

# Next steps

Today I'm tagging
[0.2.0](https://0xacab.org/kali/bitmask-openwrt/-/tree/0.2.0), and I want to
pause to think and reflect about the [roadmap→]({{< relref "roadmap"
>}}) and what my next priorities should be.

I think for the next cycles I need to focus on consolidating the webui, and
make extra sure that whatever I'm using in the nim app is not easily crashed.
If prologue + the threadpool is not stable, I might need to switch to
a different stack - one idea that comes to my mind is to leave the daemon super
small, just refreshing certs and parsing config, and move all the ui to luci...
lua seems quite nice to work with.

The frontend also needs a lot of improvement: once the backend is solid, all
the javascript parts need to be polished and made more elegant. Then I think
the project will be in good shape to be distributed to real users.

In the meantime, if you're excited about the project or just want to suggest
ideas, you can reach me in the issue tracker, or on IRC (we've moved to #leap
at libera.chat, after the whole freenode fiasco).

Happy hacking!

{{< img src="riseup-bird.png" alt="a bird watches you from the command line" caption="<em>When you're a night owl, it's soothing to be greeted by a bird against a dark background</em>" class="border-0" >}}






