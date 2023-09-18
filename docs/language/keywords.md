# Keyword Dictionary

## `protocol`

Synopsis: Specify the protocol to match.

Alternatives: `proto`

Corollary in iptables: `-p`

Examples:
```
protocol tcp
proto udp
```

---

## `incoming`

Synopsis: Specify the inbound zone traffic comes in to match this rule.

Alternatives: `in`

Corollary in iptables: `-i`

Examples:
```
incoming NET
in LAN
```

---

## `outgoing`

Synopsis: Specify the outbound zone traffic goes out to match this rule.

Alternatives: `out`

Corollary in iptables: `-o`

Examples:
```
outgoing NET
out LAN
```

---

## `source address`

Synopsis: The source address, or network, that traffic must match. You can use the following types of values to specify
the address:

  - IP Address (eg, `192.0.2.1`)
  - IP Address with netmask (eg, `192.0.2.0/255.255.255.0`)
  - IP Address with CIDR mask (eg, `192.0.2.0/24`)
  - DNS Hostname

The DNS hostname is resolved by iptables at the time the rules are put in place. If the IP Address that the Hostname
maps to changes, the rules must be reloaded to 'see' the new address. Also, only the FIRST address returned is included
in the rule, so for DNS records with multiple A records (such as Google), using the hostname is not very useful.

_Tip: netmasks can be very flexible as it allows you to wildcard any part of the address. For example, if you have a
company with multiple sites, and each site has it's own /16 address space (eg, 10.10.0.0/16, 10.20.0.0/16 etc), and
each site has a proxy at 10.x.0.1, then you can allow all the proxy servers outbound access by using the netmask
255.0.255.255._

Alternatives: None

Corollary in iptables: `-s`

Examples:
```
source address 192.168.1.1
source address 192.168.1.0/24
source address 10.0.0.1/255.0.255.255
source address mail.example.com
```

---

## `destination address`

Synopsis: The opposite of "source address". The same rules apply as above.

Alternatives: `dest address`

Corollary in iptables: `-d`

Examples:
```
destination address 192.168.1.1
destination address 192.168.1.0/24
dest address 10.0.0.1/255.0.255.255
dest address mail.example.com
```

---

## `source range`

Synopsis: Specify a range of addresses to match as the source address. This allows arbitrary ranges to be matched, where the range doesn't fit in a netmask or CIDR mask (eg, 192.168.1.1 to 192.168.1.10)

Alternatives: None

Corollary in iptables: `-m iprange --src-range`

Examples:
```
source range 192.0.2.1 to 192.10.2.10
```

## `destination range`

Synopsis: Opposite of `source range`

Alternatives: `dest range`

Corollary in iptables: `-m iprange --dst-range`

Examples:
```
destination range 192.0.2.10 to 192.0.2.50
```

---

## `source port`

Synopsis: The source port of traffic to match this rule. The port can be either a number (0 to 65535) or a name as found
in `/etc/services`. This match criteria is rarely useful; much less so than "destination port". Change "port" to "ports"
for multiport match in single rule.

Alternatives: None

Corollary in iptables: `--sport`

Examples:
```
source port 68
source port http
source ports 8080,3128
```

---

## `destination port`

Synopsis: Opposite of "source port". The leading "destination" (or "dest") can be dropped. When "port" is seen on it's
own, it is assumed to be a destination port.

Alternatives: `dest port` or `port`

Corollary in iptables: `--dport`

Examples:
```
destination port 80
destination port http
destination ports http.https
ports http.https
```

---

## `limit`

Synopsis: Sets a time-based limit on how often this rule is allowed to be matched. Format for the limit is
"count/interval". Example values include 3/sec or 60/min. Interval can be 'second', 'minute', 'hour' or 'day'.

Alternatives: None

Corollary in iptables: `-m limit --limit`

Examples:
```
limit 3/sec
limit 4/minute
```

---

## `type`

Synopsis: Matches the ICMP message type. It is invalid to specify this keyword without specifying the protocol as
`icmp`. Either the code (number) or name can be supplied (eg, "echo-request" or "8" are valid and equivalent). Check
`iptables -p icmp -h` for a list of valid names accepted by your kernel.

Alternatives: None

Corollary in iptables: `-m icmp --icmp-type=`

Examples:
```
type echo-request
type 8
```

---

## `start`

Synopsis: Specify that this rule only applied at specific times. This sets the "start" time in 24 hour format.

Alternatives: None

Corollary in iptables: `-m time --timestart`

Examples:
```
start 8:00
```

---

## `finish`

Synopsis: Specify that this rule only applied at specific times. This sets the "end" time in 24 hour format.

Alternatives: None

Corollary in iptables: `-m time --timestop`

Examples:
```
end 17:00
```

---

## `days`

Synopsis: Specify that this rule only applied at specific times. This sets that the rule should only apply on the given
day(s). Separate multiple days with a comma.

Alternatives: None

Corollary in iptables: `-m time --weekdays`

Examples:
```
days Mon
days Mon,Tue,Wed,Thu,Fri
```

---

## `every`

Synopsis: Only match this rule on every X packets. For example, you could ACCEPT only every 4th packet. This would
simulate 75% packet loss.

Alternatives: None

Corollary in iptables: `-m statistic --mode nth --every`

Examples:
```
every 4
```

---

## `offset`

Synopsis: Works with the "every" keyword to set the starting offset to start matching. By default "every" starts
counting on the first packet (numbered 0), so in the example above, it would match packets 0, 4, 8 etc. If we set the
offset to "1" then it will then match packets 1, 5, 9 etc. This is useful in some special load-balance configurations.

Alternatives: None

Corollary in iptables: `-m statistic --mode nth --offset`

Examples:
```
offset 1
```

---

## `state`

Synopsis: Consult the kernel's connection tracking to determine what "state" the connection that this packet belongs
to is in. Valid states are: `NEW`, `ESTABLISHED`, `RELATED`, `INVALID` and `UNTRACKED`.

Alternatives: None

Corollary in iptables: `-m state --state`

Examples:
```
state new
state invalid
```

---

## `mac`

Synopsis: Match this rule if the MAC Address of the source matches. The MAC Address must be in the format of
`XX:XX:XX:XX:XX:XX`. This only makes sense for packets coming from an ethernet device on the same Layer 2 network.

Alternatives: None

Corollary in iptables: `-m mac --mac-source`

Examples:
```
mac 6c:f0:49:e8:64:28
```

---

## `all`

Synopsis: This is a "noop" (aka "No Operation") keyword. It does nothing and is ignored. It is for making rules more
readable, for example when we need a rule that matches all traffic and accept it, we can write `accept all` or `accept`.

Alternatives: None

Corollary in iptables: N/A

Examples:
_See above_
