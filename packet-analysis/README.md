# Project 05: Packet Analysis with Wireshark

## Overview

This project focused on practical network packet analysis using Wireshark on Kali Linux.

After learning networking fundamentals in Project 04, this project examined the same concepts directly at the packet level.

The goal was to learn how to capture network traffic, filter large packet captures, inspect protocol fields, follow network conversations, and begin interpreting traffic from a security analyst perspective.

## Project Status

**Wireshark fundamentals:** Completed ✅  
**ARP and Ethernet analysis:** Completed ✅  
**ICMP and DNS analysis:** Completed ✅  
**TCP handshake analysis:** Completed ✅  
**TLS traffic analysis:** Completed ✅  
**Conversation analysis:** Completed ✅

## Objectives

- Capture live network traffic
- Identify the correct network interface for packet capture
- Understand Wireshark's main analysis panes
- Use display filters to isolate relevant traffic
- Inspect Ethernet frames and MAC addressing
- Analyze ARP requests and replies
- Inspect IPv4 packet headers
- Analyze ICMP Echo Requests and Replies
- Analyze DNS queries and responses
- Understand DNS A-record lookups
- Identify UDP and TCP traffic
- Analyze a TCP three-way handshake
- Inspect TCP control flags
- Isolate individual TCP streams
- Understand client and service ports
- Identify TLS Client Hello and Server Hello messages
- Recognize encrypted TLS application data
- Use Conversations to summarize communication between endpoints
- Begin applying packet analysis to SOC-style investigations

## Wireshark Fundamentals

Wireshark was launched from Kali Linux and the active Ethernet interface was selected for live packet capture.

The project introduced three primary areas of the Wireshark interface:

```text
Packet List
↓
Shows captured packets

Packet Details
↓
Shows protocols and individual protocol fields

Packet Bytes
↓
Shows the underlying captured data
```

This established the basic workflow for investigating captured traffic.

## Capturing Network Traffic

Live traffic was captured from the active Ethernet interface.

Rather than attempting to interpret every packet simultaneously, captures were stopped and analyzed using display filters.

Controlled traffic was also generated intentionally so that specific protocols could be identified and studied.

Examples included:

- Ping traffic
- DNS lookups
- HTTPS connections

This made it possible to connect specific user actions with the network packets they generated.

## Display Filters

Wireshark display filters were used throughout the project to reduce noise and focus on relevant traffic.

Examples included:

```text
arp
icmp
dns
tcp
tcp.flags.syn == 1
tcp.stream == 2
```

These filters demonstrated an important analysis principle:

```text
Large amount of traffic
        ↓
Filter relevant evidence
        ↓
Inspect specific packets
        ↓
Correlate activity
```

Display filtering allowed the original capture to remain intact while narrowing the packets shown for analysis.

## Ethernet Frames

Ethernet frame information was inspected by expanding:

```text
Ethernet II
```

This exposed fields including:

- Source MAC address
- Destination MAC address
- Ethernet type

Specific MAC addresses are intentionally excluded from this public documentation because they are unnecessary to demonstrate the learning outcome.

The exercise connected Layer 2 concepts from Project 04 directly with captured traffic.

## ARP Analysis

ARP traffic was isolated using:

```text
arp
```

A complete ARP exchange was captured and analyzed.

The request asked which system owned a particular local IPv4 address.

Conceptually:

```text
ARP Request
"Who has this IPv4 address?"
        ↓
ARP Reply
"This IPv4 address is associated with this MAC address."
```

Packet fields examined included:

- Opcode
- Sender IP address
- Sender MAC address
- Target IP address
- Target MAC address

This demonstrated how a system can discover the link-layer address associated with an IPv4 neighbor on the local network.

## IPv4 Packet Analysis

IPv4 information was inspected by expanding:

```text
Internet Protocol Version 4
```

Fields examined included:

- Source address
- Destination address
- Time To Live
- Protocol
- Packet length

This reinforced the layered relationship:

```text
Ethernet
   ↓
IPv4
   ↓
Transport or control protocol
```

## ICMP Analysis

Controlled ICMP traffic was generated using `ping`.

The Wireshark display filter:

```text
icmp
```

isolated the resulting packets.

Four ping requests produced four corresponding replies.

The packet flow was observed as:

```text
Host
   │
   │ ICMP Echo Request
   ▼
Gateway
   │
   │ ICMP Echo Reply
   ▼
Host
```

ICMP fields inspected included:

- Type
- Code
- Checksum
- Identifier
- Sequence number

An Echo Request and its corresponding Echo Reply were compared directly.

