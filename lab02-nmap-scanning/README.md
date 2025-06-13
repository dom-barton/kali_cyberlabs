# Lab 02 – Practical nmap Scanning

This lab explores several types of nmap scans on a local network target (VMware gateway or test machine).

## Target (VMware gateway)
192.168.174.1

## Tools
- Kali Linux (in VMware)
- nmap 7.95

## Scans performed

### 1. TCP Connect Scan (`-sT`)
```bash
nmap -sT 192.168.174.1 -oN scan_sT.txt
```
## Result summary
- Target: 192.168.174.1
- Host is up (0.00030s latency)
- All 1000 scanned TCP ports are in filtered state (no response)
- MAC address resolved: 00:50:56:C0:00:08 (VMware virtual NIC)
- No open or closed ports detected

## Interpretation
This scan used a full TCP connection method (3-way handshake). The results are consistent with a host that is:
- Protected by a firewall dropping unsolicited packets
- (or) Not offering any open TCP services on standard ports
Output saved in: `scan_sT.txt`

### Notes
- This scan targeted TCP ports only (the most common service ports: 1-1000).
- Because TCP is connection-oriented, a full **3-way handshake** (SYN → SYN-ACK → ACK) was used to verify if a service is listening.
- TCP Connect scan (`-sT`) relies on the OS to initiate real connections – it’s **louder** and more detectable in logs.
- Useful when scanning from environments **without administrator privileges**, or when doing a full scan intentionally.

### 2. TCP SYN Scan (`-sS`)
```bash
nmap -sS 192.168.174.1 -oN scan_sS.txt
```
## Result summary:
- (same) Target: 192.168.174.1
- Host is up (0.00053s latency)
- All 1000 TCP ports reported as `filtered` (no response)
- MAC address: `00:50:56:C0:00:08` (VMware virtual interface)
- No open or closed ports detected

## Interpretation
This was a **stealth scan**, using only the initial SYN packet to test port availability. No full connection (handshake) was established.
The filtered results indicate:
- The target system is protected by a firewall or packet filter
- No services responded during scanning
- The stealth approach was likely blocked or logged silently

### Notes
- Unlike TCP Connect (`-sT`), this method is less intrusive and harder to detect
- SYN scans require elevated privileges (root) to craft raw packets
- Frequently used in **security audits** or **reconnaissance** to avoid triggering alerts
- Despite being "stealthy", filtered results suggest **tight network controls**
---

> **Personal note:**
> Although the SYN scan is meant to avoid detection, none of the ports responded.
> This likely means that strict firewall rules are silently dropping packets without revealing any open services.  
> From a defensive perspective, this is a good example of a hardened perimeter — the system reveals nothing unless explicitly allowed.
...
