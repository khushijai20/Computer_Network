# 🌐 Day 5: Manchester Encoding and Differential Manchester Encoding

Welcome to Day 5 of the Computer Networks study guide. This lesson explains two important physical layer line coding techniques used for bit synchronization and reliable digital transmission.

## 📝 Topics Covered

- What is Manchester encoding?
- What is Differential Manchester encoding?
- How each encoding represents bits
- Clock recovery and synchronization
- Advantages and disadvantages
- Use cases and examples
- Comparison and timing diagrams

## 🎯 Learning Objectives

By the end of this lesson, you should be able to:

- Define Manchester and Differential Manchester encoding
- Explain how each method encodes a `0` and a `1`
- Understand why these techniques help with synchronization
- Compare the benefits and limitations of each encoding
- Recognize common applications for each signal type

---

## 📌 What Is Manchester Encoding?

Manchester encoding is a line coding method where each bit period contains a transition, and the transition direction defines the bit value.

- A `0` is encoded as a high-to-low transition in the middle of the bit interval.
- A `1` is encoded as a low-to-high transition in the middle of the bit interval.

### Key points

- The signal always changes in the middle of every bit.
- The transition provides a built-in clock reference.
- The polarity at the start of the bit is not fixed; the important event is the mid-bit transition.

### Example waveform

- Bit `0`: `1` then `0` inside the bit time
- Bit `1`: `0` then `1` inside the bit time

This gives a self-clocking signal because the receiver can recover timing from the guaranteed mid-bit transition.

### Why Manchester encoding is useful

- Removes long runs of constant voltage, so the receiver can stay synchronized.
- Helps prevent baseline wander and DC level problems.
- Simple decoding logic: detect the direction of the mid-bit transition.

### Disadvantages

- Requires twice the bandwidth compared to simple NRZ, because each bit includes a guaranteed transition.
- More complex than basic non-return-to-zero schemes.

---

## 📌 What Is Differential Manchester Encoding?

Differential Manchester encoding combines differential signaling with guaranteed transitions.

- A transition always occurs at the beginning of each bit period.
- A `0` is represented by a transition in the middle of the bit period.
- A `1` is represented by no transition in the middle of the bit period.

### Key points

- The data is encoded by whether the signal changes or stays the same at the middle of the bit interval.
- The start-of-bit transition acts as the clock edge.
- The value is determined by the presence or absence of the mid-bit transition, not by absolute voltage levels.

### Example waveform

- Bit `0`: transition at start of bit + another transition in middle
- Bit `1`: transition at start of bit only

This makes Differential Manchester robust to polarity inversion, because the bit meaning depends on change rather than absolute level.

### Why Differential Manchester is useful

- Provides self-clocking with a guaranteed transition every bit.
- Immune to polarity reversal on the transmission medium.
- Good for noisy environments or when the receiver cannot assume signal polarity.

### Disadvantages

- Also requires more bandwidth than NRZ because of frequent transitions.
- More difficult to implement than simple NRZ or NRZI.

---

## 🔍 Comparison: Manchester vs Differential Manchester

| Feature | Manchester Encoding | Differential Manchester Encoding |
|---|---|---|
| Bit encoding rule | Mid-bit transition direction | Mid-bit transition presence/absence |
| Clock recovery | Yes, from mid-bit transition | Yes, from start-of-bit transition |
| DC component | Balanced, no DC bias | Balanced, no DC bias |
| Polarity sensitivity | Yes | No (differential) |
| Bandwidth requirement | High | High |
| Common use | 10BASE-T Ethernet (legacy), some RFID | Token Ring, IEEE 802.5, some magnetic recording |

---

## 🧠 How to read the signals

### Manchester decode rule

- If the signal goes from high to low in the middle of the bit, the bit is `0`.
- If the signal goes from low to high in the middle of the bit, the bit is `1`.

### Differential Manchester decode rule

- Look for the transition at the start of each bit to recover the clock.
- If there is a transition in the middle of the bit, the bit is `0`.
- If there is no transition in the middle of the bit, the bit is `1`.

> Important: Differential Manchester does not care about signal polarity. If the entire waveform is inverted, the decoded bits remain the same.

---

## ⚙️ Practical examples and uses

- **Manchester encoding**: used in the original 10BASE-T Ethernet physical layer and some older communication systems where bit-level timing needed a direct clock.
- **Differential Manchester**: used in Token Ring (IEEE 802.5), some magnetic storage systems, and protocols requiring polarity-insensitive decoding.

### Real-world note

In many modern high-speed links, encoding schemes like 4B/5B, 8B/10B, or PAM are used instead of raw Manchester, but Manchester and Differential Manchester remain important concepts for understanding self-clocking line codes.

---

## ✅ Advantages summary

- Both encodings: good synchronization, balanced signal, no long constant-level runs.
- Manchester: easy to decode from mid-bit transitions.
- Differential Manchester: robust to polarity inversion and still self-clocking.

## ⚠️ When to avoid these encodings

- If bandwidth is limited and an efficient high-speed physical layer is needed, choose modern block encoding or multi-level signaling instead.
- If the channel cannot support the high transition density, alternatives like NRZI or 8B/10B may be preferred.

---

## 📌 Quick revision notes

- **Manchester**: every bit has a transition in the middle; direction encodes `0` vs `1`.
- **Differential Manchester**: every bit starts with a transition; mid-bit transition means `0`, no mid-bit transition means `1`.
- **Self-clocking**: both methods let the receiver recover timing from transitions.
- **Difference**: Manchester uses absolute level change; Differential Manchester uses relative change.

---

## 🔎 Example bit sequence

For the bit stream `1 0 1 1 0`:

- Manchester: `1` -> low-to-high, `0` -> high-to-low, `1` -> low-to-high, `1` -> low-to-high, `0` -> high-to-low.
- Differential Manchester: ensure a transition at every start; add a mid-bit transition only for `0` values.

---

## 📚 Further reading

- IEEE 802.3 10BASE-T physical layer
- IEEE 802.5 Token Ring physical layer
- Line coding and clock recovery in digital communications

_Prepared: Day 5 — Manchester Encoding and Differential Manchester Encoding._