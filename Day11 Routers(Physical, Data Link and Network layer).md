# Day 11: Routers (Physical, Data Link, and Network Layer)

## Overview
Routers are sophisticated networking devices that operate across multiple OSI layers (Layers 1, 2, and 3). Understanding how routers function at each layer is essential for comprehending network architecture, packet forwarding, and inter-network communication. This document provides an in-depth analysis of router operations across all three layers.

---

## 1. Router Architecture and Components

### Physical Components
- **Network Interface Cards (NICs):** Physical ports that connect to different networks
- **CPU (Processor):** Processes routing decisions and packet forwarding
- **Memory (RAM):** Stores routing tables, ARP tables, and packet buffers
- **ROM (Flash Memory):** Stores the router's operating system (IOS, RouterOS, etc.)
- **Backplane/Bus:** Internal communication pathway between components
- **Power Supply:** Provides power to all components
- **Cooling System:** Maintains optimal operating temperature for enterprise routers

### Software Components
- **Operating System:** Router OS (Cisco IOS, Juniper JunOS, etc.)
- **Routing Table:** Stores routes to destination networks
- **ARP Table:** Maps IP addresses to MAC addresses
- **Interface Configuration:** Settings for each network port
- **Access Control Lists (ACLs):** Security policies for filtering traffic
- **Forwarding Information Base (FIB):** Optimized copy of routing table for fast lookups

---

## 2. Layer 1 (Physical Layer) Operations

### Physical Layer Responsibilities of a Router

#### 2.1 Interface Types and Physical Connections
- **Ethernet Interfaces:** RJ-45 connectors for twisted-pair cables
  - Fast Ethernet (10/100 Mbps)
  - Gigabit Ethernet (1 Gbps)
  - 10 Gigabit Ethernet (10 Gbps)
- **Serial Interfaces:** For WAN connections (T1, E1, leased lines)
- **Fiber Optic Interfaces:** For long-distance, high-speed connections
- **Wireless Interfaces:** WiFi/802.11 for wireless connectivity

#### 2.2 Signal Transmission and Reception
- Converts digital data to electrical/optical signals
- Receives incoming signals and converts them back to digital data
- Maintains proper voltage levels and signal quality
- Implements Manchester encoding, NRZ (Non-Return to Zero), or other physical encoding schemes

#### 2.3 Clock Synchronization
- DCE (Data Communications Equipment) vs DTE (Data Terminal Equipment)
- Router can act as both DCE (providing clock) or DTE (receiving clock) on serial links
- Ensures synchronized data transmission across WAN links

#### 2.4 Connector Standards and Cable Types
| Cable Type | Connector | Speed | Distance | Use Case |
|---|---|---|---|---|
| Cat5e UTP | RJ-45 | 100 Mbps | 100m | Ethernet LAN |
| Cat6 UTP | RJ-45 | 1 Gbps | 100m | Gigabit Ethernet |
| Cat6a UTP | RJ-45 | 10 Gbps | 100m | High-speed Ethernet |
| Single-mode Fiber | LC, SC | 10+ Gbps | 10+ km | Long-distance WAN |
| Multi-mode Fiber | LC, SC | 10 Gbps | 2 km | Short-distance high-speed |
| Serial (RS-232/V.35) | DB-15, DB-60 | 64 Kbps-2 Mbps | 15m | WAN leased lines |

#### 2.5 Physical Layer Issues Handled by Routers
- **Attenuation:** Signal degradation over distance (solved with repeaters/amplifiers)
- **Noise:** Electrical interference managed through shielding and error detection
- **Bit Rate Synchronization:** Ensuring sender and receiver operate at same speed
- **Hardware Testing:** Self-test diagnostics (POST - Power-On Self-Test)

#### 2.6 Physical Layer Functions
```
Router Physical Layer Operations:
┌─────────────────────────────────────┐
│ Incoming Signal from Fiber/Ethernet │
│         (Optical/Electrical)         │
└────────────────┬────────────────────┘
                 │
         ┌───────▼────────┐
         │ Convert Signal  │
         │ to Binary Data  │
         └───────┬────────┘
                 │
         ┌───────▼────────────────┐
         │ Send to Data Link Layer │
         └────────────────────────┘
```

---

## 3. Layer 2 (Data Link Layer) Operations

### Data Link Layer Responsibilities of a Router

