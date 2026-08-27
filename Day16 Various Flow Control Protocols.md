# Day 16: Various Flow Control Protocols

## Overview

**Flow control** is the technique used to match the sender's transmission rate with the receiver's ability to accept and process data. It prevents a fast sender from overflowing the receiver's buffer.

Flow control is different from congestion control:

- **Flow control** protects the receiver.
- **Congestion control** protects the network from overload.
- **Error control** detects lost or damaged data and arranges retransmission.

This lesson covers Stop-and-Wait, sliding-window protocols, Go-Back-N, Selective Repeat, and TCP's receive-window mechanism.

---

## Topics Covered

1. Meaning and need for flow control
2. Sender and receiver windows
3. Stop-and-Wait flow control
4. Sliding-window flow control
5. Go-Back-N ARQ
6. Selective Repeat ARQ
7. Piggybacking and acknowledgements
8. TCP flow control and the advertised window
9. Window-size and efficiency formulas
10. Protocol comparison and exam questions

---

## Learning Objectives

After studying this note, you should be able to:

- Explain why flow control is required.
- Distinguish flow control, error control, and congestion control.
- Describe Stop-and-Wait and sliding-window operation.
- Compare Go-Back-N and Selective Repeat.
- Explain sender windows, receiver windows, ACKs, timers, and sequence numbers.
- Calculate bandwidth-delay product, window size, and link utilization.
- Explain how TCP prevents receiver-buffer overflow.

---

## 1. Meaning of Flow Control

### Definition

**Flow control** regulates the amount and rate of data transmitted so that the sender does not send more data than the receiver can store or process.

```text
Fast sender                         Slow receiver
    |                                      |
    |------ too much data too quickly ---->| Buffer overflow
    |                                      |
    |<--------- flow-control feedback -----|
```

Without flow control, a receiver may have to discard frames or segments when its buffer becomes full.

### Basic Responsibilities

| Component | Responsibility |
|---|---|
| **Sender** | Keeps track of data sent but not yet acknowledged |
| **Receiver** | Reports how much more data it can accept |
| **Buffer** | Temporarily stores data waiting for processing |
| **Acknowledgement** | Confirms received data or communicates the next expected data |
| **Window** | Limits the amount of unacknowledged data in transit |
| **Timer** | Detects a missing acknowledgement in reliable protocols |

### Flow Control vs Error Control vs Congestion Control

| Feature | Flow control | Error control | Congestion control |
|---|---|---|---|
| Protects | Receiver | Data correctness | The network |
| Main problem | Receiver is slower or has limited buffer | Frame/segment is lost or damaged | Too much traffic in the network |
| Common tools | Stop-and-Wait, receive window | ACK, NAK, CRC, retransmission | Slow start, congestion avoidance |
| Main feedback | Available receiver space | Delivery status | Network loss, delay, or congestion |
| Typical layers | Data Link and Transport | Data Link and Transport | Mainly Transport and Network interaction |

---

## 2. Important Terms

### Frame, Segment, and Packet

- A **frame** is the Data Link Layer protocol data unit.
- A **packet** is the Network Layer protocol data unit.
- A **segment** is commonly the Transport Layer protocol data unit for TCP.

The same flow-control idea can be applied at different layers, even though the unit being transmitted has a different name.

### Acknowledgement (ACK)

An ACK tells the sender that data was received successfully. Depending on the protocol, an ACK may:

- Confirm one particular frame.
- Confirm all frames up to a point.
- Identify the next frame expected.
- Also carry receiver-window information.

### Negative Acknowledgement (NAK)

A NAK tells the sender that a frame was missing or damaged and should be retransmitted. Some protocols do not use NAKs; they rely on duplicate ACKs and timeouts.

### Sequence Number

A sequence number identifies a frame or byte range. It helps the receiver:

- Detect duplicate data after a timeout.
- Detect missing or out-of-order data.
- Place data in the correct order.
- Match ACKs with transmitted data.

### Window

