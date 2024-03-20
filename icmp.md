# Internet Control Message Protocol

ICMP is part of the Internet protocol suite as defined in RFC 792. ICMP messages
are typically used for diagnostic or control purposes or generated in response
to errors in IP operations (as specified in RFC 1122). ICMP errors are directed
to the source IP address of the originating packet.

## Header

| Field    |                    | Bit offset |
| -------- | ------------------ | ---------- |
| Type     | [ICMP type](#type) | 0          |
| Code     | ICMP subtype       | 8          |
| Checksum | Internet checksum  | 16         |
| Other    |                    | 32         |

## Type

| Type | Name                             |
| ---- | -------------------------------- |
| 0    | Echo Reply                       |
| 3    | Destination Unreachable          |
| 5    | Redirect                         |
| 8    | Echo                             |
| 9    | Route Advertisement              |
| 10   | Route Solicitation               |
| 11   | Time Exceeded                    |
| 12   | Parameter Problem: Bad IP Header |
| 13   | Timestamp                        |
| 14   | Timestamp Reply                  |

## Echo

## Redirect

See [routing](routing.md#redirect-error).

## Router Discovery

See [routing](routing.md#internet-router-discovery).

## Unreachable

## Time Exceed
