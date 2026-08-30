# 🌐 Day 18: Error Detection and Correction

## 1) Introduction
In computer networks, data is transmitted through physical media such as cables, wireless signals, or optical fibers. During transmission, bits may be changed due to noise, distortion, interference, or hardware faults.

Because of this, the receiver must check whether the received data is correct. This is done using Error Detection and Error Correction techniques.

---

## 2) Why Errors Occur in Networks
Errors can occur because of:
- electrical noise
- signal attenuation
- interference from other devices
- transmission medium problems
- collisions in shared media
- hardware malfunction

The goal of error control is to keep data reliable and accurate.

---

## 3) Need for Error Control
When data travels from sender to receiver, the receiver must know:
- whether the data is corrupted
- whether the data must be retransmitted
- whether the error can be corrected automatically

This is important because corrupted data can lead to wrong information, failed communication, or broken applications.

---

## 4) Types of Errors
There are mainly three types of errors:

### A) Single-bit error
Only one bit changes.

Example:
- Original: 10101010
- Received: 10111010

### B) Multiple-bit error
Two or more bits are changed.

### C) Burst error
A group of bits in a continuous sequence gets corrupted.

Burst errors are common in real communication channels.

---

## 5) Error Detection
Error detection means detecting whether an error has occurred during transmission.

The sender attaches some redundant information to the data.
The receiver checks this information.

If mismatch occurs, the receiver knows the frame/data is corrupted.

Common methods are:
- Parity Check
- Checksum
- Cyclic Redundancy Check (CRC)

---

## 6) Parity Check
Parity is the simplest form of error detection.

### How it works
An extra bit called a parity bit is added to the data.
The parity bit is chosen so that the total number of 1s becomes either even or odd.

- Even parity: total number of 1s is even
- Odd parity: total number of 1s is odd

### Example
Data: 1011001
Number of 1s = 4 (even)
If even parity is used, parity bit = 0
Final data: 10110010

If one bit changes during transmission, parity becomes wrong and the receiver detects an error.

### Advantages
- easy to implement
- simple to understand

### Disadvantages
- cannot detect all errors
- cannot correct errors
- fails if two bits change

### Key point
Parity can detect only certain errors, especially odd number of bit changes.

---

## 7) Checksum
Checksum is used to detect errors in blocks of data.

### How it works
- Divide the data into fixed-size blocks
- Add the blocks together
- Send the resulting checksum with the data
- Receiver recalculates checksum and compares it

If they differ, an error is detected.

### Advantages
- better than parity check
- detects many common errors

### Disadvantages
- not as strong as CRC
- may not detect all types of errors

### Used in
- TCP/UDP
- network protocols
- data integrity checks

---

## 8) Cyclic Redundancy Check (CRC)
CRC is the most commonly used error detection method in networks.

### How it works
The sender appends a set of redundant bits (CRC bits) to the message.
These bits are generated using polynomial division.

The receiver performs the same calculation.
If the result is not zero, an error is detected.

### Common CRC types
- CRC-8
- CRC-16
- CRC-32

### Used in
- Ethernet
- Wi-Fi
- USB
- Bluetooth
- Data Link Layer protocols

### Advantages
- highly effective
- detects burst errors efficiently
- widely used in real networking

### Disadvantages
- detects errors but does not correct them

### Key point
CRC is the most important error detection method used in modern networks.

---

## 9) Error Correction
Error correction means detecting the error and then correcting it automatically.

This is more complex than error detection because the receiver must decide what the original correct data was.

Common error correction methods:
- Hamming Code
- Forward Error Correction (FEC)
- Reed-Solomon Code

---

## 10) Hamming Code
Hamming code is a popular method for correcting single-bit errors.

### How it works
Extra parity bits are inserted into the data at positions corresponding to powers of 2:
- 1, 2, 4, 8, ...

The receiver checks the parity bits.
If a parity check fails, it identifies the position of the corrupted bit.
Then the receiver flips that bit to correct it.

