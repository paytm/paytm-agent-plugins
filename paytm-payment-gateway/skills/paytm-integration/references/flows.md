# Paytm integration flows (reference)

Condensed flow map for code generation. For deep field tables and pitfalls, use the matching skill under [paytm-agent-skills](https://github.com/paytm/paytm-agent-skills/tree/feat/skills-distribution/skills).

---

## 1. JS Checkout (one-time)

1. Server: generate checksum over request body with Merchant Key.
2. Server: `POST {PAYTM_PG_DOMAIN}/theia/api/v1/initiateTransaction?mid={MID}&orderId={ORDER_ID}` → `txnToken` (≈15 min TTL).
3. Browser: load `{PAYTM_PG_DOMAIN}/merchantpgpui/checkoutjs/merchants/{MID}.js`.
4. Browser: `CheckoutJS.init` + `invoke` with `orderId`, `txnToken`, amount; wire `transactionStatus` **and** `notifyMerchant`.
5. Server callback: verify `CHECKSUMHASH` on form fields.
6. Server: **always** reconfirm `POST {PAYTM_PG_DOMAIN}/v3/order/status` before fulfilment.

Common bugs: wrong domain; staging `websiteName` on prod; `CheckoutJS.onLoad` inside click handler; amount not `"x.xx"` string; key exposed to client.

---

## 2. Subscriptions (UPI Autopay / Native Subscription)

1. Create subscription / mandate with frequency, amount, payment mode (`NATIVE_SUBSCRIPTION` / related).
2. Customer completes consent (UPI Autopay / card / eNACH as enabled).
3. Charge on schedule; handle pre-debit notifications where required.
4. Support edit / pause / cancel via Paytm subscription APIs.
5. Reconcile charges with status APIs + webhooks.

Do not reuse one-time-only status endpoints for subscription-specific reconciliation without checking product docs.

---

## 3. Payment Links

1. Server-only: create link via `/link/create` (checksum + AES-style head fields as per link APIs).
2. Share `shortUrl` (SMS / WhatsApp / email).
3. Reconcile with `/link/fetch` and `/link/fetchTransaction` — **not** `/v3/order/status`.
4. Support expire / resend notification as needed.

---

## 4. Dynamic QR

1. Server: create dynamic QR (`/paymentservices/qr/create` family) with amount / order / `posId` rules.
2. Render QR (image/base64) on page or print.
3. Customer pays with any UPI app.
4. Reconcile via status/webhook — do not assume scan implies success.

---

## 5. All-in-One SDK (Android / iOS)

1. Same backend initiate flow as JS Checkout → `txnToken`.
2. Native SDK presents Paytm-branded UI (`PaytmSDK.startTransaction` / platform equivalent).
3. Handle native callbacks; reconfirm server-side status before fulfilment.
4. Register deep links / URL schemes per platform docs.

---

## 6. Custom SDK (Android / iOS)

1. Backend initiates transaction → `txnToken`.
2. App builds custom UI; use `fetchPaymentOptions` + `processTransaction` (or SDK equivalents).
3. Cards may expand PCI scope — call out clearly in generated guidance.
4. UPI: poll/status until terminal state; handle 3DS WebView for cards.

---

## 7. Webhooks (S2S)

1. Expose HTTPS endpoint; read **raw body** for signature verification.
2. Verify Paytm signature (`head.signature` / checksum lib) before trusting payload.
3. Idempotent handling (dedupe on orderId + txnId / event id).
4. Respond quickly; process async; rely on Paytm retry/backoff.
5. Still reconfirm critical fulfilment with status API when in doubt.

---

## 8. Refunds

1. Only refund successful (or eligible) transactions.
2. `POST` refund apply with unique `refId`; support partial amounts with cumulative limits.
3. Poll refund status until terminal; handle `PENDING`.
4. Optional: refund webhooks; never double-refund without idempotency keys.

---

## Staging smoke checklist

- [ ] Staging MID + key + `WEBSTAGING` (or dashboard value) + stage domain
- [ ] Initiate → pay with test instrument → status `TXN_SUCCESS`
- [ ] Callback checksum verify + status API agree
- [ ] Webhook received (if configured) and deduped
- [ ] Refund path tested on a small amount
