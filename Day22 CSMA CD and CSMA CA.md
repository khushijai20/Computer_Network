# 🌐 Day 22: CSMA/CD and CSMA/CA

## 📝 Overview

**CSMA/CD** and **CSMA/CA** are Media Access Control techniques used when multiple devices share a communication medium.

- **CSMA/CD** means **Carrier Sense Multiple Access with Collision Detection**.
- **CSMA/CA** means **Carrier Sense Multiple Access with Collision Avoidance**.

Both protocols begin with carrier sensing: a device listens before transmitting. Their main difference is what happens after that:

- CSMA/CD detects a collision while transmission is in progress.
- CSMA/CA tries to reduce the probability of a collision before transmission and uses an acknowledgment to infer success.

CSMA/CD is associated with traditional shared, half-duplex Ethernet. CSMA/CA is associated with IEEE 802.11 wireless LANs, or Wi-Fi.

---

## 📚 Topics Covered

- Carrier sensing and shared-medium access
- Collision causes and propagation delay
- CSMA/CD operation and jam signals
- Ethernet slot time and minimum frame size
- Binary exponential backoff
- CSMA/CA operation and random backoff
- Inter-frame spaces, ACKs, contention windows, and NAV
- RTS/CTS and hidden-terminal and exposed-terminal problems
- CSMA/CD versus CSMA/CA
- Modern Ethernet and Wi-Fi notes
- Formulas, examples, misconceptions, and exam questions

---

## 🎯 Learning Objectives

By the end of this note, you should be able to:

- expand and define CSMA/CD and CSMA/CA
- explain why carrier sensing does not completely prevent collisions
- describe every major step in CSMA/CD
- explain jam signals and binary exponential backoff
- explain every major step in CSMA/CA
- describe the role of ACK, IFS, contention window, and NAV
- identify hidden-terminal and exposed-terminal problems
- compare wired collision detection with wireless collision avoidance
- solve basic propagation-delay and backoff questions
- explain why CSMA/CD is mostly historical in modern switched Ethernet

---

## 🧠 Quick Cheat Sheet

| Feature | CSMA/CD | CSMA/CA |
|---|---|---|
| Full name | Carrier Sense Multiple Access with Collision Detection | Carrier Sense Multiple Access with Collision Avoidance |
| Common environment | Traditional shared Ethernet | IEEE 802.11 Wi-Fi |
| Main strategy | Detect a collision after it starts | Reduce the chance of collision before sending |
| Can the sender listen while sending? | Usually yes in classic wired Ethernet | Generally cannot reliably do so |
| Failure feedback | Collision detection and jam signal | Missing ACK or other failure feedback |
| Backoff | Binary exponential backoff | Random contention-window backoff |
| ACK required for normal operation | Not the primary mechanism | Normally yes for unicast data |
| RTS/CTS | Not normally used | Optional |
| Current importance | Mainly legacy half-duplex Ethernet | Fundamental to wireless LAN access |

---

## 1) Foundations of CSMA

### Meaning of CSMA

**Carrier Sense Multiple Access** has three parts:

- **Carrier Sense:** the station listens to determine whether the medium is busy.
- **Multiple Access:** many stations use the same physical or radio channel.
- **Contention:** stations compete for access instead of receiving a permanent turn.

A basic CSMA station follows this pattern:

1. A frame becomes ready.
2. The station senses the medium.
3. If the medium is busy, it waits according to the protocol.
4. If the medium is idle, it attempts transmission.
5. It checks whether transmission succeeded.
6. It waits and retries if the transmission failed.

### Why collisions can still occur

Carrier sensing is local and is not instantaneous across the entire network. Two stations can both observe an idle channel and start transmitting before either signal reaches the other station. This happens because of **propagation delay**.

```text
Station A starts                         A's signal reaches B
       |------------------------------------------->|
                 propagation delay
                         Station B may start here

Result: the two signals overlap and collide
```

A useful normalized value is:

$$a = \frac{\text{propagation delay}}{\text{frame transmission time}}$$

A smaller $a$ generally gives better efficiency because the collision window is smaller compared with the time needed to transmit a frame.

### CSMA persistence recap

| Method | If channel is busy | If channel becomes idle |
|---|---|---|
| 1-persistent | Keep sensing | Transmit immediately |
| Non-persistent | Wait a random time | Sense again later |
| p-persistent | Wait for a slot | Transmit with probability $p$ |

CSMA/CD and CSMA/CA add collision-handling rules to the general CSMA process.

---

## 2) CSMA/CD

### Definition

**CSMA/CD** is a contention-based protocol in which a station senses the channel before transmission and monitors the channel during transmission to detect a collision.

It was used by classic shared Ethernet, including bus Ethernet and Ethernet hubs. It applies to a shared, half-duplex medium where several stations can transmit on the same collision domain.

### CSMA/CD operation

1. **Prepare:** the station has a frame to send.
2. **Carrier sense:** it listens to the medium.
3. **Defer:** if the medium is busy, it waits until the medium becomes idle.
4. **Transmit:** it begins sending the frame.
5. **Monitor:** while sending, it compares the transmitted signal with the signal on the medium.
6. **Success:** if the frame finishes without a collision, the transmission is complete.
7. **Collision:** if a collision is detected, the station stops sending the frame.
8. **Jam:** it transmits a jam signal so other stations recognize the collision.
9. **Backoff:** it selects a random waiting interval.
10. **Retry:** it senses and retransmits, subject to the retry limit.

```text
Sense -> idle? -> transmit and monitor
   |                  |
  busy            collision?
   |              /        \
  wait           no          yes
                  |           |
                finish    stop + jam
                                |
                         random backoff
                                |
                              retry
```

### Why transmission stops after a collision

Once a collision has damaged the frame, continuing to send the rest of that frame wastes bandwidth. Stopping early allows the station to release the medium and retry later.

### Collision detection in wired Ethernet

A wired Ethernet station can compare the signal it is transmitting with the signal present on the cable. A mismatch or abnormal voltage indicates that another station transmitted at the same time.

Wireless stations cannot normally use this method reliably because their own transmitted signal is much stronger than a received signal, and radio interference and hidden stations complicate detection.

### Jam signal

A **jam signal** is a special signal sent after collision detection. Its purpose is to ensure that all stations involved recognize that a collision occurred, even if the original collision would have been too short to observe clearly.

The jam signal does not repair the damaged frame. It only communicates that the frame must be discarded and retransmitted.

---

## 3) Ethernet Timing and Minimum Frame Size

### The collision-detection timing problem

A station must still be transmitting when a possible collision signal returns to it. Otherwise, it may finish a short frame and incorrectly assume that transmission succeeded before learning that a distant station transmitted too.

For classic Ethernet, the transmission time of the minimum frame was designed to be at least the maximum round-trip propagation time of the collision domain, including relevant hardware delay.

The general requirement is:

$$T_{frame} \geq 2T_p$$

where:

- $T_{frame}$ is the time needed to transmit the frame
- $T_p$ is the maximum one-way propagation delay
- $2T_p$ is the approximate round-trip propagation delay

This relationship is why Ethernet has a minimum frame size and a defined slot time.

### Slot time

A **slot time** is the standard time interval used by Ethernet for collision detection and backoff. In classic Ethernet, it corresponds to the time needed for a collision signal to travel across the maximum collision domain and return, with implementation margins.

A backoff value is expressed in slot times rather than arbitrary seconds.

### Ethernet collision domain

A **collision domain** is the part of a network in which simultaneous transmissions can interfere with one another. Hubs and shared bus networks create one collision domain. A switch separates collision domains at its ports.

Modern full-duplex switched Ethernet normally has no collisions because each link has a dedicated transmit and receive path.

---

## 4) Binary Exponential Backoff in CSMA/CD

