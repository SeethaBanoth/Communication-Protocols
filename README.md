# Communication-Protocols
## ✅ What is UART?

**UART (Universal Asynchronous Receiver Transmitter)** is a **serial communication protocol** used to transmit and receive data **asynchronously**, meaning **no shared clock** is used between the sender and receiver.
Instead, both devices agree on a **baud rate** to stay synchronized.

## 📌 What is USART?

**USART (Universal Synchronous/Asynchronous Receiver Transmitter)** supports:
**Asynchronous mode** Asynchronous mode → works like UART (most commonly used)

**Synchronous mode** → uses an external clock (rarely used in practice)

📌 In real-world embedded systems, **USART peripherals are mostly used in UART mode**, so people often use the terms UART and USART interchangeably.

## 🔥 One-line interview answer

UART is an asynchronous serial communication protocol that transmits data using start and stop bits without a shared clock, while USART is an extended version that can operate in both synchronous and asynchronous modes.

## 🔹 TX, RX, GND in UART
**TX (Transmit)** → Sends data out of the device

**RX (Receive)** → Receives data into the device

**GND (Ground)** → Common voltage reference between devices

## 🔹 Connection Rule (Very Important)
```css
Device A TX  → Device B RX
Device A RX  → Device B TX
Device A GND ↔ Device B GND
```

📌 TX always connects to RX, never TX-to-TX.

## 🔹 Why GND Is Mandatory

UART signals are **voltage-based**

Receiver measures TX voltage relative to GND

Without a common ground:

Logic HIGH / LOW levels become unclear

Noise increases

Data becomes corrupted
**⚠️ Missing GND = unstable or no communication**

## 🔹 Real-Life Analogy (Easy to Remember)

Think of UART like a phone call:

**TX** → Speaking

**RX **→ Listening

**GND** → Common language reference

If there’s no common language (GND), both sides hear nonsense 😄

## 🔹 Interview One-Liner

In UART communication, TX and RX lines are cross-connected between devices, and a common ground is mandatory because all voltage levels are measured relative to GND; without it, communication becomes unreliable or fails.

## 🔹 Baud Rate
### 🔸 What is Baud Rate?

**Baud rate** is the **number of signal changes (symbols) per second** in a communication channel.

📌 In **UART**,
**1 symbol = 1 bit**,
so **baud rate = bits per second (bps).**

### 🔸 Common Baud Rates

9600 bps

19200 bps

38400 bps

57600 bps

115200 bps

1 Mbps

Higher baud rate → faster communication
But also → more sensitive to noise

### 🔸 Why Both Devices Must Match

UART is **asynchronous** (no shared clock).

Transmitter and receiver **agree on timing in advance**

Receiver samples bits based on baud rate

❌ If baud rate mismatches:

Receiver samples at wrong time

Bits shift → **garbage data**

### 🔸 Example: Baud Rate = 9600
```sql
9600 bits per second
→ 1 bit time = 1 / 9600 seconds
→ ≈ 104 microseconds per bit
```

For a common **8N1**frame:

1 start bit

8 data bits

1 stop bit

📌 Total = **10 bits per character**

```sql
9600 / 10 = 960 characters per second
```
### 🔸 Baud Rate vs Throughput (Important!)

Even though baud rate = 9600 bps:

Actual data bits per frame = 8

Total bits per frame = 10

**📌 Effective data rate < baud rate**

This is why:

Higher overhead → lower throughput

Parity + extra stop bits reduce usable data

### 🔥 Interview Tip (One-liner)

Baud rate is the number of bits transmitted per second in UART, and since UART is asynchronous, both transmitter and receiver must use the same baud rate; otherwise, bit sampling errors cause corrupted data.
