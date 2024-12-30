# The OSI & TCP/IP Models

![OSI 7-Layer Model vs TCP/IP 4/5-Layer Model]

If you plug a Raspberry Pi, an iPhone, and a Windows laptop into the same network, they can share photos, exchange emails, and browse websites together without any issues. Today, this seems obvious. But a long time ago, it was impossible.

---

## What is it?

In the early days of computers (the 1960s and 70s), every technology company (like IBM) built their own proprietary network standards. If you bought IBM computers, they could only talk to other IBM computers. They spoke completely different languages.

To fix this, the industry created standardized **networking models**. These models act as a universal set of rules (protocols) that define how computers prepare, transmit, and receive data. 

Today, we have two major models:
1.  **TCP/IP Model:** The actual, practical model implemented in all operating systems that runs the internet today.
2.  **OSI Model (Open Systems Interconnection):** A theoretical 7-layer model. Although TCP/IP won the battle, network engineers still use the OSI layers as standard terminology for troubleshooting.

---

## Why do we need it?

We need networking models for two main reasons:
1.  **Interoperability:** So devices from Apple, Microsoft, Google, and Samsung can all talk to each other seamlessly.
2.  **Modular Troubleshooting:** By dividing network operations into layers, we can isolate problems. If a cable is unplugged, that is a "Layer 1" (Physical) issue. If your web browser is crashing, that is a "Layer 7" (Application) issue. This prevents network engineers from having to search the entire system for a simple problem.

---

## How does it work?

Both models divide the process of sending data into a stack of **layers**. As data travels from your screen to another computer, it goes down the stack on your device, travels across the wire, and goes up the stack on the receiving device.

### The Two Models Compared

| OSI Model Layer | TCP/IP Model Layer (Modern) | Protocol Data Unit (PDU) | Common Protocols / Devices |
|---|---|---|---|
| **7. Application** | Application | Data | HTTPS, HTTP, DNS, DHCP, FTP, SMTP |
| **6. Presentation** | Application | Data | HTML, JPEG, PDF, SSL/TLS (Encryption) |
| **5. Session** | Application | Data | L2TP, SOCKS Proxies, Session Management |
| **4. Transport** | Transport | Segment | TCP, UDP (Ports / Sockets) |
| **3. Network** | Network | Packet | IP, ICMP, Routers |
| **2. Data Link** | Data Link | Frame | MAC Addressing, Switches |
| **1. Physical** | Physical | Bits | Ethernet Cables, WAPs, Hubs, Repeater |

> **How to Memorize the OSI Layers:**
> *   **Top to Bottom (7 to 1):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
> *   **Bottom to Top (1 to 7):** **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

---

### Step-by-Step: Following a YouTube Video Request

Let's look at how the layers interact when you type `https://youtube.com/` in your browser.

#### Step 1: Preparing the Data (Layers 7, 6, and 5)
*   **Layer 7 (Application Layer):** Your web browser interfaces with the network by sending an HTTPS "GET" request, asking YouTube's server to send you the video webpage.
*   **Layer 6 (Presentation Layer):** The data is formatted into HTML (a format browsers understand) and encrypted using **SSL/TLS** so hackers can't intercept your session.
*   **Layer 5 (Session Layer):** A logical session is established between your browser and YouTube's server to keep the conversation open and separate it from other apps (like Spotify) running on your device.

#### Step 2: Choosing the Transport Method (Layer 4)
The data is passed to the **Transport Layer**. Here, the computer decides how to send the data. It has two primary protocols to choose from:

*   **TCP (Transmission Control Protocol):** Connection-oriented and highly reliable. Before sending data, it performs a **three-way handshake**:

```text
[ TCP THREE-WAY HANDSHAKE ]

  Client                               Server
    │                                    │
    │────── SYN (Let's sync up!) ───────>│
    │                                    │
    │<── SYN-ACK (Acknowledged & sync) ──│
    │                                    │
    │─────── ACK (Let's talk now!) ─────>│
    │                                    │
```

![TCP Three-Way Handshake flow]

    1.  *Client:* "Hey YouTube, let's sync up (SYN)."
    2.  *YouTube:* "I acknowledge you, let's sync (SYN-ACK)."
    3.  *Client:* "Acknowledged, let's talk (ACK)."
    If TCP sends a packet and doesn't get a confirmation reply from the receiver, it will keep resending it until it arrives. It is used for loading webpages, emails, and file transfers.
*   **UDP (User Datagram Protocol):** Connectionless and fast. It doesn't do a handshake or check if the receiver got the data—it just blasts it out continuously. It is used for real-time applications like **video streaming (YouTube video playback)**, online gaming, and voice calls. If you drop a packet during a live stream, resending it later is useless because the video has already moved on.

