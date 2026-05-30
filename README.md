# Task 1 - Scan Your Local Network for Open Ports

## Objective
Learn to discover open ports on devices in your local network to understand network exposure.

## Tools Used
- Nmap 7.95 (Kali Linux)

## Steps Performed
1. Found local IP range using `ip a` → 192.168.142.0/24
2. Ran TCP SYN scan: `sudo nmap -sS 192.168.142.0/24`
3. Noted down IP addresses and open ports
4. Identified security risks from open ports
5. Saved results as text and HTML file

## Results
- 3 hosts found: VMware Gateway (.1), DHCP Server (.254), Kali (.129)
- No open TCP ports found — all filtered by VMware NAT firewall
- Firewall is blocking all incoming connections — good security posture

## Files
- `scan_results.txt` — Nmap TCP SYN scan output
- `scan_results.html` — HTML format report
