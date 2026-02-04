# Communication-Protocols
**✅ What is UART?**

**UART (Universal Asynchronous Receiver Transmitter)** is a **serial communication protocol** used to transmit and receive data **asynchronously**, meaning **no shared clock** is used between the sender and receiver.
Instead, both devices agree on a **baud rate** to stay synchronized.

**📌 What is USART?**

**USART (Universal Synchronous/Asynchronous Receiver Transmitter)** supports:
**Asynchronous mode** Asynchronous mode → works like UART (most commonly used)

**Synchronous mode** → uses an external clock (rarely used in practice)

📌 In real-world embedded systems, **USART peripherals are mostly used in UART mode**, so people often use the terms UART and USART interchangeably.

**🔥 One-line interview answer**

UART is an asynchronous serial communication protocol that transmits data using start and stop bits without a shared clock, while USART is an extended version that can operate in both synchronous and asynchronous modes.

**🔹 TX, RX, GND in UART**
**TX (Transmit)** → Sends data out of the device

**RX (Receive)** → Receives data into the device

**GND (Ground)** → Common voltage reference between devices

**🔹 Connection Rule (Very Important)**
Device A TX  → Device B RX
Device A RX  → Device B TX
Device A GND ↔ Device B GND


📌 TX always connects to RX, never TX-to-TX.

🔹 Why GND Is Mandatory

UART signals are voltage-based

Receiver measures TX voltage relative to GND

Without a common ground:

Logic HIGH / LOW levels become unclear

Noise increases

Data becomes corrupted

⚠️ Missing GND = unstable or no communication

🔹 Real-Life Analogy (Easy to Remember)

Think of UART like a phone call:

TX → Speaking

RX → Listening

GND → Common language reference

If there’s no common language (GND), both sides hear nonsense 😄

🔹 Interview One-Liner

In UART communication, TX and RX lines are cross-connected between devices, and a common ground is mandatory because all voltage levels are measured relative to GND; without it, communication becomes unreliable or fails.
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

