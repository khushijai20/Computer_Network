# 🌐 Day 24: Token Ring

## 1) Introduction
Token Ring is a data link layer technology that was developed by IBM in 1984. It is an alternative to Ethernet for LAN (Local Area Network) communications. Token Ring operates on the principle of passing a "token" (special control frame) around the network in a logical ring topology, allowing only the device that holds the token to transmit data.

**Key Point:** Although Token Ring uses a physical star topology (typically), it operates logically as a ring where the token circulates sequentially from one device to the next.

---

## 2) What is a Token?
A **token** is a small, special control frame (24-bit field) that circulates continuously around the ring network.

- Only the device holding the token can transmit data
- When a device finishes transmitting, it passes the token to the next device
- If a device has no data to send, it simply passes the token along
- This prevents collisions because only one device transmits at a time

**Token composition:**
- Start Delimiter (SD): 1 byte
- Access Control (AC): 1 byte
- End Delimiter (ED): 1 byte

---

## 3) Token Ring Architecture

### Physical Topology:
- **Star topology** (physically)
- Uses a **Multistation Access Unit (MAU)** as the central hub
- Each device connects to the MAU via twisted pair cables

### Logical Topology:
- **Ring topology** (logically)
- Devices are connected in a logical ring, one after another
- The token travels sequentially from one device to the next

### Network Speed:
- **4 Mbps** (originally)
- **16 Mbps** (modern Token Ring)
- **100 Mbps** (High-Speed Token Ring - HSTR, less common)

---

## 4) How Token Ring Works

### Step-by-Step Process:

1. **Token Generation:**
   - When the network starts, an active monitor generates the token
   - The token is passed around the ring continuously

2. **Device Waits:**
   - Devices listen to the ring and forward frames
   - When a device wants to transmit, it waits for the free token

3. **Token Capture:**
   - When a device with data to send receives the free token, it changes the token state to "busy"
   - It then transmits the data frame

4. **Data Transmission:**
   - The transmitting device sends the frame containing:
     - Destination MAC address
     - Source MAC address
     - Data
     - Frame Check Sequence (FCS)

5. **Frame Circulation:**
   - The frame circulates around the ring
   - Each intermediate device reads the destination address
   - If it's not the destination, it passes the frame to the next device

6. **Frame Reception:**
   - When the frame reaches the destination, it:
     - Reads and accepts the data
     - Copies the data
     - Sets a "frame copied" bit
     - Passes the frame back to the sender

7. **Token Release:**
   - The sender receives its own frame back around the ring
   - It removes the frame from the ring
   - It releases the token (changes it to free state)
   - The next device in the ring can now use the token

---

## 5) Token Ring Frame Format

### Structure:

| Field | Size | Description |
|---|---:|---|
| Start Delimiter (SD) | 1 byte | Indicates start of frame, pattern: 10101011 |
| Access Control (AC) | 1 byte | Contains token bit, priority, and reservation |
| Frame Control (FC) | 1 byte | Indicates frame type (data, token, abort) |
| Destination MAC | 6 bytes | MAC address of receiver |
| Source MAC | 6 bytes | MAC address of sender |
| Routing Information (RI) | 0-18 bytes | Optional, for source routing |
| Data | 0-4472 bytes | Actual payload |
| Frame Check Sequence | 4 bytes | CRC for error detection |
| End Delimiter (ED) | 1 byte | Marks end of frame, pattern: 11010101 |
| Frame Status (FS) | 1 byte | Contains A and C bits (Address Recognized, Frame Copied) |

### Frame Size:
- **Minimum:** ~18 bytes (empty data frame)
- **Maximum:** ~4500 bytes (including routing info and data)

---

## 6) Token Ring Frame Format - Detailed

### A) Start Delimiter (SD)
- **Pattern:** 10101011 (binary)
- Special encoded bits that don't follow normal Manchester encoding
- Allows devices to recognize the start of a frame
- Cannot occur anywhere else in the frame

