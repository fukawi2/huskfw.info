# Common Firewalling

husk has built-in 'common' firewall rules, prefixed with the keyword 'common'. These rules fulfill commonly used features implemented with the Linux firewall, such as NAT'ing, Antispoof, Bogon and Port Scan protections.

## NAT

Apply a Source NAT to traffic going out ZONE, usually 'NET'

Syntax
common nat ZONE

Example
common nat NET

## SPOOF

Prevent address spoofing on the specified ZONE. You must specify ADDRESS/PREFIX to define the addresses that are expected to be seen in the given ZONE. You can add multiple 'spoof' rules per interface.

Syntax
common spoof ZONE ADDRESS/PREFIX

Examples
common spoof LAN 10.0.0.0/24
common spoof LAN 10.0.1.0/24
common spoof DMZ 74.125.237.16/29

## BOGON

Block bogon traffic on the specified ZONE. Bogon traffic is packets with source addresses that should never be seen outside private networks such as RFC1918 addresses, 127.0.0.0/8 etc. This is usually suitable only for the NET zone, or other zones that are publicly addressed, such as a routed DMZ.

Syntax
common bogon ZONE

Example
common bogon NET

## PORTSCAN

Attempt to detect, log and drop portscans coming from the given zone. This is only rudimentary, but it's better than nothing.

Syntax
common portscan ZONE

Example
common portscan NET

## XMAS

Block Christmas Tree Packets on the specified ZONE. Xmas tree packets are packets with all flags set, or the packet "is lit up like a Christmas tree" which is invalid and suspicious.

Syntax
common xmas ZONE

Example
common xmas NET

## SYN

All TCP packets that the kernel considers as belonging to a "NEW" connection should have the "SYN" flag set. If they don't, then we DROP them.

Syntax
common syn ZONE

Example
common syn NET

## LOOPBACK

Create a rule to accept traffic in/out the 'lo' interface. There are only very rare circumstances where you don't want this rule in your configuration.

Syntax
common loopback

Example
common loopback
