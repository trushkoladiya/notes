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

