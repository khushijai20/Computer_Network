# Day 10: Bridge, Switch and Hub

## Overview
This note explains three important networking devices used to connect computers and network segments: bridge, switch, and hub. These devices help in communication between hosts, but they differ in how intelligently they forward data and how they handle traffic.

## 1. Hub

### What it is
A hub is a basic networking device that connects multiple computers in the same network segment. It acts like a shared connector and sends incoming data to all connected ports.

### OSI layer
- Operates at Layer 1 (Physical Layer)

### How it works
- Receives data from one port.
- Broadcasts the same data to all other ports.
- All devices connected to the hub share the same medium and same collision domain.

### Key characteristics
- No inspection of destination addresses
- No intelligent forwarding decisions
- All connected devices receive the same data

### Types of hub
- Passive hub: just connects wires without amplification
- Active hub: boosts and regenerates the signal
- Intelligent hub: includes some management features, but still behaves like a simple Layer 1 device

### Uses
- Very small networks
- Legacy Ethernet setups
- Simple connection of multiple devices in a shared segment

### Advantages
- Cheap and simple
- Easy to install
- Useful for very small networks

### Disadvantages
- All devices share one collision domain
- High collisions and network congestion
- Low efficiency because every device receives all traffic
- Bandwidth is wasted
- Mostly replaced by switches in modern networking

### Example
If computer A sends data to computer B using a hub, all devices connected to that hub receive the data. Only B accepts it, but the rest also receive it.

---

## 2. Bridge

### What it is
A bridge is a network device that connects two or more network segments and forwards frames based on MAC addresses. It is used to divide a large network into smaller parts to reduce traffic.

### OSI layer
- Operates at Layer 2 (Data Link Layer)

### How it works
- Receives a frame from one segment.
- Reads the source and destination MAC addresses.
- Learns which device is connected to which segment.
- Builds a MAC address table.
- Forwards the frame only to the appropriate segment when needed.

### Main purpose
- Reduce unnecessary traffic
- Connect different network segments
- Reduce collisions in a busy network

### Types of bridges
- Transparent bridge: learns addresses automatically.
- Source-route bridge: used in Token Ring networks and forwards based on routing info inside the frame.
- Learning bridge: dynamically builds a table of MAC addresses.

### Uses
- Segmenting a LAN into smaller parts
- Connecting two Ethernet segments
- Reducing traffic and improving performance

### Advantages
- Filters traffic and sends it only to the correct segment
- Creates separate collision domains
- Improves performance compared to a hub
- Helps isolate network problems

### Disadvantages
- Not as fast or as efficient as a modern switch
- Can still forward broadcasts
- Requires learning time for MAC addresses
- Not suitable for large-scale networks as compared to switches

### Example
If Device A in segment 1 wants to talk to Device B in segment 2, the bridge forwards the frame only to segment 2 instead of broadcasting to all segments.

---

## 3. Switch

### What it is
A switch is a multiport network device that connects multiple devices in a LAN and forwards data based on MAC addresses. It is essentially an advanced and more efficient version of a bridge.

### OSI layer
- Operates at Layer 2 (Data Link Layer)
- Some switches can also work at Layer 3 and are called multilayer switches

### How it works
- Receives Ethernet frames on one port.
- Reads the source MAC address and stores it in a MAC address table.
- Reads the destination MAC address.
- Forwards the frame only to the port where the destination device is located.
- If the destination is unknown, the switch broadcasts the frame to all ports except the source port.

### Features of switch
- Learns MAC addresses automatically
- Stores a MAC table
- Works faster than a bridge
- Provides full-duplex communication

### Types of switch
- Unmanaged switch: plug-and-play, no configuration required
- Managed switch: supports VLANs, QoS, security, monitoring, port control
- Smart switch: a middle option between unmanaged and fully managed
- Multilayer switch: supports both Layer 2 switching and Layer 3 routing

