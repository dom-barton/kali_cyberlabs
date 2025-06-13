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
