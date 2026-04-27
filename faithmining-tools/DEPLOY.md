# Deployment Guide — tools.faithmining.net

Step-by-step Cloudflare Pages setup. Estimated time: **10–15 minutes** for the initial deploy. After that, every push to `main` auto-deploys.

**Assumes you already have:**
- Cloudflare account with faithmining.net managed there (you do)
- GitHub account (you do)
- These project files on disk

---

## Option A: GitHub → Cloudflare Pages (recommended — set up once, auto-deploys forever)

### 1. Create the GitHub repo

On GitHub.com → New repository:
- **Name:** `faithmining-tools`
- **Visibility:** Private is fine; Public if you want people to see the code (no secrets in here, either works)
- **Do NOT** initialize with README (we already have one)
- Click **Create repository**

### 2. Push the local files

In a terminal, from the `faithmining-tools/` folder:

```bash
cd path/to/faithmining-tools
git init
git branch -M main
git add .
git commit -m "Initial commit: ROI calculator + tools hub"
git remote add origin https://github.com/<your-github-username>/faithmining-tools.git
git push -u origin main
```

If you don't have git configured:
```bash
git config --global user.email "chrisdjohnson@gmail.com"
git config --global user.name "Chris Johnson"
```

### 3. Create the Cloudflare Pages project

Cloudflare dashboard → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**:

- Authorize Cloudflare for GitHub (one-time OAuth)
- Pick the `faithmining-tools` repository
- Click **Begin setup**

**Build settings** (this is a pure static site so these are minimal):
- **Project name:** `faithmining-tools`
- **Production branch:** `main`
- **Framework preset:** *None*
- **Build command:** *(leave blank)*
- **Build output directory:** `/`
- **Root directory:** `/`

Click **Save and Deploy**. Cloudflare will build and publish in ~30 seconds. You'll get a temporary URL like `faithmining-tools.pages.dev` — confirm it works.

### 4. Attach the subdomain `tools.faithmining.net`

In the Cloudflare Pages project → **Custom domains** tab → **Set up a custom domain**:

- Enter `tools.faithmining.net`
- Cloudflare auto-detects that faithmining.net is on your account and offers to create the DNS record automatically → click **Activate**

It will create a CNAME record:
```
tools.faithmining.net → faithmining-tools.pages.dev  (proxied, orange cloud)
```

SSL provisions automatically within 1–5 minutes. Test:
```
https://tools.faithmining.net/                        # landing page
https://tools.faithmining.net/bitcoin-miner-roi/      # calculator
```

### 5. Confirm it works

Open `https://tools.faithmining.net/bitcoin-miner-roi/` in an incognito window. Verify:

- [ ] Live BTC price loads in the top banner
- [ ] Network difficulty loads
- [ ] Picking an ASIC preset auto-fills hashrate/power/cost
- [ ] Payback period recalculates as you edit any input
- [ ] Sensitivity table renders with green/red coloring
- [ ] Mobile layout works (resize browser to <800px width)

---

## Option B: Direct upload (no GitHub) — quicker but no auto-deploy

Cloudflare dashboard → **Workers & Pages** → **Create application** → **Pages** → **Upload assets**:

- Project name: `faithmining-tools`
- Drag the entire `faithmining-tools/` folder into the uploader
- Deploy

Downside: every future update requires another manual drag-and-drop. Not recommended if you'll iterate on the tool.

---

## Post-deploy tasks (order matters)

### Immediately after deploy

1. **Open the live URL in incognito** and click around. Trust nothing until you've verified it live.
2. **Submit the sitemap to Google Search Console:**
   - Search Console → Add property → `https://tools.faithmining.net/`
   - Verify via Cloudflare DNS (auto-detected)
   - Sitemaps → submit `https://tools.faithmining.net/sitemap.xml`
3. **Test mobile** — check on your phone, not just the desktop browser.

### Within 1–2 weeks

4. **Apply for Google AdSense** using `tools.faithmining.net` (or your main `faithmining.net`) as the site URL.
5. Once AdSense is approved, edit `bitcoin-miner-roi/index.html`:
   - Uncomment the AdSense loader `<script>` in `<head>`
   - Replace each `<!-- TODO: AdSense -->` placeholder with the `<ins class="adsbygoogle">` snippet from AdSense
   - Commit, push → auto-deploys
6. **Add Cloudflare Web Analytics** (free, privacy-first, no cookies): Pages project → Settings → Web Analytics → Enable.

### Ongoing

- Every code change: commit to `main` → pushes → Cloudflare auto-deploys in ~30 seconds.
- Check Google Search Console weekly for indexing status + queries.
- Review Cloudflare Analytics monthly for traffic and referrer sources.

---

## Common issues & fixes

**"Site can't be reached" on tools.faithmining.net after setup**
DNS propagation. Give it 5–10 minutes. Also verify the CNAME in Cloudflare DNS points to `faithmining-tools.pages.dev` and is proxied (orange cloud).

**Live BTC price shows "loading…" forever**
Open browser console (F12). Likely CORS or API rate limit. CoinGecko occasionally rate-limits from IPs with heavy usage — the fallback to Coinbase should kick in. If both fail, user can just enter the price manually.

**Calculator returns Infinity or NaN**
One of the numeric inputs is zero or blank. Defaults should prevent this; if it happens, check the browser console for which input is the problem.

**Cloudflare Pages build fails**
Shouldn't happen for a static site, but if it does: build command should be empty, build output directory should be `/`.

---

## Updating the calculator later

1. Edit files locally
2. `git add . && git commit -m "describe change" && git push`
3. Cloudflare auto-deploys in ~30 seconds
4. Hard-refresh browser (Ctrl+Shift+R) to bypass cache

---

Questions? Open an issue in the GitHub repo or reply on YouTube.
