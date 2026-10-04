# FetchAro — AI-Driven Web Scraping Platform

**Case study** · Architect / Lead Backend Engineer · [years]
`Node.js` `TypeScript` `Python` `Redis` `SQS` `Docker` `AWS` `[proxy infra: TBD]`

> Live product: [fetch.klapify.com](https://fetch.klapify.com/)

---

## Overview

FetchAro is a scraping platform built for the hardest version of the problem:
collecting structured data in real time from websites that actively try to
stop you — including sites behind Cloudflare-class protection — without the
user ever managing proxies, browsers, or CAPTCHAs. A single REST call returns
clean JSON; everything adversarial happens on our side. An AI-based price
alert layer tracks drops and notifies users the moment they happen.

> Production system under NDA — this is a public case study, no source code.

## By the numbers

| Metric | Value |
|---|---|
| API requests processed | **10B+ per month** |
| Average API response time | **< 3 seconds** |
| Residential proxy pool | **40M+ IPs** |
| Geolocations | **195 countries** |
| Uptime | **99.9% SLA** |
| Companies relying on it | **10,000+** |

## The problem

Scraping at scale is an adversarial systems problem, not a parsing problem:

- Target sites change markup without notice — a working extractor silently
  returns garbage until someone notices.
- Bot protection (Cloudflare, custom WAFs) forces a constant
  measure-countermeasure cycle.
- Blocks are expensive: a banned IP or fingerprint takes a target offline
  for hours.
- Data has a shelf life — price alerts are worthless if a drop is detected
  three days late.

The pitch is simple: *"Focus on your data, not infrastructure."* Making that
true is a deep engineering problem.

## What the platform does

- **Web Scraping API** — JavaScript rendering, automatic proxy rotation,
  anti-bot bypass, structured JSON out.
- **Async Scraping** — queue millions of URLs, receive results via webhook
  when ready.
- **DataPipeline** — automated delivery to cloud storage and data warehouses.
- **Residential Proxies** — 40M+ IPs across 195 geolocations for maximum
  success rates.
- **Price Monitoring** — real-time competitor price tracking across thousands
  of products and marketplaces, with instant drop alerts.
- **SDKs** — Python, Node.js, Ruby, and more.

## Architecture

```
┌────────────┐   ┌──────────────────┐   ┌────────────────────────────┐
│  Target    │──▶│  Fetch workers   │──▶│  Normalization / parsing   │
│  websites  │   │  (rotating infra)│   │  (adaptive, self-healing)  │
└────────────┘   └──────────────────┘   └─────────────┬──────────────┘
                                                      │
                          ┌───────────────────────────▼────────────┐
                          │  Queue layer (SQS / Redis)             │
                          │  async jobs · retries · webhooks       │
                          └───────────────────────────┬────────────┘
                                                      │
                    ┌─────────────────┬───────────────┴───────────┐
                    ▼                 ▼                           ▼
          ┌───────────────┐  ┌────────────────┐       ┌────────────────────┐
          │  Data store   │  │  Change        │       │  AI price-alert    │
          │  (products)   │  │  detection     │       │  engine (LLM +     │
          └───────────────┘  └────────────────┘       │  price history)    │
                                                      └────────────────────┘
```

## Key decisions

**1. Adaptation over maintenance.**
Target sites change constantly; hand-fixing extractors doesn't scale. The
parsing layer detects structural change and adapts automatically — the
engineering goal was near-zero maintenance headcount per target.

**2. Treat blocking as a first-class engineering domain.**
Rotation, fingerprinting, behavioral simulation, and backoff aren't bolt-on
hacks — they're core infrastructure designed with the same rigor as the data
pipeline. That rigor is what makes a <3s average response and a 99.9% uptime
SLA possible while targets actively fight back.

**3. Async as a first-class pattern, not a workaround.**
At 10B+ requests a month, synchronous-only APIs become a bottleneck and a
brittleness. Queue-and-webhook architecture lets clients fire millions of
URLs and consume results at their own pace.

**4. Alerts as a product, not a feature.**
A price-drop notification has a shelf life of minutes. The alert engine is
optimized end-to-end: fetch → diff → classify → notify, with AI filtering
noise (pseudo-drops, currency shifts, out-of-stock relabels).

## Results

- 10B+ API requests processed monthly at <3s average response time
- 99.9% uptime SLA, sustained in production
- 10,000+ companies using the platform across e-commerce, SEO, lead
  generation, and AI/ML training-data use cases
- [Alert latency achieved — e.g., "price drops detected and delivered within X minutes"; confirm what's safe to state]

## My role

[CONFIRM: what you personally built — e.g., "Architected the core scraping
pipeline and async job system; designed the proxy rotation infrastructure;
led the AI price-alert service."]

---

*Code private. Happy to discuss anti-blocking and adaptive-parsing
architecture: [email]*hello@biswaj.it
