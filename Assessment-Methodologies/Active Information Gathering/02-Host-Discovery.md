# Host Discovery (Ping Sweeps & ARP)

## Overview

Before scanning for open ports (which takes a lot of time), I need to know which IP addresses are actually assigned to live hosts. This phase is called **Host Discovery**.
Scanning a "dead" IP is a waste of time. My goal is to create a list of live targets in the network range (e.g., `192.168.2.0/24`).

## Tools & Techniques

### 1. Nmap (Ping Sweep)

Nmap is the industry standard for network mapping. The `-sn` flag (formerly `-sP`) tells Nmap to **disable port scanning** and only check if the host is up.

**Command:**

```bash
sudo nmap -sn 192.168.2.0/24
```

**Flag Explanation:**

* `-sn`: Ping Scan — no ports are scanned.

**How it works:**

**Local Network (LAN):**

* Nmap sends ARP requests such as: "Who has IP 192.168.2.5?"
* ARP responses are extremely fast and reliable because every device must answer ARP to communicate.

**Remote Network:**

* Nmap sends ICMP Echo Requests (Ping) and TCP probes to check if the host is reachable.

---

### 2. Netdiscover (Passive/Active ARP Recon)

Netdiscover is an active/passive ARP reconnaissance tool. It's ideal for wireless networks or when I plug into a network jack and want to see who else is present — without generating too much noise.

**Command:**

```bash
sudo netdiscover -i eth0 -r 192.168.2.0/24
```

**Why use it?**

* Great for finding devices that block standard ICMP pings but still communicate via ARP.

---

### My Takeaway

* **Nmap** is my go-to for speed when I have a specific range.
* **Netdiscover** provides a live, rolling view of the network — perfect for spotting devices coming online in real time.

**Important:** These scans require `sudo` because crafting raw packets (ARP or ICMP) needs root privileges.

# Nmap Host Discovery: Advanced Techniques

## Overview
Now that I know how to scan a standard network, the focus shifts to efficiency and accuracy. In the real world, I will rarely scan a blind /24 subnet. Usually, I have a specific list of targets (Scope).
Additionally, firewalls (especially the default Windows Firewall) often block standard ICMP Pings. Therefore, I need techniques that use TCP packets to "trick" the target into responding.

---

## Managing Targets (Scopes & Lists)
There are smarter ways to feed targets into Nmap than typing IP addresses one by one.

### 1. Multiple IPs & Ranges
I can simply separate IPs with spaces or define ranges.

```bash
# Multiple specific IPs
nmap -sn 10.4.23.227 10.4.23.228

# IP Ranges - scans from .227 to .240
nmap -sn 10.4.23.227-240
```

### 2. Input from List (-iL)
This is the standard for penetration tests. I receive a list of IPs that are "in scope". I save these to a file (e.g., `targets.txt`), with each IP or range on a new line.

```bash
# Create targets.txt (e.g., using nano or vim)
# Content:
# 192.168.1.1
# 192.168.1.50-60
# 10.10.10.5

# Instruct Nmap to read the list
sudo nmap -sn -iL targets.txt
```

---

## The Scan Techniques (Bypassing Firewalls)
If the standard ping (`-sn`) says "Host down", it might be lying. Firewalls often block ICMP. Here are the alternatives.

### 1. TCP SYN Ping (-PS) - The Gold Standard
This is the most effective method. Nmap sends a TCP Packet with the SYN flag (usually the first step of a handshake).

* **Default:** Sends to Port 80.
* **Logic:**
    * Port Open -> Host replies with SYN/ACK.
    * Port Closed -> Host replies with RST (Reset).
* **Result:** In both cases, Nmap knows: "The host is alive!"
* **Custom Ports:** I can specify which ports to test (e.g., RDP or SMB) if Port 80 is filtered.

```bash
# Standard SYN Ping (Port 80)
sudo nmap -sn -PS target

# Custom Ports (SSH, RDP, SMB) - Much more accurate!
sudo nmap -sn -PS22,80,445,3389 target

# Port Range (1-1000)
sudo nmap -sn -PS1-1000 target
```

### 2. TCP ACK Ping (-PA)
Here, Nmap sends a packet with the ACK flag.

* **Logic:** The target thinks "Huh? We don't have an active connection?" and should send back an RST.
* **The Problem:** Modern Stateful Firewalls (and Windows Firewall) recognize that there is no established connection and often block the packet completely.
* **Result:** Often unreliable; shows "Host down" even if it is up.

### 3. UDP Ping (-PU)
Sends UDP packets. Good if firewalls filter TCP heavily but forget to block UDP (e.g., for NetBIOS).

* **Target:** Often targets high ports or typical UDP services.

### 4. ICMP Echo Ping (-PE)
The classic "Ping".

* **The Problem:** Windows Firewall blocks ICMP Echo Requests by default. Therefore, it is almost useless against modern Windows systems.

---

## The "Pro" Methodology
How do I actually scan during a pentest? I don't rely on defaults. I combine techniques for maximum coverage and speed.

### My Preferred Command
I combine Host Discovery (`-sn`), Verbosity (`-v`), Speed (`-T4`), and specific TCP/UDP probes on common ports.

```bash
# Comprehensive Discovery Scan
sudo nmap -sn -v -T4 -PS21,22,25,80,445,3389,8080 -PU137,138 target
```

**Flag Explanation:**

* `-sn`: Host Discovery only (no Port Scan).
* `-v`: Verbose (tells me why a host is marked as "Up").
* `-T4`: Aggressive timing (faster).
* `-PS...`: SYN Ping on Common Ports (FTP, SSH, SMTP, Web, SMB, RDP).
* `-PU...`: UDP Ping on NetBIOS ports (in case TCP is filtered).

---

## My Takeaway
1.  **Never trust "Host Down":** If a simple scan finds nothing, a firewall is likely just blocking the ping.
2.  **Customize Probes:** By default, SYN ping only checks Port 80. If the web server is off but the Admin is connected via RDP, I will miss the host. Always check multiple ports (22, 445, 3389).
3.  **Avoid Port Scanning during Discovery:** Always use `-sn`. If I forget this, Nmap starts scanning 1000 ports on every IP, which takes forever on a whole network. **Find first, scan later.**
