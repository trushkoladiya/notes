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
