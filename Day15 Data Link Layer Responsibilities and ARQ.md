# Day 15: Data Link Layer Responsibilities and ARQ Protocols

## Overview

The **Data Link Layer** is Layer 2 of the OSI model. It provides **node-to-node delivery** of data across a single physical link or local network. It receives packets from the Network Layer, places them inside frames, controls access to the shared medium, detects errors, and may retransmit damaged or lost frames.

This lesson covers:

- Framing
- Stop-and-Wait ARQ
- Go-Back-N ARQ
- Selective Repeat ARQ
- A comparison of the three ARQ protocols

---

## Topics Covered

1. Data Link Layer responsibilities
2. Frames and framing methods
3. Flow control and error control
4. Automatic Repeat reQuest (ARQ)
5. Stop-and-Wait ARQ
6. Go-Back-N ARQ
7. Selective Repeat ARQ
8. Sequence numbers, acknowledgements, and timers
9. Protocol comparison and exam questions

---

## Learning Objectives

After studying this note, you should be able to:

- Explain the role of the Data Link Layer.
- Describe why bits must be grouped into frames.
- Distinguish flow control from error control.
- Explain how acknowledgements, timers, and sequence numbers work.
- Compare Stop-and-Wait, Go-Back-N, and Selective Repeat ARQ.
- Calculate useful formulas involving window size and sequence-number bits.

---

## 1. Data Link Layer

### Definition

The Data Link Layer transfers data between two directly connected devices. Its protocol data unit is called a **frame**.

```text
Application Layer  -> Data
Transport Layer    -> Segment
Network Layer      -> Packet
Data Link Layer    -> Frame
Physical Layer     -> Bits
```

### Main Responsibilities

| Responsibility | Purpose |
|---|---|
| **Framing** | Divides a bit stream into identifiable units called frames |
| **Physical addressing** | Adds source and destination MAC addresses |
| **Flow control** | Prevents a fast sender from overwhelming a slow receiver |
| **Error control** | Detects and sometimes corrects damaged or lost frames |
| **Access control** | Decides which device may use a shared medium |
| **Reliable delivery** | Uses acknowledgements and retransmission when required |
| **Link management** | Establishes, maintains, and terminates a data-link connection where applicable |

### Sublayers

The Data Link Layer is commonly divided into two sublayers:

| Sublayer | Main function | Examples |
|---|---|---|
| **LLC (Logical Link Control)** | Flow control, error control, and protocol identification | 802.2 LLC |
| **MAC (Media Access Control)** | MAC addressing and access to the transmission medium | Ethernet, Wi-Fi MAC |

### Layer 2 Does Not Usually Provide End-to-End Delivery

Data Link Layer delivery is normally **hop-to-hop** or **node-to-node**. A router removes the incoming Layer 2 frame and creates a new Layer 2 frame for the next link. End-to-end reliability, when needed, is normally handled by the Transport Layer.

---

## 2. Framing

### Definition

**Framing** is the process of dividing a continuous stream of bits into manageable blocks called frames and adding control information around the data.

```text
Bit stream: 101101001011010010...

Frames:     | Header | Data | Trailer | Header | Data | Trailer |
```

### Why Framing Is Needed

- Identifies the beginning and end of each message.
- Allows the receiver to process data one frame at a time.
- Carries source and destination MAC addresses.
- Provides a place for error-detection information such as FCS.
- Supports flow control and reliable retransmission.
- Prevents a receiver from treating the entire bit stream as one message.

### General Frame Format

```text
+--------+---------+----------------+---------+---------+
| Header | Address | Payload/Data  | Trailer | End     |
+--------+---------+----------------+---------+---------+
```

A typical Ethernet frame includes:

| Field | Purpose |
|---|---|
| **Preamble** | Synchronizes the sender and receiver clocks |
| **Start Frame Delimiter** | Marks the actual start of the frame |
| **Destination MAC** | Identifies the receiving interface |
| **Source MAC** | Identifies the sending interface |
| **Type/Length** | Identifies the upper-layer protocol or payload length |
| **Data and padding** | Carries the network-layer packet; padding may meet minimum size |
| **FCS** | CRC-based error-detection value |

### Framing Methods

