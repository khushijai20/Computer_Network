# 🌐 Day 21: CSMA and Its Types

## 📝 Overview

**CSMA (Carrier Sense Multiple Access)** is a contention-based Media Access Control protocol. Before transmitting, a station listens to the shared channel to check whether another station is already transmitting.

CSMA reduces collisions compared with ALOHA, but it cannot remove them completely. Two stations may sense an idle channel at nearly the same time and begin transmitting before either signal reaches the other. This happens because of **propagation delay**.

CSMA is mainly studied in three persistence forms:

1. **1-persistent CSMA**
2. **Non-persistent CSMA**
3. **p-persistent CSMA**

Two important extensions are:

- **CSMA/CD**: collision detection, used by traditional shared Ethernet
- **CSMA/CA**: collision avoidance, used by IEEE 802.11 wireless LANs

---

## 📚 Topics Covered

- Carrier sensing and multiple access
- CSMA operation
- Propagation delay and the collision window
- 1-persistent, non-persistent, and p-persistent CSMA
- CSMA/CD and binary exponential backoff
- CSMA/CA, inter-frame spaces, ACKs, and random backoff
- RTS/CTS and hidden and exposed terminals
- CSMA comparison with ALOHA
- Advantages, limitations, examples, and exam questions

---

## 🎯 Learning Objectives

By the end of this note, you should be able to:

- define CSMA and explain why it is called listen before talk
- describe how carrier sensing reduces collisions
- compare the three CSMA persistence methods
- explain why collisions can still occur after sensing an idle channel
- explain the operation of CSMA/CD and CSMA/CA
- distinguish collision detection from collision avoidance
- solve basic propagation-delay and efficiency questions
- identify hidden-terminal and exposed-terminal situations

---

## 🧠 CSMA Cheat Sheet

| Protocol | Main decision | Typical use | Important limitation |
|---|---|---|---|
| 1-persistent CSMA | Transmit immediately when the channel becomes idle | Simple shared wired networks | Many waiting stations may collide |
| Non-persistent CSMA | If busy, wait a random time before sensing again | Networks where fewer collisions are preferred | More delay and idle time |
| p-persistent CSMA | In a slot, transmit with probability $p$ | Slotted channels | Requires slot synchronization |
| CSMA/CD | Sense, transmit, and detect a collision while transmitting | Traditional half-duplex Ethernet | Not practical for normal wireless links |
| CSMA/CA | Sense, wait, and reduce the chance of collision | IEEE 802.11 Wi-Fi | Collisions can still occur; control overhead exists |

---

## 1) Meaning of CSMA

**CSMA** expands to **Carrier Sense Multiple Access**.

- **Carrier Sense**: a station listens to the channel and checks whether a carrier signal is present.
- **Multiple Access**: many stations share the same physical medium.

A station with a frame normally follows this sequence:

1. It has a frame ready.
2. It senses the channel.
3. If the channel is busy, it follows the protocol's waiting rule.
4. If the channel is idle, it transmits or schedules transmission.
5. It checks for success or failure.
6. After failure, it waits for a backoff interval and retries.

CSMA is a **random access** or **contention-based** protocol. Stations compete for access instead of receiving a permanent turn.

---

## 2) Why Carrier Sensing Helps

In ALOHA, a station may transmit without checking the medium. In CSMA, a station first listens. If it hears an existing transmission, it postpones its own frame.

This prevents many collisions, especially when propagation delay is small compared with frame transmission time.

### ALOHA versus CSMA

| Feature | ALOHA | CSMA |
|---|---|---|
| Channel check before transmission | No | Yes |
| Collision reduction | Low | Better |
| Collision elimination | No | No |
| Main reason for collision | Overlapping independent attempts | Nearly simultaneous sensing and propagation delay |
| Channel utilization | Generally lower | Generally higher |
| Required feedback | ACK or timeout | ACK, collision detection, or timeout depending on type |

