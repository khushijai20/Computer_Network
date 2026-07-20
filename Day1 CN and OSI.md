# 🌐 Day 1: Introduction to Computer Networks & the OSI Model

Welcome to Day 1 of the Computer Networks & Security study journey. This guide covers the fundamentals of networking, the OSI reference model, and practical diagnostic commands.

## 📝 Topics Covered

- Core networking concepts and physical topologies: Mesh, Star, Bus, Ring
- The 7-layer OSI model and its exact responsibilities
- Protocol Data Units (PDUs) at each layer
- Encapsulation and decapsulation flow
- Real-world example: tracing a WhatsApp message through the OSI layers
- Practical terminal commands: `ping`, `ipconfig`, `arp`

## 🎯 Learning Objectives

By the end of this session, you should be able to:

- Define a computer network and its basic components
- Calculate physical links for a full mesh network using `L = N(N-1)/2`
- Explain why layered architecture is necessary for networking
- Describe the functions and services of each OSI layer
- Identify the PDU used by each OSI layer
- Trace the communication flow of a real-world message across the OSI stack
- Run and interpret `ping`, `ipconfig`, and `arp` output

## 🧠 OSI Model Cheat Sheet

| Layer | Name | PDU | Key Protocols | Hardware |
|---|---|---|---|---|
| 7 | Application | Data | HTTP, DNS, FTP, SMTP | Host/PC |
| 6 | Presentation | Data | SSL/TLS, JPEG, ASCII | Host/PC |
| 5 | Session | Data | NetBIOS, RPC, PPTP | Host/PC |
| 4 | Transport | Segment / Datagram | TCP, UDP | Gateway / Host |
| 3 | Network | Packet | IPv4, IPv6, ICMP, ARP | Router / L3 Switch |
| 2 | Data Link | Frame | Ethernet, PPP | Switch / Bridge |
| 1 | Physical | Bits | 10BaseT, DSL, Wi-Fi | Hub / Cables |

## 🔍 Layer Summary

- **Physical (L1):** raw bit transmission, cables, connectors, hubs, signal levels
- **Data Link (L2):** framing, MAC addressing, CRC error checking, switches
- **Network (L3):** logical addressing, routing, IP, packet forwarding
- **Transport (L4):** ports, segmentation, reliability, TCP/UDP
- **Session (L5):** session establishment, control, synchronization
- **Presentation (L6):** data translation, encryption, compression
- **Application (L7):** user-facing services and application protocols

## 📌 Key Concepts

- Mesh topology uses `L = N(N-1)/2` physical links
- Encapsulation adds headers (and trailers) as data moves down the stack
- Decapsulation removes them as data moves up the stack
- A WhatsApp message travels from the app through encryption, session control, transport, routing, framing, and physical transmission

## 🛠️ Practical Commands

1. `ping google.com` — checks reachability using ICMP and shows round-trip time
2. `ipconfig` — displays local network adapter configuration on Windows
3. `arp -a` — shows Layer 3 to Layer 2 address mapping in the ARP cache

## ✅ Quick Revision Notes

- Mnemonic: **Please Do Not Throw Sausage Pizza Away**
- L1: Bits, cables, hubs
- L2: Frames, MAC, switches
- L3: Packets, IP, routers
- L4: Segments/datagrams, ports, TCP/UDP
- L5: Session control
- L6: Translation, encryption, compression
- L7: Application services

## 📚 Important Definitions

- **PDU:** Protocol Data Unit, the unit of data used at a specific layer
- **Encapsulation:** wrapping higher-layer data with lower-layer headers/trailers
- **IP Address:** logical address at Layer 3
- **MAC Address:** physical hardware address at Layer 2
- **Routing:** Layer 3 decision-making process for forwarding packets
- **Port Number:** Layer 4 identifier for a process or service
- **CRC:** error-detection field in Layer 2 frames

## 📝 Example Questions

- What layer forwards packets across networks? **Network Layer**
- What is the L4 PDU? **Segment**
- Which device operates at Layer 2? **Switch**
- Which layer handles SSL/TLS? **Presentation**
- How many links are required for a full mesh of 8 nodes? **28**

## 📈 Practical Notes

- `ping` uses ICMP at Layer 3
- `ipconfig` shows your local IP, subnet mask, and gateway
- `arp -a` maps local IPs to MAC addresses

## 🔜 Tomorrow's Preview

Day 2 will compare the TCP/IP protocol suite with the OSI model, explain why TCP/IP is the standard architecture for the modern internet, and show how the models map to each other.