#### 1. Character Count

A length field tells the receiver how many characters or bytes belong to the frame.

```text
[Length = 5][HELLO]
```

**Problem:** If the length field is corrupted, the receiver can lose frame synchronization.

#### 2. Byte Stuffing

Special bytes mark the start and end of a frame. If the same special byte appears inside the data, an escape byte is inserted before it.

```text
FLAG | data | ESC FLAG | data | FLAG
```

The receiver removes the inserted escape byte after identifying the frame.

#### 3. Bit Stuffing

A special bit pattern marks the frame boundary. In HDLC-like protocols, after five consecutive `1` bits in data, the sender inserts a `0`. The receiver removes that stuffed `0`.

```text
Data:       01111110
Stuffed:    011111010
```

This prevents data from accidentally looking like a boundary flag.

#### 4. Physical-Layer Coding Violations

Some physical encodings contain signal patterns that are not used for normal data. These unused patterns can mark frame boundaries.

### Frame Loss, Duplication, and Damage

- **Lost frame:** The frame never arrives.
- **Damaged frame:** The frame arrives but its bits changed.
- **Duplicate frame:** A retransmitted frame arrives after the original acknowledgement was delayed.
- **Out-of-order frame:** Frames arrive in an order different from the sending order.

Sequence numbers allow the receiver to detect duplicates and identify missing or out-of-order frames.

---

## 3. Flow Control and Error Control

### Flow Control

Flow control regulates the sender's transmission rate so that the receiver's buffers do not overflow.

```text
Fast sender  --->  Receiver with limited buffer
        Flow control prevents overflow
```

Examples include Stop-and-Wait flow control and sliding-window flow control.

### Error Control

Error control makes communication more dependable by:

1. Detecting errors with parity, checksum, or CRC.
2. Discarding damaged frames when necessary.
3. Informing the sender through ACK or NAK.
4. Retransmitting lost or damaged frames.
5. Using sequence numbers to prevent duplicate delivery.

### Common Error-Detection Terms

| Term | Meaning |
|---|---|
| **ACK** | Positive acknowledgement: frame received successfully |
| **NAK/NACK** | Negative acknowledgement: frame should be sent again |
| **Timeout** | Sender waited too long for a response |
| **Retransmission** | Sending a frame again |
| **CRC/FCS** | Error-detection value calculated from frame bits |
| **RTT** | Round-trip time from sender to receiver and back |

---

## 4. Automatic Repeat reQuest (ARQ)

**ARQ** is a reliability technique in which the receiver acknowledges frames and the sender retransmits frames that are lost or corrupted.

### Basic ARQ Components

- **Data frames:** Carry the user's data.
- **Sequence numbers:** Identify frames and detect duplicates.
- **ACKs:** Confirm successful reception.
- **NAKs:** Request retransmission, when the protocol uses them.
- **Timers:** Detect a missing frame or acknowledgement.
- **Retransmission:** Resends a frame after NAK or timeout.

### Generic ARQ Process

```text
Sender                          Receiver
  |                               |
  |--------- Data frame --------->|
  |                               | Check CRC
  |<------------ ACK -------------|
  |                               |
```

If the frame or ACK is lost:

```text
Sender                          Receiver
  |--------- Data frame --------->|
  |                               |
  |       ACK is lost/delayed     |
  |                               |
  |---- Retransmit after timeout ->|
  |<------------ ACK -------------|
```

### Why Sequence Numbers Are Necessary

If an ACK is lost, the sender may retransmit a frame that the receiver already accepted. The receiver uses the sequence number to recognize the duplicate, discard its data, and send the appropriate ACK again.

---

## 5. Stop-and-Wait ARQ

### Definition

Stop-and-Wait ARQ sends **one frame at a time** and waits for its acknowledgement before sending the next frame.

### Operation

1. Sender sends frame 0 and starts a timer.
2. Receiver checks the frame using FCS/CRC.
3. If correct, receiver delivers the data and sends ACK.
4. Sender receives ACK, stops the timer, and sends the next frame.
5. If the frame is damaged, lost, or the timer expires, sender retransmits it.
6. Alternating sequence numbers, usually 0 and 1, identify new frames and duplicates.

