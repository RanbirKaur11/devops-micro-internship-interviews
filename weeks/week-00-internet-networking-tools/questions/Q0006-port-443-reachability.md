---
id: Q0006
title: Port 443 reachability — TCP vs TLS vs HTTPS
difficulty: intermediate
week: 00
topics: [internet, networking, tools]
tags: [port-check, tls, tcp, troubleshooting]
author: RanbirKaur11
reviewed: false
---

## Question
When troubleshooting a service on a remote host running on port 443, which commands can you use to confirm if the port is open and reachable, and how are these commands different?

## Short Answer
- `nc -vz host 443` or `Test-NetConnection -Port 443` → checks basic TCP connection
- `openssl s_client -connect host:443` → checks TLS/SSL security handshake
- `curl -vk https://host` → checks actual HTTPS web response
- `nmap -p 443 host` → scans the service state in detail

## Deep Dive
- **TCP reachability (basic network check)**
  - `nc -vz example.com 443` (Linux/macOS)
  - `Test-NetConnection example.com -Port 443` (Windows)
  - Confirms that the port is open and the server is listening, but does not check security or website response.

- **TLS handshake (secure connection check)**
  - `openssl s_client -connect example.com:443 -servername example.com`
  - Verifies if a secure SSL/TLS connection can be started and shows details like certificate and expiry.

- **HTTPS application-level (website check)**
  - `curl -vk https://example.com`
  - Sends an actual HTTPS request and shows if the website/application responds correctly.

- **Service scanning (deep inspection)**
  - `nmap -p 443 example.com`
  - Tells if the port is open/filtered/closed and can identify which service is running.

## Pitfalls
- A port can be open (TCP) but the website might still be broken (HTTPS)
- TLS can fail if the server does not know which domain you are requesting (SNI issue). Server Name Indication (SNI) tells the server which website you are trying to reach when many websites share the same IP. Without SNI, the wrong certificate may be shown and HTTPS fails.
- Curl errors may come from the application itself, not the network

## References
- https://www.iana.org
- https://www.openssl.org
- https://nmap.org
