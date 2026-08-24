# Day 13: Circuit Switching, Packet Switching, Message Switching and Virtual Switching

## Overview

**Switching** is the process of forwarding data from a source device to a destination device through one or more intermediate network devices. It allows many users to share the same communication network.

The main switching techniques are:

1. **Circuit switching**
2. **Packet switching**
3. **Message switching**
4. **Virtual-circuit switching** (often called virtual switching in basic networking notes)

---

## Topics Covered

- Meaning and purpose of switching
- Circuit switching and its three phases
- Packet switching and its two forms
- Message switching and store-and-forward delivery
- Virtual-circuit switching
- Datagram networks vs virtual-circuit networks
- Comparison of switching techniques
- Advantages, disadvantages, examples, and exam questions

---

## Learning Objectives

After studying this topic, you should be able to:

- Explain why switching is required in computer networks.
- Describe the setup, data-transfer, and teardown phases of circuit switching.
- Distinguish between datagram and virtual-circuit packet switching.
- Explain how message switching stores and forwards complete messages.
- Compare delay, resource usage, reliability, and suitability of each technique.
- Identify practical examples of each switching method.

---

## 1. What Is Switching?

A network usually contains more devices than a single physical link can connect directly. Switching selects a path and transfers data through intermediate nodes such as switches or routers.

### Basic terms

- **Source:** Device that sends data.
- **Destination:** Device that receives data.
- **Intermediate node:** Device that forwards data.
- **Switching node:** A device that chooses an outgoing link for incoming data.
- **Route/path:** The sequence of links and nodes used by data.
- **Hop:** One movement from one network device to the next.

A switching technique determines:

- Whether a dedicated path is reserved.
- The unit of data transferred.
- How much storage is required at intermediate nodes.
- Whether packets may take different routes.
- When network resources are allocated and released.

---

## 2. Circuit Switching

In **circuit switching**, a dedicated end-to-end communication path is established before data transmission begins. The reserved path remains available for the entire session.

Traditional telephone networks are the classic example.

### Three phases of circuit switching

#### 2.1 Circuit establishment

The source sends a request to the destination. Intermediate switches select a path and reserve resources such as bandwidth and time slots.

If resources are available, an end-to-end circuit is created. If not, the call or connection is rejected or delayed.

#### 2.2 Data transfer

After setup, data travels over the reserved circuit. The data normally does not need to carry a complete destination address at every hop because the switches already know the established circuit.

#### 2.3 Circuit teardown

When communication ends, a teardown message releases the reserved links and resources so that other users can use them.

### Types of circuit switching

| Type | Description |
|---|---|
| Space-division switching | Uses a separate physical path or crosspoint for the connection. |
| Time-division switching | Assigns a repeating time slot to the connection. |
| Frequency-division switching | Assigns a frequency band to the connection. |

### Advantages

- Provides predictable bandwidth after setup.
- Gives a fixed and ordered path.
- Usually has low and consistent delay during transfer.
- No packet reordering is normally required.
- Suitable for continuous real-time communication.

### Disadvantages

- Initial setup causes delay.
- Bandwidth remains reserved even when no data is being sent.
- A failed link can interrupt the whole circuit.
- Inefficient for bursty data with long idle periods.
- A circuit may be blocked when no complete path has free resources.

### Example

A traditional telephone call reserves a communication path for the duration of the call. Silence during the call may still consume the reserved circuit.

---

## 3. Packet Switching

In **packet switching**, a large message is divided into smaller units called **packets**. Each packet contains control information, such as source and destination addresses, and is forwarded through the network.

Resources are shared dynamically, so a link can carry packets belonging to many different communications.

### Parts of a packet

A packet commonly contains:

- **Header:** Addressing, sequencing, length, and control information.
- **Payload:** The actual user data.
- **Trailer:** Error-detection information in some protocols.

### Two forms of packet switching

#### 3.1 Datagram packet switching

Each packet is treated independently. Packets from the same message may travel over different paths and may arrive out of order.

Characteristics:

- No connection setup is required.
- Each packet carries enough addressing information.
- The network can route around failures or congestion.
- The destination may need to reorder and reassemble packets.
- The Internet Protocol (IP) uses a datagram approach.

#### 3.2 Virtual-circuit packet switching

A logical path is established before packets are sent. Packets follow the same logical route and may use a short virtual-circuit identifier instead of carrying a full route decision at every switch.

Characteristics:

- Requires a setup phase and usually a release phase.
- Packets normally arrive in order.
- Resources may or may not be reserved, depending on the technology.
- A route failure can require a new virtual circuit.
- It combines connection-oriented control with packet-based sharing.

### Advantages of packet switching

- Efficient for bursty data.
- Many users can share link capacity.
- No dedicated circuit is required for the whole session.
- Datagram networks can adapt to failures and congestion.
- Smaller packets reduce the amount of data that must be retransmitted after an error.

### Disadvantages of packet switching

- Variable delay and jitter can occur.
- Packets may be lost, duplicated, or delivered out of order.
- Each packet needs header overhead.
- Congestion can cause queuing and packet drops.
- Real-time applications may require buffering and quality-of-service mechanisms.

