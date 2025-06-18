# Lab 05 – SMB Enumeration on Windows 10 VM

This lab explores SMB (Server Message Block) enumeration on a Windows 10 virtual machine using two command-line tools: `smbclient` and `enum4linux`. The objective is to identify accessible file shares, system names, and domain-related data.

## Target
- Windows 10 VM
- IP address: `192.168.75.128`

## Tools
- VMware Workstation 17 Pro
- Kali Linux: Attacking machine (configured inside VMware)
- Windows 10: Target machine (configured inside VMware)

> **Personal note/reflection:**  
> This was my first time performing SMB-specific enumeration on a real operating system (Windows 10). Although the result was a denial message, it made me realize that even a failed scan tells a story, in this case, that anonymous SMB enumeration is disabled.
It also helped reinforce the role of authentication policies and host-based firewalls in defending against basic probing and enumeration.
---

## Step 1: SMB share discovery with `smbclient`

```bash
smbclient -L \\192.168.75.128 -N
```

### Result
```bash
session setup failed: NT_STATUS_ACCESS_DENIED
```

### Interpretation
- The target does **not** allow anonymous (null session) SMB access.
- This is **expected** in most secure/default Windows environments.
- It indicates that file shares cannot be listed unless valid credentials are supplied.

> **Learning note:**  
> `smbclient` is a command-line tool to interact with Windows shares. The `-L` flag lists available shares, and `-N` attempts access **without credentials**.
> Receiving `NT_STATUS_ACCESS_DENIED` means that the system is rejecting null sessions (a typical security setup).

---

## Step 2: SMB/NetBIOS Enumeration with `enum4linux` tool

```bash
enum4linux -a 192.168.75.128 | tee enum4linux_win10.txt
```

### Key output (cleaned)

The original `enum4linux` output included repeated headers, blank sections, and formatting artifacts (ASCII lines, separators, spacing).  
Below is a **reduced and cleaned version** that highlights only the useful data retrieved from the target:

```
[+] Got domain/workgroup name: WORKGROUP

Looking up status of 192.168.75.128
  DESKTOP-SUACGUP <00> - Workstation Service
  WORKGROUP       <00> - Domain/Workgroup Name
  DESKTOP-SUACGUP <20> - File Server Service
  MAC Address = 00-0C-29-48-42-A1

[E] Server doesn't allow session using username '', password ''.
```
> Note: Full unedited scan output is saved in `enum4linux_win10.txt` for reference.

### Interpretation
- Even without authentication, `enum4linux` revealed basic system info:
  - Hostname: `DESKTOP-SUACGUP`
  - Workgroup: `WORKGROUP`
  - NetBIOS services running: Workstation + File server
  - MAC address of the host
- However, more detailed enumeration (users, shares, policies) was blocked due to lack of credentials.

> **Learning note:**  
> `enum4linux` is a powerful enumeration tool for SMB, NetBIOS, and RPC. It's useful during **penetration testing** and **internal audits**.
> Even without full access, tools like this can leak useful metadata (for example hostnames, MACs, domains).
> This is part of **network reconnaissance** in ethical hacking.
>
> The `-a` flag in `enum4linux -a` stands for "**all**". It tells the tool to run **all available ("simple") enumeration modules**, including user lists, shares, group info, OS details, and more (-U -S -G -P -r -o -n -i). It's the most comprehensive scan mode.
>  
> The `tee` command is a Linux utility that lets you **save the output to a file** while **also displaying it in the terminal**.  
> In this case:  
> - `tee enum4linux_win10.txt` saves the results to a file, while you still see the results live in your terminal window.

---

> **Personal note/reflection:**  
> The scan results revealed services like RPC, NetBIOS, and SMB — I’ve seen the names before, but this was my **first time identifying them via actual nmap and enum4linux output**.
> - I’ve learned that ports like **135 (RPC)** and **445 (SMB)** are often used for **enumeration** or **lateral movement** by attackers.
> - Terms like **banner grabbing**, **vulnerability assessment**, and **lateral movement** now have clearer meaning.
> - I now better understand how simple null sessions can still reveal small details — and why blocking them is a good defense.

---

## Summary
| Tool         | Purpose                           | Outcome                         |
|--------------|-----------------------------------|----------------------------------|
| `smbclient`  | Check for accessible shares       | Anonymous access denied         |
| `enum4linux` | NetBIOS + SMB metadata discovery | Hostname, MAC, Workgroup found  |


## What's next?
Next labs will probably include:
- Using **Nmap NSE scripts** against SMB
- Exploring **authenticated access** (with created user)
- Setting up vulnerable services (for example EternalBlue) for exploit simulations
