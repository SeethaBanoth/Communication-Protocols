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

- 9600 bps

- 19200 bps

- 38400 bps

- 57600 bps

- 115200 bps

- 1 Mbps

Higher baud rate → faster communication
But also → more sensitive to noise

### 🔸 Why Both Devices Must Match

UART is **asynchronous** (no shared clock).

- Transmitter and receiver **agree on timing in advance**

- Receiver samples bits based on baud rate

❌ If baud rate mismatches:

- Receiver samples at wrong time

- Bits shift → **garbage data**

### 🔸 Example: Baud Rate = 9600
```sql
9600 bits per second
→ 1 bit time = 1 / 9600 seconds
→ ≈ 104 microseconds per bit
```

For a common **8N1**frame:

- 1 start bit

- 8 data bits

- 1 stop bit

📌 Total = **10 bits per character**

```sql
9600 / 10 = 960 characters per second
```
### 🔸 Baud Rate vs Throughput (Important!)

Even though baud rate = 9600 bps:

- Actual data bits per frame = 8

- Total bits per frame = 10

**📌 Effective data rate < baud rate**

This is why:

- Higher overhead → lower throughput

- Parity + extra stop bits reduce usable data

### 🔥 Interview Tip (One-liner)

Baud rate is the number of bits transmitted per second in UART, and since UART is asynchronous, both transmitter and receiver must use the same baud rate; otherwise, bit sampling errors cause corrupted data.

## 🔹 Start Bit, Stop Bit, Parity (UART Frame)

Since UART is asynchronous, the receiver must figure out when a frame starts and ends. That’s why start/stop/parity bits exist.

### 🔹 UART Frame Format
```scss
Idle(1) → Start(0) → Data Bits → Parity → Stop(1)
```

- Idle state of UART line is HIGH (1)

- A LOW (0) transition tells the receiver that data is starting

### 🔸 Start Bit

- Always LOW (0)

- Duration = 1 bit time

- Purpose:

-  Synchronizes the receiver

-  Tells receiver: “Start sampling data now”

📌 Receiver detects falling edge (1 → 0) and starts counting bit timing.

### 🔸 Data Bits

- Actual useful data

- Common values: 5, 6, 7, 8, or 9 bits

- Most common: 8 bits

📌 Sent LSB first (Least Significant Bit first)

Example:
```sql
Data = 0x41 (ASCII 'A')
Binary = 01000001
Sent order → 1 0 0 0 0 0 1 0
```

### 🔸 Parity Bit (Optional)

Used for error detection (not correction).

▶ Even Parity

- Total number of 1s (data + parity) = even

▶ Odd Parity

- Total number of 1s (data + parity) = odd

📌 If received parity ≠ calculated parity → Parity Error

⚠️ Detects only single-bit errors

### 🔸 Stop Bit

- Always HIGH (1)

- Can be:

-  1 stop bit

-  1.5 stop bits

-  2 stop bits

Purpose:

Marks end of frame

Gives receiver time to prepare for next frame

📌 Stop bit must be HIGH, otherwise framing error occurs.

### 🔹 Common UART Format: 8N1

Meaning:

- 8 → Data bits

- N → No parity

- 1 → One stop bit

Frame looks like:
```scss
Start(0) → 8 Data Bits → Stop(1)
```


📌 Most widely used UART configuration.

### 🔹 Timing Example (9600 Baud, 8N1)

- Bit time = 1 / 9600 ≈ 104 µs

- Total bits per frame:
```powershell
1 start + 8 data + 1 stop = 10 bits
```

- Time per byte ≈ 1.04 ms

### 🔥 Interview One-Liner

UART uses a start bit to synchronize communication, data bits to transfer information, an optional parity bit for error detection, and one or more stop bits to mark the end of the frame. The most common configuration is 8N1.
