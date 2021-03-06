OSLR UDP Broadcast Packet Relay
===============================

This program listens for packets on a specified UDP port on the IPv4
Broadcast address (255.255.255.255). When a packet is received, it
sends that packet to all specified interfaces but the one it came
from as though it originated from the original sender, which is
needed if you want transparently transfer oslr connections over
a node.

Transparent OLSR
----------------

If you want to have an olsr link briged, means a relay not seen as
a oslr hop, you have on many systems the problem that wlan interfaces
cannot be bridged. You can realise the same with proxy arp, a similary
thing to bridging. The problem is that via this way the olsr broadcasts
won't get forwarded.

The benefit of proxy-arp instead of bridging is, that no broadcasts but
oslrd is flooded through the link. Beside of the broadcasts you have
the full functionality as on bridging.

On your linux you can do this the following way:

    echo 1 > /proc/sys/net/ipv4/conf/wlan0/proxy_arp 
    echo 1 > /proc/sys/net/ipv4/conf/eth0/proxy_arp 
    echo 1 > /proc/sys/net/ipv4/conf/eth1/proxy_arp 
    ...
    echo 1 > /proc/sys/net/ipv4/ip_forward
    /some/where/udp-broadcast-relay -f 1 698 wlan0 eth0 eth1 ...

For example if you have a interface wlan0 and eth0 you do:

    echo 1 > /proc/sys/net/ipv4/conf/wlan0/proxy_arp 
    echo 1 > /proc/sys/net/ipv4/conf/eth0/proxy_arp 
    echo 1 > /proc/sys/net/ipv4/ip_forward
    /some/where/udp-broadcast-relay -f 1 698 wlan0 eth0

INSTALL
-------

    make 
    cp udp-broadcast-relay /some/where

USAGE
-----

    /some/where/udp-broadcast-relay id udp-port eth0 eth1...

udp-broadcast-relay must be run as root to be able to create a raw
socket (necessary) to send packets as though they originated from the
original sender.

COMPATIBILITY
-------------

-   Linux 2.4.x or later is enough.

EXAMPLE
-------

    /some/where/udp-broadcast-relay -f 1 698 wlan0 eth0  # Forwards OSLR packets between wlan0 and eth0

CONTRIBUTORS
-----------------

Over the last years, various people submitted code to the project. Note that I
do not use udp-broadcast-relay any more myself, so these changes were not
tested by me.

-   Patrick Huesmann submitted a patch to make udp-broadcast-relay send
    the packes to those NICs it did not recieve it from, based on the
    actual socket, not the broadcast IP. This is useful if more than one
    physical networks share the same broadcast range.
-   Савченко В. М. submitted an `ip-up.local` an `ip-down.local` file to
    automatically restart udp-broadcast-relay when new ppp-interfaces
    come up, see `ppp-if.up-local` for details.
-   Roman Hoog Antink contributed the option `-s` to spoof the source IP of
    forwarded packages.

Thanks to all contributors!

BUGS/CRITICISM/PATCHES/ETC
--------------------------

-   Web: <http://projekte.priv.de/>
-   e-mail:  Markus Schräder <<olsrudpbroadcast2014@priv.de>>
-   Github: <https://github.com/pRiVi/olsr-relay>

HISTORY
-------

*   0.4 2014-04-05

    Support for redirecting OLSR packets

*   0.3 2003-09-28

    Sending packets also to ppp addresses

*   0.2 2003-09-18

    Flags for debugging and forking, Compilefixes, Makefile-Target
    "clean"

*   0.1 2003-09-15

    Initial rewrite of udp_broadcast_fw

CREDITS
-------

This is based upon [udp_broadcast_fw](http://www.serverquery.com/udp_broadcast_fw/) by Nathan O'Sullivan and the fork [udp-broadcast-relay](http://www.joachim-breitner.de/udp-broadcast-relay/) by Joachim Breitner.

LICENSE
-------

This code is made available under the GPL. Read the COPYING file inside
the archive for more info.
