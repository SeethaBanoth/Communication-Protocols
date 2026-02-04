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