### Uses
- Connecting computers, printers, servers, and access points within the same LAN
- Building modern office and campus networks
- Segmenting traffic using VLANs
- High-speed local communication

### Advantages
- Efficient data forwarding
- Creates separate collision domains for each port
- Reduces unnecessary traffic
- Better performance than hub and bridge
- Supports VLANs and security features in managed switches

### Disadvantages
- Does not route between different IP networks unless it is a multilayer switch
- Broadcast traffic still exists within a VLAN
- More expensive than a hub
- Requires configuration in managed models

### Example
In an office with 20 computers, the switch forwards data only to the port connected to the destination computer rather than sending it to all 20 devices.

---

## 4. Bridge vs Hub vs Switch

| Device | OSI Layer | Data Forwarding | Traffic Handling | Collision Domain | Typical Use |
|---|---|---|---|---|---|
| Hub | Layer 1 | Broadcast to all ports | Sends every frame everywhere | One shared collision domain | Small/legacy LANs |
| Bridge | Layer 2 | Forwards by MAC address | Filters and forwards only when needed | Separate collision domains | Connects segments |
| Switch | Layer 2 | Forwards by MAC address | Efficient selective forwarding | One per port | Modern LANs |

## 5. Important Concepts

### Collision Domain
A collision domain is a network segment where packets may collide with each other during transmission.
- Hub: one collision domain for all connected devices
- Bridge: separates collision domains
- Switch: usually one collision domain per port

### Broadcast Domain
A broadcast domain is a network area where broadcast messages are propagated.
- Hub: same broadcast domain for all devices
- Bridge: can reduce broadcasts to some extent
- Switch: same broadcast domain within a VLAN, unless routed

### MAC Address
A MAC address is a unique hardware address assigned to a network interface card (NIC).
- Bridge and switch use MAC addresses to decide where to forward data.
- Hubs do not inspect MAC addresses.

### Learning Table
- A bridge and switch learn which MAC address belongs to which port.
- This is called a MAC address table or CAM table.

---

## 6. Bridge, Switch, and Hub in One View

### Hub
- Very simple device
- Sends data to all ports
- No intelligence
- Works at physical layer
- All devices share one collision domain

### Bridge
- More intelligent than a hub
- Uses MAC addresses
- Connects two LAN segments
- Reduces collisions by filtering traffic
- Works at data link layer

### Switch
- Advanced version of a bridge
- Has multiple ports
- Learns and forwards based on MAC addresses
- Efficient and widely used in modern networks
- Works at data link layer

---

## 7. Key Differences

### Hub vs Bridge
- Hub broadcasts traffic to all devices.
- Bridge learns MAC addresses and forwards only where required.
- Hub is slower and less efficient.
- Bridge improves network performance.

### Bridge vs Switch
- Both operate at Layer 2.
- Switch is basically a multiport bridge with better speed and features.
- Switch handles many ports efficiently and supports modern networking needs.
- Bridge is used to connect smaller segments, while switch is used in full LAN networks.

### Hub vs Switch
- Hub is simple and shared; switch is intelligent and efficient.
- Hub has one collision domain; switch has dedicated collision domains per port.
- Hub wastes bandwidth; switch reduces unnecessary traffic.

---

## 8. Notes
- Hubs are largely outdated in modern networks.
- Switches have replaced hubs in almost all LANs because they are faster and more efficient.
- Bridges still play an important role in segmenting networks and reducing congestion.
- Switches are often called multiport bridges.

## Summary
- **Hub:** Layer 1 device; broadcasts data to all ports; all devices share one collision domain.
- **Bridge:** Layer 2 device; connects network segments and forwards by MAC address.
- **Switch:** Layer 2 device; multiport bridge; forwards efficiently to correct port and reduces collisions.

> Tip: A hub is like a loudspeaker that sends everything everywhere, a bridge is a smart filter, and a switch is a fast multi-bridge that handles many devices efficiently.
