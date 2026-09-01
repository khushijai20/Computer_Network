# 🌐 Day 19: Media Access Control (MAC)

## 1) Introduction
Media Access Control (MAC) is a sublayer of the Data Link Layer (Layer 2 of the OSI model).

It is responsible for deciding how devices in a shared network medium access the communication channel and transmit data without conflict.

In simple words:
- MAC decides who gets to send data first
- It manages access to the shared medium
- It helps avoid collisions and ensures proper communication

---

## 2) What is MAC?
MAC stands for Media Access Control.

It is the protocol that governs access to the transmission medium in a network.

It is used in:
- Ethernet LANs
- Wi-Fi networks
- bus and shared-medium networks

The MAC sublayer is above the Physical Layer and below the Logical Link Control (LLC) sublayer.

---

## 3) Why MAC is Needed
In a network, multiple devices may try to send data at the same time.

If they all transmit simultaneously on the same medium, collisions occur and data gets corrupted.

MAC helps prevent or manage these collisions.

So MAC is necessary because:
- many stations share the same channel
- only one device should transmit at a time in a shared medium
- the network must be organized and fair
- collisions must be reduced or resolved

---

## 4) Functions of MAC Sublayer
The MAC sublayer performs several important tasks:

- controls access to shared transmission medium
- identifies source and destination devices using MAC addresses
- frames data for transmission
- detects collisions in some protocols
- resolves contention among stations
- ensures orderly communication in LANs

---

## 5) MAC Address
A MAC address is a unique hardware address assigned to a network interface card (NIC).

### Features:
- 48-bit address
- usually written in hexadecimal
- example: 00:1A:2B:3C:4D:5E
- unique for each device on a local network

### Types:
- Unicast: one destination device
- Multicast: group of devices
- Broadcast: all devices on the network

MAC addresses operate inside the local network, not across the whole internet.

---

## 6) MAC vs LLC
The Data Link Layer has two sublayers:

### a) LLC (Logical Link Control)
- provides interface to the Network Layer
- handles error and flow control services
- more logical and upper-layer oriented

### b) MAC (Media Access Control)
- handles channel access
- decides when a device can transmit
- deals with addressing and collision control

So:
- LLC: logic and communication management
- MAC: access to the medium

---

## 7) Need for Shared Medium Access
In networks like Ethernet or wireless LANs, many devices may share the same medium.

If all devices transmit at once:
- signals overlap
- data gets corrupted
- collisions happen
- communication fails

MAC protocols decide how to share the medium fairly and efficiently.

---

## 8) Classification of MAC Protocols

MAC protocols are generally classified into:

1. Random Access Protocols
2. Controlled Access Protocols
3. Channelization / Fixed Assignment Protocols

---

### A) Random Access Protocols
In random access, stations transmit whenever they have data.

They do not coordinate in advance, so collisions may happen.

Examples:
- ALOHA
- Slotted ALOHA
- CSMA
- CSMA/CD
- CSMA/CA

#### Advantages:
- simple
- no centralized control
- good for low-load networks

#### Disadvantages:
- collisions can occur
- efficiency may reduce under heavy load

---

### B) Controlled Access Protocols
In controlled access, stations take turns to use the medium.

Examples:
- Polling
- Token Passing
- Reservation-based access

#### Advantages:
- less collision risk
- more orderly access

#### Disadvantages:
- more complex
- may waste time if a station has no data

---

### C) Channelization / Fixed Assignment
The medium is divided into channels or time slots.

Examples:
- FDMA
- TDMA
- CDMA

Used mainly in cellular systems and satellite communication.

---

## 9) ALOHA
ALOHA is one of the earliest random access protocols.

### Basics:
- every station can transmit as soon as it has data
- if collision occurs, the sender waits and retransmits later

### Types:
- Pure ALOHA
- Slotted ALOHA

### Pure ALOHA
- stations transmit whenever ready
- collision probability is high

### Slotted ALOHA
- time is divided into slots
- stations transmit only at slot boundaries
- reduces collisions compared to pure ALOHA

#### Drawback:
- collisions still happen
- inefficient at high traffic

---

## 10) CSMA (Carrier Sense Multiple Access)
CSMA means stations sense the medium before transmitting.

### Principle:
Before sending, a station listens to check whether the medium is idle.

### Types:
- 1-persistent CSMA
- non-persistent CSMA
- p-persistent CSMA

### Advantages:
- reduces collisions compared to ALOHA
- better efficiency

### Problem:
Even if a station senses the channel idle, another station may start transmitting nearly at the same time, causing collision.

---

## 11) CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
CSMA/CD is used in wired Ethernet.

### Working:
1. Station senses the medium
2. If idle, it transmits
3. While transmitting, it listens for collisions
4. If collision is detected, it stops sending and sends a jam signal
5. The station waits a random time and retransmits

### Used in:
- classic Ethernet
- bus topology Ethernet
- shared medium wired LANs

### Why important?
CSMA/CD helped Ethernet manage collisions effectively before switched Ethernet became dominant.

---

