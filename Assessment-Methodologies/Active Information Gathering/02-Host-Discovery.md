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
