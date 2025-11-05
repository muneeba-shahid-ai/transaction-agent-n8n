# transaction-agent-n8n
Automated WhatsApp-based AI Assistant built with n8n, integrating OpenAI GPT, Google Sheets, and conditional workflow logic. This workflow automatically handles WhatsApp messages, validates permissions, interacts with GPT for intelligent responses, and updates Google Sheets for user data and transaction tracking.


````markdown
# 🤖 n8n WhatsApp AI Automation Workflow  
### _AI-Driven WhatsApp Assistant for Smart Messaging, AI Responses & Google Sheets Integration_

[![n8n](https://img.shields.io/badge/Built%20with-n8n-orange?logo=n8n)](https://n8n.io/)
[![OpenAI GPT](https://img.shields.io/badge/AI%20Model-GPT--4.1--mini-blue?logo=openai)](https://platform.openai.com/)
[![LangChain](https://img.shields.io/badge/AI%20Framework-LangChain-green?logo=python)](https://www.langchain.com/)
[![Google Sheets](https://img.shields.io/badge/Integration-Google%20Sheets-lightgrey?logo=googlesheets)](https://developers.google.com/sheets)
[![License: MIT](https://img.shields.io/badge/License-MIT-success.svg)](LICENSE)

---

## 🧭 Overview

This project is a fully automated **n8n workflow** that connects **WhatsApp**, **OpenAI GPT**, and **Google Sheets** to enable intelligent, automated communication and data logging.  

It processes incoming WhatsApp messages, validates permissions, uses AI to interpret intent, and records user and transaction data in Google Sheets.  
Perfect for automating customer service, data collection, or internal workflow management.

---

## 🧱 System Architecture

```plaintext
┌─────────────────────────────┐
│       WhatsApp Trigger      │
│   (Incoming User Message)   │
└────────────┬────────────────┘
             ↓
┌─────────────────────────────┐
│   Python Node (Get Date)    │
│   → Fetches current date    │
└────────────┬────────────────┘
             ↓
┌─────────────────────────────┐
│ JS Node (Specific Users)    │
│ → Validate & tag users      │
└────────────┬────────────────┘
             ↓
┌─────────────────────────────┐
│     IF Node (isAllowed)     │
│ → Check user permissions    │
└──────┬──────────┬───────────┘
       │          │
   [TRUE]      [FALSE]
       │          │
       ↓          ↓
┌──────────────┐  ┌────────────────────┐
│ AI Agent     │  │ WhatsApp Decline   │
│ (LangChain)  │  │ “Request declined” │
└────┬─────────┘  └────────────────────┘
     ↓
┌─────────────────────────────┐
│ WhatsApp Response (AI Text) │
│ → Sends contextual reply    │
└─────────────────────────────┘
````

---

## ✨ Key Features

### 💬 WhatsApp Automation

* Real-time message triggers using WhatsApp Cloud API.
* Automatically sends intelligent responses.

### 🧠 AI-Powered Logic

* Uses **OpenAI GPT-4.1-mini** via **LangChain Agent** for contextual reasoning.
* Interprets user requests and dynamically decides workflow actions.

### 📊 Google Sheets Integration

* Reads and writes structured data:

  * **User Data Sheet** for user profiles.
  * **Transactions Sheet** for deposits, withdrawals, and approvals.
* Uses multiple **Google Service Accounts** for separated datasets.

### ⚖️ Conditional Access

* Implements a permission check (`isAllowed`) before processing actions.
* Sends automatic “Request Declined” replies to unauthorized users.

### 🧮 Data Enrichment

* Python node retrieves the current date.
* JavaScript node modifies and enriches message payloads.

### 🧵 Conversational Memory

* Maintains context between user messages with **LangChain’s memory buffer** for coherent replies.

---

## 🧩 Components Overview

| Component                    | Purpose                                      |
| ---------------------------- | -------------------------------------------- |
| **WhatsApp Trigger**         | Starts the workflow on new WhatsApp messages |
| **Python Code (Date)**       | Captures current system date                 |
| **JS Code (Specific Users)** | Applies filters and access logic             |
| **IF Node**                  | Validates authorization (`isAllowed`)        |
| **AI Agent (LangChain)**     | Runs GPT-based logic and connects to tools   |
| **Google Sheets Tools**      | Read/append user and transaction data        |
| **WhatsApp Send Nodes**      | Deliver AI or rejection messages to the user |

---

## ⚙️ Setup Guide

### 1️⃣ Prerequisites

* [n8n](https://docs.n8n.io/hosting/installation/) (v1.80 or newer)
* WhatsApp Cloud API credentials
* OpenAI API key
* Google Cloud Service Accounts for Sheets access

### 2️⃣ Import the Workflow

1. Open your **n8n Editor UI**
2. Click **Import Workflow**
3. Paste the JSON file from this repository

### 3️⃣ Configure Credentials

Set up the following in **n8n > Credentials**:

| Credential                | Purpose                      |
| ------------------------- | ---------------------------- |
| `WhatsApp OAuth Account`  | For message triggers         |
| `WhatsApp Account`        | For sending WhatsApp replies |
| `OpenAI Account`          | For AI message generation    |
| `Google Sheets Account`   | For user data operations     |
| `Google Sheets Account 2` | For transaction logging      |

### 4️⃣ Activate

Once all credentials are set, toggle the workflow to **Active**.
Messages received on your WhatsApp number will now trigger the workflow.

---

## 📊 Google Sheets Structure

### 🧾 User Data Sheet

| Column         | Description              |
| -------------- | ------------------------ |
| Name           | User’s full name         |
| Number         | WhatsApp phone number    |
| Country        | User’s country           |
| Family_Members | Number of family members |

### 💰 Transactions Sheet

| Column               | Description            |
| -------------------- | ---------------------- |
| Name                 | Associated user name   |
| Amount               | Transaction amount     |
| isDeposited          | Boolean for deposit    |
| isWithdrawal         | Boolean for withdrawal |
| Reason of Withdrawal | Purpose of transaction |
| Approval             | Approval status        |

---

## 🧠 Example Flow

**User Message:**

> “Withdraw $100 for groceries”

**Workflow Steps:**

1. WhatsApp Trigger captures message
2. `isAllowed = true` → Passes IF Node
3. AI interprets intent as withdrawal
4. Transaction logged in Google Sheets
5. WhatsApp sends reply:
   *“Your $100 withdrawal for groceries has been approved and recorded.”*

If unauthorized:

> *“Sorry, your request has been declined.”*

---

## 🧰 Environment Variables (Optional)

For containerized or CI/CD environments, define:

```bash
OPENAI_API_KEY=your_openai_api_key
WHATSAPP_API_TOKEN=your_whatsapp_access_token
GOOGLE_SERVICE_ACCOUNT_KEY=path_to_service_account.json
```

---

## 🧩 Customization Ideas

* Add Telegram or Slack integrations
* Include automated approval flows
* Change AI prompts or tone of messages
* Replace Google Sheets with a database (e.g., PostgreSQL, Airtable)
* Integrate custom logic for multi-step approvals

---

## 🧑‍💻 Tech Stack

| Tool                          | Role                           |
| ----------------------------- | ------------------------------ |
| **n8n**                       | Workflow automation engine     |
| **OpenAI GPT (gpt-4.1-mini)** | Natural language understanding |
| **LangChain**                 | AI orchestration framework     |
| **Google Sheets API**         | Data storage and access        |
| **Python / JavaScript Nodes** | Custom business logic          |
| **WhatsApp Cloud API**        | Real-time messaging            |

---

## 📄 License

This project is licensed under the **MIT License**.
You’re free to use, modify, and distribute it with attribution.

---

## 👨‍💻 Author

Built with ❤️ using:

* [n8n](https://n8n.io/)
* [OpenAI GPT](https://platform.openai.com/)
* [LangChain](https://www.langchain.com/)
* [Google Sheets API](https://developers.google.com/sheets)
* [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/)

---

## 🖼️ Workflow Visualization (Optional)

If you want to showcase your setup visually, you can add a diagram or screenshot here:

```markdown
![Workflow Diagram](./docs/workflow-diagram.png)
```

---

### ⭐ *If you find this project helpful, consider giving it a star on GitHub!*

```

---

Would you like me to include a **`docs/` folder structure suggestion** (for screenshots, credentials template, and setup diagrams)? It would make your repository look even more professional and enterprise-ready.
```