## 12) CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)
CSMA/CA is used in wireless networks like Wi-Fi.

### Working:
- station senses the medium
- if idle, it waits for a random time before transmitting
- this reduces the chance of collisions
- it uses RTS/CTS exchange in some cases

### Why not collision detection?
In wireless networks, stations cannot reliably detect collisions because they cannot transmit and listen at the same time.

So Wi-Fi uses collision avoidance rather than detection.

### Used in:
- Wi-Fi networks
- IEEE 802.11

---

## 13) Token Passing
Token passing is a controlled access method.

### Principle:
A special frame called a token is passed around the network. Only the station holding the token may transmit.

### Example:
- station A has token → sends
- passes token to station B
- station B sends
- continues around the ring or sequence

### Types:
- Token Ring
- Token Bus

### Advantages:
- no collisions
- deterministic medium access
- fair access

### Disadvantages:
- token loss or failure can disrupt network
- more complex than random access

---

## 14) Polling
Polling is a controlled access strategy where a central controller asks each station if it wants to send data.

### Process:
- controller sends a poll message
- station responds if it has data
- then next station is polled

### Advantages:
- orderly communication
- reduces collisions

### Disadvantages:
- controller overhead
- delays if many stations are idle

---

## 15) FDMA, TDMA, CDMA
These are channelization methods.

### FDMA (Frequency Division Multiple Access)
- bandwidth is divided into frequency bands
- each user gets a unique frequency band

### TDMA (Time Division Multiple Access)
- time is divided into slots
- each user gets a time slot

### CDMA (Code Division Multiple Access)
- each user uses a unique code
- multiple users share the same frequency band simultaneously

These are mainly used in cellular and mobile communication systems.

---

## 16) Ethernet and MAC
Ethernet is the most common LAN technology.

### Ethernet MAC features:
- uses MAC addresses
- frames are transmitted between devices
- uses CSMA/CD in older shared bus Ethernet
- uses switching in modern Ethernet

### Ethernet frame contains:
- destination MAC address
- source MAC address
- type or length
- data
- CRC

This is part of the Data Link Layer framing and access control.

---

## 17) Wi-Fi and MAC
Wireless LANs use MAC protocols designed for collision avoidance.

### Wi-Fi uses:
- CSMA/CA
- RTS/CTS
- backoff timers
- ACK frames

### Why Wi-Fi differs from Ethernet:
- wireless medium is noisy
- stations cannot detect collisions as reliably as in wired Ethernet
- hidden terminal and exposed terminal problems exist

So wireless MAC is designed to avoid collisions rather than detect them after the fact.

---

## 18) Collision and Contention
### Collision
Occurs when two or more devices transmit at the same time on the same medium.

### Contention
The process of competing to access the same shared medium.

MAC protocols deal with contention and try to reduce or resolve collisions.

---

## 19) Hidden Station Problem
In wireless networks, a station may not detect another station’s transmission because they are out of range of each other.

This creates:
- hidden terminal problem
- collisions when both begin transmitting simultaneously

This is why RTS/CTS is often used in Wi-Fi.

---

## 20) Exposed Station Problem
A station may hesitate to transmit even though another station is not in its interference range.

This reduces throughput unnecessarily.

---

## 21) MAC in LANs vs WANs
### LANs
- local communication
- many devices on a single segment
- typical MAC protocols: Ethernet, Wi-Fi, token passing

### WANs
- long-distance communication
- may use point-to-point or packet-switched links
- MAC is less central than in LAN technologies

---

## 22) Important Exam Points
- MAC is part of the Data Link Layer
- It controls access to the shared medium
- It uses MAC addresses
- Random access protocols include ALOHA and CSMA
- CSMA/CD is used in Ethernet
- CSMA/CA is used in Wi-Fi
- Token passing is a controlled access method
- Polling is also controlled access
- Collision is common in shared-medium systems
- MAC is crucial for efficient and fair network communication

---

## 23) Comparison Table

| Protocol | Type | Used In | Main Feature |
|---|---|---|---|
| ALOHA | Random Access | Early satellite networks | Simple but collision-prone |
| CSMA | Random Access | Shared medium LANs | Sense before transmit |
| CSMA/CD | Random Access | Wired Ethernet | Detects collisions |
| CSMA/CA | Random Access | Wi-Fi | Avoids collisions |
| Token Passing | Controlled Access | Token Ring | Only token holder transmits |
| Polling | Controlled Access | Some LANs | Controller grants access |
| FDMA/TDMA/CDMA | Channelization | Cellular systems | Divides channel by frequency/time/code |

---

## 24) Short Definition
Media Access Control is the process by which devices in a shared network decide when and how to access the transmission medium.

---

## 25) Final Summary
Media Access Control is one of the key functions of the Data Link Layer. It ensures that multiple devices can share the same communication medium without causing chaos. Different MAC protocols exist depending on the network type: random access for Ethernet and Wi-Fi, controlled access for token-based systems, and channelization for cellular networks. MAC also uses hardware addresses, which allow devices to identify each other on a local network.
