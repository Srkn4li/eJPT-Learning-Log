# Subdomain Enumeration with Sublist3r

## Overview
Finding the main website is easy, but the real vulnerabilities often hide in the subdomains (like `dev.target.com` or `admin.target.com`).
**Sublist3r** is a Python tool designed to enumerate subdomains of a website using OSINT. It aggregates results from many search engines (Google, Yahoo, Bing, Baidu, etc.) and tools like Netcraft and Virustotal.

## Usage & Commands

### 1. The "All-In" Scan
By default, Sublist3r searches all available engines.

```bash
sublist3r -d <domain>
```

### 2. Specific Search Engines (-e)
Sometimes I want to target specific sources or exclude others.

```bash
sublist3r -d <domain> -e google,yahoo
```

- **-d:** Defines the target domain.
- **-e:** Specifies a comma-separated list of search engines to use.

## Real-World Challenges (My Experience)

### The "Google Block" Problem
I noticed that search engines, especially Google, really dislike automated scraping.

**Symptoms:** Sublist3r might hang or return connection errors because Google is throwing CAPTCHAs or blocking my IP.

**Workarounds:**
- Exclude Google: If it hangs, I run it without Google or specify other engines.
- Use a VPN: If my IP gets temporarily banned, switching VPN servers helps bypass the block.

### Reliability
It's important to remember that Sublist3r is passive. It relies on third-party data.

- It might not work always: Sometimes APIs change or search engines update their layout, breaking the tool.
- **Verification:** Just because Sublist3r finds a subdomain doesn't mean it's still online. I always need to verify the results (e.g., with ping or httprobe) afterwards.

## Why this matters
Expanding the attack surface is key. A secure main website might have a forgotten subdomain running an old, vulnerable version of software. Sublist3r helps me find these hidden doors.
