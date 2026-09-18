# 🌐 Day 30: Network Security - Complete Guide

Welcome to Day 30 of the Computer Networks study journey. This guide covers the fundamentals of network security, common attacks, protection mechanisms, and the essential concepts that every networking student should know.

## 📝 Topics Covered

- What is network security?
- CIA triad: Confidentiality, Integrity, Availability
- Types of network threats and attacks
- Malware, phishing, DoS, DDoS, MITM, sniffing, spoofing, injection attacks
- Firewalls, IDS, IPS, VPN, NAT, DMZ
- Authentication, authorization, and accounting
- Encryption and cryptography basics
- Symmetric vs asymmetric encryption
- Hashing and digital signatures
- Wireless security: WPA, WPA2, WPA3
- Security policies and best practices
- Real-world network defense practices

## 🎯 Learning Objectives

By the end of this lesson, you should be able to:

- Define network security and explain why it is needed
- Explain the CIA triad and its importance in secure communication
- Identify common network threats and attack types
- Distinguish between active and passive attacks
- Understand firewall, IDS, IPS, VPN, and DMZ concepts
- Explain encryption, authentication, and access control
- Describe common security tools and strategies used in organizations

---

## 1. Introduction to Network Security

Network security is the practice of protecting the integrity, confidentiality, and availability of data and network resources from unauthorized access, misuse, disruption, or destruction.

It includes:

- Securing devices such as routers, switches, servers, and endpoints
- Protecting data in motion and at rest
- Preventing unauthorized access or modifications
- Detecting and responding to attacks
- Maintaining continuity of services

### Why Network Security is Important

- Networks carry sensitive information such as bank records, medical data, credentials, and business secrets
- Attackers can steal information, manipulate data, or disrupt services
- Security prevents unauthorized users from exploiting vulnerabilities
- Businesses depend on digital services and cannot afford downtime

### Network Security Goals

1. Confidentiality
   - Only authorized users can access information
2. Integrity
   - Data is not altered or corrupted during transmission or storage
3. Availability
   - Services and resources remain available when needed

---

## 2. CIA Triad

The security of any system is often described using the CIA triad.

| Principle | Meaning | Example |
|---|---|---|
| Confidentiality | Information is protected from unauthorized access | Encrypted email, password protection |
| Integrity | Data is accurate and trustworthy | Checksums, hashes, digital signatures |
| Availability | Information and systems are accessible when required | Redundant servers, load balancing |

### Example
A bank server must ensure:

- Confidentiality: account details are visible only to authorized users
- Integrity: transaction amounts cannot be tampered with
- Availability: online banking remains up during working hours

---

## 3. Types of Security Threats

A threat is any potential danger that can exploit a vulnerability.

### Common Threats

- Malware
- Phishing
- Spoofing
- Sniffing
- Man-in-the-middle attack
- Denial of Service (DoS)
- Distributed Denial of Service (DDoS)
- Session hijacking
- SQL injection
- ARP spoofing
- Password attacks
- Insider attacks

---

## 4. Types of Attacks

### A. Passive Attacks
Passive attacks attempt to observe or collect information without modifying it.

Examples:

- Packet sniffing
- Traffic analysis
- Eavesdropping

Impact:

- Data leakage
- Loss of confidentiality

### B. Active Attacks
Active attacks modify system behavior or data.

Examples:

- DoS attack
- DDoS attack
- Man-in-the-middle attack
- ARP spoofing
- Malware infection

Impact:

- Data tampering
- Service disruption
- Unauthorized access

---

## 5. Common Network Attacks

### 5.1 Malware
Malware is malicious software designed to harm systems.

Types of malware:

- Virus: attaches itself to executable files and spreads
- Worm: self-replicates and spreads through networks
- Trojan: disguised as a useful program but performs malicious actions
- Ransomware: encrypts files and demands a ransom
- Spyware: secretly collects user information
- Adware: displays unwanted advertisements