If every station retransmitted immediately after a collision, the same stations could collide repeatedly. Backoff randomizes their retry times.

After collision number $k$, a station chooses a random integer:

$$K \in \{0,1,2,\ldots,2^k-1\}$$

It then waits:

$$T_{backoff} = K \times \text{slot time}$$

In actual Ethernet, the range is capped by a maximum exponent and the station stops after a maximum retry count. The phrase **binary exponential** means that the possible waiting range grows approximately by a factor of two after repeated collisions.

### Example

If a station has experienced three collisions and the range has not reached its maximum:

$$K \in \{0,1,2,3,4,5,6,7\}$$

The station randomly selects one value from these eight choices. A larger range makes it less likely that the same stations will choose the same retry time.

### Why backoff is adaptive

- Light contention produces a small waiting range and low delay.
- Heavy contention produces a larger waiting range and fewer repeated collisions.
- Randomness prevents all waiting stations from restarting together.

### CSMA/CD limitations

- A collision still wastes part of the channel capacity.
- Detection is possible only while the station is transmitting.
- Performance falls under heavy contention.
- It is not suitable for ordinary Wi-Fi operation.
- It is generally unnecessary on full-duplex switched Ethernet.

---

## 5) CSMA/CA

### Definition

**CSMA/CA** is a wireless access method that attempts to avoid collisions before they occur. It is used by IEEE 802.11 wireless LANs.

Wireless devices usually cannot detect collisions reliably while transmitting, so they use sensing, waiting, random backoff, acknowledgments, and sometimes RTS/CTS.

### Basic CSMA/CA operation

1. **Frame ready:** a wireless station has a frame to send.
2. **Sense:** it checks whether the radio channel is busy.
3. **Defer:** if the channel is busy, it waits.
4. **Inter-frame space:** if the channel is idle for the required interval, it prepares to contend.
5. **Random backoff:** it selects a counter from the current contention window.
6. **Countdown:** the counter decreases only while the channel remains idle.
7. **Freeze:** if another station begins transmitting, the counter stops.
8. **Resume:** after the channel is idle for the required interval again, the countdown resumes.
9. **Transmit:** when the counter reaches zero, the station sends the frame.
10. **ACK:** the receiver sends an acknowledgment after successfully receiving the frame.
11. **Retry:** if the ACK does not arrive, the sender assumes the frame failed and retries after another backoff.

```text
Sense channel
     |
  busy? -------- yes --------> wait
     |
    no
     |
 wait required IFS
     |
 choose random backoff
     |
 countdown while idle
     |
 channel busy? ---- yes ----> freeze counter
     |                              |
    no                         resume later
     |
 counter reaches zero
     |
 transmit -> wait for ACK -> success or retry
```

### Why the backoff counter freezes

Freezing prevents a station from starting in the middle of another frame. It also preserves the waiting time already completed, so the station does not have to choose a new counter every time it hears traffic.

### Collision avoidance is not collision elimination

Two stations may still select the same backoff value and transmit together. A hidden station may not sense another transmission. Interference may also corrupt a frame. CSMA/CA only reduces the probability of collision; it cannot guarantee a collision-free channel.

---

## 6) Important CSMA/CA Components

### A) Inter-frame space (IFS)

An **inter-frame space** is a required idle period between wireless frames. Different frame types use different waiting intervals. A shorter IFS gives certain control or response frames higher priority than ordinary data frames.

Important IEEE 802.11 terms include:

- **SIFS (Short Inter-Frame Space):** used before high-priority responses such as ACK and CTS.
- **DIFS (DCF Inter-Frame Space):** used by a station before starting normal contention under the Distributed Coordination Function.
- **AIFS (Arbitration Inter-Frame Space):** used by quality-of-service mechanisms to provide different traffic priorities.

The exact timing values depend on the physical layer and Wi-Fi standard.

### B) Contention window

The **contention window**, often written as $CW$, is the range used to select the random backoff counter.

A simplified selection is:

$$B \in \{0,1,2,\ldots,CW\}$$

The selected value is measured in slot times. After a failed transmission, the contention window normally increases, making the next retry more widely distributed. After successful transmission, it normally returns toward its minimum value.

The exact values and retry behavior vary by IEEE 802.11 version and implementation.

### C) Acknowledgment (ACK)

A receiver sends an ACK after correctly receiving a unicast data frame. The sender uses the ACK as positive delivery feedback.

- ACK received: the sender treats the exchange as successful.
- ACK missing: the sender assumes loss caused by collision, interference, fading, noise, or another problem.

A missing ACK does not prove that a collision occurred.

### D) Network Allocation Vector (NAV)

The **NAV** is a virtual carrier-sensing mechanism. A station reads the duration information in a frame and sets a timer for how long the medium is expected to remain reserved. During that time, it defers even if it cannot physically hear the data exchange itself.

Physical carrier sensing and virtual carrier sensing work together in Wi-Fi.

### E) Distributed Coordination Function (DCF)

DCF is the basic distributed access method in Wi-Fi. Stations compete without requiring a central controller for every transmission. It uses carrier sensing, IFS, random backoff, and acknowledgments.

Modern Wi-Fi also includes coordination and quality-of-service mechanisms, but the DCF process is the foundation for understanding CSMA/CA.

---

## 7) RTS/CTS

### Meaning

- **RTS:** Request to Send
- **CTS:** Clear to Send

RTS/CTS is an optional exchange used to reserve the wireless medium before sending a longer data frame.

### RTS/CTS sequence

1. The sender waits for the channel to be available.
2. The sender transmits an RTS frame.
3. The receiver answers with a CTS frame.
4. Stations that hear the RTS or CTS update their NAV and defer.
5. The sender transmits the data frame.
6. The receiver sends an ACK.

```text
Sender                         Receiver
   | -------- RTS ------------> |
   | <-------- CTS ------------ |
   | -------- DATA -----------> |
   | <--------- ACK ----------- |
```

### Benefits

- reduces the time wasted transmitting a large frame that may collide
- helps with hidden-terminal situations
- can reserve the channel for a complete exchange

### Costs

- RTS and CTS consume airtime
- they add delay and control overhead
- a short frame may finish faster without the exchange
- RTS/CTS itself can be lost or collide

RTS/CTS is therefore most useful when hidden terminals, long frames, or high contention justify its overhead. It is not required for every Wi-Fi frame.

---

## 8) Hidden and Exposed Terminals

### Hidden-terminal problem

Two stations may both communicate with an access point but may be unable to hear one another. Each station senses the channel as idle and transmits, causing a collision at the access point.

```text
Station A  ------>  Access Point  <------  Station B
     \                                   /
       A and B cannot hear each other
```

RTS/CTS can help because the CTS from the access point may reach both stations, causing the hidden station that hears it to defer.

### Exposed-terminal problem

A station hears a nearby transmission and defers, even though its own transmission to a different receiver would not interfere with the first exchange. This unnecessarily reduces spatial reuse.

RTS/CTS can sometimes provide more information, but it does not solve every exposed-terminal situation.

### Capture effect

The **capture effect** occurs when a receiver successfully decodes a stronger signal even while a weaker signal overlaps it. Therefore, a real wireless overlap does not always destroy every frame, although simplified explanations often treat every overlap as a collision.

---

## 9) CSMA/CD versus CSMA/CA

| Point of comparison | CSMA/CD | CSMA/CA |
|---|---|---|
| Main idea | Detect collision during transmission | Avoid or reduce collision before transmission |
| Medium | Shared wired medium | Shared wireless medium |
| Transmission monitoring | Monitors the medium while sending | Usually cannot reliably detect its own collision |
| Success or failure | Collision can be detected directly | Success is commonly confirmed by ACK |
| After failure | Jam signal, random backoff, retry | Increase contention window, backoff, retry |
| Randomization | Binary exponential backoff | Random contention-window backoff |
| Control frames | Jam signal | ACK; optional RTS/CTS |
| Hidden terminals | Not the usual issue | Major wireless issue |
| Duplex status | Classic half-duplex Ethernet | Wireless is shared and generally half-duplex |
| Modern use | Legacy shared Ethernet | Still central to Wi-Fi |

