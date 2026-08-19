# 🌐 Day 12: Collision Domain vs Broadcast Domain

## Overview

A network domain is a group of devices affected by a particular type of traffic. Two important domains are the **collision domain** and the **broadcast domain**.

- A **collision domain** is the part of a network in which devices can experience frame collisions.
- A **broadcast domain** is the part of a network in which a Layer 2 broadcast frame is delivered to all devices.

Understanding these domains helps explain why hubs are inefficient, why switches improve LAN performance, and why routers and VLANs are used to control broadcast traffic.

## 📝 Topics Covered

- Collision domains and Ethernet collisions
- Broadcast domains and broadcast frames
- Effect of hubs, bridges, switches, routers, and VLANs
- Half-duplex and full-duplex communication
- Collision domain versus broadcast domain
- Network examples and domain-counting rules
- Practical Windows commands

## 🎯 Learning Objectives

By the end of this session, you should be able to:

- Define a collision domain and a broadcast domain
- Explain how collisions occur and how switches reduce them
- Calculate the number of collision and broadcast domains in simple topologies
- Explain the difference between a hub, switch, router, and VLAN boundary
- Distinguish a collision domain from a broadcast domain in an exam or interview
- Use basic commands to inspect local IP, MAC, and interface information

## 🧠 Summary/Cheat Sheet

| Feature | Collision Domain | Broadcast Domain |
|---|---|---|
| Meaning | Area where frame collisions can occur | Area that receives a Layer 2 broadcast |
| Data involved | Usually unicast Ethernet frames sent at the same time | Broadcast frames sent to `FF:FF:FF:FF:FF:FF` |
| Main problem | Retransmissions, delay, and wasted bandwidth | Excess traffic and unnecessary processing |
| Separated by | Switch ports, bridges, and routers | Routers, Layer 3 switches, and VLAN boundaries |
| Hub behavior | One shared collision domain | One broadcast domain |
| Switch behavior | Usually one per port/link | One per VLAN |
| Router behavior | Each interface is a separate segment | Does not forward Layer 2 broadcasts by default |

## 1. Collision Domain

### Definition

A collision domain is a portion of an Ethernet network where two or more devices share the same transmission medium and their frames may interfere with one another.

A collision occurs when devices transmit at the same time on a shared medium. The damaged frames must be retransmitted, which reduces performance.

### Example

If four computers are connected to a hub and two computers transmit simultaneously, their electrical signals can collide. All four computers are part of the same collision domain.

```text
PC-A ─┐
PC-B ─┼── Hub
PC-C ─┤
PC-D ─┘

All PCs = one shared collision domain
```

### Important characteristics

- Collisions are associated mainly with shared and half-duplex Ethernet.
- A hub does not separate collision domains.
- A bridge or switch separates collision domains because each port is a separate Layer 2 segment.
- In full-duplex switched Ethernet, a device can transmit and receive simultaneously, so normal Ethernet collisions do not occur.
- Even when collisions do not practically occur, each separate switch port/link is commonly counted as a separate collision domain.

### CSMA/CD

Traditional half-duplex Ethernet uses **CSMA/CD**:

1. **Carrier Sense:** A device listens before transmitting.
2. **Multiple Access:** Many devices share the same medium.
3. **Collision Detection:** A transmitting device checks whether a collision occurred.
4. The device stops, sends a jam signal, waits for a random backoff period, and retransmits.

Modern switched full-duplex Ethernet generally does not use CSMA/CD because the sender and receiver have a dedicated link.

## 2. Broadcast Domain

### Definition

A broadcast domain is the logical portion of a network in which a Layer 2 broadcast frame reaches all devices.

An Ethernet broadcast uses the destination MAC address:

```text
FF:FF:FF:FF:FF:FF
```

Every device in the broadcast domain receives the frame and checks whether it should process it.

### Common examples of broadcasts

- **ARP Request:** A host asks, “Who has this IPv4 address?”
- **DHCP Discover:** A client searches for a DHCP server before it has an IP address.
- Some service-discovery and network-management protocols

### Example

```text
PC-A ─┐
PC-B ─┼── Switch ── PC-C
PC-D ─┘

Without VLANs, all four PCs are in one broadcast domain.
A broadcast from PC-A is delivered to the other ports in that VLAN.
```

### Important characteristics

- A switch forwards broadcasts to all other ports in the same VLAN.
- A router does not forward Layer 2 broadcasts from one interface to another by default.
- Each VLAN is a separate broadcast domain.
- A large broadcast domain can create unnecessary traffic and consume CPU and bandwidth on many devices.
- Broadcast control is one reason networks are divided into subnets and VLANs.