A window is the range of sequence numbers that may be sent or accepted at a particular time.

```text
Sequence numbers:  0  1  2  3  4  5  6  7
                   [------- sender window -------]
```

As ACKs arrive, the window **slides** forward, allowing new frames to be sent.

---

## 3. Stop-and-Wait Flow Control

### Definition

Stop-and-Wait sends one frame and waits for its ACK before sending the next frame.

```text
Sender                          Receiver
  |                               |
  |---------- Frame 0 ----------->|
  |<------------- ACK ------------|
  |---------- Frame 1 ----------->|
  |<------------- ACK ------------|
```

### Operation

1. The sender transmits one frame.
2. The sender stops and waits.
3. The receiver accepts the frame and sends an ACK.
4. The sender sends the next frame.
5. If the ACK does not arrive before the timer expires, the sender retransmits.

### Advantages

- Very simple to implement.
- Requires a small amount of memory.
- Naturally prevents receiver-buffer overflow.
- Works well on short, low-speed links.

### Disadvantages

- The link remains idle while the sender waits.
- Poor performance when propagation delay is large.
- Only one frame can be outstanding.
- Throughput is limited on high-bandwidth links.

### Stop-and-Wait ARQ

Stop-and-Wait becomes an error-control protocol when it uses ACKs, timers, and retransmission.

Alternating sequence numbers, usually `0` and `1`, distinguish a new frame from a duplicate:

```text
Frame 0 -> ACK 1 -> Frame 1 -> ACK 0 -> Frame 0 ...
```

In many descriptions, ACK 1 means “frame 0 was received; frame 1 is expected.” Always check whether the protocol's ACK numbering means the last frame received or the next frame expected.

---

## 4. Sliding-Window Flow Control

### Definition

A **sliding-window protocol** allows several frames to be sent before an ACK is received. The sender and receiver maintain windows of permitted sequence numbers.

```text
Before ACKs:
Sequence numbers:  0  1  2  3  4  5  6  7
                   [  0  1  2  3  ]
                    sent and unacknowledged

After ACK for 0 and 1:
Sequence numbers:  0  1  2  3  4  5  6  7
                         [  2  3  4  5  ]
                          window slides forward
```

### Sender Window

The sender window contains:

- Frames already sent but not acknowledged.
- Frames that may be sent immediately.
- Frames outside the window that must wait.

### Receiver Window

The receiver window contains the sequence numbers that the receiver is currently prepared to accept.

A receiver can reduce its advertised window when its buffer becomes full. A sender must not exceed the permitted window.

### Advantages

- Keeps the link busy during propagation delay.
- Gives higher throughput than Stop-and-Wait.
- Allows control over receiver-buffer usage.
- Supports reliable transmission with cumulative or selective ACKs.

### Disadvantages

- Requires more memory and sequence-number management.
- Needs timers and more complex ACK handling.
- Sequence-number wraparound must be handled correctly.

---

## 5. Go-Back-N ARQ

### Definition

**Go-Back-N ARQ** is a sliding-window protocol in which the sender may transmit multiple frames, but the receiver normally accepts only the next frame in order. If one frame is lost or damaged, the sender retransmits that frame and all later outstanding frames.

### Example

Suppose frames 0, 1, 2, and 3 are sent, but frame 2 is lost:

```text
Sender                          Receiver
  |---------- Frame 0 ----------->| accepted
  |---------- Frame 1 ----------->| accepted
  |---------- Frame 2 -X          | lost
  |---------- Frame 3 ----------->| discarded; 2 expected
  |<--------- ACK 2 --------------| cumulative ACK
  |------ retransmit 2 ---------->| accepted
  |------ retransmit 3 ---------->| accepted
```

The ACK may be cumulative. ACK 2 commonly means that all data before 2 was received and frame 2 is the next expected frame.

### Main Characteristics

- Receiver window is usually `1`.
- Sender can have several unacknowledged frames.
- Receiver discards out-of-order frames.
- One timer may be used for the oldest unacknowledged frame.
- Retransmits the missing frame and every later frame in the outstanding window.