#### 3.1 Frame Handling
- **Frame Reception:** Router receives complete Layer 2 frames on incoming interfaces
- **Frame Decapsulation:** Removes the Layer 2 header and trailer
- **Frame Validation:** Checks FCS (Frame Check Sequence) for errors
- **Frame Encapsulation:** Adds new Layer 2 header and trailer before forwarding

#### 3.2 MAC Address Learning and Management

**ARP (Address Resolution Protocol)**
- Router maintains an ARP table mapping IP addresses to MAC addresses
- When router needs to forward packet to directly connected device, uses ARP
- ARP request: "Who has this IP address? Tell me your MAC address"
- ARP reply: Target device responds with its MAC address
- ARP table entries age out after timeout (typically 4 hours)

**ARP Table Example:**
```
Internet Address    Hardware Address    Type      Interface
192.168.1.1        aa:bb:cc:dd:ee:01   Dynamic   Ethernet0
192.168.1.100      aa:bb:cc:dd:ee:02   Dynamic   Ethernet0
192.168.2.1        22:33:44:55:66:77   Dynamic   Ethernet1
```

#### 3.3 MAC Address Operations

**Source MAC Address:**
- When router sends frame, uses its own MAC address as source MAC
- Different for each interface (each interface has unique MAC)
- Router MAC format: Depends on hardware manufacturer

**Destination MAC Address:**
- For directly connected devices: MAC of the destination device
- For remote devices: MAC of the next-hop router (not final destination)
- This is why MAC addresses need to be learned/resolved at each hop

**Example Scenario:**
```
Host A (192.168.1.10)  →  Router1 (192.168.1.1)  →  Router2 (192.168.2.1)  →  Host B (192.168.2.100)

Hop 1: Host A → Router1
  - Source MAC: MAC of Host A
  - Destination MAC: MAC of Router1's Ethernet0
  
Hop 2: Router1 → Router2
  - Source MAC: MAC of Router1's Ethernet1
  - Destination MAC: MAC of Router2's Ethernet0
  
Hop 3: Router2 → Host B
  - Source MAC: MAC of Router2's Ethernet1
  - Destination MAC: MAC of Host B
```

#### 3.4 VLAN Support
- Router can route between different VLANs (Inter-VLAN routing)
- Trunk ports on routers carry multiple VLAN traffic
- Router removes VLAN tag (802.1Q), processes packet, adds new VLAN tag
- Each subinterface can be assigned to different VLAN

**VLAN Tagging (802.1Q):**
```
VLAN Frame Structure:
├─ Destination MAC (6 bytes)
├─ Source MAC (6 bytes)
├─ VLAN Tag (4 bytes)
│  ├─ TPID: 0x8100
│  ├─ Priority (3 bits)
│  ├─ CFI (1 bit)
│  └─ VLAN ID (12 bits)
├─ Payload Type (2 bytes)
├─ Data (46-1500 bytes)
└─ FCS (4 bytes)
```

#### 3.5 Spanning Tree Protocol (STP)
- Routers don't participate in STP (only switches do)
- But routers need to be aware of STP topology
- Redundant links are managed by router failover mechanisms (HSRP, VRRP)

#### 3.6 Interface Modes
- **Access Port (on router):** Typically not used, but can be configured
- **Trunk Port (on router):** Carries multiple VLAN traffic tagged with 802.1Q
- **Router Subinterfaces:** Virtual interfaces for VLAN routing

**Subinterface Configuration Example:**
```
Router Interface Structure:
├─ FastEthernet0/0 (Physical)
│  ├─ FastEthernet0/0.10 (Subinterface for VLAN 10)
│  ├─ FastEthernet0/0.20 (Subinterface for VLAN 20)
│  └─ FastEthernet0/0.30 (Subinterface for VLAN 30)
```

#### 3.7 Layer 2 Data Link Protocols
Routers handle different Layer 2 protocols:
- **Ethernet (802.3):** Most common for LAN
- **PPP (Point-to-Point Protocol):** For WAN serial links
- **Frame Relay:** Legacy WAN protocol
- **HDLC (High-Level Data Link Control):** Default Cisco serial protocol
- **ATM (Asynchronous Transfer Mode):** Cell-based switching

#### 3.8 Frame Switching in Different Link Types

