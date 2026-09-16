# Day 29: ARP - Complete Guide

## Table of Contents
1. [Introduction to ARP](#introduction-to-arp)
2. [What is ARP?](#what-is-arp)
3. [Need for ARP](#need-for-arp)
4. [ARP in OSI and TCP/IP Models](#arp-in-osi-and-tcpip-models)
5. [How ARP Works](#how-arp-works)
6. [ARP Request and ARP Reply](#arp-request-and-arp-reply)
7. [ARP Packet Format](#arp-packet-format)
8. [ARP Table / ARP Cache](#arp-table--arp-cache)
9. [Types of ARP](#types-of-arp)
10. [ARP vs RARP vs ICMP](#arp-vs-rarp-vs-icmp)
11. [Proxy ARP](#proxy-arp)
12. [Gratuitous ARP](#gratuitous-arp)
13. [ARP Spoofing / ARP Poisoning](#arp-spoofing--arp-poisoning)
14. [Advantages of ARP](#advantages-of-arp)
15. [Disadvantages / Security Issues](#disadvantages--security-issues)
16. [Important Exam Points](#important-exam-points)
17. [Quick Summary](#quick-summary)

---

## Introduction to ARP

**ARP** stands for **Address Resolution Protocol**.

It is used in local area networks (LANs) to map an **IP address** to the corresponding **MAC address**.

Whenever a device wants to send data to another device on the same network, it needs the destination device's **MAC address**. But the application or IP layer only knows the **IP address**, not the hardware address. That is where ARP helps.

In simple words:
- IP address = logical address
- MAC address = physical address
- ARP = finds the physical address using the logical address

---

## What is ARP?

ARP is a protocol used at the **Data Link Layer** of the OSI model and is closely related to the **Network Access Layer** in the TCP/IP model.

It is a **link-layer protocol** that resolves logical addresses into physical addresses.

### Full Form
- **A**ddress
- **R**esolution
- **P**rotocol

### Key Facts
- **Layer**: Data Link Layer (OSI)
- **Used for**: IP to MAC address resolution
- **Scope**: Local network only
- **Works on**: Ethernet, Wi-Fi, LANs
- **Main Purpose**: To find the MAC address of a host whose IP address is known

---

## Need for ARP

Suppose a PC wants to send data to another PC with IP address `192.168.1.20`.

The IP layer knows the destination IP address, but the Data Link layer requires the destination **MAC address** to send the frame.

If the sender does not know the destination MAC, it cannot create a proper Ethernet frame.

So, ARP is needed to answer:

> "Who has this IP address? Please tell me your MAC address."

This is the core idea behind ARP.

---

## ARP in OSI and TCP/IP Models

### In OSI Model
ARP belongs to the **Data Link Layer** because it helps deliver frames using physical addresses.

### In TCP/IP Model
ARP is associated with the **Network Access Layer** (Link layer), which is responsible for sending packets over the physical network.

### Role of ARP
ARP does not carry application data. It only carries control information used to discover or map addresses.

---

## How ARP Works

ARP works in a local network using a broadcast message.

### Step-by-step process
1. Host A wants to send data to Host B.
2. Host A knows Host B's IP address but not its MAC address.
3. Host A sends an **ARP Request** message.
4. This request is broadcast to all devices in the LAN.
5. Every device receives the ARP Request.
6. The device whose IP address matches the requested IP sends an **ARP Reply**.
7. Host A receives the reply and stores the IP-to-MAC mapping in its **ARP cache**.
8. Host A now sends the actual data frame to Host B using the MAC address.

### Example
Suppose:
- Host A: `192.168.1.10` / `AA:BB:CC:DD:EE:11`
- Host B: `192.168.1.20` / `11:22:33:44:55:66`

Host A sends:

> Who has 192.168.1.20? Tell 192.168.1.10

Host B responds:

> 192.168.1.20 is at 11:22:33:44:55:66

Then Host A stores this mapping.

---

## ARP Request and ARP Reply

### 1. ARP Request
The sender broadcasts an ARP request asking:

- Who has this IP address?
- Tell me your MAC address

The request typically contains:
- Sender IP address
- Sender MAC address
- Target IP address
- Target MAC address (usually unknown/empty)

### 2. ARP Reply
The intended device responds with its MAC address.

This reply is usually **unicast** (sent only to the requester), not broadcast.

### ARP Request/Reply Flow

```text
Host A                     LAN                     Host B
 |                           |                        |
 |-- ARP Request -------->  |                        |
 |                           |-- broadcast to all --> |
 |                           |                        |
 |                           |<-- ARP Reply --------- |
 |<-- ARP Reply ----------- |                        |
 |                        Data frame sent to Host B using MAC |
```

---

## ARP Packet Format

An ARP packet contains the address information needed to resolve IP to MAC.

### ARP Packet Structure

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Hardware Type | Protocol Type | HW Addr Length| Prot Addr Length |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Operation (1=request, 2=reply)                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Sender Hardware Address (MAC)                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Sender Protocol Address (IP)                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Target Hardware Address (MAC)                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Target Protocol Address (IP)                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Fields Explanation

| Field | Meaning |
|-------|---------|
| **Hardware Type** | Type of physical network (e.g., Ethernet = 1) |
| **Protocol Type** | Protocol being resolved (e.g., IPv4 = 0x0800) |
| **Hardware Length** | Length of MAC address (6 for Ethernet) |
| **Protocol Length** | Length of IP address (4 for IPv4) |
| **Operation** | 1 = ARP Request, 2 = ARP Reply |
| **Sender Hardware Address** | Source MAC address |
| **Sender Protocol Address** | Source IP address |
| **Target Hardware Address** | Destination MAC address (usually blank in request) |
| **Target Protocol Address** | Destination IP address |

### Example Values
- Ethernet hardware type: `1`
- IPv4 protocol type: `0x0800`
- MAC length: `6`
- IP length: `4`

---

## ARP Table / ARP Cache

Each device maintains an **ARP table** (also called **ARP cache**).

This table stores mappings between:
- IP addresses
- MAC addresses

### Example ARP Table

```text
Interface  IP Address        MAC Address
Ethernet0  192.168.1.1      00:1A:2B:3C:4D:5E
Ethernet0  192.168.1.10     AA:BB:CC:DD:EE:11
Ethernet0  192.168.1.20     11:22:33:44:55:66
```

### Purpose of ARP Cache
- Reduce repeated broadcasts
- Speed up communication
- Avoid sending ARP requests again and again

### Cache Timeout
ARP entries are not permanent. They usually expire after a short time and are refreshed if needed.

---

## Types of ARP

### 1. Unicast ARP
Used when the sender already knows the target's MAC address or when the reply is sent to a specific host.

### 2. Broadcast ARP
Used for ARP request. The request is sent to all hosts in the LAN.

### 3. Proxy ARP
A router answers the ARP request on behalf of another host or network.

### 4. Gratuitous ARP
A host sends an ARP message without being asked, usually to announce a new IP/MAC mapping or detect IP conflicts.

### 5. Reverse ARP (RARP)
RARP is used to find the IP address when the MAC address is known. It is mostly obsolete now and is replaced by DHCP.

---

## ARP vs RARP vs ICMP

### ARP vs RARP
| Protocol | Purpose |
|---------|---------|
| **ARP** | Finds MAC from IP |
| **RARP** | Finds IP from MAC |

RARP was used in older systems, but DHCP is more common today.

### ARP vs ICMP
| Protocol | Purpose |
|---------|---------|
| **ARP** | Maps IP to MAC in local network |
| **ICMP** | Sends error and control messages between hosts and routers |

ARP is for address discovery, while ICMP is for connectivity and error reporting.

---

## Proxy ARP

**Proxy ARP** is a technique in which a router responds to an ARP request on behalf of a different host or network.

### Why is it used?
- When a host is on a different subnet but still needs to communicate through a router
- For legacy networks or special network configurations

### Example
A host on one subnet asks:

> Who has 192.168.2.10?

A router replies with its own MAC address, so the host sends the packet to the router, which then forwards it onward.

### Note
Proxy ARP may be used in subnetting and NAT-related setups, but it also has security concerns.

---

## Gratuitous ARP

A **Gratuitous ARP** is an ARP reply sent by a host without a specific request.

### Common uses
1. Inform other devices of a change in MAC address
2. Detect duplicate IP addresses
3. Refresh ARP entries in the LAN

### Example
If a computer changes its NIC, it sends a gratuitous ARP so all hosts update their ARP tables.

This is especially useful in virtualization and failover systems.

---

## ARP Spoofing / ARP Poisoning

ARP was designed for trust and simplicity, not security. Because devices accept ARP replies without verification, attackers can exploit this.

### What is ARP Spoofing?
An attacker sends false ARP messages to associate their MAC address with another device's IP address.

### Effects
- Man-in-the-middle attack
- Traffic interception
- Data theft
- Denial of service

### Example
An attacker claims:

> IP 192.168.1.1 is at attacker's MAC

Now all traffic intended for the gateway can go to the attacker instead.

### Protection
- Static ARP entries
- Switch port security
- DHCP snooping
- Dynamic ARP Inspection (DAI)
- IDS/IPS tools

---

## Advantages of ARP

- Simple and efficient protocol
- Automatically resolves IP to MAC addresses
- Reduces manual configuration
- Helps devices communicate on the same LAN
- Works in Ethernet-like networks without complex setup

---

## Disadvantages / Security Issues

- No built-in authentication
- Susceptible to ARP spoofing and poisoning
- Can create security vulnerabilities in LANs
- Broadcast traffic increases network load
- Cache poisoning can lead to wrong delivery of packets

---

## Important Exam Points

- **ARP** = Address Resolution Protocol
- It maps **IP address to MAC address**
- It works in the **local network**
- It uses **broadcast** for request and **unicast** for reply
- It helps in delivery of packets in LAN Ethernet networks
- The request asks: **Who has this IP address?**
- The reply contains the **MAC address**
- ARP table stores IP-to-MAC mappings
- ARP is used for **same-subnet communication**
- ARP is not used for internet-wide routing
- ARP can be attacked by **ARP poisoning**
- **Proxy ARP** answers on behalf of another host or network
- **Gratuitous ARP** announces a host's address without request
- **RARP** is reverse of ARP and is mostly obsolete

---

## Quick Summary

ARP is one of the most important protocols in a local network. It resolves the IP address of a destination into its MAC address so that data can be delivered correctly on Ethernet or Wi-Fi networks.

### In one line:
> ARP is used to find the MAC address of a device when its IP address is known.

### Core idea:
- IP address tells “where” logically
- MAC address tells “where” physically
- ARP connects the logical and physical worlds

### Real-life example:
When you open a website or share files on the same network, your device sends ARP requests to find the MAC address of the target device before sending the actual packet.

---

## Final Exam Style Answer

ARP stands for Address Resolution Protocol. It is used in local networks to map an IP address to a MAC address. When a host wants to communicate with another host on the same network, it sends an ARP request in broadcast form asking who has a particular IP address. The device with that IP address replies with its MAC address. ARP is essential because IP addresses are logical addresses, while MAC addresses are required by the Data Link Layer for actual frame delivery. ARP entries are stored in the ARP cache for faster future communication. ARP is vulnerable to spoofing and poisoning, which can lead to traffic interception. Therefore, secure network measures such as static ARP entries, DHCP snooping, and Dynamic ARP Inspection are used to protect against ARP attacks.

---

If you want, I can also make this into a shorter “exam notes” version or a 1-page revision sheet.
