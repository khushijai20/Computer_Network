# Types of Cable

## Overview
This note summarizes the main cable and physical media types used in computer networking.

## 1. Twisted Pair (UTP/STP)
- Most common for Ethernet LANs.
- Categories: `Cat5e`, `Cat6`, `Cat6a`, `Cat7`.
- Typical use: `10Base-T`, `100Base-TX`, `1000Base-T`, and newer Ethernet.
- Pros:
  - Inexpensive
  - Flexible
  - Easy to install
- Cons:
  - Limited distance (typically 100 meters for Ethernet)
  - Susceptible to electromagnetic interference (EMI) unless shielded

## 2. Coaxial Cable
- Used historically for early Ethernet: `10BASE2`, `10BASE5`.
- Also used for cable television and broadband internet distribution.
- Pros:
  - Better shielding than unshielded twisted pair
  - Can carry signals over longer distances than UTP without repeaters
- Cons:
  - Bulkier and less flexible
  - Harder to install
  - Mostly legacy for modern LANs

## 3. Fiber Optic Cable
- Used for high-bandwidth, long-distance links.
- Types:
  - Single-mode fiber
  - Multi-mode fiber
- Pros:
  - Very high bandwidth
  - Very long reach
  - Immune to EMI
- Cons:
  - More expensive than copper cables
  - Requires careful handling and specialized connectors

## Connectors and Interfaces
- RJ-45: Ethernet over twisted pair
- RJ-11: Telephone lines
- BNC: Coaxial cable
- SC/LC/ST: Fiber optic cable

## Quick Comparison
- **Best for LANs:** Twisted pair (Cat5e/Cat6)
- **Legacy/older systems:** Coaxial cable
- **Best for backbones and long-distance:** Fiber optic cable

## Notes
- The physical layer of networking is responsible for transmitting raw bit streams over these media.
- Twisted pair is the dominant cable type for modern local networks.
- Fiber optic is preferred where speed and distance are critical.