**Ethernet Frame:**
```
┌─────────────┬────────────┬──────┬────────┬──────┬───┐
│ Preamble/SFD│ Dest MAC   │ Src  │ Type   │ Data │FCS│
│ (8 bytes)   │ (6 bytes)  │ MAC  │ (2B)   │      │   │
│             │            │(6B)  │        │      │   │
└─────────────┴────────────┴──────┴────────┴──────┴───┘
```

**PPP Frame (for Serial Links):**
```
┌────┬────┬──────┬─────────┬────┬──────┐
│Flag│Addr│Control│Protocol │Data│CRC  │
│ 1B │ 1B │  1B   │  2B     │ ?  │ 2-4B│
└────┴────┴──────┴─────────┴────┴──────┘
```

#### 3.9 Data Link Layer Functions at Router
```
Data Link Layer Processing:

Incoming Interface:
  Physical Signal → Frame Reception → 
  MAC Address Check → VLAN Check → 
  FCS Validation → Forwarding Decision

Outgoing Interface:
  Get Layer 3 Packet → Determine Next-Hop MAC →
  Add Layer 2 Header/Trailer → Send to Physical Layer
```

---

## 4. Layer 3 (Network Layer) Operations

### Network Layer Responsibilities of a Router

#### 4.1 IP Addressing and Routing

**IPv4 Header Structure:**
```
┌─────┬─────┬──────┬────┬────┬────┬───┬───┬───┬──────┬────────┬────────┬────────┐
│Ver  │IHL  │ToS   │ Total Length   │ID │Flags│Offset│ TTL    │Protocol│Checksum│
│(4b) │(4b) │(8b)  │    (16 bits)   │   │     │      │(8 bits)│(8bits) │(16b)   │
└─────┴─────┴──────┴────┴────┴────┬───┴───┴───┴──────┴────────┴────────┴────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
             ┌──────▼──────┐          ┌──────────▼──────┐
             │ Source IP   │          │ Destination IP  │
             │ (32 bits)   │          │  (32 bits)      │
             └─────────────┘          └─────────────────┘
```

#### 4.2 Routing Table

**Routing Table Entry Structure:**
```
Destination Network | Subnet Mask | Next-Hop | Metric | Interface | Source
192.168.1.0         | 255.255.255.0| 0.0.0.0  | 0      | Eth0      | Connected
192.168.2.0         | 255.255.255.0| 10.0.0.2 | 1      | Eth1      | OSPF
0.0.0.0             | 0.0.0.0      | 10.0.0.1 | 100    | Eth1      | Static
```

**Routing Table Operations:**
- **Lookup:** Find matching route for destination IP
- **Longest Prefix Match:** Choose most specific route if multiple matches
- **Metric Comparison:** Choose route with lowest cost if same prefix length
- **Decision:** Forward packet out appropriate interface

#### 4.3 Routing Process

**Step-by-Step Routing Decision:**
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

4. Interface Selection
   └─ Determine outgoing interface from routing table

5. Next-Hop Resolution
   └─ For directly connected: ARP to find destination MAC
   └─ For remote destination: ARP to find next-hop router MAC

6. TTL Update
   └─ Decrement TTL by 1
   └─ Recalculate checksum

7. Forwarding
   └─ Send packet to Data Link Layer with new MAC addresses
```

**Example Routing Decision:**
```
Packet arrives with destination IP: 192.168.2.50

Routing Table:
  192.168.1.0/24    → Interface: Eth0 (Connected)
  192.168.2.0/24    → Next-Hop: 10.0.0.2, Interface: Eth1
  10.0.0.0/24       → Interface: Eth1 (Connected)
  0.0.0.0/0         → Next-Hop: 10.0.0.1 (Default)

Decision Process:
  ✓ 192.168.2.50 matches 192.168.2.0/24
  ✓ Next-hop is 10.0.0.2
  ✓ Send out interface Eth1
  ✓ Need MAC of 10.0.0.2
  ✓ Use ARP to resolve 10.0.0.2 → MAC address
  ✓ Forward packet with destination MAC = MAC of 10.0.0.2
```

#### 4.4 Longest Prefix Match Algorithm

```
Destination IP: 172.16.30.5

Routing Table:
  172.16.0.0/12      (12 bits match) ✓
  172.16.30.0/24     (24 bits match) ✓ ← SELECTED (longer prefix)
  172.0.0.0/8        (8 bits match)  ✓
  0.0.0.0/0          (always match)  ✓

