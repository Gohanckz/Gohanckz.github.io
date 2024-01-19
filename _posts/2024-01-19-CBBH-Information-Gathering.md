---
layout: post
title: CBBH - Information Gathering
tags: web CSRF burpsuite bugbounty CBBH PoC
categories: [information gathering]
---

## WHOIS

| **Command** | **Description** |
|-|-|
| export TARGET="domain.tld" | Assign target to an environment variable. |
| whois $TARGET | WHOIS lookup for the target. |



## DNS Enumeration

| **Command** | **Description** |
|-|-|
| nslookup $TARGET` | Identify the A record for the target domain. |
| nslookup -query=A $TARGET` | Identify the A record for the target domain. |
| dig $TARGET @<nameserver/IP> | Identify the A record for the target domain.  |
| dig a $TARGET @<nameserver/IP> | Identify the A record for the target domain.  |
| nslookup -query=PTR <IP> | Identify the PTR record for the target IP address. |
| dig -x <IP> @<nameserver/IP> | Identify the PTR record for the target IP address.  |
| nslookup -query=ANY $TARGET` | Identify ANY records for the target domain. |
| dig any $TARGET @<nameserver/IP> | Identify ANY records for the target domain. |
| nslookup -query=TXT $TARGET` | Identify the TXT records for the target domain. |
| dig txt $TARGET @<nameserver/IP> | Identify the TXT records for the target domain. |
| nslookup -query=MX $TARGET | Identify the MX records for the target domain. |
| dig mx $TARGET @<nameserver/IP> | Identify the MX records for the target domain. |


