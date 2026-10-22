# 🤖 Amazon Product Search Bot (n8n + Telegram + AI)

An n8n automation that lets you search for products on Amazon directly from Telegram. Send a message to the bot (for example, "best running shoes"), and an AI Agent understands your request, fetches live results from Amazon using SerpApi, and replies with the top picks in chat.

---

## 📌 Features

- Chat-based product search through a Telegram bot
- AI Agent that understands natural-language requests
- Live Amazon results (amazon.in) via SerpApi
- Automatic reply sent back to the user in Telegram
- No-code setup using n8n

---

## 🧰 Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation platform |
| Telegram Bot (BotFather) | User interface / chat channel |
| AI Agent (n8n) | Understands the message and decides what to search |
| [SerpApi](https://serpapi.com) | Fetches live Amazon search results |

---

## 🔄 Workflow Overview

```
Telegram Trigger  →  AI Agent (+ SerpApi Tool)  →  Telegram: Send Message
```

1. **Telegram Trigger:** listens for new messages sent to the bot.
2. **AI Agent:** reads the user's message and extracts what to search for.
3. **SerpApi Tool (Amazon Search):** called by the agent to get live product results.
4. **Telegram Send Message:** sends the results back to the same chat.

---

## ⚙️ Setup Guide

### 1. Create a Telegram Bot
1. Open Telegram and search for **@BotFather**.
2. Send `/newbot` and follow the prompts to name your bot.
3. Copy the **API token** that BotFather gives you.

### 2. Get a SerpApi Key
1. Sign up at [serpapi.com](https://serpapi.com).
2. Copy your **API key** from the dashboard.

### 3. Set Up n8n
1. Open your n8n instance and create a new workflow.
2. Add credentials:
   - **Telegram API:** paste your bot token.
   - **SerpApi:** paste your API key.
3. Add the following nodes:
   - **Telegram Trigger** (Trigger On: `Message`)
   - **AI Agent** (connect a chat model, and add **SerpApi → Amazon Search** as a Tool)
   - **Telegram → Send Message**

### 4. Configure the AI Agent
- Set the prompt to use the incoming Telegram message:
  ```
  {{ $json.message.text }}
  ```
- Set the Amazon domain in the SerpApi tool (for example, `amazon.in`).
- Let the agent fill the search query automatically ("From AI").

### 5. Configure the Reply
- **Chat ID:** `{{ $('Telegram Trigger').item.json.message.chat.id }}`
- **Text:** the AI Agent's output (`{{ $json.output }}`)

### 6. Activate the Workflow
Toggle the workflow to **Active**, then message your bot on Telegram.

---

## 💬 Example Usage

**You:** `find me a good water bottle`

**Bot:** Returns a list of top Amazon picks with titles, prices, and ratings.

---

## 🧪 Testing Tips

- A tool node (like SerpApi) can't be run alone without input from the AI Agent. When you click **Execute step** on it, n8n asks you to provide a sample value (for example, `iPhone 15 case`).
- To test the full flow, run the whole workflow and send a real message to your bot.
- Telegram allows only **one trigger per bot** at a time, so deactivate other workflows using the same bot.

---

## 📚 What I Learned

- How triggers, nodes, and tools work together in an n8n workflow
- The difference between a normal node and a tool node used by an AI Agent
- How to test tool nodes by providing sample inputs
- How to connect APIs and credentials securely
- How AI Agents can automate real-world tasks like product research

---
