# MITRE ATT&CK

MITRE ATT&CK (Adversarial Tactics, Techniques and Common Knowledge) is a free, publicly available knowledge base that documents real-world adversary behavior. It describes what attackers do (tactics) and how they do it (techniques) — from the very first step of reconnaissance all the way through data exfiltration and impact.

Security teams use it to map their defenses, build detection rules, conduct threat hunting, and perform red/blue team exercises.

---

## 14 Tactics

1. **Reconnaissance** – Gathering information about the target before the attack  
2. **Resource Development** – Attacker prepares tools, infrastructure, or accounts for the attack  
3. **Initial Access** – Attacker gains entry into the target system  
4. **Execution** – Malicious code is executed on the system  
5. **Persistence** – Attacker maintains access even after reboot or detection  
6. **Privilege Escalation** – Attacker gains higher-level permissions  
7. **Defense Evasion** – Attacker avoids detection by security systems  
8. **Credential Access** – Attacker steals usernames and passwords  
9. **Discovery** – Attacker explores system and network environment  
10. **Lateral Movement** – Attacker moves across systems in the network  
11. **Collection** – Sensitive data is gathered from the system  
12. **Command and Control (C2)** – Attacker communicates with compromised systems  
13. **Exfiltration** – Data is transferred out of the network  
14. **Impact** – Attacker disrupts, destroys, or manipulates systems/data  

---

## Techniques (Attacker Use + Defender Detection)

### 1. Phishing (T1566)
- **Attacker Use:** Sends fake emails or messages to trick users into revealing credentials or executing malicious files  
- **Defender Detection:** Monitor email logs, suspicious links, file execution, and unusual login activity after email interaction  

---

### 2. Valid Accounts (T1078)
- **Attacker Use:** Uses stolen or legitimate credentials to log in as a normal user and access systems  
- **Defender Detection:** Detect unusual login times, geographic anomalies, impossible travel, and abnormal account behavior  

---

### 3. Drive-by Compromise (T1189)
- **Attacker Use:** Infects users by exploiting browser vulnerabilities when they visit a compromised website  
- **Defender Detection:** Monitor browser activity, unexpected downloads, network traffic, and suspicious process execution  

---