### Advantages

- Simpler receiver design than Selective Repeat.
- Cumulative ACKs reduce acknowledgement overhead.
- Efficient when errors are uncommon and the window is moderate.

### Disadvantages

- Many correctly received frames may be retransmitted.
- Inefficient on noisy links or links with long delay.
- Receiver needs enough processing capacity to discard duplicates and out-of-order frames.

---

## 6. Selective Repeat ARQ

### Definition

**Selective Repeat ARQ** allows the receiver to accept and buffer correct out-of-order frames. When a loss occurs, only the missing or damaged frame is retransmitted.

### Example

```text
Sender                          Receiver
  |---------- Frame 0 ----------->| accepted
  |---------- Frame 1 ----------->| accepted
  |---------- Frame 2 -X          | lost
  |---------- Frame 3 ----------->| buffered
  |<--------- ACK 3 --------------| frame 3 received
  |<--------- NAK 2 --------------| frame 2 missing
  |------ retransmit 2 ---------->| accepted
  |                               | delivers 2 and buffered 3
```

### Main Characteristics

- Receiver window is greater than `1`.
- Receiver stores valid out-of-order frames.
- Each frame may require its own timer.
- ACKs are usually selective rather than purely cumulative.
- Only missing or damaged frames are retransmitted.

### Advantages

- Avoids unnecessary retransmissions.
- Performs well on long-delay or error-prone links.
- Uses bandwidth efficiently.

### Disadvantages

- More complex sender and receiver.
- Requires more buffer memory.
- Requires careful sequence-number rules to avoid confusing old and new frames.

### Sequence-Number Rule

If the sequence-number field has `m` bits, the sequence-number space contains $2^m$ values.

For Selective Repeat, the window size must satisfy:

$$W \leq 2^{m-1}$$

This prevents an old delayed frame from being confused with a new frame after sequence numbers wrap around.

---

## 7. Go-Back-N vs Selective Repeat

| Feature | Go-Back-N | Selective Repeat |
|---|---|---|
| Receiver accepts out-of-order frames | Usually no | Yes, and buffers them |
| Receiver window | Usually 1 | Greater than 1 |
| ACK style | Usually cumulative | Selective or individual |
| Retransmission | Lost frame and following frames | Only lost/damaged frames |
| Timer design | Often one timer | Often one timer per outstanding frame |
| Receiver complexity | Lower | Higher |
| Buffer requirement | Lower | Higher |
| Performance on noisy links | Lower | Higher |
| Sequence-number limit | $W \leq 2^m - 1$ for common GBN design | $W \leq 2^{m-1}$ |

### Example of Retransmission Cost

If frames 0 through 4 are outstanding and frame 2 is lost:

- **Go-Back-N:** retransmits 2, 3, and 4.
- **Selective Repeat:** retransmits only 2.

Selective Repeat saves bandwidth, but it spends more memory and processing power.

---

## 8. Piggybacking

**Piggybacking** places an ACK for received data inside an outgoing data frame instead of sending a separate ACK frame.

```text
Without piggybacking:  Data --->
                      <--- ACK

With piggybacking:    Data --->
                      <--- Data + ACK
```

### Benefits

- Reduces the number of frames.
- Saves bandwidth and processing overhead.
- Is useful for two-way communication.

### Limitation

The receiver should not delay an ACK indefinitely just to find outgoing data. If no data is ready within a suitable time, it sends a separate ACK.

---

## 9. TCP Flow Control

TCP provides byte-oriented, end-to-end flow control between applications.

### Receive Window (`rwnd`)

The receiver advertises a **receive window**, often called `rwnd`, in the TCP header. It tells the sender how many more bytes the receiver can accept without overflowing its receive buffer.

```text
LastByteSent - LastByteAcknowledged < Advertised receive window
```

The sender's usable sending limit is commonly described as:

$$Effective\ Send\ Window = min(rwnd, cwnd)$$

where:

