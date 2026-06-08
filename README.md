```
 ██████╗  ██████╗ ██╗     ██████╗     ██████╗  ██████╗ ████████╗
██╔════╝ ██╔═══██╗██║     ██╔══██╗    ██╔══██╗██╔═══██╗╚══██╔══╝
██║  ███╗██║   ██║██║     ██║  ██║    ██████╔╝██║   ██║   ██║   
██║   ██║██║   ██║██║     ██║  ██║    ██╔══██╗██║   ██║   ██║   
╚██████╔╝╚██████╔╝███████╗██████╔╝    ██████╔╝╚██████╔╝   ██║   
 ╚═════╝  ╚═════╝ ╚══════╝╚═════╝     ╚═════╝  ╚═════╝    ╚═╝   
```

<div align="center">

# 🥇 AI-Powered XAUUSD Trading System

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Claude AI](https://img.shields.io/badge/Claude-Sonnet-FF6B35?style=for-the-badge)](https://anthropic.com)
[![Telegram](https://img.shields.io/badge/Telegram-Control_Panel-2CA5E0?style=for-the-badge&logo=telegram)](https://core.telegram.org/bots)
[![DigitalOcean](https://img.shields.io/badge/DigitalOcean-LIVE_24/7-0080FF?style=for-the-badge&logo=digitalocean)](https://digitalocean.com)
[![Status](https://img.shields.io/badge/Status-●_ONLINE-00E676?style=for-the-badge)]()

**Autonomous trading bot for Gold/USD — Claude AI brain, MetaTrader 5 execution, Telegram control**

*791 lines · v3.3 · Deployed on real money · First night: +$13.92*

</div>

---

## ⚡ Live Right Now

```bash
root@trading-bot-fra:~# pm2 list
┌─────┬────────────────┬──────┬────┬─────────┬─────┬────────┐
│ id  │ name           │ mode │ ↺  │ status  │ cpu │ memory │
├─────┼────────────────┼──────┼────┼─────────┼─────┼────────┤
│  7  │ fikri-ai       │ fork │  2 │ ONLINE  │ 0%  │ 65.1mb │  ← Trading Bot
│  0  │ telegram-panel │ fork │ 77 │ ONLINE  │ 0%  │ 41.5mb │  ← Control Panel
└─────┴────────────────┴──────┴────┴─────────┴─────┴────────┘
Ubuntu 24.04.3 LTS  |  Python 3.12.3  |  PM2 7.0.1  |  207.154.221.67
```

```bash
root@trading-bot-fra:~# tail -f /root/.pm2/logs/fikri-ai-out.log

09:16:04 [GOLD] ATR=9.69  SL=14.53  TP=29.07  Spread=0.04
09:16:04 [GOLD] Asking Claude...                          # → Claude AI thinking
09:16:10 [GOLD] ACTION: BUY   CONFIDENCE: 84             # → Decision made
09:16:10 [GOLD] ✅ TRADE EXECUTED — BUY 0.01 XAUUSD     # → Order fired
                              ↑
                    Only executes if confidence ≥ 70%
```

---

## 🧠 How It Thinks

```
Every 15 minutes:

MARKET DATA          AI ANALYSIS              EXECUTION
──────────           ────────────             ──────────
D1  candles   ──┐                         ┌── BUY  (conf ≥ 70%)
H4  candles   ──┤──→ EMA20/50/ATR ──→ 🤖 ──┤── SELL (conf ≥ 70%)
H1  candles   ──┤    Swing points    Claude──┤── HOLD (conf < 70%)
M15 candles   ──┘    Trend summary          └── BLOCK (risk guard)
                     News context
                     (BBC/Reuters RSS)
```

---

## 🛡️ Risk Rules — Never Broken

| Rule | Logic |
|------|-------|
| **Stop Loss** | `price - (1.5 × ATR)` — dynamic, not fixed |
| **Take Profit** | `price + (3.0 × ATR)` — 1:2 risk/reward |
| **Min Confidence** | Claude must say ≥ 70% or → HOLD |
| **Daily Loss Cap** | Hits limit → bot stops for the day |
| **2-Loss Guard** | Two losses in a row → cooling period |
| **Breakeven** | At +1×ATR → move SL to entry |
| **News Blackout** | NFP / FOMC → no trading |
| **Move Guard** | Big candle already moved → skip entry |

---

## 📱 Telegram Control Panel

> Control the entire bot from your phone — anywhere in the world

```
┌─────────────────────────────────┐
│  🤖 Gold Bot Control Panel      │
├─────────────────────────────────┤
│  [📊 Status]  [📋 Logs]        │
│  [💰 Balance] [🧠 Memory]      │
│  [▶️ Start]   [⏹️ Stop]        │
│  [🔄 Restart] [📄 Full Logs]   │
│              [❌ Close Trade]   │
└─────────────────────────────────┘

✅ Start/stop bot remotely
✅ View live logs & balance  
✅ Close open trades instantly
✅ Deploy new code via Telegram
✅ Works from any device worldwide
```

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────┐
│          DigitalOcean Ubuntu 24.04            │
│                                              │
│   ┌─────────────────┐  ┌──────────────────┐ │
│   │   fikri-ai      │  │  telegram-panel  │ │
│   │   PM2 | fork    │  │   PM2 | fork     │ │
│   │   65.1 MB RAM   │  │   41.5 MB RAM    │ │
│   └────────┬────────┘  └────────┬─────────┘ │
│            │                    │            │
└────────────┼────────────────────┼────────────┘
             │                    │
    ┌────────▼────────┐  ┌───────▼────────┐
    │  MetaAPI Cloud  │  │  Telegram API  │
    │  MT5 WebSocket  │  │  Bot Commands  │
    │  Live Account   │  │  Inline Keys   │
    └────────┬────────┘  └────────────────┘
             │
    ┌────────▼────────┐
    │   Claude AI     │
    │   Sonnet 4.5    │
    │   via OpenRouter│
    └─────────────────┘
```

---

## ⚙️ Technical Stack

```python
STACK = {
    "core":      "Python 3.12 — asyncio/await (fully async)",
    "trading":   "MetaAPI Cloud SDK v29+ (MT5 REST + WebSocket)",
    "ai":        "Claude Sonnet via OpenRouter API",
    "alerts":    "python-telegram-bot (async)",
    "server":    "DigitalOcean Droplet — Ubuntu 24.04 LTS",
    "process":   "PM2 — auto-restart, crash recovery, logs",
    "news":      "BBC + Reuters RSS feeds (xml.etree parsing)",
    "broker":    "CFI — MetaTrader 5 live account",
    "tools":     ["httpx", "asyncio", "dotenv", "subprocess"],
}
```

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/fikrigoldbot/Gold-Signal-Bot.git
cd Gold-Signal-Bot

# 2. Install
pip install -r requirements.txt

# 3. Configure
cp .env.example .env
nano .env  # add your API keys

# 4. Deploy
pm2 start golden_edge_bot_v3.3.py --name fikri-ai --interpreter python3
pm2 start telegram_panel_v2.py --name telegram-panel --interpreter python3
pm2 save && pm2 startup
```

```env
# .env
META_API_TOKEN=your_metaapi_token
ACCOUNT_ID=your_mt5_account_id
OPENROUTER_API_KEY=your_openrouter_key
TELEGRAM_BOT_TOKEN=your_bot_token
TELEGRAM_CHAT_ID=your_chat_id
```

---

## 📊 Stats

<div align="center">

| 📈 First Trade | ⚖️ Risk/Reward | 🎯 Min Confidence | 🔄 Uptime | 📝 Lines |
|:--------------:|:--------------:|:-----------------:|:---------:|:--------:|
| **+$13.92** | **1 : 2** | **70%** | **24/7** | **791** |

</div>

---

## 🔄 Changelog

```
v3.3  ████████████  Breakeven + trailing stop automation
v3.2  ████████░░░░  NFP/FOMC news blackout system  
v3.1  ████░░░░░░░░  Multi-timeframe D1/H4/H1/M15 analysis
```

---

<div align="center">

## 👤 Built by Fekri

**Python Bot Engineer · Lebanon · Available for hire**

[![GitHub](https://img.shields.io/badge/GitHub-fikrigoldbot-181717?style=flat&logo=github)](https://github.com/fikrigoldbot)
[![Upwork](https://img.shields.io/badge/Upwork-Hire_Me-brightgreen?style=flat&logo=upwork)](https://www.upwork.com/freelancers/~012fc9851c97860336)
[![Email](https://img.shields.io/badge/Email-fikriteacher@gmail.com-EA4335?style=flat&logo=gmail)](mailto:fikriteacher@gmail.com)

*⚠️ For educational purposes. Trading involves risk. Use responsibly.*

</div>
