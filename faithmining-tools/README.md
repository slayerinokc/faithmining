# Faith Mining Tools

Static site hosting free calculators and utilities for Bitcoin miners. Deployed to Cloudflare Pages at `tools.faithmining.net`.

## Tools

- **Bitcoin Miner ROI Calculator** (`/bitcoin-miner-roi/`) — Payback period, break-even BTC price, and break-even difficulty. Live BTC price + network difficulty via public APIs.
- Hashrate Converter (planned)
- Mining Profitability Comparator (planned)

## Stack

- Pure static HTML + vanilla JS + embedded CSS. No framework, no build step.
- Live data from public APIs (CoinGecko, Coinbase fallback, blockchain.info) — all client-side, no serverless functions needed.
- Cloudflare Pages for hosting + DNS (already your setup).

## Project structure

```
faithmining-tools/
├── index.html                    Tools hub / landing
├── bitcoin-miner-roi/
│   └── index.html               Single-file ROI calculator
├── favicon.svg                   Shared favicon
├── robots.txt                    Crawler directives
├── sitemap.xml                   For Google Search Console
├── _headers                      Cloudflare Pages security/cache headers
├── .gitignore
├── README.md                     This file
└── DEPLOY.md                     Step-by-step deploy instructions
```

## Local development

Open `bitcoin-miner-roi/index.html` directly in a browser, or serve with any static server:

```bash
# Python
python3 -m http.server 8000
# or Node
npx serve .
```

Then hit `http://localhost:8000/bitcoin-miner-roi/`.

## Deployment

See `DEPLOY.md` for the full Cloudflare Pages + subdomain setup.

## Notes

- AdSense placements are marked with `<!-- TODO -->` comments in each tool's HTML. Drop in the AdSense snippet once the domain is approved.
- Affiliate links (Miner Bros, Altair) use `rel="sponsored noopener"` per Google's guidelines.
- Block reward is hard-coded to 3.125 BTC. Update before the 2028 halving.
- No analytics yet — can add Cloudflare Web Analytics (free, privacy-friendly, no cookies) or Plausible later.

## License

Proprietary — Faith Mining LLC. All rights reserved.
