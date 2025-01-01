# IP Addresses & Subnetting

![IP Address Network and Host Portion]

If there is one skill that separates beginner networkers from true networking professionals, it is **subnetting**. Subnetting is the art of cutting up IP address blocks into smaller, secure, and efficient networks. 

Let's demystify it together, starting from what an IP address actually is, how to convert binary code, and how to slice up networks using Variable Length Subnet Masking (VLSM).

---

## What is it?

An **IP address** (specifically IPv4) is a logical 32-bit address assigned to every device on a network. It is written in "dotted decimal" format (like `192.168.1.10`), which consists of four sections called **octets** separated by three dots. 

Each octet is 8 bits long, meaning each of the four numbers can range from `0` to `255`. 

An IP address is split into two halves:
1.  **Network Portion:** Identifies which "street" the device lives on.
2.  **Host Portion:** Identifies the specific device (like a house number) on that street.

```text
       IP Address: 192.168. 1 . 50
                   \_________/   \__/
                     Street      House
                    (Network)    (Host)
```

### The Subnet Mask
A computer cannot look at a raw IP address and know where the network portion ends and the host portion begins. It needs **Mr. Subnet Mask** to tell it.
*   The subnet mask looks like an IP address (e.g., `255.255.255.0`).
*   In binary, the subnet mask is a string of contiguous `1`s followed by contiguous `0`s. 
*   **The Ones** tell the computer: "This part is the Network."
*   **The Zeros** tell the computer: "This part is the Host."
*   **CIDR Notation (Classless Inter-Domain Routing):** Instead of writing out `255.255.255.0`, we write `/24`, which simply means the first 24 bits of the subnet mask are `1`s.

---

## Why do we need it?

Why can't we just put every device in our house or office on the same network block?
1.  **Security:** You don't want your guest Wi-Fi users, your critical office database servers, and your smart IoT toilet on the same subnet. If a hacker takes over a smart bulb, they can easily access your servers if they are on the same local network.
2.  **Traffic Control:** Devices constantly broadcast messages (like ARP requests) to everyone on their subnet. If a network is too large (like thousands of devices on a single flat switch), the broadcast traffic will saturate the cables and slow the network to a crawl.
3.  **Address Conservation:** Public IP addresses are expensive. Subnetting allows us to allocate exactly the number of addresses we need, avoiding waste.

---

## How does it work?

To master subnetting, you need to know how computers read IP addresses (Binary), how to subnet based on network or host requirements, and how to read subnets in reverse.

### 1. Decimal to Binary Conversion (The "Nosferatu" Chart)
Computers only understand ones and zeros (binary). Each bit in an octet has a positional value based on powers of 2. Memorize this chart:

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|---|---|---|---|---|---|---|---|

#### Converting Decimal to Binary
Let's convert the number **168** to binary:

```text
[ DECIMAL TO BINARY SUBTRACTION FLOW ]

   168 ───( Can subtract 128? YES )───> [ 1 ] ───( Remaining: 40 )
                                                     │
   40  ───( Can subtract 64?  NO  )───> [ 0 ] ───────┘
                                                     │
   40  ───( Can subtract 32?  YES )───> [ 1 ] ───( Remaining: 8 )
                                                     │
   8   ───( Can subtract 16?  NO  )───> [ 0 ] ───────┘
                                                     │
   8   ───( Can subtract 8?   YES )───> [ 1 ] ───( Remaining: 0 )
                                                     │
   0   ───( The rest are all 0s )─────> [ 0 ], [ 0 ], [ 0 ]

Result: 1 0 1 0 1 0 0 0
```

![Binary to Decimal Conversion Chart]

1.  Can you subtract `128` from `168`? Yes. Put a **1** under 128. (`168 - 128 = 40`)
2.  Can you subtract `64` from `40`? No. Put a **0** under 64.
3.  Can you subtract `32` from `40`? Yes. Put a **1** under 32. (`40 - 32 = 8`)
4.  Can you subtract `16` from `8`? No. Put a **0** under 16.
5.  Can you subtract `8` from `8`? Yes. Put a **1** under 8. (`8 - 8 = 0`)
6.  The rest are **0**s.

```text
Chart:  128  64  32  16   8   4   2   1
Bits:    1   0   1   0   1   0   0   0   => 10101000 in binary
```

#### Converting Binary to Decimal
Let's convert the binary string **11000000** back to decimal. Simply add up the chart values where the bit is `1`:
`128 + 64 = 192`.

---

