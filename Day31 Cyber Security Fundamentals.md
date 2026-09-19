# 🔐 Day 31: Cyber Security Fundamentals

Welcome to Day 31 of the Computer Networks & Security study journey. This lesson introduces the foundation of cyber security, covering the core principles, common threats, security controls, and good practices used to protect digital systems.

## 📝 Topics Covered

- What is cyber security?
- CIA triad: Confidentiality, Integrity, Availability
- Assets, vulnerabilities, threats, and risks
- Types of cyber attacks
- Authentication, authorization, and accounting
- Security controls and defense in depth
- Threat actors and social engineering
- Encryption basics and malware fundamentals
- Best practices for cyber hygiene

## 🎯 Learning Objectives

By the end of this lesson, you should be able to:

- Define cyber security and explain why it is important
- Explain the CIA triad and its real-world meaning
- Identify major cyber threats and attack techniques
- Distinguish between threat, vulnerability, and risk
- Understand preventive, detective, and corrective security controls
- Explain authentication and access control concepts
- Describe common cyber security best practices

---

## 1. What is Cyber Security?

Cyber security is the practice of protecting computer systems, networks, data, and digital services from unauthorized access, misuse, damage, or disruption.

It includes protecting:

- Personal devices and computers
- Servers and cloud systems
- Mobile applications and networks
- Data in storage and during transmission
- Business operations and critical infrastructure

### Why Cyber Security is Important

- Sensitive information such as passwords, bank data, and health records is stored online
- Attackers can steal data, damage systems, or interrupt business services
- Organizations rely on digital workflows, so downtime can be expensive
- Security helps maintain trust, compliance, and continuity

---

## 2. Core Security Goals: CIA Triad

The most common model in cyber security is the CIA triad.

| Principle | Meaning | Example |
|---|---|---|
| Confidentiality | Only authorized people can access data | Encrypted email, access control |
| Integrity | Data remains accurate and unaltered | Hash verification, digital signatures |
| Availability | Systems and data are accessible when needed | Backups, redundancy, cloud uptime |

### Example
A hospital system must ensure:

- Confidentiality: patient records are visible only to authorized staff
- Integrity: medical records are not changed without permission
- Availability: patient system remains available during emergencies

---

## 3. Important Security Terms

### Asset
An asset is anything valuable that needs protection.

Examples:

- Data files
- Laptops and servers
- User accounts
- Network devices
- Intellectual property

### Vulnerability
A vulnerability is a weakness in hardware, software, or configuration that can be exploited.

Examples:

- Weak password policy
- Old operating system
- Unpatched application
- Misconfigured firewall

### Threat
A threat is a possible danger or harmful event.

Examples:

- Malware
- Phishing attack
- Insider misuse
- Natural disaster

### Risk
Risk is the chance that a threat will exploit a vulnerability and cause harm.

Risk = Threat × Vulnerability × Impact

### Exploit
An exploit is a malicious method used to take advantage of a vulnerability.

---

## 4. Common Types of Cyber Attacks

### 4.1 Malware
Malware is malicious software designed to damage or exploit computer systems.

Types:

- Virus: spreads through infected files
- Worm: self-replicates across networks
- Trojan: disguised as legitimate software
- Ransomware: encrypts files and demands payment
- Spyware: secretly monitors user activity
- Adware: displays unwanted ads

### 4.2 Phishing
Phishing tricks users into revealing sensitive information by pretending to be trusted.

Examples:

- Fake login pages
- Fake bank emails
- Fake OTP messages

### 4.3 Social Engineering
Social engineering manipulates people into giving away secrets or access.

Examples:

- Pretending to be IT support
- Creating urgency or fear
- Asking for passwords or codes

### 4.4 Denial of Service (DoS)
A DoS attack overwhelms a system so legitimate users cannot access it.

### 4.5 Distributed Denial of Service (DDoS)
A DDoS attack uses multiple compromised computers to flood the target.

### 4.6 Man-in-the-Middle (MITM)
An attacker intercepts communication between two parties and may steal or alter data.

### 4.7 Password Attacks
Attempts to guess, crack, or steal passwords.

Examples:

- Brute force
- Dictionary attack
- Credential stuffing

### 4.8 SQL Injection
Attackers insert malicious SQL code into input fields to manipulate databases.

### 4.9 Zero-Day Attack
A zero-day attack exploits a vulnerability before the vendor releases a patch.

### 4.10 Insider Threat
An insider threat comes from an employee, contractor, or trusted person who misuses access.

---

## 5. Security Principles