### Important limitation

Carrier sensing is based on what a station can hear at its location. A station may hear silence even though another station is transmitting elsewhere. Also, a second station may start during the time before the first signal propagates across the network.

---

## 3) Propagation Delay and Collision Window

**Propagation delay** is the time required for a signal to travel from one station to another.

Suppose Station A senses an idle channel and begins transmitting. Before A's signal reaches distant Station B, B may also sense the channel as idle and transmit. The two frames then collide.

```text
A starts       A's signal reaches B
|----------------------->|
      propagation delay
       B may start here

A frame and B frame overlap: collision
```

### Key rule

The shorter the propagation delay compared with the frame transmission time, the more effective CSMA becomes.

A useful normalized measure is:

$$a = \frac{\text{propagation delay}}{\text{frame transmission time}}$$

A small $a$ usually means fewer collision opportunities and better efficiency. A large $a$ means that a station may transmit for a significant time before discovering that another transmission began elsewhere.

---

## 4) Persistence Methods

A **persistence method** defines what a station does after sensing the channel. It controls the balance between delay, collisions, and idle time.

### A) 1-persistent CSMA

Operation:

1. Sense the channel continuously.
2. If the channel is busy, keep sensing.
3. When the channel becomes idle, transmit immediately with probability 1.
4. If a collision occurs, wait according to a backoff algorithm and retry.

**Why it is called 1-persistent:** the station transmits with probability 1 as soon as the channel becomes idle.

**Advantages:**

- low waiting time when only one station is waiting
- simple to implement
- good performance under light traffic

**Disadvantages:**

- if many stations are waiting, they all transmit as soon as the channel becomes idle
- that simultaneous reaction can produce a collision
- repeated collisions can occur under heavy load

### B) Non-persistent CSMA

Operation:

1. Sense the channel.
2. If the channel is idle, transmit.
3. If the channel is busy, do not keep sensing continuously.
4. Wait for a random time.
5. Sense the channel again and repeat the process.

**Advantages:**

- reduces the chance that many stations transmit at the same instant
- performs better than 1-persistent CSMA under heavy contention
- reduces repeated collisions

**Disadvantages:**

- random waiting increases delay
- the channel may become idle while a station is still waiting
- utilization can fall when traffic is light

### C) p-persistent CSMA

p-persistent CSMA is used with a **slotted** channel. When the station finds the channel idle at a slot boundary:

- it transmits with probability $p$
- it defers by one slot with probability $1-p$
- it repeats the decision in the next slot if the channel remains idle

If the channel is busy, the station waits until it becomes idle and then applies the probability rule.

**Special cases:**

- $p = 1$ behaves like 1-persistent access at slot boundaries
- a smaller $p$ reduces simultaneous transmissions but increases waiting

**Advantages:**

- balances collision probability and delay
- useful when time slots are available
- several waiting stations do not necessarily transmit together

**Disadvantages:**

- requires synchronization
- choosing a suitable $p$ depends on traffic conditions
- performance worsens if the channel is not properly slotted

### Persistence comparison

| Property | 1-persistent | Non-persistent | p-persistent |
|---|---|---|---|
| Channel type | Continuous or shared channel | Continuous or shared channel | Slotted channel |
| Busy channel action | Keep sensing | Random wait | Wait for a slot decision |
| Idle channel action | Transmit immediately | Transmit | Transmit with probability $p$ |
| Collision risk | High with many waiting stations | Lower | Controlled by $p$ |
| Delay | Usually low | Usually higher | Adjustable |
| Synchronization | Not required | Not required | Required |

---

## 5) CSMA/CD

**CSMA/CD** means **Carrier Sense Multiple Access with Collision Detection**. It extends CSMA by checking the medium during transmission.

It was used by traditional shared, half-duplex Ethernet. The abbreviation means:

- **Carrier Sense**: listen before transmitting
- **Multiple Access**: many stations share the medium
- **Collision Detection**: monitor the medium while sending and stop quickly if a collision is detected

### CSMA/CD operation

1. A station has a frame to send.
2. It senses the channel.
3. If the channel is busy, it waits.
4. If the channel is idle, it begins transmitting.
5. While transmitting, it continues monitoring the medium.
6. If no collision is detected, it completes the frame.
7. If a collision is detected, it stops transmitting immediately.
8. It sends a **jam signal** so other stations recognize the collision.
9. It waits for a random backoff time.
10. It retransmits, subject to the retry limit.

Stopping early saves bandwidth compared with transmitting the entire damaged frame.

### Why a jam signal is used

A collision may be too brief for every station to recognize reliably. The jam signal extends the collision notification so all participating stations know that the frame failed and must be retried.

### Binary exponential backoff

After the first collision, a station chooses a random number of slot times from a small range. After more collisions, the range grows, reducing the chance that the same stations choose the same retry time.

After collision number $k$, the usual choice is:

$$K \in \{0, 1, 2, \ldots, 2^k - 1\}$$

The actual standard applies a maximum backoff range and a retry limit. The general idea is exponential growth, not an unlimited range.

### CSMA/CD timeline

```text
Sense -> transmit -> monitor
                     |
              collision detected?
                 /             \
               No               Yes
               |                 |
        finish the frame   stop + jam signal
                                    |
                              random backoff
                                    |
                                  retry
```

### Ethernet notes

- A collision can be detected only while a station is still transmitting.
- The frame must be long enough, relative to the network's maximum round-trip propagation time, for a collision to be detected before transmission ends.
- This requirement influenced the minimum Ethernet frame size in classic shared Ethernet.
- Modern switched Ethernet usually uses full-duplex links, so each link has separate transmit and receive paths and collisions do not occur.
- Therefore, CSMA/CD is historically important but is normally inactive in modern full-duplex Ethernet.

---

## 6) CSMA/CA

**CSMA/CA** means **Carrier Sense Multiple Access with Collision Avoidance**. It is the access method associated with IEEE 802.11 wireless LANs.

A wireless station generally cannot detect collisions as reliably as a wired Ethernet station because it cannot transmit and listen to its own signal with the same accuracy, and other stations may be hidden from it. Therefore, Wi-Fi tries to reduce the probability of collision before transmitting.

### Basic CSMA/CA operation

1. The station senses the wireless channel.
2. If the channel is busy, it waits.
3. If the channel is idle for the required inter-frame space, it chooses a random backoff counter.
4. The counter decreases only while the channel remains idle.
5. If another station starts transmitting, the counter freezes.
6. When the channel becomes idle again, the countdown resumes.
7. When the counter reaches zero, the station transmits.
8. The receiver sends an ACK after a correct frame.
9. If the ACK is not received, the sender assumes a collision, interference, or loss.
10. The sender increases its contention window and retries later.

### Why the countdown freezes

Freezing the counter when the channel becomes busy prevents a station from transmitting in the middle of another frame. It also preserves the waiting progress already made.

### Inter-frame spaces

An **inter-frame space (IFS)** is a required quiet interval between wireless transmissions. Different frame types may use different IFS lengths. A shorter IFS can give a high-priority response an earlier opportunity to transmit.

### ACKs in CSMA/CA

Wireless senders normally rely on an acknowledgment because they cannot safely infer success by listening for a collision. If the sender does not receive the ACK within the expected time, it retransmits after another backoff.

An ACK confirms that the receiver decoded the frame. It does not mean that every station in the area heard it.

---

## 7) RTS/CTS and Wireless Problems

### Hidden-terminal problem

Two stations can both reach an access point but cannot hear each other. They may each sense the channel as idle and transmit to the access point at the same time.

```text
A  ---->  Access Point  <----  B
 \                         /
  A and B cannot hear each other
```

