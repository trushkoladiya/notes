# Basic Cryptography & Network Commands

![Symmetric vs Asymmetric Key Encryption]

When you browse the web, send passwords, or download files, you need to be sure that your data is private, that it hasn't been changed by hackers in transit, and that you can troubleshoot your connection when things break down. This is where **cryptography** and **command-line tools** come in.

---

## What is it?

**Cryptography** is the science of protecting information using mathematical equations. It ensures three things:
1.  **Confidentiality (Encryption):** Making data unreadable to snoops.
2.  **Integrity (Hashing):** Proving the data hasn't been changed.
3.  **Authenticity (Digital Signatures):** Proving who sent the data.

**Command-line Tools** (like `ping` and `ipconfig`) are basic network utility programs built into Windows, Mac, and Linux that help you test network connections and display your network adapter settings.

---

## Why do we need it?

Without cryptography, all data sent over the internet would travel in plain text. Any hacker sitting on your public Wi-Fi could read your passwords, credit cards, and private chats. 

We also need hashing to guarantee that files we download haven't been modified or corrupted (Data Integrity).

And when a network connection fails, we need standard troubleshooting commands so we don't have to guess where the break in the chain is.

---

## How does it work?

Let's explore how computers protect data and how we test connections.

### 1. Symmetric Encryption
*   **The Concept:** Symmetric encryption uses the **exact same secret key** to both encrypt (lock) and decrypt (unlock) the data.
*   **Analogy (The Caesar Cipher):** A simple cipher where you shift letters forward in the alphabet by a key number. If the secret key is `2`:
    *   `hello` becomes `jggnq` (Ciphertext).
    *   To decrypt, the receiver shifts letters backward by `2` to restore `hello` (Plaintext).
*   **Real-world Algorithms:** Computers use highly complex math like **AES (Advanced Encryption Standard)** (the current industry standard) and **Blowfish**.
*   **The Key Exchange Problem:** Both sides need the same key, but sending it over the wire is unsafe—a hacker could steal it.
*   **The Solution (Diffie-Hellman):** A protocol that uses complex math to allow both sides to independently calculate the same secret key without ever sharing the key itself over the network.

---

### 2. Asymmetric Encryption
*   **The Concept:** Also called "public-key cryptography," it uses a mathematically linked **key pair**:
    1.  **Public Key:** Shared with everyone.
    2.  **Private Key:** Kept strictly secret by the owner.
*   **The Rule:** If you encrypt data with the Public Key, *only* the corresponding Private Key can decrypt it. If you encrypt with the Private Key, *only* the corresponding Public Key can decrypt it.
*   **Real-world Algorithm:** **RSA** (named after its inventors Rivest, Shamir, and Adelman).
*   **How it works:**
    *   If Computer A wants to send a secret message to Computer B:
    *   A encrypts the message using **B's Public Key**.
    *   A sends the encrypted ciphertext over the internet.
    *   B decrypts it using **B's Private Key**. Even if a hacker intercepts it, they cannot decrypt it because they don't have B's private key.

| Feature | Symmetric Encryption | Asymmetric Encryption |
| :--- | :--- | :--- |
| **Number of Keys** | 1 key (shared secret key). | 2 keys (Public + Private key pair). |
| **Key Distribution** | Difficult (must securely share the single key). | Easy (public key is shared, private key is secret). |
| **Speed** | Blazing fast (great for bulk data). | Slow, mathematically intense (great for key exchange). |
| **Algorithm Examples** | AES, Blowfish, Caesar Cipher. | RSA, Diffie-Hellman. |
| **Key Use** | Same key encrypts and decrypts. | Public key encrypts, Private key decrypts. |

---

### 3. Hashing
*   **The Concept:** Hashing is a **one-way function** that takes any input size (a letter, a PDF, or a 10GB movie) and calculates a **fixed-length random output** (called a hash value, checksum, or digital fingerprint).

