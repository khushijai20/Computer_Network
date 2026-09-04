# 🌐 Day 21: Pure ALOHA and Slotted ALOHA

## 📝 Overview

ALOHA is a **random access protocol** used by multiple stations that share one communication channel. A station sends a frame when it has data. If two or more frames overlap, a collision occurs and the affected frames must be retransmitted.

ALOHA is important because it introduced a simple, decentralized way for stations to share a common medium. It is also the foundation for understanding later protocols such as CSMA, CSMA/CD, and CSMA/CA.

There are two main versions:

1. **Pure ALOHA**: a station can transmit at any time.
2. **Slotted ALOHA**: a station can transmit only at the beginning of a synchronized time slot.

Slotted ALOHA reduces the time during which a frame is vulnerable to collision and therefore has twice the maximum theoretical throughput of Pure ALOHA.

---

## 📚 Topics Covered

- Random access and contention-based access
- ALOHA terminology
- Pure ALOHA operation
- Slotted ALOHA operation
- Vulnerable period
- Throughput and offered load
- Collision and success probability
- Retransmission and backoff
- Pure ALOHA versus Slotted ALOHA
- Advantages, disadvantages, and applications
- Numerical problems and exam questions

---

## 🎯 Learning Objectives

By the end of this note, you should be able to:

- define Pure ALOHA and Slotted ALOHA
- explain how collisions occur in both protocols
- identify the vulnerable period of each protocol
- calculate throughput using the ALOHA equations
- find the offered load that gives maximum throughput
- explain why Slotted ALOHA is more efficient than Pure ALOHA
- compare both protocols by synchronization, delay, and efficiency
- solve basic probability and numerical questions

---

## 🧠 ALOHA Cheat Sheet

| Feature | Pure ALOHA | Slotted ALOHA |
|---|---|---|
| Transmission rule | Transmit whenever a frame is ready | Transmit only at a slot boundary |
| Time synchronization | Not required | Required |
| Frame alignment | Frames may start at any time | Frames start at slot boundaries |
| Vulnerable period | $2T$ | $T$ |
| Throughput equation | $S = Ge^{-2G}$ | $S = Ge^{-G}$ |
| Maximum offered load | $G = 0.5$ | $G = 1$ |
| Maximum throughput | $S_{max} = \frac{1}{2e} \approx 0.184$ | $S_{max} = \frac{1}{e} \approx 0.368$ |
| Maximum efficiency | About 18.4% | About 36.8% |
| Collision risk | Higher | Lower |
| Main cost | Wasted frames and retransmissions | Synchronization and slot wastage |

Here, $T$ is the time required to transmit one frame, $G$ is the offered load, and $S$ is the successful throughput.

---

## 1) Random Access Protocols

A random access protocol does not assign every station a permanent turn. Any station may compete for the channel when it has a frame to send.

### General characteristics

- stations make their own transmission decisions
- no central controller is required
- collisions are possible
- retransmission is used after a collision
- performance depends on the amount of traffic

### Contention

The competition among stations for a shared channel is called **contention**. When traffic is light, contention is low and ALOHA can deliver frames successfully. When traffic becomes heavy, many stations transmit and collide, causing throughput to fall.

### Why ALOHA needs acknowledgments

A sender cannot assume that a transmitted frame was received successfully. The receiver normally sends an **ACK** after receiving a valid frame.

If the sender does not receive an ACK before a timeout:

1. it assumes that the frame was lost or collided
2. it waits for a random backoff period
3. it retransmits the frame

A retry limit is usually used so that a frame is not retransmitted forever.

---

## 2) Important ALOHA Terms

### Frame time ($T$)

**Frame time** is the time needed to place one complete frame onto the channel.

$$T = \frac{\text{Frame size in bits}}{\text{Data rate in bits per second}}$$

Example: for a 1,000-bit frame sent over a 1 Mbps link:

$$T = \frac{1000}{1,000,000} = 0.001 \text{ seconds} = 1 \text{ ms}$$

### Offered load ($G$)

**Offered load** is the average number of transmission attempts made during one frame time. It includes:

- newly generated frames
- retransmissions of previously collided frames

$G$ is dimensionless. It is not only the rate at which new frames arrive.

