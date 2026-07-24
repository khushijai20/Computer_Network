# Day 3 — TCP/IP and Physical Layer

## Overview

This document covers the TCP/IP model and the Physical Layer of computer networks, including their functions, components, protocols, and examples.

---

## TCP/IP Model

### Layers and Mapping

- **Application Layer**: High-level protocols and user-facing services (HTTP, FTP, SMTP, DNS, SSH). Handles data representation, encoding, and session management.
- **Transport Layer**: Provides end-to-end communication, flow control, error detection/recovery, and multiplexing. Key protocols: TCP and UDP.
- **Internet Layer**: Routing of packets across networks. Main protocol: IP (IPv4/IPv6). Includes ICMP for control messages and ARP (address resolution — sometimes placed between Link and Internet layers).
- **Link (Network Interface) Layer**: Interacts with physical network hardware, frames packets for the medium, and handles physical addressing (MAC). Includes technologies such as Ethernet, Wi‑Fi, PPP.

Note: The TCP/IP model is often described with four layers. It maps roughly to the OSI model but with different boundaries and fewer layers.

### Application Layer

- Provides network services to user applications.
- Protocol examples: HTTP(S), FTP, SMTP, IMAP, POP3, DNS, DHCP, SSH, Telnet, SNMP.
- Responsibilities: data formatting, encryption (TLS/SSL), session management, and APIs for applications.

### Transport Layer

- **TCP (Transmission Control Protocol)**
  - Connection-oriented: establishes a connection using a three-way handshake (SYN, SYN-ACK, ACK).
  - Reliable: guarantees in-order delivery, retransmits lost segments, and uses sequence and acknowledgment numbers.
  - Flow control: uses a sliding window and the receiver's advertised window to prevent overwhelming the receiver.
  - Congestion control: algorithms like Slow Start, Congestion Avoidance, Fast Retransmit, Fast Recovery.
  - Segment structure: source port, destination port, sequence number, acknowledgment number, flags (SYN, ACK, FIN, RST), window size, checksum, urgent pointer, options (e.g., MSS).

- **UDP (User Datagram Protocol)**
  - Connectionless and unreliable: no handshake, no guaranteed delivery or order.
  - Lightweight: reduced overhead, suitable for real-time applications (VoIP, DNS queries, streaming).
  - Datagram structure: source port, destination port, length, checksum.

### Internet Layer

- **IP (Internet Protocol)**
  - IPv4: 32-bit addresses, header includes version, IHL, total length, identification, flags, TTL, protocol, header checksum, source/destination addresses, options.
  - IPv6: 128-bit addresses, simplified header, no header checksum, extension headers for optional features.
- **ICMP (Internet Control Message Protocol)**: used for diagnostic and control messages (e.g., ping, destination unreachable).
- **ARP (Address Resolution Protocol)**: resolves IPv4 addresses to MAC addresses on a local network (operates at Link/Internet boundary).

### Link Layer

- Deals with framing, physical addressing (MAC), error detection (FCS), and medium access control.
- Technologies: Ethernet (IEEE 802.3), Wi‑Fi (IEEE 802.11 family), Token Ring (legacy), PPP, Frame Relay (legacy), FDDI (legacy).
- Frame structure (Ethernet II): destination MAC, source MAC, Ethertype/length, payload (e.g., IP packet), Frame Check Sequence (FCS).

---

## Physical Layer

### Purpose and Responsibilities

- The Physical Layer is Layer 1 of the OSI model and the lowest layer in the TCP/IP link-layer stack.
- Responsible for the transmission and reception of raw bit streams over a physical medium.
- Main functions:
  - Encoding and signaling: convert digital data into electrical, optical, or radio signals.
  - Bit synchronization: ensure sender and receiver are synchronized at the bit level.
  - Physical topology: defines how devices are connected (bus, star, ring, mesh).
  - Transmission mode: simplex, half-duplex, full-duplex.
  - Medium and connectors: copper (UTP/STP coaxial), fiber optics, wireless (radio, microwave).
  - Data rate and bandwidth: defines the maximum bit rate supported by the medium and equipment.

### Encoding and Signaling Techniques

- **Digital-to-analog / Analog-to-digital**: necessary when baseband digital data must be sent over an analog medium.
- **Baseband transmission**: sends digital signals directly over the medium (e.g., Ethernet over UTP using Manchester, MLT-3, PAM5).
- **Broadband transmission**: multiple frequency channels using modulation (e.g., cable TV systems).

