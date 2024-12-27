# Introduction to Networking Basics

![Network hosts sharing a printer]

Imagine you are playing an online multiplayer game like Call of Duty. You click "shoot" on your controller, and instantly, a player on the other side of the world gets hit. How did that action travel from your room to their screen in the blink of an eye? 

That is the magic of computer networks. A network is simply a group of computers or devices connected together so they can talk and share stuff. 

---

## What is it?

A **computer network** is a group of two or more computers (or other devices like smartphones, printers, and smart TVs) that are connected together. This connection can be through physical wires (like ethernet cables) or wireless signals (like Wi-Fi).

Each device that can send or receive data on a network is called a **host**. In the past, only computers were hosts. Today, hosts include:
*   Laptops and desktop computers
*   Smartphones and tablets
*   Smartwatches and smart home devices (like smart bulbs or refrigerators)
*   Printers and servers

---

## Why do we need it?

Before networks existed, if you had a file on your computer and wanted to give it to a friend, you had to copy it to a physical drive (like a floppy disk) and hand it to them. That was slow and annoying.

We need networks for two main reasons:

1.  **Data Communication:** To send messages, videos, emails, and play games in real-time.
2.  **Resource Sharing:** Instead of buying a printer for every single employee in an office, we can buy *one* printer, connect it to the network, and let everyone use it. This also applies to internet connections (sharing one Wi-Fi network) and file storage (sharing one central file server).

---

## How does it work?

To understand how a network works, we need to look at the physical components, the addresses devices use, and how data is broken down to travel.

### 1. Physical Components (Cables & Ports)
To connect devices, we need a medium.

![Ethernet LAN Port and RJ45 Connectors]

*   **Ethernet Cables (Network Cables):** These are the physical wires used for wired connections. They have standard plastic plugs on each end called **RJ45 connectors**. 
*   **LAN Port (Network/Ethernet Port):** This is the rectangular socket on your computer, router, or printer where the RJ45 connector clicks in.
*   **Network Interface Card (NIC):** This is the actual hardware chip inside your device that handles the network connection. Your computer has a wired NIC (for the Ethernet port) and a wireless NIC (for Wi-Fi).
*   **Transmission Media:** Data can travel through copper wires (electric signals), fiber-optic cables (pulses of light), or the air (radio waves for Wi-Fi).

### 2. MAC Addresses vs. IP Addresses
To send mail, you need a name and an address. Computers need the same thing to send data.

| Feature | MAC Address (Media Access Control) | IP Address (Internet Protocol) |
| :--- | :--- | :--- |
| **What is it?** | Physical address burned into the hardware NIC. | Logical address assigned by the network. |
| **Permanence** | Permanent (never changes). | Temporary (changes based on location). |
| **Format** | 12-digit hexadecimal (e.g., `00:1A:2B:3C:4D:5E`). | 4 octets in dotted-decimal (e.g., `192.168.1.50`). |
| **Analogy** | Your government-issued ID or name. | Your physical postal mailing address. |
| **Function** | Used to deliver data to the device on the local LAN. | Used to route data across different global networks. |

> **Why do we need both?**
> Think of the **IP address** like your postal address (e.g., "123 Main St, New York"). It helps the mail system route letters to your city and house. Think of the **MAC address** like your government-issued ID card or your name. Once the mail carrier arrives at the house (the IP address), they use your name (the MAC address) to deliver the letter to the exact person.
> *   **IP Address:** Used to locate the device across different networks.
> *   **MAC Address:** Used to deliver the data to the physical device once it is on the local network.

### 3. How IP Addresses are Assigned (DHCP)
Right now, you know that IP addresses are "logical addresses assigned by the network," but how does that actually happen? You don't have to manually type in an IP address every time you connect to a new Wi-Fi network. Instead, your device talks to a **DHCP (Dynamic Host Configuration Protocol)** server.

#### What is DHCP?
DHCP is a network protocol used to automatically assign IP addresses and other network configuration settings to devices on a network. 

#### The DORA Process (How it Works)
When your device connects to a network, it goes through a 4-step handshake to lease an IP address. Think of it as **DORA**:

1.  **Discover:** The client broadcasts a message to the network: *"Hey, is there a DHCP server out there? I need an IP address!"*
2.  **Offer:** The DHCP server replies with an offer: *"Yes, I'm here! I have IP address 192.168.1.50 available for you to use."*
3.  **Request:** The client replies: *"Awesome, I would like to lease IP address 192.168.1.50, please!"*
4.  **Acknowledge (ACK):** The DHCP server confirms: *"Got it! IP address 192.168.1.50 is now locked in for your device. Go ahead and use it!"*

