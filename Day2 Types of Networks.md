# 🌐 Day 2: Types of Networks

Welcome to Day 2 of the Computer Networks study guide. This note explains the main categories of network types, their characteristics, and where each is used.

## 📝 Topics Covered

- Local Area Network (LAN)
- Metropolitan Area Network (MAN)
- Wide Area Network (WAN)
- Personal Area Network (PAN)
- Campus Area Network (CAN)
- Differences between network types
- Use cases and examples

## 🎯 Learning Objectives

By the end of this session, you should be able to:

- Define the main network types used in computer networking
- Compare size, scope, and ownership for LAN, MAN, WAN, PAN, and CAN
- Identify common devices and media used in each network type
- Explain real-world examples for each network category
- Understand when one network type is more suitable than another

## 🧠 Network Type Cheat Sheet

| Network Type | Scope | Typical Area | Example | Ownership |
|---|---|---|---|---|
| PAN | Very small | Personal workspace | Bluetooth headset, smartphone tethering | Individual |
| LAN | Small | Home, office, school | Ethernet network, Wi-Fi in a building | Private organization |
| CAN | Medium | Campus, university, corporate park | University network, business campus | Single organization |
| MAN | Large | City or metro area | City-wide fiber network, ISP backbone | Public / private |
| WAN | Very large | Country or global | Internet, national telecom networks | Multiple organizations |

## 🔍 Network Type Summary

- **PAN (Personal Area Network):** connects personal devices within a few meters, often using Bluetooth or USB.
- **LAN (Local Area Network):** covers a small physical area like an office or home, usually using Ethernet or Wi-Fi.
- **CAN (Campus Area Network):** links multiple buildings in a campus or business park under one administrative domain.
- **MAN (Metropolitan Area Network):** spans a city or metropolitan region, often built with fiber and used by ISPs or governments.
- **WAN (Wide Area Network):** connects networks across long distances, such as countries and continents, using leased lines, satellites, or MPLS.

## 📌 Key Concepts

- **Scale:** PAN is smallest, WAN is largest.
- **Ownership:** LAN and CAN are typically owned by a single organization; MAN and WAN may cross ownership boundaries.
- **Media:** LAN uses Ethernet/Wi-Fi; MAN/WAN use fiber, microwave, or leased circuits.
- **Latency:** Generally increases with the geographic size of the network.

## 🛠️ Practical Notes

1. `ping` is useful on a LAN and can also test connectivity over a WAN, but latency will be higher on larger networks.
2. `ipconfig` shows your local LAN or PAN settings on Windows.
3. `tracert` can help identify whether traffic leaves a LAN and travels across a WAN.

## ✅ Quick Revision Notes

- **PAN:** personal, very small, Bluetooth/USB
- **LAN:** local, home/office, Ethernet/Wi-Fi
- **CAN:** campus, multiple buildings, single organization
- **MAN:** city-wide, fiber/microwave, ISP-level
- **WAN:** global, leased lines/satellite, multiple owners

## 📚 Important Definitions

- **Network Type:** classification based on size, physical scope, and purpose.
- **Topology:** physical or logical layout of how devices connect.
- **Bandwidth:** data transfer capacity available on the network.
- **Latency:** time taken for data to travel from source to destination.
- **Backbone:** the central part of a network that carries major traffic loads.

## 📝 Example Questions

- What network type covers a university campus? **CAN**
- Which network type is typically used for Bluetooth headphones? **PAN**
- Which network type can span a country or continent? **WAN**
- Which network type is commonly built with Ethernet switches and access points? **LAN**
- Which network type often uses fiber optics and municipal infrastructure? **MAN**

## 🔜 Tomorrow's Preview

Day 3 will introduce the TCP/IP model, compare it with the OSI model, and explain how the internet uses TCP/IP protocols for reliable communication.