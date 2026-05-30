# Task 1 - Network Port Scanning with Nmap

## Objective
Discover open ports on local network devices to understand network exposure.

## Environment
- OS: Kali Linux (VMware NAT Network)
- Tool: Nmap 7.95
- Network Range: 192.168.142.0/24

## Scans Performed

### 1. TCP SYN Scan (Network-wide)

sudo nmap -sS 192.168.142.0/24

**Result:** 3 hosts discovered — VMware Gateway (.1), DHCP server (.254), Kali (.129)
No open TCP ports found on network hosts (all filtered by VMware NAT firewall).

### 2. Service Version + Localhost Scan

sudo nmap -sS -sV -O -p- 127.0.0.1

**Result:**
- Port 35551/tcp OPEN — Golang net/http server (internal application)
- OS Detected: Linux 5.0 - 6.2

### 3. Aggressive Scan

sudo nmap -A 192.168.142.0/24

**Result:** Traceroute confirmed 1-hop distance to gateway. All ports filtered.

### 4. UDP Scan (Top 20 ports)

sudo nmap -sU --top-ports 20 192.168.142.0/24

**Result:** VMware NAT hosts show open|filtered UDP ports including:
- 53/udp (DNS), 67/udp (DHCP), 123/udp (NTP)

## Hosts Discovered

| IP Address | Role | MAC | TCP Ports | UDP Ports |
|------------|------|-----|-----------|-----------|
| 192.168.142.1 | VMware NAT Gateway | 00:50:56:C0:00:01 | All filtered | DNS, DHCP, NTP (filtered) |
| 192.168.142.254 | VMware DHCP Server | 00:50:56:FC:36:B0 | All filtered | DNS, DHCP, NTP (filtered) |
| 192.168.142.129 | Kali Linux (local) | — | 35551 open (localhost only) | All closed |

## Security Risk Analysis

| Port/Service | Host | Risk Level | Description |
|-------------|------|-----------|-------------|
| 53/udp DNS | Gateway | Medium | DNS amplification attack possible if misconfigured |
| 67/udp DHCP | Gateway | Medium | Rogue DHCP server attack possible |
| 123/udp NTP | Gateway | Medium | NTP amplification DDoS risk |
| 35551/tcp HTTP | Localhost | Low | Internal Golang server, not exposed externally |
| All filtered ports | .1 & .254 | Low | VMware firewall blocking — good security posture |

## Key Observations
- VMware NAT network is well-firewalled — no exposed services
- Localhost has one internal HTTP service (port 35551) — not network-accessible
- `open|filtered` UDP state means firewall is dropping probe packets
- `closed` ports on Kali = reachable but no service listening (secure state)

## Key Concepts Learned
- **TCP SYN Scan**: Half-open scan — sends SYN, reads SYN-ACK response without completing handshake
- **Port States**: open, closed, filtered, open|filtered
- **Network Reconnaissance**: Mapping live hosts and services on a subnet
- **UDP vs TCP Scanning**: UDP is connectionless — harder to confirm open state
- **Filtered ports**: Indicate firewall presence — packets are silently dropped

## Files in this Repository
- `scan_results.txt` — TCP SYN + service version scan
- `localhost_scan.txt` — Full port scan of localhost with OS detection
- `aggressive_scan.txt` — Aggressive scan with traceroute
- `udp_scan.txt` — UDP top 20 ports scan
- `scan_results.xml` — XML format output
- `scan_results.html` — HTML format report
- `screenshots/` — Terminal screenshots