```text
[ HASHING FUNCTION (One-Way & Fixed Length) ]

  Input A: "B" (1 byte) ─────────┐
                                 ▼
                         ┌──────────────┐
                         │ Hash Function│ ───> Hash Output A: "a3f89e2c" (Fixed size)
                         └──────────────┘
                                 ▲
  Input B: Video File (2GB) ─────┘
                                 ▼
                         ┌──────────────┐
                         │ Hash Function│ ───> Hash Output B: "9b2d7f1e" (Same fixed size!)
                         └──────────────┘
```

*   **Key Characteristics:**
    *   **One-Way:** You can easily convert data -> hash, but it is mathematically impossible to convert hash -> data.
    *   **Fixed Length:** The hash is always the same length (e.g., SHA-256 always produces 64 characters) regardless of input size.
    *   **Unique:** Even a tiny change (changing a capital letter to lowercase) changes the hash output completely (the avalanche effect).
    *   **Collision Resistant:** A collision occurs if two different inputs produce the exact same hash output. Good algorithms (like **SHA-256**) make collisions practically impossible.
*   **How it checks Data Integrity:**
    1.  Website displays a file's hash (e.g. MD5 or SHA-256) next to the download button.
    2.  You download the file.
    3.  You run a hashing tool on your computer to calculate the downloaded file's hash.
    4.  If the hashes match, your file is unaltered and safe. If they don't, the file was corrupted or tampered with.

---

### 4. Basic Network Commands

#### The `ping` Command
*   **What it does:** Uses **ICMP (Internet Control Message Protocol)** packets to send a "hello" message to a destination IP or domain name and waits for a reply. It is the gold standard for checking if a host is alive.
*   **Command usage:**
    *   *Windows:* `ping google.com` (sends 4 packets by default and stops).
    *   *Linux/Mac:* `ping google.com` (runs forever until stopped manually with **Ctrl + C**).
    *   *Limit packets in Linux:* `ping -c 5 google.com` (sends exactly 5 packets).
*   **Reading results:**
    *   *100% Packets Received:* Target is online, and your internet is working.
    *   *Error: "Temporary failure in name resolution":* Your internet is down, or your DNS settings are wrong (cannot translate name to IP).
    *   *0% Packets Received:* The host is either down, or its firewall is blocking ICMP packets for security.

#### The `ipconfig` (Windows) / `ifconfig` (Linux/Mac) Command
*   **What it does:** Displays the IP address configuration of your network interface cards.
*   **Windows Usage:**
    *   `ipconfig /all`: Reveals every network detail, including your physical MAC address, current IPv4/IPv6 addresses, subnet mask, default gateway (router), and DNS servers.

#### The `traceroute` (Linux) / `tracert` (Windows) Command
*   **What it does:** Traces the path a packet takes hop-by-hop from your computer to a destination IP or domain name.
*   **When to use it:** Diagnosing where a connection dies. If you cannot reach a website, `traceroute` shows you exactly which router along the path dropped your packet.
*   **Command usage:**
    *   *Windows:* `tracert google.com`
    *   *Linux:* `traceroute google.com` (or `tracepath google.com`)
*   **What the output looks like:**
    ```text
    $ tracepath -m 5 google.com
     1?: [LOCALHOST]                        0.015ms pmtu 1500
     1:  2409:40c1:4019:25b5::5                                5.069ms
     2:  no reply
     3:  2405:200:5210:3:3925::1                             345.072ms asymm  4
     4:  no reply
    ```
    Each line represents a "hop" (a router). If you see `* * *` or `no reply`, it means that router timed out or blocked the request, helping you pinpoint where the network connection died.

#### The `netstat -an` / `ss -tulnp` Command
*   **What it does:** Displays active network connections and listening ports on your machine.
*   **Why it matters:** Checking which service is listening on which port. This is essential for verifying if your web server (e.g., Apache on port 80/443) or SSH server (port 22) is actually running and listening for traffic.
*   **Command usage:**
    *   *Windows/Linux:* `netstat -an` (lists all active connections and ports numerically).
    *   *Linux:* `ss -tulnp` (lists listening TCP/UDP ports and shows the process names/PIDs running them).
