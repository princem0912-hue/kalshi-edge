# Kalshi Edge — secure API-connected website starter

This project adds a Flask backend so the browser never receives the Kalshi private key.

## Security and modes

- Defaults to **demo** API mode (`https://demo-api.kalshi.co/trade-api/v2`).
- It never asks for your Kalshi username or password.
- API key ID and private key must be stored as server environment variables / secret settings.
- Live mode is blocked unless `KALSHI_MODE=live` AND `LIVE_TRADING_ENABLED=true` are set on the server.
- Order endpoint accepts only limit buys, 1–10 contracts per order, with a confirmation checkbox and browser confirmation prompt.
- Live mode still submits real-money orders. There is no profit guarantee.
- This starter does not include an autonomous AI strategy; it is an authenticated market/portfolio console with controlled manual order submission.

## Run locally

Requires Python 3.10+.

1. Create a virtual environment (recommended).
2. Install dependencies: `pip install -r requirements.txt`
3. Generate an API key in the **Kalshi demo account** and download the private key. Demo and production credentials are separate.
4. Put the demo private key file in this folder as `kalshi_private_key.pem` (or configure a different path).
5. Set environment variables in your terminal:
   - `KALSHI_MODE=demo`
   - `KALSHI_API_KEY_ID=your-demo-key-id`
   - `KALSHI_PRIVATE_KEY_PATH=./kalshi_private_key.pem`
   - `FLASK_SECRET_KEY=a-long-random-secret`
6. Run: `python app.py`
7. Open `http://127.0.0.1:8080`.

The browser UI has **Connect / test credentials**, **Load live market data**, portfolio refresh, and a small limit-order form. In demo mode, orders use simulated demo funds.

## Deploying

Use a Python-capable host. Configure the same environment variables in its private secret/environment-variable settings and store the private key in a protected file or secret manager. Do not upload the private key into the public website/static folder. Use HTTPS. Do not expose Flask's development server; run behind a production WSGI server and reverse proxy. Add user authentication, CSRF protection, rate limiting, persistent audit logs, monitoring, and proper key management before allowing internet users.

## Turning on live mode (not recommended until thoroughly tested)

1. Test connection and orders in demo mode first.
2. Create a separate production API key in the real Kalshi account settings.
3. Store the production key ID and private key as server secrets.
4. Set `KALSHI_MODE=live`.
5. Only after you understand the risk, set `LIVE_TRADING_ENABLED=true`.

Restart the app after changing environment variables. Do not use a shared/public host without securing it. The live-order toggle is a server setting, not a browser button.

## Notes

Kalshi API schemas and signature requirements can change. Check the official docs before using the app with real money:
- https://docs.kalshi.com/
- https://help.kalshi.com/en/articles/13823854-kalshi-api

This is starter code, not financial advice. It is not a recommendation to place any particular trade.
