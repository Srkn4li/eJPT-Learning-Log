# Email Harvesting with theHarvester

## Overview
Reconnaissance isn't just about finding servers; it's about finding **people**.
**theHarvester** is a classic OSINT tool designed to gather emails, subdomains, hosts, employee names, open ports, and banners from public sources like search engines and PGP key servers.

* **Goal:** Create a list of valid email addresses for a specific domain.
* **Why:** In a Red Team engagement, this list is the foundation for Password Spraying attacks or Phishing campaigns.
* **Legal Status:** Since this uses **Passive OSINT** (publicly available data), gathering this list is generally legal. However, sending emails to them without permission would cross the line.

## Usage & Commands
The syntax is straightforward, but choosing the right "backend" (search engine) is key.

```bash
theHarvester -d <domain> -b <source> -l <limit>
```

- **-d:** The target domain (e.g., ine.com).
- **-b:** The data source (e.g., google, bing, linkedin, duckduckgo, or all).
- **-l:** Limits the number of results to search (default is often 500).

### Example Command

```bash
theHarvester -d ine.com -b duckduckgo,baidu,bing,yahoo -l 500
```

## Real-World Challenges (My Experience)

### 1. The Search Engine Ban
Just like with Sublist3r, search engines (especially Google) hate automated scrapers.

**Issue:** Google often blocks theHarvester instantly with a CAPTCHA, returning zero results.

**Solution:** I rely more on bing, duckduckgo, or baidu as they are slightly more lenient.

### 2. The Power of API Keys
The tool is severely limited without API keys.

- By default, it scrapes HTML code (which breaks often).
- Ideally, API keys for services like Hunter.io, Shodan, or Intelx should be configured in the `api-keys.yaml` file.

This makes the tool far more powerful and reliable because it queries the database directly instead of scraping a search engine.

## What to Do with the Data?
Finding `john.doe@target.com` gives me the naming convention of the company (e.g., Firstname.Lastname).

Once I know the pattern, I can generate potential usernames for all employees found on LinkedIn, even if I didn't find their specific email address yet.