### Throughput ($S$)

**Throughput** is the average number of successfully transmitted frames during one frame time.

It is also called normalized throughput because it is measured in frames per frame time.

$$0 \leq S \leq 1$$

A throughput of $S = 0.25$ means that, on average, one-quarter of a frame is successfully delivered per frame time.

### Vulnerable period

The **vulnerable period** is the time interval during which the transmission of another frame would cause a collision with a particular frame.

A shorter vulnerable period means a lower chance of collision.

### Collision

A collision occurs when two or more frames overlap on the shared channel. The receiver usually cannot decode the overlapping frames, so they are discarded.

---

## 3) Pure ALOHA

Pure ALOHA allows a station to transmit immediately whenever it has a frame.

### Basic operation

1. A station gets a frame ready.
2. It transmits immediately without waiting for a slot or sensing the channel.
3. The receiver checks the frame.
4. If the frame is correct, the receiver sends an ACK.
5. If a collision occurs, no usable ACK arrives.
6. After a timeout, the sender waits a random backoff period.
7. The sender retransmits the frame.

### Timeline example

Assume the tagged frame starts at time $t_0$ and takes $T$ seconds to transmit.

- It occupies the channel from $t_0$ to $t_0 + T$.
- Another frame that starts between $t_0 - T$ and $t_0$ overlaps its beginning.
- Another frame that starts between $t_0$ and $t_0 + T$ overlaps its ending.

Therefore, another frame can cause a collision if it starts within:

$$t_0 - T \quad \text{to} \quad t_0 + T$$

The total vulnerable period is:

$$2T$$

### Important property

Pure ALOHA does not require global time synchronization. This makes it simple, but it also allows frames to begin at arbitrary times, which increases the chance of overlap.

---

## 4) Pure ALOHA Vulnerable Period

Suppose a frame begins at time $t_0$ and lasts until $t_0 + T$.

For the frame to survive:

- no other frame may begin during the $T$ seconds before $t_0$
- no other frame may begin during the $T$ seconds after $t_0$

Thus:

$$\text{Vulnerable period} = T + T = 2T$$

This is the central reason that Pure ALOHA has lower efficiency. A frame is exposed to possible collision for twice its own transmission duration.

### Simple diagram

```text
Potential interfering start times
        <------ T ------>|<------ T ------>
                         t0                 t0 + T
                         |==================|
                         |   Tagged frame   |
                         |==================|
        vulnerable period = 2T
```

---

## 5) Pure ALOHA Throughput

The normalized throughput of Pure ALOHA is:

$$S = Ge^{-2G}$$

where:

- $S$ = successful throughput
- $G$ = offered load
- $e$ = Euler's number, approximately $2.71828$

### Derivation using probability

Assume frame transmission attempts follow a Poisson process.

The probability that exactly zero other frames are generated during an interval of length $2T$ is:

$$P(0 \text{ arrivals in } 2T) = e^{-2G}$$

For a tagged frame to succeed:

1. one attempt must be made for the tagged frame
2. no other attempt may occur during its vulnerable period

Therefore:

$$S = G \times e^{-2G}$$

$$\boxed{S = Ge^{-2G}}$$

### Maximum throughput

To maximize $S = Ge^{-2G}$, differentiate with respect to $G$:

$$\frac{dS}{dG} = e^{-2G}(1 - 2G)$$

Set the derivative to zero:

$$e^{-2G}(1 - 2G) = 0$$

Since $e^{-2G}$ is never zero:

$$1 - 2G = 0$$

$$G = 0.5$$

Substitute $G = 0.5$:

$$S_{max} = 0.5e^{-1} = \frac{1}{2e} \approx 0.184$$

Therefore:

- maximum offered load = $0.5$
- maximum throughput = $0.184$
- maximum efficiency = approximately **18.4%**

### Throughput behavior

- At very low $G$, few frames are attempted, so throughput is low.
- As $G$ increases, throughput increases.
- At $G = 0.5$, throughput is maximum.
- Beyond $G = 0.5$, collisions increase faster than successful transmissions.
- At very high $G$, throughput approaches zero.

---

## 6) Slotted ALOHA

Slotted ALOHA divides time into equal intervals called **slots**. Each slot has a duration equal to one frame time, $T$.

