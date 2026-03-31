# TradingView Webhook Bot

A production-ready webhook handler that receives TradingView alerts and automatically executes orders on Binance (Spot) and Flattrade (NSE/BSE). Deploy on Google Cloud Run, Railway, or any Docker-compatible platform.

![GitHub contributors](https://img.shields.io/github/contributors/Rishav-raj-github/tradingview-webhook-bot)
![GitHub last commit](https://img.shields.io/github/last-commit/Rishav-raj-github/tradingview-webhook-bot)
![GitHub License](https://img.shields.io/github/license/Rishav-raj-github/tradingview-webhook-bot)
![Python](https://img.shields.io/badge/Python-3.8+-blue)
![Flask](https://img.shields.io/badge/Flask-2.3+-green)

## Features

- **Multi-Broker Support**: Execute on Binance (Spot/TESTNET) and Flattrade (Indian stocks)
- **TradingView Webhooks**: Receive real-time alerts via JSON POST requests
- **Google Cloud Run Ready**: Free tier deployment with 2M requests/month
- **Railway Deploy**: Procfile included for one-click Railway deployment
- **Docker Support**: Dockerfile included for containerized deployment
- **Health Check Endpoint**: `/health` endpoint for uptime monitoring
- **Error Handling**: Comprehensive error responses with broker-specific messages

## Architecture

```
TradingView Alert
       │
       ▼
┌─────────────────────┐
│   Flask Webhook     │
│   POST /webhook     │
└─────────────────────┘
       │
       ├──► Binance Spot (TESTNET / Live)
       └──► Flattrade (NSE/BSE Stocks)
```

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Rishav-raj-github/tradingview-webhook-bot.git
cd tradingview-webhook-bot
```

### 2. Set Environment Variables

Copy `.env.example` to `.env` and configure your API keys:

```bash
cp .env.example .env
```

Edit `.env`:

```
# Binance API Credentials
BINANCE_API_KEY=your_binance_api_key_here
BINANCE_API_SECRET=your_binance_api_secret_here
BINANCE_REAL_API_KEY=your_real_binance_key_here
BINANCE_TESTNET=true

# Flattrade API Credentials
FLATTRADE_API_KEY=your_flattrade_api_key_here
FLATTRADE_API_SECRET=your_flattrade_api_secret_here
FLATTRADE_USER_ID=your_flattrade_user_id_here

# Flask Settings
PORT=5000
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Locally

```bash
python app.py
```

The server starts at `http://localhost:5000`

## API Endpoints

### POST /webhook

Receives TradingView alerts and places orders.

**Request Body:**
```json
{
  "symbol": "BTCUSDT",
  "side": "BUY",
  "quantity": 0.01,
  "broker": "BINANCE"
}
```

**For Flattrade (NSE stocks):**
```json
{
  "symbol": "NSE_EQ|INE002A01018",
  "side": "BUY",
  "quantity": 10,
  "broker": "FLATTRADE"
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "BUY order executed on Binance",
  "orderId": 1234567890,
  "broker": "BINANCE"
}
```

**Response (Error):**
```json
{
  "status": "error",
  "message": "Insufficient balance. Available: 0, Required: 0.01",
  "broker": "BINANCE"
}
```

### GET /health

Health check endpoint.

```bash
curl http://localhost:5000/health
```

**Response:**
```json
{
  "status": "healthy",
  "binance_client_initialized": true,
  "flattrade_configured": true
}
```

## Supported Brokers

| Broker | Market | Order Types | Status |
|--------|--------|-------------|--------|
| Binance | Crypto Spot | Market Buy/Sell | ✅ Live |
| Binance TESTNET | Crypto Spot (Test) | Market Buy/Sell | ✅ Test |
| Flattrade | NSE/BSE Stocks | Market | ✅ Live |

## Deployment

### Google Cloud Run

See the comprehensive [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for step-by-step instructions.

**Automated Deployment:**
```bash
chmod +x deploy-gcloud.sh
./deploy-gcloud.sh
```

### Railway

The `Procfile` is pre-configured. Simply connect your GitHub repo to Railway.

### Docker

```bash
docker build -t tradingview-webhook-bot .
docker run -p 5000:5000 --env-file .env tradingview-webhook-bot
```

## TradingView Alert Setup

1. Open TradingView and create/edit an alert
2. In the alert settings, find the **Webhook URL** field
3. Enter your deployed webhook URL: `https://your-app-url.a.run.app/webhook`
4. In the alert message, include JSON like:
   ```
   {"symbol": "BINANCE:BTCUSDT", "side": "{{strategy.order.action}}", "quantity": 0.01, "broker": "BINANCE"}
   ```
5. Save the alert

## File Structure

```
tradingview-webhook-bot/
├── app.py                    # Flask webhook server (main entry point)
├── requirements.txt          # Python dependencies
├── .env.example              # Environment variables template
├── Dockerfile                # Docker container configuration
├── .dockerignore             # Docker build optimization
├── Procfile                  # Railway deployment config
├── deploy-gcloud.sh          # Automated Google Cloud Run deployment
├── DEPLOYMENT_GUIDE.md       # Detailed deployment instructions
└── README.md                 # This file
```

## Requirements

- Python 3.8+
- Flask 2.3.2
- requests 2.31.0
- binance-connector 3.3.0
- python-dotenv 1.0.0
- Gunicorn 21.2.0 (for production)

## Security

- API keys stored in environment variables only
- Never commit `.env` file to version control
- Use Binance TESTNET for testing before live trading
- Implement your own authentication for production use

## Roadmap

- [ ] Add limit order support
- [ ] Add stop-loss / take-profit orders
- [ ] Add portfolio tracking
- [ ] Add Telegram/Discord notifications
- [ ] Add position sizing based on risk percentage
- [ ] Add multi-symbol watchlist
- [ ] Add historical performance logging
- [ ] Add rate limiting and IP allowlist
- [ ] Add JWT authentication for webhook

## License

MIT License - see [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

- Google Cloud Run: https://cloud.google.com/run/docs
- TradingView Webhooks: https://www.tradingview.com/pine_script_docs/
- Binance API: https://binance-docs.github.io/apidocs/
- Flattrade API: https://docs.flattrade.in/

## Author

**Rishav Raj** - [Rishav-raj-github](https://github.com/Rishav-raj-github)

---

⭐ Star this repo if it helped you automate your trading!
