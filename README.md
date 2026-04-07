# Lab 01 — Network Scanning & Enumeration

## Objective
Perform network reconnaissance against a deliberately vulnerable target using Nmap to identify open ports, running services, version information, and known CVEs — replicating the initial discovery phase of a real security assessment.

## Skills Demonstrated
- Network scanning and host discovery
- Service and version enumeration
- Vulnerability identification using Nmap NSE scripts
- CVE research and risk classification
- Documentation and report writing

## Tools Used
- **Kali Linux** — attack machine
- **Nmap 7.98** — network scanner and vulnerability detection
- **Metasploitable 2** — intentionally vulnerable target VM
- **VirtualBox** — virtualization and isolated lab network

## Environment
- Attacker: Kali Linux 2026.1 (VirtualBox VM)
- Target: Metasploitable 2 — IP 10.0.2.3
- Network: Isolated NAT Network (no internet exposure)

## Methodology

### Phase 1 — Service Detection Scan
```bash
nmap -sV 10.0.2.3 -oN lab_scan_results.txt
```
Identified 20+ open ports and running services including Apache, MySQL, PostgreSQL, VNC, FTP, and SSH.

### Phase 2 — Vulnerability Script Scan
```bash
nmap --script vuln 10.0.2.3 -oN lab1_vuln_scan.txt
```
Ran Nmap NSE vulnerability scripts against all open ports to identify known CVEs and exploitable conditions.

## Key Findings

| Severity | Port | Service | Vulnerability | CVE |
|----------|------|---------|--------------|-----|
| Critical | 21 | vsFTPd 2.3.4 | Backdoor — confirmed root shell | CVE-2011-2523 |
| Critical | 6667 | UnrealIRCd | Trojaned version with backdoor | — |
| Critical | 1524 | Bindshell | Open root shell, no auth required | — |
| High | 1099 | Java RMI | Remote code execution | — |
| High | 25 | SMTP | SSL/TLS Logjam downgrade attack | CVE-2015-4000 |
| High | 5432 | PostgreSQL | CCS Injection, session hijacking | CVE-2014-0224 |
| High | 80 | HTTP | SQL injection on multiple endpoints | — |
| Medium | 80 | HTTP | Slowloris DoS attack | CVE-2007-6750 |
| Medium | 80 | HTTP | CSRF vulnerabilities on multiple forms | — |
| Medium | 8180 | Tomcat | Admin panel exposed, no auth | — |

## Summary
The target presented a critically insecure attack surface with multiple remotely exploitable backdoors, unpatched CVEs dating from 2007–2015, exposed database services, and vulnerable web applications. In a real environment this would constitute a P1 incident requiring immediate isolation and remediation.

## Screenshots
*Screenshots of scan output stored in /screenshots folder*

## References
- [CVE-2011-2523](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523)
- [CVE-2015-4000 Logjam](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000)
- [CVE-2014-0224 CCS Injection](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224)
- [CVE-2007-6750 Slowloris](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750)
- [Nmap NSE Documentation](https://nmap.org/nsedoc/)
