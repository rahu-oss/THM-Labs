Breakme — Penetration Test & Vulnerability Assessment

A full black-box-to-root penetration test against Breakme, a vulnerable Linux/WordPress lab hosted on TryHackMe. This repo documents the complete attack chain, methodology, and a professional client-style report.

⚠️ Disclaimer: This assessment was performed against a legal, purpose-built training environment (TryHackMe) that I was authorized to test. No production systems, real organizations, or third parties were involved. Shared for educational and portfolio purposes only — do not use these techniques against systems you don't own or have explicit written permission to test.

📄 Full Report

The complete professional report — executive summary, findings with CVSS-style risk ratings, evidence, and remediation guidance — is available here:

Penetration_Test_Report.pdf
Penetration_Test_Report.docx
🎯 Overview
	
Target	Breakme (TryHackMe) — Debian Linux, Apache, WordPress
Testing style	Black-box external → authenticated internal → privilege escalation
Result	Full compromise — unauthenticated access to root
Overall risk rating	Critical
🔗 Attack Chain Summary
Recon — Nmap + Gobuster identify SSH, Apache, and a WordPress install
Enumeration — WPScan fingerprints an outdated WordPress core and a vulnerable third-party plugin (WP Data Access); discloses usernames admin and bob
Privilege escalation (web) — Broken access control / mass assignment in profile.php lets bob self-promote to Administrator
RCE — Admin access abused via the theme/plugin editor to get a reverse shell as www-data
Pivoting — A loopback-only service (127.0.0.1:9999, owned by john) is exposed via a Chisel tunnel
Race condition — A custom privileged file-reader is exploited via a TOCTOU / symlink-swap race to leak youcef's SSH private key
Credential cracking — The key's passphrase is cracked offline in seconds with ssh2john + John the Ripper against rockyou.txt
Privilege escalation (root) — An insecure NOPASSWD sudo rule for a Python "jail" script is broken out of via __builtins__ introspection, yielding a root shell
🛠️ Tools Used

nmap · gobuster · wpscan · Burp Suite (manual requests) · netcat · chisel · ssh2john / john (John the Ripper) · custom Bash race-condition harness · Python

🧠 Skills Demonstrated
Network & web reconnaissance, service/version fingerprinting
CMS and plugin vulnerability research (CVE mapping)
Web application logic flaws — broken access control / mass assignment
Post-exploitation enumeration and internal pivoting via tunneling
Race condition (TOCTOU) exploitation
Offline credential/passphrase cracking
Linux privilege escalation (sudo misconfiguration, sandbox escape)
Professional report writing — risk rating, evidence documentation, remediation guidance
