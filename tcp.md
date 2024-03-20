# Transmission Control Protocol

## Operation

- Basic data transfer
- Reliability: No damage, lost, duplicate or out-of-order
- Flow Control: The receiver governs the data sent by the sender.
- Multiplexing: A single host can handle multiple TCP communication simultaneously.
- Connections: A connection handles status supporting reliability and flow-control.
- Precedence and Security: ?

## Header Structure

| Field                 |                                                | Bit offset | Bit size |
| --------------------- | ---------------------------------------------- | ---------- | -------- |
| Source Port           |                                                | 0          | 16       |
| Destination Port      |                                                | 16         | 16       |
| Sequence Number       | The SN of the first data octet in the segment  | 32         | 32       |
| Acknowledgment Number | The next SN the sender is willing to receive   | 64         | 32       |
| Data Offset           |                                                | 96         | 4        |
| Reserved              |                                                | 100        | 6        |
| Control Flags         |                                                | 106        | 6        |
| Window                | Number of octets is willing to receive from AN | 112        | 16       |
| Checksum              |                                                | 128        | 16       |
| Urgent Pointer        |                                                | 144        | 16       |
| Options               |                                                | 160        | Variable |
| Padding               |                                                | Variable   | Variable |

Control Flags

| Flag | Description                  |
| ---- | ---------------------------- |
| URG  | Urgent pointer is valid      |
| ACK  | Acknowledgment is valid      |
| PSH  | Receiver should push ASAP    |
| RST  | Reset the connection         |
| SYN  | Synchronize sequence numbers |
| FIN  | No more data from sender     |

## Terminology

|         |                                                           |
| ------- | --------------------------------------------------------- |
| SND.UNA | Oldest unacknowledged sent SN                             |
| SND.NXT | Next SN to be sent                                        |
| SND.WND | Send window size                                          |
| SND.UP  | Send urgent pointer                                       |
| SND.WL1 | segment sequence number used for last window update       |
| SND.WL2 | segment acknowledgment number used for last window update |
| ISS     | Initial send sequence number                              |
| RCV.NXT | Next SN to receive                                        |
| RCV.WND | Receive window size                                       |
| RCV.UP  | receive urgent pointer                                    |
| IRS     | Initial receive sequence number                           |
| SEG.SEQ | SN of the first octet                                     |
| SEG.ACK | Next SN expected                                          |
| SEG.LEN | Data length                                               |
| SEG.WND | segment window                                            |
| SEG.UP  | segment urgent pointer                                    |
| SEG.PRC | segment precedence value                                  |
| TCB     | Transmission control block                                |

## Status Machine

R: receive
S: send
P: Passive
A: Active
+: Create
-: Delete

| From       | Trigger      | Action      | To         |
| ---------- | ------------ | ----------- | ---------- |
| CLOSED     | P OPEN       | + TCB       | LISTEN     |
| CLOSED     | A OPEN       | + TCB S SYN | SYN-SENT   |
| LISTEN     | SEND         | S SYN       | SYN-SENT   |
| LISTEN     | R SYN        | S SYN+ACK   | SYN-RCVD   |
| LISTEN     | CLOSE        | - TCB       | CLOSED     |
| SYN-SENT   | CLOSE        | - TCB       | CLOSED     |
| SYN-SENT   | R SYN        | S ACK       | SYN-RCVD   |
| SYN-SENT   | R SYN+ACK    | S ACK       | ESTAB      |
| SYN-RCVD   | R ACK of SYN |             | ESTAB      |
| SYN-RCVD   | CLOSE        | S FIN       | FIN-WAIT-1 |
|            |              |             |            |
| ESTAB      | CLOSE        | S FIN       | FIN-WAIT-1 |
| ESTAB      | R FIN        | S ACK       | CLOSE-WAIT |
| FIN-WAIT-1 | R FIN        | S ACK       | CLOSING    |
| FIN-WAIT-1 | R ACK of FIN |             | FIN-WAIT-2 |
| FIN-WAIT-2 | R FIN        | S ACK       | TIME-WAIT  |
| CLOSING    | R ACK of FIN |             | TIME-WAIT  |
| CLOSE-WAIT | CLOSE        | S FIN       | LAST-ACK   |
| LAST-ACK   | R ACK of FIN | - TCB       | CLOSED     |
| TIME-WAIT  | timeout 2MSL | - TCB       | CLOSED     |

## Window

The sender will send data in the allowed window part.
The sender will not send data in the not-allowed window part.
The sender will retransmit data in the unacknowledged part when it timeouts.

| Send Window Part | Begin           |
| ---------------- | --------------- |
| Acknowledged     | ISS             |
| Unacknowledged   | SND.UNA         |
| Allowed          | SND.NXT         |
| Not allowed      | SND.UNA+SND.WND |

The receiver will only receive data in the allowed window part.

| Receive Window Part | Begin           |
| ------------------- | --------------- |
| Acknowledged        | IRS             |
| Allowed             | RCV.NXT         |
| Not allowed         | RCV.NXT+RCT.WND |

## Maximum Segment Size

Finished during three way handshake in option field.

Avoid IP fragmentation which leads to overhead especially in high packet loss
environment.

Path Maximum Transmission Unit Discovery (PMTUD)

## Connection Establishment