## 3. Effect of Network Devices

| Device | Collision Domains | Broadcast Domains | Reason |
|---|---|---|---|
| Hub | One for all connected ports | One | Repeats every signal everywhere |
| Bridge | One per segment/interface | Usually one | Filters frames but forwards broadcasts |
| Layer 2 switch | Usually one per port/link | One per VLAN | Forwards frames by MAC address and floods broadcasts within a VLAN |
| Router | One per interface/segment | One per routed interface | Does not forward Layer 2 broadcasts by default |
| Layer 3 switch | One per switch port/link | One per VLAN or routed interface | Performs switching and routing |
| Wireless access point | Shared wireless collision domain | Usually one per mapped VLAN/SSID | Wireless clients share the radio medium |

## 4. Collision Domain Counting Rules

Use these rules for common diagrams:

- **One hub:** one collision domain, regardless of the number of devices connected to it.
- **One unmanaged or Layer 2 switch:** normally one collision domain per active switch port/link.
- **Two switches connected together:** the inter-switch link is another collision domain.
- **A bridge:** each connected segment is a separate collision domain.
- **A router:** each active interface connects to a separate collision domain.
- In a full-duplex switched network, count the links as separate collision domains for topology questions, but remember that actual collisions are not expected.

### Example 1: Hub

```text
PC-A ─┐
PC-B ─┼── Hub ── PC-C
PC-D ─┘
```

- Collision domains: **1**
- Broadcast domains: **1**

### Example 2: Switch

```text
PC-A ─┐
PC-B ─┼── Switch ── PC-C
PC-D ─┘
```

- Collision domains: **4**, one for each PC-to-switch link
- Broadcast domains: **1**, assuming one VLAN

### Example 3: Two VLANs

```text
VLAN 10: PC-A ─┐
               ├── Switch
VLAN 20: PC-B ─┘
```

- Collision domains: one per active switch link
- Broadcast domains: **2**, because VLAN 10 and VLAN 20 are separate
- Communication between VLANs requires a router or Layer 3 switch

### Example 4: Two networks connected by a router

```text
LAN 1 ── Router ── LAN 2
```

- Collision domains: at least **2**, one on each router interface/segment
- Broadcast domains: **2**, because the router separates the LANs
- A broadcast from LAN 1 is not forwarded to LAN 2 by default

## 5. Collision Domain vs Broadcast Domain

### Main differences

| Question | Collision Domain | Broadcast Domain |
|---|---|---|
| What is shared? | A transmission medium | Broadcast reachability |
| What causes the problem? | Simultaneous transmission | A broadcast sent to many hosts |
| Relevant address | Ethernet frame transmission | Broadcast MAC address or IPv4 broadcast address |
| Layer | Mainly Layer 1 and Layer 2 behavior | Layer 2 behavior, controlled at Layer 3 boundaries |
| Switch effect | Divides collision domains by port | Keeps devices together unless VLANs are used |
| Router effect | Separates connected segments | Separates broadcast domains |
| Typical solution | Switches and full-duplex links | Routers, VLANs, and subnet design |

### Memory trick

- **Collision:** devices interfere with each other while sending.
- **Broadcast:** one device sends and everyone in the domain hears it.
- **Switch breaks collisions.**
- **Router breaks broadcasts.**

## 6. VLANs and Broadcast Domains

A **VLAN (Virtual Local Area Network)** logically divides one physical switch into multiple Layer 2 networks.

```text
                 Switch
          ┌─────────────────┐
VLAN 10:  │ PC-A       PC-B │  Broadcast domain 1
VLAN 20:  │ PC-C       PC-D │  Broadcast domain 2
          └─────────────────┘
```

- Devices in the same VLAN share a broadcast domain.
- Devices in different VLANs are in different broadcast domains.
- A VLAN does not necessarily reduce the number of physical switch ports or collision links.
- Inter-VLAN communication requires routing.
- VLANs improve organization, security, and broadcast control.

## 7. Unicast, Broadcast, and Collision Behavior

### Unicast

A frame sent from one source to one destination. A switch normally forwards it only through the destination port after learning the destination MAC address.

### Broadcast

A frame sent to every device in the same broadcast domain. A switch floods it within the same VLAN, but a router normally stops it at the Layer 3 boundary.

### Unknown unicast

A frame whose destination MAC address is not yet in the switch MAC table. The switch floods it within the same VLAN, similar to a broadcast from the perspective of forwarding, although its destination address is not the broadcast address.

