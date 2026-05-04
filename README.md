# Network Port Scanning & Traffic Analysis Lab

## Overview
Performed real-world Nmap scanning and live packet analysis using Wireshark 
on a TryHackMe lab target. Practiced different scan types, firewall evasion 
techniques, and NSE script usage.

## Tools Used
- Nmap
- Wireshark
- Debian Linux

## Target
TryHackMe lab machine (10.49.138.198) — Windows host
thm:room/furthernmap 
---

## 1. Basic Port Scan
```
[Basic Nmap Scan Output](text/ping_result.txt)
```

---

## 2. Full SYN Scan (First 5000 ports)
```
[Basic Nmap Scan Output](text/syn_result.txt)
```
Found 5 open ports: 21 (FTP), 53 (DNS), 80 (HTTP), 135 (MSRPC), 3389 (RDP)

---

## 3. Xmas Scan (First 999 ports)
```
[Basic Nmap Scan Output](text/xmas_result.txt)
```
All ports returned open|filtered — expected behavior against Windows targets 
because Windows does not send RST packets in response to malformed flag combinations,
making firewall evasion scans unreliable against Windows.

---

## 4. TCP Connect Scan + Wireshark Analysis
Ran a TCP Connect scan against port 80 while capturing traffic in Wireshark.

![TCP Handshake](screenshots/1.png)

Observed full 3-way handshake:
- SYN → target
- SYN/ACK ← target  
- ACK → target
- RST/ACK → target (Nmap closes connection after confirming port is open)

---

## 5. FTP Anonymous Login — ftp-anon NSE Script
Deployed the ftp-anon script against port 21 while capturing in Wireshark.

![FTP Anonymous Login](screenshots/2.png)

Wireshark captured the full FTP conversation:
- Nmap attempted anonymous login (USER anonymous / PASS IEUser@)
- Server responded with 230 Logged on — anonymous login successful
- Confirms FTP server allows unauthenticated access (security risk)

---

## What I Learned
- Different Nmap scan types and when to use each
- Why -Pn is needed for Windows targets that block ICMP
- How FIN/NULL/Xmas scans attempt firewall evasion and why they 
  fail against Windows
- How to read live packet captures in Wireshark
- How NSE scripts automate service interaction (ftp-anon)
- How to save scan results with -oN for documentation

## References
- TryHackMe Nmap Room: https://tryhackme.com/room/furthernmap
- My TryHackMe Profile: https://tryhackme.com/p/redowanislammomin