### Example

When a web page is loaded, its data is split into packets. Packets from several users share the same links and are forwarded by routers toward their destinations.

---

## 4. Message Switching

In **message switching**, the complete message is treated as one unit. Each intermediate switch receives the entire message, stores it, and then forwards it to the next node.

This is called **store-and-forward switching**.

### Working

1. The source sends the complete message to the first switching node.
2. The node stores the message in memory or on disk.
3. The node examines the destination address.
4. The node forwards the complete message when the next link is available.
5. The process repeats until the message reaches the destination.

### Advantages

- No dedicated path is required.
- Links are used efficiently through sharing.
- A switching node can queue messages during temporary link unavailability.
- The network can select different routes for different messages.
- Message priority can be supported in some systems.

### Disadvantages

- A complete message may require a large amount of storage.
- Delay can be very high because the full message must be received before forwarding.
- Not suitable for interactive voice or live video.
- A large message can occupy a link for a long time.
- A failure may require retransmission of the entire message.

### Example

Early telegraph and email-style store-and-forward systems used message-oriented delivery. Modern networks generally use packet switching instead because packets are smaller and easier to manage.

---

## 5. Virtual-Circuit Switching

**Virtual-circuit switching** is a connection-oriented form of packet switching. It creates a logical connection between source and destination, but the physical links are still shared by many users.

The word **virtual** means that the path behaves like a dedicated connection logically, even though no single physical circuit is permanently dedicated to that user.

### Phases

1. **Setup:** A virtual-circuit identifier and route are selected.
2. **Data transfer:** Packets follow the established logical path.
3. **Teardown:** The virtual circuit is removed after communication ends.

### Virtual-circuit table

Each switching device can maintain an entry such as:

| Incoming interface | Incoming VC ID | Outgoing interface | Outgoing VC ID |
|---|---:|---|---:|
| 1 | 27 | 3 | 42 |

The switch reads the incoming virtual-circuit identifier and replaces it with the outgoing identifier before forwarding the packet.

### Characteristics

- Connection-oriented.
- Packets normally follow the same route.
- Packets normally arrive in order.
- A small identifier can replace a full destination address after setup.
- Setup delay is required before data transfer.
- The logical path may be removed if a link or node fails.

### Advantages

- More predictable delivery than datagram switching.
- Less addressing overhead during transfer.
- Packets usually remain in sequence.
- Can support traffic management and quality of service.
- Often provides better control for long-lived flows.

### Disadvantages

- Setup and teardown add overhead.
- A route failure can affect the whole logical connection.
- Switches must maintain virtual-circuit state.
- The network may need to establish a new circuit after failure.

### Examples

Examples of technologies associated with virtual-circuit concepts include **X.25**, **Frame Relay**, and **ATM**. MPLS uses label-switched paths that are conceptually similar in some respects, although it is not identical to traditional virtual-circuit switching.

> Note: In some notes, “virtual switching” may refer to virtual-circuit switching. In modern Ethernet discussions, “virtual switching” can also mean software-based switching, such as a virtual switch in a hypervisor. These are related ideas but not the same technology.

---

## 6. Datagram vs Virtual-Circuit Switching

| Feature | Datagram packet switching | Virtual-circuit switching |
|---|---|---|
| Connection | Connectionless | Connection-oriented |
| Setup phase | Not required | Required |
| Route | May differ for every packet | Usually fixed for the virtual circuit |
| Addressing | Full destination address in each packet | VC identifier after setup |
| Packet order | May be out of order | Usually in order |
| Failure handling | Can often route around failure | May require a new virtual circuit |
| State in switches | Little or no per-flow state | Per-virtual-circuit state |
| Example | IP network | X.25, Frame Relay, ATM |

---

## 7. Comparison of Switching Techniques

| Feature | Circuit switching | Packet switching | Message switching | Virtual-circuit switching |
|---|---|---|---|---|
| Dedicated path | Yes | No | No | Logical path only |
| Data unit | Continuous bit stream or fixed stream | Packet | Complete message | Packet |
| Setup required | Yes | No for datagrams; yes for virtual circuits | No | Yes |
| Intermediate storage | Small buffers may be used | Packet buffers | Large storage for full messages | Packet buffers and circuit state |
| Delay | Setup delay, then predictable | Variable | Usually high | Setup delay, then more predictable |
| Resource use | Can be wasteful during idle time | Efficient for bursty data | Efficient sharing, but large queues | Shared and controlled |
| Packet reordering | Not applicable | Possible | Not applicable for one message | Usually not needed |
| Best suited for | Traditional voice and continuous traffic | Internet data and bursty traffic | Non-real-time delivery | Long-lived, controlled packet flows |

---

## 8. Delay in Switching Networks

Total network delay can include:

- **Processing delay:** Time to examine headers and make a forwarding decision.
- **Queuing delay:** Time waiting in a buffer because the outgoing link is busy.
- **Transmission delay:** Time to place all bits on the link.
- **Propagation delay:** Time for the signal to travel through the medium.
- **Setup delay:** Time to establish a circuit or virtual circuit.