### 5.2 Phishing
Phishing tricks users into revealing sensitive information by impersonating trusted entities.

Examples:

- Fake banking email
- Fake login page
- Social engineering message

### 5.3 Spoofing
Spoofing means pretending to be another system or user.

Examples:

- IP spoofing
- MAC spoofing
- Email spoofing
- DNS spoofing

### 5.4 Sniffing
Sniffing is capturing data packets from a network.

Attackers use packet analyzers to capture:

- Usernames and passwords
- Email content
- Login sessions
- Sensitive information

Usually done on unencrypted or poorly protected networks.

### 5.5 Man-in-the-Middle (MITM)
A MITM attack occurs when an attacker intercepts communication between two parties and secretly alters or relays it.

Example:

- User connects to public Wi-Fi
- Attacker intercepts traffic between user and server
- Sensitive data can be stolen or modified

### 5.6 Denial of Service (DoS)
DoS attacks overwhelm a system or network with excessive traffic so that legitimate users cannot access the service.

Examples:

- Sending too many requests to a web server
- Flooding a router with packets
- Exhausting CPU or memory resources

### 5.7 Distributed Denial of Service (DDoS)
A DDoS attack uses multiple compromised devices (botnets) to flood a target.

Characteristics:

- Large scale attack
- Harder to block
- Often uses infected computers worldwide

### 5.8 ARP Spoofing / ARP Poisoning
ARP maps IP addresses to MAC addresses. Attackers can send fake ARP replies and trick devices into sending data to the attacker instead of the real host.

Impact:

- Traffic redirection
- Credential theft
- Session hijacking

### 5.9 SQL Injection
Attackers insert malicious SQL commands into input fields so that the database executes unintended queries.

Example:

- Username field: ' OR '1'='1

This can expose or modify sensitive data.

### 5.10 Insider Threats
Insiders such as employees or contractors may intentionally or accidentally compromise systems.

Examples:

- Leaking confidential data
- Misusing privileged access
- Installing unauthorized software

---

## 6. Security Attacks by Layer

### Application Layer Attacks
- Phishing
- SQL injection
- Cross-site scripting (XSS)
- Malware delivered via email or websites

### Transport Layer Attacks
- TCP SYN flood
- Session hijacking
- Port scanning

### Network Layer Attacks
- IP spoofing
- ICMP flooding
- Routing attacks
- ARP poisoning

### Data Link Layer Attacks
- MAC flooding
- VLAN hopping
- ARP spoofing

---

## 7. Security Devices and Mechanisms

### 7.1 Firewall
A firewall is a security device or software that monitors and filters incoming and outgoing traffic based on predefined rules.

Types of firewalls:

- Packet-filtering firewall
- Stateful firewall
- Proxy firewall
- Next-generation firewall (NGFW)

Purpose:

- Block unauthorized access
- Filter malicious traffic
- Protect internal networks

### 7.2 Intrusion Detection System (IDS)
An IDS detects suspicious activity or policy violations.

Types:

- Network-based IDS (NIDS)
- Host-based IDS (HIDS)

It generates alerts but does not usually block traffic automatically.

### 7.3 Intrusion Prevention System (IPS)
An IPS works like IDS but can actively block or prevent attacks in real time.

Difference:

- IDS = detects and alerts
- IPS = detects and prevents

### 7.4 VPN (Virtual Private Network)
A VPN creates a secure encrypted tunnel over a public network such as the internet.

Uses:

- Remote access
- Secure communication between offices
- Private data transmission over public networks

### 7.5 NAT (Network Address Translation)
NAT hides internal private IP addresses behind one or more public IP addresses.

Benefits:

- IP conservation
- Security through address hiding
- Simplified public addressing

### 7.6 DMZ (Demilitarized Zone)
A DMZ is a separate network segment placed between a private internal network and the internet.

Purpose:

