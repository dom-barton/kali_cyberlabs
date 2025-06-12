# Lab 01 – Basic Network Analysis (Wireshark & nmap in Kali Linux)

## Summary
This is my first practical network analysis lab using Kali Linux in a virtual environment.  
I used Wireshark to capture a basic TCP three-way handshake during an HTTPS request.  
Then I performed a simple SYN scan with `nmap` on my router.

## Tools
- Wireshark
- nmap
- Kali Linux (in VMware)

## Part 1 – TCP handshake capture
Traffic was captured on interface `eth0` while accessing a website (HTTPS).

### Observed packets:
- SYN → from my machine to remote server (port 443)
- SYN, ACK → response from server
- ACK → connection established

### TCP flags:
- SYN: request to initiate connection
- ACK: confirms previous packet
- SYN, ACK: combination of both – used by server in handshake

See `handshake.png` for screenshot from Wireshark.

## Part 2 – Basic nmap scan

Executed from terminal:
```bash
nmap -sS 192.168.174.1 -oN scan1.txt

This was a SYN scan (-sS) performed against my local gateway IP 192.168.174.1
(likely the VMware NAT or bridged network interface).

### Scan results summary:

- Host is up (responded with very low latency)
- All 1000 TCP ports returned `filtered`
- This typically means:
  - Firewall is silently dropping packets,
  - No services are actively listening,
  - Host is deliberately configured to not respond (stealth mode)
- The scan output was saved to `scan1.txt` and is available in this folder.

## Notes
- First time I used Wireshark to actively trace TCP flags in a real connection.
- Observed how the source port is randomly assigned by the OS during connection.
- Planning to explore TLS negotiation steps and perform subnet scanning in future labs.
- Now completed a first live SYN scan with nmap and saved the output for interpretation.
