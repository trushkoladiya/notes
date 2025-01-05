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
