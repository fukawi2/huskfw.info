Q. What does option X do?
A. I've spent a lot of time writing documentation; Please refer to the man pages:
man husk
man husk.conf
man fire

If you can't find the answer to your question there, please do contact me and I'll be happy to help, plus update the documentation :)

Q. I can't make any connections (eg, to browse a website) after activating my rules.
A. Remember the default policy for ALL chains in the 'filter' table is DROP. This includes the 'OUTPUT' chain so you need to explicitly allow (or write appropriate rules for) outbound traffic. For most hosts it is acceptable to accept all traffic:
define rules OUTPUT
accept all
end define

Q. I get errors complaining about being unable to load match 'conntrack'?
A. Some kernels (still) do not have support for the newer "conntrack" iptables module. If this is your kernel, you can configure husk to use the older "state" module instead by adding "old_state_track = 1" to your husk.conf file. This issue is known to affect CentOS 5, but only with IPv6.

Q. I get errors complaining about being unable to load match 'comment'?
A. Some early IPv6 kernels did not have support for the "comment" iptables module. Husk includes a comment with all rules to help identify the source of a particular rule. This issue is known to affect CentOS 5. To disable comments on IPv6 rules, set "no_ipv6_comments = 1" in your husk.conf file.

Q. I get errors starting with '_SYNTAX' is not defined at'
A. This means your husk.conf file is still completely commented. At a minimum, uncomment one or both of "do_ipv4" or "do_ipv6"

Q. My hooks don't run
A. Make sure you have set them to be executable.