- Hosts public services such as web servers or email servers
- Protects internal network from direct exposure

### 7.7 Proxy Server
A proxy server acts as an intermediary between clients and servers.

Benefits:

- Caching
- Content filtering
- Access control
- Privacy and monitoring

### 7.8 Honeypot
A honeypot is a decoy system used to attract attackers so that their methods can be studied and monitored.

Uses:

- Detect attacks
- Study attacker behavior
- Gather threat intelligence

---

## 8. Authentication, Authorization, and Accounting (AAA)

### Authentication
Authentication confirms the identity of a user or device.

Examples:

- Username and password
- OTP
- Biometrics
- Smart cards
- Digital certificates

### Authorization
Authorization decides what an authenticated user is allowed to do.

Example:

- A normal user may view files, but an admin may delete them

### Accounting
Accounting records usage details for monitoring, auditing, and billing.

Example:

- Logging user login times
- Tracking resource consumption

---

## 9. Access Control

Access control limits who can access resources and what actions they can perform.

### Common Types

1. Discretionary Access Control (DAC)
   - Owner decides access rights

2. Mandatory Access Control (MAC)
   - System-enforced policies

3. Role-Based Access Control (RBAC)
   - Access based on roles such as admin, user, auditor

4. Attribute-Based Access Control (ABAC)
   - Access based on attributes like time, location, device type

---

## 10. Cryptography Basics

Cryptography is the science of securing information by transforming it into unreadable form.

### Goals of Cryptography

- Confidentiality
- Integrity
- Authentication
- Non-repudiation

### Basic Terms

- Plaintext: original readable message
- Ciphertext: encrypted message
- Encryption: converting plaintext into ciphertext
- Decryption: converting ciphertext back to plaintext
- Key: secret value used in encryption/decryption

---

## 11. Types of Encryption

### A. Symmetric Encryption
Same key is used for encryption and decryption.

Examples:

- AES
- DES
- 3DES
- RC4

Advantages:

- Fast
- Efficient for large data

Disadvantages:

- Key distribution problem

### B. Asymmetric Encryption
Different keys are used: public key and private key.

Examples:

- RSA
- ECC
- Diffie-Hellman

Advantages:

- Better key exchange
- Supports digital signatures

Disadvantages:

- Slower than symmetric encryption

### Common Hybrid Approach
Modern secure communication often uses:

- Asymmetric encryption to exchange the session key
- Symmetric encryption to encrypt the actual data

---

## 12. Hashing

Hashing converts data into a fixed-length value called a hash.

Purpose:

- Verify integrity
- Store passwords securely
- Detect tampering

Examples:

- MD5
- SHA-1
- SHA-256

### Properties of a Good Hash

- Deterministic
- Fixed length
- Fast to compute
- Hard to reverse
- Small change in input gives large change in output

### Example
If a file is modified, its hash changes. This helps detect unauthorized changes.

---

## 13. Digital Signatures

A digital signature is created using the sender's private key and verified using the sender's public key.

Benefits:

- Authentication
- Integrity
- Non-repudiation

Used in:

- SSL/TLS certificates
- Software verification
- Secure email

---

## 14. SSL/TLS

SSL (Secure Sockets Layer) and TLS (Transport Layer Security) provide secure communication over networks.

### Purpose

- Protect data in transit
- Prevent eavesdropping and tampering
- Authenticate servers and sometimes clients

### Common Use

- HTTPS websites
- Secure email communication
- Secure remote connections

### TLS Handshake

1. Client requests secure connection
2. Server sends certificate
3. Client verifies certificate authority and validity
4. Session keys are exchanged
5. Encrypted communication begins

---

## 15. Security Mechanisms in Networks

### 15.1 IPsec
IPsec is a suite of protocols used to secure IP communication.

Provides:

- Authentication Header (AH)
- Encapsulating Security Payload (ESP)
- Tunnel mode and transport mode