Their frames collide at the access point even though each station sensed silence locally.

### RTS/CTS solution

**RTS (Request to Send)** and **CTS (Clear to Send)** reserve the medium before the data frame:

1. Sender transmits RTS.
2. Receiver answers CTS.
3. Nearby stations that hear RTS or CTS defer for the announced duration.
4. Sender transmits the data frame.
5. Receiver sends an ACK.

RTS/CTS can reduce hidden-terminal collisions, especially for long frames or networks with many hidden stations.

**Cost:** RTS and CTS add extra control frames and delay. Therefore, they may not be worthwhile for every short frame.

### Exposed-terminal problem

A station may hear another nearby transmission and unnecessarily remain silent even though its own transmission would not interfere with the intended receiver. This reduces spatial reuse.

RTS/CTS can sometimes help distinguish whether a transmission actually prevents another exchange, but it cannot eliminate every wireless performance problem.

### Capture effect

Sometimes a receiver successfully decodes a stronger signal even when a weaker signal overlaps it. This is called the **capture effect**. It means that not every overlap necessarily destroys every frame in a real wireless channel, although basic CSMA analysis often assumes collisions cause failure.

---

## 8) CSMA/CD versus CSMA/CA

| Feature | CSMA/CD | CSMA/CA |
|---|---|---|
| Full name | Collision Detection | Collision Avoidance |
| Main environment | Traditional shared Ethernet | Wireless LANs |
| During transmission | Monitors for collision | Tries to avoid simultaneous transmission |
| Failure indication | Collision signal or detection | Missing ACK or failure feedback |
| Backoff | Usually binary exponential backoff | Random contention-window backoff |
| Jam signal | Used | Not used in the same way |
| RTS/CTS | Not normally needed | Optional mechanism for hidden terminals |
| Why chosen | Wired station can detect collision | Wireless station cannot reliably detect collision |
| Modern status | Mostly replaced by switched full-duplex Ethernet | Still fundamental to Wi-Fi access |

### Important exam sentence

**CSMA/CD detects a collision after transmission begins, while CSMA/CA attempts to avoid a collision before transmission begins.**

---

## 9) ALOHA, CSMA, CSMA/CD, and CSMA/CA

| Protocol | Senses first? | Detects collision while sending? | Uses ACK? | Typical setting |
|---|---:|---:|---:|---|
| Pure ALOHA | No | No | Usually | Simple random-access systems |
| Slotted ALOHA | No | No | Usually | Synchronized random-access systems |
| CSMA | Yes | Depends on extension | Depends on system | Shared channels |
| CSMA/CD | Yes | Yes | Not the main failure mechanism | Classic shared Ethernet |
| CSMA/CA | Yes | Generally no | Yes | Wi-Fi |

Carrier sensing makes CSMA more efficient than basic ALOHA under many conditions, but it adds sensing rules and still depends on propagation delay and backoff.

---

## 10) Numerical Examples

### Example 1: Propagation delay

A signal travels at $2 \times 10^8$ meters per second through a cable of length $1,000$ meters. Find the one-way propagation delay.

$$t_p = \frac{\text{distance}}{\text{propagation speed}}$$

$$t_p = \frac{1000}{2 \times 10^8} = 5 \times 10^{-6}\text{ seconds}$$

**Answer:** The one-way propagation delay is **5 microseconds**. The round-trip delay is approximately **10 microseconds**.

### Example 2: Normalized propagation delay

A frame takes $100$ microseconds to transmit, and the one-way propagation delay is $5$ microseconds.

$$a = \frac{5}{100} = 0.05$$

**Answer:** $a = 0.05$. The propagation delay is small relative to frame time, which is favorable for CSMA efficiency.

### Example 3: Binary exponential backoff

A station has experienced three collisions. Ignoring the standard maximum range, it chooses:

$$K \in \{0,1,2,3,4,5,6,7\}$$

