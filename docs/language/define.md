
Define Paragraphs
Traffic passing between zones is known as cross-zone traffic - eg LAN to NET. These rules are defined in rule paragraphs.
define rules ZONE to ZONE

Defining rules for this cross-zone traffic is done in a 'define rules' block.

There is a special in-built interface called 'ANY' which as the name suggests, allows you to write a match calls that ignores one (or both) of the IN and OUT zones. For example:

Allow the whole world to access Secure POP3 on our mail server.
define rules ANY to DMZ
accept protocol tcp port pop3s destination address mail.example.com
end define

This is effectively the same as the FORWARD table, but only NEW connections are passed through here:
define rules ANY to ANY
accept protocol tcp port 873 # rsync anywhere is fine
end define

NOTE: The 'ANY' zone excludes the other interface to avoid bounce routing issues. So a match calls for 'ANY to LAN' doesn't include traffic 'LAN to LAN'. If you need to allow bounce routing, then add a rule such as this:
define rules FORWARD
accept incoming LAN outgoing LAN
accept incoming DMZ outgoing DMZ
end define

Syntax
define rules INBOUND-ZONE to OUTBOUND-ZONE
rules go here
end define

Example
define rules LAN to NET
rules go here
end define
define rules UDC

You can create your own user-defined chains (UDC) using a 'define rules' block. These user-defined chains can then be called from cross-zone blocks.

define rules SMB_PORTS
rules go here
end define

define rules (INPUT|OUTPUT|FORWARD)

To add rules to the standard iptables filter table chains (INPUT, FORWARD and OUTPUT), write a 'define rules' UDC block for the appropriate chain:
define rules INPUT
rules go here
end define

