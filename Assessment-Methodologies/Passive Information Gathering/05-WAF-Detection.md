# WAF Detection with wafw00f

## Overview
Before starting any active attacks (like SQL Injection or XSS), you need to know: **Is there a WAF in front of the target?**
A Web Application Firewall filters traffic between you and the server. If you ignore it, your attacks may get blocked or your IP banned.

The tool **wafw00f** in Kali Linux answers two key questions:

- Is the site protected?
- Which WAF is in place? (Cloudflare, AWS, F5, etc.)

## How It Works
wafw00f identifies WAFs using a structured approach:

1. **Normal Request:** Sends a standard HTTP request and inspects the response.
2. **Malicious Request:** Sends slightly suspicious payloads (e.g., small directory traversal strings).
3. **Fingerprint Matching:** Compares headers, cookies, and response patterns against its signature database.

## Usage & Commands

### Basic Scan
A fast, low-noise scan for common WAFs.

```bash
wafw00f <domain>
```

Result: Shows either *No WAF detected* or identifies the vendor.

### Aggressive Scan (`-a`)
Useful when a WAF is stealthy or less common.

```bash
wafw00f -a <domain>
```

What it does: Tests all signatures in the database.

Note: This is noisy and can trigger SOC alerts in real engagements.

## Why It Matters
The result shapes your strategy:

- **No WAF:** Standard payloads and automated tools are more likely to work.
- **Cloudflare / AWS WAF:** Expect filtering. Consider bypass techniques or focus on logic flaws that WAFs miss.