**Answer:** It randomly selects one of eight slot counts and waits that many slot times before retrying.

### Example 4: p-persistent decision

A station uses p-persistent CSMA with $p = 0.25$. When the channel is idle at a slot boundary, it:

- transmits with probability $0.25$
- defers by one slot with probability $0.75$

**Answer:** It does not always transmit immediately. The probability rule reduces simultaneous transmissions by spreading attempts across slots.

---

## 11) Advantages and Disadvantages

### Advantages of CSMA

- reduces unnecessary collisions by sensing before transmission
- decentralized and does not require a central controller
- works well for bursty traffic
- adapts to the number of active stations
- can be combined with detection, avoidance, acknowledgments, and backoff

### Disadvantages of CSMA

- carrier sensing cannot remove collisions completely
- propagation delay creates a collision window
- performance decreases under heavy contention
- backoff makes delay unpredictable
- a station may sense silence even when a distant station is transmitting
- sensing and control logic add implementation complexity

### Advantages of CSMA/CD

- detects collisions early
- stops damaged transmissions instead of wasting the entire frame time
- binary exponential backoff adapts to contention

### Limitations of CSMA/CD

- requires the ability to detect a collision while transmitting
- unsuitable for normal wireless operation
- collision handling still wastes some bandwidth
- largely unnecessary on modern full-duplex switched Ethernet

### Advantages of CSMA/CA

- suitable for wireless shared media
- random backoff reduces simultaneous transmissions
- ACKs provide explicit delivery feedback
- RTS/CTS can help with hidden terminals

### Limitations of CSMA/CA

- cannot guarantee that collisions never happen
- backoff and inter-frame spaces add delay
- RTS/CTS adds overhead
- hidden and exposed terminals remain possible
- interference and signal fading can cause a missing ACK even without a collision

---

## 12) Common Misconceptions

### Misconception 1: CSMA guarantees collision-free transmission

False. Two stations can sense an idle channel at almost the same time. Propagation delay can cause them to transmit together.

### Misconception 2: CSMA/CD and CSMA/CA are the same

False. CSMA/CD detects collisions during transmission. CSMA/CA tries to reduce collisions before transmission and normally uses ACKs to infer success.

### Misconception 3: A busy channel means every station everywhere is transmitting

False. Carrier sensing is local. A station may not hear a distant or blocked transmission.

### Misconception 4: Wireless devices use CSMA/CD just like old Ethernet

Usually false. Wireless stations generally use CSMA/CA because reliable collision detection during their own transmission is difficult.

### Misconception 5: RTS/CTS is always required for Wi-Fi

False. It is optional and is mainly useful when hidden terminals or large frames make its overhead worthwhile.

### Misconception 6: A missing Wi-Fi ACK always proves a collision

False. The frame may have been lost because of interference, fading, noise, or a receiver problem.

### Misconception 7: Full-duplex switched Ethernet needs CSMA/CD

Normally false. A dedicated full-duplex link has no shared collision domain, so collisions do not occur during normal operation.

---

## 13) Important Definitions

- **Carrier**: the signal or energy that indicates activity on a communication medium.
- **Carrier sensing**: listening to the medium before attempting transmission.
- **Contention**: competition among stations for access to a shared channel.
- **Propagation delay**: time taken by a signal to travel through the medium.
- **Persistence**: the rule that determines what a station does when the channel is busy or becomes idle.
- **Backoff**: a waiting interval selected before a retransmission.
- **Collision detection**: discovering that two or more transmissions overlap while they are being sent.
- **Collision avoidance**: using waiting, randomization, and reservations to reduce the chance of collision.
- **Jam signal**: a signal used by CSMA/CD to make a collision visible to other stations.
- **Contention window**: the range from which a random wireless backoff value is selected.
- **Hidden terminal**: a station that cannot hear another sender but can interfere at the receiver.
- **Exposed terminal**: a station that unnecessarily defers because it hears a nearby transmission that would not prevent its own transmission.
- **RTS/CTS**: a request-and-clear exchange used to reserve the wireless medium.