A station may start transmitting only at the beginning of a slot.

### Basic operation

1. Time is divided into slots of length $T$.
2. All stations are synchronized to the slot boundaries.
3. A station with a frame waits for the next slot boundary.
4. It transmits at that boundary.
5. If exactly one station transmits in a slot, the frame succeeds.
6. If two or more stations transmit in the same slot, a collision occurs.
7. Colliding stations wait for random future slots before retrying.

### Slot outcomes

| Number of transmitting stations | Result |
|---:|---|
| 0 | Idle slot |
| 1 | Successful transmission |
| 2 or more | Collision |

### Important property

A station cannot begin in the middle of a slot. This prevents a frame from partially overlapping two neighboring frame intervals and reduces the vulnerable period.

---

## 7) Slotted ALOHA Vulnerable Period

A frame transmitted in one slot can collide only with another frame transmitted in that same slot.

The previous slot cannot overlap it because all frames have equal length and begin on slot boundaries. The next slot cannot overlap it for the same reason.

Therefore:

$$\text{Vulnerable period} = T$$

This is half the vulnerable period of Pure ALOHA.

### Simple diagram

```text
Slot 1              Slot 2              Slot 3
|-------------------|===================|-------------------|
                    |  Tagged frame     |
                    |===================|
                    Only transmissions starting in Slot 2
                    can collide with the tagged frame.
```

---

## 8) Slotted ALOHA Throughput

The normalized throughput of Slotted ALOHA is:

$$S = Ge^{-G}$$

### Derivation using probability

For a tagged frame to succeed, no other station may transmit in the same slot.

Under the Poisson assumption, the probability of zero other attempts in one slot is:

$$P(0 \text{ arrivals in } T) = e^{-G}$$

Multiplying the attempt rate by the success probability gives:

$$S = G \times e^{-G}$$

$$\boxed{S = Ge^{-G}}$$

### Maximum throughput

Differentiate:

$$\frac{dS}{dG} = e^{-G}(1 - G)$$

Set the derivative to zero:

$$e^{-G}(1 - G) = 0$$

Therefore:

$$G = 1$$

Substitute $G = 1$:

$$S_{max} = e^{-1} = \frac{1}{e} \approx 0.368$$

Therefore:

- maximum offered load = $1$
- maximum throughput = $0.368$
- maximum efficiency = approximately **36.8%**

### Throughput behavior

- At low load, throughput is approximately equal to offered load.
- Throughput increases until $G = 1$.
- At $G = 1$, the channel achieves its maximum theoretical throughput.
- At higher load, collisions increase and throughput falls.
- At very high $G$, throughput approaches zero.

---

## 9) Collision and Success Probabilities

### Pure ALOHA

For a tagged frame, the probability that no other frame starts during its vulnerable period is:

$$P_{success} = e^{-2G}$$

Therefore:

$$P_{collision} = 1 - e^{-2G}$$

### Slotted ALOHA

For a tagged frame, the probability that no other station transmits in its slot is:

$$P_{success} = e^{-G}$$

Therefore:

$$P_{collision} = 1 - e^{-G}$$

### Comparison

For the same offered load $G$:

$$e^{-G} > e^{-2G} \quad \text{for } G > 0$$

Thus, Slotted ALOHA has a higher success probability than Pure ALOHA at the same positive offered load.

---

## 10) Retransmission and Backoff

A collision does not usually cause an immediate retransmission. If all collided stations retransmit at the same time, they are likely to collide again.

Instead, each station chooses a random backoff delay.

### Typical backoff process

1. Transmit the frame.
2. Wait for an ACK.
3. If the ACK does not arrive before the timeout, declare failure.
4. Select a random delay.
5. Wait for the delay to expire.
6. Retransmit.
7. Repeat until success or the retry limit is reached.

### Why random backoff is needed

Random backoff separates retransmission times. It prevents a group of stations involved in one collision from repeatedly starting together.

The exact backoff algorithm can vary by system. ALOHA itself describes the random-access idea; later protocols define more detailed backoff rules.

---

## 11) Pure ALOHA vs Slotted ALOHA

