# 🤖 AI-Powered Cryptonaute Engager — Mistral AI + Python + MetaTrader 5

> Monitors any Telegram signal channel, uses **Mistral AI** to parse signals in any format,
> and automatically places trades on MetaTrader 5 with stop loss and take profit in under 3 seconds.

---

## ❓ What Is This?

A free, open-source bot that:

1. **Monitors** any Telegram signal channel you're subscribed to in real time
2. **Sends** each message to **Mistral AI** which reads and understands the signal — no matter how it's formatted
3. **Places** the trade on **MetaTrader 5** automatically with the correct stop loss and take profit
4. **Displays** everything in a live Rich terminal dashboard

---

## 🧠 Why Mistral AI Instead of Regex?

Most signal copiers use hardcoded regex patterns to parse signals. The moment a signal provider
changes their format — even slightly — the copier breaks.

This bot uses **Mistral AI** to read signals the way a human would. All of these formats work:

```
GOLD BUY NOW
Entry: 4718
SL: 4700
TP1: 4730
TP2: 4750
```

```
XAUUSD SELL NOW @ 4735
TP : 4720 / TP : 4710
SL : 4750
```

```
Buy gold at market, stop loss 4690, take profit 4725 and 4740
```

```
Close all gold trades
```

And if the channel posts commentary or trade updates — Mistral knows it's not a signal and skips it.

---

## ⚡ Features

- **Mistral AI signal parsing** — works with any signal format from any provider
- **All 6 order types** — market buy/sell, buy/sell limit, buy/sell stop
- **Multiple take profits** — splits lot across TP1, TP2, TP3 automatically
- **Close signals** — detects "close all" instructions and closes positions
- **Live Rich dashboard** — connection status, signals, positions with P&L, trade history
- **JSON trade logging** — every trade saved for review
- **Real-time monitoring** — catches signals instantly via Telethon (not polling)

---

## 📁 Project Structure

```
signal_copier/
├── config.py              # All settings and environment variables
├── signal_listener.py     # Telethon client — monitors Telegram channel
├── signal_parser.py       # Mistral AI signal parser — the brain
├── mt5_executor.py        # MetaTrader 5 trade execution
├── dashboard.py           # Rich terminal dashboard
├── trade_logger.py        # JSON trade logging
├── main.py                # Entry point — ties everything together
├── .env.example           # Template — rename to .env and fill in
└── requirements.txt       # Python packages
```

---

## 🔧 Requirements

- **Python 3.12** (not 3.13/3.14 — MT5 library doesn't support them yet)
- **MetaTrader 5** installed and logged into your broker account
- **Telegram account** subscribed to the signal channel
- **Telegram API credentials** — free from https://my.telegram.org
- **Mistral API key** — from https://console.mistral.ai

---

## 🚀 Setup

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Get your API keys

| Key | Where to get it | Cost |
|-----|-----------------|------|
| Telegram API ID & Hash | https://my.telegram.org | Free |
| Telegram Phone Number | Your registered number | — |
| MetaTrader 5 Credentials | Your broker | Free (demo account) |
| Mistral API Key | https://console.mistral.ai | Pay-as-you-go |

### 3. Create your `.env` file

Rename `.env.example` to `.env` and fill in your values:

```env
TELEGRAM_API_ID=your_telegram_api_id
TELEGRAM_API_HASH=your_telegram_api_hash
TELEGRAM_PHONE=+your_phone_number

SIGNAL_CHANNEL=Forex

MT5_ACCOUNT=your_mt5_account_number
MT5_PASSWORD=your_mt5_password
MT5_SERVER=your_broker_server

MISTRAL_API_KEY=your_mistral_api_key
```

### 4. Run the bot

Make sure MetaTrader 5 is open and logged in, then:

```bash
python main.py
```

On first run, Telethon asks for your phone number and a Telegram verification code.
After that it saves a session file and connects automatically.

---

## 🔄 How It Works

```
Signal posted in Telegram channel
        │
        ▼
Telethon catches it instantly (real-time)
        │
        ▼
Message sent to Mistral AI for parsing
        │
        ▼
Mistral returns: direction, entry, SL, TP[], order type
        │
        ▼
Is it a real signal? ──No──▶ Skip, keep listening
        │
       Yes
        │
        ▼
MetaTrader 5 places the trade (splits lot across TPs)
        │
        ▼
Dashboard updates, trade logged to JSON
```

---

## ⚙️ Mistral Model Options

In `config.py`, you can change `MISTRAL_MODEL`:

| Model | Speed | Cost | Best for |
|-------|-------|------|----------|
| `mistral-large-latest` | Medium | Higher | Best accuracy (recommended) |
| `mistral-small-latest` | Fast | Lower | Good balance |
| `open-mistral-7b` | Fastest | Lowest | Testing only |

---

## ⚠️ Important Notes

- **Always test with a small lot size first** (0.01) before scaling up
- **Not all signal providers are profitable** — the bot executes faster, it doesn't make bad signals good
- **Protect your session file** — the `.session` file gives access to your Telegram account. Never share it or upload it to GitHub
- **Python 3.12 only** — the MetaTrader 5 library does not work with Python 3.13 or 3.14

---

## ⚖️ Disclaimer

This project is for **educational purposes only**. Nothing in this repository constitutes
financial advice. Always use a demo account for testing. Trade at your own risk.

---

**Built with ❤️ by Bonnahin**
