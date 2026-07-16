# Wireshark Network Traffic Analysis

## Overview

In this project I analyzed a network capture (.pcap) using Wireshark.

The goal was to identify:

- the infected host
- the hostname
- the user account
- suspicious network traffic
- indicators of compromise (IOCs)

---

## Tools

- Wireshark

---

## Investigation Steps

1. Identified the infected host.
2. Found the MAC address.
3. Discovered the hostname using NBNS.
4. Identified the user account with Kerberos.
5. Reviewed network conversations.
6. Investigated suspicious HTTP traffic.
7. Followed the TCP stream.

---

## Screenshots

| Step | Description |
|------|-------------|
| 01 | Case overview |
| 02 | MAC address |
| 03 | Hostname discovery |
| 04 | User account |
| 05 | Network conversations |
| 06 | Suspicious HTTP traffic |
| 07 | TCP stream |

---

## Key Findings

- Internal IP: **10.2.28.88**
- Hostname: **DESKTOP-TEYQ2NR**
- User: **brolf**
- Suspicious IP: **45.131.214.85**
- Malware/User-Agent: **NetSupport Manager**

---

## Skills Learned

- Wireshark filtering
- Kerberos analysis
- NBNS analysis
- TCP Stream analysis
- Network traffic investigation
