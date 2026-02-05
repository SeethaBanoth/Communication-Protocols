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

  - 1 stop bit

  - 1.5 stop bits

  - 2 stop bits

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

## 🔴 Framing Error (UART)
### 🔹 What is a Framing Error?

A framing error occurs when the UART does not detect the expected STOP bit (logic HIGH) at the correct time.

📌 In simple words:
👉 The receiver loses track of where the character ends.

### 🔹 Why STOP Bit Matters

UART has no clock, so the receiver:

Detects the start bit

Samples bits based on the configured baud rate

Expects the stop bit at the end

If the stop bit is missing or not HIGH, the frame is considered invalid.

### 🔹 When Does Framing Error Occur?
#### 1️⃣ Baud Rate Mismatch (Most Common)

Example:

Transmitter: 9600 bps

Receiver: 115200 bps

⏱ Receiver samples at the wrong time → stop bit check fails.

📌 Result: framing error flag set.

#### 2️⃣ Noise on the Line

Electrical interference

Long wires

Poor grounding

Noise can flip the stop bit from 1 → 0.

📌 Receiver thinks frame never ended.

#### 3️⃣ Wrong Frame Configuration

Transmitter: 8N1

Receiver: 8E1 or 8N2

📌 Parity/stop bit expectations don’t match.

### 🔹 What Happens Internally?

UART hardware sets Framing Error (FE) flag

Received byte may be discarded or corrupted

Next bytes may also be misaligned

📌 Receiver loses synchronization.

### 🔹 How to Detect a Framing Error

Check UART status register

FE flag indicates stop bit failure

Example (conceptual):

if (UART_STATUS & FRAMING_ERROR) {
    // handle error
}

### 🔹 How to Fix / Prevent Framing Errors

✔ Match baud rate exactly
✔ Match frame format (8N1, parity, stop bits)
✔ Use proper grounding
✔ Keep UART lines short
✔ Lower baud rate if noise exists
✔ Use shielding if required

### 🔥 Interview-Ready One-Liner

A framing error occurs when the UART receiver fails to detect a valid stop bit, usually due to baud rate mismatch, noise, or incorrect frame configuration, causing loss of byte boundary synchronization.

## 🔹 Framing Error
### ✅ What is a Framing Error?

A framing error occurs when the UART receiver does not detect a valid stop bit at the expected time.

📌 In simple words:

The receiver loses track of where one data frame ends.

### 🔸 Why Stop Bit Matters

UART frame:

Start(0) → Data bits → (Parity) → Stop(1)


Stop bit must be HIGH

It tells the receiver: “This byte is finished”

If stop bit is missing or wrong → Framing Error

### 🔸 Causes of Framing Error

Baud rate mismatch (most common)

Noise or signal distortion

Incorrect UART configuration (stop bits)

Clock drift in long communication

### 🔸 Effect

Receiver may:

Discard the byte

Misalign the next frames

Data becomes unreliable

📌 Receiver doesn’t know where the frame ends

### 🔸 Real-Life Example

MCU baud rate = 9600

PC baud rate = 115200

➡ Stop bit sampled at the wrong time
➡ Framing error occurs

### 🔹 Parity Error
#### ✅ What is a Parity Error?

A parity error occurs when the received parity bit does not match the calculated parity of received data bits.

#### 🔸 How Parity Works (Quick Recap)

Example (Even parity):

Data: 1011001
Number of 1s = 4 (even)
Parity bit = 0

If receiver counts odd number of 1s → Parity Error

#### 🔸 Causes of Parity Error

Noise flipping a bit

Signal integrity issues

Long cables / EMI

#### 🔸 Effect

Receiver detects error

But cannot correct the data

📌 Parity can detect only single-bit errors

### 🔹 Key Differences (Interview Favorite)
Feature |	Framing Error	| Parity Error
|:--- |:--- |:--- |
Related to |	Stop bit |	Parity bit
Error type|	Timing / frame boundary	| Data integrity
Main cause | Baud mismatch, noise |	Bit flip
Detection |	Hardware UART |	Parity logic
Correction |	❌ No |	❌ No
### 🔥 One-Line Interview Answers

**Framing Error:**

Occurs when the stop bit is not detected correctly, usually due to baud rate mismatch or noise, causing loss of frame synchronization.

**Parity Error:**

Occurs when the received parity bit does not match the calculated parity, indicating a possible single-bit data error.

## 🔹 Flow Control (UART)

Flow control is used to prevent data loss when the receiver cannot process incoming data fast enough.

📌 Without flow control → receiver buffer overflows → data is lost.

## 🔸 Hardware Flow Control (RTS / CTS)

Uses dedicated hardware signals.

- RTS (Request To Send)
→ Sender asks: “Can I send data?”

- CTS (Clear To Send)
→ Receiver replies: “Yes, I’m ready.”

📌 Rule:
Sender transmits data only when CTS is asserted (active).

### ✅ Advantages

- Very fast

- Highly reliable

- Not affected by data content

### ❌ Disadvantages

- Requires extra pins/wires

- Not always available on low-pin MCUs

📌 Used in:
High-speed UART, industrial systems, modems

## 🔸 Software Flow Control (XON / XOFF)

Uses special control characters sent in data stream.

- XOFF (0x13) → Receiver says: “Stop sending”

- XON (0x11) → Receiver says: “Resume sending”

📌 These characters are transmitted like normal UART data.

### ✅ Advantages

- No extra hardware pins required

- Simple to implement

### ❌ Disadvantages

- Slower than hardware flow control

- Control characters may conflict with actual data

- Not reliable for binary data

📌 Used in:
Terminals, low-speed links, simple devices

## 🔥 Hardware vs Software Flow Control (Quick Compare)
Feature	RTS / CTS	XON / XOFF
Extra pins	Yes	No
Speed	Fast	Slower
Reliability	High	Medium
Data dependency	No	Yes
Best for	High-speed	Low-speed
## 🎯 Interview One-Liner

Hardware flow control uses RTS/CTS signals to control data transmission at the hardware level, making it fast and reliable, while software flow control uses XON/XOFF control characters within the data stream and is slower but does not require extra pins.
