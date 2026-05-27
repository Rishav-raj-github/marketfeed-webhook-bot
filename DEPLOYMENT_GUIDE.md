# Google Cloud Run Deployment Guide

## 🚀 Quick Start

Your MarketFeed webhook bot is ready to deploy on Google Cloud Run. This guide walks you through the deployment process.

### What You'll Get
- ✅ Working webhook that receives MarketFeed alerts
- ✅ Automatic order placement on DigitalAsset
- ✅ Completely free deployment (Google Cloud Free Tier)
- ✅ No credit card required
- ✅ 2M free requests per month

---

## 📋 Prerequisites

1. Google Cloud Account (free tier)
2. Your API Keys:
   - DigitalAsset API Key & Secret
   - FlatTrade API Key, Secret & User ID
3. Git installed locally
4. Terminal/Command prompt access

---

## 🔧 Deployment Methods

### Method 1: Automated Script (Recommended)

**Fastest way - let the script handle everything:**

```bash
# Clone the repository
git clone https://github.com/Rishav-raj-github/market_feed-webhook-bot.git
cd market_feed-webhook-bot

# Run the automated deployment script
chmod +x deploy-gcloud.sh
./deploy-gcloud.sh
```

The script will:
1. Install Google Cloud SDK
2. Authenticate with your Google account
3. Set up the project
4. Enable required APIs
5. Prompt for your API keys
6. Deploy to Cloud Run
7. Show you the webhook URL
8. Test the endpoint

### Method 2: Manual Commands

**If you prefer running commands individually:**

```bash
# 1. Install Google Cloud SDK
curl https://sdk.cloud.google.com | bash

# 2. Initialize
gcloud init

# 3. Login
gcloud auth login

# 4. Set project
gcloud config set project linen-epigram-476218-s9

# 5. Enable APIs
gcloud services enable run.googleapis.com
gcloud services enable cloudbuild.googleapis.com

# 6. Deploy (replace YOUR_* with actual values)
gcloud run deploy market_feed-webhook \
  --source . \
  --platform managed \
  --region us-central1 \
  --memory 512Mi \
  --cpu 1 \
  --allow-unauthenticated \
  --set-env-vars \
    BINANCE_API_KEY=YOUR_BINANCE_KEY,\
    BINANCE_API_SECRET=YOUR_BINANCE_SECRET,\
    BINANCE_TESTNET=true,\
    FLATTRADE_API_KEY=YOUR_FT_KEY,\
    FLATTRADE_API_SECRET=YOUR_FT_SECRET,\
    FLATTRADE_USER_ID=YOUR_FT_USER_ID
```

---

## ✅ After Deployment

### 1. Get Your Webhook URL

After deployment, you'll see output like:
```
Service URL: https://market_feed-webhook-XXXXX.a.run.app
```

Save this URL - you'll need it for MarketFeed.

### 2. Test the Webhook

```bash
curl https://market_feed-webhook-XXXXX.a.run.app/health
```

You should see: `{"status":"healthy"}`

### 3. Update MarketFeed Alerts

1. Go to MarketFeed → Alerts
2. Edit **BULLISH** alert
   - Click "Webhook URL"
   - Replace old URL with: `https://market_feed-webhook-XXXXX.a.run.app/webhook`
   - Click Update

3. Edit **BEARISH** alert
   - Repeat the same process

### 4. Test with a Live Signal

1. In MarketFeed chart, click the alert
2. Click "Test" or "Fire"
3. Check DigitalAsset demo account
4. You should see a new order placed!

---

## 💰 Pricing

**Google Cloud Free Tier Includes:**
- ✅ 2 million requests per month - **FREE**
- ✅ First generation Cloud Run
- ✅ 0.4 CPU, 512 MB RAM per instance
- ✅ No credit card required

**When charges might apply:**
- Only if you exceed 2M requests/month (very unlikely for execution signals)
- Charges are per 100K requests ($0.40)

**Your usage estimate:**
- 1 signal per minute = 1,440 requests/day
- 1,440 × 30 = 43,200 requests/month
- **Still within free tier!**

---

## 🔍 Troubleshooting

### Webhook not receiving alerts
- Check MarketFeed alert is pointing to correct URL
- Verify URL ends with `/webhook`
- Test using curl command above

### Orders not placing in DigitalAsset
- Check DigitalAsset API key and secret are correct
- Verify BINANCE_TESTNET=true is set
- Check order format in app.py

### gcloud command not found
- Restart terminal after installing SDK
- Add gcloud to PATH: `export PATH=/usr/local/google-cloud-sdk/bin:$PATH`

### Permission denied running deploy-gcloud.sh
- Run: `chmod +x deploy-gcloud.sh`
- Then: `./deploy-gcloud.sh`

---

## 📚 Files Included

- `app.py` - Flask webhook server
- `requirements.txt` - Python dependencies
- `Dockerfile` - Docker container config
- `.dockerignore` - Optimize Docker build
- `deploy-gcloud.sh` - Automated deployment script
- `DEPLOYMENT_GUIDE.md` - This file

---

## 🎯 Next Steps

1. ✅ Deploy using deploy-gcloud.sh or manual commands
2. ✅ Get webhook URL from deployment output
3. ✅ Update MarketFeed alerts with new URL
4. ✅ Test with a live signal
5. ✅ Monitor orders in DigitalAsset
6. ✅ Scale to live execution when ready

---

## 📞 Support

- Google Cloud Documentation: https://cloud.google.com/run/docs
- MarketFeed Webhooks: https://www.market_feed.com/pine_script_docs/
- DigitalAsset API: https://digital_asset-docs.github.io/apidocs/

---

**Status: ✅ READY FOR DEPLOYMENT**

Your bot is configured and ready to go. Deploy now to start automating your trades!
