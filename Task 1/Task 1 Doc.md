# 🔍 Nmap Network Scanning Lab

> A step-by-step guide to performing TCP SYN network scans, analyzing results, and identifying potential security risks from open ports.

---

## Prerequisites

| Requirement | Details |
|---|---|
| OS | Windows / Linux / macOS |
| Privileges | Root / Administrator (required for SYN scan) |
| Tools | Nmap, Wireshark (optional) |
| Network | Access to local LAN |

---

## Step 1: Install Nmap

### 🐧 Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install nmap -y
nmap --version
```



### 🪟 Windows

1. Visit the official website: [https://nmap.org/download.html](https://nmap.org/download.html)
2. Download the **Windows self-installer** (`.exe`)
3. Run the installer and follow the setup wizard
4. Open **Command Prompt as Administrator**
5. Verify installation:

```cmd
nmap --version
```
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/nmap.png">

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/nmap%201.png">

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/nmap%20install.png">

> ✅ **Tip:** On Windows, Nmap installs **Npcap** (packet capture driver) automatically — required for SYN scans.

---

## Step 2: Find Your Local IP Range

### 🐧 Linux / macOS

```bash
ip a
# or
ifconfig
```

Look for your network interface (e.g., `eth0`, `wlan0`) and note the IP address.

**Example output:**
```
inet 192.168.1.105/24
```
Your IP range is: `192.168.1.0/24`

### 🪟 Windows

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/ip.png">

```cmd
ipconfig
```

Look for **IPv4 Address** and **Subnet Mask**:
```
IPv4 Address. . . : 192.168.0.193
Subnet Mask . . . : 255.255.255.0
```
Your IP range is: `192.168.0.1/24`

---

## Step 3: Run TCP SYN Scan

> ⚠️ **Requires root/administrator privileges.**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/scan.png">

# Fast scan (top 100 ports)
sudo nmap -sS -F 192.168.1.0/24
```

### What is a TCP SYN Scan?

A TCP SYN scan (also called a **"half-open scan"**) works by:

1. Sending a **SYN** packet to the target port
2. If the port is **open** → target replies with **SYN-ACK**
3. Nmap sends a **RST** (reset) instead of completing the handshake
4. If the port is **closed** → target replies with **RST**
5. If **filtered** → no response (firewall drops the packet)

```
Attacker          Target
   |---[SYN]------->|   Port Open?
   |<--[SYN-ACK]----|   Yes → Open
   |---[RST]------->|   Connection reset (stealthy)
```

---

## Step 4: Note IP Addresses & Open Ports

After running the scan, record the discovered hosts and open ports.

**Example scan result:**

```
Nmap scan report for 192.168.0.200
Host is up (0.0023s latency).
PORT     STATE  SERVICE
8008/tcp   open   http
8009/tcp   open   ajp13
8443/tcp  open   https-alt
9000/tcp open   cslistener

Nmap scan report for 192.168.0.193
Host is up (0.0011s latency).
PORT     STATE  SERVICE
135/tcp  open   msrpc
139/tcp  open   netbios-ssn
445/tcp  open   microsoft-ds
912/tcp open   apex-mesh
```

## Step 5: Analyze with Wireshark (Optional)

### Install Wireshark

- **Linux:** `sudo apt install wireshark -y`
- **macOS:** `brew install --cask wireshark`
- **Windows:** Download from [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)

### Capture While Scanning

1. Open Wireshark and select your network interface
2. Start capture (click the **blue shark fin** icon)
3. Run your Nmap scan in a terminal
4. Stop capture after the scan completes


### Useful Wireshark Filters
```wireshark
# Filter SYN packets only
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Filter SYN-ACK (open ports)
tcp.flags.syn == 1 && tcp.flags.ack == 1

# Filter RST packets (closed ports)
tcp.flags.reset == 1

# Filter by source IP (your machine)
ip.src == 192.168.1.105

# Filter traffic to specific target
ip.dst == 192.168.1.1
```
<img src=https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/wireshark%201.png>
---

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/33742b6b1ae7970270920a7120849d6bf851595d/Task%201/Images/wireshark.png">


## Step 6: Common Services on Ports

| Port | Protocol | Service | Description |
|---|---|---|---|
| 21 | TCP | FTP | File Transfer Protocol |
| 22 | TCP | SSH | Secure Shell (remote login) |
| 23 | TCP | Telnet | Unencrypted remote login (legacy) |
| 25 | TCP | SMTP | Email sending |
| 53 | TCP/UDP | DNS | Domain Name System |
| 80 | TCP | HTTP | Web traffic (unencrypted) |
| 110 | TCP | POP3 | Email retrieval |
| 135 | TCP | MSRPC | Windows Remote Procedure Call |
| 139 | TCP | NetBIOS | Windows file sharing (legacy) |
| 143 | TCP | IMAP | Email retrieval |
| 443 | TCP | HTTPS | Web traffic (encrypted) |
| 445 | TCP | SMB | Windows file/printer sharing |
| 3306 | TCP | MySQL | Database server |
| 3389 | TCP | RDP | Windows Remote Desktop |
| 5432 | TCP | PostgreSQL | Database server |
| 5900 | TCP | VNC | Remote desktop (Virtual Network Computing) |
| 6379 | TCP | Redis | In-memory data store |
| 8080 | TCP | HTTP-Alt | Alternative web server port |
| 27017 | TCP | MongoDB | NoSQL database server |

---

## Step 7: Identify Security Risks

### 🔴 High Risk Ports

- 445 SMB and 139 NetBIOS are the most dangerous — directly linked to WannaCry/EternalBlue. Block at the firewall from external access.
- 135 MSRPC should never be internet-facing — disable via Windows Firewall if not needed on LAN.
- 8009 AJP (Ghostcat vulnerability, CVE-2020-1938) — disable in your Tomcat/Apache config if you're not using it internally.

### 🟡 Medium Risk Ports

- 902 and 912 are VMware ESXi ports — strong sign a hypervisor is on the network. Restrict to a management VLAN, never expose to the internet.
- 8008 — unencrypted HTTP on an alternate port. 
- 7070 — uncertain service. Use nmap -sV -p 7070 <IP> to fingerprint exactly what's running.

### 🟢 Generally Safe Ports

- 8443 HTTPS-alt and 9000 are acceptable if firewalled internally, but verify what's behind them (Portainer? PHP-FPM? Admin panel?).


## Step 8: Save Scan Results

### Save as Text File

```bash
# Normal output to text file
sudo nmap -sS 192.168.1.0/24 -oN scan_results.txt

# Grepable output
sudo nmap -sS 192.168.1.0/24 -oG scan_results_grepable.txt

# All formats at once
sudo nmap -sS 192.168.1.0/24 -oA scan_results
```

### Save as HTML File

```bash
# XML output (required for HTML conversion)
sudo nmap -sS 192.168.1.0/24 -oX scan_results.xml

# Convert XML to HTML using xsltproc
xsltproc /usr/share/nmap/nmap.xsl scan_results.xml -o scan_results.html
```

Open `scan_results.html` in any browser for a formatted, readable report.

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/c34db95a5e5360218e3aee72205522310a9d9a8d/Task%201/Images/XML%20file.png">

---
