### 1. Nmap

- Network scanning and security auditing tool
- Supports SSL/TLS assessment through **NSE (Nmap Scripting Engine)**
- Can identify:
    - Supported TLS versions
    - Weak cipher suites
    - Certificate details
    - Insecure configurations

Commonly used for **active reconnaissance**.

## Using Nmap Scripts for SSL/TLS Scanning

# 1. Scan Supported Cipher Suites

```bash
nmap --script ssl-enum-ciphers -p 443 example.com

```

Identifies:

- TLS versions enabled (TLS 1.0, 1.1, 1.2, 1.3)
- Cipher suites per protocol
- Cipher strength (weak, medium, strong)

## Output

```bash
 nmap --script ssl-enum-ciphers -p 443 brototype.com
Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-26 23:26 EST
Nmap scan report for brototype.com (157.245.98.112)
Host is up (0.070s latency).

PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers: 
|   TLSv1.2: 
|     ciphers: 
|       TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 2048) - A
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (secp256r1) - A
|     compressors: 
|       NULL
|     cipher preference: client
|   TLSv1.3: 
|     ciphers: 
|       TLS_AKE_WITH_AES_128_GCM_SHA256 (ecdh_x25519) - A
|       TLS_AKE_WITH_AES_256_GCM_SHA384 (ecdh_x25519) - A
|       TLS_AKE_WITH_CHACHA20_POLY1305_SHA256 (ecdh_x25519) - A
|     cipher preference: client
|_  least strength: A

Nmap done: 1 IP address (1 host up) scanned in 3.54 seconds

```

## FINDINGS

## 1. Port and Service Status

```
PORT    STATE SERVICE
443/tcp open  https

```

- HTTPS service is running on port **443**
- TLS is enabled and accepting connections

---

## 2. TLS Versions Supported

```
TLSv1.2
TLSv1.3

```

- **TLS 1.0 and TLS 1.1 are NOT supported**
- Only **modern, secure TLS versions** are enabled
    
    ✅ This is a strong security configuration
    

---

## 3. TLS 1.2 Cipher Analysis

```
TLSv1.2:

```

### Key Exchange Methods

```
DHE_RSA (dh2048)
ECDHE_RSA (secp256r1)

```

- **DHE_RSA**
    - Uses Diffie–Hellman with 2048-bit parameters
    - Provides forward secrecy
- **ECDHE_RSA**
    - Elliptic Curve Diffie–Hellman
    - Curve: `secp256r1`
    - Faster and more secure than traditional DH

✅ All key exchanges provide **forward secrecy**

---

### Encryption Algorithms

```
AES_128_GCM
AES_256_GCM
CHACHA20_POLY1305

```

- All are **AEAD ciphers**
- No CBC mode
- No 3DES
- No RC4

✅ Resistant to padding oracle and SWEET32 attacks

---

### Cipher Strength

```
-A

```

- Nmap rates all TLS 1.2 ciphers as **strong**
- No weak or legacy algorithms present

---

## 4. TLS 1.3 Cipher Analysis

```
TLSv1.3:

```

### Supported Ciphers

```
TLS_AES_128_GCM_SHA256
TLS_AES_256_GCM_SHA384
TLS_CHACHA20_POLY1305_SHA256

```

- Mandatory strong ciphers defined by TLS 1.3
- Uses **X25519** elliptic curve
- Forward secrecy is mandatory
- No RSA key exchange

✅ This is the **best possible TLS configuration**

---

## 5. Cipher Preference

```
cipher preference:client

```

- Client selects the cipher from the secure list
- Safe because all offered ciphers are strong

---

## 6. TLS Compression

```
compressors:
NULL

```

- TLS compression is disabled
- Protects against **CRIME attack**

---

## 7. Least Strength: A

```
leaststrength:A

```

- Nmap reports **A** because:
    - No weak protocols
    - No weak ciphers
    - No insecure key exchange methods

✅ This indicates **excellent SSL/TLS security posture**

---

## 8. Security Assessment Summary

### Secure Configuration Indicators

- TLS 1.0 and TLS 1.1 disabled
- Only AEAD ciphers used
- Forward secrecy enabled everywhere
- Strong key sizes (2048-bit DH, modern ECC curves)
- TLS 1.3 enabled
- No compression

![Screenshot 2026-01-27 095527.png](attachment:6843e6e2-5562-4b55-ab28-55a5478c47a2:Screenshot_2026-01-27_095527.png)

