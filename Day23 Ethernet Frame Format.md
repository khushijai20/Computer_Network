# 🌐 Day 23: Ethernet Frame Format

## 1) Introduction
Ethernet is the most widely used Local Area Network (LAN) technology. It operates at the Data Link Layer (Layer 2) of the OSI model.

At Layer 2, data is transmitted in the form of a frame. An Ethernet frame is a structured packet of bits that contains:
- source and destination MAC addresses
- control information
- payload (actual data)
- error detection bits

The frame format is standardized to allow communication between devices on the same LAN.

---

## 2) What is an Ethernet Frame?
An Ethernet frame is the Data Link Layer PDU (Protocol Data Unit).

It contains:
- header
- data/payload
- trailer

In simple words:

Sender -> Data -> Ethernet Frame -> Physical Medium -> Receiver

The frame is created by the Ethernet controller and sent over the wire.

---

## 3) Ethernet Frame Structure
A standard Ethernet frame has the following main parts:

1. Preamble
2. Start Frame Delimiter (SFD)
3. Destination MAC Address
4. Source MAC Address
5. Length/Type field
6. Data/Payload
7. Frame Check Sequence (FCS)

---

## 4) Standard Ethernet Frame Format (IEEE 802.3)

| Field | Size | Description |
|---|---:|---|
| Preamble | 7 bytes | Used for synchronization, alternating 0s and 1s |
| SFD | 1 byte | Marks the start of the frame |
| Destination MAC | 6 bytes | MAC address of the receiver |
| Source MAC | 6 bytes | MAC address of the sender |
| Length/Type | 2 bytes | Indicates either payload length or EtherType |
| Data/Payload | 46 to 1500 bytes | Actual data being carried |
| Pad | 0 to 46 bytes | Added if payload is too small |
| FCS | 4 bytes | CRC for error detection |

### Total frame size
- Minimum Ethernet frame size = 64 bytes
- Maximum Ethernet frame size = 1518 bytes

This includes:
- header = 18 bytes (without VLAN)
- payload = 46 to 1500 bytes
- trailer = 4 bytes FCS

Hence:
- minimum = 18 + 46 + 4 = 68 bytes? 

Important note:
The actual minimum frame size on the wire is 64 bytes, because the preamble and SFD are also included in the physical frame on the wire.

So the practical Ethernet frame on the wire is:
- 7 + 1 + 6 + 6 + 2 + payload + 4 = 64 to 1518 bytes

---

## 5) Field-by-Field Explanation

### A) Preamble
- Size: 7 bytes
- Purpose: Allows the receiving NIC to synchronize with the incoming signal
- Pattern: alternating 10101010...

### B) Start Frame Delimiter (SFD)
- Size: 1 byte
- Purpose: Indicates the end of the preamble and the beginning of the actual frame
- Pattern: 10101011

### C) Destination MAC Address
- Size: 6 bytes
- Purpose: Identifies the recipient device on the LAN
- Example: `00:1A:2B:3C:4D:5E`

### D) Source MAC Address
- Size: 6 bytes
- Purpose: Identifies the sender device
- Helps the receiving side know from whom the data came

### E) Length/Type Field
- Size: 2 bytes
- This field has two roles:

#### If value <= 1500
It is interpreted as the length of the payload.

#### If value > 1500
It is interpreted as EtherType, which tells the upper layer protocol type.

Examples of EtherType:
- 0x0800 -> IPv4
- 0x0806 -> ARP
- 0x86DD -> IPv6
- 0x8100 -> VLAN Tag

### F) Data/Payload
- Size: 46 to 1500 bytes
- Contains the actual upper-layer data
- Examples: IP packet, ARP packet, etc.

### G) Padding
If the payload is smaller than 46 bytes, Ethernet adds padding to reach the minimum size.

This ensures:
- collisions are detected properly
- minimum frame size requirement is met

### H) Frame Check Sequence (FCS)
- Size: 4 bytes
- Contains CRC value
- Used to detect transmission errors

The receiver recomputes CRC and compares it with the received FCS.

If mismatch occurs, the frame is discarded.

---

## 6) Ethernet Header Format (Simple Diagram)

```text
+----------------------+--------------------+--------------------+----------------------+----------------------+----------------------+--------------------+
| Preamble (7 bytes)    | SFD (1 byte)       | Dest MAC (6)       | Src MAC (6)         | Type/Len (2)         | Data (46-1500)       | FCS (4)            |
+----------------------+--------------------+--------------------+----------------------+----------------------+----------------------+--------------------+
```

