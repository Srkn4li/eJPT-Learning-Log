# Website Recon & Footprinting

## Passive Information Gathering
In this module, I learned how to gather information about a target website without directly interacting with the server in a malicious way.

### 1. Standard Files
Every web developer leaves trails. I learned to check these first:
* **robots.txt**: Instructs search engine crawlers which pages *not* to index. This is often a goldmine for finding hidden directories (e.g., `/admin` or `/backup`).
* **sitemap.xml**: Provides a list of accessible pages. Useful to understand the structure of the target.

### 2. Browser Extensions
Tools to identify the technology stack (Frameworks, CMS, OS, Analytics) just by visiting the site:
* **BuiltWith**: Detailed profile of the technology running the website.
* **Wappalyzer**: Identifies technologies instantly (e.g., WordPress, Apache, PHP versions).

### 3. Kali Linux Tools
* **WhatWeb**: A command-line tool to identify websites.
  * Command: `whatweb <domain>`
  * *My takeaway:* It's like Wappalyzer but for the terminal, very fast for scanning multiple targets.
* **HTTrack**: A website copier utility.
  * *Usage:* Allows downloading a World Wide Web site from the Internet to a local directory.
  * *Use Case:* I can browse the site offline and search for sensitive info in the source code without triggering alerts on the live server.
