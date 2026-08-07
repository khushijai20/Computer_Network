# Day 8: Switch and Router

## Overview
This note explains two core networking devices used in modern LAN and WAN infrastructure: switches and routers. It covers their purpose, how they operate in the OSI model, their forwarding behavior, and how they differ.

## 1. Switch

### What it is
A switch is a network device that connects multiple devices within the same local area network (LAN) and forwards frames based on MAC addresses.

### OSI layer
- Operates primarily at Layer 2 (Data Link Layer).
- Some switches also support Layer 3 features and are called multilayer switches.

### How it works
- Receives Ethernet frames on one port.
- Reads the source MAC address and learns which device is on that port.
- Builds and maintains a MAC address table (also called a CAM table).
- Checks the destination MAC address and forwards the frame only to the port where the destination device is located.
- If the destination MAC is unknown, the switch floods the frame to all ports in the same VLAN except the source port.

### Frame forwarding methods
- Store-and-forward: Switch receives the entire frame, checks it for errors, then forwards it.
- Cut-through: Switch forwards the frame as soon as the destination MAC is read, with lower latency.
- Fragment-free: Switch waits until the first 64 bytes are received before forwarding, reducing collisions and errors.

### Collision and broadcast domains
- Each switch port creates a separate collision domain.
- A switch does not separate broadcast domains; broadcasts are forwarded to all ports within the same VLAN.

### Common switch types
- Unmanaged switch: Simple plug-and-play device with no configuration.
- Managed switch: Allows configuration, monitoring, VLANs, QoS, security policies, and port management.
- Smart switch: Offers some manageability and features between unmanaged and fully managed switches.
- Multilayer switch: Can perform both Layer 2 switching and Layer 3 routing.

### Use cases
- Connecting computers, printers, servers, and access points within a LAN.
- Segmenting a network into separate collision domains.
- Enabling VLANs for traffic segmentation.
- Providing port-based access and local switching in offices, campuses, or data centers.

### Pros
- Efficiently forwards only required frames, reducing unnecessary traffic.
- Improves network performance by isolating collision domains.
- Supports VLANs, QoS, and security features on managed models.
- Scales better than hubs and bridges.

### Cons
- Does not route between different IP networks without Layer 3 capabilities.
- Broadcast traffic is still shared across the VLAN.
- Can become a single point of failure if not designed with redundancy.

### Example
- A 24-port Ethernet switch connects 24 workstations in an office. When one workstation sends a frame to another, the switch forwards it only to the correct port instead of all 24 ports.

## 2. Router

### What it is
A router is a network device that connects different networks and forwards packets based on IP addresses.

### OSI layer
- Operates at Layer 3 (Network Layer).
- Uses network-layer addresses such as IPv4 or IPv6.

### How it works
- Receives packets on one interface.
- Examines the destination IP address.
- Uses a routing table to determine the best next-hop interface.
- Forwards the packet toward its destination network.
- Updates TTL/Hop Limit and may perform packet fragmentation when needed.

### Routing table and decision process
- The routing table contains network prefixes, next hops, and interface assignments.
- The router chooses the longest-prefix match to select the best route.
- If no matching route exists, the packet is dropped unless a default route is configured.

### Routing protocols
- Static routing: Administrator configures routes manually.
- Dynamic routing: Routers learn routes automatically using protocols.
  - Distance-vector protocols: RIP, IGRP.
  - Link-state protocols: OSPF, IS-IS.
  - Path-vector protocols: BGP.

### Additional router features
- Network Address Translation (NAT): Maps private IP addresses to public IPs.
- DHCP relay: Forwards DHCP requests between clients and servers.
- Firewall/filtering: Applies access control lists (ACLs) and security policies.
- WAN connectivity: Connects to the internet, VPNs, MPLS, and leased lines.

### Use cases
- Connecting a home or office LAN to the internet.
- Linking multiple LANs in different buildings or geographic sites.
- Isolating broadcast domains between networks.
- Enabling inter-VLAN routing when switching is configured with VLANs.

### Pros
- Routes traffic between different IP networks.
- Enforces policies, security, and traffic control at the network edge.
- Enables communication across WANs and the internet.
- Supports advanced services such as VPN, NAT, and QoS.

### Cons
- Slower than switching for local traffic due to packet inspection and routing decisions.
- More complex to configure and manage than a simple switch.
- Can be more expensive, especially for enterprise-grade routers.

### Example
- A router connects a corporate LAN to the internet and routes traffic between the internal network (192.168.10.0/24) and the service provider network.

## 3. Switch vs Router

| Feature | Switch | Router |
|---|---|---|
| Primary OSI layer | Layer 2 | Layer 3 |
| Forwarding decision | MAC address | IP address |
| Broadcast domain | Same VLAN | Different networks separated |
| Collision domain | One per port | One per interface |
| Typical function | Connect devices within a LAN | Connect different networks and route traffic |
| Uses | LAN switching, VLANs, local traffic | Internet access, inter-network routing, WAN links |
| Security | Port security, VLANs, ACLs on managed switches | ACLs, NAT, firewall, VPN |
| Example device | Campus switch, access switch | Edge router, home router |

## 4. Key concepts

### MAC address table
- A switch stores MAC addresses and associated ports to forward frames efficiently.
- Entries age out after a timeout if no traffic is seen from a device.

### Routing table
- A router stores routes to destination networks and uses them to forward packets.
- Entries can be static or learned from routing protocols.

### Default gateway
- On hosts, the default gateway is the router IP used for traffic destined outside the local subnet.
- Switches do not act as default gateways unless they have Layer 3 routing enabled.

### VLAN (Virtual LAN)
- A VLAN segments a switch into multiple logical LANs.
- VLANs separate broadcast domains on the same physical switch.
- Routers or Layer 3 switches are required for communication between VLANs.

### Multilayer switching
- A multilayer switch can switch traffic at Layer 2 and route traffic at Layer 3.
- This blends the speed of switching with the network segmentation of routing.

## 5. Notes
- In modern enterprise networks, access switches connect end devices and routers connect those switched networks to other networks.
- Routers are essential for inter-network communication and internet access, while switches are essential for efficient local traffic delivery.
- A home network device often combines a switch, router, wireless access point, and firewall into one small appliance.

## Summary
- **Switch:** Layer 2 device that forwards frames by MAC address, creates separate collision domains, and is used to build LANs.
- **Router:** Layer 3 device that forwards packets by IP address, separates broadcast domains, and connects different networks.
- **Main difference:** switches operate within a LAN, routers operate between LANs and WANs.

> Tip: Think of a switch as the local traffic manager inside a building, and a router as the network gateway that directs traffic between buildings and to the outside world.