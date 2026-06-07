# 🦈 Network Traffic Capture & Protocol Analysis
### Wireshark Practical Lab — Task 5


## 📋 Objective

Capture live network packets using **Wireshark 4.6.6**, identify at least **3 distinct protocols**, analyse packet-level details using display filters, and document findings with annotated screenshots.

---

## 🗂️ Repository Structure

```
task-5-wireshark/
├── screenshots/
│   ├── wireshark_open.png     # Wireshark startup — interface selection
│   ├── tcp_record.png         # TCP filter — live capture & handshake details
│   ├── tls.png                # TLS filter — TLSv1.2 & QUIC application data
│   ├── DNS_Linkedin.png       # DNS filter — LinkedIn & CDN queries
│   ├── dns_record.png         # DNS filter — WhatsApp, Google CDN responses
│   ├── http.png               # HTTP filter — GET / POST / SUBSCRIBE traffic
│   └── (tcp_record_2.png)     # TCP filter — full conversation with ACK/FIN
├── network_capture.pcap       # Exported packet capture file
├── Wireshark_Lab_Report_Task5.docx
└── README.md
```

---

## 🛠️ Environment & Setup

| Item | Detail |
|------|--------|
| **Tool** | Wireshark 4.6.6 (v4.6.6-0-g3a22c3ef473d) |
| **OS** | Windows (with automatic updates enabled) |
| **Capture Interface** | **Wi-Fi** (active wireless adapter) |
| **Total Packets Captured** | **43,191** |
| **Capture Duration** | ~23+ seconds of active session |
| **Client IP** | `192.168.0.193` |
| **Gateway / Router** | `192.168.0.1` |
| **Protocols Identified** | DNS, HTTP, TCP, TLS v1.2, TLS v1.3, QUIC, UDP |

---

## ⚙️ Procedure

1. Opened Wireshark 4.6.6 — selected **Wi-Fi** as the capture interface
2. Started live capture (red record button)
3. Browsed websites (LinkedIn, WhatsApp, Google services) to generate real traffic
4. Stopped capture after ~60 seconds
5. Applied display filters one by one: `dns`, `http`, `tcp`, `tls`
6. Inspected packet details in the middle and hex panes
7. Exported capture as `network_capture.pcap`

---

## 📸 Step-by-Step Screenshots

### Step 1 — Wireshark Launch & Interface Selection

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/download.png">

> Wireshark 4.6.6 startup screen showing all available network interfaces. **Wi-Fi** was selected as the active interface for capture. Other visible interfaces include Loopback, Bluetooth, VMware adapters, and McAfee VPN. The conference banner confirms **SharkFest'26 US** — Nashville, July 18–23, 2026.

---

### Step 2 — TCP Filter (Live Capture)

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/tcp%20record.png">

> **Filter applied:** `tcp` | **Packets displayed:** 95 (100%) during live capture  
> Shows TCP segments between `192.168.0.193` (client) and `192.168.0.200` (local device) on **port 8009**, alongside TLSv1.2 application data from `57.144.147.32`.  
> The detail pane reveals:
> - `Source Port: 15365` → `Destination Port: 8009`
> - `IPv4 Header` — TTL: 128, Protocol: TCP (6)
> - IP: `192.168.0.193` → `192.168.0.200`
> - Flags: ACK active, no SYN (mid-stream packet)

---

### Step 3 — TLS Filter

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/tls.png">

> **Filter applied:** `tls` | **Total:** 3,480 packets | **Displayed:** 712 (20.5%) | **Marked:** 1  
> Captures encrypted **TLSv1.2 Application Data** flowing between the client and multiple servers. Key observations:
> - `TLSv1.2` — `192.168.0.193` ↔ `57.144.147.32` (Fastly CDN) — repeated 164-byte application data frames
> - `TLSv1.2` — Packet 63 — **14,090-byte** large data transfer (bulk content delivery)
> - **QUIC** — Packets 88 & 93 — `192.168.0.193` → `167.82.59.42` — Initial handshake with CRYPTO + PADDING
> - `TLSv1.3` — Packet 95 — **Client Hello** (SNI: `ohttp-relay-safebrowsing-chrome.google.fastly.edge.com`)
> - `TLSv1.3` — Packet 99 — **Server Hello + Change Cipher Spec** from `151.101.105.91`
>
> **Detail pane (Packet 11):**
> ```
> Transport Layer Security
>   TLSv1.2 Record Layer: Application Data
>     Content Type: Application Data (23)
>     Version: TLS 1.2 (0x0303)      ← highlighted
>     Length: 105
>     Encrypted Application Data: 06b92331aa62a2c9976b89f9aa51ec646bc868cffd356730ec...
> ```

