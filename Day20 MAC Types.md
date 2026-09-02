# 🌐 Day 20: Types of MAC Protocols

## 📝 Overview

Media Access Control (MAC) protocols define how multiple devices share a common communication channel. They are part of the MAC sublayer of the Data Link Layer (Layer 2).

The main goal is to provide an organized method for transmitting frames while reducing collisions, delay, and wasted bandwidth.

MAC protocols are divided into three major types:

1. **Random access protocols**: devices compete for the channel.
2. **Controlled access protocols**: devices take turns according to an access rule.
3. **Channelization protocols**: the channel is divided among users.

---

## 🎯 Learning Objectives

By the end of this note, you should be able to:

- classify MAC protocols into their three main families
- explain Pure ALOHA, Slotted ALOHA, and CSMA
- compare CSMA/CD with CSMA/CA
- describe reservation, polling, and token passing
- differentiate FDMA, TDMA, and CDMA
- select an appropriate MAC method for a network scenario
- solve basic ALOHA throughput questions

---

## 🧠 MAC Types Cheat Sheet

| Type | Access decision | Collision possibility | Typical examples |
|---|---|---:|---|
| Random access | Each station contends for access | Yes | ALOHA, CSMA, CSMA/CD, CSMA/CA |
| Controlled access | A rule or controller grants access | Very low or none | Reservation, polling, token passing |
| Channelization | Each station receives a frequency, time, or code share | Low when synchronized correctly | FDMA, TDMA, CDMA |

---

## 1) Random Access Protocols

In a random access protocol, no station has a permanent turn. A station transmits when it has a frame. If another station transmits at the same time, a collision may occur.

### General operation

1. A station gets a frame to send.
2. It accesses or senses the channel according to the protocol.
3. It transmits the frame.
4. It detects success, detects a collision, or waits for an acknowledgment.
5. After failure, it waits for a backoff period and tries again.

### Advantages

- simple and decentralized
- works well when traffic is light or bursty
- no central controller is required
- new stations can join without receiving a fixed slot

### Disadvantages

- collisions waste bandwidth
- delay is unpredictable
- performance decreases as the number of active stations increases
- fairness depends on the backoff algorithm

---

## 2) ALOHA

ALOHA is the simplest random access protocol. A station transmits immediately whenever it has data.

### Pure ALOHA

- a station may transmit at any time
- a collision occurs if another frame overlaps it
- after a collision, the station waits a random time and retransmits
- acknowledgment and timeout are normally used to identify failure

For a frame duration of $T$, a frame is vulnerable to collisions during $2T$. The maximum theoretical throughput is:

$$S = Ge^{-2G}$$

where $G$ is the offered traffic. The maximum throughput is approximately **18.4%** at $G = 0.5$.

### Slotted ALOHA

- time is divided into equal slots
- a station may transmit only at the beginning of a slot
- stations must be synchronized
- the vulnerable period is reduced from $2T$ to $T$

Its throughput is:

$$S = Ge^{-G}$$

The maximum theoretical throughput is approximately **36.8%** at $G = 1$.

### Pure ALOHA vs Slotted ALOHA

| Feature | Pure ALOHA | Slotted ALOHA |
|---|---|---|
| Transmission time | Any time | Slot boundary only |
| Synchronization | Not required | Required |
| Vulnerable period | $2T$ | $T$ |
| Maximum throughput | 18.4% | 36.8% |
| Collision probability | Higher | Lower |

---

## 3) CSMA

CSMA means **Carrier Sense Multiple Access**. A station listens to the channel before transmitting.

Carrier sensing reduces collisions, but it cannot eliminate them. Two stations may both sense an idle channel and transmit before either station hears the other signal. This delay is called **propagation delay**.

### CSMA persistence methods

| Method | Behavior when channel is busy | Behavior when channel becomes idle |
|---|---|---|
| 1-persistent | Keep sensing continuously | Transmit immediately with probability 1 |
| Non-persistent | Wait a random time instead of continuously sensing | Sense again later |
| p-persistent | Used with slotted channels | Transmit with probability $p$; defer with probability $1-p$ |

### Trade-off

- **1-persistent CSMA** has low waiting time but can create collisions when many stations are waiting.
- **Non-persistent CSMA** reduces collisions but can increase delay.
- **p-persistent CSMA** balances transmission and waiting using probability $p$.

---

## 4) CSMA/CD

CSMA/CD means **Carrier Sense Multiple Access with Collision Detection**. It was used by traditional shared, half-duplex Ethernet.

