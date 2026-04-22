# 🛡️ MITRE ATT&CK Framework Summary

**MITRE ATT&CK®** (Adversarial Tactics, Techniques, and Common Knowledge) is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations. It provides a common language for both red teams (attackers) and blue teams (defenders) to understand and communicate cyber threats.

> [!TIP]
> Use this framework to map your organization's defense posture against actual threat actor behaviors.

---

## 📊 The 14 Enterprise Tactics

Tactics represent the "why" of an ATT&CK technique or sub-technique. It is the adversary's tactical objective: the reason for performing an action.

| # | Tactic | Definition (1-Line Summary) |
| :--- | :--- | :--- |
| 1 | **Reconnaissance** | Gathering information to plan future adversary operations. |
| 2 | **Resource Development** | Establishing infrastructure and accounts to support operations. |
| 3 | **Initial Access** | Techniques used to gain an initial foothold within a network. |
| 4 | **Execution** | Techniques that result in adversary-controlled code running on a local or remote system. |
| 5 | **Persistence** | Maintaining access across restarts, changed credentials, and other interruptions. |
| 6 | **Privilege Escalation** | Gaining higher-level permissions on a system or network. |
| 7 | **Defense Evasion** | Avoiding detection throughout their compromise. |
| 8 | **Credential Access** | Stealing credentials like names and passwords. |
| 9 | **Discovery** | Gaining knowledge about the system and internal network. |
| 10 | **Lateral Movement** | Moving from one system to another within the network. |
| 11 | **Collection** | Gathering data of interest to the adversary goal. |
| 12 | **Command & Control** | Communicating with systems under their control within a target network. |
| 13 | **Exfiltration** | Stealing data from your network. |
| 14 | **Impact** | Manipulating, interrupting, or destroying your systems and data. |

---

## 🔍 Deep Dive: Techniques & Detections

Below are three common techniques, how attackers use them, and how we can detect them as defenders.

### 1. Phishing ([T1566](https://attack.mitre.org/techniques/T1566/))
*   **Attacker Use**: Sending fraudulent emails (Spearphishing) with malicious attachments or links to compromise a user's workstation.
*   **Defender Detection**: 
    *   **Email Gateway**: Look for suspicious attachments (.iso, .zip, .exe) or unusual sender domains.
    *   **Endpoint Logs**: Monitor for unusual parent-child process relationships (e.g., `outlook.exe` spawning `powershell.exe`).

### 2. Valid Accounts ([T1078](https://attack.mitre.org/techniques/T1078/))
*   **Attacker Use**: Using stolen or "found" credentials (via data breaches or password spraying) to log in as a legitimate user.
*   **Defender Detection**: 
    *   **SIEM Analysis**: Detect "Impossible Travel" (logins from two distant locations in a short time).
    *   **Behavioral Monitoring**: Monitor for unusual login times or access to resources the user doesn't typically touch.

### 3. Drive-by Compromise ([T1189](https://attack.mitre.org/techniques/T1189/))
*   **Attacker Use**: Compromising a website that the target audience frequently visits to automatically deliver malware to visitors' browsers.
*   **Defender Detection**: 
    *   **Web Proxy Logs**: Monitor for redirects to unknown or low-reputation domains.
    *   **EDR**: Detect suspicious browser-level activity, such as a browser process executing a script in a temporary directory.

---

> [!IMPORTANT]
> **Conclusion**: MITRE ATT&CK is not just a list; it is a mindset. By understanding the "how" and "why" of an attack, we can build robust, intelligence-driven defenses.

*For more details, visit the official [MITRE ATT&CK Website](https://attack.mitre.org/)*
