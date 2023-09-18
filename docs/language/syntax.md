
Rule Syntax

Rules are made up of 2 mandatory parts, and optional comments:
<target> <criteria> # Comment about this rule

Target is the action to take when Criteria is matched. Target can be either a built-in such as ACCEPT, REJECT or DROP, or a User-Deifned Chain (UDC). All criteria keywords can be combined to make rules more specific. All keywords in a single rule (line) are subject to AND logic. It is generally invalid to include the same keyword twice such as:
accept source address 192.168.1.1 source address 192.168.1.2

This is invalid since a single packet can never have 2 different source addresses!

Example rules are below. For full details of available Keywords, see the Keywords Dictionary page. All keywords can be mixed and matched to suit the rule(s) you require.

# Accept anything from source address 192.168.100.100
accept source address 192.168.100.100

# Allow TCP port 80 to whatever 'google.com.au' resolves to via DNS.
accept proto tcp port http destination address google.com.au

# Allow any TCP port 80 and 443 traffic from addresses .1 to .10
accept proto tcp ports http,https source range 192.168.0.1 to 192.168.0.10

# Anything from the given MAC Address is allowed.
accept mac 00:14:22:d8:f9:55

# Reject with ICMP unreachable packet any traffic from the given IP network
reject source address 169.254.53.0/24

# Jump to the user-defined chain "SMB_PORTS" for traffic to 192.168.1.100
SMB_PORTS destination address 192.168.1.100

# Accept everything. Note that the 'all' keyword is optional and has
# no meaning. It is allowed purely to improve readability.
accept all
# Just drop all packets. Note the 'all' keyword is missing here, but
# it matches every packet the same as above.
drop

# Allow 4 ICMP 'echo-request' packets per second, bursting to 8pps
accept proto icmp type echo-request limit 4/sec burst 8

# Allow 4 ICMP 'echo-reply' packets per second, bursting to 12pps
accept proto icmp type echo-reply limit 4/sec burst 12

# Accept any icmp packets that come in the LAN interface.
accept in LAN protocol icmp
Other Rules

These rules can be defined anywhere in the rules file, including outside of a 'define' paragraph.
Port Forwarding (DNAT)

If you need to forward incoming traffic from one interface to another using the NAT feature of the kernel, you can use the 'map' rule.

For example, to NAT port 80 traffic coming in the "NET" interface to an internal server:
map in NET protocol tcp port 80 74.132.12.56 to 172.16.1.1

You can also translate the ports from one to another by appending to the destination port:

map in NET destination address 74.132.12.56:80 to 172.16.1.1:8080

Don't forget that this only does the Destination NAT. You must also write a corresponding rule to allow the traffic through the filtering part of the firewall. For example, the corresponding for the above port 80 NAT to the internal web server could look like this to allow the whole world access:
define rules NET to DMZ
accept proto tcp port http destination address172.16.1.1
end define
Intercepting / Redirection

Using the 'trap' or 'redirect' target, you can silently redirect traffic to the local computer. This is useful for example to intercept all outgoing SMTP traffic to force it through the local SMTP gateway:
trap in LAN protocol tcp port 25

Redirecting to alternative ports is also possible:
redirect incoming NET protocol tcp port 80 to 8080
redirect incoming NET protocol tcp port 2222 to 22

You must also define corresponding filter rules to allow this traffic the same as 'map' rules.
Raw iptables

Directly writing iptables rules is also supported for putting your own rules in using iptables syntax. This allows you to write complex rules or use modules that husk doesn't support. For example, to use the NOTRACK target in the 'raw' table:
iptables -t raw -A OUTPUT -d 10.0.0.0/8 -j NOTRACK

In order to use this feature to include rules within chains that husk generates, you can use the special value %CHAIN% which will be replaced at compile-time with the current chain. For example, you can include a rule within the 'NET to ME' chain without having to worry about what the name of the chain that husk generates is:
define NET to ME
iptables -A %CHAIN% -p tcp --dport http -j ACCEPT
end define

In this example, %CHAIN% will be automatically replaced with 'crs_NET_ME' at compile time.
File Includes

If your ruleset is complicated, you can spread the rules over multiple files then consoldate them all together at compile time by using the 'include' keyword:
include outbound.rules
include inbound.rules

Includes can be either relative paths (to the conf dir) or absolute paths.You should be able to nest indefintiely, but be careful not to create loops such as:
[rule-one.conf]
include rules-two.conf

[rules-two.conf]
include rules-one.conf

There is NO PROTECTION against this, and may have some 'interesting' results.

