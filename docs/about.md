# About

Comparison of husk to other firewall compilers/wrappers. Note that I believe each package performs the job it was
written for, and both fwbuilder and shorewall are excellent packages. However they didn't fulfill my requirements,
so I developed husk.

|                                   | huskfw | fwbuilder | shorewall                |
|-----------------------------------|--------|-----------|--------------------------|
| Graphical User Interface (GUI)    | No     | Yes       | via Webmin               |
| Text Only Interface (TUI)         | Yes    | ???       | ???                      |
| Support for Traffic Shaping       | No     | Yes       | ???                      |
| Built on top of standard iptables | Yes    | Yes       | Yes, and other platforms |
| Language                          | Perl   | Perl      | C                        |
| IPv6 Support                      | Yes!   | Yes       | ???                      |

# License

For full licensing details, view the complete GPLv2 license.

husk is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as
published bythe Free Software Foundation, either version 2 of the License, or (at your option) any later version.

husk is distributed in the hope that it will be useful,but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.
