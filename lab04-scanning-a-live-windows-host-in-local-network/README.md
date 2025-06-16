# Lab 04 – Scanning a Live Windows Host in Local Network

This lab continues the practical use of `nmap` by scanning a real Windows 10 machine running in a VMware virtual network.

## Target:
- Windows 10 VM
- IP: `192.168.75.128`

## Tools
- Kali Linux VM (attacker)
- Windows 10 VM (target)
- Both running in **VMware** on the same **Host-Only** network
- nmap 7.95

## Scans:

### 1. TCP Connect Scan (`-sT`)
```bash
nmap -sT 192.168.75.128 -oN lab04_scan_sT.txt
```
## Result summary – TCP Connect Scan (`-sT`)
- Host is up (0.00043s latency)
- Open TCP ports:
  - 135/tcp — msrpc
  - 139/tcp — netbios-ssn
  - 445/tcp — microsoft-ds
- MAC address: 00:0C:29:48:42:A1 (VMware virtual interface)

- ### 2. TCP SYN Scan (`-sS`)
```bash
nmap -sS 192.168.75.128 -oN lab04_scan_sS.txt
```
## Result summary – TCP SYN Scan (`-sS`)
- Host is up (0.00048s latency)
- Same 3 ports confirmed as open:
  - 135/tcp — msrpc
  - 139/tcp — netbios-ssn
  - 445/tcp — microsoft-ds
- This scan is faster and stealthier than full connect

- ### 3. Service Detection Scan (`-sV`)
```bash
nmap -sV 192.168.75.128 -oN lab04_scan_sV.txt
```
## Result summary – Service Version Detection (`-sV`)
- Host is up (0.00032s latency)
- Detected services:
  - 135/tcp — Microsoft Windows RPC
  - 139/tcp — Microsoft netbios-ssn
  - 445/tcp — Microsoft SMB
- OS fingerprint (CPE): `cpe:/o:microsoft:windows`
- No additional version banners retrieved

## Interpretation
This lab introduced a **new VM target** (Windows 10), replacing the previous VMware gateway scans.

All three nmap scans (`-sT`, `-sS`, and `-sV`) successfully identified **open and active TCP services**:
- Port 135 (RPC), port 139 (NetBIOS), and port 445 (SMB) were consistently detected as open.
- The version detection scan (`-sV`) confirmed that these are **native Windows services**, commonly found on default configurations.

This demonstrates that:
- The target VM is responsive and running services,
- These ports are often leveraged in **network enumeration, lateral movement, or vulnerability assessment**,
- Nmap’s scan types reveal different levels of detail: TCP connect is easiest but “loudest,” SYN is stealthier, and `-sV` adds application-level info.

The **Windows firewall initially blocked ping and scanning**; connectivity was established only after disabling it — a reminder of how **host-based firewalls** affect reconnaissance.


### Notes
- These ports are commonly exposed on Windows systems and play key roles in file sharing and RPC-based communication.
- Port 445 (SMB) is a known target for several exploits (for example EternalBlue).
- This lab sets the ground for further enumeration, vulnerability discovery, and eventually penetration testing against Windows environments.
---
> **Personal note/reflection:**  
> This is my first scan against a real OS, not just a gateway IP. I manually installed Windows 10 in VMware as the target. Initially, I couldn’t reach the target from Kali.After some troubleshooting, I realized Windows Firewall was blocking ICMP and other inbound requests.Once I disabled the firewall, Kali successfully pinged the host and the scans produced real results.
This feels like a meaningful shift – from passive discovery to real-world service enumeration. I now have a real target with known Windows services, and I can begin learning how to interact with and analyze these surfaces.
---
> **Learning note:**  
> The scan results revealed services like RPC, NetBIOS and SMB — I’ve seen the names before, but this was my **first time identifying them via actual nmap output**.  
> - I’ve learned that port 135 (RPC) and 445 (SMB) are often **leveraged** during enumeration or lateral movement by attackers — “leveraged” in this context means “actively used as part of the attack path.”  
> - Terms like **network enumeration**, **vulnerability assessment**, and **lateral movement** were previously known to me *theoretically* (from Cisco courses), but now they’re starting to make sense *practically* in context.  
> - I also encountered the concept of **banner grabbing** in `-sV` scan, even if in this case, no actual banner was returned — it's the idea of retrieving version/service info during scanning.  
> - This was also my **first scan against a real host (Windows 10)**. I didn’t get any response at first, and only after disabling the Windows Firewall did the host become discoverable and scannable.  
> - That helped reinforce how **host-based firewalls** block external probing and make assets “invisible” during passive/active scans — and why firewall misconfiguration is a frequent security issue.
