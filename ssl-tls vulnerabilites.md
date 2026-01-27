# SSL / TLS Vulnerabilities and Their Mitigation

## 1. Legacy Protocol Vulnerabilities

- **SSL 2.0 and SSL 3.0**
    - Weak MAC construction
    - No proper handshake integrity protection
    - Vulnerable to downgrade and padding oracle attacks (POODLE)

**Mitigation**

- Disable SSL 2.0 and SSL 3.0
- Enforce TLS 1.2 or TLS 1.3 only

---

## 2. Protocol Downgrade Attacks

- Attacker forces client and server to use weaker protocol versions
- Example: POODLE, protocol rollback attacks

**Mitigation**

- Disable legacy TLS versions
- Enable `TLS_FALLBACK_SCSV`
- Enforce strict version negotiation

---

## 3. Weak Cipher Suite Vulnerabilities

- Use of weak or broken ciphers:
    - RC4
    - DES / 3DES
    - Export-grade ciphers

**Impact**

- Enables cryptographic attacks
- Compromises confidentiality

**Mitigation**

- Disable weak ciphers
- Use strong AEAD ciphers:
    - AES-GCM
    - ChaCha20-Poly1305

---

## 4. Weak Hash Function Vulnerabilities

- Use of MD5 and SHA-1
- Susceptible to collision attacks

**Impact**

- Certificate forgery
- Signature bypass

**Mitigation**

- Use SHA-256 or stronger hash algorithms
- Reject certificates signed with weak hashes

---

## 5. Key Exchange Vulnerabilities

- **RSA key exchange**
    - No forward secrecy
    - Past sessions compromised if private key leaks
- **Weak Diffie–Hellman parameters**
    - Small or reused DH groups

**Mitigation**

- Use ECDHE or DHE for forward secrecy
- Use strong, secure DH parameters

---

## 6. Padding Oracle Attacks

- Exploit CBC mode padding errors
- Examples: BEAST, Lucky13

**Impact**

- Partial plaintext recovery

**Mitigation**

- Disable CBC cipher suites
- Use AEAD ciphers only

---

## 7. Compression-Based Attacks

- Exploit TLS or HTTP compression
- Examples: CRIME, BREACH

**Impact**

- Leakage of session cookies and secrets

**Mitigation**

- Disable TLS-level compression
- Avoid compressing sensitive data

---

## 8. Implementation Vulnerabilities

- Bugs in TLS libraries
- Example: Heartbleed

**Impact**

- Leakage of memory contents
- Exposure of private keys and credentials

**Mitigation**

- Patch vulnerable libraries
- Rotate keys and reissue certificates
- Use secure, updated TLS implementations

---

## 9. Certificate and PKI Vulnerabilities

- Compromised or malicious Certificate Authorities
- Improper certificate validation

**Impact**

- Enables man-in-the-middle attacks

**Mitigation**

- Strict certificate validation
- Certificate Transparency
- Certificate pinning (where applicable)

---

## 10. TLS Misconfiguration Vulnerabilities

- Allowing outdated protocols
- Weak cipher ordering
- Missing security headers

**Mitigation**

- Follow secure TLS configuration guidelines
- Regular TLS configuration audits
- Use automated TLS testing tools

---

## 11. SSL Stripping Attacks

- Downgrades HTTPS connections to HTTP

**Mitigation**

- Enable HTTP Strict Transport Security (HSTS)
- Force HTTPS connections

---

## 12. Side-Channel Attacks

- Timing attacks
- Traffic analysis

**Mitigation**

- Constant-time cryptographic operations
- Use TLS 1.3 to reduce metadata leakage