### Working steps

1. The station senses the medium.
2. If the medium is busy, it waits.
3. If the medium is idle, it starts transmitting.
4. While transmitting, it monitors the medium for a collision.
5. If a collision occurs, it stops the frame transmission.
6. It sends a jam signal so other stations know about the collision.
7. It waits using **binary exponential backoff**.
8. It retries, up to the protocol's retry limit.

### Binary exponential backoff

After repeated collisions, the station chooses a random waiting time from an increasingly large range. This lowers the chance that the same stations collide again.

### Important note

Modern full-duplex switched Ethernet normally has a dedicated link between a device and a switch, so collisions do not occur and CSMA/CD is not needed. It is mainly important for understanding classic shared Ethernet.

---

## 5) CSMA/CA

CSMA/CA means **Carrier Sense Multiple Access with Collision Avoidance**. It is used by IEEE 802.11 wireless LANs.

### Working steps

1. A station senses the wireless channel.
2. If the channel is busy, it waits.
3. If the channel is idle for the required inter-frame space, it starts a random backoff countdown.
4. The countdown pauses if the channel becomes busy.
5. When the countdown reaches zero, the station transmits.
6. The receiver sends an acknowledgment if the frame arrives correctly.
7. If no acknowledgment arrives, the sender assumes failure and retries after a larger backoff.

### RTS/CTS

For some transmissions, the sender first sends **Request to Send (RTS)**. The receiver answers with **Clear to Send (CTS)**. Nearby stations that hear the reservation defer their transmissions.

RTS/CTS can reduce the hidden-terminal problem, but it adds control-frame overhead and is not required for every frame.

### Why wireless uses avoidance

A wireless station generally cannot transmit and listen for a collision as reliably as a wired Ethernet station. Signal strength, interference, hidden terminals, and the wireless half-duplex medium make collision detection impractical.

---

## 6) Controlled Access Protocols

Controlled access protocols coordinate stations so that only an allowed station transmits at a time.

### A) Reservation

Stations first reserve future transmission opportunities. A reservation phase can use bits or mini-slots, followed by a data phase in which reserved stations transmit.

**Advantages:** collisions are avoided during the data phase and access can be fair.

**Disadvantages:** reservation overhead is wasteful when few stations have data, and synchronization is required.

### B) Polling

A central controller, called the **primary**, asks each station, called a **secondary**, whether it has data to send.

**Advantages:** simple coordination, orderly access, and no data-frame collisions.

**Disadvantages:** the controller is a single point of failure, polling messages consume bandwidth, and idle stations add delay.

Polling is useful when a network needs centralized control or predictable access.

### C) Token Passing

A short control frame called a **token** circulates among stations. Only the station holding the token may transmit.

**Advantages:** collision-free access, fair turns, and predictable waiting time.

**Disadvantages:** token loss or duplication must be handled, a failed station or link can affect the logical sequence, and token management creates overhead.

Examples include legacy Token Ring and Token Bus systems. Token passing is also used as a general access-control idea in some industrial networks.

### Controlled access comparison

| Method | Who grants access? | Main risk | Best characteristic |
|---|---|---|---|
| Reservation | Reservation phase | Reservation overhead | Planned, fair access |
| Polling | Central controller | Controller failure and delay | Centralized management |
| Token passing | Current token holder | Lost or duplicated token | Distributed, deterministic access |

---

## 7) Channelization Protocols

Channelization, also called **channel partitioning** or **fixed assignment**, divides one shared channel among multiple users. The division may be by frequency, time, or code.

### A) FDMA

**Frequency Division Multiple Access** assigns every user a separate frequency band.

- users transmit at the same time on different frequencies
- guard bands may separate adjacent channels
- common in older analog cellular and satellite systems
- a user’s unused frequency remains wasted unless dynamic allocation is supported

### B) TDMA

**Time Division Multiple Access** assigns users different time slots on the same frequency.

- users take turns in repeating frames
- tight clock synchronization is required
- guard time may be needed between slots
- an unused slot may waste capacity in fixed TDMA

### C) CDMA

**Code Division Multiple Access** lets users transmit at the same time and in the same frequency band using different spreading codes.

- the receiver uses the intended code to recover the signal
- users appear as interference to one another
- codes should have suitable correlation properties
- power control is important because a nearby strong signal can overpower a distant weak signal

### Channelization comparison