# 2. Retrieve Certificate Information

```bash
nmap --script ssl-cert -p 443 example.com

```

Extracts:

- Subject and issuer
- Validity period
- Public key type and size
- Signature algorithm

```bash

PORT    STATE SERVICE
443/tcp open  https
| ssl-cert: Subject: commonName=brototype.com
| Subject Alternative Name: DNS:brototype.com, DNS:www.brototype.com
| Issuer: commonName=R12/organizationName=Let's Encrypt/countryName=US
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-01-08T03:46:22
| Not valid after:  2026-04-08T03:46:21
| MD5:   47fc:a1b9:e0b1:3a02:bd6f:e3c6:a44a:7bc5
|_SHA-1: 0635:b52a:6e27:4bc0:e0ac:3e82:1e4e:7109:724f:f0e5

Nmap done: 1 IP address (1 host up) scanned in 0.80 seconds
```

## FINDINGS

SSL Certificate Details

```bash
| ssl-cert: Subject: commonName=brototype.com
| Subject Alternative Name: DNS:brototype.com, DNS:www.brototype.com
| Issuer: commonName=R12/organizationName=Let's Encrypt/countryName=US

```

### Explanation (simple):

- **Subject (commonName=brototype.com)**
    
    → The certificate is issued to **brototype.com**, proving the website’s identity.
    
