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
| nslookup $TARGET | Identify the A record for the target domain. |
| nslookup -query=A $TARGET | Identify the A record for the target domain. |
| dig $TARGET @<nameserver/IP> | Identify the A record for the target domain.  |
| dig a $TARGET @<nameserver/IP> | Identify the A record for the target domain.  |
| nslookup -query=PTR <IP> | Identify the PTR record for the target IP address. |
| dig -x <IP> @<nameserver/IP> | Identify the PTR record for the target IP address.  |
| nslookup -query=ANY $TARGET | Identify ANY records for the target domain. |
| dig any $TARGET @<nameserver/IP> | Identify ANY records for the target domain. |
| nslookup -query=TXT $TARGET | Identify the TXT records for the target domain. |
| dig txt $TARGET @<nameserver/IP> | Identify the TXT records for the target domain. |
| nslookup -query=MX $TARGET | Identify the MX records for the target domain. |
| dig mx $TARGET @<nameserver/IP> | Identify the MX records for the target domain. |


## Passive Subdomain Enumeration

| **Resource/Command** | **Description** |
|-|-|
| VirusTotal | [https://www.virustotal.com/gui/home/url](https://www.virustotal.com/gui/home/url) |
| Censys | [https://censys.io/](https://censys.io/) |
| Crt.sh | [https://crt.sh/](https://crt.sh/) |
| curl -s https://sonar.omnisint.io/subdomains/{domain} \| jq -r '.[]' \| sort -u | All subdomains for a given domain. |
| curl -s https://sonar.omnisint.io/tlds/{domain} \| jq -r '.[]' \| sort -u | All TLDs found for a given domain. |
| curl -s https://sonar.omnisint.io/all/{domain} \| jq -r '.[]' \| sort -u | All results across all TLDs for a given domain. |
| curl -s https://sonar.omnisint.io/reverse/{ip} \| jq -r '.[]' \| sort -u | Reverse DNS lookup on IP address. |
| curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} \| jq -r '.[]' \| sort -u | Reverse DNS lookup of a CIDR range. |
| curl -s "https://crt.sh/?q=${TARGET}&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u | Certificate Transparency. |
| cat sources.txt \| while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}-${TARGET}";done | Searching for subdomains and other information on the sources provided in the source.txt list. |

#### Sources.txt

~~~ txt
baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
~~~