### B) Access Control (AC) Field
- **Bit 0 (T bit):** Token bit
  - **T=0** → Free token
  - **T=1** → Busy token
- **Bits 1-3 (PPP):** Priority bits (0-7)
  - Determines which priority level frames can use the token
- **Bit 4 (M bit):** Monitor bit
  - Used to detect frames looping infinitely

### C) Frame Control (FC) Field
- Indicates the type of frame:
  - **00 (MAC frame):** Management/Control frame
  - **01 (LLC frame):** Data/User frame
  - Other patterns for different purposes

### D) Destination MAC Address
- 6 bytes identifying the target device
- Can be unicast, multicast, or broadcast

### E) Source MAC Address
- 6 bytes identifying the sending device

### F) Routing Information (RI) - Optional
- Used in source routing for interconnected rings
- Contains route descriptors telling frames how to traverse bridges
- Present only when destination is on different ring

### G) Data/Payload
- Contains the actual information being transmitted
- Variable length: 0 to 4472 bytes

### H) Frame Check Sequence (FCS)
- 4 bytes (32-bit CRC)
- Used for error detection
- Sender calculates and inserts; receiver verifies

### I) End Delimiter (ED)
- **Pattern:** 11010101 (binary)
- Special encoded bits marking frame end
- Contains error detection capability

### J) Frame Status (FS)
- **A bit (Address Recognized):**
  - Set to 1 by destination when it recognizes its MAC address
- **C bit (Frame Copied):**
  - Set to 1 by destination when it successfully copies the frame

---

## 7) Token Ring Priority System

Token Ring supports **8 priority levels (0-7):**

- **Priority 0:** Lowest priority
- **Priority 7:** Highest priority

### How Priority Works:

1. A device holding the token can only transmit frames with priority ≥ token priority
2. If a device wants to transmit higher priority data, it sets the "reservation bits" to request a higher priority token
3. When the next token is generated, it will be at the higher priority level
4. Lower priority devices must wait for the priority to drop

---

## 8) Token Ring Media Types

### 1) Shielded Twisted Pair (STP)
- **Cable Type:** 150-ohm impedance
- **Distance:** Up to 100 meters per segment
- **Connectors:** Token Ring hermaphroditic connector or DB-9
- **Advantages:** Better noise immunity, supports higher speeds
- **Disadvantages:** More expensive, less flexible

### 2) Unshielded Twisted Pair (UTP)
- **Cable Type:** 100-ohm impedance
- **Distance:** Up to 45 meters per segment (for 16 Mbps)
- **Connectors:** RJ-45
- **Advantages:** Cheaper, easier to install
- **Disadvantages:** Susceptible to interference

### 3) Fiber Optic
- Used in backbone connections between MAUs
- Greater distances possible
- Better performance in high-noise environments

---

## 9) Multistation Access Unit (MAU)

### What is a MAU?
- Central hub-like device in Token Ring networks
- Connects multiple workstations in a star configuration
- Creates the logical ring internally
- Typically supports 8-10 ports

### Functions:
1. Connects devices in a logical ring
2. Provides automatic bypass capability for faulty connections
3. Monitors ring health
4. Allows hot insertion/removal of devices
5. Can be daisy-chained with other MAUs for network expansion

### MAU Features:
- **Bypass Relays:** Automatically disconnects faulty devices from the ring
- **Ring In/Out Ports:** Connect to adjacent MAUs
- **Lobe Ports:** Connect to workstations
- **Monitor Port:** Connects to active monitor

---

## 10) Active Monitor & Standby Monitors

### Active Monitor:
- **Role:** Manages ring health and stability
- **Functions:**
  1. Generates tokens at network startup
  2. Removes old frames that loop continuously (checks M bit)
  3. Maintains timing and synchronization
  4. Ensures only one token exists on the ring
  5. Performs periodic ring polls to check device status

- **Selection:** Highest MAC address device becomes active monitor

### Standby Monitors:
- Every other device on the ring
- Ready to take over if active monitor fails
- Perform periodic Active Monitor Present (AMP) frames
- If no AMP received within 7 seconds, become active monitor