### Exam sentence

**CSMA/CD detects a collision after transmission begins, whereas CSMA/CA attempts to avoid a collision before transmission begins and uses acknowledgment to confirm likely success.**

---

## 10) Why Wireless Uses CSMA/CA Instead of CSMA/CD

Wireless networks generally prefer collision avoidance for several reasons:

1. **Self-signal problem:** a station's own transmitted signal can overwhelm a received collision signal.
2. **Hidden terminals:** a station cannot detect transmissions from devices outside its sensing range.
3. **Variable radio conditions:** fading, interference, noise, and obstacles change signal quality.
4. **Shared half-duplex radio:** a normal wireless station cannot transmit and listen with equal reliability at the same time.
5. **Remote collision location:** a collision may occur at the receiver even though the sender cannot observe it locally.

For these reasons, Wi-Fi uses waiting, randomization, virtual sensing, optional RTS/CTS, and ACKs instead of relying on direct collision detection.

---

## 11) Modern Ethernet and Wi-Fi Notes

### Modern Ethernet

Most current Ethernet networks use switches and full-duplex links:

- each device has a dedicated link to a switch port
- transmitting and receiving use separate paths
- the link does not have competing stations sharing one collision domain
- collisions do not occur during normal operation
- CSMA/CD is therefore disabled or not applicable

CSMA/CD remains important for understanding Ethernet history, hubs, half-duplex operation, and networking exam questions.

### Modern Wi-Fi

Wi-Fi still uses contention because many stations share the same radio channel. However, practical Wi-Fi behavior also depends on:

- access-point placement and radio range
- channel width and channel overlap
- signal strength and noise
- interference from other networks
- retransmissions and rate adaptation
- quality-of-service priorities
- power-saving behavior
- newer Wi-Fi coordination features

The core CSMA/CA concepts remain carrier sensing, inter-frame spaces, random backoff, ACKs, and retransmission.

---

## 12) Numerical Examples

### Example 1: Propagation delay

A signal travels through a 1,000-meter cable at $2 \times 10^8$ meters per second. Find the one-way propagation delay.

$$T_p = \frac{\text{distance}}{\text{propagation speed}}$$

$$T_p = \frac{1000}{2 \times 10^8} = 5 \times 10^{-6}\text{ seconds}$$

**Answer:** The one-way propagation delay is **5 microseconds**. The approximate round-trip propagation delay is **10 microseconds**.

### Example 2: Normalized propagation delay

A frame takes 100 microseconds to transmit and the one-way propagation delay is 5 microseconds.

$$a = \frac{5}{100} = 0.05$$

**Answer:** $a = 0.05$. The propagation delay is small compared with frame transmission time, which is favorable for CSMA efficiency.

### Example 3: CSMA/CD backoff

A station experiences three collisions. Ignoring the standard maximum range, choose its possible backoff values.

$$K \in \{0,1,2,3,4,5,6,7\}$$

**Answer:** It randomly chooses one of eight slot counts and waits that many slot times before retrying.

### Example 4: Backoff time

Suppose a station selects $K=6$ and the slot time is 10 microseconds.

$$T_{backoff} = K \times \text{slot time}$$

$$T_{backoff} = 6 \times 10 = 60\text{ microseconds}$$

**Answer:** The station waits 60 microseconds before trying again, assuming the channel remains available according to the protocol.

### Example 5: p-persistent comparison

A slotted station uses $p=0.25$. At an idle slot boundary:

- probability of transmitting = $0.25$
- probability of deferring one slot = $1-p=0.75$

**Answer:** The station spreads possible attempts across slots instead of always transmitting immediately.

