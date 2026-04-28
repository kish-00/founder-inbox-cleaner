# Founder Inbox Cleaner

An n8n workflow that automatically classifies incoming emails by priority using ChatGPT and applies labels/archiving actions.

## Features

- Runs every 15 minutes
- Fetches unread emails from Gmail
- Uses OpenAI ChatGPT to classify priority: **HIGH**, **MEDIUM**, or **LOW**
- Applies Gmail labels:
  - `HighPriority` — urgent emails, stays in inbox
  - `MediumPriority` — important but not urgent, stays in inbox
  - `LowPriority` — low priority, gets archived
- Marks all processed emails as read

## Prerequisites

1. **n8n instance** (self-hosted or n8n.cloud)
2. **Gmail API credentials** (OAuth2)
3. **OpenAI API key** (ChatGPT)

## Setup

### 1. Gmail Integration

- In n8n, go to **Credentials** → **Add credential**
- Select **Gmail OAuth2**
- Follow the Google OAuth flow to grant access to your Gmail account
- Ensure the Gmail API is enabled in your Google Cloud project

### 2. OpenAI Integration

- Add a new credential in n8n
- Select **OpenAI API**
- Enter your OpenAI API key

### 3. Create Gmail Labels (Optional)

The workflow expects these labels to exist in Gmail:
- `HighPriority`
- `MediumPriority`
- `LowPriority`

You can create them manually in Gmail settings, or the workflow will attempt to create them (requires additional API permissions).

### 4. Import Workflow

1. Download `workflow.json` from this repository
2. In n8n, click **Import** → Upload the JSON file
3. Click **Activate** to enable the workflow

### 5. Configure Credentials

When prompted, select:
- Your Gmail OAuth2 credential
- Your OpenAI API credential

## How It Works

1. **Schedule Trigger** — fires every 15 minutes
2. **Gmail: Get Unread Emails** — retrieves up to 50 most recent unread messages
3. **Classify Priority with ChatGPT** — sends email subject + body to ChatGPT with system prompt to classify priority
4. **Decide Action** — code node parses ChatGPT's response and decides:
   - Add appropriate label
   - Archive if low priority
   - Mark as read
5. **Gmail actions** — apply label, archive thread, mark as read (executed in parallel)

## Customization

- **Interval**: Change the Schedule Trigger node (default 15 minutes)
- **Max emails**: Adjust `maxMessages` in Gmail node (default 50)
- **Classification prompt**: Edit the `systemMessage` in the ChatGPT node to fine-tune priority criteria

## Costs

- n8n: Free (self-hosted) or paid (n8n.cloud)
- Gmail API: Free within quotas
- OpenAI ChatGPT: ~$0.002 per 1K tokens (very low for short emails)

## License

MIT