Result: Route with /24 prefix is chosen as most specific
```

#### 4.5 TTL (Time To Live) Field

**Purpose:**
- Prevents packets from looping infinitely in network
- Each router decrements TTL by 1
- When TTL reaches 0, router discards packet and sends ICMP Time Exceeded message

**Example:**
```
Host A sends packet to Host B (unknown route)

Original TTL: 64
Router 1 → TTL: 63
Router 2 → TTL: 62
Router 3 → TTL: 61
...
Router 64 → TTL: 0 (DROPPED - ICMP Time Exceeded sent back)
```

**Traceroute Utility:** Uses TTL to discover router hops
```
tracert google.com output:
  1. 192.168.1.1 (TTL=1)
  2. 10.0.0.1 (TTL=2)
  3. 203.0.113.1 (TTL=3)
  ... continues until destination reached
```

#### 4.6 Fragmentation and MTU

**Maximum Transmission Unit (MTU):**
- Maximum size of packet that can be transmitted on interface
- Ethernet default: 1500 bytes
- Serial links: 1500 bytes typical
- Fiber: Can support larger MTUs (jumbo frames: 9000 bytes)

**Fragmentation Process:**
```
If packet > MTU of outgoing interface:

Option 1: Fragment the packet
  Large Packet (3000 bytes, MTU=1500)
  ├─ Fragment 1 (1500 bytes, MF=1)
  ├─ Fragment 2 (1500 bytes, MF=0)
  
  Each fragment has:
  - Same packet ID
  - Different offsets
  - MF (More Fragments) flag
  
  Receiver reassembles fragments using ID and offset fields

Option 2: Drop packet (if DF flag set)
  - Send ICMP: Fragmentation Needed
  - Sender uses PMTUD (Path MTU Discovery) to find largest safe size
```

#### 4.7 ICMP (Internet Control Message Protocol)

Router uses ICMP for network diagnostics:
- **Echo Request/Reply:** Ping utility
- **Destination Unreachable:** No route to destination
- **Time Exceeded:** TTL reached 0
- **Redirect:** Inform host of better route
- **Parameter Problem:** Invalid packet

```
ICMP Message Types:
8  → Echo Request (Ping request)
0  → Echo Reply (Ping response)
3  → Destination Unreachable
11 → Time Exceeded
5  → Redirect
```

#### 4.8 Routing Protocols

**Static Routing:**
- Administrator manually configures routes
- No overhead, predictable
- Used for small networks or specific routes
```
Command: ip route 192.168.2.0 255.255.255.0 10.0.0.2
Meaning: To reach 192.168.2.0, send via next-hop 10.0.0.2
```

**Dynamic Routing - Distance Vector (RIP, IGRP, EIGRP):**
```
Router shares entire routing table with neighbors
R1 ├─ Metric: 1 → R2 ├─ Metric: 1 → R3
    │                 │
    └─ Metric: 2 ────┘

R1 learns: R3 is 2 hops away via R2
R1 route: 192.168.3.0/24 → Next-hop R2, Metric 2
```

**Dynamic Routing - Link State (OSPF, IS-IS):**
```
Each router knows:
- State of all links
- Cost of each link
- Calculates shortest path using Dijkstra's algorithm

Router floods Link State Advertisements (LSAs)
All routers build identical topology database
Each router independently calculates best routes
```

**Dynamic Routing - Path Vector (BGP):**
```
Primarily for internet routing between ASes (Autonomous Systems)
Path: AS65001 ← AS65002 ← AS65003 ← AS65004
BGP advertises: "Reach network X via this AS path"
Routers can avoid loops by checking if their AS is in path
```

#### 4.9 Routing Metrics and Path Selection

**Metric Comparison:**
| Protocol | Metric | Example |
|---|---|---|
| RIP | Hop count | 3 routers = metric 3 |
| IGRP | Bandwidth + Delay | 256×(delay + bandwidth) |
| EIGRP | Weighted composite | K1=bandwidth, K2=delay, K3=reliability, K4=load, K5=MTU |
| OSPF | Cost | 100 Mbps / link bandwidth |
| BGP | AS-path length | Prefer shorter paths |

#### 4.10 Default Route and Route Summarization

**Default Route:**
```
Destination: 0.0.0.0/0
Mask: 255.255.255.0
Matches any IP address (default)
Used when no specific route found
```

**Route Summarization (Aggregation):**
```
Specific routes:
  192.168.0.0/24
  192.168.1.0/24
  192.168.2.0/24
  192.168.3.0/24