---

### Step 4 — DNS Filter (LinkedIn Session)

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/DNS%20Linkedin.png">

> **Filter applied:** `dns` | **Total:** 3,480 packets | **Displayed:** 251 (7.2%)  
> Captured during active LinkedIn browsing. Multiple domains resolved simultaneously:

| Packet | Query / Response | Domain | Result |
|--------|-----------------|--------|--------|
| 72 | Query HTTPS | `in.linkedin.com` | — |
| 73 | Query A | `in.linkedin.com` | — |
| 75 | Query HTTPS | `ohttp-relay-safebrowsing-chrome.google.fastly.edge.com` | — |
| 77 | Query HTTPS | `static.licdn.com` | — |
| 79 | **Response** | `in.linkedin.com` | `172.64.146.215` |
| 80 | Query A | `wpad.www.tendawifi.com` | — |
| 83 | **Response** | `ohttp-relay-...fastly.edge.com` | `A 151...` |
| 89 | **Response (SOA)** | `wpad.www.tendawifi.com` | No such name |
| 97 | **Response** | `in.linkedin.com` | CNAME → `cf-afd.cc...` |

> **Detail pane (Packet 72):**
> ```
> Domain Name System (query)
>   Transaction ID: 0x2c87
>   Flags: 0x0100  Standard query
>   Questions: 1
>   Answer RRs: 0
>   Queries:
>     in.linkedin.com: type HTTPS, class IN
>   [Response In: 97]
> ```

---

### Step 5 — DNS Filter (Google & WhatsApp CDN)

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/dns%20record.png">

> **Filter applied:** `dns` | **Total:** 43,191 packets | **Displayed:** 891 (2.1%)  
> Broader session view showing DNS resolution for multiple Google and Meta services:

| Packet | Direction | Domain | Resolved To |
|--------|-----------|--------|-------------|
| 173–174 | Query | `www.whatsapp.com` | — |
| 175–176 | **Response** | `www.whatsapp.com` | CNAME `mmx-ds.cdn.whatsapp.net` → `57.144.147.x` |
| 296 | Query A | `edgedl.me.gvt1.com` | — |
| **298** | **Response** | `edgedl.me.gvt1.com` | **`34.104.35.123`** ✅ |
| 647 | Query HTTPS | `beacons.gcp.gvt2.com` | — |
| 651 | **Response** | `beacons.gcp.gvt2.com` | CNAME `beacons-handoff.gcp.gvt2.com` → `A 14...` |
| 649–658 | Query/Response | `encrypted-tbn0.gstatic.com` | SOA `ns1.google.com` |

> **Detail pane (Packet 298) — Frame metadata:**
> ```
> Arrival Time  : Jun 7, 2026  18:13:47.565219800  India Standard Time
> Interface     : Wi-Fi  (\Device\NPF_{BF575E00-E502-4382-B92F-9ABC3E9DC0B4})
> Frame Length  : 94 bytes (752 bits)
> DNS Response  : edgedl.me.gvt1.com  A  34.104.35.123
> ```

---

### Step 6 — HTTP Filter

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/http.png">

> **Filter applied:** `http` | **Total:** 43,191 packets | **Displayed:** 336 (0.8%)  
> Unencrypted HTTP traffic showing real application-layer communication:


> **Interesting finding:** The client is actively querying the **router's UPnP interface** (`192.168.0.1`) via plain HTTP — including `SUBSCRIBE` and `POST` methods for WAN interface configuration. This is a UPnP/SOAP protocol on top of HTTP.
>
> **Detail pane (Packet 302):**
> ```
> TCP  Src Port: 27500 → Dst Port: 80
> Stream index: 25  |  Conversation: Complete WITH_DATA
> Flags: SYN ✓  SYN-ACK ✓  Data ✓  FIN ✓  ACK ✓
> ```

---

### Step 7 — TCP Filter (Full Session View)

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/a719f68f5ddfb65cc3cd384cd7219606ec9812d7/Task%205/Images/tcp%20record.png">

> **Filter applied:** `tcp` | **Total:** 43,191 | **Displayed:** 17,234 (39.9%)  
> Broad TCP view revealing the full conversation lifecycle alongside TLS:

| Packet | Time (s) | Flow | Info |
|--------|----------|------|------|
| 234 | 20.025 | → `142.250.146.101:443` | TLSv1.3 Change Cipher Spec |
| 240 | 20.067 | → `142.250.146.101:443` | TLSv1.3 Application Data (1074 B) |
| 244 | 20.110 | → `142.250.146.101:443` | TCP ACK Seq=2395 |
| 286 | 20.503 | → `216.239.32.223:443` | TCP ACK (Seq=1, Ack=1) |
| **287** | 20.537 | ← `216.239.32.223` | **TCP ACK SLE=1 SRE=2** (Selective ACK) |
| 299 | 20.684 | → `34.104.35.123:80` | **TCP SYN** (new HTTP connection) |
| 300 | 20.688 | ← `34.104.35.123:80` | **TCP SYN-ACK** MSS=1400, WS=256 |
| 301 | 20.688 | → `34.104.35.123:80` | **TCP ACK** (handshake complete ✅) |
| 302 | 20.688 | → `34.104.35.123:80` | HTTP HEAD request |
| 304 | 20.699 | ← `34.104.35.123:80` | HTTP 200 OK |
| 305 | 20.751 | → `192.168.0.1:1980` | TCP SYN (new connection to router) |

> **Detail pane (Packet 287):**
> ```
> Source Port: 443 → Destination Port: 26114
> Stream index: 24  |  Conversation: Incomplete (28)
> Flags: FIN ✓  Data ✓  ACK ✓
> Sequence Number: 1  (relative)
> Acknowledgment Number: 2
> ```

---

## 🔍 Protocols Identified

### 1. 🟦 DNS — Domain Name System
- **Layer:** Application | **Transport:** UDP/53
- **Displayed:** 891 packets (2.1% of capture)
- **Domains resolved:** `in.linkedin.com` → `172.64.146.215`, `www.whatsapp.com` → `57.144.147.x`, `edgedl.me.gvt1.com` → `34.104.35.123`
- **Notable:** `wpad.www.tendawifi.com` returned **SOA / No such name** (WPAD proxy auto-detection failure)
- **Filter:** `dns`

### 2. 🟩 HTTP — HyperText Transfer Protocol
- **Layer:** Application | **Transport:** TCP/80
- **Displayed:** 336 packets (0.8% of capture)
- **Methods seen:** `HEAD`, `GET`, `POST`, `SUBSCRIBE`, `NOTIFY`
- **Notable:** Router UPnP queries (`192.168.0.1`) — `WANCommonInterfaceConfig` via plain HTTP (unencrypted)
- **Filter:** `http`

### 3. 🟨 TCP — Transmission Control Protocol
- **Layer:** Transport
- **Displayed:** 17,234 packets (39.9% of capture)
- **Observed:** Full 3-way handshake (SYN → SYN-ACK → ACK), Selective ACK (SACK), FIN teardown, MSS negotiation (1400/1460 bytes)
- **Filter:** `tcp`

### 4. 🔒 TLS — Transport Layer Security (v1.2 & v1.3)
- **Layer:** Application (over TCP/443)
- **Displayed:** 712 packets (20.5% of 3,480)
- **Observed:** TLSv1.2 Application Data (encrypted), TLSv1.3 Client Hello with SNI, Server Hello + Change Cipher Spec
- **Filter:** `tls`

