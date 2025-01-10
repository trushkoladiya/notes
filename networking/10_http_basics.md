# HTTP Basics & Web Security

![HTTP Request and Response Banner]

Every time you browse a website, interact with an API, download a Docker image, or run a `curl` command, you are using the foundation of the World Wide Web: **HTTP (Hypertext Transfer Protocol)**. 

Let's demystify how HTTP works, how requests and responses are structured, and how HTTPS keeps your connection secure.

---

## What is it?

**HTTP** stands for **Hypertext Transfer Protocol**. It is an Application Layer (Layer 7) protocol used by web browsers and servers to communicate. It is a client-server protocol, meaning a client (like your browser) initiates a request, and a server replies with a response.

**HTTPS** stands for **Hypertext Transfer Protocol Secure**. It is the secure version of HTTP. It encrypts all communication using **SSL/TLS** (Secure Sockets Layer / Transport Layer Security) so that no one can eavesdrop on your data.

---

## Why do we need it?

We need a standardized protocol so that any web browser (Chrome, Firefox, Safari) can talk to any web server (Apache, Nginx, IIS) without needing custom configurations. 

We need **HTTPS** specifically because raw HTTP transmits all data—including usernames, passwords, and credit card numbers—in plain text. Anyone on your local Wi-Fi or router path could intercept this data (a man-in-the-middle attack). HTTPS encrypts this data, making it completely unreadable to interceptors.

---

## How does it work?

Let's break down HTTP methods, status codes, message structures, and secure handshakes.

### 1. HTTP Methods
When a client sends a request to a server, it specifies an **HTTP Method** (or verb) to declare what action it wants to perform:

| Method | What it does (One-Line Summary) | Typical Action |
| :---: | :--- | :--- |
| **GET** | Retrieves data or webpages from the server. | Loading a blog post or website. |
| **POST** | Submits new data to the server to create a resource. | Submitting a login form or registration. |
| **PUT** | Replaces or updates an entire existing resource. | Updating your user profile details. |
| **PATCH** | Partially modifies an existing resource. | Changing just your profile profile picture. |
| **DELETE** | Removes a specified resource from the server. | Deleting a blog post or account. |

---

### 2. HTTP Status Codes
Every response from a server starts with a 3-digit **Status Code** indicating if the request succeeded, failed, or needs redirection:

| Code | Status Name | Category | What it means |
| :---: | :--- | :---: | :--- |
| **200** | OK | Success (2xx) | The request was successful, and the data is returned. |
| **201** | Created | Success (2xx) | The request succeeded, and a new resource was created. |
| **301** | Moved Permanently | Redirection (3xx) | The requested page has moved to a new permanent URL. |
| **400** | Bad Request | Client Error (4xx) | The server couldn't understand the request (bad syntax). |
| **401** | Unauthorized | Client Error (4xx) | You must log in/authenticate to access this resource. |
| **403** | Forbidden | Client Error (4xx) | You are logged in, but you don't have permission to view it. |
| **404** | Not Found | Client Error (4xx) | The server cannot find the requested webpage or file. |
| **500** | Internal Server Error | Server Error (5xx) | The server encountered a generic error in its backend code. |
| **502** | Bad Gateway | Server Error (5xx) | An intermediate proxy server got an invalid response from upstream. |
| **503** | Service Unavailable | Server Error (5xx) | The server is overloaded, down, or undergoing maintenance. |

---

### 3. HTTP Request & Response Structure
HTTP messages are written in plain text (before encryption). Here is how they are structured:

```text
[ HTTP REQUEST STRUCTURE ]

  ┌────────────────────────────────────────────────────────┐
  │ GET /index.html HTTP/1.1              <-- Status Line  │
  ├────────────────────────────────────────────────────────┤
  │ Host: www.google.com                  <-- Headers      │
  │ User-Agent: Mozilla/5.0                                │
  │ Accept: text/html                                      │
  ├────────────────────────────────────────────────────────┤
  │ (Empty Line)                                           │
  ├────────────────────────────────────────────────────────┤
  │ [ Optional Request Body (e.g. form data) ]             │
  └────────────────────────────────────────────────────────┘

[ HTTP RESPONSE STRUCTURE ]

  ┌────────────────────────────────────────────────────────┐
  │ HTTP/1.1 200 OK                       <-- Status Line  │
  ├────────────────────────────────────────────────────────┤
  │ Date: Fri, 17 Jul 2026 12:00:00 GMT   <-- Headers      │
  │ Server: Apache/2.4.41                                  │
  │ Content-Type: text/html                                │
  ├────────────────────────────────────────────────────────┤
  │ (Empty Line)                                           │
  ├────────────────────────────────────────────────────────┤
  │ <html><body><h1>Hello World!</h1></body></html> <--Body│
  └────────────────────────────────────────────────────────┘
```

