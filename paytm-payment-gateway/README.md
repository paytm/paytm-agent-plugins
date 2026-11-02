# Paytm Payment Gateway

Cursor skills plugin for Paytm Payment Gateway code generation. It does not include MCP.

- Plugin id: `paytm-payment-gateway`
- Skill: `paytm-integration`

## What it generates

JS Checkout, subscriptions (UPI Autopay / native subscription), payment links, dynamic QR, All-in-One SDK, Custom SDK, webhooks, and refunds.

For a generic "Add Paytm checkout" prompt, the skill uses JS Checkout.

## Install

This folder is one plugin in the [paytm-agent-plugins](https://github.com/paytm/paytm-agent-plugins) repo. Cursor loads it from the repo root `marketplace.json`.

A team admin imports `https://github.com/paytm/paytm-agent-plugins` under Dashboard → Plugins & MCPs → Team Marketplaces. After that, install **paytm-payment-gateway** from Customize.

## Credentials

Never commit secrets or paste a Merchant Key in chat. Generated apps should use `.env` and `.env.example` with empty placeholders.

| Variable | Purpose |
|---|---|
| `PAYTM_MID` | Merchant ID |
| `PAYTM_MERCHANT_KEY` | Merchant Key (server-only) |
| `PAYTM_ENVIRONMENT` | `staging` or `production` |
| `PAYTM_WEBSITE_NAME` | Dashboard `websiteName` (staging often `WEBSTAGING`; production often `DEFAULT`) |
| `PAYTM_PG_DOMAIN` | Optional override |

| Environment | `PAYTM_PG_DOMAIN` |
|---|---|
| Staging | `https://securestage.paytmpayments.com` |
| Production | `https://secure.paytmpayments.com` |

Do not use legacy hosts: `securegw.paytm.in`, `securegw-stage.paytm.in`, `pguat.paytm.com`.

Dashboard: https://dashboard.paytmpayments.com/next/apikeys

## Layout

    paytm-payment-gateway/
    ├── .cursor-plugin/plugin.json
    ├── skills/paytm-integration/SKILL.md
    ├── skills/paytm-integration/references/flows.md
    ├── README.md
    └── LICENSE