---

## 11) Token Ring Access Control

### Token States:

1. **Free Token:**
   - Circulates continuously when no data transmission
   - Any device can capture it to transmit data
   - AC byte: T-bit = 0 (free)

2. **Busy Token:**
   - Indicates data frame is circulating
   - Cannot be captured by other devices
   - AC byte: T-bit = 1 (busy)

### Reservation Mechanism:
- Devices can reserve higher priority tokens
- Reservation bits (R bits) in AC field
- Used when:
  - Device needs to transmit time-critical data
  - Multiple devices contend for the token
  - Ensures fair access and priority delivery

---

## 12) Ring Maintenance & Recovery

### Duplicate Token Detection:
- Active monitor checks for multiple tokens using M-bit
- Removes duplicate tokens
- Issues new token if none detected

### Ring Poll:
- Active monitor periodically polls all devices
- Each device responds with its status
- Identifies failed or removed devices

### Beaconing:
- Triggered when device detects ring fault
- Failing device sends beacon frames upstream
- Identifies and isolates the problematic segment
- Ring can reconfigure around the fault

### Auto-Bypass:
- MAU can automatically bypass faulty workstations
- Uses bypass relays at each lobe port
- Maintains ring integrity when device fails
- Allows hot-swapping without disrupting the ring

---

## 13) Token Ring vs Ethernet

| Feature | Token Ring | Ethernet |
|---|---|---|
| **Access Method** | Token passing | CSMA/CD |
| **Topology (Physical)** | Star | Bus/Star |
| **Topology (Logical)** | Ring | Linear/Star |
| **Collision** | No collisions | Collision possible |
| **Performance** | Predictable under load | Degrades with load |
| **Speed** | 4/16 Mbps | 10-1000+ Mbps |
| **Cable Cost** | More expensive (STP) | Less expensive (UTP) |
| **Complexity** | More complex | Simpler |
| **Scalability** | Limited | High |
| **Fault Tolerance** | Better (auto-bypass) | Less fault-tolerant |
| **Priority Support** | 8 priority levels | No priority support |
| **Current Use** | Rarely used | Dominant technology |

---

## 14) Key Advantages of Token Ring

1. **Deterministic Performance:**
   - No collisions or contention
   - Predictable access times
   - Suitable for real-time applications

2. **Fair Access:**
   - All devices get equal access opportunity
   - No device can monopolize bandwidth

3. **Priority Support:**
   - 8 priority levels for different QoS requirements
   - Time-critical data can be prioritized

4. **Fault Tolerance:**
   - Auto-bypass removes faulty devices without disrupting network
   - Active/Standby monitor mechanism
   - Automatic ring recovery

5. **Token Passing Efficiency:**
   - Works well in heavy traffic loads
   - Ethernet performance degrades with collisions

6. **Better for Predictable Bandwidth:**
   - Guaranteed minimum access time
   - No unpredictable delays from collisions

---

## 15) Disadvantages of Token Ring

1. **Higher Cost:**
   - STP cabling is expensive
   - MAU hardware costs more than switches
   - Overall higher network cost

2. **Complexity:**
   - More complex protocols and hardware
   - Difficult troubleshooting
   - Requires trained personnel

3. **Slower Speed:**
   - Originally 4/16 Mbps (Ethernet now 1000+ Mbps)
   - HSTR 100 Mbps never gained adoption

4. **Limited Scalability:**
   - Adding devices increases ring latency
   - Max devices practically limited to ~250

5. **Single Point of Failure:**
   - Ring break disconnects entire network
   - Though auto-bypass helps mitigate this

6. **Token Overhead:**
   - Token circulation adds overhead
   - Small frames transmitted frequently

7. **Obsolescence:**
   - Technology is largely obsolete
   - Few vendors still support it
   - Hard to find spare parts

---

## 16) Real-World Applications

### Historical Use:
- **IBM Systems:** Primary use in IBM mainframe environments
- **Universities:** Research and educational networks
- **Government:** Some government agencies
- **Manufacturing:** Factory automation and control systems