---

## 7) Ethernet II vs IEEE 802.3
There are two common Ethernet frame types:

### A) Ethernet II (DIX Ethernet)
Most common in modern LANs.

- Uses EtherType field to identify the upper-layer protocol
- Most Internet traffic uses Ethernet II

### B) IEEE 802.3
- Uses the length field to specify payload size
- Common in older networking standards

### Key difference
- Ethernet II: frame type is identified using Type field
- 802.3: frame length is identified using Length field

---

## 8) VLAN Tagged Ethernet Frame
When VLAN tagging is used, a 4-byte VLAN tag is inserted between the source MAC and Type field.

### VLAN tag contains:
- TCI (Tag Control Information)
- Priority bits
- CFI bit
- VLAN ID

### VLAN frame structure
```text
Preamble | SFD | Destination MAC | Source MAC | 802.1Q Tag | Type | Data | FCS
```

The EtherType value becomes `0x8100` for VLAN-tagged frames.

---

## 9) Minimum and Maximum Frame Size
### Minimum frame size
- 64 bytes on the wire
- Includes header + data + CRC + preamble + SFD

### Maximum frame size
- 1518 bytes for normal Ethernet
- With VLAN tag: 1522 bytes

### Why this matters
Ethernet requires minimum frame size to ensure reliable collision detection.

If a frame is too short, a sender may finish transmitting before the collision signal reaches other nodes.

---

## 10) Role of MAC Addresses in Ethernet Frame
The Ethernet frame uses MAC addresses (Media Access Control addresses) to identify devices.

### MAC address characteristics
- 48-bit address
- written in hexadecimal
- usually 12 digits grouped as 6 pairs
- Example: `00:0A:35:00:1E:5A`

### Destination MAC
- identifies the receiver NIC

### Source MAC
- identifies the sender NIC

This helps Ethernet switches forward frames within the LAN.

---

## 11) Error Detection in Ethernet Frame
Ethernet uses CRC (Cyclic Redundancy Check) in the FCS field.

### Purpose
- detect transmission errors
- detect corrupted bits caused by noise or interference

### How it works
The sender calculates CRC over the frame and appends it.
The receiver recalculates it.
If different, the frame is dropped.

### Important point
CRC can detect errors, but it cannot correct them.

---

## 12) Why Ethernet Frames Matter
Ethernet frames are important because:
- they provide addressing at Layer 2
- support communication inside a LAN
- enable switching and forwarding
- detect errors through FCS
- allow compatibility among devices

Without the frame format, devices would not know where to send the data or how to interpret it.

---

## 13) Exam-Friendly Summary
### Ethernet frame is the Layer 2 PDU.
### Standard Ethernet frame contains:
- Preamble
- SFD
- Destination MAC
- Source MAC
- Type/Length
- Data
- FCS

### Minimum size: 64 bytes
### Maximum size: 1518 bytes
### FCS uses CRC for error detection
### MAC addresses are 48-bit hardware addresses
### Type field identifies protocol (IPv4, ARP, IPv6)

---

## 14) Very Short Revision Notes
- Ethernet works at Layer 2
- Ethernet frame is used for data transmission in LANs
- MAC addresses are used for local delivery
- FCS provides CRC-based error detection
- Frame length can be interpreted as EtherType or payload length depending on value
- Ethernet II is the most common format today

---

## 15) Important Questions
### Q1. What is the Layer 2 PDU called?
Answer: Frame

### Q2. What does the MAC address do?
Answer: Identifies the sender and receiver on the LAN

### Q3. What does FCS contain?
Answer: CRC value for error detection

### Q4. What is the minimum Ethernet frame size?
Answer: 64 bytes

### Q5. What is the maximum Ethernet frame size?
Answer: 1518 bytes (or 1522 with VLAN tagging)

### Q6. What field identifies the upper-layer protocol?
Answer: Type field in Ethernet II

---

## 16) Final Takeaway
Ethernet frame format is a standard way of organizing data for local network transmission. It ensures efficient delivery, MAC-based addressing, protocol identification, and error checking. In practical networking, Ethernet frames are the foundation of LAN communication.

> Ethernet is the most common LAN technology because it is simple, efficient, and widely supported by switches, NICs, and routers.

---

## ✅ Day 23 Completion Note
This topic is very important for Computer Networks exams, especially for:
- Data Link Layer
- MAC addressing
- switching
- LAN communication
- error detection

If you revise this once every 2 days, you will remember the frame fields and sizes easily.