```text
Sender                          Receiver
  |--- Frame 0 ----------------->|
  |<--- ACK 1 -------------------|  Ready for frame 1
  |--- Frame 1 ----------------->|
  |<--- ACK 0 -------------------|  Ready for frame 0
```

The ACK number often identifies the **next frame expected**, so ACK 1 can mean frame 0 was received and frame 1 is expected.

### Lost or Damaged Frame

```text
Sender                          Receiver
  |--- Frame 0 ----------------->|  Damaged; discard
  |<--- NAK 0 -------------------|
  |--- Frame 0 again ------------>|
```

If NAK is not used, the receiver stays silent and the sender retransmits after timeout.

### Lost ACK and Duplicate Handling

```text
Sender                          Receiver
  |--- Frame 0 ----------------->|  Accepts frame 0
  |<--- ACK 1 ----X               |  ACK lost
  |                              |
  |--- Frame 0 again ----------->|  Duplicate; discard data
  |<--- ACK 1 -------------------|  ACK sent again
```

### Advantages

- Very simple to implement.
- Requires little receiver memory.
- Easy to understand and debug.
- Suitable for links with large bandwidth-delay product only when traffic is light or simplicity is more important.

### Disadvantages

- Poor link utilization when propagation delay is high.
- Only one frame can be outstanding.
- A lost frame stops all further transmission until recovery.
- Low throughput on long-distance or high-speed links.

### Stop-and-Wait Efficiency

Ignoring processing time and ACK transmission time:

$$
\text{Utilization} \approx \frac{T_f}{T_f + 2T_p}
$$

where:

- $T_f$ = frame transmission time
- $T_p$ = one-way propagation delay

With negligible errors, the sender is busy only while transmitting the frame. The rest of the time is spent waiting.

---

## 6. Go-Back-N ARQ

### Definition

**Go-Back-N ARQ** is a sliding-window protocol that allows the sender to transmit several frames without waiting for individual ACKs. If a frame is lost or damaged, the sender retransmits that frame and all later frames already sent in the affected window.

### Main Rules

- Sender may send up to **N outstanding frames**.
- Receiver normally accepts only the next frame in order.
- Receiver discards out-of-order frames.
- ACKs are usually cumulative.
- One timer is commonly maintained for the oldest unacknowledged frame.
- On timeout, sender retransmits from the missing frame onward.

### Example

Suppose frames 0 through 4 are sent, but frame 2 is lost:

```text
Sender                          Receiver
  |--- Frame 0 ----------------->| Accept
  |--- Frame 1 ----------------->| Accept
  |--- Frame 2 ----X              | Lost
  |--- Frame 3 ----------------->| Discard; waiting for 2
  |--- Frame 4 ----------------->| Discard; waiting for 2
  |<--- ACK 2 -------------------| Next expected frame is 2
  |--- Frame 2 ----------------->| Retransmit
  |--- Frame 3 ----------------->| Retransmit
  |--- Frame 4 ----------------->| Retransmit
```

The receiver may repeatedly send ACK 2, called a **duplicate ACK**, to show that frame 2 is still expected.

### Cumulative Acknowledgement

An ACK confirms receipt of all frames before the ACK number. For example, ACK 5 generally means frames 0 through 4 were received in order and frame 5 is expected next.

### Window Sizes

If sequence numbers use $m$ bits, the sequence-number space contains $2^m$ values. For Go-Back-N:

$$
1 \leq W_s \leq 2^m - 1
$$

where $W_s$ is the sender window size. The receiver window is effectively 1 because it accepts only the next in-order frame.

### Advantages

- Better utilization than Stop-and-Wait.
- Simple receiver design.
- Cumulative ACKs reduce acknowledgement traffic.
- Effective when errors are rare and frames are small.

### Disadvantages

- Retransmits correctly received frames after one missing frame.
- Wastes bandwidth when the error rate is high.
- Receiver cannot normally buffer out-of-order frames.
- A single damaged frame can cause many retransmissions.

---

## 7. Selective Repeat ARQ

### Definition

**Selective Repeat ARQ** is a sliding-window protocol in which the receiver accepts and buffers correct out-of-order frames. Only missing or damaged frames are retransmitted.

