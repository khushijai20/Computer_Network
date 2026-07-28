# Day 7: Repeater, Hub, Bridge

## Overview
This note explains the basic network devices used at the physical and data link layers: repeaters, hubs, and bridges. These devices help extend or segment a network, but each works differently.

## 1. Repeater

### What it is
A repeater is a physical-layer device that receives a signal, regenerates it, and retransmits it to extend the distance of a network segment.

### How it works
- Repeats and amplifies the incoming electrical signal.
- Restores the signal strength and timing, removing noise and attenuation.
- Does not change the frame or packet contents.

### OSI layer
- Operates at Layer 1 (Physical Layer).

### Use cases
- Extending Ethernet cable length beyond the standard maximum distance.
- Connecting two cable segments to keep signal quality over longer distances.

### Pros
- Simple and inexpensive.
- Extends cable reach without requiring protocol changes.

### Cons
- Does not reduce collisions or traffic.
- Can amplify noise if the signal is already corrupted.
- Every repeater adds latency.

### Example
- A repeater can connect two 100-meter Ethernet segments to form a 200-meter run, but the network remains one collision domain.

## 2. Hub

### What it is
A hub is also a Layer 1 device that connects multiple Ethernet devices in a network segment. It forwards incoming bits to all other ports.

### How it works
- Receives a signal on one port and broadcasts the same signal out all other ports.
- All connected devices share the same physical segment.
- Hubs do not inspect MAC addresses or make forwarding decisions.

### OSI layer
- Operates at Layer 1 (Physical Layer).

### Types of hubs
- Passive hub: simply connects cables without amplification.
- Active hub: regenerates and amplifies signals, like a repeater with multiple ports.
- Intelligent hub: may include basic management features but still acts as a Layer 1 device.

### Use cases
- Small legacy networks where inexpensive multiport connectivity is needed.
- When simple signal distribution is sufficient and traffic levels are low.

### Pros
- Very simple and cheap.
- Easy to install.

### Cons
- All ports share a single collision domain.
- Collisions increase as more devices are connected.
- Inefficient bandwidth usage because every frame is sent to every port.
- Largely replaced by switches in modern networks.

### Example
- A 4-port Ethernet hub connects four computers, but if one computer sends data, all four ports receive it. Only the intended recipient accepts the frame.

## 3. Bridge

### What it is
A bridge is a network device that connects two or more network segments and forwards frames based on MAC addresses.

### How it works
- Operates at Layer 2 (Data Link Layer).
- Learns MAC addresses by inspecting source addresses of incoming frames.
- Builds a MAC address table mapping devices to ports.
- Forwards frames only to the segment where the destination device is located.
- Filters traffic by blocking frames destined for devices on the same segment as the source.

### Types of bridges
- Transparent bridge: learns MAC addresses automatically and forwards frames without needing configuration.
- Source-route bridge: used in Token Ring networks and forwards based on path information embedded in frames.
- Learning bridge: builds its MAC table dynamically as traffic passes through.

### Use cases
- Segmenting a network to reduce collisions.
- Connecting two Ethernet segments without routing.
- Improving performance by limiting broadcast and collision domains.

### Pros
- Reduces unnecessary traffic by forwarding only needed frames.
- Creates separate collision domains for each connected segment.
- Helps isolate network problems.

### Cons
- Can add latency while learning and forwarding frames.
- Less flexible than switches for larger networks.
- Still passes broadcast traffic across segments.

### Example
- A bridge connects two Ethernet segments. If a frame from device A on segment 1 is destined for device B on segment 2, the bridge forwards it only to segment 2.

## 4. Comparison: Repeater vs Hub vs Bridge

| Device | OSI Layer | Forwarding | Collision Domain | Traffic Handling | Typical use |
|---|---|---|---|---|---|
| Repeater | Layer 1 | Broadcast to all ports after regeneration | Single collision domain | Simple signal repeat | Extend cable distance |
| Hub | Layer 1 | Broadcast to all other ports | Single collision domain | Sends every frame to every port | Connect multiple devices cheaply |
| Bridge | Layer 2 | Forward based on MAC address | Multiple collision domains | Sends only to destination segment | Segment networks and reduce collisions |

## 5. Notes
- In modern Ethernet networks, switches have replaced hubs and bridges in most cases. A switch is like a multiport bridge with better performance.
- Repeaters and hubs are still conceptually useful for understanding how physical-layer devices differ from data-link devices.
- A bridge can connect different physical media types if they use compatible frame formats, while repeaters and hubs cannot.

## Summary
- **Repeater:** regenerates physical signals, extends distance, Layer 1.
- **Hub:** connects multiple devices in one shared segment, Layer 1, broadcasts to all ports.
- **Bridge:** connects segments at Layer 2, forwards by MAC address, reduces collisions.

> Tip: Think of a bridge as a smart connector that knows where devices are, while a hub is a dumb connector that just copies signals everywhere.