Used in:

- VPNs
- Secure site-to-site communication

### 15.2 Secure Shell (SSH)
SSH provides secure remote login and command execution.

Benefits:

- Encrypts remote access
- Protects against eavesdropping

### 15.3 Secure Email
Security for email uses:

- SMTP with TLS
- S/MIME
- PGP

### 15.4 Wireless Security
Wireless networks are vulnerable to unauthorized access.

#### WPA (Wi-Fi Protected Access)
- Improves on WEP
- Uses stronger encryption and authentication

#### WPA2
- Most common standard for secure wireless networks
- Uses AES-based encryption

#### WPA3
- More robust than WPA2
- Better protection against brute-force guessing
- Improved security for open networks

---

## 16. Common Security Policies and Best Practices

### Best Practices for Network Security

- Use strong passwords and multi-factor authentication
- Keep systems and software updated with patches
- Use firewalls and intrusion detection systems
- Encrypt sensitive communication
- Restrict access by role and principle of least privilege
- Disable unnecessary services and open ports
- Use antivirus and anti-malware software
- Monitor logs and alerts continuously
- Backup critical data regularly
- Educate users about phishing and social engineering

### Principle of Least Privilege
Users should be given only the minimum privileges required to do their job.

This reduces exposure to accidental or malicious misuse.

### Defense in Depth
Multiple layers of security are better than a single control.

Examples:

- Firewall + IDS + antivirus + access controls + encryption

---

## 17. Network Security Tools

### Useful Security Tools

- Wireshark: packet capture and analysis
- Nmap: network scanning and vulnerability discovery
- Netstat: network connection overview
- tcpdump: packet capture on Linux
- Snort: IDS/IPS
- Metasploit: penetration testing framework
- OpenVAS: vulnerability assessment

These are used by administrators to detect, analyze, and prevent threats.

---

## 18. Types of Security Controls

### Preventive Controls
Stop attacks before they happen.

Examples:

- Firewalls
- Antivirus
- Access control lists
- Encryption
- Multi-factor authentication

### Detective Controls
Identify attacks after they happen.

Examples:

- IDS
- Logging
- SIEM systems
- Monitoring tools

### Corrective Controls
Reduce damage after an attack.

Examples:

- Restoring backups
- Replacing compromised systems
- Disconnecting infected machines

### Deterrent Controls
Discourage attackers.

Examples:

- Security warnings
- CCTV monitoring
- Legal policies

---

## 19. Threats to Wireless Networks

Wireless networks are vulnerable to:

- Unauthorized access
- Rogue access points
- Evil twin attacks
- Packet sniffing
- Jamming
- WPA/WPA2 credential attacks

### Preventive Steps

- Use strong encryption like WPA2/WPA3
- Disable SSID broadcasting if required
- Use enterprise authentication
- Place access points in secure locations
- Monitor for rogue devices

---

## 20. Security in Routers and Switches

### Router Security

- Use strong admin passwords
- Disable unused interfaces
- Update firmware
- Restrict remote access
- Use ACLs
- Enable logging

### Switch Security

- Use VLAN segmentation
- Disable unused ports
- Use port security
- Prevent MAC flooding and DHCP snooping
- Enable 802.1X authentication

---

## 21. Network Security Standards and Compliance

Many organizations follow security standards such as:

- ISO 27001
- NIST Cybersecurity Framework
- CIS Controls
- PCI DSS
- HIPAA
- GDPR

These standards define security requirements for confidentiality, data handling, risk management, and access control.

---

## 22. Security Incident Response

When a security incident occurs, the process is usually:

1. Detect the incident
2. Contain the impact
3. Investigate and analyze root cause
4. Eradicate the threat
5. Recover systems and services
6. Learn and improve defenses

### Important Logs to Check

- Firewall logs
- IDS/IPS alerts
- Authentication logs
- Web server logs
- DNS logs
- Application logs

