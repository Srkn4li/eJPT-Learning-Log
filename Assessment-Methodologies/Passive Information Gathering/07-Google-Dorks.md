# Google Dorks (Google Hacking)

## Overview
Google Dorking involves using advanced search operators to find specific information that websites might have unintentionally exposed. Instead of searching for general topics, I use these operators to "hack" the search results and find sensitive files, admin portals, or network details.

## Essential Operators

### 1. Domain & Subdomain Filtering
* **`site:target.com`**
  * Limits results to the specific domain and its subdomains.
  * *Usage:* This is my base filter. I almost always start with this to remove noise from other sites.
* **`site:*.target.com` (Subdomain Hunting)**
  * Used to explicitly look for subdomains.
  * *Pro Tip:* Combine with `-www` (e.g., `site:target.com -www.target.com`) to filter out the main website and only see interesting subdomains like `dev.target.com` or `vpn.target.com`.

### 2. Finding Admin Panels & Portals
* **`site:target.com inurl:admin`**
  * Looks for the word "admin" specifically in the URL string.
* **`site:target.com intitle:admin`**
  * Looks for pages where the HTML title tag contains "admin".
  * *Why use both?* Sometimes the URL is cryptic (`/login.php`), but the page title says "Admin Login".

### 3. File Hunting (`filetype`)
* **`site:target.com filetype:pdf`**
  * Finds publicly indexed PDF documents.
  * *Why it matters:* Companies often leak internal policies, organizational charts, or user manuals in PDFs.
* **Other interesting types:** `filetype:docx` (Word docs), `filetype:xlsx` (Excel sheets with data), `filetype:conf` (Configuration files).

## Critical Vulnerabilities (The "Danger Zone")

### Directory Listing
* **`intitle:"index of"`**
  * *What it finds:* Servers that are misconfigured to list all files in a directory (instead of showing a web page).
  * *Risk:* I can browse files freely, often finding backups or keys.

### Sensitive Files
* **`inurl:auth_user_file.txt`** or **`inurl:passwd.txt`**
  * Attempts to find files containing passwords or user hashes.
* **GHDB (Google Hacking Database):**
  * A massive collection of these dorks maintained by Exploit-DB. It classifies dorks into categories like "Files containing passwords" or "Vulnerable Servers".

## Historical Reconnaissance

### Google Cache
* **`cache:target.com`**
  * Shows the version of the website that Google has saved. Useful if the site is currently offline or if content was recently deleted.

### The Wayback Machine (Archive.org)
* Unlike Google Cache (which is recent), the **Wayback Machine** is a time machine.
* I can view the website as it looked 5 or 10 years ago.
* *Value:* Old versions might reveal contact names, old email addresses, or legacy server structures that are still active but unlinked today.
