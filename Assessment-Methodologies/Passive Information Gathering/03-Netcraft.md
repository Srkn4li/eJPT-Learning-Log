# Website Footprinting with Netcraft

## Overview
Netcraft is a powerful internet services company that provides a free web interface for passive information gathering. It is excellent for analyzing a target domain's infrastructure without sending direct packets from my own machine.

### 1. Key Information Gathered
By simply visiting `https://sitereport.netcraft.com` and entering the target domain, I can find:
* **Background:** Domain registrar, registration date, and hosting country.
* **Network:** IP address owner and autonomous system (AS).
* **Email:** Often lists contact addresses (admin/tech) associated with the domain.

### 2. SSL/TLS Security Analysis
This is a critical part of the report. Netcraft analyzes the target's encryption standards:
* **Certificate Expiry:** Knowing when the SSL certificate expires is vital. Expired certificates can indicate abandoned infrastructure or poor maintenance.
* **Vulnerabilities:** Netcraft checks for known SSL weaknesses, such as:
  * **SSLv3 / POODLE:** Legacy protocols that should be disabled.
  * **Heartbleed:** A critical vulnerability in OpenSSL.
* **Protocol Support:** Shows if modern TLS versions (1.2/1.3) are enforced.

### 3. Web Trackers & Technology
* **Web Trackers:** Identifies third-party scripts (like Google Analytics, Facebook Pixel) running on the site. This reveals business relationships and marketing tools used.
* **Site Technology:** Similar to "BuiltWith", it lists the web server (e.g., Apache, Nginx) and other underlying technologies.

## Conclusion
Netcraft provides a deep "X-Ray" of a website's history and security posture purely through public records. It is an essential first step before active scanning.
