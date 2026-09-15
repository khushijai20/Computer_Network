# IPv4 (Internet Protocol Version 4) - Complete Guide

## Overview
**IPv4** is the primary version of the Internet Protocol used for logical addressing and routing in computer networks. It operates at **Layer 3 (Network Layer)** of the OSI model and provides a connectionless, unreliable best-effort delivery service.

---

## Key Characteristics

| Characteristic | Details |
|---|---|
| **Bit Length** | 32 bits |
| **Address Format** | Dotted decimal notation (e.g., 192.168.1.1) |
| **Total Addresses** | 2³² = 4,294,967,296 (~4.3 billion) |
| **Delivery Type** | Connectionless & Unreliable (Best-Effort) |
| **PDU** | Packet/Datagram |
| **Typical Header Size** | 20 bytes (minimum) |
| **Maximum Header Size** | 60 bytes (with options) |
| **Standards** | RFC 791 |

---

## IPv4 Address Structure

### Address Format
```
192.168.1.1
│   │   │  │
│   │   │  └─ Host ID (0-255)
│   │   └──── Subnet (0-255)
│   └─────── Network (0-255)
└────────── Class identifier (0-255)
```

### Address Classes
| Class | Range | Purpose |
|-------|-------|---------|
| **A** | 1.0.0.0 - 126.255.255.255 | Large networks (16 million hosts) |
| **B** | 128.0.0.0 - 191.255.255.255 | Medium networks (65,000 hosts) |
| **C** | 192.0.0.0 - 223.255.255.255 | Small networks (254 hosts) |
| **D** | 224.0.0.0 - 239.255.255.255 | Multicast addresses |
| **E** | 240.0.0.0 - 255.255.255.255 | Reserved/Experimental |

### Special IP Addresses
| Address | Purpose |
|---------|---------|
| 0.0.0.0 | Default/unspecified |
| 127.x.x.x | Loopback (localhost) |
| 255.255.255.255 | Broadcast |
| 224.0.0.0 - 239.255.255.255 | Multicast |
| 169.254.0.0 - 169.254.255.255 | Link-local (APIPA) |
| 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 | Private addresses |

---

## IPv4 Header Structure

### IPv4 Header Format
```
┌─────────────────────────────────────────────────────────────────┐
│ IPv4 HEADER (20 bytes minimum)                                  │
├─────────────────────────────────────────────────────────────────┤
│ Version | IHL | DSCP | ECN | Total Length (16 bits)            │
│ Identification (16) | Flags | Fragment Offset (13 bits)         │
│ Time To Live (TTL) | Protocol | Header Checksum (16 bits)      │
│ Source IP Address (32 bits)                                     │
│ Destination IP Address (32 bits)                                │
│ Options (if IHL > 5)                                             │
├─────────────────────────────────────────────────────────────────┤
│ PAYLOAD (Data from Transport Layer)                             │
└─────────────────────────────────────────────────────────────────┘
```

### IPv4 Header Fields Explained