| Comparison point | Pure ALOHA | Slotted ALOHA |
|---|---|---|
| Channel access | Immediate transmission | Wait for a slot boundary |
| Synchronization | Not needed | Required among stations |
| Frame start time | Any time | Only at discrete boundaries |
| Vulnerable period | $2T$ | $T$ |
| Maximum throughput | $1/(2e)$ | $1/e$ |
| Maximum throughput percentage | 18.4% | 36.8% |
| Collision probability | Higher | Lower |
| Idle time | No forced slot boundary waiting | May waste an entire empty slot |
| Implementation | Simpler | More complex because timing is needed |
| Delay | Can transmit immediately | May wait until the next slot |
| Synchronization overhead | None | Clock or timing coordination needed |
| Best condition | Very simple, light, bursty traffic | Shared channel where synchronization is available |

### Main conclusion

Slotted ALOHA is more efficient because it limits possible transmission starts to slot boundaries. Its vulnerable period is half that of Pure ALOHA, so its maximum throughput is twice as high.

The improvement is not because collisions disappear. Collisions still occur when multiple stations choose the same slot.

---

## 12) Advantages and Disadvantages

### Pure ALOHA advantages

- very simple design
- no synchronization mechanism is needed
- decentralized operation
- stations can transmit immediately
- suitable for low-traffic and bursty communication

### Pure ALOHA disadvantages

- high collision probability
- low maximum efficiency of 18.4%
- unpredictable delay
- retransmissions waste bandwidth
- performance collapses under heavy traffic
- duplicate frames must be handled if an ACK is delayed or lost

### Slotted ALOHA advantages

- twice the maximum throughput of Pure ALOHA
- lower vulnerable period
- simpler collision model because collisions happen within slots
- decentralized channel access after synchronization
- useful for systems with a natural time-slot structure

### Slotted ALOHA disadvantages

- requires station synchronization
- a newly ready frame may wait for the next slot
- an empty slot wastes one full slot duration
- collisions still occur
- throughput remains much lower than well-designed carrier-sensing protocols
- synchronization errors can cause timing problems

---

## 13) Assumptions Behind the Throughput Equations

The equations $S = Ge^{-2G}$ and $S = Ge^{-G}$ are idealized analytical results. They generally assume:

- a shared broadcast channel
- equal frame transmission times
- a fixed frame time $T$
- independent station attempts
- a large population of stations
- Poisson-distributed transmission attempts
- immediate collision visibility or ACK-based failure detection
- no propagation delay in the simplified model
- unlimited or sufficiently large retransmission opportunities
- collisions destroy all overlapping frames

Real systems may have additional effects such as propagation delay, capture effects, finite retry limits, variable frame sizes, channel errors, and processing delays.

---

## 14) Worked Numerical Examples

### Example 1: Pure ALOHA throughput at $G = 0.5$

Given:

$$S = Ge^{-2G}$$

Substitute $G = 0.5$:

$$S = 0.5e^{-1}$$

$$S \approx 0.184$$

**Answer:** Throughput is approximately **18.4%**, which is the maximum Pure ALOHA throughput.

### Example 2: Slotted ALOHA throughput at $G = 1$

Given:

$$S = Ge^{-G}$$

Substitute $G = 1$:

$$S = 1 \times e^{-1}$$

$$S \approx 0.368$$

**Answer:** Throughput is approximately **36.8%**, which is the maximum Slotted ALOHA throughput.

### Example 3: Compare both protocols at $G = 0.5$

Pure ALOHA:

$$S_P = 0.5e^{-1} \approx 0.184$$

Slotted ALOHA:

$$S_S = 0.5e^{-0.5} \approx 0.303$$

**Answer:** At $G = 0.5$, Slotted ALOHA provides approximately **30.3%** throughput, while Pure ALOHA provides approximately **18.4%**.

### Example 4: Collision probability in Pure ALOHA at $G = 0.5$

$$P_{collision} = 1 - e^{-2G}$$

$$P_{collision} = 1 - e^{-1}$$

$$P_{collision} \approx 1 - 0.368 = 0.632$$

**Answer:** The collision probability for a tagged attempt is approximately **63.2%**.

### Example 5: Collision probability in Slotted ALOHA at $G = 1$

$$P_{collision} = 1 - e^{-G}$$

