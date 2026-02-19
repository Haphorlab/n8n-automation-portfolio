# 🔧 Detailed Setup Guide

This guide walks you through setting up the Crypto Futures Signal Bot from scratch.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Environment Setup](#environment-setup)
3. [Telegram Bot Configuration](#telegram-bot-configuration)
4. [OpenAI Setup](#openai-setup)
5. [n8n Installation](#n8n-installation)
6. [Workflow Import](#workflow-import)
7. [Credential Configuration](#credential-configuration)
8. [Testing](#testing)
9. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Accounts

- **Telegram Account** - For creating and testing the bot
- **OpenAI Account** - With API access and GPT-4 enabled
- **n8n Instance** - Self-hosted or cloud (n8n.cloud)

### Technical Requirements

- Node.js 18+ (if self-hosting n8n)
- Internet connection
- Basic understanding of APIs and webhooks

---

## Environment Setup

### Option 1: n8n Cloud (Easiest)

1. Go to [n8n.cloud](https://n8n.cloud/)
2. Sign up for free trial
3. Create new instance
4. Skip to [Telegram Bot Configuration](#telegram-bot-configuration)

### Option 2: Self-Hosted n8n

#### Using Docker (Recommended)

```bash
# Create directory for n8n data
mkdir -p ~/.n8n

# Run n8n container
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

#### Using npm

```bash
# Install n8n globally
npm install n8n -g

# Start n8n
n8n start
```

Access n8n at: `http://localhost:5678`

---

## Telegram Bot Configuration

### Step 1: Create Bot with BotFather

1. Open Telegram and search for **@BotFather**
2. Start chat and send `/newbot`
3. Choose a name (e.g., "Crypto Signal Bot")
4. Choose a username (must end in 'bot', e.g., `crypto_signal_bot`)
5. **Save the API token** - you'll need this!

Example token format: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`

### Step 2: Configure Bot Settings (Optional)

```
/setdescription - Set bot description
/setabouttext - Set about text
/setuserpic - Upload bot profile picture
```

### Step 3: Test Bot

1. Search for your bot username in Telegram
2. Click "Start"
3. Send a test message

---

## OpenAI Setup

### Step 1: Create API Key

1. Go to [OpenAI Platform](https://platform.openai.com/)
2. Sign in or create account
3. Navigate to **API Keys** section
4. Click **"Create new secret key"**
5. Name it (e.g., "Crypto Bot")
6. **Copy and save the key immediately** (you won't see it again!)

### Step 2: Verify GPT-4 Access

1. Go to **Settings → Limits**
2. Check if GPT-4 is available
3. If not, you may need to:
   - Add payment method
   - Wait for access (may take time)
   - Use GPT-3.5-turbo as alternative (less accurate)

### Step 3: Set Usage Limits (Recommended)

1. Go to **Settings → Limits**
2. Set monthly spending limit (e.g., $10-20/month)
3. Enable email notifications for usage alerts

---

## n8n Installation

### Create Account & Workspace

1. Open n8n (localhost:5678 or your cloud instance)
2. Create account (email + password)
3. Set up workspace name

---

## Workflow Import

### Step 1: Download Workflow

1. Download `workflow.json` from this GitHub repo
2. Save to your computer

### Step 2: Import to n8n

1. In n8n, click **"Workflows"** in sidebar
2. Click **"Import workflow"** button (top right)
3. Select your downloaded `workflow.json` file
4. Click **"Import"**

### Step 3: Review Workflow

The workflow should now appear with these nodes:
- Telegram Trigger
- Extract command
- If (conditional)
- HTTP Request (Bybit API)
- Calculate indicators
- Build context
- 3 AI agents (Scalper, Swing Trader, Risk Manager)
- Merge
- Extract signal
- Moderator
- Send message

---

## Credential Configuration

### Step 1: Add Telegram Credentials

1. Click on **"Telegram Trigger"** node
2. Click **"Credential to connect with"** dropdown
3. Select **"Create New Credential"**
4. Name: `Telegram Bot`
5. **Access Token**: Paste your bot token from BotFather
6. Click **"Save"**

Do the same for the **"Send a text message"** nodes.

### Step 2: Add OpenAI Credentials

1. Click on **"Scalper (Short-term)"** node
2. Click **"Credential to connect with"**
3. Select **"Create New Credential"**
4. Name: `OpenAI Account`
5. **API Key**: Paste your OpenAI API key
6. Click **"Save"**

**Important**: Apply this same credential to all AI agent nodes:
- Scalper (Short-term)
- Swing Trader (Medium-term)
- Risk Manager Agent
- Moderator

### Step 3: Verify All Credentials

Go through each node and ensure credentials are set:

✅ Telegram Trigger - Telegram credentials
✅ Send a text message (both nodes) - Telegram credentials
✅ All AI agent nodes - OpenAI credentials

---

## Testing

### Step 1: Activate Workflow

1. Click the **"Active"** toggle (top right)
2. Status should show as "Active"

### Step 2: Test with Telegram

1. Open Telegram
2. Find your bot
3. Send: `/start` or `help`
4. You should receive welcome message

### Step 3: Test Signal Generation

Send a coin ticker:
```
BTC
```

Expected response:
- Processing should take 5-15 seconds
- Should receive formatted trading signal
- Signal includes LONG/SHORT, entry, TPs, SL

### Step 4: Check n8n Executions

1. In n8n, go to **"Executions"** tab
2. You should see successful execution
3. Click to view detailed logs
4. Verify all nodes executed successfully

---

## Troubleshooting

### Bot Doesn't Respond

**Problem**: No response when messaging bot

**Solutions**:
1. Check workflow is **Active** (toggle on)
2. Verify Telegram credentials are correct
3. Check webhook is registered:
   - Click on Telegram Trigger node
   - Look for webhook URL
   - Test webhook URL in browser

### "Invalid Credentials" Error

**Problem**: Authentication errors

**Solutions**:
1. Re-create credentials from scratch
2. Ensure no extra spaces in API keys
3. Verify OpenAI has GPT-4 access
4. Check Telegram token is correct format

### OpenAI Rate Limit Errors

**Problem**: "Rate limit exceeded" from OpenAI

**Solutions**:
1. Wait a few minutes
2. Check OpenAI usage on platform
3. Upgrade OpenAI plan if needed
4. Add delays between AI agent calls

### "Coin Not Found" Error

**Problem**: Bot doesn't recognize coin ticker

**Solutions**:
1. Ensure ticker is supported (see coin map in Extract command node)
2. Try common tickers: BTC, ETH, SOL
3. Add new coins to coin map in code node

### Bybit API Errors

**Problem**: Can't fetch market data

**Solutions**:
1. Check coin symbol format (e.g., BTCUSDT)
2. Verify Bybit API is accessible
3. Check rate limits (100 requests per minute)
4. Ensure coin is traded on Bybit

### Wrong Signal Format

**Problem**: Response doesn't match expected format

**Solutions**:
1. Check all AI agents use latest system prompts
2. Verify GPT-4 model is selected (not 3.5)
3. Review moderator agent output format
4. Check for JSON parsing errors in logs

---

## Performance Optimization

### Reduce Costs

1. **Use GPT-3.5-turbo** for less critical agents
2. **Reduce max_tokens** in AI nodes (currently 300)
3. **Add caching** for repeated coin queries
4. **Limit concurrent users** if self-hosting

### Improve Response Speed

1. **Cache technical indicators** (5-minute refresh)
2. **Parallel AI agent execution** (already implemented)
3. **Use faster n8n hosting** (if self-hosted)
4. **Optimize code nodes** (remove unnecessary calculations)

---

## Advanced Configuration

### Add New Coins

Edit the **"Extract command"** code node:

```javascript
const coinMap = {
  "BTC":"BTC",
  "ETH":"ETH",
  // Add your coin here:
  "NEWCOIN":"NEWCOIN"
};
```

### Modify Signal Format

Edit the **"Moderator"** node system prompt:

```
Output format:
SIGNAL: LONG/SHORT
ENTRY: $price
TP1: $price (%)
...
```

### Change Timeframes

Edit the **"HTTP Request"** node URL:

```
interval=15  // Change to: 5, 15, 30, 60, 240
```

---

## Security Best Practices

1. **Never commit credentials** to GitHub
2. **Use environment variables** for secrets
3. **Limit bot access** (private group only)
4. **Monitor API usage** regularly
5. **Set spending limits** on OpenAI
6. **Regularly rotate API keys**
7. **Enable 2FA** on all accounts

---

## Support

### Getting Help

- **GitHub Issues**: Report bugs or request features
- **n8n Community**: [community.n8n.io](https://community.n8n.io)
- **n8n Docs**: [docs.n8n.io](https://docs.n8n.io)

### Useful Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [OpenAI API Reference](https://platform.openai.com/docs)
- [Bybit API Docs](https://bybit-exchange.github.io/docs/)

---

## Next Steps

Once your bot is running:

1. ✅ Test with multiple coins
2. ✅ Monitor OpenAI costs
3. ✅ Track signal accuracy
4. ✅ Gather user feedback
5. ✅ Iterate and improve

**Congratulations! Your crypto signal bot is now live! 🎉**