| Field | Size | Purpose |
|-------|------|---------|
| **Version** | 4 bits | IP version number (4 for IPv4) |
| **IHL** (Internet Header Length) | 4 bits | Header length in 32-bit words (minimum 5, maximum 15) |
| **DSCP** (Differentiated Services Code Point) | 6 bits | Quality of Service (QoS) marking |
| **ECN** (Explicit Congestion Notification) | 2 bits | Network congestion notification |
| **Total Length** | 16 bits | Total packet size (header + data), max 65,535 bytes |
| **Identification** | 16 bits | Unique identifier for packet fragments |
| **Flags** | 3 bits | Control flags (DF=Don't Fragment, MF=More Fragments, Reserved) |
| **Fragment Offset** | 13 bits | Position of fragment in original packet (in 8-byte units) |
| **TTL** (Time To Live) | 8 bits | Hops remaining before packet discarded (decremented at each router) |
| **Protocol** | 8 bits | Upper layer protocol (TCP=6, UDP=17, ICMP=1, IGMP=2) |
| **Header Checksum** | 16 bits | Verifies header integrity (recalculated at each hop) |
| **Source IP Address** | 32 bits | Sending device's IP address |
| **Destination IP Address** | 32 bits | Receiving device's IP address |
| **Options** | Variable | Optional features (if IHL > 5) |

---

## IPv4 Functions & Responsibilities

### 1. **Logical Addressing**
- Assigns 32-bit logical addresses to identify devices
- Enables communication across multiple networks
- Independent from physical (MAC) addresses

### 2. **Routing**
- Determines best path from source to destination
- Examines destination IP and consults routing table
- Supports:
  - **Static Routing**: Manually configured by administrators
  - **Dynamic Routing**: Automatically adapts using routing protocols (RIP, OSPF, BGP, EIGRP)

### 3. **Forwarding**
- Moves packets from input to output interface based on routing decision
- Performs hop-by-hop forwarding at each router
- Updates MAC addresses at each hop (for Layer 2)

### 4. **Fragmentation & Reassembly**
- **Fragmentation**: Breaks large packets into smaller fragments if MTU (Maximum Transmission Unit) is exceeded
- **Reassembly**: Reconstructs fragmented packets at destination
- **MTU Size**: Typically 1500 bytes for Ethernet

### 5. **Quality of Service (QoS)**
- Prioritizes traffic based on type and importance
- Manages packet delay, jitter, and bandwidth allocation
- Uses DSCP and ECN fields in header

### 6. **Error Handling**
- Performs checksum verification on IP header
- Decrements TTL at each hop
- Discards packets when TTL reaches 0
- Generates ICMP messages for errors

### 7. **Congestion Control**
- Monitors network congestion
- Implements traffic shaping and rate limiting
- Uses feedback mechanisms to inform upper layers

### 8. **Protocol Translation**
- Handles **Network Address Translation (NAT)** for private/public IP conversion
- Manages protocol encapsulation/decapsulation

---

## IPv4 Communication Types

### 1. **Unicast** (One-to-One)
- Packet sent to single destination IP
- Most common type of communication
- Example: 192.168.1.5

### 2. **Broadcast** (One-to-All)
- One-to-all communication within network segment
- Destination address: 255.255.255.255
- Limited to local network (not routed across networks)
- Examples: ARP requests, DHCP discovery

### 3. **Multicast** (One-to-Many)
- One-to-many communication
- Destination range: 224.0.0.0 to 239.255.255.255
- Used for video streaming, IPTV, online gaming
- Only interested recipients receive the packet

### 4. **Anycast** (One-to-Nearest)
- Multiple nodes share same IP
- Packet sent to closest matching address
- Used for content delivery networks

---

## Related IPv4 Protocols

### **ICMP (Internet Control Message Protocol)**
- Sends error reports and diagnostic messages
- Used by **ping** (Echo/Echo Reply) and **traceroute** (Time Exceeded)
- Messages: Echo, Echo Reply, Destination Unreachable, Time Exceeded, Redirect

### **ARP (Address Resolution Protocol)**
- Maps IPv4 addresses to MAC addresses on local network
- Operates at boundary between Layer 2 and Layer 3
- **ARP Request**: "Who has this IPv4 address? Tell me your MAC address"
- **ARP Reply**: "I have that IP, my MAC is xxxxxxxx"

### **IGMP (Internet Group Management Protocol)**
- Manages multicast group membership
- Allows hosts to join/leave multicast groups

---

## Routing with IPv4

### Routing Table Structure
```
Destination Network | Subnet Mask | Next-Hop | Metric | Interface | Source
192.168.1.0         | 255.255.255.0| 0.0.0.0  | 0      | Eth0      | Connected
192.168.2.0         | 255.255.255.0| 10.0.0.2 | 1      | Eth1      | OSPF
0.0.0.0             | 0.0.0.0      | 10.0.0.1 | 100    | Eth1      | Static (Default)
```

### Routing Process
```
1. Packet Arrival
   └─ Extract destination IP from Layer 3 header

2. Routing Table Lookup
   └─ Search for matching destination network
   └─ Use longest prefix match algorithm

3. Route Selection
   └─ If match found → proceed
   └─ If no match and default route exists → use default
   └─ If no match and no default → drop packet (ICMP unreachable)

4. Forwarding
   └─ Send packet out appropriate interface
   └─ Update TTL (decrement by 1)
   └─ Recalculate header checksum
```

### Longest Prefix Match
- Selects most specific route if multiple matches
- Example: If packet matches both 192.168.0.0/16 and 192.168.1.0/24, use /24

---

## Subnet Mask & Subnetting

### Subnet Mask Function
- Defines network and host portions of IPv4 address
- Example: 192.168.1.0 with mask 255.255.255.0
  - Network: 192.168.1.0
  - Host range: 192.168.1.1 to 192.168.1.254
  - Broadcast: 192.168.1.255

### CIDR Notation
- **Classless Inter-Domain Routing**
- Notation: IP/prefix length
- Example: 192.168.1.0/24 (same as 192.168.1.0 with mask 255.255.255.0)

---

## IPv4 Challenges & Limitations

### 1. **Address Exhaustion**
- Only 4.3 billion addresses available
- Insufficient for growing number of devices
- Solutions: NAT, IPv6, CIDR

### 2. **Security Issues**
- **IP Spoofing**: Forged source addresses
- **DDoS Attacks**: Overwhelming routers with malicious traffic
- Solutions: Firewalls, IPSec, ingress filtering

### 3. **Fragmentation Problems**
- Reduces throughput and increases latency
- Path MTU Discovery helps mitigate

### 4. **Interoperability**
- Different network technologies must work together
- NAT complexities with P2P and multimedia

### 5. **Efficiency**
- TTL mechanism can be inefficient for multi-hop paths
- Load balancing challenges

---

## IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| **Address Bit Length** | 32 bits | 128 bits |
| **Total Addresses** | 4.3 billion | 340 trillion trillion |
| **Address Format** | Dotted decimal | Colon hexadecimal |
| **Header Checksum** | Yes | No |
| **Fragmentation** | Router & host | Host only |
| **Security** | Optional (IPSec) | Built-in (IPSec) |
| **Header Size** | 20-60 bytes | 40 bytes (fixed) |
| **Quality of Service** | DSCP/ECN | Traffic Class/Flow Label |

---

## IPv4 vs Data Link Layer (Layer 2)

| Aspect | Data Link Layer (Layer 2) | Network Layer (Layer 3) |
|--------|--------------------------|------------------------|
| **Addressing** | MAC addresses | IPv4 addresses |
| **Scope** | Same network segment | End-to-end across networks |
| **Devices** | Switches, bridges | Routers, Layer 3 switches |
| **PDU** | Frame | Packet/Datagram |
| **Delivery** | Reliable (for some) | Unreliable (best-effort) |
| **Hop Scope** | One hop | Multiple hops |

### Key Difference
- **Layer 2 MAC addresses change at each hop**
- **Layer 3 IPv4 addresses remain same throughout journey**

---

## IPv4 vs Transport Layer (Layer 4)

| Aspect | Network Layer (Layer 3) | Transport Layer (Layer 4) |
|--------|-------------------------|--------------------------|
| **Addressing** | IPv4 addresses | Port numbers |
| **Delivery** | Host-to-host | Process-to-process |
| **Connection** | Connectionless | Connection-oriented (TCP) or Connectionless (UDP) |
| **Protocols** | IPv4, ICMP, IGMP | TCP, UDP, SCTP |
| **Reliability** | No guarantee | TCP ensures in-order delivery |

---

## IPv4 Packet Journey Example

```
Source Host (192.168.1.100)
         ↓
1. Create IP header with source IP (192.168.1.100) and destination IP (8.8.8.8)
2. Set TTL = 64
3. Set Protocol = 6 (TCP) or 17 (UDP)
         ↓
Router 1 (Gateway 192.168.1.1)
1. Receive frame with IPv4 packet
2. Extract destination IP: 8.8.8.8
3. Lookup routing table
4. Find next hop: 10.0.0.2
5. Decrement TTL: 64 → 63
6. Recalculate checksum
7. Update MAC addresses (Source MAC: Router1, Dest MAC: Router2)
8. Forward to next router
         ↓
Router 2 (10.0.0.2)
1. Repeat process
2. TTL: 63 → 62
         ↓
... (multiple hops)
         ↓
Destination Host (8.8.8.8)
1. Receive frame with IPv4 packet
2. Check destination IP matches its own
3. Extract source IP: 192.168.1.100
4. Extract protocol field to know which transport protocol handled this
5. Pass payload to transport layer
```

---

## IPv4 Configuration Tools

### Windows
```
ipconfig /all          # Display IPv4 address, subnet mask, gateway, DNS
ipconfig /renew        # Renew DHCP lease
ipconfig /release      # Release DHCP lease
```

### Linux/Mac
```
ifconfig               # Display IP configuration
ip addr show           # Modern IP display command
ip route show          # Display routing table
ping <IP>              # Test connectivity
traceroute <IP>        # Trace path to destination
```

---

## Summary

**IPv4 is essential for:**
✅ Identifying devices uniquely across networks  
✅ Routing packets to correct destinations  
✅ Enabling communication between multiple networks  
✅ Supporting quality of service and congestion control  
✅ Providing error handling and diagnostic capabilities  

**Current Status:**
- Still the dominant IP version (alongside IPv6)
- Addresses are increasingly scarce (NAT, private addresses used extensively)
- IPv6 slowly replacing due to address exhaustion
- Many networks run dual-stack (IPv4 + IPv6)

---

## References
- RFC 791 — Internet Protocol (IPv4)
- RFC 2460 — Internet Protocol, Version 6 (IPv6)
- RFC 3986 — Uniform Resource Identifier (URI) Generic Syntax