### 5. ⚡ QUIC — Quick UDP Internet Connections
- **Layer:** Transport (UDP-based)
- **Observed:** Initial QUIC handshake to `167.82.59.42` with CRYPTO + PADDING frames (PKN: 1 & 2)
- **Filter:** `quic`

### 6. 🌐 UDP — User Datagram Protocol
- **Layer:** Transport
- **Role:** DNS transport (port 53), QUIC transport
- **Filter:** `udp`

### 7. 🔵 IPv4 — Internet Protocol v4
- **Layer:** Network
- **Observed:** TTL values of 128 (Windows client), various server TTLs; no fragmentation; DSCP CS0 / Not-ECT
- **Filter:** `ip`

---

## 🔎 Wireshark Display Filters Used

```wireshark
dns          # DNS queries and responses             → 891 packets displayed
http         # Plaintext HTTP traffic                → 336 packets displayed
tcp          # All TCP segments                      → 17,234 packets displayed
tls          # TLS encrypted sessions                → 712 packets displayed
quic         # QUIC protocol (UDP-based HTTP/3)      → visible in tls/quic filter
udp          # All UDP datagrams

# Specific filters used during analysis
ip.addr == 34.104.35.123     # Google CDN (edgedl.me.gvt1.com)
ip.addr == 192.168.0.1       # Router / Gateway
tcp.port == 80               # HTTP port
tcp.port == 443              # HTTPS / TLS port
tcp.flags.syn == 1           # SYN packets only (new connections)
```

---

## 📊 Capture Statistics

| Metric | Value |
|--------|-------|
| Total packets captured | **43,191** |
| Client IP | `192.168.0.193` |
| Gateway | `192.168.0.1` |
| DNS server | `192.168.0.1` (router forwards DNS) |
| External servers contacted | `57.144.147.32` (Fastly), `34.104.35.123` (Google), `142.250.146.101` (Google), `167.82.59.42` (Cloudflare), `151.101.105.91` (Fastly) |
| Most common protocol | TCP (39.9% of packets) |
| Encrypted traffic | ~20%+ (TLS) — payload not visible |
| Plaintext traffic | HTTP to router (UPnP), DNS queries |
| QUIC (HTTP/3) | Present — modern browser fallback |

---

## 🔐 Security Observations

> Real findings from the actual capture — not theoretical examples.

| Finding | Risk | Recommendation |
|---------|------|----------------|
| **HTTP to router `192.168.0.1`** — UPnP `SUBSCRIBE`/`POST` unencrypted | Medium — LAN-local but plaintext config exposed | Disable UPnP if unused; use router HTTPS admin panel |
| **DNS queries plaintext** — all domains visible to network observer | Medium — reveals full browsing intent | Use **DNS over HTTPS (DoH)** or **DNS over TLS (DoT)** |
| **WPAD probe (`wpad.www.tendawifi.com`)** — returned SOA/No such name | Low — browser sent proxy auto-discovery | Disable WPAD/PAC in browser/OS network settings |
| **TLSv1.2 still in use** — legacy version alongside TLSv1.3 | Low-Medium — TLS 1.2 has known weaknesses | Prefer TLS 1.3; deprecate 1.2 where possible |
| **QUIC observed** — UDP-based, harder to inspect/block | Informational | Monitor QUIC traffic; some firewalls block UDP/443 |

---

## 🧠 Key Takeaways

- A single browsing session generates traffic across **7+ protocols simultaneously**
- **DNS always fires first** — every connection starts with name resolution
- **TCP 3-way handshake** (SYN → SYN-ACK → ACK) is visible and verifiable in Wireshark
- **TLS encrypts everything above transport** — Wireshark shows metadata (IP, port, SNI) but not payload
- **QUIC is replacing TCP+TLS** for modern web — Google services already use it heavily
- **UPnP on the router uses plain HTTP** — a common overlooked local network exposure
- Wireshark's **display filters** (`dns`, `http`, `tcp`, `tls`) are essential for isolating signal from noise in a 43,000-packet capture

---
