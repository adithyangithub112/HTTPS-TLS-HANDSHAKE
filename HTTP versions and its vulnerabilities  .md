## 1. HTTP/1.1

### Overview

- Introduced in **1997**
- Major improvements:
    - Persistent connections
    - Chunked transfer encoding
    - Host header (virtual hosting)
- Still widely used

### Vulnerabilities

1. **Plain Text Communication**
    - Data can be read and modified by attackers
2. **Session Hijacking**
    - Cookies sent in clear text
3. **HTTP Request Smuggling**
    - Ambiguous request parsing between proxies and servers
4. **HTTP Response Splitting**
    - Header injection via CRLF sequences
5. **Slowloris Attack**
    - Partial requests exhaust server connections

### Mitigation

- Enforce HTTPS using TLS
- Use secure cookie flags (`Secure`, `HttpOnly`)
- Implement proper request parsing
- Configure timeouts and rate limiting
- Use Web Application Firewalls (WAFs)

---

## 2. HTTPS (HTTP over TLS)

### Overview

- Not a new HTTP version, but HTTP secured using **TLS**
- Provides:
    - Encryption
    - Authentication
    - Integrity

### Vulnerabilities

1. **TLS Misconfiguration**
    - Weak cipher suites
    - Old TLS versions
2. **Certificate Issues**
    - Expired or misissued certificates
    - Weak trust chains
3. **Downgrade Attacks**
    - Forcing use of weaker protocols

### Mitigation

- Enforce TLS 1.2 or TLS 1.3
- Disable weak cipher suites
- Use HSTS (HTTP Strict Transport Security)
- Implement certificate pinning where appropriate

---

## 3. HTTP/2

### Overview

- Introduced in **2015**
- Uses binary framing instead of text
- Features:
    - Multiplexing
    - Header compression (HPACK)
    - Single TCP connection per origin
- Usually used over HTTPS

### Vulnerabilities

1. **HPACK Compression Attacks**
    - CRIME/BREACH-style side-channel attacks
2. **Stream Abuse**
    - Resource exhaustion via multiple streams
3. **Implementation Complexity**
    - Increases risk of bugs

### Mitigation

- Always deploy HTTP/2 over HTTPS
- Limit concurrent streams
- Apply rate limiting
- Keep HTTP/2 libraries updated
- Monitor for abnormal stream behavior

---

## 4. HTTP/3

### Overview

- Latest version
- Built on **QUIC**, which uses UDP
- Features:
    - Faster connection establishment
    - Built-in encryption
    - No head-of-line blocking at transport level

### Vulnerabilities

1. **UDP Amplification Attacks**
2. **QUIC Implementation Bugs**
3. **Firewall and Network Visibility Issues**

### Mitigation

- Enforce strict rate limiting
- Use modern QUIC implementations
- Deploy intrusion detection systems supporting QUIC
- Proper firewall and UDP traffic controls

---

## 5. Common HTTP-Level Attacks (All Versions)

| Attack | Description | Mitigation |
| --- | --- | --- |
| MITM | Intercepting traffic | HTTPS, HSTS |
| Session Hijacking | Stealing cookies | Secure cookies, TLS |
| CSRF | Unauthorized actions | CSRF tokens |
| XSS | Script injection | Input validation, CSP |
| Clickjacking | UI manipulation | X-Frame-Options |

---

## 6. Comparative Summary Table

| Version | Encrypted | Key Vulnerabilities | Status |
| --- | --- | --- | --- |
| HTTP/0.9 | No | No security | Obsolete |
| HTTP/1.0 | No | Sniffing, MITM | Deprecated |
| HTTP/1.1 | No | Smuggling, hijacking | Legacy |
| HTTPS | Yes | TLS misconfig | Standard |
| HTTP/2 | Yes | Stream abuse | Modern |
| HTTP/3 | Yes | QUIC-based risks | Emerging |