$$P_{collision} = 1 - e^{-1}$$

$$P_{collision} \approx 0.632$$

**Answer:** At its maximum-throughput load, the collision probability is also approximately **63.2%**. Maximum throughput does not mean that most attempts are successful; it balances attempt rate and success probability.

### Example 6: Calculate frame time

A 2,000-bit frame is transmitted at 2 Mbps.

$$T = \frac{2000}{2,000,000} = 0.001 \text{ seconds}$$

So:

- frame time = **1 ms**
- Pure ALOHA vulnerable period = **2 ms**
- Slotted ALOHA vulnerable period = **1 ms**

---

## 15) Common Misconceptions

### Misconception 1: ALOHA senses the channel first

Basic ALOHA does not sense the channel before transmitting. Carrier sensing belongs to CSMA.

### Misconception 2: Slotted ALOHA eliminates collisions

It does not eliminate collisions. Multiple stations can still select the same slot.

### Misconception 3: $G$ means only newly generated traffic

$G$ includes both new transmission attempts and retransmission attempts.

### Misconception 4: 36.8% means 36.8% of all bits are always delivered

It is the maximum normalized theoretical throughput under the model assumptions, not a universal real-world data rate.

### Misconception 5: ALOHA and Ethernet are the same

ALOHA is a random transmission protocol. Traditional Ethernet used CSMA/CD, which senses the channel and detects collisions. Modern switched full-duplex Ethernet normally has no collisions.

### Misconception 6: Acknowledgment is unnecessary

ACKs or equivalent success feedback are needed so the sender can distinguish a successful transmission from a collision or lost frame.

---

## 16) Applications and Historical Importance

Pure ALOHA was developed for the University of Hawaii's radio communication system, where geographically separated stations shared a radio channel.

ALOHA-style random access is useful when:

- many stations send occasional short messages
- traffic is bursty rather than continuous
- a central controller is undesirable
- a simple contention mechanism is sufficient

Slotted ALOHA has been used or adapted in systems involving time slots, including some satellite, radio, wireless, and machine-to-machine access scenarios.

Modern communication systems often use improved protocols such as:

- CSMA/CA
- reservation-based access
- scheduled TDMA
- polling
- token-based access
- random access preambles in cellular networks

The original ALOHA concepts remain important for understanding how shared-medium protocols trade simplicity, delay, collision risk, and throughput.

---

## 17) ALOHA and Related Protocols

| Protocol | Main idea | Collision handling |
|---|---|---|
| Pure ALOHA | Transmit at any time | Random retransmission |
| Slotted ALOHA | Transmit at slot boundaries | Random future slot |
| CSMA | Listen before transmitting | Avoid or detect some collisions |
| CSMA/CD | Listen and detect while transmitting | Stop, jam, and back off |
| CSMA/CA | Listen and try to avoid collision | Backoff, ACK, optional RTS/CTS |
| TDMA | Assigned time slots | Scheduled access, normally collision-free |

ALOHA is less efficient than protocols that sense or schedule access, but its simplicity can be valuable when sensing or centralized scheduling is difficult.

---

## 18) Important Definitions

- **ALOHA:** A random access protocol in which stations compete for a shared channel.
- **Pure ALOHA:** ALOHA variant that permits transmission at any time.
- **Slotted ALOHA:** ALOHA variant that permits transmission only at slot boundaries.
- **Frame time:** Time required to transmit one complete frame.
- **Offered load ($G$):** Average number of transmission attempts per frame time, including retransmissions.
- **Throughput ($S$):** Average number of successful frames per frame time.
- **Vulnerable period:** Time during which another transmission can collide with a tagged frame.
- **Contention:** Competition among stations for access to a shared channel.
- **Backoff:** A waiting period selected before retransmitting after failure.
- **ACK:** Acknowledgment sent by the receiver to indicate successful reception.
- **Collision:** Overlap of two or more transmissions that prevents correct decoding.
- **Synchronization:** Agreement among stations about common slot boundaries.

---

## 19) Example Questions and Answers

### Q1. What is Pure ALOHA?

**Answer:** Pure ALOHA is a random access protocol in which a station transmits immediately whenever it has a frame. If a collision occurs, it waits for a random time and retransmits.

