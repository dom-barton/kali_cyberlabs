# Lab 03 – Service Version Detection (`nmap -sV`)

This lab demonstrates how to perform service version detection using `nmap` against a virtual gateway interface in a VMware-hosted environment.

## Target
- `192.168.174.1` (VMware gateway IP)

## Tools
- Kali Linux (in VMware Workstation)
- nmap 7.95

## Scan: Service Version Detection (`-sV`)
```bash
nmap -sV 192.168.174.1 -oN scan_sV.txt
```
## Result summary
- Host is up (0.00032s latency)
- All 1000 TCP ports reported as filtered (no response)
- No open or closed ports detected
- No version banners retrieved
- MAC address: 00:50:56:C0:00:08 (VMware virtual interface)

- Output saved in: `scan_sV.txt`

## Interpretation
This scan attempted to identify software and version banners for any listening TCP services.  
The output indicates that **all ports are filtered**, meaning:
- the host is likely behind a firewall or NAT device that blocks scans,
- there may be no services currently listening on common ports,
- or the system is configured in a way that does not respond to these probes (intentionally or by default).

Since version detection relies on responses from running services, no version info could be retrieved in this case.

### Notes
- `-sV` (Service Version Detection) performs banner grabbing and protocol-based probing to identify running software.
- Useful for matching versions to known vulnerabilities (e.g. outdated Apache, OpenSSH).
- This scan returned no usable data – but that's also a valid and realistic result.
---

> **Personal note/reflection:**  
> This was the final scan executed against the VMware gateway (192.168.174.1).  
>   
> The purpose of labs 01–03 was to build foundational skills with basic `nmap` scan types (`-sT`, `-sS`, `-sV`), to learn to interpret filtered/closed/open port states, and to get comfortable with documenting output and observations.  
>   
> What's next? From `Lab 04` onward, scans will be performed against a dedicated target VM (e.g. Ubuntu or Windows 10) on the same virtual network. These targets will be configured with real services (e.g. SSH, HTTP) to allow deeper exploration:  
> - service discovery  
> - vulnerability assessment  
> - response fingerprinting  
> - and later, exploitation
---
> **Learning note:**  
> In network scanning, a "banner" is a message sent by a service when a connection is made — it often includes software name and version.
> 
> Tools like `nmap -sV` try to collect this info to help identify possible vulnerabilities.