- `rwnd` is the receiver's advertised window.
- `cwnd` is the congestion window maintained by the sender.

The `rwnd` part provides flow control; the `cwnd` part provides congestion control.

### TCP Window Operation

1. The receiver places available buffer space in the Window field of its ACK.
2. The sender limits unacknowledged bytes to the advertised value.
3. As the application reads data, buffer space becomes available.
4. The receiver advertises a larger window in later ACKs.
5. If the receiver has no available space, it may advertise a zero window.

### Zero-Window Condition

A zero window means that the receiver cannot accept more data temporarily. The sender pauses normal transmission and uses a **zero-window probe** after a suitable interval to discover whether the window has opened again.

### TCP Window Scaling

The original TCP window field is 16 bits. The **window-scale option**, negotiated during connection setup, allows much larger effective windows on high-bandwidth, high-delay paths.

### TCP Flow Control Is Byte-Based

Data-link sliding windows often count frames. TCP sequence numbers count bytes. A TCP ACK usually identifies the next byte expected and is therefore cumulative.

---

## 10. Important Formulas

### Bandwidth-Delay Product

The bandwidth-delay product estimates how much data can be in transit on a link:

$$BDP = Bandwidth \times RTT$$

Use consistent units. For example, convert bandwidth to bits per second and RTT to seconds.

### Stop-and-Wait Utilization

Ignoring ACK transmission time and processing delay:

$$Utilization = \frac{T_f}{T_f + 2T_p}$$

where:

- $T_f$ is frame transmission time.
- $T_p$ is one-way propagation delay.

A common equivalent form is:

$$Utilization = \frac{1}{1 + 2a}, \quad a = \frac{T_p}{T_f}$$

### Sliding-Window Utilization

A simplified utilization estimate is:

$$Utilization = min\left(1, \frac{W}{1 + 2a}\right)$$

where $W$ is the sender-window size. A window large enough to keep the link continuously busy satisfies approximately:

$$W \geq 1 + 2a$$

### Throughput

A basic throughput estimate is:

$$Throughput = Utilization \times Link\ Rate$$

Actual throughput can be lower because of headers, ACKs, processing time, loss, and congestion.

### Example

A link has a frame transmission time of `1 ms` and a one-way propagation delay of `10 ms`.

For Stop-and-Wait:

$$Utilization = \frac{1}{1 + 2(10/1)} = \frac{1}{21} \approx 4.76\%$$

A sliding window of at least approximately `21` frames is needed to keep this idealized link busy.

---

## 11. Protocol Comparison Cheat Sheet

| Protocol | Frames in flight | Receiver behavior | Retransmission | Complexity | Best suited for |
|---|---:|---|---|---|---|
| Stop-and-Wait | 1 | Accepts one frame, then waits | One frame | Low | Short, simple links |
| Go-Back-N | Several | Usually accepts only in-order data | Missing frame and later frames | Medium | Reliable links with occasional errors |
| Selective Repeat | Several | Buffers valid out-of-order data | Only missing/damaged frames | High | Long-delay or noisy links |
| TCP receive window | Many bytes | Advertises available buffer space | Lost byte ranges/segments are recovered | High | End-to-end Internet communication |

### Memory Trick

- **Stop-and-Wait:** send one, wait.
- **Go-Back-N:** one loss makes the sender go back and resend later frames.
- **Selective Repeat:** repeat only the selected missing frames.
- **TCP `rwnd`:** receiver tells sender how many bytes it can hold.

---

## 12. Common Misconceptions

- **Flow control is not the same as error control.** A receiver may have space but still receive a corrupted frame.
- **Flow control is not congestion control.** A receiver may be ready while routers on the path are congested.
- **A larger window does not always improve performance.** It can increase memory use and may worsen congestion if congestion control is ignored.
- **Go-Back-N does not normally buffer every out-of-order frame.** Selective Repeat does.
- **TCP is not Stop-and-Wait.** TCP uses a large sliding window and cumulative byte ACKs.
- **An ACK does not always mean only one frame was received.** It may acknowledge a cumulative range.
- **A timeout does not prove that data was lost.** The data or its ACK may simply be delayed.