Can be summarized to:
  192.168.0.0/22 (covers all four /24 networks)

Benefits: Smaller routing tables, faster lookups
```

#### 4.11 Layer 3 Processing Flow

```
Incoming Packet:
  ├─ Extract Layer 3 header
  ├─ Verify checksum
  ├─ Check destination IP
  ├─ Lookup routing table
  ├─ Find matching route
  ├─ Select outgoing interface
  ├─ Decrement TTL
  ├─ Recalculate checksum
  ├─ Remove incoming Layer 2 frame
  ├─ Add new Layer 2 frame with next-hop MAC
  └─ Send to outgoing interface
```

---

## 5. Complete Packet Forwarding Example

### Multi-Layer Processing Illustration

```
Scenario: Host A (192.168.1.10) sends to Host C (192.168.3.100)
Network: A ← Router1 ← Router2 ← C

┌─────────────────────────────────────────────────────────────┐
│ HOST A: 192.168.1.10 sends packet to 192.168.3.100         │
└─────────────────────────────────────────────────────────────┘

┌─ LAYER 3: Create IP Packet
│  ├─ Source IP: 192.168.1.10
│  ├─ Destination IP: 192.168.3.100
│  ├─ TTL: 64
│  └─ Checksum: calculated

┌─ LAYER 2: Create Ethernet Frame (Host A → Router1)
│  ├─ Source MAC: MAC_HostA
│  ├─ Destination MAC: MAC_Router1_Eth0
│  └─ Type: 0x0800 (IPv4)

┌─ LAYER 1: Send electrical signal on UTP cable


────────────────────────────────────────────────────────────────

┌─ ROUTER1 RECEIVES (Interface Eth0/0)
│
├─ LAYER 1: Receive electrical signal
│  └─ Convert to binary data
│
├─ LAYER 2: Process Ethernet frame
│  ├─ Extract source MAC: MAC_HostA
│  ├─ Extract destination MAC: MAC_Router1_Eth0 ✓ (for me)
│  ├─ Extract type: 0x0800 ✓ (IPv4)
│  ├─ Verify FCS ✓ (valid)
│  └─ Pass to Layer 3
│
├─ LAYER 3: Process IP packet
│  ├─ Extract destination IP: 192.168.3.100
│  ├─ Lookup routing table
│  │  ├─ 192.168.1.0/24 → Connected via Eth0/0
│  │  ├─ 192.168.2.0/24 → Next-hop 192.168.2.2 via Eth0/1
│  │  └─ 192.168.3.0/24 → Next-hop 192.168.2.2 via Eth0/1
│  ├─ Best match: 192.168.3.0/24 → Route via Eth0/1
│  ├─ Decrement TTL: 64 → 63
│  ├─ Recalculate checksum
│  └─ Need MAC of next-hop (192.168.2.2)
│
├─ ARP RESOLUTION (Layer 2/3)
│  ├─ Check ARP table for 192.168.2.2
│  ├─ If found: Use cached MAC → MAC_Router2_Eth0/0
│  └─ If not found: Send ARP request "Who has 192.168.2.2?"
│
├─ LAYER 2: Create new Ethernet frame (Router1 Eth0/1 → Router2 Eth0/0)
│  ├─ Source MAC: MAC_Router1_Eth0/1
│  ├─ Destination MAC: MAC_Router2_Eth0/0
│  ├─ Type: 0x0800 (IPv4)
│  ├─ Data: IP packet (with TTL=63)
│  └─ FCS: calculated
│
├─ LAYER 1: Send via Eth0/1
│  └─ Send electrical signal on serial link (PPP)


────────────────────────────────────────────────────────────────

