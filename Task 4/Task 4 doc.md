# Task 4: Setup and Use a Firewall on Windows/Linux

**Objective:** Configure and test basic firewall rules to allow or block network traffic using built-in OS firewall tools.

**Tools Used:** Windows Defender Firewall / UFW (Uncomplicated Firewall) on Linux (Kali)

---

## Table of Contents

1. [Introduction to Firewalls](#1-introduction-to-firewalls)
2. [Linux – UFW (Uncomplicated Firewall)](#2-linux--ufw-uncomplicated-firewall)
3. [Windows – Windows Defender Firewall](#3-windows--windows-defender-firewall)
4. [Summary: How a Firewall Filters Traffic](#4-summary-how-a-firewall-filters-traffic)

---

## 1. Introduction to Firewalls

A **firewall** is a network security system that monitors and controls incoming and outgoing network traffic based on predefined security rules. It acts as a barrier between a trusted internal network and untrusted external networks.

| Feature | Description |
|---|---|
| **Traffic Direction** | Inbound (incoming) and Outbound (outgoing) |
| **Rule Basis** | IP address, port number, protocol (TCP/UDP) |
| **Action** | Allow, Block/Deny, or Log |
| **Types** | Packet filtering, Stateful inspection, Application layer |

---

## 2. Linux – UFW (Uncomplicated Firewall)

UFW is a user-friendly front-end for managing `iptables` firewall rules on Linux.

---

### Step 1: Install UFW

```bash
sudo apt update
sudo apt install ufw
```

**Screenshot – Installing UFW on Kali Linux:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/1.png">

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/2.png">

---

### Step 2: Enable UFW & List Current Firewall Rules

```bash
# Enable UFW
sudo ufw enable

# Check status
sudo ufw status

# Verbose output (shows default policies)
sudo ufw status verbose

# Numbered rules list
sudo ufw status numbered
```

**Screenshot – Enabling UFW and checking status (inactive → active):**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/3%20(2).png">

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/4.png">

> **Observed:** UFW was initially `inactive`. After running `sudo ufw enable`, status changed to `active`. Default policy: **deny (incoming)**, **allow (outgoing)**.

---

### Step 3: Add Rules – Allow SSH (Port 22) & HTTP (Port 80)

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw status numbered
```

**Screenshot – Rules added for SSH (22) and HTTP (80):**
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/5.png">


> **Observed:** Four rules added (IPv4 + IPv6 for each): port 22/tcp and 80/tcp both show `ALLOW IN` from `Anywhere`.
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/6.png">
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/6.png">
---

### Step 4: Allow SSH from a Specific Subnet Only

```bash
# Restrict SSH access to a specific subnet
sudo ufw allow from 192.168.1.0/28 to any port 22 proto tcp
sudo ufw status numbered
```

**Screenshot – SSH allowed only from subnet 192.168.1.0/28:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/7.png">


> **Observed:** Rule `[2] 22/tcp ALLOW IN 192.168.1.0/28` confirms SSH is restricted to the local subnet only — a best-practice security configuration.

---

### Step 5: Remove a Test Rule

```bash
# Remove the subnet-specific SSH rule
sudo ufw delete allow from 192.168.1.0/28 to any port 22 proto tcp
sudo ufw status numbered
```

**Screenshot – Rule deleted, firewall restored:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/8.png">

---

### Step 6: Delete the SSH Allow Rule (Port 22) – Restore Original State

```bash
sudo ufw delete allow 22/tcp
sudo ufw status numbered
```

**Screenshot – SSH rule removed, only HTTP (80) remains:**


> **Observed:** After deletion, only port 80/tcp rules remain. The firewall is restored to original state with SSH rule removed.

---

### UFW Quick Command Reference

| Command | Description |
|---|---|
| `sudo ufw enable` | Enable the firewall |
| `sudo ufw disable` | Disable the firewall |
| `sudo ufw status numbered` | List all rules with numbers |
| `sudo ufw allow <port>/<proto>` | Allow traffic on a port |
| `sudo ufw deny <port>/<proto>` | Block traffic on a port |
| `sudo ufw delete <rule_number>` | Remove a rule by number |
| `sudo ufw reset` | Reset all rules to default |

---

## 3. Windows – Windows Defender Firewall

---

### Step 1: Open Firewall – Control Panel View

Navigate to: `Control Panel → System and Security → Windows Defender Firewall`

**Screenshot – Windows Defender Firewall (Control Panel):**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/9.png">

> **Observed:** Firewall is **ON** for both Private and Public (Guest) profiles. Active network: `Sachin_5G 2` (Public). Incoming connections are blocked for apps not on the allowed list.

---

### Step 2: Open Advanced Security – View Current Rules

Run `wf.msc` or click **Advanced settings** in the Control Panel.

**Screenshot – Windows Defender Firewall with Advanced Security:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/10.png">

> **Observed:** All three profiles (Domain, Private, Public) show:
> - Firewall: **ON**
> - Inbound connections not matching a rule: **Blocked**
> - Outbound connections not matching a rule: **Allowed**

---

### Step 3: Create New Inbound Rule to Block Port 23 (Telnet) – GUI

1. Click **Inbound Rules** → **New Rule**
2. Select rule type

**Screenshot – New Inbound Rule Wizard: Rule Type selection:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/11.png">

> Select **Port** → Next → Choose **TCP**, enter port **23** → Select **Block the connection** → Apply to all profiles → Name the rule.

3. Name the rule and finish

**Screenshot – Naming the rule "Block telnet port 23":**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/12.png">

> **Observed:** Rule named `Block telnet port 23` is ready. All wizard steps (Rule Type, Protocol and Ports, Action, Profile) are marked complete (green dots).

---

### Step 4: Test the Block Rule – PowerShell

```powershell
# Test TCP connection to port 23
Test-NetConnection -Port 23
```

**Screenshot – PowerShell: Waiting for connection test (in progress):**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/13.png">

**Screenshot – PowerShell: Test result confirming port 23 is blocked:**

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/9417e75c0975254a22506431c78cc65edc6fb0e7/Task%204/Images/14.png">

> **Observed:** `TcpTestSucceeded : False` — TCP connection to port 23 failed, confirming the firewall rule is working correctly. Ping (`PingSucceeded: True`) still works since ICMP is not blocked.

---

### Step 5: Remove the Test Block Rule

```powershell
# PowerShell
Remove-NetFirewallRule -DisplayName "Block telnet port 23"
```

Or via GUI: **Inbound Rules** → right-click `Block telnet port 23` → **Delete**.

---

### Windows Firewall Quick Reference

| Method | Command / Action |
|---|---|
| Open advanced GUI | `wf.msc` |
| List inbound rules (PS) | `Get-NetFirewallRule -Direction Inbound` |
| Block port (PS) | `New-NetFirewallRule -LocalPort 23 -Action Block` |
| Test connection (PS) | `Test-NetConnection -Port 23` |
| Delete rule (PS) | `Remove-NetFirewallRule -DisplayName "..."` |
| Delete rule (CMD) | `netsh advfirewall firewall delete rule name="..."` |

---

## 4. Summary: How a Firewall Filters Traffic

A firewall filters traffic by **inspecting network packets** and comparing them against a set of defined rules:

```
Incoming Packet
      │
      ▼
┌─────────────────────────┐
│   Firewall Rule Engine  │
│                         │
│  1. Check Source IP     │
│  2. Check Destination   │
│     Port & Protocol     │
│  3. Match Against Rules │
│     (top-down order)    │
└──────────┬──────────────┘
           │
    ┌──────┴──────┐
    │             │
  ALLOW         DENY
    │             │
    ▼             ▼
Packet passes  Packet dropped
to application (connection refused
                or silently dropped)
```

### Key Filtering Mechanisms

| Mechanism | Description |
|---|---|
| **Packet Filtering** | Inspects individual packets based on IP, port, and protocol |
| **Stateful Inspection** | Tracks active connections; allows return traffic automatically |
| **Rule Priority** | Rules evaluated top-to-bottom; first matching rule wins |
| **Default Policy** | UFW: deny incoming, allow outgoing. Windows: same defaults |
| **Direction** | Rules apply to **inbound** or **outbound** traffic separately |
| **Logging** | Firewalls can log allowed/denied packets for audit purposes |

### Why This Matters

- **Port 23 (Telnet)** — Blocked because it transmits data in plain text; easily interceptable
- **Port 22 (SSH)** — Allowed (with subnet restriction) for encrypted remote access
- **Port 80/443 (HTTP/HTTPS)** — Allowed for web traffic
- **Principle of Least Privilege** — Only open ports that are absolutely necessary
- **Defense in Depth** — Firewalls are one layer in a multi-layered security strategy

---

> **Conclusion:** A properly configured firewall is a foundational security control. By explicitly defining what traffic is permitted and denying everything else, firewalls significantly reduce the attack surface of a system or network.

---