| Method | Resource divided | Simultaneous users | Main requirement |
|---|---|---:|---|
| FDMA | Frequency | Yes, on separate bands | Frequency separation and guard bands |
| TDMA | Time | No, users take slots | Synchronization and guard time |
| CDMA | Code | Yes, same band | Code separation and power control |

---

## 8) Random Access vs Controlled Access vs Channelization

| Feature | Random access | Controlled access | Channelization |
|---|---|---|---|
| Access style | Competition | Permission or turn-taking | Preassigned share |
| Collision handling | Detect, avoid, or retransmit | Usually prevented | Usually prevented by separation |
| Control | Distributed | Centralized or distributed | Planning and synchronization |
| Delay | Variable | More predictable | Predictable for assigned resources |
| Efficiency under heavy load | Often decreases | Usually remains orderly | High if allocation is well used |
| Efficiency under light or bursty load | Often good | Control overhead may dominate | Fixed allocations may be wasted |
| Examples | Wi-Fi, classic Ethernet | Token Ring, polling | Cellular and satellite systems |

---

## 9) Choosing a MAC Type

- Use **random access** when stations are numerous, traffic is unpredictable, and decentralized operation is valuable.
- Use **controlled access** when fairness, collision-free transmission, or bounded delay is important.
- Use **channelization** when users need simultaneous access to a broad shared resource, as in cellular communication.
- Use **CSMA/CD** only as a classic shared-Ethernet concept; modern switched full-duplex Ethernet does not normally need it.
- Use **CSMA/CA** for wireless LAN contention, with RTS/CTS when hidden terminals make it useful.

---

## 📌 Important Definitions

- **Contention:** competition among stations for access to a shared medium.
- **Collision:** overlapping transmissions that corrupt frames.
- **Backoff:** a waiting period selected before retransmission.
- **Propagation delay:** time for a signal to travel through the medium.
- **Carrier sensing:** listening to determine whether a channel is busy.
- **Token:** a control frame that grants transmission permission.
- **Vulnerable period:** time during which another transmission can cause a collision.
- **Guard band:** unused frequency space that reduces interference between FDMA channels.
- **Guard time:** unused time between TDMA slots that prevents overlap.

---

## 🛠️ Practical Windows Commands

1. `getmac` — displays MAC addresses of local network adapters.
2. `ipconfig /all` — shows adapter details, including the physical MAC address.
3. `arp -a` — displays cached local IP-to-MAC address mappings.
4. `netsh wlan show interfaces` — shows the connected Wi-Fi interface and radio details.

These commands inspect MAC addressing and interfaces; they do not change which MAC access protocol a network uses.

---

## 📝 Example Questions and Answers

**Q1. What are the three main types of MAC protocols?**

**Answer:** Random access, controlled access, and channelization.

**Q2. Which ALOHA version has better maximum throughput?**

**Answer:** Slotted ALOHA, with about 36.8%, compared with Pure ALOHA's 18.4%.

**Q3. Why does CSMA not completely prevent collisions?**

**Answer:** Two stations can sense the medium as idle at nearly the same time and begin transmitting before the other signal reaches them.

**Q4. Why does Wi-Fi use CSMA/CA instead of CSMA/CD?**

**Answer:** Wireless stations cannot reliably detect collisions while transmitting, so they try to avoid collisions and use acknowledgments to detect unsuccessful delivery.

**Q5. Which controlled-access method uses a special control frame?**

**Answer:** Token passing uses a token.

**Q6. What does TDMA divide?**

**Answer:** It divides access by time slots.

**Q7. What does CDMA assign to users?**

**Answer:** It assigns distinct spreading codes.

**Q8. Why is CSMA/CD not normally used in modern switched Ethernet?**

**Answer:** Full-duplex switched links provide separate send and receive paths, so stations do not share one collision domain.

---

## ✅ Quick Revision Notes

- **ALOHA:** transmit first, recover after collision.
- **CSMA:** listen before transmitting.
- **CSMA/CD:** detect collisions in classic wired shared Ethernet.
- **CSMA/CA:** avoid collisions in Wi-Fi.
- **Reservation:** reserve before sending.
- **Polling:** controller asks each station.
- **Token passing:** token holder sends.
- **FDMA:** separate frequencies.
- **TDMA:** separate time slots.
- **CDMA:** separate codes.
- Pure ALOHA maximum throughput is **18.4%**.
- Slotted ALOHA maximum throughput is **36.8%**.

---

## 🔜 Tomorrow's Preview

Day 21 will cover Ethernet frame format, IEEE 802.3 standards, MAC frame fields, frame size, and how switches process Ethernet frames.