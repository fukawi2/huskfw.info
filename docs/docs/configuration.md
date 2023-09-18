# Configuration

## General Configuration: `husk.conf`

Refer to the man page.

## Interfaces: `interfaces.conf`

Give your interfaces friendly (ie, Zone) names in the file 'interfaces.conf'. Zone names can be comprised of
alphanumeric and '_' characters.

An example file might look like this:

```
zone ME is lo
zone NET is eth0
zone LAN is eth1
zone DMZ is eth2
```

I recommend simple 3 letter friendly names for your zones, but there should be no (unreasonable) limit to the length. If you want to name your interface NETWORK_CONNECTION_TO_THE_INTERNET then that should be fine!

**IMPORTANT:** loopback ('lo') must be called the special name 'ME'

## Address Groups: `addr_groups.conf`

To avoid repetition and to abstract data (addresses) from logic (rules), you can define groups of addresses in addr_groups.conf then use these groups within rules. For example, if you have multiple web-servers, you can define them as a group, then apply rules to the group. Any future changes to the rules will apply to all the hosts in the group, and any changes to the group will be reflected in the rules.

There are some examples in the default file. A definition of a web servers group with 4 servers might look like this:

```
[web-servers]
desc = Public Web Servers
hosts = <<EOT
192.0.2.101
192.0.2.102
192.0.2.103
192.0.2.104
EOT
```

Hosts can be either specific addresses (as above) or networks with a CIDR mask (eg, 192.168.0.0/24).

## Rules: `rules.conf`

This is the "meat"; your rules. The rules file contains various types of information:

  - "Common Rules"
  - "Define" Paragraphs
  - Raw iptables rules

"Define" Paragraphs are for defining Cross Zone traffic (ie, from your LAN to the Internet; LAN to NET), and User Defined Chains (UDC). UDC's are explained further below.

Cross Zone Rules define what traffic is allowed to pass from one zone to another. On a firewall/router there could be many possible vectors that traffic could be seen (eg, "LAN to NET", "NET to DMZ", "DMZ to LAN", "NET to ME", "LAN to ME" etc). On a "standalone" server, you'll have much less vectors, generally only have a single paragraph such as "NET to ME".

UDC's are useful for "breaking out" similar rules into a set of sub-rules. For example, you could create a UDC that accepts traffic from a specific list of addresses, then call that UDC in "NET to ME" only for TCP port 22 (SSH) traffic. This way, instead of many rules for port 22 AND all the different addresses having to be tested for each packet, the UDC is only checked for port 22 traffic.

```
define SSH_OK
accept source address sydney.example.com
accept source address melbourne.example.com
accept source address brisbane.example.com
accept source address perth.example.com
accept source address auckland.example.com
accept source address tokyo.example.com
end define

define NET to ME
SSH_OK protocol tcp port ssh
end define
```

This makes rules cleaner, as well as faster for the kernel to evaluate since only 1 rule has to be checked for any packets that are NOT port 22, instead of 6 rules if they were all defined in 'NET to ME' without a UDC.

For further details on the syntax of Define Paragraphs, see the Define Paragraphs and Rule Syntax page.