Common line coding methods:
- **NRZ (Non-Return-to-Zero)**: two voltage levels represent bits; simple but poor synchronization.
- **NRZI (Non-Return-to-Zero Inverted)**: signal change indicates a '1', no change indicates '0' (or vice versa); used in USB and old standards.
- **Manchester encoding**: each bit contains a transition; self-clocking and good synchronization (used in 10BASE-T Ethernet original specs).
- **4B/5B, 8B/10B**: map data to codes with enough transitions for clock recovery; used in fast Ethernet and serial links.

Modulation methods for analog carrier signals:
- **ASK (Amplitude Shift Keying)**
- **FSK (Frequency Shift Keying)**
- **PSK (Phase Shift Keying)** and variants (BPSK, QPSK)
- **QAM (Quadrature Amplitude Modulation)**: combines amplitude and phase variations, used in high-speed modems and Wi‑Fi.

### Physical Media

- **Twisted Pair (UTP/STP)**
  - Categories: Cat5e, Cat6, Cat6a, Cat7. Used for Ethernet (10/100/1000Base-T and beyond).
  - Pros: inexpensive, flexible. Cons: distance-limited, susceptible to EMI.
- **Coaxial Cable**
  - Historically used for early Ethernet (10BASE2, 10BASE5) and cable TV. Better shielding than unshielded twisted pair.
- **Fiber Optic**
  - Single-mode vs Multi-mode. High bandwidth, long distances, immune to EMI.
- **Wireless (Radio)**
  - Wi‑Fi (IEEE 802.11 a/b/g/n/ac/ax), Bluetooth, cellular (LTE/5G), microwave links.

### Connectors and Physical Interfaces

- Common connectors: RJ-45 (Ethernet), RJ-11 (telephone), BNC (coax), SC/LC/ST (fiber), SMA (RF), USB.
- PHY devices: physical transceivers often called PHY chips handle the analog/digital conversion and line coding.

### Multiplexing and Multiple Access

- **Frequency Division Multiplexing (FDM)**: split bandwidth into frequency bands for multiple channels.
- **Time Division Multiplexing (TDM)**: allocate time slots to channels.
- **Wavelength Division Multiplexing (WDM)**: optical multiplexing for fiber (multiple wavelengths on same fiber).
- **Multiple Access**: CSMA/CD (Ethernet), CSMA/CA (Wi‑Fi), TDMA, FDMA, CDMA (spread spectrum).

### Transmission Impairments and Solutions

- **Attenuation**: signal loss over distance. Mitigation: repeaters, amplifiers, higher-power transmitters.
- **Noise and Interference**: thermal noise, crosstalk, EMI. Mitigation: shielding, filtering, error detection/correction.
- **Distortion**: waveform changes due to medium. Mitigation: equalization and adaptive filters.
- **Latency and Jitter**: delay and variation in delay; important for real-time media.

---

## Physical Layer Devices

- **Repeaters**: regenerate and amplify signals at the physical layer.
- **Hubs**: multiport repeaters that forward bits to all ports (Layer 1 device).
- **Transceivers**: convert signals between media types (e.g., fiber media converters).

---

## TCP vs UDP: Comparison

- **Reliability**: TCP provides reliability; UDP does not.
- **Connection**: TCP is connection-oriented; UDP is connectionless.
- **Overhead**: TCP has larger header and control mechanisms; UDP is lightweight.
- **Use cases**: TCP for web, email, file transfer; UDP for DNS, VoIP, streaming, gaming.

---

## Common TCP/IP Tools and Utilities

- `ping` (uses ICMP) — reachability and round-trip time
- `traceroute` / `tracert` — path discovery and per-hop latency
- `netstat` — network connections, routing tables, interface statistics
- `tcpdump` / `Wireshark` — packet capture and analysis
- `ipconfig` / `ifconfig` / `ip` — interface configuration and addresses

---

## Example TCP Three-Way Handshake

1. Client sends SYN (SEQ=x)
2. Server replies SYN-ACK (SEQ=y, ACK=x+1)
3. Client sends ACK (ACK=y+1)

After this handshake, data transfer begins. To close a connection, TCP uses FIN/ACK sequences.

---

## Quick Reference Tables

- TCP flags: `SYN`, `ACK`, `FIN`, `RST`, `PSH`, `URG`.
- Common ports: HTTP=80, HTTPS=443, FTP=21, SSH=22, DNS=53, SMTP=25, POP3=110, IMAP=143, DHCP=67/68.

---

## Further Reading

- RFC 791 (IPv4), RFC 2460 (IPv6), RFC 793 (TCP), RFC 768 (UDP)
- IEEE 802.3 (Ethernet), IEEE 802.11 (Wi‑Fi)

---

_Prepared: Day 3 — TCP/IP and Physical Layer._