### Collision

A physical transmission problem caused by simultaneous transmissions on a shared or half-duplex medium. A collision is not the same thing as a broadcast.

## 8. Practical Commands

1. `ipconfig /all` — displays IPv4 address, subnet mask, default gateway, DNS servers, and adapter details.
2. `arp -a` — displays the local ARP cache, including learned IP-to-MAC mappings.
3. `ping 192.168.1.1` — tests reachability to a local gateway using ICMP.
4. `tracert 8.8.8.8` — shows Layer 3 hops toward a destination and helps identify routed boundaries.
5. `getmac /v` — displays MAC addresses and adapter connection information.
6. `netstat -e` — displays Ethernet statistics, including received and sent errors on supported Windows versions.

### What commands can and cannot show

- `ipconfig /all` can show the local subnet and gateway, but not the complete broadcast domain.
- `arp -a` shows nearby Layer 2 neighbors learned through ARP, not every device in the broadcast domain.
- `tracert` reveals routed hops, which can help identify where broadcast domains are separated.
- Managed-switch commands such as `show vlan brief`, `show mac address-table`, and `show interfaces` are needed for detailed VLAN, MAC, and interface analysis on Cisco devices.

## 📚 Important Definitions

- **Collision domain:** A network segment where simultaneous transmissions may collide.
- **Broadcast domain:** A group of devices that receive the same Layer 2 broadcast.
- **Collision:** Interference caused by two devices transmitting on a shared medium at the same time.
- **Broadcast frame:** An Ethernet frame sent to `FF:FF:FF:FF:FF:FF`.
- **VLAN:** A logical Layer 2 network created on a switch.
- **Half-duplex:** Communication in which devices can transmit in either direction, but not simultaneously.
- **Full-duplex:** Communication in which devices can transmit and receive simultaneously.
- **CSMA/CD:** The traditional Ethernet method for detecting and recovering from collisions.
- **Broadcast boundary:** A Layer 3 boundary, usually a router or Layer 3 switch interface, that stops Layer 2 broadcasts.

## 📝 Example Questions and Answers

### Q1. What is a collision domain?

A collision domain is a part of a network where devices share a medium and their transmissions may collide.

### Q2. What is a broadcast domain?

A broadcast domain is a group of devices that receive a Layer 2 broadcast frame.

### Q3. How many collision domains does a four-port hub create?

One collision domain. All devices connected to the hub share the same medium.

### Q4. How many collision domains does a four-port switch create when all ports are active?

Normally four collision domains, one per active port/link.

### Q5. How many broadcast domains does a switch create?

One broadcast domain per VLAN. A switch with no VLAN separation has one broadcast domain.

### Q6. Which device separates broadcast domains?

A router or a Layer 3 switch separates broadcast domains. VLANs also create separate broadcast domains on a switched network.

### Q7. Does a switch stop broadcasts?

No. A Layer 2 switch floods broadcasts to all ports in the same VLAN, except the receiving port.

### Q8. Does a router forward broadcasts?

A router does not forward Layer 2 broadcasts by default. Special features such as DHCP relay can intentionally help a service cross a routed boundary.

### Q9. Can a collision domain exist without practical collisions?

Yes. A full-duplex switch link is commonly counted as a separate collision domain, although simultaneous transmission does not cause normal Ethernet collisions on that dedicated link.

### Q10. How can broadcast traffic be reduced?

Use routers or Layer 3 switches, divide the network into VLANs and subnets, and avoid unnecessarily large Layer 2 networks.

## ✅ Quick Revision Notes

- A **hub** creates one shared collision domain.
- A **switch** creates a separate collision domain per port/link.
- A Layer 2 switch normally keeps one broadcast domain per VLAN.
- A **router** separates broadcast domains.
- A VLAN is a separate broadcast domain.
- Full-duplex Ethernet prevents normal collisions, but switched links are still counted separately in topology questions.
- Broadcasts stay within a VLAN unless a routing feature is deliberately configured.
- **Switch breaks collision domains; router breaks broadcast domains.**

## 📌 Final Summary

Collision domains describe where Ethernet frames can interfere with each other. Broadcast domains describe where broadcast frames can travel. Hubs place all connected devices in one shared collision domain, while switches create separate collision domains for their ports. A switch still forwards broadcasts within the same VLAN, so routers, Layer 3 switches, or VLAN boundaries are required to divide broadcast domains.

## 🔜 Tomorrow's Preview

Day 13 will cover VLANs, access ports, trunk ports, and inter-VLAN routing in greater detail.