### Main Rules

- Sender may transmit multiple frames without waiting.
- Receiver checks every frame independently.
- Correct out-of-order frames are buffered.
- Receiver acknowledges frames individually or selectively.
- Each outstanding frame generally has its own timer.
- Only the frame whose timer expires, or whose NAK is received, is retransmitted.

### Example

Suppose frames 0 through 4 are sent, but frame 2 is lost:

```text
Sender                          Receiver
  |--- Frame 0 ----------------->| Accept and deliver
  |--- Frame 1 ----------------->| Accept and deliver
  |--- Frame 2 ----X              | Missing
  |--- Frame 3 ----------------->| Buffer
  |--- Frame 4 ----------------->| Buffer
  |<--- ACK 0 -------------------|
  |<--- ACK 1 -------------------|
  |<--- NAK 2 -------------------| or timeout for frame 2
  |--- Frame 2 ----------------->| Accept
  |                               | Deliver 2, 3, 4 in order
```

### Window-Size Rule

If sequence numbers use $m$ bits, Selective Repeat requires:

$$
W_s \leq 2^{m-1}
$$

and normally:

$$
W_r \leq 2^{m-1}
$$

This prevents an old frame from being confused with a new frame after sequence numbers wrap around.

### Advantages

- Efficient when errors are frequent.
- Retransmits only lost or damaged frames.
- Uses bandwidth better than Go-Back-N on noisy links.
- Receiver can continue accepting frames after a gap.

### Disadvantages

- More complex sender and receiver.
- Requires receiver buffers for out-of-order frames.
- Requires more timers or timer management.
- ACKs and sequence-number processing are more complicated.
- Needs a smaller maximum window relative to the sequence-number space.

---

## 8. ARQ Protocol Comparison

| Feature | Stop-and-Wait ARQ | Go-Back-N ARQ | Selective Repeat ARQ |
|---|---|---|---|
| Frames in transit | One | Multiple | Multiple |
| Sender window | 1 | N | N |
| Receiver window | 1 | 1 | Multiple |
| Out-of-order frames | Not applicable | Discarded | Buffered |
| ACK style | Individual | Usually cumulative | Individual/selective |
| Retransmission | One frame | Missing frame and all following frames | Only missing/damaged frames |
| Timers | One | Usually one for oldest frame | Usually one per outstanding frame |
| Complexity | Low | Medium | High |
| Receiver memory | Very low | Low | Higher |
| Noisy-link efficiency | Low | Medium to low | High |
| Best use | Simple or low-speed links | Low-error links | High-delay or noisy links |

### Retransmission Example

If frames 0, 1, 2, 3, and 4 are sent and frame 2 is lost:

| Protocol | Retransmitted frames |
|---|---|
| Stop-and-Wait | Frame 2, after earlier frames were individually completed |
| Go-Back-N | Frames 2, 3, and 4 |
| Selective Repeat | Frame 2 only |

---

## 9. Important Formulas

### Sequence-Number Space

For $m$ sequence-number bits:

$$
\text{Number of sequence numbers} = 2^m
$$

### Go-Back-N Maximum Window

$$
W_s \leq 2^m - 1
$$

### Selective Repeat Maximum Window

$$
W_s, W_r \leq 2^{m-1}
$$

### Stop-and-Wait Utilization

$$
U \approx \frac{T_f}{T_f + 2T_p}
$$

### Bandwidth-Delay Product

$$
\text{BDP} = \text{Bandwidth} \times \text{RTT}
$$

A sliding window should be large enough to keep the link busy. If the window is too small compared with the bandwidth-delay product, the sender must wait and link utilization falls.

---

## 10. Practical Examples

### Example 1: Choosing a Protocol

A satellite link has a large propagation delay and occasional errors. Stop-and-Wait wastes much of the available capacity. Selective Repeat is usually preferable because it keeps multiple frames in flight and retransmits only damaged frames.

### Example 2: Identifying Go-Back-N

A receiver discards frames 6 and 7 because frame 5 is missing. The sender then retransmits frames 5, 6, and 7. This is **Go-Back-N ARQ**.

### Example 3: Identifying Selective Repeat

