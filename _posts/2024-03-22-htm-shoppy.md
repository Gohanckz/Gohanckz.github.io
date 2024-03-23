---
layout: post
title: Shoppy
tags: htb wup linux rce ffuf mattermost sqli ssh docker 
categories: HackTheBox 
---

# Description

|||
| -- | -- |
| S.O | Linux |


# Recon

In this case, we use nmap for scan the ports and services.

~~~ bash
nmap -sC -sV 10.10.11.180 -p- -vvv -Pn -n --open
~~~