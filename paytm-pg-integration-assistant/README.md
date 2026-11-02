# Paytm PG Integration Assistant

Paytm Plugin for Cursor is to integrate Paytm Payment Gateway in minutes - JS Checkout, Subscriptions, payment links, and QR codes for Node.js, Python, Java, and Spring.

- Plugin id: `paytm-pg-integration-assistant`
- Skill: `paytm-integration`

## What it generates

This skills helps developers integrate and build payment experiences using Paytm Payment Gateway (PG) through built-in knowledge of Paytm APIs, integration flows, and best practices.
Instead of switching between documentation and code editors, developers can simply describe their requirements in plain English and get production-ready integration code.
It supports multiple programming languages and frameworks, including Node.js, Python, Java, Spring Boot, and HTML/JavaScript.

The skill supports multiple use cases, including:
- JS Checkout: Integrate Paytm's JavaScript-based checkout experience.
- Recurring Payments: Set up subscriptions and automated recurring payments.
- Payment Links: Generate, manage, and track payment links.
- Payment Processing: Handle transactions, orders, refunds, and payment status.
- Integration Support: Understand API flows, request parameters, callbacks, and validations.

## Install

This folder is one plugin in the [paytm-agent-plugins](https://github.com/paytm/paytm-agent-plugins) repo. Cursor loads it from the repo root `marketplace.json`.

A team admin imports `https://github.com/paytm/paytm-agent-plugins` under Dashboard → Plugins & MCPs → Team Marketplaces. After that, install **paytm-pg-integration-assistant** from Customize.

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

    paytm-pg-integration-assistant/
    ├── .cursor-plugin/plugin.json
    ├── skills/paytm-integration/SKILL.md
    ├── skills/paytm-integration/references/flows.md
    ├── README.md
    └── LICENSE
