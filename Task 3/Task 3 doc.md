# 🛡️ Vulnerability Scan Report
**Target:** `192.168.32.164` | **Tool:** OpenVAS Community Edition | **Date:** June 3, 2026

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4e119752fc38bf29f60dafabfb319b04c74310ab/Task%203/Images/target%20ip.png">

---

## 📌 Objective

Use free vulnerability scanning tools to identify common security vulnerabilities on a local machine, document the findings, and research mitigations for each issue found.

---

## 🛠️ Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| OpenVAS Community Edition | GVM 22.x | Vulnerability scanning |
| Greenbone Security Assistant | Web UI | Scan management & reporting |

---
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4e119752fc38bf29f60dafabfb319b04c74310ab/Task%203/Images/open%20vas.png">

## ⚙️ Scan Configuration

| Parameter | Value |
|-----------|-------|
| **Target IP** | `192.168.32.164` |
| **Scan Profile** | Full and Fast |
| **Ports Scanned** | All TCP/UDP |
| **Scan Duration** | ~45 minutes |
| **OS Detected** | Windows 10 / Windows Server 2019 |
| **Open Ports Found** | 22 (SSH), 1524 (Ingreslock), 5900 (VNC), 445 (SMB) |

---
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4e119752fc38bf29f60dafabfb319b04c74310ab/Task%203/Images/Add%20target%201.png">

## 📊 Results Summary

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/ef8e9a5399a44e1e5de48c64cb16cd32cc63697b/Task%203/Images/result.png">

| Severity | Count |
|----------|-------|
| 🔴 Critical | 14 |
| 🟠 High | 10 |
| 🟡 Medium | 40 |
| 🟢 Low / Info | 6 |
| **Total** | **163** |

---

## 🔍 Vulnerabilities Found

### 1. 🔴 Possible Backdoor — Ingreslock (Port 1524)

| Field | Details |
|-------|---------|
| **Severity** | Critical (CVSS 9.8) |
| **CVE** | CVE-1999-0526 |
| **Port** | TCP 1524 |
| **Plugin** | OID: 1.3.6.1.4.1.25623.1.0.10028 |

**Description:**
The Ingreslock service running on TCP port 1524 is a well-known UNIX database backdoor. Its presence strongly indicates the machine has been compromised. It provides unauthenticated root-level shell access — any attacker able to reach this port gains full control of the system with zero credentials.

**Scanner Output:**
```
Port 1524/tcp is open
Service: ingreslock
Result: The host is running ingreslock service.
This is a known backdoor. Immediate remediation required.
```

**Mitigation Steps:**
- [ ] Kill the process on port 1524: `netstat -ano` → find PID → `taskkill /PID <pid> /F`
- [ ] Block port 1524 at the firewall (inbound + outbound)
- [ ] Run a full malware scan (Malwarebytes, Windows Defender Offline)
- [ ] Audit recently modified executables and scheduled tasks
- [ ] Consider a clean OS reinstall if full compromise cannot be ruled out

---

### 2. 🟠 Passwordless Login — SSH (Port 22)

| Field | Details |
|-------|---------|
| **Severity** | High (CVSS 8.1) |
| **CVE** | CVE-2018-10933 / CWE-521 |
| **Port** | TCP 22 |
| **Plugin** | OID: 1.3.6.1.4.1.25623.1.0.900239 |

**Description:**
One or more SSH accounts on this host allow login with an empty password. This misconfiguration lets any attacker on the network gain direct shell access without any credentials.

**Scanner Output:**
```
NVT: SSH Passwordless Login
Port: 22/tcp
It was possible to login with no password via SSH.
Account: 'root' allows empty password login.
```

**Mitigation Steps:**
- [ ] Set strong passwords for all accounts: `passwd <username>`
- [ ] Edit `/etc/ssh/sshd_config` — set `PermitEmptyPasswords no`
- [ ] Disable direct root login: `PermitRootLogin no`
- [ ] Enforce key-based authentication: `PasswordAuthentication no`
- [ ] Review `/etc/passwd` and `/etc/shadow` for all empty-password accounts
- [ ] Restart SSH service: `systemctl restart sshd`

---

### 3. 🟠 VNC Brute-Force Login (Port 5900)

| Field | Details |
|-------|---------|
| **Severity** | High (CVSS 7.5) |
| **CVE** | CVE-2006-2369 |
| **Port** | TCP 5900 |
| **Plugin** | OID: 1.3.6.1.4.1.25623.1.0.10342 |

**Description:**
VNC (Virtual Network Computing) is running on port 5900 with a weak default password. OpenVAS successfully brute-forced the login, gaining full remote graphical desktop access. An attacker with VNC access can control the desktop, steal data, install malware, or pivot to other systems.

**Scanner Output:**
```
NVT: VNC Brute Force Login
Port: 5900/tcp
Successful VNC login with password: 'password'
VNC protocol version: RFB 003.008
Authentication type: VNC Authentication (Type 2)
```

**Mitigation Steps:**
- [ ] Change VNC password immediately (min 16 characters, mixed complexity)
- [ ] Restrict VNC access by IP using firewall rules
- [ ] Tunnel VNC over SSH: `ssh -L 5900:localhost:5900 user@host`
- [ ] Enable connection lockout after 5 failed attempts
- [ ] Disable VNC if not needed — use RDP with Network Level Authentication instead
- [ ] Consider replacing VNC with a solution supporting MFA

---

## 📋 Remediation Priority Matrix

| # | Vulnerability | Severity | Priority | Action |
|---|---------------|----------|----------|--------|
| 1 | Ingreslock Backdoor | 🔴 Critical | P1 — Immediate | Isolate machine, kill process, full forensic scan |
| 2 | Passwordless SSH Login | 🟠 High | P1 — Immediate | Set passwords, harden `sshd_config` |
| 3 | VNC Brute-Force | 🟠 High | P2 — Within 24hrs | Change password, restrict by IP, SSH tunnel |

---

## 🧠 Key Learnings

- **Default/weak credentials** are one of the most common and easiest-to-exploit vulnerabilities. Always change defaults before deploying any service.
- **Unnecessary services** (VNC, SSH) should be disabled if not actively needed. Every open port is a potential entry point.
- **Backdoors like Ingreslock** may indicate prior compromise — a scan alone is not sufficient if a backdoor is found. Full incident response procedures are needed.
- **OpenVAS** is a powerful free tool that can identify serious real-world vulnerabilities on a local machine in under an hour.

---

## 📸 Screenshots
<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/ef8e9a5399a44e1e5de48c64cb16cd32cc63697b/Task%203/Images/vul%201.png">

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/ef8e9a5399a44e1e5de48c64cb16cd32cc63697b/Task%203/Images/vul%202.png">


---

## ✅ Conclusion

>  **Overall Risk Rating: CRITICAL**

The scan of `192.168.32.165` identified **3 vulnerabilities** — 1 critical and 2 high severity. The presence of the Ingreslock backdoor is particularly alarming and should be treated as an active security incident. The machine should be **isolated from the network** until all findings are fully remediated and a clean rescan confirms no remaining issues.

---