### 2. How to Subnet Step-by-Step
Subnetting is the process of stealing bits from the **Host portion** (zeros) and giving them to the **Network portion** (ones) to create more subnets.

#### The Magic Formulas
*   **Number of Subnets created:** `2^n` (where `n` is the number of stolen network bits).
*   **Number of usable Hosts per Subnet:** `2^h - 2` (where `h` is the number of remaining host bits/zeros). 
    *   *Why subtract 2?* Every subnet reserves the **first** address (Subnet ID) and the **last** address (Broadcast address). You cannot assign these to devices.

---

### Example A: Subnetting by Network Requirements (Creating 4 Subnets)
Suppose we start with the network `192.168.1.0/24`. The default subnet mask in binary is:
`11111111.11111111.11111111.00000000` (three octets of ones, last octet of zeros).

We need **4 networks** for our home (User, IoT, WAPs, Guest).
1.  **Find how many bits to steal:** Look at the powers of 2. `2^2 = 4`. We need to steal **2 bits** from the host portion.
2.  **Modify the Mask:** We steal 2 bits from the left side of the last octet.
    *   Old last octet: `00000000`
    *   New last octet: `11000000` (The mask becomes `/26` or `255.255.255.192` since `128 + 64 = 192`).
3.  **Find the Increment:** The increment is the decimal value of the *last network bit* (the rightmost `1` in the mask).
    *   In our last octet (`11000000`), the last `1` is in the `64` column. Our **increment is 64**.
4.  **Write out the Subnets:** Add the increment (`64`) starting from `.0`:
    *   **Subnet 1:** `192.168.1.0/26` (Usable IP range: `.1` to `.62`, Broadcast: `.63`)
    *   **Subnet 2:** `192.168.1.64/26` (Usable IP range: `.65` to `.126`, Broadcast: `.127`)
    *   **Subnet 3:** `192.168.1.128/26` (Usable IP range: `.129` to `.190`, Broadcast: `.191`)
    *   **Subnet 4:** `192.168.1.192/26` (Usable IP range: `.193` to `.254`, Broadcast: `.255`)

---

### Example B: Subnetting by Host Requirements (Designing a Coffee Shop)
Suppose we have a network block `10.1.1.0/24` and we need to design a network for a coffee shop. The shop needs **40 host addresses** (wiggle room included).

1.  **Find how many host bits to save:** How many zeros do we need to keep from right to left?
    *   `2^5 = 32` (Too small, we need 40).
    *   `2^6 = 64` (Perfect). We must **save 6 host bits (zeros)**.
2.  **Modify the Mask:** Starting from the right, keep 6 zeros, and turn the rest into ones.
    *   New last octet: `11000000` (Mask is `/26` or `255.255.255.192`).
3.  **Find the Increment:** The last network bit is in the `64` column. The **increment is 64**.
4.  **Write out the Subnet:** 
    *   `10.1.1.0/26` (Usable: `10.1.1.1` to `10.1.1.62`, Broadcast: `10.1.1.63`). This gives us 62 usable hosts, which easily covers our requirement of 40.

---

### Example C: Reverse Subnetting (Troubleshooting a Ticket)
You get a ticket: *"Beatrice cannot connect to anything. Her IP is `172.17.12.255` and the mask is `255.255.240.0` (/20). Her default gateway is `172.17.0.1`."* Let's reverse-engineer this configuration:

1.  **Convert the Subnet Mask to Binary:**
    *   `255.255.240.0` => `11111111.11111111.11110000.00000000`
2.  **Find the Increment:**
    *   The subnetting is happening in the **third octet** (`11110000`).
    *   The last `1` is in the 4th column (`128 | 64 | 32 | 16`), so the **increment is 16**.
3.  **Build the Subnet Boundaries:**
    *   Subnet 1: `172.17.0.0` to `172.17.15.255` (Broadcast: `172.17.15.255`)
    *   Subnet 2: `172.17.16.0` to `172.17.31.255`
4.  **Analyze Beatrice's IP:**
    *   Beatrice's IP is `172.17.12.255`. Since it falls between `172.17.0.0` and `172.17.15.255`, she belongs to the first subnet.
    *   **Is her IP valid?** Yes! In this `/20` subnet, the broadcast address is `172.17.15.255`. Thus, `172.17.12.255` is a valid usable host address!
    *   **What is the problem?** Her default gateway is `172.17.0.1`. The default gateway belongs to the same subnet, but wait: is the router physically configured on that subnet? Let's check the router. If the router's interface is configured in a different subnet, she won't be able to route traffic.

---

