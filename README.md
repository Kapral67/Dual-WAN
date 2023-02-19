# Dual-WAN for DD-WRT

[![Releases](https://github.com/Kapral67/Dual-WAN/actions/workflows/release.yml/badge.svg?branch=main&event=push)](https://github.com/Kapral67/Dual-WAN/actions/workflows/release.yml)

## DISCLAIMER

- This is in no way fully-tested

- Some or all of this repo will need to be modified dependent on your router's hardware

- Use at your own risk

- DD-WRT's base WAN currently must be disabled

- You may give up some GUI features using this script; but everything can be implemented just with terminal commands and well placed, well named scripts

- UPNP hasn't been tested, but likely will not work
    - This is more of a feature than a problem since UPNP shouldn't be trusted anyways

## Testing

### Hardware

- Netgear R7000P

### Software

- DD-WRT v3.0-r51679

## Methodology

- This repo is a modified version of a [DD-WRT Wiki Article](https://wiki.dd-wrt.com/wiki/index.php/Dual,_Triple_(and_probably_quad)_WAN_with_multiple_active_WAN_links_and_source_routing)

- Some differences are:

    - This repo moves away from the net-tools dependency

    - Vlans should be setup using swconfig (which has a GUI in newer builds)

    - wan* aliases are treated as hotswappable rather than binding to a vlan

    - lowered numbered wan (i.e. wan0) have priority over higher numbered wan and therefore all wan must have a priority number

        - That being said the current configuration sets aside your lan's `*.64/27` block of addresses as being higher numbered wan priority

        - The config assumes a lan subnet `255.255.255.0` and as such only reserves the following subnets besides the aforementioned:

            - `*.0/26`

            - `*.96/27`

            - `*.128/25`

    - httpd is restricted to https only regardless of GUI settings

    - an ntpclient ip address must be set, no domains

    - dns cannot be pulled from ISP, must be set statically in nvram

    - do not use any nvram variables reserved for dd-wrt's base wan, this may inadvertently enable the base wan which will mess up the routing tables

        - except for `wan_dns` a space separated list of dns servers

    - this repo is preconfigured for dual-wan on interfaces `vlan2` & `vlan3`

        - both those interfaces unbridged, and support Masquerade / NAT

    - this repo assumes connmark is enabled in your device's kernel/iptables and does not load any kernel modules for this purpose

## Confirmed Working GUI Features

- Port Forwarding
- Port Range Forwarding
- Everything else except obviously anything pertaining to WAN
- Also, UPNP probably won't work (not fully tested)
- DMZ may work given some edits to the firewall script
- DD-WRT base firewall probably is dead when using this, rely on the firewall script

*This is probably an incomplete list*