A receiver stores frames 6 and 7 while waiting for missing frame 5. The sender retransmits only frame 5. This is **Selective Repeat ARQ**.

### Example 4: Sequence Bits

If a protocol uses 3 sequence-number bits:

- Sequence-number space = $2^3 = 8$.
- Maximum Go-Back-N sender window = $8 - 1 = 7$.
- Maximum Selective Repeat window = $2^{3-1} = 4$.

---

## 11. Important Definitions

- **Frame:** Data-link-layer protocol data unit.
- **Framing:** Dividing a bit stream into identifiable frames.
- **Flow control:** Regulation of transmission speed to protect the receiver.
- **Error control:** Detection and recovery from damaged or lost data.
- **ARQ:** Error-control method using acknowledgements and retransmission.
- **Sliding window:** A range of sequence numbers that may be sent or accepted.
- **ACK:** Positive acknowledgement.
- **NAK:** Negative acknowledgement requesting retransmission.
- **Cumulative ACK:** One acknowledgement confirming several consecutive frames.
- **Timeout:** Expiration of a timer while waiting for an expected response.
- **Piggybacking:** Attaching an ACK to an outgoing data frame instead of sending a separate ACK frame.
- **Sequence-number wraparound:** Reusing sequence numbers after the end of the sequence-number space.
- **FCS:** Frame Check Sequence used for error detection.

---

## 12. Common Exam Questions and Answers

### Q1. What is the main function of the Data Link Layer?

It provides reliable node-to-node delivery by framing data, using physical addresses, controlling flow, detecting errors, and controlling access to the medium.

### Q2. What is the difference between flow control and error control?

Flow control prevents receiver-buffer overflow. Error control detects damaged or lost frames and recovers through acknowledgements and retransmissions.

### Q3. Why is framing necessary?

Framing identifies boundaries in a continuous bit stream and carries addressing, control, and error-detection information.

### Q4. What happens in Stop-and-Wait ARQ when an ACK is lost?

The sender's timer expires and it retransmits the frame. The receiver uses the sequence number to detect the duplicate and sends the ACK again.

### Q5. Which ARQ protocol retransmits all frames after a missing frame?

Go-Back-N ARQ.

### Q6. Which ARQ protocol buffers out-of-order frames?

Selective Repeat ARQ.

### Q7. Which protocol is simplest?

Stop-and-Wait ARQ.

### Q8. Which protocol uses bandwidth most efficiently on a noisy link?

Selective Repeat ARQ, because it retransmits only the missing or damaged frames.

### Q9. Why must Selective Repeat use a smaller window?

A smaller window prevents an old frame and a new frame with the same wrapped sequence number from being confused.

### Q10. What is piggybacking?

Piggybacking combines an acknowledgement with a data frame traveling in the opposite direction, reducing overhead.

---

## Summary / Cheat Sheet

- Data Link Layer = **OSI Layer 2**.
- Its protocol data unit is the **frame**.
- Main responsibilities: **framing, physical addressing, flow control, error control, access control, and link management**.
- Framing methods include **character count, byte stuffing, bit stuffing, and physical-layer coding violations**.
- ARQ uses **sequence numbers, ACKs, timers, and retransmissions**.
- **Stop-and-Wait:** one frame at a time; simplest but slow.
- **Go-Back-N:** multiple frames; discards out-of-order frames and retransmits from the missing frame onward.
- **Selective Repeat:** buffers out-of-order frames and retransmits only missing or damaged frames.
- Go-Back-N with $m$ bits: maximum sender window is $2^m - 1$.
- Selective Repeat with $m$ bits: maximum window is $2^{m-1}$.
- High delay and noisy links generally favor **Selective Repeat**.

---

## Quick Revision Notes

```text
Framing       = bits to frames
Flow control  = protect receiver
Error control = detect and recover
ACK           = received correctly
NAK           = send again
Timeout       = response did not arrive in time

Stop-and-Wait = send 1, wait
Go-Back-N     = send many, repeat from error
Selective     = send many, repeat only error
```

---

## Tomorrow's Preview

Day 16 can cover **Data Link Layer error detection and correction**, including parity checks, checksums, CRC, Hamming distance, and Hamming code.
