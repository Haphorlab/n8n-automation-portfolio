# 🤖 Crypto Futures Signal Bot

An AI-powered Telegram bot that analyzes cryptocurrency markets in real-time and provides actionable LONG/SHORT trading signals using a multi-agent architecture.

## 🎯 Overview

This automated trading signal bot helps traders make informed decisions by analyzing crypto futures markets across multiple timeframes and perspectives. It uses three specialized AI agents that work together to provide comprehensive trading signals via Telegram.

### Key Features

- 🔄 **Real-time Market Analysis** - Fetches live price data from Bybit exchange
- 🧠 **Multi-Agent AI System** - Three specialized GPT-4 agents analyze markets from different perspectives
- 📊 **Technical Indicators** - Calculates EMA, RSI, support/resistance, and volume analysis
- 💬 **Telegram Interface** - Easy-to-use bot that responds to coin tickers instantly
- 📈 **Complete Trading Signals** - Provides entry price, take profit levels, and stop loss
- ⚡ **Fast Response** - Automated workflow processes signals in seconds

## 🏗️ Architecture

### Multi-Agent System

The bot employs three specialized AI agents that analyze the market independently:

1. **Scalper Agent** - Short-term (15min-1hour) focus
   - Analyzes RSI levels for quick reversals
   - Monitors volume spikes
   - Identifies momentum opportunities

2. **Swing Trader Agent** - Medium-term (4-24 hours) focus
   - Evaluates trend strength using EMAs
   - Assesses support/resistance levels
   - Considers broader market context

3. **Risk Manager Agent** - Risk assessment specialist
   - Validates signal quality
   - Calculates position sizing
   - Sets stop loss and take profit levels

### Workflow Pipeline

```
User sends ticker (BTC, ETH, etc.)
          ↓
Extract & Validate Coin
          ↓
Fetch Live Market Data (Bybit API)
          ↓
Calculate Technical Indicators
    (EMA20, EMA50, RSI, Volume, S/R)
          ↓
    Build Analysis Context
          ↓
    ╔═══════════════════════╗
    ║   3 AI Agents Run     ║
    ║   Parallel Analysis   ║
    ╚═══════════════════════╝
          ↓
    Merge & Extract Signals
          ↓
    Moderator Agent (Final Decision)
          ↓
    Send Formatted Signal to User
```

## 📊 Technical Indicators Calculated

- **EMA (20, 50)** - Trend direction and momentum
- **RSI (14)** - Overbought/oversold conditions
- **Support/Resistance** - Key price levels from 20-period analysis
- **Volume Ratio** - Current vs average volume (20-period)
- **Price Changes** - 1-hour and 4-hour percentage moves

## 💡 How It Works

### 1. User Interaction
```
User: BTC
Bot: [Analyzes and responds with signal]
```

### 2. Market Data Collection
- Fetches 100 candles of 15-minute data from Bybit
- Normalizes data into OHLCV format
- Calculates technical indicators

### 3. AI Analysis
Each agent receives:
- Current price and indicators
- Trend analysis
- Recent price action
- Volume context

### 4. Signal Generation
The moderator agent synthesizes all three perspectives to provide:
- **Signal Direction**: LONG or SHORT
- **Confidence Level**: Low/Medium/High
- **Entry Price**: Recommended entry point
- **Take Profit Levels**: TP1, TP2, TP3
- **Stop Loss**: Risk management level
- **Reasoning**: Clear explanation of the signal

## 🚀 Example Output

```
🚀 BTC/USDT FUTURES SIGNAL

📊 SIGNAL: LONG
💪 CONFIDENCE: High

💰 Entry: $45,230

🎯 Take Profits:
TP1: $45,680 (1%)
TP2: $46,130 (2%)
TP3: $46,805 (3.5%)

🛑 Stop Loss: $44,780 (-1%)

📈 REASONING:
Strong bullish momentum with price above both EMAs. 
RSI at 58 shows room for upside. Volume spike 
confirms buying pressure. Risk/reward: 1:2.5

⚠️ Manage your risk. Never invest more than you can afford to lose.
```

## 🛠️ Tech Stack

- **n8n** - Workflow automation platform
- **OpenAI GPT-4** - AI analysis agents
- **Bybit API** - Real-time market data
- **Telegram Bot API** - User interface
- **Node.js** - JavaScript runtime for calculations

## 📋 Prerequisites

- n8n instance (self-hosted or cloud)
- OpenAI API key
- Telegram Bot Token
- Bybit API access (no authentication needed for market data)

## ⚙️ Setup Instructions

### 1. Create Telegram Bot

1. Message [@BotFather](https://t.me/botfather) on Telegram
2. Create new bot with `/newbot`
3. Save your bot token

### 2. Get OpenAI API Key

1. Go to [OpenAI Platform](https://platform.openai.com/)
2. Create API key
3. Ensure you have GPT-4 access

### 3. Import Workflow to n8n

1. Download `workflow.json` from this repo
2. In n8n, go to Workflows → Import
3. Select the downloaded file

### 4. Configure Credentials

Add the following credentials in n8n:
- **Telegram Bot**: Your bot token from BotFather
- **OpenAI**: Your OpenAI API key

### 5. Activate Workflow

1. Click "Active" toggle in n8n
2. Test by messaging your bot on Telegram

## 📱 Usage

Simply send a coin ticker to your Telegram bot:

```
BTC
ETH
SOL
AVAX
```

The bot will respond with a complete trading signal within seconds.

For help, send:
```
/start
help
```

## 🎓 Supported Coins

Currently supports major cryptocurrencies including:
- Bitcoin (BTC)
- Ethereum (ETH)
- Solana (SOL)
- Binance Coin (BNB)
- XRP (Ripple)
- Cardano (ADA)
- Polygon (MATIC)
- Avalanche (AVAX)
- And many more...

*The bot can be easily extended to support any coin traded on Bybit.*

## ⚠️ Disclaimer

**This bot is for educational and informational purposes only.**

- Not financial advice
- Past performance doesn't guarantee future results
- Crypto trading carries significant risk
- Always do your own research (DYOR)
- Only trade with money you can afford to lose
- The author is not responsible for any trading losses

## 🔮 Future Enhancements

- [ ] Add more exchanges (Binance, OKX)
- [ ] Implement backtesting capabilities
- [ ] Historical win rate tracking
- [ ] Multi-timeframe analysis
- [ ] Sentiment analysis from social media
- [ ] Position tracking and portfolio management
- [ ] Custom alert preferences
- [ ] Web dashboard with charts


## 🙏 Acknowledgments

- Built with [n8n](https://n8n.io/)
- Powered by [OpenAI GPT-4](https://openai.com/)
- Market data from [Bybit](https://www.bybit.com/)

---

⭐ If you find this project useful, please star it on GitHub!