A simplified expression is:

```text
Total delay = processing + queuing + transmission + propagation + setup delay
```

Circuit switching usually has a noticeable setup delay but predictable transfer delay afterward. Datagram packet switching has no setup delay but may experience variable queuing delay at every hop.

---

## 9. Switching and Multiplexing

**Multiplexing** combines multiple signals over one physical link. **Switching** chooses where those signals or data units should go.

They often work together:

- Circuit switching may use time-division or frequency-division multiplexing.
- Packet switching statistically multiplexes packets from many users.
- Virtual circuits use labels or identifiers to share links while preserving logical connections.

### Statistical multiplexing

Packet switching commonly uses **statistical multiplexing**. Capacity is assigned to whichever user currently has data to send instead of reserving a fixed portion for every user.

This improves utilization, but congestion can occur when many users transmit simultaneously.

---

## 10. Real-World Uses

| Use case | Suitable technique or concept | Reason |
|---|---|---|
| Traditional telephone call | Circuit switching | Continuous communication and predictable resources |
| Web browsing | Datagram packet switching | Bursty data and shared network capacity |
| File transfer | Packet switching | Reliable delivery can be provided by higher-layer protocols |
| Email delivery | Store-and-forward message concepts | Delay is acceptable and delivery can be queued |
| ATM or Frame Relay service | Virtual-circuit switching | Logical connections and controlled forwarding |
| Virtual machines communicating on one host | Virtual switch | Software-based Layer 2 forwarding |

---

## 11. Important Definitions

- **Circuit switching:** Switching method that reserves a dedicated end-to-end path for a session.
- **Packet switching:** Switching method that divides data into packets and shares network links dynamically.
- **Message switching:** Switching method that stores and forwards an entire message at each intermediate node.
- **Store-and-forward:** Receiving and storing data before sending it to the next node.
- **Datagram:** An independently routed packet in a connectionless network.
- **Virtual circuit:** A logical connection established over shared physical links.
- **Connection-oriented:** A communication method that establishes logical state before data transfer.
- **Connectionless:** A communication method that sends each packet independently without prior setup.
- **Jitter:** Variation in packet delay over time.
- **Congestion:** A condition in which offered traffic exceeds available network capacity.
- **Statistical multiplexing:** Dynamic sharing of a link according to current traffic demand.

---

## 12. Example Questions and Answers

### Q1. What is the main difference between circuit and packet switching?

**Answer:** Circuit switching reserves a dedicated path for the entire session. Packet switching divides data into packets and shares network resources dynamically.

### Q2. Why is message switching called store-and-forward switching?

**Answer:** Each intermediate node stores the complete message before forwarding it to the next node.

### Q3. What happens if packets take different paths in a datagram network?

**Answer:** They may experience different delays and may arrive out of order. The destination or a higher-layer protocol can reorder and reassemble them.

### Q4. Why is circuit switching inefficient for bursty traffic?

**Answer:** Resources remain reserved even during periods when the user has no data to send.

### Q5. What is a virtual circuit?

**Answer:** It is a logical, connection-oriented path established across a shared packet-switched network.

### Q6. Which technique is most suitable for traditional voice calls?

**Answer:** Circuit switching, because it provides predictable bandwidth and delay after setup.

### Q7. Which technique is used by the Internet?

**Answer:** The Internet primarily uses connectionless datagram packet switching through IP. Reliable, ordered delivery may be provided by protocols such as TCP.

### Q8. Is a virtual circuit a physically dedicated cable?

**Answer:** No. It is a logical path using shared physical links.

### Q9. Why is packet switching more efficient than circuit switching for web traffic?

**Answer:** Web traffic is bursty. Packet switching allows link capacity to be used by other users when one user is idle.

### Q10. What is the major disadvantage of message switching?

**Answer:** Intermediate nodes must store the complete message, which can require large storage and produce high delay.

---

## Quick Revision Notes

- **Circuit switching:** Reserve first, communicate, release later.
- **Packet switching:** Divide data into packets and share links.
- **Datagram:** No setup; each packet is routed independently.
- **Message switching:** Store and forward the whole message.
- **Virtual circuit:** Set up a logical path, then send packets along it.
- Circuit switching gives predictable service but wastes capacity during idle periods.
- Packet switching is efficient but can have congestion, loss, jitter, and reordering.
- Message switching is suitable for delay-tolerant communication, not live communication.
- Virtual-circuit switching combines packet-based sharing with connection-oriented behavior.
- The Internet primarily uses datagram packet switching.

## Final Summary

Switching determines how data moves through a network. Circuit switching reserves a dedicated path and is predictable, but it can waste resources. Packet switching divides data into packets and uses shared links efficiently, but it may introduce variable delay and packet loss. Message switching stores and forwards complete messages, which makes it flexible but slow and storage-intensive. Virtual-circuit switching establishes a logical path and forwards packets over shared links, offering ordered and more controlled communication.

## Tomorrow's Preview

Next, study **VLANs, access ports, trunk ports, and inter-VLAN routing**.
