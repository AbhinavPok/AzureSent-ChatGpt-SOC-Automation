# Building ActionsGPT: My Journey to Automating Cybersecurity with ChatGPT + Azure Sentinel

**ActionsGPT** is a project I built to combine the power of **ChatGPT (GPT-4o)** with **Microsoft Sentinel** to create an automated cybersecurity incident response system.

It uses Logic Apps in Azure to send alert data from Sentinel to GPT, get back AI-generated analysis, and post it back into the incident — all automatically.

---

## 🚀 What This Project Includes

### ✅ ChatGPT / OpenAI
- Created custom prompts for cybersecurity alerts
- Structured GPT responses with observations, findings, and actions
- Connected to threat intelligence APIs like:
  - VirusTotal
  - AbuseIPDB
  - ThreatFox
- Even used GPT to generate Excel macros for reporting

### ✅ Microsoft Azure
- Set up a **SIEM using Microsoft Sentinel**
- Deployed **Azure OpenAI GPT-4o**
- Customized prompts in **OpenAI Playground**
- Built a **Logic App** playbook to:
  - Trigger on Sentinel incidents
  - Send alert data to GPT
  - Return analysis and next steps

---

## 🔧 How It Works

1. Sentinel detects a threat (like login from a Tor IP)
2. Logic App gets triggered
3. Alert data is sent to GPT-4o
4. GPT returns a response like:

```json
{
  "Observations": "Login from Tor node",
  "Findings": ["User: admin@domain.com", "IP: 185.x.x.x"],
  "Recommendations": ["Enable MFA", "Investigate session", "Block IP"]
}
