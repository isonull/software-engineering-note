# Internet Protocol

## Function

The function or purpose of Internet Protocol is to move datagrams
through an interconnected set of networks. This is done by passing
the datagrams from one internet module to another until the
destination is reached. The internet modules reside in hosts and
gateways in the internet system. The datagrams are routed from one
internet module to another through individual networks based on the
interpretation of an internet address.

- Addressing
- Fragmentation

What IP does NOT provide:

- No acknowledgment
- No error control other than checksum
- No retransmission
- No flow control

## Mechanism

- Type of service
- Time to live
- Header checksum
- Options

## Header

| Group              | Field                  | Description                      | Bit Offset | Bit Size |
| ------------------ | ---------------------- | -------------------------------- | ---------- | -------- |
|                    | Version                | IP version (IPv4/IPv6)           | 0          | 4        |
|                    | IHL                    | Internet header length           | 4          | 4        |
| Quality of Service | Precedence             | Precedence                       | 8          | 3        |
| Quality of Service | Flag 0                 | Low delay                        | 11         | 1        |
| Quality of Service | Flag 1                 | High throughput                  | 12         | 1        |
| Quality of Service | Flag 2                 | High Reliability                 | 13         | 1        |
| Quality of Service | ECN                    | Explicit congestion notification | 14         | 2        |
|                    | Total length           | IP packet Length                 | 16         | 16       |
| Fragmentation      | Identification         | Identification                   | 32         | 16       |
| Fragmentation      | Flag 0                 | Reserved                         | 48         | 1        |
| Fragmentation      | Flag 1                 | Do not fragment                  | 49         | 1        |
| Fragmentation      | Flag 2                 | More fragments                   | 50         | 1        |
| Fragmentation      | Fragment offset        | Fragment offset                  | 51         | 13       |
|                    | TTL                    | Time To Live                     | 64         | 8        |
|                    | Protocol               | Protocol in IP data field        | 72         | 8        |
|                    | Header checksum        | Header checksum                  | 80         | 16       |
|                    | Source IP Address      | Source IP address                | 96         | 32       |
|                    | Destination IP Address | Destination IP address           | 128        | 32       |
|                    | Options                | Options                          | ?          | ?        |
|                    | Padding                | Ensure header 32-bit alignment   | ?          | ?        |
