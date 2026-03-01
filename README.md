# Telegram-Appointment-Assistant
<img width="1427" height="504" alt="image" src="https://github.com/user-attachments/assets/7cedca2c-33a3-4443-95a3-8e28944e3a48" />


# Telegram Appointment Assistant (n8n Workflow)

## Overview

This n8n workflow acts as an AI-powered Telegram assistant that can handle text and voice messages, manage contacts, send emails, and book calendar events. It uses Google Gemini as the conversational brain and integrates with Google services to perform real actions.

### High-Level Flow

User sends message → Detect message type → (Transcribe if voice) → AI Agent processes intent → Execute action (reply / email / book event)

---

## Features

* Handles **Telegram text messages**
* Handles **Telegram voice messages** (automatic transcription)
* AI-powered conversation using **Google Gemini**
* Can:

  * Reply to user messages
  * Search contacts
  * Send emails
  * Book calendar events
* Uses memory to maintain conversation context
* Modular tool-based AI agent architecture

---

## Workflow Architecture (Node-by-Node)

1. **Telegram Trigger** – Listens for incoming messages
2. **Send a Text Message** – Optional acknowledgment or intermediate reply
3. **Switch (Rules Mode)** – Routes based on message type (text vs voice)
4. **Telegram Voice Message (Get File)** – Retrieves voice file
5. **Transcribe a Recording** – Converts audio to text
6. **AI Agent** – Central intelligence that:

   * Uses Google Gemini Chat Model
   * Maintains memory
   * Uses connected tools
7. **Tools Connected to AI Agent:**

   * Google Gemini Chat Model (LLM)
   * Simple Memory (conversation context)
   * Get Contacts (search records)
   * Send Email
   * Book Event (create calendar event)
8. **Send a Text Message (Final Response)** – Sends AI-generated reply to Telegram user

---

## Prerequisites

### Required Accounts

* Telegram Bot (Bot Token from @BotFather)
* Google Gemini API
* Google Account (for email + calendar)
* Airtable/CRM (if using contact search tool)

### n8n Requirements

* n8n v1.x or later
* Telegram credentials configured
* Gemini API key configured
* Google credentials for:

  * Gmail
  * Google Calendar

---

## Setup Instructions

### 1. Create Telegram Bot

* Open Telegram → Search `@BotFather`
* Create new bot
* Copy Bot Token
* Add token to n8n Telegram credentials

### 2. Import Workflow

* Open n8n
* Import workflow JSON
* Connect credentials

### 3. Configure Switch Node

* Route:

  * Text → AI Agent directly
  * Voice → Transcription → AI Agent

### 4. Configure Transcription Node

* Ensure correct binary file mapping
* Confirm audio format compatibility

### 5. Configure AI Agent

The AI Agent should:

* Use Gemini Chat Model
* Enable Simple Memory
* Be connected to:

  * Contact search tool
  * Email tool
  * Calendar booking tool

Ensure the system prompt clearly defines assistant behavior:

* Professional tone
* Ask for missing details before booking
* Confirm before sending emails

### 6. Configure Tool Mapping

#### Send Email Tool

* Map:

  * To → extracted contact email
  * Subject → AI-generated subject
  * Body → AI-generated content

#### Book Event Tool

* Map:

  * Title
  * Start date/time
  * End date/time
  * Attendees

Use structured output format for reliable event creation.

---

## Example Use Cases

User:

> "Book a meeting with John tomorrow at 3 PM"

Assistant:

* Searches contact
* Confirms availability
* Books Google Calendar event
* Sends confirmation

User:

> (Voice message requesting email)

Assistant:

* Transcribes audio
* Extracts intent
* Sends email
* Confirms delivery

---

## Reliability & Best Practices

* Add retries to Gemini calls
* Limit concurrency if high usage expected
* Use structured output parser for event creation
* Validate date/time formats before booking
* Confirm critical actions (email sending, booking)

---

## Troubleshooting

### Voice Not Transcribing

* Confirm file mapping from Telegram node
* Ensure correct binary property
* Check audio format

### AI Not Using Tools

* Confirm tools are connected to AI Agent
* Ensure prompt allows tool usage
* Verify tool permissions

### Calendar Booking Fails

* Check Google Calendar credential permissions
* Validate ISO datetime format

### Email Sending Issues

* Verify Gmail OAuth
* Check quota limits

---

## Security Notes

* Do NOT commit Telegram Bot token
* Do NOT commit Gemini API key
* Use environment variables where possible
* Restrict bot access if used internally