*   **Status Line (Request):** Declares the Method (`GET`), the path (`/index.html`), and the HTTP version (`HTTP/1.1`).
*   **Status Line (Response):** Declares the HTTP version and the Status Code/Reason (`200 OK`).
*   **Headers:** Key-value pairs containing metadata about the message (content type, server type, date, cookies, user browser type).
*   **Body:** The actual content. For requests, it contains form data or JSON files. For responses, it contains the HTML webpage, images, or API files.

---

### 4. What HTTPS Adds Over HTTP
HTTPS is simply HTTP running on top of an encrypted connection. It adds a security layer using **SSL/TLS**.

```text
[ HTTP vs. HTTPS SECURITY LAYERS ]

        [ HTTP ]                         [ HTTPS ]
   ┌─────────────────┐             ┌─────────────────┐
   │ 7. Application  │             │ 7. Application  │ (HTTP Data)
   ├─────────────────┤             ├─────────────────┤
   │ 4. Transport    │             │    SSL / TLS    │ (Encryption Layer)
   └─────────────────┘             ├─────────────────┤
                                   │ 4. Transport    │ (TCP Port 443)
                                   └─────────────────┘
```

*   **Port Change:** HTTP uses **Port 80** by default. HTTPS uses **Port 443** by default.
*   **The TLS Handshake:** Before sending any HTTP data, the client and server perform a handshake:

```text
[ SSL/TLS HANDSHAKE FLOW ]

  Client (Browser)                               Web Server
         │                                           │
         │──────── 1. ClientHello (Cipher list) ────>│
         │                                           │
         │<─────── 2. ServerHello & Certificate ─────│ (Includes Server Public Key)
         │                                           │
         │──────── 3. Verify Cert & Key Exchange ───>│ (Client encrypts pre-master
         │                                           │  secret with server public key)
         │<─────── 4. Session Keys Established ──────│ (Symmetric session key generated)
         │                                           │
         │<====== Encrypted HTTPS Data Exchanged ===>│ (All subsequent data encrypted)
```

    1.  **Cipher Agreement:** Client and server agree on which encryption algorithms to use.
    2.  **Certificate Verification:** The server sends its **SSL/TLS Certificate** containing its **Public Key**. The client verifies this certificate with trusted authorities to ensure the website is genuine (e.g., verifying it's really google.com and not a fake site).
    3.  **Key Exchange:** The client and server securely establish a symmetric session key.
    4.  **Symmetric Encryption:** Once established, all HTTP headers and body data are encrypted using this symmetric key, making the connection private and tamper-proof.

---

## Important Things to Remember

*   **HTTP Methods:** Action verbs (`GET` for reading, `POST` for creating, `PUT`/`PATCH` for updating, `DELETE` for removing).
*   **Status Code Ranges:** 
    *   `2xx` = Success
    *   `3xx` = Redirection
    *   `4xx` = Client Error (your fault)
    *   `5xx` = Server Error (server's fault)
*   **HTTPS Encryption:** Uses asymmetric encryption (certificates/public keys) to securely exchange keys, then switches to fast symmetric encryption for the rest of the web traffic.

---

## Related Topics

*   [The OSI & TCP/IP Models](file:///home/trush/Downloads/x/networking/03_osi_and_tcpip_models.md) - How HTTP runs on top of TCP.
*   [Basic Cryptography & Network Commands](file:///home/trush/Downloads/x/networking/07_cryptography.md) - Explains symmetric vs asymmetric encryption and using `curl -I` to inspect status codes.
