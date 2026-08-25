# Day 14: Unicast, Broadcast, and Multicast

## Overview
These are three fundamental communication modes in networking that differ based on how many devices receive the transmitted data.

---

## 1. UNICAST

### Definition
Unicast is a **one-to-one** communication model where data is sent from a single source to a single destination.

### Key Characteristics
- **Source:** Single sender
- **Destination:** Single receiver
- **Packet Forwarding:** Routers forward packets based on destination IP address
- **Most Common:** Default communication mode in networking
- **Efficiency:** Dedicated connection path

### How Unicast Works
```
Sender → Router → Router → Single Receiver
(e.g., 192.168.1.10 → 192.168.1.20)
```

### Examples
- **Email transmission** - One person sends email to another person
- **Web browsing** - Client requests a webpage from a server
- **SSH sessions** - Direct connection between client and server
- **Database queries** - Client connects to database server
- **FTP downloads** - Single source sends file to single destination
- **Telephone calls** - One person talks to another

### Advantages
✓ **Direct and reliable** - Ensures message reaches intended recipient
✓ **Security** - Only destination receives the message
✓ **Bandwidth efficient** - One transmission per recipient
✓ **Easy to implement** - Standard routing mechanisms
✓ **Predictable paths** - Well-defined routing

### Disadvantages
✗ **Inefficient for multiple recipients** - Need to send multiple copies for different destinations
✗ **Resource wastage** - Sender must replicate message for each recipient
✗ **Scalability issues** - Problems with high-volume distribution (e.g., live video to many users)
✗ **Network congestion** - Multiple identical packets consume bandwidth

### IPv4 Address Range
- **0.0.0.0 to 223.255.255.255**
- All class A, B, C private addresses are unicast

---

## 2. BROADCAST

### Definition
Broadcast is a **one-to-all** communication model where data is sent from a single source to ALL devices in the network segment/subnet.

### Key Characteristics
- **Source:** Single sender
- **Destination:** All devices in the network
- **Packet Forwarding:** Routers do NOT forward broadcast packets (limited to LAN)
- **Scope:** Limited to the same subnet/network segment
- **Address:** Special broadcast address (last address in subnet)
- **Cannot cross routers** - Broadcast is domain-limited

### How Broadcast Works
```
Single Sender → All Devices in the Network
(e.g., 192.168.1.255 broadcasts to all devices in 192.168.1.0/24)
```

### Broadcast Address Calculation
- **Formula:** Set all host bits to 1
- **Example:** 
  - Network: 192.168.1.0/24
  - Broadcast: 192.168.1.255 (all devices from .1 to .254 receive)

### Examples
- **ARP (Address Resolution Protocol)** - "Who has IP 192.168.1.5?" sent to all devices
- **DHCP Discovery** - New device broadcasts to find DHCP server
- **Wake-on-LAN** - "Magic packet" sent to wake up sleeping computers
- **Network announcements** - Printer discovery, file server availability
- **Router advertisements** - Routers broadcast their presence
- **Spanning Tree Protocol (STP)** - Bridge/switch convergence messages
- **Booting and initialization** - Devices broadcast during startup

### Advantages
✓ **One transmission for all** - Single packet reaches everyone
✓ **Simple mechanism** - No need for address lists
✓ **Device discovery** - Easy to find devices without knowing addresses
✓ **Efficient for network-wide messages** - No individual packet replication needed
✓ **Network synchronization** - All devices updated simultaneously

### Disadvantages
✗ **Network congestion** - All devices must process the packet
✗ **Performance impact** - CPU overhead on all hosts checking broadcast packets
✗ **Security risk** - Everyone sees the broadcast message
✗ **Cannot cross routers** - Limited scope (only LAN)
✗ **Broadcast storms** - Excessive broadcasts can paralyze network
✗ **Unwanted traffic** - Devices receive messages they might not need

### IPv4 Address Format
- **Last address in subnet** is broadcast address
- **Limited Broadcast:** 255.255.255.255 (entire network if no routing info)
- **Directed Broadcast:** Network broadcast address (e.g., 192.168.1.255)

### Broadcast Domain
- **Definition:** Group of devices that receive each other's broadcast frames
- **Boundaries:** 
  - Layer 2 switches forward broadcasts
  - Routers do NOT forward broadcasts (breaks broadcast domain)
  - Routers separate broadcast domains

---

## 3. MULTICAST

### Definition
Multicast is a **one-to-many** communication model where data is sent from a single source to a specific group of interested receivers.

### Key Characteristics
- **Source:** Single sender
- **Destination:** Multiple interested receivers (group)
- **Scope:** Can span multiple networks via multicast routing
- **Efficiency:** Single transmission, multiple receivers
- **Selective:** Only interested members receive the message
- **Address Range:** 224.0.0.0 to 239.255.255.255 (Class D addresses)
- **Port:** Combination of multicast IP + port identifies group

### How Multicast Works
```
Single Sender → Multicast Group (224.0.0.1, port 5000)
                    ↓
    [Receiver 1] [Receiver 2] [Receiver 3] [Receiver n]
    (subscribed)  (subscribed)  (subscribed)  (subscribed)
```

### Multicast Group Management
1. **IGMP (Internet Group Management Protocol)** - Hosts join/leave groups
2. **Multicast Routing** - PIM (Protocol Independent Multicast), DVMRP used
3. **Group membership** - Dynamic; hosts can join/leave anytime

