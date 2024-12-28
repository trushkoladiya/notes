# Network Devices

![Switches, Routers, and Modems in a home network]

If you want to build a network, you can't just throw computers in a room and hope they start talking. You need specialized hardware to connect them, route their messages, and keep them safe. Let's look at the key network devices, starting from the oldest (and dumbest) to the modern tools that run the internet.

---

## What is it?

Network devices are the physical hardware components that connect computers, smartphones, printers, and other hosts together. Each device has a specific job:

| Device | Primary Role | OSI Layer | Key Characteristic |
| :--- | :--- | :--- | :--- |
| **Hub** | Dumb central connector | Layer 1 | Re-broadcasts signals to all ports. |
| **Switch** | Smart local LAN connector | Layer 2 | Forwards frames using MAC addresses. |
| **Router** | Inter-network gateway | Layer 3 | Routes packets using IP addresses. |
| **Modem** | Signal translator | Layer 1 | Converts digital <=> analog signals. |
| **Wireless Access Point (WAP)** | Wireless extender | Layer 1/2 | Bridges Wi-Fi clients to wired network. |
| **Firewall** | Security guard | Layer 3/4+ | Inspects and blocks malicious traffic. |
| **SOHO Router** | All-in-one consumer gateway | Mixed | Combines Router, Switch, WAP, Firewall, Modem. |

---

## Why do we need it?

Without these devices, networks wouldn't scale. If you wanted to connect multiple computers directly to each other without a central device, you would need to run cables between every single pair of computers.

| Number of Computers | Direct Cables Needed | Wiring Complexity |
| :---: | :---: | :--- |
| **2** | 1 | Simple single link |
| **4** | 6 | Messy star/mesh shape |
| **10** | 45 | Massive, impossible tangle |

Wiring would quickly become a massive, impossible mess. Central devices like switches solve this wiring problem. Furthermore, devices like routers are needed because we can't just put the entire world on a single giant switch (it would crash from too much traffic). We need routers to divide the world into smaller, manageable networks and pass data between them.

---

## How does it work?

Let's look at each device step-by-step to see how they handle traffic.

### 1. Hubs (The Predecessor)
*   **The Concept:** A hub is a central box with multiple ports. You plug ethernet cables from computers into it.

```text
[ HUB TRAFFIC FLOW (Dumb Broadcast) ]

      [Computer A] (Sends frame to B)
           │
           ▼ (Port 1)
     ┌───────────┐
     │    HUB    ├────────┐ (Copies signal to all ports)
     └┬─────────┬┘        │
      │ (Port 2)│ (Port 3)│ (Port 4)
      ▼         ▼         ▼
 [Comp B]  [Comp C]  [Comp D]
 (Target)  (Snoops!) (Snoops!)
```

*   **How it handles traffic:** When a hub receives a data packet on one port, it has no brain to check who it is for. It simply repeats the electrical signal and broadcasts (sends) it to **every single device** connected to it.
*   **The Flaws:**
    *   **Insecure:** Since everyone receives the packet, anyone with a packet sniffer (like a hacker) can read everyone else's data.
    *   **Congestion:** It wastes network bandwidth by flooding the wires with useless traffic.

### 2. Switches (The Smart LAN Connector)
*   **The Concept:** A switch operates at **Layer 2 (Data Link Layer)** and handles data in units called **frames**.

```text
[ SWITCH TRAFFIC FLOW (Smart Unicast) ]

      [Computer A] (Sends frame to B)
           │
           ▼ (Port 1)
     ┌───────────┐  [MAC Table Lookup]
     │  SWITCH   │  Port 1: MAC A
     └┬─────────┬┘  Port 2: MAC B
      │ (Port 2)│ (Port 3)
      ▼         ▼
 [Comp B]  [Comp C] (No traffic received)
 (Target)
```

*   **How it handles traffic:** Unlike a hub, a switch has a brain. It builds and maintains a **MAC Address Table** (also known as a **CAM Table** - Content Addressable Memory). 
    1.  When a computer sends a frame, the switch looks at the *source* MAC address and notes down which physical port it came from.
    2.  It then looks at the *destination* MAC address in the frame.
    3.  If it knows which port that destination MAC address belongs to, it forwards the frame **only to that specific port**.
    4.  If it doesn't know yet (an "unknown unicast"), or if it's a broadcast frame, it will send it to all ports. Once the destination device replies, the switch learns its port and won't flood again.
*   **Cisco CLI Command:** You can look inside a Cisco switch's brain using the command: `show mac-address-table`.

