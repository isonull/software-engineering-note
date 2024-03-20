# Address Resolution Protocol

In a local network, a device want to communicate with another device with known
IP address but unknown MAC address.

## ARP table

On routers

- IP address
- MAC address

## ARP table learning

- Request: broadcast the source IP/MAC address with the target IP address.
- Reply: with target MAC address.

## ARP packet

| Byte Offset | Field                     |
| ----------- | ------------------------- |
| 0           | Hardware type             |
| 2           | Protocol type             |
| 4           | Hardware length           |
| 5           | Protocol length           |
| 6           | Operation                 |
| 8           | Sender hardware address   |
| 14          | Sender protocol address   |
| 18          | Receiver hardware address |
| 24          | Receiver protocol address |
| 28          |                           |
