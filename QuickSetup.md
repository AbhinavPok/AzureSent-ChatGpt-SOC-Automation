# 🛠️ Full Setup Guide for ActionsGPT: Azure Sentinel + ChatGPT SOC Automation

This guide walks you through setting up **ActionsGPT**, an AI-powered SOC automation project that integrates:

- **Microsoft Sentinel** (SIEM)
- **Azure Logic Apps** (SOAR)
- **Azure OpenAI (GPT-4o)**
- Custom analytics rules
- External threat intelligence APIs (VirusTotal, AbuseIPDB, etc.)

---

## 📋 Prerequisites

Before starting, ensure you have:

- ✅ An **Azure subscription**
- ✅ Access to **Microsoft Sentinel**
- ✅ Access to **Azure OpenAI** (GPT-4o)
- ✅ (Optional) API keys for:
  - VirusTotal
  - AbuseIPDB
  - ThreatFox

---

## 🚩 Step 1: Set Up Microsoft Sentinel

1. **Create a Log Analytics Workspace**
   - Go to Azure Portal → Search “Log Analytics Workspaces”
   - Create a new workspace (name: `SentinelWorkspace`)

2. **Enable Microsoft Sentinel**
   - Go to Microsoft Sentinel → Select your workspace
   - Click **“+ Add Microsoft Sentinel”**

3. **Connect Data Sources**
   - Connect built-in data like:
     - `Azure Active Directory SigninLogs`
     - `AzureActivity`
   - Avoid noisy sources (e.g., Defender, Syslog) for free-tier usage

4. **Ingestion Controls**
   - In **Log Analytics > Usage and estimated costs**, set **daily cap** to control data usage

---

## 🔔 Step 2: Create and Import Custom Alert Rule

1. Use the provided rule: `AR-Successful-SignIns-from-Tor-Network.json`

2. Deploy the rule:
   - Go to **Sentinel → Analytics**
   - Click **“Create” → “Import from JSON”**
   - Select `AR-Successful-SignIns-from-Tor-Network.json`
   - Edit rule frequency and name if needed

3. Validate:
   - Confirm it's listed under Analytics Rules
   - Set rule status to **enabled**

---

## 🧠 Step 3: Deploy GPT-4o on Azure OpenAI

1. **Request Access (if needed)**
   - Apply for Azure OpenAI access [here](https://aka.ms/oai/access)

2. **Create Azure OpenAI Resource**
   - Search “Azure OpenAI” → Create
   - Location: *East US 2*
   - Pricing tier: Standard S0

3. **Deploy GPT-4o**
   - Navigate to your OpenAI resource
   - Go to **Deployments** → Create deployment
   - Model: `gpt-4o`
   - Name it something like `actions-gpt`

4. **Note Your Credentials**
   - Record:
     - API key
     - Endpoint URL
     - Deployment name

---

## ⚙️ Step 4: Deploy the Logic App

This is the heart of automation — it takes Sentinel incidents and sends them to GPT.

### 🔧 Method A: Deploy via JSON Template

1. Go to Azure → Logic Apps → Create a new **Consumption Logic App**
2. After creation, open the Designer
3. Click **“Import from JSON”**
4. Use the provided file: `ChatSetup.json`
5. Review the workflow:
   - **Trigger**: When an incident is created in Sentinel
   - **Variables**: Stores key info
   - **HTTP Action**: Sends incident data to GPT
   - **Response Parsing**: Receives GPT’s findings

### 🛡️ Assign IAM Role

Give the Logic App permission to read Sentinel alerts:

- Go to Logic App → **Identity**
- Enable **System-assigned identity**
- Go to **Access control (IAM)** → Add role assignment
  - Role: `Microsoft Sentinel Responder`
  - Scope: Your Sentinel workspace

---

## 🔑 Step 5: Configure GPT API Call

Update the `HTTP` block in Logic App:

- **Method**: POST
- **URL**: Your Azure OpenAI endpoint
  - Format: `https://YOUR-RESOURCE.openai.azure.com/openai/deployments/YOUR-DEPLOYMENT/chat/completions?api-version=2023-07-01-preview`
- **Headers**:
  - `Content-Type`: `application/json`
  - `api-key`: Your OpenAI API key
- **Body**: Use format from `ChatGPT-Body.json`

Include:
- `system` role from `ChatGPT-Role.txt`
- `user` message with incident context

---

## 🧪 Step 6: Test the System

### Trigger a Simulated Alert

- Use Azure SignInLogs or inject custom data
- Simulate a sign-in from a **Tor exit node IP**
- This will trigger your **custom rule**

### Observe the Logic App Execution

- Go to **Logic Apps → Runs history**
- View logs for:
  - Trigger
  - HTTP request/response
  - GPT output

### Verify GPT’s Response

GPT should return a structured reply like:

```json
{
  "Observations": "Login from known Tor node",
  "Findings": ["User: admin@domain.com", "Outside business hours"],
  "Recommendations": ["Enforce MFA", "Add IP to watchlist"]
}
