Penetration Testing & Vulnerability Assessment Portfolio

A collection of full black-box-to-root penetration tests against vulnerable lab environments, each documented as a professional, client-style report. Built to demonstrate real-world offensive security methodology — recon through exploitation, privilege escalation, and (where applicable) threat-intelligence analysis.

⚠️ Disclaimer: All assessments in this repo were performed against legal, purpose-built training environments (TryHackMe) that I was authorized to test. No production systems, real organizations, or third parties were involved. Shared for educational and portfolio purposes only — do not use these techniques against systems you don't own or have explicit written permission to test.

📋 Engagements
Report	Target	Platform	Result
Breakme	WordPress / Linux host	TryHackMe — Breakme	Unauthenticated → root
Bricks Heist	WordPress / Bricks Builder	TryHackMe — TryHack3M: Bricks Heist	Unauthenticated RCE → discovery of pre-existing ransomware-affiliated compromise
🧱 Breakme

Full compromise of a Debian/WordPress lab host, chaining a vulnerable plugin, a broken-access-control bug, an internal service pivot, a TOCTOU race condition, and a sudo misconfiguration to go from zero credentials to root.

📄 Breakme/Penetration_Test_Report.pdf
📄 Breakme/Penetration_Test_Report.docx

Attack chain:

Recon — Nmap + Gobuster identify SSH, Apache, and a WordPress install
Enumeration — WPScan fingerprints an outdated WordPress core and a vulnerable third-party plugin (WP Data Access); discloses usernames admin and bob
Privilege escalation (web) — Broken access control / mass assignment in profile.php lets bob self-promote to Administrator
RCE — Admin access abused via the theme/plugin editor to get a reverse shell as www-data
Pivoting — A loopback-only service (127.0.0.1:9999, owned by john) is exposed via a Chisel tunnel
Race condition — A custom privileged file-reader is exploited via a TOCTOU / symlink-swap race to leak youcef's SSH private key
Credential cracking — The key's passphrase is cracked offline in seconds with ssh2john + John the Ripper against rockyou.txt
Privilege escalation (root) — An insecure NOPASSWD sudo rule for a Python "jail" script is broken out of via __builtins__ introspection, yielding a root shell

Tools: nmap · gobuster · wpscan · Burp Suite (manual requests) · netcat · chisel · ssh2john / john · custom Bash race-condition harness · Python

🧨 TryHack3M: Bricks Heist

Exploitation of a critical, publicly known unauthenticated RCE in the WordPress Bricks Builder theme — which, during post-exploitation enumeration, led to uncovering indicators that the lab host had already been compromised by a separate, simulated threat actor with ties to the LockBit ransomware group.

📄 Bricks-Heist/Bricks_Heist_Report.pdf

Attack chain:

Recon — Nmap identifies SSH, an unusual WebSockify listener on port 80, HTTPS WordPress on 443, and an exposed MySQL service on 3306
Enumeration — Gobuster + WPScan fingerprint WordPress 6.5 with the Bricks Builder theme (v1.9.5), disclose the administrator username, and find an exposed phpMyAdmin instance
Exploitation — Unauthenticated RCE via CVE-2024-25600 (Bricks Builder theme) yields command execution as apache
Sensitive data exposure — wp-config.php discloses plaintext MySQL root credentials and WordPress secret keys/salts
Indicator discovery — A systemd service masquerading as ubuntu.service and a root-owned hidden file in the web root point to a prior, unrelated compromise of the host
Artifact recovery — A hex → Base64 → Base64-obfuscated identifier decodes to a Bitcoin wallet address found elsewhere in plaintext
Threat-intel correlation — Open-source research links the wallet to a sanctioned individual ("Bassterlord") associated with the LockBit ransomware group

Tools: nmap · gobuster · wpscan · public CVE-2024-25600 PoC exploit · manual Linux enumeration · hex/Base64 decoding · OSINT / blockchain-attribution research

🧠 Skills Demonstrated Across This Portfolio
Network & web reconnaissance, service/version fingerprinting
CMS and plugin/theme vulnerability research (CVE mapping and exploitation)
Web application logic flaws — broken access control, mass assignment
Post-exploitation enumeration and internal pivoting via tunneling
Race condition (TOCTOU) exploitation
Offline credential/passphrase cracking
Linux privilege escalation (sudo misconfiguration, sandbox escape)
Indicator-of-compromise triage and basic digital forensics
Open-source threat-intelligence correlation and attribution
Professional report writing — risk rating, evidence documentation, remediation guidance
