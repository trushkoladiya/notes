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
