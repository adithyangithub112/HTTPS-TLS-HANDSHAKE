# QUIC vs Traditional TLS (TCP + TLS) — Summary Table

| Aspect | Traditional TLS (over TCP) | QUIC (over UDP + TLS 1.3) | Improvement / Benefit |
| --- | --- | --- | --- |
| Transport Protocol | TCP | UDP | QUIC avoids TCP limitations while adding its own reliability |
| Handshake Process | Separate TCP + TLS handshakes | Combined transport + crypto handshake | Fewer round trips |
| Connection Setup Time | Slow (2–3 RTTs) | Fast (0-RTT / 1-RTT) | Reduced latency |
| Encryption | Optional (TLS can be disabled) | Mandatory encryption | Stronger security by default |
| TLS Version | TLS 1.2 / 1.3 | TLS 1.3 only | Modern cryptography only |
| Head-of-Line Blocking | Present (TCP-level) | Eliminated (stream-based) | Better performance |
| Packet Loss Handling | TCP blocks entire stream | Loss isolated per stream | Faster recovery |
| Reliability | TCP kernel-based | QUIC user-space based | Faster updates & improvements |
| Connection Migration | Not supported | Supported | Seamless network switching |
| Security Against MITM | Good | Stronger | Encrypted headers + TLS 1.3 |
| Middlebox Interference | High | Low | Encrypted transport metadata |
| Use in Web Protocols | HTTP/1.1, HTTP/2 | HTTP/3 | Next-generation web |
