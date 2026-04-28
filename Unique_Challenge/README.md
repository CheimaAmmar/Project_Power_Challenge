🚗 CAN Bus Analysis – Write-up
📌 Challenge Overview

We were given a .sal file (Saleae Logic capture) containing signals from a car’s serial network. The goal was to analyze the communication and extract the VIN / flag transmitted on the bus.

🔍 Step 1 – Opening the Capture

The .sal file was opened using Logic 2 (Saleae).

The capture showed two complementary signals, suggesting a differential bus.

📡 Step 2 – Identifying the Protocol

From the waveform characteristics:

Two خطوط (CAN_H / CAN_L)
Opposite voltage transitions
Stable idle state

We identified the protocol as CAN (Controller Area Network).

CAN uses differential signaling for robustness and is widely used in automotive systems.

⚙️ Step 3 – Decoding the Signal

We added a CAN analyzer in Logic 2.

We initially tried common bitrates:

1 Mbps
500 kbps
250 kbps

❌ None produced valid decoding.

🧠 Step 4 – Finding the Bitrate

We manually measured the bit duration from the waveform:

1 bit ≈ 7.8 µs

Using:

bitrate = 1 / bit_time
bitrate ≈ 1 / (7.8 × 10⁻⁶) ≈ 128 kbps

Closest standard value:

👉 125 kbps

✅ Step 5 – Successful Decode

After setting:

Bitrate = 125000 bits/s

The CAN frames were decoded correctly.

🚩 Step 6 – Extracting the Flag

In the decoded CAN frames, ASCII data appeared:

HTB{...}

The flag was directly visible in the payload.

📚 CAN Background (Brief)
CAN is a message-based protocol
Uses differential signaling (CAN_H / CAN_L)
Common in automotive ECUs
Max payload: 8 bytes per frame (classic CAN)

Reference:
https://www.circuitbread.com/tutorials/understanding-can-a-beginners-guide-to-the-controller-area-network-protocol

🎯 Conclusion
The capture contained a CAN bus communication
Bitrate was determined via bit timing analysis
Correct bitrate: 125 kbps
Flag extracted directly from decoded frames
