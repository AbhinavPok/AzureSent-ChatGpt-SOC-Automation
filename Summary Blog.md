# Building ActionsGPT: AI‑Powered SOC Automation with ChatGPT and Azure Sentinel

## 🚀 Introduction

As a cybersecurity enthusiast and hands-on learner, I set out to integrate two of the most powerful platforms available today:

- **OpenAI’s ChatGPT** — a state-of-the-art large language model (LLM) capable of advanced natural language reasoning.
- **Microsoft Sentinel** — a cloud-native SIEM/SOAR platform designed for modern Security Operations Centers (SOCs).

The result is **ActionsGPT** — a fully automated, AI-powered SOC solution that detects threats, enriches alerts with threat intelligence, and intelligently analyzes incidents using **GPT‑4o**.

This document outlines my real-world journey: from advanced prompt engineering in ChatGPT to building a scalable, automated SOC in Azure using Sentinel and Logic Apps.

---

## 🤖 Part 1: Engineering GPT for Cybersecurity

Before working in Azure, I began by using **ChatGPT itself** to design a custom cybersecurity assistant named **ActionsGPT**.

The goal was to create an AI agent capable of:

- Interpreting security alerts
- Summarizing incident context
- Producing structured, analyst-style responses
- Recommending next steps with clear reasoning

---

### ✳️ Advanced Prompt Design

Rather than using generic prompts like *“analyze this alert”*, I created a **structured prompt framework**.

Key design decisions:

- Defined a **system role** that behaves like a cybersecurity analyst
- Enforced a structured response format:
  - **Observations**
  - **Key Findings**
  - **Reasoning**
  - **Recommended Actions**

This approach made GPT responses:
- More consistent
- Easier to audit
- Suitable for SOC workflows

> **Best Practice:** Always separate `system` and `user` messages when using the OpenAI API to ensure predictable AI behavior.

---

### 🔗 Threat Intelligence API Integration

To reduce hallucinations and improve accuracy, I planned integrations with external threat intelligence platforms:

- **VirusTotal** — IP, domain, and file reputation
- **AbuseIPDB** — community-reported IP abuse data
- **ThreatFox** — malware indicators of compromise (IoCs)

These sources are intended to:

- Enrich alerts before GPT analysis
- Provide real-world data instead of assumptions
- Improve confidence in incident recommendations

Future iterations will pass summarized threat intel directly into GPT’s context.

---

### 📊 Excel Automation Using ChatGPT

I also experimented with **ChatGPT-generated Excel VBA macros** to:

- Export GPT findings automatically
- Format investigation results into tables
- Visualize timelines and incident data

This enables easy reporting and compliance-friendly exports for non-technical stakeholders.

---

## ☁️ Part 2: Building a Cloud SIEM with Azure Sentinel

With the AI logic defined, I moved to Azure to implement the operational SOC environment using **Microsoft Sentinel**.

---

### 🏗️ Sentinel Lab Setup

The initial setup included:

- Creating a **Log Analytics Workspace**
- Enabling **Microsoft Sentinel**
- Connecting core data sources:
  - Azure AD Sign‑In Logs
  - Azure Activity Logs
- Applying **data ingestion limits** to control cost

This environment served as a lab for alert testing and automation.

---

### 🔔 Custom Alert Creation

One of the primary detections I built was a custom analytics rule:

**Successful Sign‑Ins from Tor Network**

- Uses KQL and external Tor IP lists
- Runs every 5 minutes
- Automatically creates incidents

The rule was deployed using an **ARM JSON template** and verified in the Sentinel Analytics Rules dashboard. I also mapped detections to **MITRE ATT&CK**, including technique **T1133 – External Remote Services**.

---

### 🧠 Deploying GPT‑4o in Azure OpenAI

To integrate AI securely, I deployed **GPT‑4o** using **Azure OpenAI**:

- Region: East US 2
- Tested prompts using the **OpenAI Playground**
- Validated API requests using:
  - `ChatGPT-Body.json`
  - `ChatGPT-Role.txt`

This allowed full control over token usage, temperature, and response behavior.

---

### ⚙️ Logic App Playbook Automation

The core automation was built using **Azure Logic Apps**:

1. Trigger on Sentinel incident creation  
2. Initialize OpenAI variables (API key, endpoint, payload)  
3. Send HTTP POST request to GPT‑4o  
4. Receive structured analysis from GPT  
5. Attach results back to the incident or store externally  

For security, the Logic App was assigned the **Microsoft Sentinel Responder** IAM role.

---

## 🧪 Test Scenario: Tor-Based Login Detection

To validate the pipeline, I simulated a login from a known Tor exit node.

Flow:

- Sentinel detected the activity
- Logic App triggered automatically
- GPT‑4o analyzed the incident and returned:

```json
{
  "Observations": "Login from known Tor exit node.",
  "Findings": [
    "User: admin@company.com",
    "IP: 185.220.101.15 (Tor network)",
    "Login outside business hours"
  ],
  "Recommendations": [
    "Investigate the user session",
    "Consider enforcing MFA",
    "Add IP to watchlist"
  ]
}
