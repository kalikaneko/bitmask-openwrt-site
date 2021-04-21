---
title: "Roadmap 🚀"
description: "Roadmap for BitmaskVPN"
lead: "A brief roadmap for the project."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "hacking"
weight: 700
toc: true
---

## 0.0.1
* [x] initial OpenWRT package
* [x] support for MT300/AR750
* [x] simple rest api
* [x] ability to read config files
* [x] minimal branding to use riseup/calyx

## 0.1.0
* [x] gateway selection
* [x] leds/buttons support
* [x] autostart if switch on
* [x] copy BTN_0 script on first run
* [x] basic routing rules
* [x] image builder
* [x] ready-to-flash images
* [x] killswitch: firewall/routing (use /etc/config/firewall)
* [x] Tor support for initial bootstrap
* [ ] beryl support

## 0.2.0
* [ ] daemonize openvpn
* [ ] reconnect to openvpn if it crashed
* [ ] proper daemon / logs
* [ ] external watchdog
* [ ] bitmaskctl util (symlinked to same binary)
* [ ] cli help 
* [ ] measure distance to gateways
* [ ] add password to telnet interface
* [ ] use unix domain socket for management
* [ ] init check for running openvpn

## 0.3.0
* [ ] web interface 
* [ ] luci integration
* [ ] parse gw protocols (udp/pt)
* [ ] fix metrics for busyBox ping
* [ ] tplink archer support

## 0.4.0
* [ ] use uci config
* [ ] traffic stats
* [ ] dockerize image builder
* [ ] CI integration

## 0.5.0
* [ ] use pluggable transports
* [ ] diagnose if Tor has a working circuit
