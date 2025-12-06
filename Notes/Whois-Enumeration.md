# Whois Enumeration

## Overview
Whois is a query and response protocol that is used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name or an IP address block.

### 1. Kali Linux Command
The primary tool in Kali is the command-line client.
* **Command:** `whois <target-domain.com>`
* **Usage:** useful to quickly retrieve registration data directly from the terminal.

### 2. Web-Based Lookups
If the command line is blocked or I need a more graphical view, I can use web interfaces:
* **Tools:** `who.is`, `whois.domaintools.com`, or other online registrars.
* **Benefit:** Sometimes provides historical data or clearer formatting.

### 3. Key Findings & Analysis
The most critical information to look for in the output:
* **Registrant Info:** Who owns the domain? (Often redacted for privacy/GDPR, but sometimes visible).
* **Nameservers (NS Records):** **Crucial Finding!**
  * *Why it matters:* The nameservers tell me where the website is hosted (e.g., Cloudflare, AWS, or a private hosting provider).
  * *Next Step:* Knowing the hosting provider helps in planning further scans (e.g., checking for specific misconfigurations or avoiding WAFs).