### Q2. What is the vulnerable period of Pure ALOHA?

**Answer:** The vulnerable period is $2T$, where $T$ is the frame transmission time.

### Q3. What is the vulnerable period of Slotted ALOHA?

**Answer:** The vulnerable period is $T$ because all frames start at slot boundaries.

### Q4. State the throughput equation for Pure ALOHA.

**Answer:**

$$S = Ge^{-2G}$$

### Q5. State the throughput equation for Slotted ALOHA.

**Answer:**

$$S = Ge^{-G}$$

### Q6. What is the maximum throughput of Pure ALOHA?

**Answer:**

$$S_{max} = \frac{1}{2e} \approx 0.184$$

It is approximately **18.4%** at $G = 0.5$.

### Q7. What is the maximum throughput of Slotted ALOHA?

**Answer:**

$$S_{max} = \frac{1}{e} \approx 0.368$$

It is approximately **36.8%** at $G = 1$.

### Q8. Why is Slotted ALOHA more efficient?

**Answer:** Slotted ALOHA restricts transmission starts to slot boundaries. This reduces the vulnerable period from $2T$ to $T$, reducing the probability of collision.

### Q9. Does Slotted ALOHA require synchronization?

**Answer:** Yes. All stations must agree on the beginning and duration of slots.

### Q10. Does ALOHA use carrier sensing?

**Answer:** No. Basic ALOHA transmits without first sensing whether the channel is idle. Carrier sensing is used by CSMA.

### Q11. What happens after a collision?

**Answer:** The sender waits for an acknowledgment. If it does not arrive before the timeout, the sender waits for a random backoff period and retransmits.

### Q12. What is the difference between $G$ and $S$?

**Answer:** $G$ is the total offered transmission attempt rate, including retransmissions. $S$ is the successful delivery rate.

### Q13. Why does throughput decrease at high offered load?

**Answer:** More stations attempt transmission, so collisions and retransmissions increase. Eventually, most channel time is spent on unsuccessful attempts.

### Q14. Which is simpler: Pure ALOHA or Slotted ALOHA?

**Answer:** Pure ALOHA is simpler because it does not require synchronization. Slotted ALOHA is more efficient but needs common timing.

---

## 20) Quick Revision Notes

- ALOHA is a random access MAC protocol.
- Pure ALOHA transmits whenever a frame is ready.
- Slotted ALOHA transmits only at the beginning of a slot.
- Frame time is represented by $T$.
- Offered load is represented by $G$.
- Successful throughput is represented by $S$.
- Pure ALOHA vulnerable period is $2T$.
- Slotted ALOHA vulnerable period is $T$.
- Pure ALOHA throughput: $S = Ge^{-2G}$.
- Slotted ALOHA throughput: $S = Ge^{-G}$.
- Pure ALOHA maximum: 18.4% at $G = 0.5$.
- Slotted ALOHA maximum: 36.8% at $G = 1$.
- Slotted ALOHA has twice the maximum theoretical throughput of Pure ALOHA.
- Both protocols still experience collisions.
- ACK, timeout, and random backoff support retransmission.
- Heavy traffic decreases ALOHA throughput.
- ALOHA does not perform carrier sensing.

---

## 21) Final Summary

Pure ALOHA and Slotted ALOHA allow many stations to share one channel without a central access controller. Pure ALOHA favors simplicity: a station can transmit at any time, but a frame is vulnerable for $2T$ and the maximum throughput is only 18.4%.

Slotted ALOHA adds synchronization and restricts transmissions to slot boundaries. This reduces the vulnerable period to $T$ and raises maximum throughput to 36.8%.

The most important relationship to remember is:

$$\text{Pure ALOHA: } S = Ge^{-2G}, \qquad S_{max} = 18.4\%$$

$$\text{Slotted ALOHA: } S = Ge^{-G}, \qquad S_{max} = 36.8\%$$

The improvement comes from reducing the vulnerable period, not from removing collisions completely.

---

## 🔭 Tomorrow's Preview

- CSMA: Carrier Sense Multiple Access
- 1-persistent, non-persistent, and p-persistent CSMA
- Why carrier sensing improves ALOHA efficiency
- Propagation delay and the remaining collision problem
