# Network Design & Home Network Security

![Three-Tier Campus Network architecture showing Access, Distribution, and Core layers]

If you design a network badly, a single chewed cable or a smart bulb hacked by a hacker could bring down your entire company. To prevent this, network engineers use structured design models (two-tier and three-tier architectures) and basic security hardening.

---

## What is it?

**Network Design** is the practice of structuring network devices (routers, switches) in a way that is organized, scalable, and resilient. 

**Network Security Hardening** is the practice of locking down devices (like your home router) to prevent hackers from scanning your network, sniffing your traffic, or exploiting software vulnerabilities.

---

## Why do we need it?

Many small networks are built by simply daisy-chaining switches together in a long line. While this works, it creates two major problems:
1.  **Single Point of Failure (SPOF):** If a single switch in the middle dies, or a cable gets broken, every device downstream loses connection.
2.  **No Scale:** As you add more buildings, a flat network becomes too complex, slow, and expensive to wire and manage.

Structured designs use **Redundancy** (backup cables and devices) to ensure that if a link goes down, traffic automatically route-paths around it.

On the security side, standard consumer routers ship with dangerous default settings (like remote management and default passwords). Hardening them protects your personal devices from being compromised.

---

## How does it work?

Let's look first at how we structure corporate networks, and then how we secure our home networks.

### 1. Corporate Network Architecture

Cisco defines two standard designs depending on the size of the company:

#### Two-Tier (Collapsed Core) Design
Ideal for small-to-medium networks (like a single office building). It collapses the network into two layers:
1.  **Access Layer (Tier 1):** The local switches where your devices (computers, printers, APs) physically plug in.
2.  **Collapsed Core/Distribution Layer (Tier 2):** High-speed **Multi-layer Switches (Layer 3 Switches)** that act as the backbone. These special switches are smart: they can route traffic between subnets (Layer 3 IP routing) *and* handle local switching (Layer 2 MACs) at blazing fast speeds. They also handle security policies (Access Control Lists) and connect to the internet routers.

#### Three-Tier Campus Design
Ideal for large campuses spanning multiple buildings. It splits the collapsed core into two separate layers, creating three distinct tiers:
1.  **Access Layer:** Floor-level switches connecting end-user devices.
2.  **Distribution Layer:** Aggregates all the access switches inside a specific building. It handles inter-VLAN routing, security policies (Access Control Lists), and filtering.
3.  **Core Layer:** The ultimate backbone. It does *not* do slow policy filtering. Its only job is to be extremely fast, reliable, and have ultra-low latency, grunting and forwarding massive amounts of data between the different buildings (using beastly switches like the Cisco Catalyst 9600 series that push 25.6 Terabits per second).

| Design Model | Recommended Network Size | Key Layer Structures | Main Benefits |
| :--- | :--- | :--- | :--- |
| **Two-Tier (Collapsed Core)** | Small-to-medium offices | Access + Collapsed Core/Distribution | Simple setup, lower cost, unified routing/backbone. |
| **Three-Tier Campus** | Large multi-building campuses | Access + Distribution + Core | Scalable, clean cable layout, dedicated high-speed transit. |

---

### 2. Home Router Security Hardening (7 Key Steps)

Your home router is an all-in-one device (Router + Switch + Access Point + Firewall). To secure it, follow these 7 steps:

| Step | Action Item | Why It Matters |
| :---: | :--- | :--- |
| **1** | **Enable Built-in Firewall** | Blocks generic unsolicited incoming attacks. |
| **2** | **Disable Port Forwarding** | Prevents exposure of internal devices directly to the public web. |
| **3** | **Disable Remote Management** | Restricts admin access solely to devices inside the physical home network. |
| **4** | **Change Default Credentials** | Stops automated scripts from logging in using `admin/admin` or `admin/password`. |
| **5** | **Update Device Firmware** | Patches software vulnerabilities before hackers can exploit them. |
| **6** | **Secure Wireless & Guest network** | Encrypts Wi-Fi (WPA2/WPA3), hides hardware type (SSID), and isolates IoT/guests. |
| **7** | **Disable WAN Ping Responses** | Prevents your router from revealing its online presence during port scans. |

