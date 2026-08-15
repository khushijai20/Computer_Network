# Day 9: Types of Cable, Repeater and Hub

## Overview
This note covers the main types of transmission media used in networking and explains two important physical-layer devices: repeaters and hubs. These devices help in signal transmission and network connectivity, but they work differently from switches and routers.

## 1. Types of Cable

### 1. Twisted Pair Cable
Twisted pair cable is the most widely used cable in local area networks (LANs). It consists of pairs of copper wires twisted together to reduce electromagnetic interference.

#### Types
- UTP (Unshielded Twisted Pair)
- STP (Shielded Twisted Pair)

#### Categories
- Cat5e
- Cat6
- Cat6a
- Cat7

#### Characteristics
- Cheap and easy to install
- Flexible and commonly used in offices and homes
- Suitable for Ethernet networks
- Supports data rates from 10 Mbps to 10 Gbps and beyond depending on category

#### Use cases
- Computer networking
- Telephone lines
- Ethernet LAN connections

#### Pros
- Low cost
- Easy to install and maintain
- Widely available

#### Cons
- Limited distance (usually around 100 meters for Ethernet)
- Can be affected by EMI if not properly shielded

#### Connector
- RJ-45

---

### 2. Coaxial Cable
Coaxial cable has a central copper conductor, an insulating layer, a metallic shield, and an outer cover. It provides better shielding than twisted pair cable.

#### Characteristics
- Better resistance to interference
- Stronger signal carrying capability
- Used in older Ethernet and cable TV networks

#### Use cases
- Cable TV
- Broadband internet
- Legacy Ethernet networks like 10BASE2 and 10BASE5

#### Pros
- Better shielding than UTP
- Good for longer distances than twisted pair in some cases
- Durable and stable

#### Cons
- Bulkier and less flexible
- More difficult to install than twisted pair
- Mostly replaced by twisted pair and fiber optic cable in modern networks

#### Connector
- BNC connector

---

### 3. Fiber Optic Cable
Fiber optic cable uses light signals instead of electrical signals to transmit data. It is made of glass or plastic fibers and is used for high-speed and long-distance communication.

#### Types
- Single-mode fiber
- Multi-mode fiber

#### Characteristics
- Very high bandwidth
- Long-distance transmission
- Immune to electromagnetic interference

#### Use cases
- Internet backbones
- Data centers
- Long-distance communication systems
- High-speed enterprise networks

#### Pros
- Extremely high speed
- Very long transmission distance
- Not affected by EMI or crosstalk

#### Cons
- Expensive
- Requires special installation and connectors
- More delicate than copper cable

#### Connectors
- SC
- LC
- ST

---

## 2. Comparison of Cable Types

| Cable Type | Medium | Speed | Distance | Cost | Common Use |
|---|---|---|---|---|---|
| Twisted Pair | Copper | Medium | Short to medium | Low | LANs, Ethernet |
| Coaxial | Copper | Medium | Medium | Medium | Cable TV, legacy Ethernet |
| Fiber Optic | Light | Very high | Very long | High | Backbone, long-distance, data centers |

## 3. Repeater

### What it is
A repeater is a physical-layer device that receives a weak signal, amplifies or regenerates it, and sends it again to extend network distance.

### OSI layer
- Operates at Layer 1 (Physical Layer)

### How it works
- Receives an incoming signal from one segment
- Regenerates the signal to restore strength and quality
- Re-transmits it to the next segment

### Use cases
- Extending the length of a cable segment
- Connecting two network segments that are separated by distance

### Pros
- Simple and inexpensive
- Increases transmission range
- Useful in large networks where signal weakens

### Cons
- Does not filter traffic or reduce collisions
- Cannot improve network performance significantly
- If the signal is noisy, the repeater may amplify the noise too

### Example
- A repeater can help extend an Ethernet cable beyond the normal range by restoring signal strength.

---

## 4. Hub

### What it is
A hub is a simple networking device that connects multiple devices in the same network segment. It broadcasts incoming data to all connected devices.

### OSI layer
- Operates at Layer 1 (Physical Layer)

### How it works
- Receives data from one port
- Sends the same signal to all other ports
- All connected devices share the same bandwidth and collision domain

### Types of hub
- Passive hub
- Active hub
- Intelligent hub

### Use cases
- Small local networks
- Legacy Ethernet setups
- Simple device interconnection when low cost matters

### Pros
- Very cheap
- Easy to install and use
- Useful in basic network setups

### Cons
- All devices share one collision domain
- High collision and traffic congestion
- Inefficient because data is sent to every port
- Mostly replaced by switches in modern networks

### Example
- In a 4-port hub, if one computer sends data, all connected computers receive it, even if only one of them is the intended target.

---

## 5. Repeater vs Hub

| Device | Layer | Function | Traffic Handling | Collision Domain |
|---|---|---|---|---|
| Repeater | Layer 1 | Regenerates and boosts signal | No filtering | Same collision domain |
| Hub | Layer 1 | Connects multiple ports and broadcasts data | Sends to all ports | Same collision domain |

## 6. Key Notes
- Cables are the physical media used to carry data in a network.
- Twisted pair is the most common cable for LANs.
- Fiber optic cable is preferred for long distances and high-speed networks.
- A repeater strengthens the signal.
- A hub connects multiple devices but sends data to all connected ports.
- Both repeater and hub operate at the physical layer and do not make intelligent forwarding decisions.

## Summary
- **Cable types:** twisted pair, coaxial, and fiber optic
- **Repeater:** regenerates signal and extends network distance
- **Hub:** connects multiple devices and broadcasts data to all ports
- **Main idea:** these devices work at the physical layer and are simpler than switches and routers

> Tip: Think of a repeater as a signal booster and a hub as a shared broadcast connector. Both are physical-layer devices and do not perform smart routing or switching.
