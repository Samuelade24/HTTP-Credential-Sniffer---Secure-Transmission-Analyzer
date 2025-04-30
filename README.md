**Project Description**

HTTP Credential Sniffer is a PowerShell-based security tool that monitors and analyzes network traffic to detect unencrypted credential transmission. The tool helps identify security vulnerabilities where sensitive data like usernames and passwords are sent in cleartext over HTTP connections.

**Key Features:**

Real-time network traffic monitoring
Automatic detection of credential transmission
HTTP/HTTPS traffic analysis
Sensitive data pattern matching
Professional vulnerability reporting
Non-intrusive passive scanning

**Installation:**

# Install required modules
Install-Module -Name PcapDotNet -Force
Install-Module -Name SecurityProtocol -Force

# Clone repository
git clone https://github.com/yourusername/http-credential-sniffer.git
cd http-credential-sniffer

# Run tool (Admin privileges required)
.\CredentialSniffer.ps1

**Usage Examples**
**Basic monitoring:**
.\CredentialSniffer.ps1 -Interface "Wi-Fi"

**Targeted website analysis:**
.\CredentialSniffer.ps1 -TargetUrl "http://testphp.vulnweb.com" -OutputReport

**Comprehensive scan:**
.\CredentialSniffer.ps1 -AllInterfaces -CaptureDuration 300 -Verbose

**Technical Implementation:**
function Start-CredentialCapture {
    param(
        [string]$Interface,
        [int]$Duration = 60,
        [switch]$OutputReport
    )

    # Initialize capture session
    $capture = New-PacketCapture -Interface $Interface
    
    # Start monitoring for credentials
    $results = Watch-Traffic -Capture $capture -Duration $Duration |
        Where-Object { $_.Protocol -eq "HTTP" -and $_.ContainsCredentials }

    # Generate report if requested
    if($OutputReport) {
        New-VulnerabilityReport -Findings $results -Format HTML
    }

    return $results
}

**Sample of my Report:**
# INSECURE CREDENTIAL TRANSMISSION REPORT

## Target: http://testphp.vulnweb.com/login.php
## Date: $(Get-Date -Format "yyyy-MM-dd")

### Critical Findings:
- [x] Credentials transmitted in cleartext over HTTP
- [x] No SSL/TLS encryption detected
- [x] Session cookies exposed in plaintext

### Captured Data:
```http
POST /login.php HTTP/1.1
Host: testphp.vulnweb.com
Content-Type: application/x-www-form-urlencoded

uname=admin&pass=password1234

**Risk Assessment:**
Vulnerability	Severity
Cleartext credentials	Critical
Missing encryption	High
Cookie exposure	Medium

**Recommendations:**
Implement HTTPS across entire application
Enable HSTS to prevent HTTP access
Add secure and HttpOnly flags to cookies
Consider implementing multi-factor authentication


## Security Considerations
- Requires administrative privileges for packet capture
- Only captures traffic on authorized networks
- Includes ethical use disclaimer
- Automatically filters out sensitive data in reports

## Roadmap
- [ ] HTTPS upgrade checker
- [ ] Automated certificate validation
- [ ] Integration with Wireshark/tshark
- [ ] Cloud deployment version

## License
MIT License - Free for non-commercial use with attribution

## Contribution Guidelines
We welcome contributions for:
- New detection patterns
- Improved reporting formats
- Additional protocol support
- Performance optimizations

```diff
+ Ethical Note: Always obtain proper authorization before monitoring network traffic
! Warning: Unauthorized packet capture may violate privacy laws
# Best Practice: Use only on networks you own or have permission to test

