# AzureSent-ChatGpt-SOC-Automation

Integration of Microsoft Sentinel with Azure OpenAI (GPT-4) using Logic Apps for SOC automation. This project sets up automated incident response workflows powered by AI, including API enrichment, playbook deployment, and alert investigation from Microsoft Sentinel.

**ActionsGPT** is a hands-on security automation project that connects **Microsoft Sentinel** with **Azure OpenAI (GPT-4o)** using **Logic Apps**. This solution turns incident data into actionable intelligence using LLM-based analysis.

It’s designed to simulate how a Tier 1/2 SOC analyst would review security alerts, triage them, and recommend next steps — all through automation.

---

## 📌 What This Project Does

- ✅ Uses **Microsoft Sentinel** to detect real-world threats (e.g., Tor sign-ins)
- ✅ Triggers **Logic Apps** automatically when alerts fire
- ✅ Sends contextual alert data to **GPT-4o** (via Azure OpenAI)
- ✅ GPT responds with structured threat analysis & response recommendations
- ✅ Optionally exports the results, adds them to incidents, or notifies SOC

---

## 🧠 Project Workflow

```mermaid
flowchart TD
    A[Sentinel Analytics Rule Fires] --> B[Logic App Triggered]
    B --> C[Format Incident Data]
    C --> D[Send Data to GPT-4o via Azure OpenAI API]
    D --> E[Receive Structured Response]
    E --> F[Post Back to Sentinel / Notify Analyst / Export]
