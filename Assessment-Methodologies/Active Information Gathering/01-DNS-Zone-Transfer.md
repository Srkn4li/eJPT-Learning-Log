# DNS Zone Transfers & Interrogation

## Overview
Now moving into **Active Information Gathering**. Unlike passive recon, I am now directly interacting with the target's DNS servers.
The goal is **DNS Interrogation**: probing the DNS servers to map out the network infrastructure, find IP addresses, and identify internal servers.

## What is DNS?
Browsers and humans don't speak the same language.
- **Humans** use domain names (e.g., `google.com`).
- **Computers** use IP addresses (e.g., `142.250.1.1`).
- **DNS (Domain Name System)** is the phonebook that translates names into IPs.

### Important DNS Record Types
When querying a server, these are the records I am looking for:
- **A:** Maps a hostname to an IPv4 address.
- **AAAA:** Maps a hostname to an IPv6 address.
- **NS (Name Server):** Shows which servers are authoritative for the domain.
- **MX (Mail Exchange):** Points to the email servers.
- **CNAME:** Canonical Name (an alias, e.g., `www` → `main-server`).
- **TXT:** Often contains verification keys (SPF, Google verification).

## DNS Zone Transfer (AXFR)
This is the "Holy Grail" of DNS recon.
- **The Concept:** DNS servers need to sync with each other (Master → Slave). They use a protocol called **AXFR** to copy *all* records from one server to another.
- **The Vulnerability:** If the administrator misconfigures the server to allow AXFR requests from *anyone*, I can download the **entire** zone.
- **Impact:** Instant access to every subdomain, internal IP, and server name hidden in the zone file.

## Tools & Commands

### 1. dnsenum (The Automated Way)
`dnsenum` is a powerful Perl script that checks for zone transfers and performs brute-forcing if needed.

```bash
dnsenum <domain>
```

*Note:* It automatically discovers the Name Servers (NS) and asks each one for a zone transfer.

### 2. dig (The Manual/Professional Way)
`dig` is the standard tool for DNS lookups. Running it manually shows mastery of DNS.

```bash
dig axfr @<nameserver_IP> <domain>
```
- **axfr:** Request for full zone transfer.
- **@:** Defines which nameserver to ask.

### 3. fierce
A lightweight scanner great for finding non-contiguous IP space and hidden hosts.

```bash
fierce --domain <domain>
```
*Note:* Great fallback if `dnsenum` is too noisy.

## My Takeaway
Successful Zone Transfers are rare today—admins finally learned. But if one slips through, it hands me the target's entire network layout on a silver platter and saves days of scanning.