---

## 14) Exam Questions and Answers

### Q1. What is CSMA?

CSMA is a MAC protocol in which a station senses the shared channel before transmitting. If the channel is busy, the station waits according to its persistence rule.

### Q2. Why can collision occur in CSMA?

Because of propagation delay. Two stations may sense the channel as idle before either station's signal reaches the other.

### Q3. What is the difference between 1-persistent and non-persistent CSMA?

1-persistent CSMA transmits immediately when the channel becomes idle. Non-persistent CSMA waits a random time after finding the channel busy and then senses again.

### Q4. What is p-persistent CSMA?

It is a slotted CSMA method in which a station transmits with probability $p$ and defers one slot with probability $1-p$ when the channel is idle.

### Q5. Explain CSMA/CD in one sentence.

CSMA/CD senses before transmitting, monitors during transmission, stops when a collision is detected, sends a jam signal, and retries after backoff.

### Q6. Explain CSMA/CA in one sentence.

CSMA/CA senses the wireless medium, waits through an inter-frame space and random backoff, transmits, and uses an ACK to identify likely failure.

### Q7. Why is CSMA/CD not normally used in Wi-Fi?

A wireless station usually cannot reliably detect a collision while transmitting because of its own signal, hidden terminals, interference, and the half-duplex wireless medium.

### Q8. What is the purpose of binary exponential backoff?

It spreads retries over an increasingly larger random time range after repeated collisions, reducing the probability of another immediate collision.

### Q9. What problem does RTS/CTS address?

It mainly reduces hidden-terminal collisions by reserving the channel before the data frame is sent.

### Q10. Which CSMA type is used by traditional Ethernet and Wi-Fi?

Traditional shared half-duplex Ethernet used CSMA/CD. IEEE 802.11 Wi-Fi uses CSMA/CA.

---

## 15) Quick Revision Notes

- CSMA means **listen before talk**.
- Carrier sensing reduces collisions but cannot eliminate them.
- Propagation delay is the main reason collisions can still occur.
- 1-persistent CSMA transmits immediately when idle.
- Non-persistent CSMA waits randomly when busy.
- p-persistent CSMA uses probability $p$ in a slotted channel.
- CSMA/CD detects collisions while transmitting.
- CSMA/CA tries to avoid collisions before transmitting.
- CSMA/CD uses a jam signal and binary exponential backoff.
- CSMA/CA uses random backoff, ACKs, and optionally RTS/CTS.
- Hidden terminals can collide at a receiver without hearing one another.
- Modern full-duplex switched Ethernet normally has no CSMA/CD collisions.

### Memory trick

**CD = Collision Detected** after transmission starts.

**CA = Collision Avoided** as much as possible before transmission starts.

---

## 16) Final Comparison

| Point | 1-persistent CSMA | Non-persistent CSMA | p-persistent CSMA | CSMA/CD | CSMA/CA |
|---|---|---|---|---|---|
| Senses channel | Yes | Yes | Yes | Yes | Yes |
| Main action when busy | Keep sensing | Random wait | Wait for slot | Wait | Freeze or choose backoff |
| Collision handling | Retry after backoff | Retry after backoff | Retry after backoff | Detect, jam, backoff | ACK failure, enlarge window, retry |
| Collision detection | No extension | No extension | No extension | Yes | Usually no |
| Collision avoidance | Basic sensing | Random waiting | Probability $p$ | Basic sensing | Stronger waiting/backoff |
| Typical environment | Shared channel | Shared channel | Slotted shared channel | Classic Ethernet | Wi-Fi |

---

## 🔭 Tomorrow's Preview

The next topic can cover **network devices and internetworking concepts**, including bridges, switches, routers, collision domains, and broadcast domains.