```text
[ THE DORA PROCESS ]

   Client (Device)                             DHCP Server
         │                                          │
         │───────── DISCOVER (Broadcast) ──────────>│
         │                                          │
         │<────────── OFFER (Unicast) ──────────────│
         │                                          │
         │───────── REQUEST (Broadcast) ───────────>│
         │                                          │
         │<────────── ACKNOWLEDGE (Unicast) ────────│
         │                                          │
```

#### What a DHCP Server Assigns
It doesn't just hand out an IP address. To fully configure your device's network settings, it assigns four critical values:
*   **IP Address:** Your unique logical address on the street.
*   **Subnet Mask:** Defines how big the street network is.
*   **Default Gateway:** The exit door (router's IP) to get off your street and onto the internet.
*   **DNS Server:** The phone book server that translates domain names into IP addresses.

#### Where does the DHCP Server live?
*   **In Home Networks:** It runs directly inside your all-in-one **home router**.
*   **In Enterprise Networks:** Large companies have too many devices for a router to manage, so they run DHCP on a dedicated, central **Enterprise Server**.

### 4. Packets (How Data Travels)
When you send a large file (like a photo or a video), it is not sent all at once. If it were, and a single error occurred, you would have to resend the entire file.

```text
[Large File (e.g. Photo)]
       │
       ▼ (Fragmentation)
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│Packet 1 (Hdr)│   │Packet 2 (Hdr)│   │Packet 3 (Hdr)│  (Typically max 1500 bytes)
└──────┬───────┘   └──────┬───────┘   └──────┬───────┘
       │                  │                  │
       └──────────────────┼──────────────────┘
                          ▼ (Transmitted over wire/air)
┌────────────────────────────────────────────────────┐
│                    Network Wire                    │
└─────────────────────────┬──────────────────────────┘
                          ▼ (Reassembly at destination)
[Original Large File Reconstructed]
```

*   **Data Packet:** The network breaks the large file down into tiny pieces called packets (typically capped at **1,500 bytes** of data).
*   **Packet Header:** Every packet has a "header" stuck to the front of it. The header contains critical information, including the sender's IP address and the receiver's IP address.
*   **Reassembly:** Once all the packets reach the destination, the receiver's device puts them back in order to reconstruct the original file.
*   **Jumbo Frames:** Some specialized networks support larger packets up to **9,000 bytes** to speed up data transfer.

### 5. Communication Types
Devices talk to each other in three different ways:
*   **Unicast:** One-to-one communication (like a private WhatsApp message).
*   **Multicast:** One-to-selected-many communication (like a Zoom video call with a specific group of people).
*   **Broadcast:** One-to-all communication (like a radio station broadcasting to every radio in range).

### 6. Network Types (Based on Size)
We classify networks by how much physical area they cover:

| Type | Full Name | Geographic Scope | Typical Example |
| :--- | :--- | :--- | :--- |
| **PAN** | Personal Area Network | Immediate user space (~10 meters) | Bluetooth phone connecting to smartwatch. |
| **LAN** | Local Area Network | Single building, home, or office | Home Wi-Fi network or school office. |
| **WLAN** | Wireless LAN | Wireless LAN coverage area | Devices connected to a home router without cables. |
| **MAN** | Metropolitan Area Network | Town, city, or large campus | College Wi-Fi spread across a city campus. |
| **WAN** | Wide Area Network | Countries, continents, or global | The Internet (the ultimate WAN). |

---

## Important Things to Remember

*   **Host:** Any device connected to a network that can send/receive data.
*   **NIC (Network Interface Card):** The physical hardware inside a device that lets it connect to a network.
*   **MAC Address:** A permanent, 12-digit physical address burned into the hardware.
*   **IP Address:** A temporary, logical address that changes based on your network location.
*   **Packets:** Small pieces of data (usually max 1500 bytes) sent over the network to make communication reliable and efficient.
*   **LAN vs WAN:** LAN is local (your house/office), WAN covers huge geographic distances (the Internet).

---

## Diagram

Here is a simple look at how devices connect to share a printer in a Local Area Network (LAN):

```text
  +------------------+          +------------------+
  |    Computer A    |          |    Computer B    |
  | MAC: AA:AA:AA... |          | MAC: BB:BB:BB... |
  +--------+---------+          +--------+---------+
           |                             |
     Ethernet Cable                 Ethernet Cable
           |                             |
  +--------v-----------------------------v----------+
  |                  Network Switch                 |
  +------------------------+------------------------+
                           |
                     Ethernet Cable
                           |
                  +--------v---------+
                  |  Shared Printer  |
                  | MAC: PP:PP:PP... |
                  +------------------+
```

---

## Related Topics

*   [Network Devices](file:///home/trush/Downloads/x/02_network_devices.md) - The hardware (like switches and routers) that physically connects these devices together.
*   [IP Addresses & Subnetting](file:///home/trush/Downloads/x/04_ip_addresses_and_subnetting.md) - A deep dive into how IP addresses work and how we divide them.