### Multicast Address Classes (Reserved)
| Range | Purpose |
|-------|---------|
| 224.0.0.0 - 224.0.0.255 | Local Network Control (reserved) |
| 224.0.1.0 - 224.0.1.255 | Internetwork Control |
| 224.1.0.0 - 238.255.255.255 | User Multicast Addresses |
| 239.0.0.0 - 239.255.255.255 | Limited Scope (organizational use) |

### Examples
- **Live video streaming** - Sports events, conferences (YouTube Live, Twitch)
- **Online gaming** - Multiple players in same session
- **Stock market data** - Transmitted to multiple trading terminals
- **Online meetings** - Webinars with multiple participants
- **Multimedia distribution** - IPTV services
- **Sensor data** - IoT devices broadcasting to multiple collectors
- **DNS-SD (Service Discovery)** - mDNS on 224.0.0.251:5353
- **Network Time Protocol (NTP)** - Time synchronization across multiple servers

### IGMP (Internet Group Management Protocol)
**Purpose:** Manages multicast group membership

| Message Type | Purpose | Sent By |
|--------------|---------|---------|
| IGMP Membership Query | Router asks who wants to join | Router |
| IGMP Membership Report | Host requests to join group | Host |
| IGMP Leave Group | Host leaves multicast group | Host |

### Advantages
✓ **Efficient bandwidth** - Single transmission reaches multiple receivers
✓ **Scalability** - Can handle large number of receivers
✓ **Selective delivery** - Only interested parties receive data
✓ **Cross-network support** - Can span multiple networks
✓ **Real-time applications** - Ideal for streaming, live events
✓ **Reduced server load** - Server doesn't replicate for each client

### Disadvantages
✗ **Complex implementation** - Requires IGMP, multicast routing
✗ **Limited ISP support** - Many ISPs block multicast
✗ **UDP-based** - Unreliable (no guaranteed delivery)
✗ **Routing complexity** - Multicast routers more complex
✗ **Addresses can collide** - Same group address in different networks
✗ **Security challenges** - Difficult to control who joins group

### Required Protocols for Multicast
1. **IGMP** - Group membership management
2. **IGMP Snooping** - Switches learn multicast group membership
3. **Multicast Routing Protocols:**
   - **PIM-DM (Dense Mode)** - Good for small networks, flood-and-prune
   - **PIM-SM (Sparse Mode)** - Good for large networks, group joins
   - **DVMRP** - Legacy, rarely used
   - **MOSPF** - OSPF-based multicast

---

## Comparison Table

| Feature | Unicast | Broadcast | Multicast |
|---------|---------|-----------|-----------|
| **Sender** | One | One | One |
| **Receiver** | One | All in network | Specific group |
| **Packet Type** | Directed | Limited/Directed | Group address |
| **Router Forward** | Yes (via routing table) | No (stops at router) | Yes (with multicast routing) |
| **Scope** | Any network | LAN only | Multiple networks |
| **Efficiency** | Low (for many) | High (bandwidth) | Very high |
| **Scalability** | Poor | Fair | Excellent |
| **Reliability** | TCP/UDP | N/A | UDP |
| **Use Case** | 1-to-1 comm. | Network discovery | Live streaming |
| **Address Range** | All except 224+, 255 | Network broadcast | 224.0.0.0-239.255.255.255 |
| **Complexity** | Simple | Simple | Complex |

---

## Real-World Scenarios

### Scenario 1: YouTube Live Stream
- **Type:** Multicast (ideally)
- **How:** YouTube encodes once, multicast group receives stream
- **Reality:** Actually uses unicast (multiple unicast connections)
- **Why:** ISPs limit multicast, unicast more controllable

### Scenario 2: LAN ARP Request
- **Type:** Broadcast
- **How:** Device A sends "Who is 192.168.1.5?" to 192.168.1.255
- **Result:** Device with .5 replies (unicast) with its MAC address
- **Scope:** Limited to local network segment

### Scenario 3: Online Gaming
- **Type:** Unicast + Multicast hybrid
- **Unicast:** Server sends player position updates
- **Multicast:** Could broadcast physics events (usually unicast instead)
- **Modern:** Most use unicast to server, server manages distribution

### Scenario 4: DHCP Discovery
- **Type:** Broadcast (client-to-server discovery)
- **Steps:**
  1. Client broadcasts DHCPDISCOVER on 255.255.255.255
  2. DHCP server unicasts DHCPOFFER back
  3. Client broadcasts DHCPREQUEST
  4. Server unicasts DHCPACK

---

## Key Differences Summary

### Unicast
- **"Call a friend"** - Direct one-to-one conversation
- **Used:** Client-server communication, most web services
- **Transport:** TCP/UDP

### Broadcast
- **"Shout in a room"** - Everyone in the room hears
- **Used:** Finding devices, network discovery
- **Transport:** Layer 2/3 (not standard transport layer)

### Multicast
- **"Join a club"** - Only club members hear the announcement
- **Used:** Streaming, group communication
- **Transport:** UDP (unreliable but efficient)

---

## Summary

| Aspect | Unicast | Broadcast | Multicast |
|--------|---------|-----------|-----------|
| **Definition** | One sender, one receiver | One sender, all receivers | One sender, multiple interested receivers |
| **Best For** | Regular communication | Network discovery | Large-scale distribution |
| **Efficiency** | Low with many recipients | High but wastes network | Very high, selective |
| **Router Crossing** | Yes | No | Yes (with support) |
| **Common Issues** | High latency with many | Broadcast storms | Limited ISP support |

