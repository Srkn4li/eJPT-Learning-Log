# Port Scanning with Nmap

## Overview

Once I have a list of live IPs, I need to know: **"Which doors are open?"**
Port scanning identifies running services (Apache, RDP, SMB) and gives me the information I need to spot vulnerabilities.

---

## 1. Port Selection Flags

By default, Nmap scans the top 1000 most common ports. I can change this behavior:

* **Scan Top 1000 (Default):** `nmap <IP>`
* **Fast Scan (Top 100):** `nmap -F <IP>`
  *Use case:* Quick overview.
* **Scan ALL Ports (1–65535):** `nmap -p- <IP>`
  *Use case:* Full pentest, ensures nothing hides on high ports.
* **Specific Ports:** `nmap -p 80,445,3389 <IP>`
* **Port Range:** `nmap -p 1-10000 <IP>`

---

## 2. Scan Types & Protocols

### **Skip Host Discovery (`-Pn`)**

Tells Nmap: *"Assume the host is online, don’t ping it first."*
Useful when ICMP (Ping) is blocked but ports are still open.

### **UDP Scan (`-sU`)**

Scans for UDP services such as DNS, SNMP, DHCP.
*Note:* **Very slow** because UDP is stateless and often sends no reply.

---

## 3. Service & OS Detection ("The Deep Dive")

```bash\ nmap -Pn -sV -sC -O <IP>
```

**Flag Explanation:**

* **`-sV` (Version Detection):** Shows service versions (e.g., Apache 2.4.49). Helps identify CVEs.
* **`-O` (OS Detection):** Guesses operating system based on TCP/IP fingerprinting.
* **`-sC` (Default Scripts):** Runs default NSE scripts for extra info and basic vulnerability checks.

---

## 4. Timing & Performance (`-T`)

Controls the aggressiveness of the scan:

* **`-T0 / -T1` (Paranoid/Sneaky):** Extremely slow, used for IDS evasion.
* **`-T3` (Normal):** Default.
* **`-T4` (Aggressive):** Ideal for labs. Fast and reliable.
* **`-T5` (Insane):** Very fast, high risk of missing ports or crashing targets.

**My rule:** Stick to **`-T4`** unless stealth is required.

---

## 5. Output Management

Saving results is mandatory.

```bash
nmap -Pn -p- -T4 -oN scan.txt <IP>
```

* **`-oN filename.txt`**: Normal readable output.
* **`-oX filename.xml`**: XML format, compatible with tools like Metasploit or SearchSploit.

---

## My Takeaway

The go-to starting command in most labs:

```bash
sudo nmap -Pn -sV -sC -O -T4 <Target-IP>
```

It gives versions, scripts, and OS info in a reasonable timeframe — the perfect balance for recon.