#### Port Numbers
The Transport Layer also assigns **Port Numbers** so the server knows which service you want to use.
*   **Destination Ports (Well-Known Ports, 0-1023):** Services run on standard ports. HTTPS runs on **443**, HTTP runs on **80**, SSH runs on **22**, FTP runs on **21**, and TFTP runs on **69**.
*   **Source Ports (Ephemeral Ports, 1024-65535):** Your browser randomly chooses a temporary port (e.g., `57095`) as its return address. When YouTube replies, it sends the data to your IP address on port `57095`, which tells your computer to deliver it specifically to your active web browser tab and not your Spotify app.

```text
[ PORT MULTIPLEXING / MAPPING ]

                         Incoming Traffic
                                │
                                ▼ (Router / Network Card)
                         ┌──────────────┐
                         │ Destination  │
                         │  Port Check  │
                         └──────┬───────┘
                                │
                  ┌─────────────┴─────────────┐
                  ▼ (Port 57095)              ▼ (Port 43210)
         ┌──────────────────┐        ┌──────────────────┐
         │  Web Browser Tab │        │   Spotify App    │
         │ (YouTube Video)  │        │   (Music Stream) │
         └──────────────────┘        └──────────────────┘
```

*At the Transport Layer, the data is encapsulated with a Layer 4 header. We call this a **Segment**.*

#### Step 3: Routing the Packet (Layer 3)
The segment is passed to the **Network Layer**. Here, a Layer 3 header containing the **Source IP** (your computer) and **Destination IP** (YouTube's server) is attached.
*   *At this layer, the message is called a **Packet**.*
*   Routers use these IP addresses to inspect the packet and pass it from network to network.

#### Step 4: Local Delivery (Layer 2)
The packet is passed to the **Data Link Layer**. Here, a Layer 2 header containing the **Source MAC** (your computer) and **Destination MAC** (your router / next-hop gateway) is attached.
*   *At this layer, the message is called a **Frame**.*
*   Switches use these MAC addresses to deliver the frame locally to the next hop.

#### Step 5: Firing the Bits (Layer 1)
Finally, the frame reaches the **Physical Layer**. Your network card converts the digital frame into raw electrical signals, light pulses, or radio waves (Wi-Fi) and sends them down the physical cables toward the internet.

---

### Encapsulation and De-encapsulation

*   **Encapsulation:** As data moves down the sender's stack, each layer wraps the data from the layer above it in a new envelope, adding its own headers and trailers.
*   **De-encapsulation:** On the receiving end, the server performs the reverse. It strips off the Layer 2 header to read the MAC, strips the Layer 3 header to read the IP, strips the Layer 4 header to check the Port, and finally delivers the raw application data to the web server software.

```text
[ Sender: Encapsulation ]                      [ Receiver: De-encapsulation ]
      Data (HTTP Request)                                Data (HTTP Request)
              |                                                  ^
              v                                                  |
     [L4 Header | Data]  (Segment)                      [L4 Header | Data]
              |                                                  ^
              v                                                  |
[L3 Header | L4 Header | Data]  (Packet)            [L3 Header | L4 Header | Data]
              |                                                  ^
              v                                                  |
[L2 Header | L3 Header | L4 Header | Data | Trailer]    [L2 Header | L3 Header | L4 Header | Data | Trailer]
              |                                                  ^
              v                                                  |
         101011110001  (Physical Bits)  -------------------> 101011110001
```

---

## Important Things to Remember

*   **Encapsulation:** Wrapping data in headers (envelopes) as it moves from Layer 7 to Layer 1.
*   **De-encapsulation:** Opening envelopes as data moves from Layer 1 to Layer 7.
*   **TCP vs UDP:** TCP is reliable, connection-oriented (handshake), and resends lost packets. UDP is fast, connectionless, and doesn't verify delivery (ideal for video/gaming).
*   **Ports:** Tell the device which specific program should receive the data. (HTTPS = 443, HTTP = 80, Telnet = 23, SSH = 22, FTP = 21, TFTP = 69).
*   **Layer 3 = Packets & Routers & IPs:** Directions for the global internet.
*   **Layer 2 = Frames & Switches & MACs:** Directions for the local street hop.

---

## Related Topics

*   [Network Devices](file:///home/trush/Downloads/x/02_network_devices.md) - The devices operating at Layer 1 (Hubs), Layer 2 (Switches), and Layer 3 (Routers).
*   [IP Addresses & Subnetting](file:///home/trush/Downloads/x/04_ip_addresses_and_subnetting.md) - How Layer 3 IP addressing works in detail.
