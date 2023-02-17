# Dual-WAN for DD-WRT

## DISCLAIMER

- This is in no way fully-tested

- Some or all of this repo will need to be modified dependent on your router's hardware

- Use at your own risk

- DD-WRT's base WAN currently must be disabled

- UPnP & Port-Forwarding GUI settings may or may not work (haven't tried or tested)

  - You give up a lot of GUI features using this script; but everything can be implemented just with terminal commands and well placed, well named scripts

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

## Reader's Due Diligence

When dealing with DD-WRT, it's important that you understand anything and everything that is occurring on your device. Therefore, this repo is not meant to be a step-by-step guide. A lot that you need to know to get this to work will come by reading through the code, and linking it together in your mind. Just as I had to do with the Wiki Article, except a lot of the heavy-lifting I have already done for you. Think of it as a fun scavenger hunt that will leave you a better system admin, shell scripter, and network architect when its all over. To start, try cloning this repo and grepping for nvram variables you may have to set. Have Fun!