---

## 13) Advantages and Disadvantages

### CSMA/CD advantages

- simple and decentralized
- detects collisions early
- stops damaged transmissions before the entire frame is sent
- adaptive binary exponential backoff handles changing contention

### CSMA/CD disadvantages

- collisions still consume bandwidth
- performance decreases as contention increases
- requires reliable collision detection during transmission
- unsuitable for ordinary wireless networks
- mostly unnecessary in modern full-duplex switched Ethernet

### CSMA/CA advantages

- suitable for shared wireless channels
- random backoff reduces simultaneous transmissions
- ACKs provide delivery feedback
- RTS/CTS can reduce hidden-terminal collisions
- no central controller is required for basic distributed access

### CSMA/CA disadvantages

- collisions can still happen
- backoff, IFS, ACK, RTS, and CTS consume airtime
- a missing ACK does not identify the exact cause of failure
- hidden and exposed terminals remain possible
- interference and fading can cause retransmissions without a collision
- access delay is unpredictable under heavy traffic

---

## 14) Important Definitions

- **Carrier sense:** listening to a medium before attempting transmission.
- **Collision:** overlapping transmissions that interfere with one another.
- **Collision domain:** the area in which simultaneous transmissions can collide.
- **Propagation delay:** time required for a signal to travel from sender to receiver.
- **CSMA/CD:** CSMA with collision detection, mainly associated with classic shared Ethernet.
- **CSMA/CA:** CSMA with collision avoidance, mainly associated with Wi-Fi.
- **Jam signal:** signal sent by CSMA/CD after collision detection to notify stations of failure.
- **Backoff:** waiting period selected before a transmission or retransmission.
- **Binary exponential backoff:** a backoff method whose possible range grows after repeated collisions.
- **Contention window:** range from which a wireless station selects a random backoff value.
- **ACK:** acknowledgment sent by a receiver after successful frame reception.
- **IFS:** required idle interval between wireless frame exchanges.
- **NAV:** virtual carrier-sensing timer that tells a Wi-Fi station how long to defer.
- **RTS:** Request to Send control frame.
- **CTS:** Clear to Send control frame.
- **Hidden terminal:** station that cannot hear another sender but can interfere at the receiver.
- **Exposed terminal:** station that unnecessarily defers because it hears a transmission that would not interfere with its own exchange.
- **Full-duplex:** communication in both directions simultaneously on a dedicated link.

---

## 15) Common Misconceptions

### Misconception 1: CSMA/CD prevents all collisions

False. It detects collisions after they begin. Propagation delay allows more than one station to start transmitting.

### Misconception 2: CSMA/CA guarantees no collisions

False. Two stations may choose the same backoff value, or hidden stations may transmit simultaneously.

### Misconception 3: A missing Wi-Fi ACK proves a collision

False. The frame may have been lost because of interference, fading, noise, or a receiver problem.

### Misconception 4: RTS/CTS is required for every Wi-Fi transmission

False. RTS/CTS is optional and adds overhead. It is most helpful when hidden terminals or long frames justify the cost.

### Misconception 5: A switch uses CSMA/CD for every modern Ethernet frame

Usually false. Full-duplex switched links do not have collisions, so CSMA/CD is normally inactive.

### Misconception 6: Carrier sensing tells a station what every device is doing

False. Sensing is local. A distant, hidden, blocked, or weak signal may not be detected.

### Misconception 7: CSMA/CD and CSMA/CA differ only by the last letter

False. Their environments and failure strategies are fundamentally different: wired collision detection versus wireless collision avoidance.

---

## 16) Practical Commands and Observation

These commands do not manually run CSMA/CD or CSMA/CA, but they help inspect the network environment in which the protocols operate.

