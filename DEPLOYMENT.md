# EPL Predictor — Live M-Pesa Premium Backend

This package implements a **non-wagering Premium Predictions subscription**. It does not provide betting, wagering, deposits for gambling, withdrawals, or cash-out.

## 1. Database
Create a PostgreSQL database and run `schema.sql`.

## 2. Environment
Copy `.env.example` to `.env` and fill in your own Daraja production credentials. Never commit `.env`.

Required variables:
- `DATABASE_URL`
- `MPESA_CONSUMER_KEY`
- `MPESA_CONSUMER_SECRET`
- `MPESA_SHORTCODE` — this must be the live PayBill/Till/short code configured for your business; a personal phone number is not itself a Daraja shortcode.
- `MPESA_PASSKEY`
- `MPESA_CALLBACK_URL` — public HTTPS URL ending in `/api/mpesa/callback`
- `ADMIN_API_KEY`

`PREMIUM_PRICE=299` and `PREMIUM_DAYS=30` are configurable.

## 3. Install and run
```bash
npm install
npm start
```

The frontend calls `POST /api/mpesa/stkpush`. The backend obtains the Daraja OAuth token, sends the STK Push, stores the request, receives the asynchronous callback, and marks a successful payment as `PAID`. The frontend polls the payment status and shows confirmation.

## 4. Go live
Safaricom states that production integrations require a live M-PESA PayBill/Till/B2C setup and a Daraja go-live process. Callback URLs must be publicly reachable; Safaricom also documents callback/IP-whitelisting requirements. See the official Daraja documentation before enabling production credentials.

## 5. Security
- Keep Consumer Key, Consumer Secret, Passkey and database credentials server-side.
- Use HTTPS in production.
- Do not trust a frontend success message as proof of payment.
- Grant premium only after the verified server callback marks the transaction `PAID`.
- Keep callback processing idempotent.
- Rotate `ADMIN_API_KEY` if exposed.
