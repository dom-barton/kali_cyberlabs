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