### Least Privilege
Users should have only the minimum permissions required for their work.

### Defense in Depth
Multiple layers of security are used instead of relying on a single control.

Example:

- Firewall
- Antivirus
- MFA
- Intrusion detection system
- User awareness training

### Separation of Duties
Critical tasks are split among different people to reduce fraud and misuse.

### Fail-Safe Defaults
Systems should default to secure behavior.

### Regular Updates and Patching
Software updates fix known vulnerabilities and reduce attack surface.

---

## 6. Authentication, Authorization, and Accounting (AAA)

### Authentication
Authentication verifies who a user is.

Examples:

- Username and password
- OTP
- Biometrics
- Smart cards

### Authorization
Authorization determines what an authenticated user is allowed to do.

Example:

- Admin can modify system settings
- Normal user can only view reports

### Accounting
Accounting tracks what users do and records system activity.

Examples:

- Login logs
- File access history
- Firewall logs

---

## 7. Security Controls

Security controls are actions or tools used to protect systems.

### Preventive Controls
Stop attacks before they happen.

Examples:

- Firewall
- Antivirus
- MFA
- Strong passwords
- Encryption

### Detective Controls
Detect attacks or suspicious events.

Examples:

- IDS/IPS
- Log monitoring
- SIEM alerts
- Security audits

### Corrective Controls
Reduce damage after an incident.

Examples:

- Restoring backups
- Removing malware
- Patching vulnerabilities
- Incident response

### Deterrent Controls
Discourage attacks.

Examples:

- Warning banners
- Security policies
- Visible surveillance

---

## 8. Common Security Tools

### Firewall
A firewall filters incoming and outgoing network traffic based on security rules.

### IDS / IPS
- IDS: detects suspicious activity
- IPS: detects and actively blocks suspicious activity

### Antivirus / Anti-malware
Detects, removes, and blocks malicious software.

### VPN
A Virtual Private Network creates a secure encrypted tunnel over public networks.

### MFA (Multi-Factor Authentication)
MFA requires more than one form of authentication.

Example:

- Password + OTP
- Password + fingerprint

### Encryption
Encryption converts readable data into unreadable form so only authorized users can read it.

---

## 9. Basics of Encryption

### Symmetric Encryption
Same key is used for encryption and decryption.

Examples:

- AES
- DES (older)

Advantages:

- Fast
- Efficient for large amounts of data

Disadvantages:

- Key distribution is a challenge

### Asymmetric Encryption
Different keys are used: public key and private key.

Examples:

- RSA
- ECC

Use case:

- Secure communication over the internet
- Digital signatures

### Hashing
Hashing converts data into a fixed-length value.

Examples:

- SHA-256

Used for:

- Password storage
- Data integrity checks
- Digital signatures

### Digital Signature
A digital signature verifies the identity of the sender and ensures the message was not altered.

---

## 10. Types of Threat Actors

Common attackers include:

- Hackers: may exploit vulnerabilities for fun, profit, or challenge
- Crackers: malicious hackers focused on damaging systems
- Script kiddies: use pre-written tools without deep understanding
- Cybercriminals: financially motivated attackers
- State-sponsored attackers: politically or militarily motivated
- Insider threats: compromised or malicious employees

---

## 11. Cyber Security Best Practices

- Use strong and unique passwords
- Enable multi-factor authentication
- Keep software and systems updated
- Back up important data regularly
- Be careful with email links and attachments
- Use antivirus and firewall protection
- Limit user permissions
- Educate users about phishing and scams
- Monitor logs and alerts
- Follow company security policies

---

## 12. Quick Revision

### Cyber Security = Protection of systems, networks, and data

### Core goals:
- Confidentiality
- Integrity
- Availability

### Common threats:
- Malware
- Phishing
- Social engineering
- DDoS
- MITM
- Insider threats

### Common controls:
- Firewall
- Antivirus
- IDS/IPS
- MFA
- Encryption
- Patching
- Access control

### Security mindset:
- Prevent attacks
- Detect suspicious behavior
- Respond quickly
- Recover safely

---

## ✅ Final Summary

Cyber security is the protection of digital assets from unauthorized access, misuse, and damage. It combines technical defenses, policy enforcement, user awareness, and incident response to ensure systems remain secure and reliable. Understanding the CIA triad, common attack methods, and key security controls is essential for every student of networking and IT.

## 🧠 Short Mnemonic

Think of cyber security as:

- Protect
- Detect
- Respond
- Recover

This is the ongoing cycle of defending digital systems.