┌─ ROUTER2 RECEIVES (Interface Eth0/0)
│
├─ LAYER 1: Receive signal from serial link
│
├─ LAYER 2: Process PPP frame
│  ├─ Check frame validity
│  ├─ Extract payload (IP packet)
│  └─ Pass to Layer 3
│
├─ LAYER 3: Process IP packet
│  ├─ Extract destination IP: 192.168.3.100
│  ├─ Lookup routing table
│  │  ├─ 192.168.1.0/24 → Next-hop 192.168.2.1 via Eth0/0
│  │  ├─ 192.168.2.0/24 → Connected via Eth0/0
│  │  └─ 192.168.3.0/24 → Connected via Eth0/1 ✓
│  ├─ Best match: 192.168.3.0/24 → Out Eth0/1 (Connected)
│  ├─ Decrement TTL: 63 → 62
│  ├─ Recalculate checksum
│  └─ Destination 192.168.3.100 is directly connected
│     Need MAC of 192.168.3.100
│
├─ ARP RESOLUTION
│  ├─ Send ARP request: "Who has 192.168.3.100?"
│  └─ Host C responds with MAC_HostC
│
├─ LAYER 2: Create Ethernet frame (Router2 Eth0/1 → Host C)
│  ├─ Source MAC: MAC_Router2_Eth0/1
│  ├─ Destination MAC: MAC_HostC
│  ├─ Type: 0x0800 (IPv4)
│  ├─ Data: IP packet (with TTL=62)
│  └─ FCS: calculated
│
├─ LAYER 1: Send via Eth0/1
│  └─ Send electrical signal on UTP cable


────────────────────────────────────────────────────────────────

┌─ HOST C RECEIVES
│
├─ LAYER 1: Receive electrical signal
│  └─ Convert to binary
│
├─ LAYER 2: Verify Ethernet frame
│  ├─ Destination MAC: MAC_HostC ✓ (for me)
│  └─ Extract payload
│
├─ LAYER 3: Process IP packet
│  ├─ Destination IP: 192.168.3.100 ✓ (for me)
│  ├─ TTL check: 62 > 0 ✓
│  ├─ Checksum valid ✓
│  └─ Pass to application layer
│
└─ Host C processes packet and sends reply
```

---

## 6. Special Router Functions

### 6.1 Network Address Translation (NAT)

**Purpose:** Convert private IP addresses to public IPs

```
Private Network: 192.168.1.0/24
Public IP: 203.0.113.5

Outgoing: 192.168.1.100:5000 → 203.0.113.5:5000
Router changes source IP and port

Incoming replies: 203.0.113.5:5000 → 192.168.1.100:5000
Router looks up NAT table and converts back

Benefits:
- Save public IP addresses
- Hide internal network structure
- Basic security through obscurity
```

### 6.2 DHCP Relay

```
DHCP Request: 192.168.1.100 (no IP yet)
  └─ Broadcast: Who will give me an IP?
  └─ Reaches Router (does not cross router normally)

Router sees DHCP Broadcast
  └─ Converts to unicast
  └─ Sends to DHCP Server (192.168.2.100)

DHCP Server responds
  └─ "You get 192.168.1.101"

Router forwards reply back to client
```

### 6.3 Access Control Lists (ACLs)

```
Standard ACL (Layer 3 only):
  permit 192.168.1.0 0.0.0.255    (allow this subnet)
  deny any                         (deny everything else)

Extended ACL (Layer 3 + Layer 4):
  permit tcp 192.168.1.0 0.0.0.255 any eq 80   (allow HTTP)
  permit tcp 192.168.1.0 0.0.0.255 any eq 443  (allow HTTPS)
  deny ip any any                               (deny all else)

Applied to interface:
  ip access-group 101 in   (filter incoming)
  ip access-group 101 out  (filter outgoing)
```

### 6.4 Quality of Service (QoS)

```
Router can prioritize traffic:
- VoIP packets: High priority (buffer size: 1KB)
- Video streaming: Medium priority (buffer size: 10KB)
- File downloads: Low priority (buffer size: 100KB)

Uses traffic classes:
  class-map VoIP
    match protocol voip
    
  policy-map Qos_Policy
    class VoIP
      priority percent 30      (guarantee 30% bandwidth)
      
  service-policy output Qos_Policy
```

---

## 7. Types of Routers

### 7.1 Core Routers
- Sit at internet backbone
- Handle millions of packets/second
- Very high throughput (terabits/sec)
- No direct connection to end users
- Examples: Cisco ASR9000, Juniper MX series

### 7.2 Edge Routers
- Connect ISP network to customer networks
- Handle BGP routing
- Apply policies, QoS, NAT
- Examples: Cisco ASR1000, Edge routers at data centers

### 7.3 Access Routers
- Connect end user to ISP
- Consumer or small business routers
- Built-in switch, WiFi, firewall
- Examples: Home routers, SOHO routers

### 7.4 Enterprise Routers
- Mid-size to large organization networks
- Multiple interfaces, high throughput
- Advanced routing features
- Examples: Cisco ISR4000 series

### 7.5 Virtual Routers
- Software-based routing
- Run on x86 servers or cloud
- Examples: VyOS, Cumulus Linux, OpenStack

---

## 8. Router Interface Types in Detail

### 8.1 Ethernet Interface
```
Speed: 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps
Layer 1: Electrical signals in twisted pair
Layer 2: Ethernet frames (802.3)
Layer 3: IP addressing

