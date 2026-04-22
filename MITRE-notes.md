#  MITRE ATT&CK
**MITRE ATT&CK®** (Adversarial Tactics Technique and Common Knowledge) is a free, publicly available knowledge base that document real world adversary behavior. It describes what attacker do (tactics) and how they do (Technique) – from the very first step of reconnaissance all the way through data exfiltration and impact.
Security teams use it to map their defense, build detection rules, conduct threat hunting and perform red\blue team exercise.



---

## 14 Tactics

Tactics represent the "why" of an ATT&CK technique or sub-technique. It is the adversary's tactical objective: the reason for performing an action.

| # | Tactic | Definition (1-Line Summary) |
| :--- | :--- | :--- |
| 1 | **Reconnaissance** |– Gathering information about target before the attack. |
| 2 | **Resource Development** | Attacker prepare the tools, infrastructure, or account for the attack. |
| 3 | **Initial Access** | Attacker gain the entry into the target system.|
| 4 | **Execution** | Malicious code is executed on the system.|
| 5 | **Persistence** | Attacker maintain the access even after reboot or detection. |
| 6 | **Privilege Escalation** |Attacker gain the higher-level permission. |
| 7 | **Defense Evasion** | Attacker avoid detection by security systems |
| 8 | **Credential Access** | Attacker steals username & password. |
| 9 | **Discovery** | Attacker explore system & network environment. |
| 10 | **Lateral Movement** | Attacker move across the system in the network |
| 11 | **Collection** | Sensitive data is gathering from the system. |
| 12 | **Command & Control** | CAttacker communicate with compromised system. |
| 13 | **Exfiltration** | Data is transferred out of the network. |
| 14 | **Impact** | Attacker disrupt. Destroy, or manipulate system/data. |

---

## Techniques (Attacker Use + Defender Detection)

Below are three common techniques, how attackers use them, and how we can detect them as defenders.

### 1. Phishing ([T1566](https://attack.mitre.org/techniques/T1566/))
*   **Attacker Use**: Sending fraudulent emails (Spearphishing) with malicious attachments or links to compromise a user's workstation.
*   **Defender Detection**: Monitor email logs, suspicious links, file execution, and unusual login activity after email interaction. 

### 2. Valid Accounts ([T1078](https://attack.mitre.org/techniques/T1078/))
*   **Attacker Use**: Using stolen or "found" credentials (via data breaches or password spraying) to log in as a legitimate user.
*   **Defender Detection**: : Detect unusual login times, geographic anomalies, impossible travel, and abnormal account behaviour. 

### 3. Drive-by Compromise ([T1189](https://attack.mitre.org/techniques/T1189/))
*   **Attacker Use**: Compromising a website that the target audience frequently visits to automatically deliver malware to visitors' browsers.
*   **Defender Detection**: Monitor browser activity, unexpected downloads, network traffic, and suspicious process execution. 
---

> **Conclusion**: MITRE ATT&CK is not just a list; it is a mindset. By understanding the "how" and "why" of an attack, we can build robust, intelligence-driven defenses.

*For more details, visit the official [MITRE ATT&CK Website](https://attack.mitre.org/)*
