---
---
Cybersecurity & network interview questions with answers

## 🎯 Cybersecurity interview questions — with answers

### Networking

**What happens when you type a URL?**
DNS resolution → TCP handshake (SYN/SYN-ACK/ACK) → TLS handshake → HTTP request → response render. Bonus: mention ARP, NAT, routing on the way.

**TCP vs UDP?**
TCP: reliable, ordered, connection-oriented (web, SSH). UDP: fire-and-forget, low latency (DNS, VoIP, QUIC builds reliability on top).

**What's a SYN flood and how do you mitigate?**
Half-open connections exhaust backlog. Mitigate: SYN cookies, rate limiting, backlog tuning.

**Explain NAT vs PAT.**
NAT maps IPs 1:1 or pool; PAT (NAT overload) multiplexes many hosts behind one IP using ports — what your home router does.

### Security fundamentals

**CIA triad?**
Confidentiality, Integrity, Availability — the three properties every control protects.

**Symmetric vs asymmetric crypto — when each?**
Symmetric (AES): fast, bulk data. Asymmetric (RSA/ECC): key exchange, signatures. Real systems use hybrid (TLS: asymmetric handshake → AES session).

**Hashing vs encryption?**
Hashing is one-way (integrity, passwords — use bcrypt/argon2). Encryption is reversible with a key (confidentiality).

**What is XSS and the 3 types?**
Reflected (in URL/response), Stored (persisted in DB), DOM-based (client-side sink). Mitigate: output encoding, CSP, sanitization.

**CSRF vs SSRF?**
CSRF tricks the *user's browser* into acting; SSRF tricks the *server* into making requests. Different trust boundaries.

### Practical

**How do you investigate a suspicious process?**
`ps`/`lsof`/`ss` for connections → check binary hash (VirusTotal) → parent process tree → persistence mechanisms (cron, launchd, registry) → network indicators (beaconing, DGA-looking domains).

**A user reports their laptop "talks to a server in a country they don't know." First steps?**
Identify the process owning the connection (per-process visibility tools like [Snitch](https://github.com/aixisstudio/Snitch) exist for this), check domain reputation, look at beaconing intervals, snapshot before killing.

**How does DNS get abused?**
Tunneling (data in TXT queries), DGA for C2, DNS-over-HTTPS to evade visibility, cache poisoning (mostly fixed by randomization + DNSSEC).

### Behavioral

**Tell me about a security incident you handled.**
Structure: detection → containment → eradication → recovery → lessons. Show you know post-incident review matters more than heroics.

**How do you stay current?**
Named sources: advisories, vendor feeds, community, labs. A real answer with real names beats "I read a lot."


## ❓ Frequently Asked Questions

**What questions are asked in cybersecurity interviews?**

Expect networking (TCP vs UDP, SYN flood), crypto (symmetric vs asymmetric), web security (XSS, CSRF, SSRF) and incident response scenarios — this repo covers them with model answers.


---

Maintained by [aixisstudio](https://github.com/aixisstudio) — author of [Snitch](https://github.com/aixisstudio/Snitch), the open-source application firewall. See also: [all resources](https://aixisstudio.github.io).
