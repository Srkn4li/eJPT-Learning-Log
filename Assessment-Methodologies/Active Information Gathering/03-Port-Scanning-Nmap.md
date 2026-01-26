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

# Port Scanning Basics with Nmap

> **Source:** Port Scanning With Nmap - Part 1 & 2

After host discovery (finding out *who* is online), the next logical step is port scanning (finding out *what* services are listening).

---

## The Default Behavior
If I run Nmap without specifying any scan flags, it performs a "Default Scan" on the top 1000 ports.

```bash
sudo nmap <target-ip>
```

What happens under the hood depends on my privileges:
* **Running as root (`sudo`):** Nmap defaults to a **SYN Scan (-sS)**.
* **Running as non-root user:** Nmap defaults to a **TCP Connect Scan (-sT)**.

### The -Pn Switch (Critical for Windows)
By default, Nmap pings a host first. If the ping fails (which happens on almost all Windows targets due to firewalls), Nmap thinks the host is down and stops.

**Solution:** Always use `-Pn` to skip host discovery and force the port scan.

```bash
# "Treat all hosts as online" - Skip ping
sudo nmap -Pn <target-ip>
```

---

## Scan Types

### 1. TCP SYN Scan (-sS) - The Gold Standard
The preferred, default, and fastest method when running as root. It is "Stealthy" because it never completes the TCP handshake.

* **How it works:** Sends SYN. If Target sends SYN/ACK, Nmap immediately sends RST.
* **Pros:** Fast, quieter in logs.

```bash
sudo nmap -Pn -sS <target-ip>
```

### 2. TCP Connect Scan (-sT)
The fallback for non-root users.

* **How it works:** Completes the full 3-Way Handshake (SYN -> SYN/ACK -> ACK).
* **Cons:** Slower, noisier (appears in system logs).

```bash
nmap -Pn -sT <target-ip>
```

### 3. UDP Scan (-sU)
Required for services like DNS (53), SNMP (161), and DHCP (67/68).

* **How it works:** Sends empty UDP packets.
* **Cons:** Extremely slow and often ambiguous (firewalls make it hard to distinguish Open vs. Filtered).

```bash
# Scan common UDP ports only (Recommended)
sudo nmap -Pn -sU -p 53,137,138,139 <target-ip>
```

---

## Understanding Port States
This is the most important concept for identifying firewalls.

| State | Meaning | Implication (Windows) |
| :--- | :--- | :--- |
| **Open** | Service is listening. | I can attack this service. |
| **Closed** | Host responded with RST. | **No Firewall.** The host is up, but nothing is running on this port. |
| **Filtered** | No response (Silence). | **Firewall Detected.** Packets are being dropped. |

**The Windows Logic:**
* If ports are **Closed**: Windows Firewall is likely **OFF**.
* If ports are **Filtered**: Windows Firewall is likely **ON**.

---

## Port Specification & Timing

### Define Targets

```bash
# Top 100 common ports (Fast)
nmap -F <ip>

# Specific Ports
nmap -p 80,445,3389 <ip>

# All TCP Ports (1-65535) - Takes time!
nmap -p- <ip>
```

### Speed (-T Template)
* **-T3:** Normal (Default).
* **-T4:** Aggressive (Recommended for CTFs/Labs).
* **-T5:** Insane (Can miss ports, too fast).

---

## Cheat Sheet: Practical Examples

### 1. Quick Recon (Windows Target)
Skips ping, uses SYN scan, scans top 100 ports.
```bash
sudo nmap -Pn -sS -F <ip>
```

### 2. Full TCP Scan (The "Real" Scan)
Scans all ports with aggressive timing.
```bash
sudo nmap -Pn -sS -p- -T4 <ip>
```

### 3. Targeted TCP + UDP
Checks specific services often found on Windows.
```bash
sudo nmap -Pn -sS -sU -p 53,80,139,445,3389 <ip>
```
