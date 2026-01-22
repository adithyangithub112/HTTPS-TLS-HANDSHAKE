# **TLS / HTTPS Handshake — Notes**

<img width="2789" height="3183" alt="image" src="https://github.com/user-attachments/assets/018a82ad-e07a-4cfa-8f7a-10875564cdad" />
<img width="474" height="379" alt="image" src="https://github.com/user-attachments/assets/bfa0ca78-4c98-4412-a1ba-afac3d554426" />


## 1️⃣ Purpose of the TLS Handshake

- Establish a **secure communication channel**
- Authenticate the **server identity**
- Securely derive a **shared symmetric session key**
- Prevent **eavesdropping, tampering, and MITM attacks**

---

## 2️⃣ Information Exchanged During the Handshake

### 🔹 What the **Client Sends**

- **ClientHello**
    - Supported TLS versions
    - Supported cipher suites
    - Client random value
- **Ephemeral public key** (for ECDHE key exchange)
- **Finished message** (verifies handshake integrity)

---

### 🔹 What the **Server Sends**

- **ServerHello**
    - Selected TLS version
    - Selected cipher suite
    - Server random value
- **Digital Certificate**
    - Contains server public key
    - Issued by a trusted Certificate Authority (CA)
- **Ephemeral public key** (for ECDHE)
- **Certificate signature**
    - Proves server authenticity
- **Finished message**

---

## 3️⃣ What Is NEVER Sent

- ❌ Private keys
- ❌ Symmetric session keys
- ❌ Plaintext application data

---

## 4️⃣ Key Exchange (Forward Secrecy)

- Uses **ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)**
- Both sides:
    - Generate temporary (ephemeral) private keys
    - Exchange only **public values**
- Session key is:
    - Derived independently by both sides
    - Never transmitted
    - Destroyed after session ends

---

## 5️⃣ After the Handshake

- Both client and server:
    - Switch to **symmetric encryption**
    - Use algorithms like **AES or ChaCha20**
- All HTTP data is:
    - Encrypted
    - Integrity-protected
    - Authenticated

---

# **Difference Between HTTP and HTTPS**

## 1️⃣ HTTP (HyperText Transfer Protocol)

- Transmits data in **plaintext**
- Provides **no encryption**
- Provides **no authentication**
- Provides **no data integrity**
- Vulnerable to:
    - Packet sniffing
    - Man-in-the-Middle (MITM) attacks
    - Data modification
- Commonly uses **port 80**
- Unsafe for:
    - Login forms
    - Payment data
    - Sensitive information

---

## 2️⃣ HTTPS (HyperText Transfer Protocol Secure)

- Uses **TLS (Transport Layer Security)** to secure communication
- Encrypts data using **symmetric encryption**
- Authenticates the server using **digital certificates**
- Ensures **data integrity**
- Protects against:
    - Eavesdropping
    - MITM attacks
    - Data tampering
- Commonly uses **port 443**
- Required for:
    - Secure logins
    - Online payments
    - Compliance standards (PCI-DSS, GDPR)

---

## 3️⃣ Key Differences (Comparison Table)

| Feature | HTTP | HTTPS |
| --- | --- | --- |
| Encryption | ❌ None | ✅ Encrypted (TLS) |
| Authentication | ❌ No | ✅ Yes (Certificates) |
| Data Integrity | ❌ No | ✅ Yes |
| MITM Protection | ❌ No | ✅ Yes |
| Port Number | 80 | 443 |
| Security Level | Low | High |
| Usage Today | Deprecated | Standard |

---

## 4️⃣ Real-World Security Impact

- **HTTP**
    - Attackers can read and modify traffic
    - Passwords and cookies can be stolen easily
- **HTTPS**
    - Traffic is unreadable to attackers
    - Session hijacking is prevented
    - Server identity is verified

# **How HTTPS Secures Communication**

### 1. Encryption (Confidentiality)

- Data is encrypted using **symmetric encryption** (AES/ChaCha20)
- Prevents attackers from reading transmitted data
- Encrypted traffic appears as ciphertext

---

### 2. Authentication

- The server presents a **digital certificate**
- Certificate:
    - Issued by a trusted **Certificate Authority (CA)**
    - Confirms the server’s identity
- Prevents fake or malicious websites

---

### 3. Data Integrity

- Uses cryptographic hash functions and MACs
- Ensures data is not altered during transmission
- Any modification is detected and rejected

---

## 4️⃣ HTTPS Connection Establishment (Brief)

1. Client sends a request to the HTTPS server
2. TLS handshake begins
3. Server sends its digital certificate
4. Client verifies the certificate
5. A secure session key is established
6. Encrypted HTTP communication starts