- **Subject Alternative Name (SAN)**
    
    → The certificate is valid for:
    
    - brototype.com
    - [www.brototype.com](http://www.brototype.com/)
        
        This allows the same certificate to work for both domains.
        
- **Issuer**
    
    → The certificate is issued by **Let's Encrypt**, a trusted Certificate Authority.
    

---

## 5. Cryptographic Details

```bash
| Public Keytype: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption

```

### Explanation:

- **Public Key type: RSA**
    
    → RSA is used for encryption and key exchange.
    
- **Public Key bits: 2048**
    
    → RSA-2048 is secure and widely accepted.
    
- **Signature Algorithm: SHA256 with RSA**
    
    → SHA-256 is a strong hash algorithm used to verify the certificate’s authenticity.
    

---

## 6. Certificate Validity Period

```bash
| Not valid before: 2026-01-08T03:46:22
| Not valid after:  2026-04-08T03:46:21

```

### Explanation:

- The certificate is valid for about **90 days**
- Short validity reduces risk if the certificate is compromised
- This is normal for Let’s Encrypt certificates

---

## 7. Certificate Fingerprints

### 7.1 MD5 Fingerprint

```bash
| MD5: 47fc:a1b9:e0b1:3a02:bd6f:e3c6:a44a:7bc5

```

- MD5 is **cryptographically broken**
- Included only for **legacy identification**
- ❌ Not used for security today

---

### 7.2 SHA-1 Fingerprint

```bash
| SHA-1: 0635:b52a:6e27:4bc0:e0ac:3e82:1e4e:7109:724f:f0e5

```

- SHA-1 is **deprecated**
- Included for compatibility
- Modern systems rely on **SHA-256**, not SHA-1

# Using OpenSSL for Manual Verification

### Check TLS Version and Cipher

```bash
openssl s_client -connect example.com:443

```

### Force Specific TLS Version

```bash
openssl s_client -connect example.com:443 -tls1_2

```

Used to confirm:

- Protocol support
- Cipher negotiation
- Certificate chain correctness

```bash

Connecting to 104.18.26.120
CONNECTED(00000003)
depth=3 C=US, O=SSL Corporation, CN=SSL.com TLS ECC Root CA 2022
verify return:1
depth=2 C=US, O=SSL Corporation, CN=SSL.com TLS Transit ECC CA R2
verify return:1
depth=1 C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
verify return:1
depth=0 CN=example.com
verify return:1
---
Certificate chain
 0 s:CN=example.com
   i:C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
   a:PKEY: EC, (prime256v1); sigalg: ecdsa-with-SHA256
   v:NotBefore: Dec 16 19:39:32 2025 GMT; NotAfter: Mar 16 18:32:44 2026 GMT
 1 s:C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
   i:C=US, O=SSL Corporation, CN=SSL.com TLS Transit ECC CA R2
   a:PKEY: EC, (prime256v1); sigalg: ecdsa-with-SHA384
   v:NotBefore: May 29 19:49:45 2025 GMT; NotAfter: May 27 19:49:44 2035 GMT
 2 s:C=US, O=SSL Corporation, CN=SSL.com TLS Transit ECC CA R2
   i:C=US, O=SSL Corporation, CN=SSL.com TLS ECC Root CA 2022
   a:PKEY: EC, (secp384r1); sigalg: ecdsa-with-SHA384
   v:NotBefore: Oct 21 17:02:23 2022 GMT; NotAfter: Oct 17 17:02:22 2037 GMT
 3 s:C=US, O=SSL Corporation, CN=SSL.com TLS ECC Root CA 2022
   i:C=GB, ST=Greater Manchester, L=Salford, O=Comodo CA Limited, CN=AAA Certificate Services
   a:PKEY: EC, (secp384r1); sigalg: sha256WithRSAEncryption
   v:NotBefore: Aug  1 00:00:00 2025 GMT; NotAfter: Dec 31 23:59:59 2028 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIID5jCCA4ygAwIBAgIQHt6YMH6gWUgj6oG69hVKZzAKBggqhkjOPQQDAjBRMQsw
CQYDVQQGEwJVUzEYMBYGA1UECgwPU1NMIENvcnBvcmF0aW9uMSgwJgYDVQQDDB9D
bG91ZGZsYXJlIFRMUyBJc3N1aW5nIEVDQyBDQSAzMB4XDTI1MTIxNjE5MzkzMloX
DTI2MDMxNjE4MzI0NFowFjEUMBIGA1UEAwwLZXhhbXBsZS5jb20wWTATBgcqhkjO
PQIBBggqhkjOPQMBBwNCAAQxGeN57lXS1F2bzwodXUhr+ATB5io3CVYr/hHs4/rb
3mvCUW52NRIbNkrRHQ2uMGU5uwBkjIaIDqay+5zgOsOUo4ICfzCCAnswDAYDVR0T
AQH/BAIwADAfBgNVHSMEGDAWgBSDA/3n9vVKTRVB9O0iFtMyCj7KZjBsBggrBgEF
BQcBAQRgMF4wOQYIKwYBBQUHMAKGLWh0dHA6Ly9pLmNmLWkuc3NsLmNvbS9DbG91
ZGZsYXJlLVRMUy1JLUUzLmNlcjAhBggrBgEFBQcwAYYVaHR0cDovL28uY2YtaS5z
c2wuY29tMCUGA1UdEQQeMByCC2V4YW1wbGUuY29tgg0qLmV4YW1wbGUuY29tMCMG
A1UdIAQcMBowCAYGZ4EMAQIBMA4GDCsGAQQBgqkwAQMBATATBgNVHSUEDDAKBggr
BgEFBQcDATBTBgNVHR8ETDBKMEigRqBEhkJodHRwOi8vYy5jZi1pLnNzbC5jb20v
YWU4MDFlZDFjNTViYjU3OWQ3OTIwOGIwZDc3MmFjZmI4Y2MzYTIwOC5jcmwwDgYD
VR0PAQH/BAQDAgeAMA8GCSsGAQQBgtpLLAQCBQAwggEDBgorBgEEAdZ5AgQCBIH0
BIHxAO8AdgBkEcRspBLsp4kcogIuALyrTygH1B41J6vq/tUDyX3N8AAAAZsotfor
AAAEAwBHMEUCIH6tkhTl46lP0v/8OqzmV0Vv6DV2BwGM4ti3XoDRQ1nFAiEA86Rz
a7JZdJ606/IiceXynxVojyGT9ln8CuejwqnEv0EAdQDLOPcViXyEoURfW8Hd+8lu
8ppZzUcKaQWFsMsUwxRY5wAAAZsotfpMAAAEAwBGMEQCIHy1dzJqqc7jEtWwPemT
AYGpiVj3fLkRhQ3yac050aCYAiAaMfO5uaoqMBAyDjjmGbPuXR+Zj+0srsBcuev+
5cHDajAKBggqhkjOPQQDAgNIADBFAiEA8GtCQg+hhl3iG0L+9oJoB11mxvmgxw1W
ItuzTEQUqM8CIEsu4IvUQFmf2RYc3XiMWn+jQHISY/4G22VAmCdC9W28
-----END CERTIFICATE-----
subject=CN=example.com
issuer=C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: ecdsa_secp256r1_sha256
Negotiated TLS1.3 group: X25519MLKEM768
---
SSL handshake has read 5070 bytes and written 1760 bytes
Verification: OK
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Protocol: TLSv1.3
Server public key is 256 bit
This TLS version forbids renegotiation.
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
```

## 1. Command Used

```bash
openssl s_client -connect example.com:443
```

### What this does:

- Connects to `example.com` on **port 443**
- Performs a **real TLS handshake**
- Shows **certificates, algorithms, TLS version, cipher, and session info**

👉 Think of this as **“manual HTTPS inspection”**

---

## 2. Connection Status

```bash
Connecting to 104.18.26.120
CONNECTED(00000003)
```

- `example.com` resolves to **104.18.26.120**
- TCP connection successful
- TLS handshake can start

---

## 3. Certificate Chain Verification (Depth)

```bash
depth=3 C=US, O=SSL Corporation, CN=SSL.com TLS ECC Root CA 2022
verifyreturn:1
depth=2 C=US, O=SSL Corporation, CN=SSL.com TLS Transit ECC CA R2
verifyreturn:1
depth=1 C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
verifyreturn:1
depth=0 CN=example.com
verifyreturn:1
```

### What “depth” means:

- **depth=0** → Server certificate (example.com)
- **higher depth** → Intermediate and root CAs

### Interpretation:

- Certificate chain goes from:
    1. `example.com`
    2. Cloudflare Issuing CA
    3. SSL.com Intermediate CA
    4. SSL.com Root CA
- `verify return:1` at every level = ✅ **trusted**

👉 Browser would trust this certificate

---

## 4. Certificate Chain Details

```bash
Certificate chain
 0 s:CN=example.com
   i:C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
```

### Simple meaning:

- **Subject (s)** → example.com
- **Issuer (i)** → Cloudflare TLS Issuing CA

---

### Cryptography Used (Leaf Certificate)

```bash
a:PKEY: EC, (prime256v1); sigalg: ecdsa-with-SHA256
```

- Public key type: **Elliptic Curve (ECC)**
- Curve: **prime256v1**
- Signature: **ECDSA with SHA-256**

✅ Stronger and faster than RSA

---

### Validity Period

```bash
v:NotBefore: Dec 16 19:39:32 2025 GMT
v:NotAfter:  Mar 16 18:32:44 2026 GMT
```

- Valid for about **90 days**
- Short validity = **better security**
- Common for automated certificates

---

## 5. Server Certificate Block

```bash
-----BEGIN CERTIFICATE-----
MIID5jCCA4ygAwIBAgIQHt6YMH6gWUgj6oG69hVKZzAKBggqhkjOPQQDAjBRMQsw
...
-----END CERTIFICATE-----
```

- This is the **actual X.509 certificate**
- PEM encoded (Base64)
- Contains:
    - Public key
    - Subject
    - Issuer
    - Extensions
    - Signature

👉 This is what browsers receive and verify

---

## 6. Certificate Identity

```bash
subject=CN=example.com
issuer=C=US, O=SSL Corporation, CN=Cloudflare TLS Issuing ECC CA 3
```

- Certificate belongs to **example.com**
- Issued by **SSL Corporation via Cloudflare**

---

## 7. Client Authentication

```bash
No client certificate CA names sent
```

- Server does **not** require client certificates
- Normal for public websites

---

## 8. Cryptographic Parameters Negotiated

```bash
Peer signing digest: SHA256
Peer signaturetype: ecdsa_secp256r1_sha256
Negotiated TLS1.3 group: X25519MLKEM768
```

### Meaning:

- Hash algorithm: **SHA-256**
- Signature: **ECDSA**
- Key exchange group:
    - **X25519** (modern ECC)
    - **MLKEM768** (post-quantum hybrid)

✅ Very strong, future-ready cryptography

---

## 9. TLS Handshake Summary

```bash
SSL handshake hasread 5069 bytes and written 1760 bytes
Verification: OK
```

- Handshake completed successfully
- Certificate verification passed

---

## 10. Final TLS Session Details

```bash
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Protocol: TLSv1.3
Server public key is 256 bit
This TLS version forbids renegotiation.
```

### Key points:

- **TLS version**: TLS 1.3 (latest & safest)
- **Cipher suite**:
    - AES-256 (encryption)
    - GCM (authenticated encryption)
    - SHA-384 (hashing)
- Renegotiation disabled → prevents attacks