---

## 23. Comparison Table: Security Concepts

| Term | Meaning | Example |
|---|---|---|
| Firewall | Filters network traffic | Block port 22 from internet |
| IDS | Detects suspicious traffic | Alert on port scan |
| IPS | Prevents detected attacks | Block malicious traffic automatically |
| VPN | Secure tunnel over public network | Remote office connectivity |
| NAT | Hides private IPs | Internal PCs behind one public IP |
| DMZ | Isolated public service area | Web server zone |
| ACL | Access rule set | Permit only HR subnet |
| Hash | Data fingerprint | SHA-256 of file |
| Encryption | Converts data to unreadable form | AES encryption |

---

## 24. Quick Revision Notes

- Network security protects data, devices, and services from unauthorized access and attacks
- CIA stands for Confidentiality, Integrity, and Availability
- Passive attacks try to observe, while active attacks modify or disrupt
- Firewall, IDS, IPS, VPN, DMZ, and NAT are key network security components
- Encryption protects confidentiality; hashing protects integrity
- Authentication confirms identity; authorization controls privileges
- Strong policies, updates, backups, and monitoring are essential

---

## 25. Important Definitions

- Security attack: An attempt to compromise security goals
- Vulnerability: Weakness in a system
- Threat: Potential danger that exploits a vulnerability
- Risk: Probability and impact of a threat
- Firewall: Device that filters network traffic
- IDS: System that detects suspicious activity
- IPS: System that prevents attacks
- VPN: Encrypted tunnel over public networks
- Encryption: Converting readable data into encoded data
- Hashing: Producing fixed-length fingerprint of data
- Digital signature: Cryptographic proof of sender identity and integrity

---

## 26. Example Questions

### Q1. What does CIA stand for?
Answer: Confidentiality, Integrity, Availability

### Q2. What is the difference between IDS and IPS?
Answer: IDS detects and alerts; IPS detects and blocks attacks

### Q3. What is a firewall?
Answer: A device or software that filters traffic based on rules

### Q4. Why is encryption important?
Answer: It protects data from unauthorized access during transmission or storage

### Q5. What is a DDoS attack?
Answer: A distributed denial-of-service attack using multiple compromised systems to flood a target

### Q6. What is a VPN used for?
Answer: Secure communication over untrusted networks such as the internet

### Q7. What is hashing used for?
Answer: To verify data integrity and store passwords securely

### Q8. What is the difference between symmetric and asymmetric encryption?
Answer: Symmetric uses one shared key; asymmetric uses public and private keys

---

## 27. Practical Notes

- Use firewall rules to restrict traffic to trusted sources
- Monitor logs for failed login attempts and suspicious traffic
- Use HTTPS instead of HTTP where possible
- Keep routers, switches, and servers patched
- Segment networks with VLANs and DMZs
- Use WPA2/WPA3 for wireless networks
- Regularly back up data and test restoration

---

## 28. Exam-Friendly Summary

### Network Security in One Line
Network security is the protection of digital resources and communication against unauthorized access, manipulation, and disruption.

### Key Security Components
- Firewall
- IDS/IPS
- VPN
- Encryption
- Authentication
- Access control
- Logging and monitoring

### Key Exam Topics
- CIA triad
- DoS vs DDoS
- Malware and phishing
- MITM attack
- Encryption methods
- Firewall, IDS, IPS
- Hashing and digital signatures

---

## 29. Tomorrow's Preview

Day 31 will continue with advanced network security topics such as cryptography in depth, wireless security, secure communication protocols, and common real-world network attack scenarios.

---

## ✅ Final Summary

Network security is essential in today’s connected world because networks carry sensitive and valuable information. A secure network combines technical controls like firewalls, IDS/IPS, VPNs, authentication, encryption, and access control with policy, monitoring, and user awareness. The core idea is to protect confidentiality, preserve integrity, and maintain availability of network resources.
