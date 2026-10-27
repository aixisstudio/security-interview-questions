# Security Interview Questions

[English](README.md) | **Français** | [Español](README.es.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Русский](README.ru.md)

Questions d'entretien cybersécurité et réseau, avec réponses

_Le contenu détaillé (listes, liens, descriptions) est en anglais pour rester maintenable — les sections ci-dessous te guident._

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

## 🧪 Scenario Questions (with answer frameworks)

**"You find an unknown process beaconing to an IP every 60s. Walk me through it."**
→ Identify the process (`lsof -i`, `ps`), hash it (VirusTotal), check persistence (launchd/registry/service), isolate the host, then hunt the same beacon pattern fleet-wide. Mention Snitch-style egress monitoring as the detection layer.

**"How would you detect data exfiltration over DNS?"**
→ Look for: high query volume to one domain, long labels (>63 chars), entropy in subdomains, TXT record abuse. Tools: dnstap, passive DNS, your own resolver logs.

**"A developer pushed an AWS key to GitHub. Timeline?"**
→ Assume compromised instantly (scanners find it in minutes). Revoke → rotate → audit CloudTrail for use → force-push history cleanup → postmortem on prevention (git-secrets, pre-commit hooks).

**"Explain TLS interception pros/cons in an enterprise."**
→ Pro: visibility into encrypted egress. Con: breaks cert pinning, privacy/legal exposure, central decryption point = target. Alternatives: JA3 fingerprinting, metadata-only analysis.

## 🏗️ Design Questions

**"Design a secrets-management system for a startup."**
→ Vault/SOPS + short-lived creds + no secrets in env vars at rest + rotation + audit log + break-glass procedure. Mention that the *process* matters more than the tool.

**"How do you secure CI/CD?"**
→ Signed commits, branch protection, ephemeral runners, OIDC instead of stored cloud keys, dependency pinning + SBOM, least-privilege tokens, artifact signing (cosign).

## 🎓 Junior-Specific Expectations

Interviewers at junior level want: **honest "I don't know" + how you'd find out**, basic CIA triad fluency, knowing the difference between authN/authZ, and curiosity. Nobody expects you to recite RFCs.

---
## Contribuer

Les pull requests sont les bienvenues : ouvre une PR, elle sera relue et fusionnée. Maintenu par [aixisstudio](https://github.com/aixisstudio), auteur de [Snitch](https://github.com/aixisstudio/Snitch), pare-feu applicatif open source.

## Licence: MIT