1. `ipconfig /all` — displays Ethernet and Wi-Fi adapter details.
2. `netsh wlan show interfaces` — displays connected Wi-Fi SSID, signal, radio type, channel, and receive/transmit rates.
3. `netsh wlan show networks mode=bssid` — lists visible Wi-Fi networks, channels, signal levels, and access-point BSSIDs.
4. `ping <address>` — observes reachability and round-trip delay; packet loss can have several causes and does not by itself prove a collision.
5. `tracert <address>` — displays the Layer 3 path toward a destination.

A packet analyzer such as Wireshark can be used to inspect wireless management and control frames when the adapter and capture mode support them. Capture visibility depends on the operating system, adapter, driver, encryption, and monitor-mode support.

---

## ✅ Quick Revision Notes

- **CSMA:** listen before transmitting.
- **CSMA/CD:** detect a collision while transmitting; stop, send jam, back off, and retry.
- **CSMA/CA:** wait, choose a random backoff, transmit, wait for ACK, and retry if needed.
- Collision detection is practical for classic shared wired Ethernet.
- Collision avoidance is used for Wi-Fi because wireless collision detection is unreliable.
- Propagation delay is the reason CSMA cannot eliminate every collision.
- CSMA/CD uses a jam signal; CSMA/CA normally uses ACKs.
- Ethernet uses binary exponential backoff after repeated collisions.
- Wi-Fi uses contention windows, IFS, and a backoff counter that freezes when the channel becomes busy.
- RTS/CTS can reduce hidden-terminal problems but adds overhead.
- Modern full-duplex switched Ethernet normally has no collisions.
- A missing Wi-Fi ACK indicates failed delivery, not necessarily a collision.

---

## 📝 Exam Questions and Answers

### Q1. What is CSMA/CD?

CSMA/CD is a MAC protocol in which a station senses the medium, transmits if it is idle, monitors the medium for a collision, sends a jam signal if a collision is detected, waits for a random backoff interval, and retries.

### Q2. What is CSMA/CA?

CSMA/CA is a wireless MAC protocol that senses the medium, waits for the required interval, uses random backoff to reduce simultaneous transmissions, sends the frame, and uses an ACK to confirm likely successful delivery.

### Q3. Why can a collision occur even after carrier sensing?

Because sensing is local and propagation takes time. Two distant stations may both sense an idle channel and transmit before either signal reaches the other.

### Q4. Why does CSMA/CD send a jam signal?

The jam signal makes the collision long enough for all stations involved to recognize the failure and discard their damaged frames.

### Q5. What is binary exponential backoff?

It is a method that increases the range of randomly selected waiting times after repeated collisions, reducing the chance of another immediate collision.

### Q6. Why does Wi-Fi use ACKs?

A wireless sender usually cannot reliably detect a collision while transmitting. An ACK from the receiver provides feedback that the frame was decoded successfully.

### Q7. What happens to the CSMA/CA backoff counter when the channel becomes busy?

The counter freezes. It resumes after the channel has been idle for the required inter-frame space.

### Q8. What is the hidden-terminal problem?

It occurs when two stations cannot hear one another but can both reach the same receiver. They may transmit simultaneously and collide at that receiver.

### Q9. How does RTS/CTS help?

RTS/CTS reserves the medium before the data frame. Stations that hear the RTS or CTS defer for the announced duration, reducing some hidden-terminal collisions.

### Q10. Why is CSMA/CD not normally used in modern Ethernet?

Modern Ethernet usually uses full-duplex switched links. Each device has a dedicated link, so stations do not contend for a shared collision domain.

### Q11. Compare CSMA/CD and CSMA/CA in one sentence.

CSMA/CD detects and reacts to a collision after transmission begins, while CSMA/CA tries to reduce collision probability before transmission and uses acknowledgments to confirm delivery.

### Q12. Does a missing ACK always mean that a collision happened?

No. A missing ACK can also result from interference, noise, fading, range, receiver failure, or other transmission problems.

---

## 🔮 Tomorrow's Preview

- Wireless LAN architecture and IEEE 802.11 frame types
- Access points, stations, BSS, ESS, and roaming
- Wi-Fi authentication, association, and channel selection