This demonstrated what occurs at the packet level when the `ping` command successfully verifies reachability.

## DNS Analysis

DNS traffic was generated using a hostname lookup and isolated using:

```text
dns
```

A DNS query and its corresponding response were captured.

The query requested an IPv4 address for a domain name using an A record.

Conceptually:

```text
Client
   │
   │ DNS Query
   │ "What IPv4 address belongs to this hostname?"
   ▼
DNS Resolver
   │
   │ DNS Response
   │ "Here are the IPv4 answers."
   ▼
Client
```

Fields examined included:

- Transaction ID
- DNS flags
- Question count
- Answer count
- Query name
- Query type
- Query class
- Answer records

The matching transaction ID demonstrated how a DNS response can be correlated with the corresponding query.

## UDP and DNS

The DNS capture also demonstrated UDP traffic.

The packet structure showed:

```text
Ethernet
   ↓
IPv4
   ↓
UDP
   ↓
DNS
```

The DNS request used the standard DNS service port.

This reinforced the relationship between:

```text
Protocol
+
IP address
+
Port
+
Application-layer service
```

## TCP Traffic

TCP traffic was isolated using:

```text
tcp
```

TCP SYN packets were then identified using:

```text
tcp.flags.syn == 1
```

This reduced a larger capture to packets involved in TCP connection establishment.

The analysis identified:

- Client IP
- Server IP
- Temporary client port
- Destination service port
- SYN packets
- SYN-ACK packets

## TCP Streams

A specific TCP conversation was isolated using a TCP stream filter.

Example:

```text
tcp.stream == 2
```

This transformed a large packet capture into one focused communication sequence.

The stream contained:

```text
TCP handshake
↓
TLS negotiation
↓
Encrypted application traffic
```

This demonstrated how packet analysts can move from a large capture to an individual conversation.

## TCP Three-Way Handshake

A complete TCP three-way handshake was captured and analyzed.

The sequence was:

```text
Client                         Server

SYN
----------------------------->

              SYN, ACK
<-----------------------------

ACK
----------------------------->

        Connection established
```

The individual TCP flags were inspected directly.

The first packet showed:

```text
SYN = Set
ACK = Not set
```

The second packet showed:

```text
SYN = Set
ACK = Set
```

The third packet showed:

```text
SYN = Not set
ACK = Set
```

This provided packet-level confirmation of how TCP establishes a connection before application communication begins.

## TCP Sequence and Acknowledgment Concepts

TCP sequence and acknowledgment numbers were introduced during handshake analysis.

At a foundational level:

```text
Sequence Number
↓
Tracks position in the sender's TCP byte stream

Acknowledgment Number
↓
Communicates what data the receiving side expects next
```

These concepts help TCP provide ordered and reliable communication.

More advanced TCP sequence analysis will be reinforced in future packet-analysis and incident-response exercises.

## Ports

The TCP stream demonstrated the difference between a temporary client port and a familiar service port.

Conceptually:

```text
Client IP : Temporary Port
          ↓
         TCP
          ↓
Server IP : Service Port
```

Port 443 was observed during TLS/HTTPS-related communication.

The project reinforced an important security-analysis principle:

```text
A familiar port is a clue,
not proof of which application
or protocol is actually responsible.
```

Additional evidence should be used to determine context.

## TLS Client Hello

After TCP connection establishment, TLS negotiation became visible.

The client sent:

```text
Client Hello
```

Fields exposed by Wireshark included information related to:

- Handshake type
- Version information
- Random values
- Session information
- Cipher suites
- Extensions
- Server-name information when visible

During analysis, the observed TLS hostname did not match the traffic originally expected.

Rather than forcing the packet evidence to fit the initial assumption, the evidence was treated as indicating a different browser-related connection.

This reinforced an important investigation principle:

```text
Follow the evidence,
not the expectation.
```

## TLS Server Hello

The server responded with:

```text
Server Hello
```

This demonstrated the beginning of TLS parameter negotiation between the client and server.

The traffic flow became:

```text
TCP connection established
        ↓
TLS Client Hello
        ↓
TLS Server Hello
        ↓
TLS negotiation
        ↓
Protected communication
```

## Encrypted TLS Application Data

After TLS negotiation, packets were observed containing:

```text
Encrypted Application Data
```

The protected application content was not treated as readable plaintext.

However, useful metadata remained available, including:

- Source and destination addresses
- Source and destination ports
- Protocol
- Packet direction
- Timing
- Packet size
- TCP stream
- Connection behavior
- Visible TLS negotiation information

This demonstrated an important security concept:

```text
Encrypted traffic
does not mean
invisible traffic.
```

Even when payload contents are protected, network metadata can still provide useful investigative context.

## Conversation Analysis

Wireshark's Conversations view was used to summarize communication between endpoints.

The TCP conversation view showed information including:

```text
Endpoint A
Port A
Endpoint B
Port B
Packet count
Byte count
Stream ID
```

This introduced a useful investigation workflow:

```text
Large capture
      ↓
Summarize conversations
      ↓
Identify interesting endpoints
      ↓
Select a TCP stream
      ↓
Inspect individual packets
      ↓
Analyze protocol fields
```

This approach is more efficient than manually reading every packet in a large capture.

## Guided SOC-Style Investigation

The captured traffic was used for a small guided security-analysis exercise.

Several outbound TCP connection attempts were identified.

The investigation asked:

- Which host initiated the connections?
- Which protocol was used?
- Which destination ports were involved?
- Did remote systems respond?
- Was a TCP connection successfully established?
- What protocol followed the TCP handshake?
- Did the network evidence alone prove malicious activity?

The evidence showed outbound TCP connections from the Kali system toward remote systems using a port commonly associated with HTTPS.

SYN packets were followed by SYN-ACK responses, demonstrating that the remote systems responded to the connection attempts.

TLS traffic followed successful TCP establishment.

The available evidence did not independently establish malicious activity.

This reinforced the principle that an analyst should gather additional context before assigning meaning to network activity.

## Troubleshooting Lessons

Several issues encountered during the project became useful learning opportunities.

Examples included:

- Initially capturing little or no visible traffic
- Generating controlled traffic to verify the capture interface
- Accidentally using a terminal already occupied by the Wireshark process
- Opening a second terminal for controlled traffic generation
- Filtering traffic to reduce noise
- Realizing that a captured TLS stream did not match the initially expected application activity
- Re-evaluating assumptions using packet evidence

The recurring troubleshooting workflow was:

```text
Observe
↓
Verify capture conditions
↓
Generate controlled traffic
↓
Filter
↓
Inspect packet fields
↓
Compare request and response
↓
Correlate the conversation
↓
Re-evaluate assumptions
↓
Reach a supported conclusion
```

## SOC Relevance

Packet analysis is useful in security operations because network captures can provide evidence about communication between systems.

A security analyst may need to determine:

- Which system initiated communication
- Which remote endpoint was contacted
- Which protocol was used
- Which ports were involved
- Whether a TCP connection completed successfully
- Whether DNS resolution occurred
- Whether ICMP communication succeeded
- Whether TLS encryption followed a TCP handshake
- How much traffic occurred between endpoints
- Which conversations deserve deeper investigation
- Whether network evidence supports or contradicts an initial hypothesis

Packet evidence is most useful when combined with endpoint, process, identity, log, and threat-intelligence context.

## What I Learned

After completing this project, I can:

- Capture live traffic using Wireshark
- Identify the appropriate capture interface
- Understand the Packet List, Packet Details, and Packet Bytes panes
- Apply Wireshark display filters
- Inspect Ethernet frames
- Identify source and destination MAC fields
- Analyze ARP requests and replies
- Inspect IPv4 packet headers
- Identify ICMP Echo Requests and Replies
- Analyze DNS queries and responses
- Recognize DNS A-record lookups
- Identify UDP-carried DNS traffic
- Filter TCP traffic
- Identify TCP SYN and SYN-ACK packets
- Isolate an individual TCP stream
- Verify a full TCP three-way handshake
- Inspect TCP control flags
- Understand the basic purpose of sequence and acknowledgment numbers
- Distinguish client-side and service-side ports
- Identify TLS Client Hello and Server Hello messages
- Recognize encrypted TLS application data
- Understand that encrypted traffic can still expose useful metadata
- Use Wireshark Conversations to summarize communication
- Begin interpreting packet evidence from a SOC perspective
- Re-evaluate an assumption when packet evidence points toward a different explanation

## Result

Project 05 established a practical foundation in packet capture and network traffic analysis.

The project transformed networking concepts from Project 04 into observable packet-level behavior, including Ethernet, ARP, IPv4, ICMP, DNS, UDP, TCP connection establishment, TLS negotiation, and encrypted application traffic.

The project also introduced the process of moving from individual packets to streams, conversations, and security-relevant conclusions.

## Next Project

**Project 06: Windows Security**

The next project will expand the homelab beyond Kali Linux and begin developing Windows-focused security knowledge.

Future exercises will introduce Windows system administration and defensive concepts that are important for SOC and cybersecurity analyst work.