### Advantages
- detects and corrects single-bit errors
- useful in memory and communication systems

### Disadvantages
- not effective against multiple-bit or burst errors
- extra overhead

### Key point
Hamming code is mainly used for correcting a single-bit error.

---

## 11) Forward Error Correction (FEC)
FEC means the sender adds enough redundancy so the receiver can correct the error without asking for retransmission.

### Example
Used in:
- wireless communication
- satellite links
- deep-space communication

### Advantages
- no retransmission needed
- useful when feedback is slow or impossible

### Disadvantages
- extra data overhead
- more complex implementation

---

## 12) Error Detection vs Error Correction

### Error Detection
- only finds that an error occurred
- examples: parity, checksum, CRC
- often used with retransmission

### Error Correction
- finds and corrects the error
- more complex
- examples: Hamming code, FEC

---

## 13) ARQ (Automatic Repeat Request)
ARQ is used when an error is detected and the receiver asks for retransmission.

### Types of ARQ
- Stop-and-Wait ARQ
- Go-Back-N ARQ
- Selective Repeat ARQ

### How it works
1. Sender sends a frame
2. Receiver checks it
3. If correct, ACK is sent
4. If error, NAK is sent or timeout happens
5. Sender retransmits the frame

### Why it is important
ARQ is used in reliable communication systems to ensure correct delivery.

---

## 14) Stop-and-Wait ARQ
In Stop-and-Wait ARQ:
- sender sends one frame
- waits for acknowledgment
- then sends next frame

### Advantages
- easy to implement

### Disadvantages
- low efficiency
- waiting time is large

---

## 15) Go-Back-N ARQ
The sender can send multiple frames before waiting for ACK.

If one frame is corrupted, all frames after it may be retransmitted.

### Advantages
- better efficiency than stop-and-wait

### Disadvantages
- unnecessary retransmission may happen

---

## 16) Selective Repeat ARQ
Only the corrupted or missing frames are retransmitted.

### Advantages
- more efficient than Go-Back-N
- fewer unnecessary retransmissions

### Disadvantages
- more complex to implement

---

## 17) Difference Between Parity, Checksum, and CRC

### Parity
- simplest error detection
- detects only limited errors
- not very reliable

### Checksum
- better than parity
- used in many protocols

### CRC
- best common method
- detects burst errors efficiently
- used widely in Data Link Layer and networking

---

## 18) Practical Example
Suppose a file is sent over a LAN:
- data is converted into frames
- CRC is added to each frame
- receiver checks CRC
- if mismatch occurs, the frame is discarded or retransmitted

This ensures that the communication remains reliable and accurate.

---

## 19) Exam Definition
Error detection is the process used to identify whether data has been altered during transmission. Error correction is the method used to recover the original data after detecting an error.

---

## 20) Key Points for Revision
- Errors occur mainly due to noise and signal distortion
- Error detection checks for corruption
- Error correction fixes corruption
- Parity is simple but weak
- Checksum is better than parity
- CRC is most widely used in networking
- Hamming code corrects single-bit errors
- FEC corrects without retransmission
- ARQ uses retransmission for reliability

---

## 21) Short 5-Mark Answer
Error detection and correction are important techniques used in networking to ensure reliable data transfer. Errors may occur due to noise, interference, or signal degradation. Common error detection methods include parity check, checksum, and CRC, where extra bits are added to detect corruption. Error correction methods such as Hamming code and FEC help the receiver fix the error without retransmission. In many protocols, if an error is detected, the receiver requests retransmission using ARQ, thereby improving reliability.

---

## 22) Final Summary
Error detection and correction are essential for reliable communication in computer networks. They protect data from corruption caused by noisy transmission paths. Whether through parity, checksum, CRC, Hamming code, or ARQ, these methods ensure that the information received is as accurate as possible. Without them, data communication would be unreliable and insecure.
