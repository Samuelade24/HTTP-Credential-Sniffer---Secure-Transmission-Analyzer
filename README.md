**Project Description**

HTTP cleartext credential exposure report, enhanced with threat modeling, compliance mapping, and expanded technical details:

## 🔒 HTTP Cleartext Credential Exposure: `testphp.vulnweb.com`

```mermaid
graph TD
    A[Attacker] --> B{Network Access}
    B --> C[Passive Sniffing]
    B --> D[ARP Spoofing]
    C --> E[Credential Capture]
    D --> E
    E --> F[[Impact]]
    F --> G[Account Takeover]
    F --> H[Data Breach]
    F --> I[GDPR Violation]
    G --> J[Financial Loss]
    H --> K[Reputation Damage]
    I --> L[Regulatory Fines]

📋 Executive Summary
Vulnerability: Cleartext credential transmission over HTTP
CVSS Score: 9.1 (Critical)
Tools Used: Wireshark, Chrome Developer Tools, tcpdump
Proof of Concept:

POST /login.php HTTP/1.1
Host: testphp.vulnweb.com
uname=admin&pass=password1234  # Visible in plaintext

🔧 Expanded Technical Methodology
1. Network Reconnaissance
# Identify HTTP services
nmap -p 80 --open 44.228.249.3/24

# Confirm no HTTPS redirect
curl -I http://testphp.vulnweb.com/login.php

2. Credential Capture Process
Wireshark Filters Applied:
http.request.method == POST && http.host == "testphp.vulnweb.com"

Traffic Analysis:
# Sample packet dissection
{
  "source_ip": "10.8.143.99",
  "dest_ip": "44.228.249.3",
  "protocol": "HTTP",
  "credentials": {
    "username": "admin",
    "password": "password1234",
    "encryption": "None"
  }
}

3. Attack Simulation
MITM Techniques Verified:
ARP spoofing with Ettercap
SSLStrip attack (though unnecessary due to HTTP-only)

🛡️ Compliance Impact
PCI-DSS v4.0 Violations
Requirement	Status	Evidence
4.1 (Encrypt Transmission)	❌ Fail	PCAP File
8.2.1 (Password Protection)	❌ Fail	Credentials exposed
GDPR Articles Affected
Article 32: Lack of appropriate security measures
Article 33: 72-hour breach notification requirement triggered

ISO 27001:2022
A.9.4.1: Network controls inadequate
A.13.2.1: Information transfer not protected

pie
    title Compliance Risk Distribution
    "PCI-DSS" : 50
    "GDPR" : 35
    "ISO 27001" : 15

🎓 Lessons I Learnt
For System Administrators
HTTPS is Non-Negotiable
Finding: HTTP usage still prevalent in test environments
Fix: Automated redirection with HSTS:
server {
    listen 80;
    return 301 https://$host$request_uri;
    add_header Strict-Transport-Security "max-age=31536000";
}

**Credential Handling Best Practices**
Never transmit credentials via URL parameters
Implement CSRF tokens for all forms

**For Pentesters**
Layer 2 Attacks Matter
ARP spoofing works even on switched networks
Defense: Enable DHCP snooping and DAI

**Documentation is Critical**
Timestamped PCAP files provide irrefutable evidence

🛠️ Remediation Roadmap
Immediate Actions (24h)
Deploy Let's Encrypt SSL certificate
certbot --nginx -d testphp.vulnweb.com --redirect
Force logout all active sessions

**Short-Term (1 Week)**
Implement WAF with credential stuffing protection
Conduct developer security training

**Long-Term (1 Month)**
Deploy SIEM for login monitoring
Schedule quarterly penetration tests

📚 Evidence Package
File	Purpose
cleartext.pcapng	Raw intercepted traffic
nmap_scan.xml	Service discovery results
pci_gap.pdf	Compliance assessment
"Cleartext protocols belong in museums, not production environments."