---

## 13. Practical Observation Commands

These commands help observe TCP windows and retransmissions on a local system. They do not manually configure the flow-control algorithms.

### Windows PowerShell

1. `Get-NetTCPConnection` - Displays active TCP connections and their states.
2. `Get-NetTCPSetting` - Displays available TCP configuration settings.
3. `netstat -ano` - Shows TCP connections, listening ports, and process IDs.
4. `Test-NetConnection example.com -Port 443` - Tests TCP connectivity to port 443.

### Packet Capture

Wireshark filters can show TCP flow-control behavior:

- `tcp.analysis.window_full` - Sender has filled the receiver window.
- `tcp.analysis.zero_window` - Receiver advertises a zero window.
- `tcp.analysis.retransmission` - Possible TCP retransmission.
- `tcp.window_size` - Advertised TCP window value.

Interpret captures carefully: scaling, delayed ACKs, loss, and congestion can affect what is observed.

---

## 14. Example Questions and Answers

### Q1. What is the main purpose of flow control?

**Answer:** To prevent a fast sender from overwhelming a slower receiver or filling the receiver's buffer.

### Q2. What is the main weakness of Stop-and-Wait?

**Answer:** The sender remains idle while waiting for each ACK, so utilization is poor when propagation delay is high.

### Q3. What happens in Go-Back-N when frame 2 is lost?

**Answer:** The receiver rejects or discards later out-of-order frames, and the sender retransmits frame 2 and the following outstanding frames.

### Q4. Why is Selective Repeat more efficient than Go-Back-N on a noisy link?

**Answer:** It retransmits only the missing or damaged frames instead of retransmitting correctly received later frames.

### Q5. Which protocol needs more receiver memory: Go-Back-N or Selective Repeat?

**Answer:** Selective Repeat, because it buffers correctly received out-of-order frames.

### Q6. What is the difference between `rwnd` and `cwnd` in TCP?

**Answer:** `rwnd` limits data according to receiver capacity; `cwnd` limits data according to network capacity. The sender uses the smaller limit.

### Q7. What does a TCP zero window indicate?

**Answer:** The receiver's buffer has no available space, so the sender must temporarily stop normal transmission.

### Q8. If a protocol uses 3-bit sequence numbers, what is the maximum common Selective Repeat window?

**Answer:** The sequence-number space is $2^3 = 8$. The maximum Selective Repeat window is $2^{3-1} = 4$.

### Q9. Why are sequence numbers needed when ACKs are used?

**Answer:** They identify new data, detect duplicates, preserve ordering, and prevent a delayed or repeated frame from being delivered twice.

---

## Quick Revision Notes

- Flow control protects the receiver's buffer.
- Stop-and-Wait permits one outstanding frame.
- Sliding-window protocols permit multiple outstanding frames.
- Go-Back-N retransmits from the missing frame onward.
- Selective Repeat retransmits only missing or damaged frames.
- Piggybacking combines an ACK with an outgoing data frame.
- TCP advertises available receive-buffer space using `rwnd`.
- TCP's effective window is limited by `min(rwnd, cwnd)`.
- A larger window is needed when bandwidth-delay product is large.
- For `m` sequence bits, common Selective Repeat requires $W \leq 2^{m-1}$.

---

## Summary

Flow control keeps transmission within the receiver's processing and buffer capacity. Stop-and-Wait is simple but wastes link capacity on long-delay paths. Sliding-window protocols improve utilization by allowing multiple frames in transit. Go-Back-N is simpler but may retransmit correctly received frames, while Selective Repeat is more efficient but requires more memory and logic. TCP applies a byte-based sliding window and combines receiver flow control with separate congestion control.

---

## Tomorrow's Preview

- Medium access control
- Random access and controlled access
- CSMA/CD and CSMA/CA
- Collision handling in shared networks