*   **What the output looks like (`ss -tulnp`):**
    ```text
    $ ss -tulnp
    Netid State  Recv-Q Send-Q   Local Address:Port    Peer Address:Port  Process
    udp   UNCONN 0      0            127.0.0.1:53           0.0.0.0:*
    tcp   LISTEN 0      4096         127.0.0.1:631          0.0.0.0:*
    tcp   LISTEN 0      32           127.0.0.1:53           0.0.0.0:*
    tcp   LISTEN 0      10           127.0.0.1:34949        0.0.0.0:*     users:(("antigravity",pid=57398,fd=68))
    ```
    *   `LISTEN` means the port is open and waiting for incoming connections.
    *   `Local Address:Port` shows the IP and port number (e.g., `127.0.0.1:53` is DNS on localhost).

#### The `curl -I` Command
*   **What it does:** Fetches only the response headers from a web server without downloading the HTML body.
*   **Why it matters:** Checking server status codes and server software types directly from the command line.
*   **Command usage:** `curl -I https://google.com`
*   **What the output looks like:**
    ```text
    $ curl -I https://google.com
    HTTP/2 301
    location: https://www.google.com/
    content-type: text/html; charset=UTF-8
    date: Fri, 17 Jul 2026 11:33:12 GMT
    cache-control: public, max-age=2592000
    server: gws
    ```
    *   `HTTP/2 301` shows the HTTP version and status code (301 Moved Permanently).
    *   `server: gws` tells you the server software (Google Web Server).

#### The `dig` Command
*   **What it does:** A powerful DNS lookup utility for querying DNS name servers.
*   **Why it matters:** Troubleshooting DNS issues. It shows the raw DNS records returned, query time, and the server that responded.
*   **Command usage:** `dig google.com`
*   **What the output looks like:**
    ```text
    $ dig google.com
    ; <<>> DiG 9.20.18-1ubuntu2.1-Ubuntu <<>> google.com
    ;; QUESTION SECTION:
    ;google.com.                    IN      A

    ;; ANSWER SECTION:
    google.com.             0       IN      A       216.239.38.120

    ;; Query time: 1 msec
    ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
    ```
    *   `QUESTION SECTION` shows what you queried (`google.com` type `A` record).
    *   `ANSWER SECTION` shows the result (IP address `216.239.38.120`).
    *   `SERVER: 127.0.0.53#53` shows that your local DNS resolver resolved the query.

---

## Important Things to Remember

*   **Symmetric:** One key for both locking and unlocking. Fast, but requires secure key exchange (Diffie-Hellman).
*   **Asymmetric:** Two keys (Public and Private). More secure because private keys are never shared, but mathematically slower.
*   **Hashing:** One-way, fixed-length fingerprint used to verify **Integrity** (no changes).
*   **Collision:** When two different inputs produce the same hash. Secure hashes avoid this.
*   **Ping:** Troubleshooting tool that sends ICMP packets.
*   **ipconfig /all:** Displays active adapter configuration, including IPs, default gateways, and MAC addresses.

---

## Diagram

### How Asymmetric Encryption Keeps a Message Private

```text
[ Sender: Computer A ]                                     [ Receiver: Computer B ]
                                                                 
                                                               B's Public Key
  Plaintext message: "Hello"                                   (Shared publicly)
            |                                                        |
            v                                                        v
   [ Encrypt with B's Public Key ]  ---------------------------------+
            |
            v
   Ciphertext: "jggnq" (encrypted)
            |
            +------------( Travels over Internet )------------+
                                                              |
                                                              v
                                                   [ Decrypt with B's Private Key ]
                                                              |
                                                              v
                                                  Plaintext message: "Hello"
                                                  (Decrypted successfully!)
```

---

## Related Topics

*   [Introduction to Networking Basics](file:///home/trush/Downloads/x/01_networking_basics.md) - The basics of IP and MAC addresses.
*   [The OSI & TCP/IP Models](file:///home/trush/Downloads/x/03_osi_and_tcpip_models.md) - How encryption fits into the Presentation Layer (Layer 6).