### Variable Length Subnet Masking (VLSM)
In the real world, you don't cut subnets into equal sizes. You want to subnet a subnet so that different groups have different sized networks to avoid wasting IPs. This is called **VLSM**.

> **CRITICAL RULE FOR VLSM:**
> Always subnet for the **largest host requirement first**, then the next largest, down to the smallest. Start each subnet immediately where the previous one ended.

#### VLSM Example
Address Block: `172.21.42.0/24` (256 addresses total). 
Requirements: Workers (117 hosts), Robots (57 hosts), Servers (26 hosts), Guest (10 hosts).

```text
[ VLSM SUBNET TREE ]

                     172.21.42.0/24 (256 IPs)
                                │
            ┌───────────────────┴───────────────────┐
     Workers (/25)                           [Remaining]
   172.21.42.0/25 (128 IPs)            172.21.42.128/25 (128 IPs)
   Usable: .1 - .126                                │
                                  ┌─────────────────┴─────────────────┐
                           Robots (/26)                        [Remaining]
                         172.21.42.128/26 (64 IPs)           172.21.42.192/26 (64 IPs)
                         Usable: .129 - .190                         │
                                                       ┌─────────────┴─────────────┐
                                                Servers (/27)               [Remaining]
                                              172.21.42.192/27 (32 IPs)   172.21.42.224/27 (32 IPs)
                                              Usable: .193 - .222                  │
                                                                           ┌───────┴───────┐
                                                                     Guests (/28)       Unused
                                                                   172.21.42.224/28    .240-.255
                                                                   Usable: .225-.238   (16 IPs)
```

![VLSM sizing chart mapping hosts to subnets]

1.  **Workers (117 hosts):**
    *   We need `2^7 = 128` addresses (7 host bits). Subnet mask is `/25`.
    *   **Subnet:** `172.21.42.0/25` (Range: `.0` to `.127`, Broadcast: `.127`).
2.  **Robots (57 hosts):**
    *   We pick up exactly where the last subnet ended: **`.128`**.
    *   We need `2^6 = 64` addresses (6 host bits). Subnet mask is `/26`.
    *   **Subnet:** `172.21.42.128/26` (Range: `.128` to `.191`, Broadcast: `.191`).
3.  **Servers (26 hosts):**
    *   We pick up at **`.192`**.
    *   We need `2^5 = 32` addresses (5 host bits). Subnet mask is `/27`.
    *   **Subnet:** `172.21.42.192/27` (Range: `.192` to `.223`, Broadcast: `.223`).
4.  **Guests (10 hosts):**
    *   We pick up at **`.224`**.
    *   We need `2^4 = 16` addresses (4 host bits). Subnet mask is `/28`.
    *   **Subnet:** `172.21.42.224/28` (Range: `.224` to `.239`, Broadcast: `.239`).

---

## Important Things to Remember

*   **Network address:** The first IP in a subnet (e.g. `.0`). Reserved, cannot be used.
*   **Broadcast address:** The last IP in a subnet (e.g. `.255`). Reserved, sends packets to everyone.
*   **Stealing bits:** Steal from the *left* of the host bits when creating networks.
*   **Saving bits:** Save bits from the *right* of the host bits when building for host counts.
*   **Increment:** The value of the last network bit. It is the step size of your subnets.
*   **VLSM Order:** Always address from the **largest** network requirement to the **smallest**.

---

## Diagram

Here is how a single `/24` network is divided into four unequal subnets using VLSM:

```text
+---------------------------------------------------------------------------------+
|                                 172.21.42.0/24                                  |
+----------------------------------------+----------------------------------------+
|             172.21.42.0/25             |             172.21.42.128/25           |
|            Workers (128 IPs)           |                 (Split)                |
|           .0 - .127 usable             +--------------------+-------------------+
|                                        |  172.21.42.128/26  | 172.21.42.192/26  |
|                                        |  Robots (64 IPs)   |      (Split)      |
|                                        | .128 - .191 usable +---------+---------+
|                                        |                    |  /27    |  /27    |
|                                        |                    | Servers | Guests  |
|                                        |                    | (32 IPs)| (32 IPs)|
+----------------------------------------+--------------------+---------+---------+
```

---

## Related Topics

*   [NAT & Private IPs](file:///home/trush/Downloads/x/05_nat_and_private_ips.md) - How we use private subnets to address internal devices and connect them to the internet.
*   [Network Design & Security](file:///home/trush/Downloads/x/06_network_design_and_security.md) - How we assign these subnets to physical structures using VLANs.