1.  **Enable the built-in Firewall:** Ensure it is turned on to block unauthorized incoming traffic.
2.  **Disable Port Forwarding:** Do not open paths from the public internet directly to your internal devices unless absolutely necessary.
3.  **Disable Remote Management:** Turn off the setting that allows your router to be configured from the internet (WAN side). Administrative access should only be allowed from the physical home network (LAN).
4.  **Change Default Credentials:** Never leave the username and password as `admin/admin` or `admin/password`. Hackers scan for these defaults first.
5.  **Update Device Firmware:** Manufacturers release updates to patch software vulnerabilities. If your router’s software is out of date, hackers can use known exploits to break in.
6.  **Secure Your Wireless (Wi-Fi) Settings:**
    *   Use the strongest security encryption standard: **WPA2** or **WPA3**.
    *   Change the default **SSID (Network Name)** from standard defaults (like `tp-link` or `netgear`). Generic names tell hackers exactly what hardware you have.
    *   Use a long, randomly generated password.
    *   **Enable a Guest Network:** Never let guests or smart home IoT devices (like Alexa or smart bulbs) connect to your primary Wi-Fi. Put them on an isolated Guest Network so if an IoT device is hacked, your primary computers remain safe. (In business networks, we do this using **VLANs** or Virtual Local Area Networks).
7.  **Disable WAN Ping Responses:** Configure your router to *not* respond to pings from the internet. When a hacker scans the web for targets, you want your router to stay completely silent (stealth mode) rather than saying "Yes, I'm here."

---

## Important Things to Remember

*   **SPOF (Single Point of Failure):** A vulnerability where one device or link failure drops the whole network. Fix it with **Redundancy**.
*   **Layer 3 Switches:** Switches that can also route packets using IP addresses, making local subnet-to-subnet traffic incredibly fast.
*   **Three-Tier Layers:**
    *   *Access:* Connects users.
    *   *Distribution:* Policies, security (ACLs), routing.
    *   *Core:* High-speed, low-latency backbone routing.
*   **SSID:** Service Set Identifier (the broadcasted name of your Wi-Fi network).
*   **Guest Network / VLAN:** Keeps untrusted or guest devices logically separated from your critical files.

---

## Diagram

### Two-Tier (Collapsed Core) vs. Three-Tier Architecture

```text
       [ TWO-TIER DESIGN ]                         [ THREE-TIER DESIGN ]

          +------------+                               +------------+
          |  Internet  |                               |  Internet  |
          +-----+------+                               +-----+------+
                |                                            |
          +-----+------+                               +-----+------+
          |   Router   |                               |   Router   |
          +-----+------+                               +-----+------+
                |                                            |
     ========================                     ========================
     COLLAPSED CORE/DISTRIBUTION                         CORE LAYER
     (Layer 3 Switches)                           (High-speed backbone switches)
      [Switch A]--[Switch B]                       [Core A]---------[Core B]
       /   \      /   \                             /    \           /    \
      /     \    /     \                           /      \         /      \
     +-------+--+-------+                         +--------+-------+--------+
     |                  |                         DISTRIBUTION LAYER
     |   ACCESS LAYER   |                         (Building-level Layer 3 routing)
     |  (User Switches) |                          [Dist A]---------[Dist B]
     |                  |                           /    \           /    \
     +------------------+                          /      \         /      \
                                                  +--------+-------+--------+
                                                  ACCESS LAYER
                                                  (User Switches inside building)
```

---

## Related Topics

*   [Network Devices](file:///home/trush/Downloads/x/02_network_devices.md) - Explains multi-layer switches, routers, and firewalls in detail.
*   [IP Addresses & Subnetting](file:///home/trush/Downloads/x/04_ip_addresses_and_subnetting.md) - How we design the logical subnet layout before applying routing policies in the distribution layer.
