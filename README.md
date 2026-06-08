# 🥇 Gold-Signal-Bot — AI-Powered XAUUSD Trading System

[![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat&logo=python)](https://python.org)
[![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?style=flat&logo=telegram)](https://core.telegram.org/bots)
[![Claude AI](https://img.shields.io/badge/Claude-Sonnet-orange?style=flat)](https://anthropic.com)
[![DigitalOcean](https://img.shields.io/badge/DigitalOcean-Live-0080FF?style=flat&logo=digitalocean)](https://digitalocean.com)
[![Status](https://img.shields.io/badge/Status-Live%20Production-brightgreen?style=flat)]()

> Autonomous 24/7 trading bot for XAUUSD (Gold/USD) — powered by Claude AI, MetaTrader 5, and deployed on DigitalOcean Ubuntu server.

---

## 📸 Live System

```
root@trading-bot-fra:~# pm2 list

┌─────┬────────────────┬──────┬──────┬─────────┬─────┬────────┐
│ id  │ name           │ mode │  ↺   │ status  │ cpu │ memory │
├─────┼────────────────┼──────┼──────┼─────────┼─────┼────────┤
│  7  │ fikri-ai       │ fork │  2   │ online  │ 0%  │ 65.1mb │
│  0  │ telegram-panel │ fork │  77  │ online  │ 0%  │ 41.5mb │
└─────┴────────────────┴──────┴──────┴─────────┴─────┴────────┘

Server: Ubuntu 24.04.3 LTS | Python 3.12.3 | PM2 7.0.1
IP: 207.154.221.67 | Uptime: 24/7
```

```
08:31:05 [GOLD] ATR=13.00  SL=19.50  TP=39.00  Spread=0.04
08:31:05 [GOLD] Asking Claude...
08:31:10 [GOLD] ACTION: SELL   CONFIDENCE: 52
08:31:10 [GOLD] HOLD conf=52% (min 70%) — Risk Guard Active
09:16:04 [GOLD] ATR=9.69  SL=14.53  TP=29.07
09:16:04 [GOLD] Asking Claude...
09:16:10 [GOLD] ACTION: BUY   CONFIDENCE: 84
09:16:10 [GOLD] ✅ TRADE EXECUTED — BUY 0.01 XAUUSD
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│           DigitalOcean Ubuntu Server         │
│                                             │
│  ┌──────────────┐    ┌──────────────────┐  │
│  │  fikri-ai    │    │  telegram-panel  │  │
│  │  (PM2 fork)  │    │  (PM2 fork)      │  │
│  └──────┬───────┘    └────────┬─────────┘  │
│         │                     │             │
└─────────┼─────────────────────┼─────────────┘
          │                     │
    ┌─────▼──────┐        ┌─────▼──────┐
    │ MetaAPI    │        │  Telegram  │
    │ Cloud SDK  │        │  Bot API   │
    │ (MT5 Live) │        │ 9 buttons  │
    └─────┬──────┘        └────────────┘
          │
    ┌─────▼──────┐
    │  Claude AI │
    │  Sonnet    │
    │ (Anthropic)│
    └────────────┘
```

---

## ⚙️ Core Components

### 1. Trading Bot (`golden_edge_bot_v3.3.py`) — 791 lines
- Async Python with `asyncio/await`
- Connects to MT5 via MetaAPI Cloud SDK v29+
- Fetches OHLCV data: **D1, H4, H1, M15** simultaneously
- Calculates: **EMA20, EMA50, ATR** per timeframe
- Builds structured AI prompt with full market context
- Sends to **Claude Sonnet** → receives BUY/SELL/HOLD + confidence
- Executes trades only when confidence ≥ 70%

### 2. Telegram Control Panel (`telegram_panel_v2.py`)
```
📱 9-Button Interface:
[📊 Status]  [📋 Logs]   [💰 Balance]
[🧠 Memory]  [▶️ Start]  [⏹ Stop]
[🔄 Restart] [📄 Full Logs] [❌ Close Trade]
```

### 3. Risk Management Engine
| Rule | Value |
|------|-------|
| Stop Loss | 1.5 × ATR |
| Take Profit | 3.0 × ATR (1:2 RR) |
| Daily Loss Cap | Auto-stop on limit hit |
| Consecutive Loss Guard | Stop after 2 losses |
| Breakeven | Move SL to entry at +1×ATR |
| News Blackout | Auto-pause on NFP/FOMC |
| Min Confidence | 70% (Claude AI) |

### 4. Multi-Timeframe Analysis
```python
timeframes = {
    "D1":  {"ema20": 2698.4, "ema50": 2651.2, "atr": 28.4, "trend": "BULLISH"},
    "H4":  {"ema20": 2743.1, "ema50": 2721.8, "atr": 14.2, "trend": "BULLISH"},
    "H1":  {"ema20": 2751.6, "ema50": 2748.3, "atr": 8.7,  "trend": "RANGING"},
    "M15": {"ema20": 2749.2, "ema50": 2750.1, "atr": 4.1,  "trend": "BEARISH"},
}
# → All fed into Claude AI prompt for final decision
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.12 (async/await) |
| Trading API | MetaAPI Cloud SDK v29+ |
| AI Brain | Claude Sonnet (Anthropic) via OpenRouter |
| Notifications | Telegram Bot API (python-telegram-bot) |
| Server | DigitalOcean Droplet — Ubuntu 24.04 LTS |
| Process Manager | PM2 (auto-restart, crash recovery) |
| News Feed | BBC / Reuters RSS parsing |
| Broker | CFI — MetaTrader 5 live account |

---

## 📦 Installation

```bash
# Clone the repo
git clone https://github.com/fikrigoldbot/Gold-Signal-Bot.git
cd Gold-Signal-Bot

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your API keys

# Run with PM2
pm2 start golden_edge_bot_v3.3.py --name fikri-ai --interpreter python3
pm2 start telegram_panel_v2.py --name telegram-panel --interpreter python3
pm2 save
```

---

## 🔑 Environment Variables

```env
# MetaAPI Cloud
META_API_TOKEN=your_metaapi_token
ACCOUNT_ID=your_mt5_account_id

# AI
OPENROUTER_API_KEY=your_openrouter_key

# Telegram
TELEGRAM_BOT_TOKEN=your_bot_token
TELEGRAM_CHAT_ID=your_chat_id
```

---

## 📊 Performance

| Metric | Value |
|--------|-------|
| First trade P&L | +$13.92 |
| Risk/Reward | 1:2 |
| Min confidence threshold | 70% |
| Uptime | 24/7 |
| Version | v3.3 |
| Lines of code | 791 |

---

## 🔄 Version History

| Version | Changes |
|---------|---------|
| v3.3 | Added breakeven + trailing stop |
| v3.2 | NFP/FOMC news blackout |
| v3.1 | Multi-timeframe analysis |

---

## 👤 Author

**Fekri Ismail Abdel Hamid**
- 🌐 GitHub: [@fikrigoldbot](https://github.com/fikrigoldbot)
- 💼 Upwork: [Profile](https://www.upwork.com/freelancers/~012fc9851c97860336)
- 📧 Email: fikriteacher@gmail.com
- 📍 Tyre, Lebanon (UTC+3)

---

## ⚠️ Disclaimer

This bot is for educational and personal use. Trading involves significant risk. Past performance does not guarantee future results. Use at your own risk.

---

*Built with Python · Claude AI · MetaTrader 5 · DigitalOcean · June 2026*
