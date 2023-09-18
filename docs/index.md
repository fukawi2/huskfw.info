# Welcome to huskfw

husk is a natural language wrapper around the Linux iptables packet filtering engine (iptables).

It is designed to abstract the sometimes confusing syntax of iptables, allowing use of rules that have better
readability, and expressed in a more 'freeform' fashion compared to normal 'raw' iptables rules.

husk can be used on either firewall/router computers (with multiple network interfaces), or standalone systems (with one
network interface)

Each interface (real or virtual) is called a 'zone' in husk. Zones are given a friendly name which is what is used in
the rule definitions. This abstracts the Linux device names (eg, eth0, ppp0, bond0 etc) into much more intuitive names
such as NET, LAN and DMZ. This has the added benefit of moving interfaces in the future can be done simply by changing
the name-to-device mapping.

```
git clone git://github.com/fukawi2/husk.git
```
