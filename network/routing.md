# Routing

## Routing Table

| Field       | Description                                        |
| ----------- | -------------------------------------------------- |
| Destination | Destination network or IP address                  |
| Gateway     | Next hop IP address or interface                   |
| Interface   | Network interface associated with the route        |
| Metric      | Metric or cost associated with the route           |
| Expire      | Expiration time for dynamic routes (if applicable) |

## Static Routing

For small network with single connection point to other networks without
redundant routes. If any of these conditions failed, dynamic routing is normally
used.

## Internet Router Discovery

The ICMP Router Solicitation message is sent from a computer host to any routers
on the local area network to request that they advertise their presence on the
network.
The ICMP Router Advertisement message is sent by a router on the local area
network to announce its IP address as available for routing.

## Host and Network Unreachable Error

The ICMP "host unreachable" error message is sent by a router when it receives
an IP datagram that it cannot deliver or forward.

## Redirect Error

When router 1 receives the datagram, determines router 2 is the next hop
and forwards the datagram to router 2.  If router 2 has the same interface as
the datagram arrived router 1 will send a ICMP redirect to the datagram sender
telling it to send future datagrams to router 2.

## Dynamic Routing

Dynamic routing occurs when routers talk to adjacent routers, informing each
other of what networks each router is currently connected to.
A *routing protocol* is how routers communicate.
A *routing daemon* is the process on the router communicate with neighbor routers
which updates the routing table.

The Internet is a network of networks, and *autonomous systems* (AS) are the big
networks that make up the Internet. More specifically, an AS is a large network
or group of networks that has a unified routing policy. Every computer or device
that connects to the Internet is connected to an AS.

An *interior gateway protocol* (IGP) or Interior routing protocol is a type of
routing protocol used for exchanging routing table information between gateways
(commonly routers) within an autonomous system.

### Routing Information Protocol (RIP)

RFC 1058