Media Types:
  100BASE-TX (Cat5, 100m)
  1000BASE-T (Cat5e, 100m)
  10GBASE-T (Cat6a, 100m)
```

### 8.2 Serial Interface (WAN)
```
Speed: 64 Kbps to 2.5 Gbps
Layer 1: V.35, X.21, or RS-232 signaling
Layer 2: HDLC, PPP, Frame Relay
Layer 3: IP addressing

Setup: Router acts as DCE or DTE
  DCE: Provides clock signal
  DTE: Receives clock signal
  
Typical setup: Router = DTE, CSU/DSU = DCE
```

### 8.3 Fiber Optic Interface
```
Speed: 1 Gbps to 400 Gbps+
Layer 1: Optical signals in fiber
Layer 2: Ethernet frames
Layer 3: IP addressing

Types: Single-mode, Multi-mode
  Single-mode: 10+ km distance
  Multi-mode: 2 km distance
  
Connectors: LC, SC, ST
```

### 8.4 Wireless Interface
```
Layer 1: 802.11 radio signals
Layer 2: WiFi frames, security (WPA2/WPA3)
Layer 3: IP addressing

Frequency: 2.4 GHz, 5 GHz, 6 GHz
Speed: 11 Mbps to 3 Gbps (WiFi 6/6E)

Used in: Access points, router AP mode
```

---

## 9. Summary Table: Router Operations Across Layers

| Aspect | Layer 1 (Physical) | Layer 2 (Data Link) | Layer 3 (Network) |
|---|---|---|---|
| **Unit of Data** | Bits | Frames | Packets |
| **Addressing** | Physical connector | MAC address | IP address |
| **Key Devices** | NIC, connectors | ARP table | Routing table |
| **Main Function** | Signal transmission | Frame delivery | Packet routing |
| **Typical Protocol** | Ethernet PHY, PPP PHY | Ethernet, PPP, HDLC | IP (IPv4/IPv6) |
| **Errors Checked** | Bit errors, signal quality | FCS (CRC) | Checksum, TTL |
| **Forwarding Decision** | None | Destination MAC | Destination IP |
| **Rewrite Headers** | None | Entire L2 header | TTL only |
| **Latency Impact** | Minimal | Low | Medium (lookup) |
| **Speed** | Fastest | Very fast | Slower than L2 (lookup) |

---

## 10. Key Concepts and Formulas

### 10.1 MTU and Fragmentation
```
MTU = Maximum Transmission Unit (bytes)
Packet Size > MTU → Fragmentation needed

Fragment Offset = (Byte Position / 8)
Example: 1500-byte packet, MTU=500
  Fragment 1: Bytes 0-499, Offset=0
  Fragment 2: Bytes 500-999, Offset=62.5 (actually 62)
  Fragment 3: Bytes 1000-1499, Offset=125
```

### 10.2 CIDR Notation
```
IP/PrefixLength where PrefixLength = network bits

Examples:
  192.168.1.0/24 = 192.168.1.0 with 24 network bits
  Hosts: 192.168.1.1 to 192.168.1.254 (256 addresses)
  
  10.0.0.0/8 = 10.0.0.0 with 8 network bits
  Hosts: 10.0.0.1 to 10.255.255.254 (16 million addresses)