### Modern Use:
- Largely replaced by Ethernet
- Still used in some legacy systems
- FDDI (Fiber Distributed Data Interface) was an evolution
- Concepts applied in modern technologies like MPLS

---

## 17) Token Ring Standards

### IEEE 802.5:
- Original Token Ring standard
- Published in 1985
- Defines:
  - Ring format and frame structure
  - Access control mechanism
  - Priority levels
  - Media specifications

### Variants:
- **802.5 4 Mbps:** Original specification
- **802.5r 16 Mbps:** Higher speed version
- **ANSI/IEEE 802.5:** American national standard
- **HSTR (High-Speed Token Ring):** 100 Mbps (less successful)

---

## 18) Token Ring in OSI Model

### Physical Layer (Layer 1):
- Defines media types: STP, UTP, fiber optic
- Specifies electrical/optical signals
- Transmission speed specifications

### Data Link Layer (Layer 2):
- Token generation and passing mechanism
- Frame format and structure
- MAC addressing and access control
- Priority and reservation system
- Error detection (CRC/FCS)
- Ring management and monitoring

### Network Layer & Above:
- Token Ring provides MAC layer only
- Upper layers (IP, TCP) operate independently
- Bridge connections for multi-ring networks

---

## 19) Quick Reference: Token Ring vs Modern Networks

| Aspect | Token Ring | Modern Networks |
|---|---|---|
| **Speed** | 4-16 Mbps | 100 Mbps - 100+ Gbps |
| **Reliability** | Deterministic | Statistical (probabilistic) |
| **Implementation** | Complex protocol | Simpler protocols |
| **Adoption** | Historical/Obsolete | Industry standard (Ethernet) |
| **Learning Value** | Academic/Historical | Limited modern application |
| **Fault Recovery** | Auto-bypass | VLAN/redundancy |

---

## 20) Summary

**Token Ring Key Points:**
- ✅ Logical ring, physical star topology
- ✅ Token passing ensures no collisions
- ✅ Deterministic, predictable performance
- ✅ Supports 8 priority levels
- ✅ Automatic fault bypass and recovery
- ✅ More complex and expensive than Ethernet
- ✅ Largely obsolete, replaced by Ethernet
- ✅ Historical importance in networking evolution
- ✅ Operates at Data Link Layer (Layer 2)
- ✅ Used STP, UTP, or fiber optic cabling

---

## 21) Common Interview Questions

**Q1: Why is Token Ring called "Token Ring"?**
A: Because it uses a token (special control frame) that passes from device to device in a logical ring formation.

**Q2: How does Token Ring prevent collisions?**
A: By ensuring only one device can transmit at a time—only the device holding the token can send.

**Q3: What is the maximum number of devices on a Token Ring network?**
A: Practically limited to ~250 devices, but usually much less due to latency and ring traversal time.

**Q4: Why did Token Ring fail in the market?**
A: Ethernet offered lower cost, simpler implementation, and eventually much higher speeds. Token Ring's complexity and expense made it uncompetitive.

**Q5: What is beaconing in Token Ring?**
A: Mechanism for detecting and isolating ring faults by sending beacon frames upstream to identify the problem area.

**Q6: How many tokens should exist on a Token Ring network?**
A: Exactly one token at any given time. Multiple tokens indicate a ring problem.

**Q7: What is the role of the Active Monitor?**
A: Generate tokens, remove looping frames (using M-bit), maintain synchronization, and ensure network stability.

**Q8: Explain priority and reservation in Token Ring.**
A: Token Ring supports 8 priority levels; devices can set reservation bits to request higher-priority tokens for time-sensitive data.

---

## 📚 Additional Resources

- IEEE 802.5 Standard (Token Ring)
- IBM Token Ring Architecture Documentation
- Network+ and CCNA study materials
- Computer Network Textbooks (Kurose & Ross, Tanenbaum)

---

**Last Updated:** September 8, 2026
**Status:** Comprehensive Overview ✅
