# How HTTPS Works

## 1. Introduction to HTTPS

HTTPS (Hyper Text Transfer Protocol Secure) is the secure version of HTTP. It ensures that communication between a client (browser) and a server is **confidential, authenticated, and tamper-proof**. HTTPS achieves this by using **TLS (Transport Layer Security)** on top of TCP.

In simple terms:

> HTTPS = HTTP + TLS
> 

---

## 2. Layers Involved in HTTPS Communication

HTTPS operates across multiple layers of the network stack:

1. **Application Layer** – HTTP (requests and responses)
2. **Security Layer** – TLS (encryption, authentication, integrity)
3. **Transport Layer** – TCP (reliable connection)
4. **Network Layer** – IP (routing)

---

## 3. TCP Connection Establishment

Before any security is applied, a TCP connection must be established.

### TCP Three-Way Handshake

1. Client sends **SYN**
2. Server replies with **SYN-ACK**
3. Client responds with **ACK**

After this, a reliable connection is established on port **443**.

---

## 4. TLS Handshake (Core of HTTPS)

The TLS handshake sets up a **secure communication channel** before any HTTP data is exchanged.

### 4.1 Client Hello

The client sends:

- Supported TLS versions
- Supported cipher suites
- Client random value
- Extensions (SNI, ALPN, supported groups)

Purpose:

- Propose security parameters
- Start key agreement

---

### 4.2 Server Hello

The server replies with:

- Chosen TLS version
- Chosen cipher suite
- Server random value
- Key share (for key exchange)

Purpose:

- Confirm cryptographic parameters

---

### 4.3 Certificate

The server sends its **digital certificate**, which contains:

- Server’s public key
- Domain name
- Validity period
- CA digital signature

Purpose:

- Authenticate the server’s identity

---

### 4.4 Certificate Verification (Client Side)

The client verifies:

- CA signature using trusted root certificates
- Domain name match
- Certificate validity and revocation status

If verification fails, the connection is terminated.

---

### 4.5 Certificate Verify (TLS 1.3)

The server signs the handshake transcript using its **private key**.

Purpose:

- Prove ownership of the private key corresponding to the certificate

---

### 4.6 Finished Messages

Both client and server send **Finished** messages containing a MAC over the entire handshake.

Purpose:

- Ensure handshake integrity
- Confirm that no tampering occurred

At this point, the secure channel is established.

---

## 5. Key Exchange and Session Keys

Using the exchanged random values and key shares:

- Both sides derive the **same symmetric session keys**
- Asymmetric cryptography is used only during the handshake
- Symmetric encryption is used for all data transfer

Reason:

- Symmetric encryption is significantly faster

---

## 6. Encrypted HTTP Data Transfer

After the handshake:

- HTTP requests and responses are encrypted
- Encryption ensures confidentiality
- Message authentication ensures integrity
- Sequence numbers prevent replay attacks

Even though data is encrypted:

- IP addresses, ports, and TCP flags remain visible

---

## 7. Connection Termination

The HTTPS connection is closed using TCP termination:

- FIN → ACK → FIN → ACK
- Or abruptly using RST in case of errors

---

## 8. Security Guarantees Provided by HTTPS

### 8.1 Confidentiality

Prevents attackers from reading data.

### 8.2 Integrity

Prevents modification of data in transit.

### 8.3 Authentication

Ensures communication with the legitimate server.

---

## 9. What HTTPS Does NOT Provide

HTTPS does not:

- Hide user IP addresses
- Guarantee website trustworthiness
- Protect against malicious content from a legitimate site
- Provide anonymity

---

## 10. TLS 1.3 Improvements (Brief)

- Fewer handshake messages
- Faster connection establishment
- Stronger default cryptography
- Removal of insecure legacy features
- More encrypted handshake metadata