```

### 10.3 Subnet Mask to Prefix Length
```
Subnet Mask: 255.255.255.0
Binary: 11111111.11111111.11111111.00000000
Count 1s: 24
Prefix Length: /24
```

---

## 11. Practice Scenarios

### Scenario 1: Multi-Router Packet Forwarding
```
Setup:
  Host A (172.16.1.50) needs to reach Host B (172.16.3.100)
  Path: Host A → Router1 → Router2 → Host B
  
  Router1:
    Eth0: 172.16.1.1/24 (connected to Host A's network)
    Eth1: 172.16.2.1/24 (connected to Router2)
    Route: 172.16.3.0/24 via 172.16.2.2 (Router2's Eth0)
    
  Router2:
    Eth0: 172.16.2.2/24 (connected to Router1)
    Eth1: 172.16.3.1/24 (connected to Host B's network)
    Route: 172.16.1.0/24 via 172.16.2.1 (Router1's Eth1)

Question: What are the MAC addresses in each frame?

Answer:
  Host A → Router1 Frame:
    Source MAC: MAC_HostA
    Dest MAC: MAC_Router1_Eth0
    Dest IP: 172.16.3.100
    
  Router1 → Router2 Frame:
    Source MAC: MAC_Router1_Eth1
    Dest MAC: MAC_Router2_Eth0
    Dest IP: 172.16.3.100 (unchanged)
    TTL: 63 (decremented from 64)
    
  Router2 → Host B Frame:
    Source MAC: MAC_Router2_Eth1
    Dest MAC: MAC_HostB
    Dest IP: 172.16.3.100 (unchanged)
    TTL: 62
```

### Scenario 2: Fragmentation Decision
```
Host A sends 2500-byte packet to Host B

Hops:
  Path 1: A → R1 (MTU=1500) → R2 (MTU=576) → B

At R1:
  - Receives 2500-byte packet, MTU=1500
  - 2500 > 1500, so fragment
  - Creates 2 fragments:
    Fragment 1: 1500 bytes (includes 20-byte header)
      - Payload: 1480 bytes
      - Offset: 0
      - MF: 1 (more fragments)
    Fragment 2: 1020 bytes (includes 20-byte header)
      - Payload: 1000 bytes
      - Offset: 185 (1480/8)
      - MF: 0 (last fragment)

At R2:
  - Receives Fragment 1 (1500 bytes), MTU=576
  - 1500 > 576, need to fragment again
  - Creates fragments:
    F1: 576 bytes (payload 556)
    F2: 576 bytes (payload 556)
    F3: 348 bytes (payload 328)
  - Same process for Fragment 2
  
At Host B:
  - Receives all fragments with same ID
  - Reassembles using offset field
  - Final packet: 2500 bytes
```

---

## 12. Interview Questions

1. **What's the difference between switching and routing?**
   - Switching (L2): Forwards frames based on MAC addresses within same network
   - Routing (L3): Forwards packets based on IP addresses between different networks

2. **Why does router decrement TTL?**
   - Prevents infinite loops in case of routing loops
   - After 255 hops, packet is discarded

3. **What is ARP and when is it used?**
   - Resolves IP addresses to MAC addresses
   - Used when router needs to send frame to next-hop or destination

4. **Explain the longest prefix match algorithm**
   - Router selects most specific (longest) matching route
   - Ensures correct path even if multiple routes could match

5. **What's the purpose of a default route?**
   - Matches any destination IP (0.0.0.0/0)
   - Used when no specific route found
   - Typically points to ISP gateway

6. **How does fragmentation work?**
   - Packet larger than MTU is split into smaller pieces
   - Each fragment has same ID, different offset
   - Receiver reassembles using ID and offset fields

7. **What is NAT and why use it?**
   - Translates private IPs to public IPs
   - Saves public IP addresses, provides basic security

8. **Explain VLAN routing**
   - Each VLAN gets different subinterface
   - Router separates traffic between VLANs
   - Enables communication between VLANs

---

## 13. Key Takeaways

✓ **Layer 1 (Physical):** Routers receive electrical/optical signals, convert to bits
✓ **Layer 2 (Data Link):** Routers rewrite source and destination MAC addresses for each hop
✓ **Layer 3 (Network):** Routers lookup routing table, decrement TTL, forward based on destination IP
✓ **Key difference from switches:** Routers separate broadcast domains, switches don't
✓ **ARP is critical:** Router must resolve IP to MAC for each hop
✓ **MAC changes at each hop:** Data Link addresses change, IP addresses don't
✓ **TTL prevents loops:** Each router decrements TTL, packet expires after 255 hops
✓ **Routing is hierarchical:** Longest prefix match ensures correct path selection

---

## 14. Related Topics to Study

- **OSPF:** Detailed study of Link-State protocol
- **BGP:** Inter-AS routing protocol
- **ACLs and Firewalls:** Advanced filtering
- **QoS and Traffic Engineering:** Bandwidth management
- **VPN and Tunneling:** Secure remote access
- **Load Balancing:** Traffic distribution
- **Redundancy and Failover:** High availability

---

**Last Updated:** Day 11 - Comprehensive Router Study
**Difficulty Level:** Intermediate to Advanced
**Prerequisites:** Understanding of OSI model, IP addressing, subnetting