See [state machine](#status-machine)

The main purpose of TCP handshake is to exchange the necessary information
including initial sequence number to setup initial states of the connection.

CASES NEED MORE VERIFICATION.

### Normal Three-Way Handshake

| Status 1    |     | SN  | AN  | CTL     |     | Status 2     |
| ----------- | --- | --- | --- | ------- | --- | ------------ |
| CLOSED      |     |     |     |         |     | LISTEN       |
| SYN-SENT    | >   | 100 |     | SYN     | >   | SYN-RECEIVED |
| ESTABLISHED | <   | 300 | 101 | SYN,ACK | <   | SYN-RECEIVED |
| ESTABLISHED | >   | 101 | 301 | ACK     | >   | ESTABLISHED  |

### Simultaneous Open

Both sides send SYN segments and then respond with SYN-ACK segments
simultaneously.

|     | Status 1     |     | SN  | AN  | CTL     |     | Status 2     |
| --- | ------------ | --- | --- | --- | ------- | --- | ------------ |
|     | CLOSED       |     |     |     |         |     | LISTEN       |
| 1   | SYN-SENT     | >   | 100 |     | SYN     |     | LISTEN       |
|     | SYN-RECEIVED | <   | 300 |     | SYN     | <   | SYN-SENT     |
| 1   |              |     | 100 |     | SYN     | >   | SYN-RECEIVED |
| 2   | SYN-RECEIVED | >   | 100 | 301 | SYN,ACK |     |              |
|     | ESTABLISHED  | <   | 300 | 101 | SYN,ACK | <   | SYN-RECEIVED |
| 2   |              |     | 101 | 301 | ACK     | >   | ESTABLISHED  |

### Half-Open Connection

A connection where one side has sent a SYN segment but has not yet received a
response.  This could occur if the SYN segment is lost, and the sender needs to
retransmit it.

### SYN Flooding

An attack where an attacker sends a flood of SYN segments to a server to exhaust
its resources.  The server may respond with SYN-ACK segments to the spoofed IP
addresses, leading to half-open connections.

### Zero Window Probe

Occurs when one side has data to send, but the receiver's window size is zero,
indicating it cannot accept any more data.  The sender may periodically send a
segment with no data (a probe) to check if the receiver's window has increased
(zero window probing).

### Fast Open

A mechanism introduced in TCP to reduce latency in connection establishment.
Allows data to be sent in the initial SYN segment, eliminating the need for a
separate round-trip for the data exchange.  Requires support from both the
client and server, and the server must have cached state for the client.

### TCP Retransmission Timeout (RTO)

If a sender does not receive an acknowledgment for a segment within the
retransmission timeout period, it may retransmit the segment.  This can occur
during the initial handshake or during data transmission.

### Connection Re-establishment

Occurs when an existing TCP connection is terminated due to some reason (e.g.,
network failure, timeout, application termination) and needs to be
re-established.  The process typically involves closing the existing connection
(four-way handshake) and then initiating a new connection (three-way handshake).

## Connection Termination

### Normal Termination

| Status 1     |     | SN  | AN  | CTL     |     | Status 2    |
| ------------ | --- | --- | --- | ------- | --- | ----------- |
| ESTABLISHED  |     |     |     |         |     | ESTABLISHED |
| FIN-WAIT-1   | >   | 100 | 300 | FIN,ACK | >   | CLOSE-WAIT  |
| FIN-WAIT-2   | <   | 300 | 101 | ACK     | <   | CLOSE-WAIT  |
| TIME-WAIT    | <   | 300 | 101 | FIN,ACK | <   | LAST-ACK    |
| TIME-WAIT    | >   | 101 | 301 | ACK     | >   | CLOSED      |
| CLOSED 2 MSL |     |     |     |         |     |             |

There is no guarantee that IP packet will be delivered in order. Therefore,
the sender of the last ACK waits further for 2 maximum segment lifetime (MSL)
for delayed segments. Further more, this prevents the confusion of the
subsequent connection using the same port.

### Simultaneous Termination

|     | Status 1     |     | SN  | AN  | CTL     |     | Status 2     |
| --- | ------------ | --- | --- | --- | ------- | --- | ------------ |
|     | ESTABLISHED  |     |     |     |         |     | ESTABLISHED  |
| 1   | FIN-WAIT-1   | >   | 100 | 300 | FIN,ACK |     |              |
|     | CLOSING      | <   | 300 | 100 | FIN,ACK | <   | FIN-WAIT-1   |
| 1   |              |     | 100 | 300 | FIN,ACK | >   | CLOSING      |
| 2   |              | >   | 101 | 301 | ACK     |     |              |
|     | TIME-WAIT    | <   | 301 | 101 | ACK     | <   |              |
| 2   |              |     | 101 | 301 | ACK     | >   | TIME-WAIT    |
|     | CLOSED 2 MSL |     |     |     |         |     | CLOSED 2 MSL |

### Half-Open Connection Detection

One side of the connection abruptly terminates without sending a FIN segment.
The other side, after a period of time (typically governed by a timeout), detects that no more data is being received and assumes the connection is half-open.
The side detecting the half-open connection sends its own FIN segment to
initiate termination.

### RST (Reset) Segment

A host sends a TCP RST segment to abruptly terminate a connection in response to an unexpected or invalid condition.
This can occur if a host encounters an error or receives a segment for a
non-existent or closed connection.

### Idle Timeout

A connection is terminated due to inactivity or exceeding a predefined idle timeout period.
Either host may close the connection if no data has been exchanged for a
specified duration.

### Abnormal Termination

Connection termination may occur abruptly due to network failures, crashes, or other abnormal conditions.
In such cases, hosts may not follow the standard termination process and may not
send FIN segments.

### Rage Quit

A host forcefully closes the connection without following the proper termination process.
This can occur due to an application crash or intentional termination without
properly closing the TCP connection.
